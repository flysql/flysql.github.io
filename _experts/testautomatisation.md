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
### Durch FlySql als REST Resource verfügbare Scripte
Alle Resourcen werden als GET und POST Methode exponiert. 
#### POST Query Login name by emailadress  
<textarea class="textarea-code-snippet mb-2" rows="2" cols="70" id="post1" >
POST: `/ebanking/login-by-emailadress.sql`
PAYLOAD: { "EMAILADRESS":"flysql@flysql.net" }
</textarea>  
#### GET Name for Market SIX
<textarea class="textarea-code-snippet mb-2" rows="1" cols="70" id="post1" >
GET: `/assets/markets/marketname-by-marketkey.avq?MARKET_KEY=SIX`
</textarea> 
#### Geldübertrag von CHF 12'000
<textarea class="textarea-code-snippet mb-2" rows="2" cols="70" id="post1" >
POST: `/orders/xfer/new-with-creditcontainer.avq`
PAYLOAD: { "CREDIT_CONTAINER_MACC":"1856-0098", "AMOUNT: 12000 }
</textarea>

### Aufruf einer FlySql Resource in Java Junit Test
<textarea class="textarea-code-snippet" rows="22" cols="70" >
package ebankingtest.bank.ch
...

@Test
void marketSix_returnsId185() {
  RestTemplate restTemplate = new RestTemplate();
  String baseUrl = "http://flysql.uat1.bank.ch";
  String path = "/ebanking/login-by-emailadress.sql";

  URI uri = new URI(baseUrl + path);
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

</textarea>
