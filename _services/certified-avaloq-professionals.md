---
title: "Certified Avaloq Professionals"
date: 2022-06-01T00:00:00+02:00
featured: true
weight: 1
---

Avaloq Certified Professionals stellen ihre Expertise zur Verfügung, um Avaloq Scripte und SQL Statements zu erstellen. Diese erstellten Scripte und Statements können mit Parametern versehen werden und werden FlySql durch ein Git Repository zur Verfügung gestellt. FlySql stellt diesse Scripte und Statements als REST Resourcen zu Verfügung, um gezielte Interaktionen mit Avaloq im Testablauf zu ermöglichen. 

## Werkzeuge
- Avaloq Script Editor: Avaloq ICE
- SQL und PL/SQL Editoren: Toadt, SQL Developper, Notepad++
- Git Werkzeuge: Git Bash, Tortoise, Sourcetree
- Browser: um FlySql Resourcen anzuschauen oder AdHoc Queries bei der Erstellung neuer Statements und Scripts auszuführen

## Beispiele
### SQL Statement mit Parametern
```
SELECT ebankingid as LOGIN, loginid as EMAILADRESS
FROM ebankinglogintable
WHERE active = '+' and loginid = '${EMAILADRESS}'; 
```
### Avaloq Statement mit Parametern
```
lookup.mkt('${MARKET_KEY}'
   , key_type => 30
   , eff_date => session.today)
   .name
```
### Avaloq Script mit Parametern
```
DECLARE
  l_cred_macc_id number;
  l_deb_macc_id number;
  /* more declarations here */
  l_doc_id number;
  l_return number;

BEGIN
  l_cred_macc_id := lookup.macc_id('${CREDIT_CONTAINER_MACC}');
  l_deb_macc_id := 20195226;
  l_trx_date := lookup.date('-1v');
  l_amount := ${AMOUNT};
  /* more statements here */
  
  with new mem_doc_xfermon(1) as tdoc do
    tdoc.deb_macc_id := l_deb_macc_id;
    tdoc.cred_macc_id := l_cred_macc_id;
    /* more declarations here */
    l_doc_id := tdoc.doc_id;
  end with;
  
  with mem_doc_xfermon(session.doc_mgr.load_doc(l_doc_id)) as tdoc do
    tdoc.do_wfc_action(4199);
  end with;

  return l_doc_id;

EXCEPTION
  /* error handling here */
END;
```