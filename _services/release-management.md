---
title: "Release Management"
date: 2022-06-01T00:00:00+02:00
featured: true
weight: 4
---

Avaloq Release Manager stellen für die jeweiligen Avaloq Umgebungen FlySql Services zur Verfügung. Eine Herausgabe des K - Passworts ist nicht notwendig. Zugriffe auf die Avaloq Testumgebung können durch einen einfachen Stop des FlySql Service unterbunden werden, zum Beispiel für Jahresendtests oder Wartungsarbeiten.

## Werkzeuge
- Kubernetes Plattform Werkzeuge
- PipelineScripts: Bamboo, Jenkins

## Beispiel Konfiguration
<textarea class="textarea-sql mb-2" rows="6" cols="50" id="post1" >
avaloq:
  db:
    - sid:
      username: K
      password: flySqlAway
      connectionstring: flysql01.net:1527:DB1
</textarea>
