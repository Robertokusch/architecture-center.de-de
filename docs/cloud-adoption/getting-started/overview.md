---
title: 'CAF: Erste Schritte mit dem Framework für die Cloudeinführung (Cloud Adoption Framework)'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: Bietet eine Übersicht über die erste Phase der digitalen Transformation eines Unternehmens, das Azure-Cloudtechnologien einführt.
ms.date: 02/11/2019
ms.openlocfilehash: 29b02964b5f1cd09857a51c17cf94d6c64e05e88
ms.sourcegitcommit: 0a8a60d782facc294f7f78ec0e9033e3ee16bf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068955"
---
# <a name="getting-started-with-the-cloud-adoption-framework"></a>Erste Schritte mit dem Framework für die Cloudeinführung (Cloud Adoption Framework)

Die **digitale Transformation** zu Cloud Computing bedeutet eine Verlagerung von der lokalen Umgebung auf eine Cloudumgebung. Diese Verlagerung wirkt sich auch auf geschäftliche Abläufe aus. So werden beispielsweise Investitionsaufwendungen für Software und Datencenterhardware zu Betriebsaufwendungen für die Nutzung von Cloudressourcen. Betrachten wir, wie Sie mit dem [Microsoft Cloud Adoption Framework für Azure](../overview.md) beginnen können.

## <a name="the-digital-transformation-process"></a>Der digitale Transformationsprozess

Für eine erfolgreiche Cloudeinführung müssen Unternehmen ihre Organisation, Mitarbeiter und Prozesse auf die digitale Transformation vorbereiten. Da jedes Unternehmen eine andere Organisationsstruktur besitzt, gibt es kein allgemeingültiges Konzept für die organisatorische Bereitschaft. In diesem Dokument werden daher die allgemeinen Schritte beschrieben, die Ihr Unternehmen zur Vorbereitung ausführen kann. Ihre Organisation muss einen detaillierten Plan für jeden der aufgeführten Schritte ausarbeiten.

Die digitale Transformation umfasst folgende allgemeine Schritte:

1. Stellen Sie ein Cloudstrategieteam zusammen. Dieses Team ist für die Durchführung der digitalen Transformation zuständig. Darüber hinaus müssen in dieser Phase auch ein Governance-Team und ein Sicherheitsteam für die digitale Transformation zusammengestellt werden.
2. Die Mitglieder des Cloudstrategieteams informieren sich darüber, was bei Cloudtechnologien neu und anders ist.
3. Zur Vorbereitung des Unternehmens erarbeitet das Cloudstrategieteam den Business Case für die digitale Transformation. Dazu erstellt es eine Liste mit allen aktuellen Lücken in der Unternehmensstrategie und ermittelt allgemeine Lösungen, um diese Lücken zu schließen.
4. Stimmen Sie die allgemeinen Lösungen mit Unternehmensgruppen ab. Bestimmen Sie Projektbeteiligte in den einzelnen Unternehmensgruppen, die sich um den Entwurf und die Implementierung der jeweiligen Lösung kümmern.
5. Übertragen Sie vorhandene Rollen, Fähigkeiten und Prozesse auf cloudbasierte Rollen, Fähigkeiten und Prozesse.

<!--6. Develop processes for operating in the cloud to make solutions more robust in terms of availability, resiliency, and security.
1. Optimize solutions for performance, scalability, and cost efficiency.-->

## <a name="step-1-create-a-cloud-strategy-team"></a>Schritt 1: Zusammenstellen eines Cloudstrategieteams

Der erste Schritt auf dem Weg zur digitalen Transformation Ihres Unternehmens ist die Bildung eines Cloudstrategieteams (CST) unter Einbeziehung von Führungskräften aus der gesamten Organisation. Dieses Team setzt sich aus Führungskräften der Finanz-, IT-Infrastruktur- und Anwendungsgruppen zusammen. Diese Teams können bei der Cloudanalyse- und Experimentierphase mitwirken.

Ein Cloudstrategieteam kann beispielsweise vom CTO geleitet werden und Mitglieder des Teams für die Unternehmensarchitektur, Mitarbeiter der IT-Finanzabteilung, erfahrene Technologieexperten aus verschiedenen IT-Anwendungsgruppen (Personalabteilung, Finanzabteilung usw.) sowie Führungskräfte aus dem Infrastruktur-, Sicherheits- und Netzwerkteam umfassen.

