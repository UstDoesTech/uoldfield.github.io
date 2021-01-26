Some systems use a single currency as a base, which is something that I noticed recently when working with IBM Cognos Controller, e.g. USD to convert local currencies into. But what if you want / need to rebase into another currency but still retain the original base?

This doesn’t appear to be easy to achieve within Cognos Controller itself, but it is achievable within SQL and a wider ETL framework.

The basis for rebasing exchange rates uses the technique of triangulation arbitrage. The technique is most often used within the banking system and more information can be found here: http://en.wikipedia.org/wiki/Triangular_arbitrage 

In principle you deal with two known exchange rates and one unknown.

For example, if you have an exchange rate base of USD and know that the GBP/USD exchange rate is 1.54631 and the EUR/USD exchange rate is 1.11470 and wish to rebase from USD to EUR. You would begin by finding the inverse rates of GBP/USD (1/1.54631 = 0.6467), multiply by the EUR/USD rate (0.6467*1.11470 = 0.72087649) which produces the EUR/GBP and then find the inverse (1/0.72087649 = 1.3872) to produce the GBP/EUR rate. In order to find the USD/EUR exchange rate one simply finds the reverse of the EUR/USD rate (1/1.11470 = 0.8971).

That might sound simple, but most exchange rates are held in a table across a range of dates. This complicates the calculation somewhat. I’ve used CTEs because I find that it makes the script neater and easier to debug. Below is an example of the triangulation using the Sales.CurrencyRate table in the AdventureWorks2012 database.
{% highlight sql %}
USE AdventureWorks2012
;

WITH
LocalExchange AS (
SELECT
ToCurrencyCode                                                                                    AS CurrencyCode
,CASE WHEN ToCurrencyCode <> ‘EUR’ THEN (1 / AverageRate) END                                    AS LocalCurrencyConversion
,CurrencyRateDate
FROM [Sales].[CurrencyRate] ),
EuroExchange AS (
SELECT
ToCurrencyCode                                                                                    AS CurrencyCode
,CASE WHEN(ToCurrencyCode = ‘EUR’)THEN AverageRate END                                            AS EuroConversion
,CurrencyRateDate
FROM [Sales].[CurrencyRate] WHERE CASE
WHEN(ToCurrencyCode = ‘EUR’)  THEN AverageRate END IS NOT NULL
)
SELECT DISTINCT
–Keys
C.CurrencyRateID
,C.CurrencyRateDate
,CASE
WHEN C.ToCurrencyCode = ‘EUR’ THEN ‘USD’
WHEN C.AverageRate = 1 THEN ‘EUR’
ELSE C.ToCurrencyCode
END                                                                                                AS CurrencyCode
,1/(CASE
WHEN C.AverageRate = 1 THEN 1 ELSE
(1 / (CASE
WHEN C.ToCurrencyCode <> ‘EUR’ THEN (L.LocalCurrencyConversion * E.EuroConversion)
ELSE C.AverageRate
END))
END)                                                                                                AS ImplicitEuroExchangeRate
–Measures
–,C.ExchangeRate                                                                                        AS ExchangeRate
FROM
[Sales].[CurrencyRate] AS C
INNER JOIN LocalExchange AS L
ON L.CurrencyRateDate = C.CurrencyRateDate
AND L.CurrencyCode = C.ToCurrencyCode
INNER JOIN EuroExchange AS E
ON E.CurrencyRateDate = C.CurrencyRateDate
WHERE
CASE
WHEN c.ToCurrencyCode <> ‘EUR’ THEN (E.EuroConversion * L.LocalCurrencyConversion)
ELSE 1
END    IS NOT NULL
ORDER BY C.CurrencyRateID
{% end highlight %}

As always, if you have any feedback and can suggest a simpler way of performing the triangulation I would love to hear it.
