---
title: "Testautomatisierer"
date: 2022-06-01T00:00:00+02:00
featured: true
weight: 2
---

Testautomatisierer erstellen automatisierte Tests um Integrations- oder Regressionstests sicherzustellen. Sie sollten frei in der Wahl ihrer Werkzeuge und Tools sein. FlySql unterstützt sie dabei maximal, es erlaubt die Verwendung aller Programmiersprachen und Drittsoftware, die mit REST Schnittstellen arbeiten können. Zudem können die Interaktion zu Avaloq dank FlySql punktuell an beliebigen Stellen im Testablauf eingebaut werden.

## Werkzeuge
- Entwicklungswerkzeuge: IntelliJ, Eclipse
- Script Tools: Tricentis Tosca, Ranorex
- Browser: um FlySql Resourcen anzuschauen oder AdHoc Queries auszuführen

## Beispiele
### Von FlySql als REST Resource verfügbare Scripte 
- POST: `/ebanking/login-by-emailadress.sql` mit Payload `{ "EMAILADRESS":"flysql@flysql.net" }`
- POST: `/assets/markets/marketname-by-marketkey.avq` mit Payload `{ "MARKET_KEY":"SIX" }`
- POST: `/orders/xfer/new-with-creditcontainer.avq` mit Payload `{ "CREDIT_CONTAINER_MACC":"1856-0098", "AMOUNT: 12000 }`

### Aufruf einer FlySql Resource in Java Junit Test
```
...

@Test
void marketSix_returnsId185() {
  RestTemplate restTemplate = new RestTemplate();
  String baseUrl = "http://flysql.uat1.bank.ch" + "/ebanking/login-by-emailadress.sql";
  URI uri = new URI(baseUrl);
  MarketDto market = new Market("SIX");

  HttpHeaders headers = new HttpHeaders();
  ...
  HttpEntity<Employee> request = new HttpEntity<>(market, headers);
  ResponseEntity<String> result = restTemplate.postForEntity(uri, request, String.class);
  ...
  Assert.assertEquals(201, result.getStatusCodeValue());
  String body = result.getBody();
  ...
}
```