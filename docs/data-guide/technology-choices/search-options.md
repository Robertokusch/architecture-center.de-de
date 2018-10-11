---
title: Auswählen eines Suchdatenspeichers
description: ''
author: zoinerTejada
ms:date: 02/12/2018
ms.openlocfilehash: 9576bbba9609a04ccc7851d55dd28853ffc6b701
ms.sourcegitcommit: f6be2825bf2d37dfe25cfab92b9e3973a6b51e16
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48858179"
---
# <a name="choosing-a-search-data-store-in-azure"></a>Auswählen eines Suchdatenspeichers in Azure

In diesem Artikel werden die Technologieoptionen für Suchdatenspeicher in Azure verglichen. Mit einem Suchdatenspeicher werden spezialisierte Indizes zum Ausführen von Suchvorgängen für Freiformtext erstellt und gespeichert. Der Text, der indiziert wird, kann sich in einem separaten Datenspeicher befinden, etwa einem Blobspeicher. Eine Anwendung sendet eine Abfrage an den Suchdatenspeicher. Als Ergebnis wird eine Liste der übereinstimmenden Dokumente zurückgegeben. Weitere Informationen zu diesem Szenario finden Sie unter [Processing free-form text for search](../scenarios/search.md) (Verarbeiten von Freiformtext für die Suche). 

## <a name="what-are-your-options-when-choosing-a-search-data-store"></a>Welche Suchdatenspeicher-Optionen stehen zur Verfügung?
In Azure erfüllen durch Bereitstellung eines Suchindexes alle folgenden Datenspeicher die grundlegenden Anforderungen für das Durchsuchen von Freiformtextdaten:
- [Azure Search](/azure/search/search-what-is-azure-search)
- [Elasticsearch](https://azuremarketplace.microsoft.com/marketplace/apps/elastic.elasticsearch?tab=Overview)
- [HDInsight mit Solr](/azure/hdinsight/hdinsight-hadoop-solr-install-linux)
- [Azure SQL-Datenbank mit Volltextsuche](/sql/relational-databases/search/full-text-search)


## <a name="key-selection-criteria"></a>Wichtige Auswahlkriterien

Beginnen Sie bei Suchszenarien mit der Auswahl des geeigneten Suchdatenspeichers für Ihre Anforderungen, indem Sie die folgenden Fragen beantworten:

- Möchten Sie einen verwalteten Dienst verwenden, anstatt Ihre eigenen Server zu verwalten?

- Können Sie Ihr Indexschema zur Entwurfszeit angeben? Wenn dies nicht der Fall ist, wählen Sie eine Option, die aktualisierbare Schemas unterstützt.

- Ist ein Index nur für die Volltextsuche erforderlich, oder benötigen Sie auch eine schnelle Aggregation von numerischen Daten und weitere Analysefunktionen? Wenn Sie nicht nur Funktionen für die Volltextsuche benötigen, ziehen Sie Optionen in Erwägung, die zusätzliche Analysefunktionen unterstützen.

- Benötigen Sie einen Suchindex für die Protokollanalyse mit Unterstützung für Protokollerfassung, Aggregation und Visualisierungen für indizierte Daten? Falls ja, ziehen Sie den Elasticsearch-Engine in Erwägung, der Teil eines Protokollanalysestapels ist.

- Müssen Sie Daten in allgemeinen Dokumentformaten wie PDF, Word, PowerPoint und Excel indizieren? Ist dies der Fall, wählen Sie eine Option, die Dokumentindexer bereitstellt.

- Gelten für Ihre Datenbank bestimmte Sicherheitsanforderungen? Falls ja, ziehen Sie die unten aufgeführten Sicherheitsfeatures in Erwägung.

## <a name="capability-matrix"></a>Funktionsmatrix

In den folgenden Tabellen sind die Hauptunterschiede in Bezug auf die Funktionen zusammengefasst.

### <a name="general-capabilities"></a>Allgemeine Funktionen

| | Azure Search | Elasticsearch | HDInsight mit Solr | SQL-Datenbank | 
| --- | --- | --- | --- | --- | 
| Verwalteter Dienst | JA | Nein | Ja | JA |  
| REST-API | JA | Ja | JA | Nein  |
| Programmierbarkeit | .NET | Java | Java | T-SQL | 
| Dokumentindexer für allgemeine Dateitypen (PDF, DOCX, TXT usw.) | JA | Nein | Ja | Nein  |

### <a name="manageability-capabilities"></a>Verwaltbarkeitsfeatures

| | Azure Search | Elasticsearch | HDInsight mit Solr | SQL-Datenbank | 
| --- | --- | --- | --- | --- |
| Aktualisierbares Schema | Nein  | Ja | Ja | JA |
| Unterstützung für horizontales Hochskalieren  | JA | Ja | JA | Nein  |

### <a name="analytic-workload-capabilities"></a>Funktionen für Analyseworkloads

| | Azure Search | Elasticsearch | HDInsight mit Solr | SQL-Datenbank | 
| --- | --- | --- | --- | --- | 
| Unterstützung von Analysen über die Volltextsuche hinaus | Nein  | Ja | Ja | JA |
| Teil eines Protokollanalysestapels | Nein  | Ja (ELK) |  Nein  | Nein  |
| Unterstützung der semantischen Suche | Ja (nur Suche von ähnlichen Dokumenten) | JA | Ja | JA | 

### <a name="security-capabilities"></a>Sicherheitsfunktionen

| | Azure Search | Elasticsearch | HDInsight mit Solr | SQL-Datenbank | 
| --- | --- | --- | --- | --- | 
| Sicherheit auf Zeilenebene | Teilweise (Anwendungsabfrage zum Filtern nach Gruppen-ID erforderlich) | Teilweise (Anwendungsabfrage zum Filtern nach Gruppen-ID erforderlich) | JA | JA | 
| Transparent Data Encryption | Nein  | Nein  | Nein  | JA |  
| Beschränken des Zugriffs auf bestimmte IP-Adressen | Nein  | Ja | Ja | JA |   
| Beschränken des Zugriffs, um nur den Zugriff auf virtuelle Netzwerke zuzulassen | Nein  | Ja | Ja | JA |  
| Active Directory-Authentifizierung (integrierte Authentifizierung) | Nein  | Nein  | Nein  | JA | 

## <a name="see-also"></a>Weitere Informationen

[Processing free-form text for search](../scenarios/search.md) (Verarbeiten von Freiformtext für die Suche)
