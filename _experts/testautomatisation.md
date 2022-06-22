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

## Verwendung der durch FlySql zur Verfügung gestellten Resourcen
<table class="table">
  <thead class="thead-light">
    <tr>
      <th>Pfad</th>
      <th>Result Format</th>
      <th>Beschreibung</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>execute/&lt;DB&gt;/sql</td>
      <td><textarea class="textarea-code-snippet" rows="5" cols="28">{ "rowsize":2,
  "columns":["col1", "col2"], 
  "rows":[["r1c1","r1c2"],
          ["r2c1","r2c2"]],
  "scalarValue":"r1c1"  }</textarea></td>  
      <td>Aufruf eines SQL Statements, das Result ist behinhaltet alle Zeilen und Spalten des Resultats</td>
    </tr>
    <tr>
      <td>execute/&lt;DB&gt;/sql-scalar</td>
      <td><textarea class="textarea-code-snippet" rows="1" cols="28">"Resultat Zeile 1, Spalte 1"</textarea></td>
      <td>Aufruf eines SQL Statements, das Resultat ist ein String mit dem Wert der ersten Zeile, erste Spalte</td>
    </tr>
    <tr>
      <td>execute/&lt;DB&gt;/avq/</td>
      <td><textarea class="textarea-code-snippet" rows="2" cols="28">"Resultat Avaloq Skripts oder des return statements"</textarea></td>
      <td>Aufruf eines Avaloq Skripts, das Resultat ist der String eines Statements oder des mit 'return' angegebenen Wertes</td>
    </tr>
  </tbody>
</table>


## Beispiel - SQL Resourcen
Beispiel Daten:  <span class="code-snippet">select ISIN, NAME from assets where EXCHANGE='${EXCHANGE}'</span>
<table class="table">
  <thead class="thead-light">
    <tr>
      <th>Isin</th>
      <th>Name</th>
      <th>Exchange</th>
      <th>Handelswährung</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>CH0012221716</td>
      <td>ABB</td>
      <td>SWX</td>
      <td>CHF</td>
    </tr>
    <tr>
      <td>CH0012032113</td>
      <td>Roche</td>
      <td>SWX</td>
      <td>CHF</td>
    </tr>
    <tr>
      <td>US4592001014</td>
      <td>IBM</td>
      <td>NYSE</td>
      <td>USD</td>
    </tr>
  </tbody>
</table>  

### Finde alle Aktie an der SWX
<table class="table">
  <thead class="thead-light">
    <tr>
      <th>Method</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="1" cols="63" id="post1" >execute/AVQDB-1/sql/assets/assets-by-market.sql?EXCHANGE=SIX
        </textarea>
      </td>
    </tr>
    <tr>
      <td>POST</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="3" cols="63" id="post1" >execute/AVQDB-1/sql        
PAYLOAD : { "path":"/assets/assets-by-market.sql",
            "parameterValues": {"EXCHANGE":"SIX"} }
        </textarea> 
      </td>
    </tr>
    <tr>
      <td>Resultat</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="4" cols="63" id="post1" >
{ "rowsize":2,
  "columns":["ISIN", "NAME"], 
  "rows":[["CH0012221716","ABB"],["CH0012032113","Roche"]],
  "scalarValue":"CH0012221716"  }
        </textarea>
      </td>
    </tr>
  </tbody>
</table>

### Finde irgendeine ISIN einer Aktie an der SWX
<table class="table">
  <thead class="thead-light">
    <tr>
      <th>Method</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="2" cols="63" id="post1" >execute/AVQDB-1/sql-scalar/assets/assets-by-market.sql?EXCHANGE=SIX
        </textarea>
      </td>
    </tr>
    <tr>
      <td>POST</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="3" cols="63" id="post1" >execute/AVQDB-1/sql-scalar        
PAYLOAD : { "path":"/assets/assets-by-market.sql",
            "parameterValues": {"EXCHANGE":"SIX"} }
        </textarea> 
      </td>
    </tr>
    <tr>
      <td>Resultat</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="1" cols="63" id="post1" >
CH0012221716
        </textarea>
      </td>
    </tr>
  </tbody>
</table>

## Beispiel - Avaloq Script Resource
Beispiel Preisabfrage:  <span class="code-snippet">lookup.obj_asset('${ISIN}').price</span>
<table class="table">
  <thead class="thead-light">
    <tr>
      <th>Method</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="2" cols="63" id="post1" >execute/AVQDB-1/avq/FLYSQLUSER/assets/price.avq?ISIN=CH0012221716
        </textarea>
      </td>
    </tr>
    <tr>
      <td>POST</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="4" cols="63" id="post1" >execute/AVQDB-1/avq        
PAYLOAD : { "path":"/assets/assets/price.avq",
            "user":"FLYSQLUSER",
            "parameterValues": {"ISIN":"CH0012221716"} }
        </textarea> 
      </td>
    </tr>
    <tr>
      <td>Resultat</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="1" cols="63" id="post1" >
15.65
        </textarea>
      </td>
    </tr>
  </tbody>
</table>

Beispiel Zahlungsorder:
<textarea class="textarea-code-snippet" rows="19" cols="70" >
DECLARE
  ...
BEGIN
  l_cred_macc_id := lookup.macc_id('${CREDIT_MACC}');
  l_amount := ${AMOUNT};
  /* more statements here */
  
  with new mem_doc_xfermon(1) as tdoc do
    ....
  end with;
  
  with mem_doc_xfermon(session.doc_mgr.load_doc(l_doc_id)) as tdoc do
    ...
  end with;

  return l_doc_id;
EXCEPTION
  /* error handling here */
END;
</textarea>

<table class="table">
  <thead class="thead-light">
    <tr>
      <th>Method</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="2" cols="63" id="post1" >execute/AVQDB-1/avq/FLYSQLUSER/orders/xfer.avq?CREDIT_MACC=12548-CHF&AMOUNT=12000
        </textarea>
      </td>
    </tr>
    <tr>
      <td>POST</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="5" cols="63" id="post1" >execute/AVQDB-1/avq/        
PAYLOAD : { "path":"/orders/xfer.avq",
            "user":"FLYSQLUSER",
            "parameterValues": 
                {"CREDIT_MACC":"12548-CHF", "AMOUNT":"12000"} }
        </textarea> 
      </td>
    </tr>
    <tr>
      <td>Resultat</td>
      <td>
        <textarea class="textarea-code-snippet mb-2" rows="1" cols="63" id="post1" >
705698012
        </textarea>
      </td>
    </tr>
  </tbody>
</table>

## Aufruf einer FlySql Resource in Java Junit Test
<textarea class="textarea-code-snippet" rows="22" cols="70" >
package ebankingtest.bank.ch
...

@Test
void assetPrice_byIsin() {
  RestTemplate restTemplate = new RestTemplate();
  String baseUrl = "http://flysql.uat1.bank.ch";
  String path = "execute/AVQDB-1/sql-scalar";

  URI uri = new URI(baseUrl + path);
  PostDataDto postDataDto = new PostDataDto("/assets/assets-by-market.sql", Map.of("EXCHANGE", "SIX"));

  HttpHeaders headers = new HttpHeaders();
  ...
  HttpEntity<Employee> request = new HttpEntity<>(postDataDto, headers);
  ResponseEntity<String> result = restTemplate.postForEntity(uri, request, String.class);
  ...
  Assert.assertEquals(201, result.getStatusCodeValue());
  String body = result.getBody();
  ...
}

</textarea>