Darüber hinaus müssen noch zwei weitere übergeordnete Teams gebildet werden: ein Governance-Team und Sicherheitsteam. Diese Teams sind zuständig für die Gestaltung, die Implementierung und die fortlaufende Überwachung der Governance- und Sicherheitsrichtlinien des Unternehmens. Das Governance-Team muss Personen enthalten, die mit Ressourcenschutz, Kostenverwaltung, Gruppenrichtlinien und Ähnlichem vertraut sind. Die Mitglieder des Sicherheitsteams müssen bestens mit aktuellen branchenspezifischen Sicherheitsstandards sowie mit den Sicherheitsanforderungen des Unternehmens vertraut sein.

![Cloudstrategieteam, mit Governance- und Sicherheitsteams](../_images/getting-started-overview-1.png)

Das Governance-Team ist dafür zuständig, das Governance-Modell des Unternehmens zu entwerfen und in der Cloud zu implementieren sowie die Ressourcen der freigegebenen Infrastruktur, die Teil der digitalen Transformation sind, bereitzustellen und zu verwalten. Hierzu zählen Hardware-, Software- und Cloudressourcen, die benötigt werden, um eine Verbindung zwischen dem lokalen Netzwerk und dem virtuellen Netzwerk in der Cloud herzustellen.

Die Aufgabe des Sicherheitsteams besteht darin, die Sicherheitsrichtlinie des Unternehmens in der Cloud zu entwerfen und zu implementieren. Dabei arbeitet es eng mit dem Governance-Team zusammen. Das Sicherheitsteam ist für die Erweiterung der Sicherheitsgrenze des lokalen Netzwerks auf das virtuelle Netzwerk in der Cloud zuständig. Dazu muss sich das Team unter Umständen um die Firewalls für ein- und ausgehenden Datenverkehr des virtuellen Cloudnetzwerks kümmern sowie sicherstellen, dass Tools und Richtlinien die Bereitstellung nicht autorisierter Ressourcen verhindern.

## <a name="step-2-learn-whats-new-in-the-cloud"></a>Schritt 2: Informieren über Neuerungen in der Cloud

Im nächsten Schritt der digitalen Transformation Ihres Unternehmens müssen sich die Mitglieder des Cloudstrategieteams darüber informieren, inwiefern sich die Cloudtechnologie auf die geschäftlichen Abläufe des Unternehmens auswirkt. Dieser Schritt dient zur Vorbereitung und Planung von Veränderungen für das Unternehmen, die Mitarbeiter und die Technologie. Die Mitglieder des Cloudstrategieteams müssen die Neuerungen und Unterschiede der Cloud im Vergleich zur lokalen Umgebung kennen.

![Cloudstrategie-, Governance- und Sicherheitsteams machen sich mit den Best Practices für die Cloud vertraut.](../_images/getting-started-overview-2.png)

Dazu müssen sie sich zunächst ganz allgemein mit der [Funktionsweise von Azure](what-is-azure.md) vertraut machen. Danach müssen sie sich über die [Grundlagen der Governance in Azure](what-is-governance.md) informieren, bevor sie sich schließlich mit den [Grundlagen der Ressourcenzugriffsverwaltung](azure-resource-access.md) befassen.

Darüber hinaus sollte sich das Governance-Team mit den Konzepten und Entwurfshandbüchern beschäftigen, die im Inhaltsverzeichnis im Abschnitt zu Governance zu finden sind. Die Abschnitte zu Infrastruktur und Workloads enthalten hilfreiche Informationen zu gängigen Architekturen und Workloads in der Cloud.

## <a name="step-3-identify-gaps-in-business-strategy"></a>Schritt 3: Ermitteln von Lücken in der Unternehmensstrategie

In diesem Schritt erstellt das Cloudstrategieteam eine Liste mit den geschäftlichen Problemen, die einer Lösung für die digitale Transformation bedürfen. Ein Unternehmen kann beispielweise über ein lokales Datencenter mit älterer Hardware verfügen, die ausgetauscht werden muss. Ein weiteres Beispiel wäre ein Unternehmen, das möglicherweise Probleme mit der Markteinführung neuer Features und Dienste hat und deshalb nicht mehr mit den Mitbewerbern Schritt halten kann. Diese Lücken stellen die *Ziele* der digitalen Transformation Ihres Unternehmens dar.

Lücken in der Unternehmensstrategie können wie folgt kategorisiert werden:

|Category (Kategorie)|BESCHREIBUNG|
|-----|-----|
|Kostenverwaltung|Eine Lücke bei der Bezahlung für Technologie durch das Unternehmen.|
|Governance|Eine Lücke bei den Prozessen, mit denen das Unternehmen seine Ressourcen vor nicht ordnungsgemäßer Verwendung schützt, die möglicherweise zu Budgetüberschreitungen oder zu Sicherheits- oder Kompatibilitätsproblemen führen. |
|Compliance|Eine Lücke bei der Einhaltung interner Prozesse und Richtlinien sowie externer Gesetze, Bestimmungen und Standards. |
|Sicherheit|Eine Lücke beim Schutz der Technologie und Datenressourcen vor externen Bedrohungen. |
|Datengovernance|Eine Lücke bei der Verwaltung der Daten des Unternehmens (insbesondere Kundendaten). So gelten beispielsweise in der Europäischen Union aufgrund der neuen Datenschutz-Grundverordnung (DSGVO) strenge Vorgaben für den Schutz von Kundendaten, die ggf. neue Hardware und Software erfordern.|

Nachdem Ihr Unternehmen alle Lücken in der Unternehmensstrategie entsprechend kategorisiert hat, müssen als Nächstes allgemeine Lösungen für die einzelnen Probleme gefunden werden.

Die folgende Tabelle enthält einige Beispiele dazu:

|Lücke in der Unternehmensstrategie|Kategorie &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|Lösung &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
|-----|-----|-----|
| Bei lokal gehosteten Diensten kommt es zu Spitzenzeiten mit hoher Auslastung (etwa zehn Prozent der Nutzung) zu Problemen mit der Verfügbarkeit, Resilienz und Skalierbarkeit. Das lokale Datencenter enthält veraltete Server. Die IT-Abteilung des Unternehmens rät zum Kauf neuer Hardware für das lokale Datacenter, die zur Bewältigung von Auslastungsspitzen geeignet ist.| Kostenverwaltung | Migrieren Sie betroffene lokale Workloads zu skalierbaren Ressourcen in der Cloud, und bezahlen Sie lediglich für die Nutzung. |
| Aufgrund von externen Gesetzen und Vorschriften zur Datenverwaltung muss das Unternehmen eine Reihe von Standardkontrollen umsetzen, die die Verschlüsselung ruhender Daten erfordern, wofür wiederum neue Hardware und Software nötig sind. | Datengovernance | Migrieren Sie ruhende Daten nach Azure Storage Service Encryption. |
| Bei öffentlichen, in einem lokalen Datencenter gehosteten Diensten kam es zu DDoS-Angriffen (Distributed Denial of Service). Die Angriffe lassen sich nur schwer abwehren und erfordern neue Hardware und Software sowie neues Sicherheitspersonal, um wirkungsvolle Maßnahmen zu ergreifen. | Sicherheit | Migrieren Sie die Dienste nach Azure, um Azure DDoS Protection zu nutzen.|

Nachdem alle Lücken in der Geschäftsstrategie aufgelistet und allgemeine Lösungen gefunden wurden, muss die Liste priorisiert werden. Hierzu können Sie die Lücken in der Geschäftsstrategie mit den kurz- und langfristigen Unternehmenszielen in der jeweiligen Kategorie abgleichen. Wenn das Unternehmen also kurzfristig die IT-Ausgaben in den nächsten beiden Geschäftsquartalen senken möchte, können die Lücken in der Kategorie *Kostenverwaltung* auf der Grundlage der jeweils zu erwartenden Kosteneinsparungen priorisiert werden.

So erhalten Sie eine nach Rangfolge sortierte Liste allgemeiner Lösungen für Unternehmenskategorien.

## <a name="step-4-align-high-level-solutions-with-business-groups-to-design-solutions"></a>Schritt 4: Abstimmen der allgemeinen Lösungen mit Unternehmensgruppen, um Lösungen zu erarbeiten

Nachdem es eine Liste mit den Zielen der digitalen Transformation erstellt sowie die Liste priorisiert und allgemeine Lösungen vorgeschlagen hat, besteht der nächste Schritt für das Cloudstrategieteam darin, die einzelnen allgemeinen Lösungen mit Entwurfs- und Implementierungsteams in den einzelnen Unternehmensgruppen abzugleichen.

Die Teams arbeiten sich anhand der priorisierten Listen durch die einzelnen allgemeinen Lösungen, um einen entsprechenden Entwurf zu erarbeiten. Der Entwurfsprozess umfasst unter anderem Angaben zu neuer Infrastruktur und neuen Workloads. Unter Umständen verändern sich auch die Rollen der Mitarbeiter sowie die Prozesse, mit denen sie arbeiten. In dieser Phase ist es zudem äußerst wichtig, dass jedes Entwurfsteam sowohl das Governance- als auch das Sicherheitsteam in die Prüfung der jeweiligen Entwürfe mit einbezieht. Jeder Entwurf muss sich innerhalb der Richtlinien und Verfahren bewegen, die das Governance- und das Sicherheitsteam definiert haben, und diese Teams müssen in die endgültige Freigabe des jeweiligen Entwurfs eingebunden werden.

![Cloudstrategieteam übergibt die allgemeinen Lösungen an Entwurfs- und Implementierungsteams.](../_images/getting-started-overview-3.png)

Das Entwerfen einer Lösung ist eine anspruchsvolle Aufgabe, und der Entwurf muss jeweils im Kontext anderer Lösungsentwürfe von anderen Teams betrachtet werden. Wenn beispielsweise mehrere der Entwürfe die Migration bereits vorhandener lokaler Anwendungen und Dienste in die Cloud vorsehen, ist es unter Umständen effizienter, diese zusammenzufassen und eine allgemeine Migrationsstrategie zu erarbeiten. In einem anderen Fall können beispielsweise einige vorhandene lokale Anwendungen und Dienste nicht migriert werden, und die Lösung besteht darin, sie entweder durch Neuentwicklungen oder durch Dienste von Drittanbietern zu ersetzen. In diesem Fall ist es ggf. effizienter, sie zusammenzufassen und die Überschneidung zwischen ihnen zu ermitteln, um zu bestimmen, ob ein Drittanbieterdienst für mehrere Lösungen verwendet werden kann.

Wenn der Lösungsentwurf fertig ist, beginnt für das Team die Implementierungsphase des jeweiligen Entwurfs. Die Implementierungsphase für den jeweiligen Lösungsentwurf kann mithilfe gängiger Projektmanagementprozesse durchgeführt werden.

## <a name="step-5-translate-existing-roles-skills-and-process-for-the-cloud"></a>Schritt 5: Übertragen vorhandener Rollen, Fähigkeiten und Prozesse auf die Cloud

In jeder Evolutionsphase der IT-Geschichte ging mit besonders bemerkenswerten Veränderungen der Branche häufig auch eine Veränderung der Personalrollen einher. Während der Umstellung von Mainframes auf das Client-Server-Modell wurden Anlagenbediener größtenteils durch Systemadministratoren ersetzt. Mit Anbruch des Virtualisierungszeitalters ging der Bedarf für Personen, die mit physischen Servern arbeiten, zurück. Stattdessen waren nun Experten für Virtualisierung gefragt. Und auch beim Umstieg auf Cloud Computing ist mit ähnlichen Veränderungen zu rechnen. So können Datencenterspezialisten beispielsweise durch Cloud-Finanzanalytiker ersetzt werden. Aber auch in Fällen, in denen die Bezeichnungen für IT-Berufe nicht geändert wurden, haben sich die alltäglichen Arbeitsaufgaben deutlich gewandelt.

IT-Mitarbeiter machen sich möglicherweise Sorgen um ihre Rollen und Positionen, da ihnen bewusst wird, dass sie für Cloudlösungen andere Fähigkeiten benötigen. Flexible Mitarbeiter, die sich für neue Cloudtechnologien interessieren und sich mit ihnen vertraut machen, müssen sich jedoch keine Gedanken machen. Sie können bei der Einführung von Clouddiensten eine Führungsrolle übernehmen und der Organisation dabei helfen, die entsprechenden Veränderungen zu verstehen und umzusetzen.

### <a name="capturing-concerns"></a>Dokumentieren von Problemen

Während der digitalen Transformation sollte jedes Team mögliche Probleme der Mitarbeiter dokumentieren, sobald diese auftreten. Dabei muss Folgendes dokumentiert werden:

* Die Art des Problems. Mitarbeiter können sich beispielsweise gegen die veränderten Aufgaben sperren, die mit der digitalen Transformation einhergehen.
* Die Auswirkungen, wenn das Problem nicht behandelt wird. Der Widerstand gegen die digitale Transformation kann beispielsweise dazu führen, dass nötige Änderungen nur schleppend umgesetzt werden.
* Der Bereich, der zur Behebung des Problems geeignet ist. Wenn beispielsweise IT-Mitarbeiter nicht bereit sind, sich neue Fähigkeiten anzueignen, ist der IT-Bereich am besten geeignet, dieses Problem zu beheben. Bei manchen Problemen ist der Bereich eindeutig identifizierbar. In diesem Fall ist ggf. eine Eskalation an die Unternehmensleitung erforderlich.

### <a name="identify-gaps"></a>Ermitteln von Lücken

Ein weiterer Aspekt bei der Bewältigung von Problemen mit der digitalen Transformation Ihres Unternehmens ist die Ermittlung von **Lücken**. Bei Lücken handelt es sich um Rollen, Fähigkeiten oder Prozesse, die für die digitale Transformation erforderlich, aber derzeit in Ihrem Unternehmen nicht vorhanden sind.

Erstellen Sie zunächst eine Liste mit den neuen Aufgaben, die die digitale Transformation mit sich bringt, und konzentrieren Sie sich dabei auf neue Aufgaben sowie auf aktuelle Aufgaben, die nicht mehr benötigt werden. Ermitteln Sie jeweils den Bereich, der der Aufgabe zugeordnet ist. Bestimmen Sie bei neuen Aufgaben, wie eng sie mit dem Bereich verknüpft sind. Da einige Aufgaben unter Umständen mehrere Bereiche umfassen, ist dies eine gute Gelegenheit für eine bessere Ausrichtung, die als Problem dokumentiert werden sollte. Sollte kein zuständiger Bereich gefunden werden, dokumentieren Sie dies als Lücke.

Ermitteln Sie als Nächstes die Fähigkeiten, die für die Aufgabe nötig sind. Bestimmen Sie, ob Ihr Unternehmen über vorhandene Ressourcen mit diesen Fähigkeiten verfügt. Sollten keine entsprechenden Ressourcen vorhanden sein, ermitteln Sie, welche Schulungsprogramme oder Anwerbungsmaßnahmen erforderlich sind. Bestimmen Sie den Zeitrahmen, in dem die Aufgabe unterstützt werden muss, um den Zeitplan Ihrer digitalen Transformation einzuhalten.

Ermitteln Sie abschließend die Rollen, die diese Fähigkeiten umsetzen. Einige Ihrer vorhandenen Mitarbeiter können Teile der Rolle übernehmen, in manchen Fällen ist jedoch eine ganz neue Rolle erforderlich.

### <a name="partner-across-teams"></a>Teamübergreifende Zusammenarbeit

Die Fähigkeiten, die zum Füllen der Lücken in der digitalen Transformation Ihrer Organisation erforderlich sind, sind in der Regel nicht auf eine einzelne Rolle und häufig nicht einmal auf eine einzelne Abteilung beschränkt. Die Beziehungen und Abhängigkeiten von Fähigkeiten können sich über mehrere Rollen erstrecken, die sich wiederum in verschiedenen Abteilungen befinden können. Ein Workloadbesitzer kann beispielsweise darauf angewiesen sein, dass ihm ein IT-Mitarbeiter grundlegende Ressourcen wie Abonnements und Ressourcengruppen zur Verfügung stellt.

Diese Abhängigkeiten stellen neue Prozesse dar, die Ihre Organisation implementiert, um den Workflow zwischen Rollen zu verwalten. Im obigen Beispiel gibt es verschiedene Arten von Prozessen, die die Beziehung zwischen dem Workloadbesitzer und der IT-Rolle unterstützen können. So kann beispielsweise ein Workflowtool erstellt werden, um den Prozess zu verwalten, oder es kann eine einfache E-Mail-Vorlage verwendet werden.

Verfolgen Sie diese Abhängigkeiten, notieren Sie sich die Prozesse, die ihnen zugrunde liegen, und geben Sie an, ob der Prozess derzeit vorhanden ist. Stellen Sie bei Prozessen, die Tools erfordern, sicher, dass der Zeitplan für die Toolbereitstellung mit dem übergeordneten Zeitplan der digitalen Transformation in Einklang steht.

## <a name="next-steps"></a>Nächste Schritte

Die digitale Transformation ist ein iterativer Prozess, und jede weitere Iteration macht die beteiligten Teams effizienter.

> [!div class="nextstepaction"]
> [Wie funktioniert Azure?](what-is-azure.md)
