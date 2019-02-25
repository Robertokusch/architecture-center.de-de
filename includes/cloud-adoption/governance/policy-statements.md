<!-- TEMPLATE FILE - DO NOT ADD METADATA -->

## <a name="policy-statements"></a>Richtlinienanweisungen

Die folgenden Richtlinienanweisungen legen die Anforderungen zum Verringern der definierten Risiken fest. Diese Richtlinien definieren die Funktionsanforderungen für das Governance-MVP. Jede wird in der Implementierung der Governance-MVP dargestellt.

Beschleunigung der Bereitstellung:

- Alle Ressourcen müssen gruppiert und gemäß der definierten Gruppierungs- und Markierungsstrategien markiert werden.
- Alle Ressourcen müssen ein genehmigtes Bereitstellungsmodell verwenden.
- Sobald eine Governancegrundlage für einen Cloudanbieter festgelegt ist, müssen alle Tools für die Bereitstellung mit den Tools kompatibel sein, die vom Governanceteam definiert werden.

Identitätsbaseline:

- Alle in der Cloud bereitgestellten Ressourcen sollten mit Identitäten und Rollen kontrolliert werden, die von aktuellen Governancerichtlinien genehmigt werden.
- Alle Gruppen in der lokalen Active Directory-Infrastruktur mit erhöhten Rechten sollten einer genehmigten RBAC-Rolle zugeordnet werden.

Sicherheitsbaseline:

- Jede in der Cloud bereitgestellte Ressource muss eine genehmigte Datenklassifizierung haben.
- Keine Ressourcen, die mit einer geschützten Datenebene identifiziert werden, können in der Cloud bereitgestellt werden, solange nicht genügend Anforderungen für Sicherheit und Governance genehmigt und implementiert werden können.
- Solange keine minimalen Netzwerksicherheitsanforderungen überprüft und gesteuert werden können, werden Cloudumgebungen als demilitarisierte Zone betrachtet und sollten ähnliche Anforderungen an die Verbindung mit anderen Rechenzentren oder internen Netzwerken erfüllen.

Kostenmanagement:

- Für die Nachverfolgung müssen alle Ressourcen innerhalb einer der zentralen Geschäftsfunktionen einem Anwendungsbesitzer zugewiesen werden.
- Wenn Kostenprobleme auftreten, werden zusätzliche Governanceanforderungen mit dem Finanzteam festgelegt.

Ressourcenkonsistenz:

- Da in dieser Phase keine unternehmenskritischen Workloads bereitgestellt werden, müssen keine SLA-, Leistungs- oder BCDR-Anforderungen verwaltet werden.
- Wenn unternehmenskritische Workloads bereitgestellt werden, werden zusätzliche Governanceanforderungen mit der IT-Abteilung festgelegt.

## <a name="processes"></a>Prozesse

Für die kontinuierliche Überwachung und Durchsetzung dieser Governancerichtlinien wurde kein Budget zugeordnet. Aus diesem Grund stehen dem Cloudgovernanceteam einige Ad-hoc-Methoden zur Verfügung, um die Einhaltung von Richtlinienanweisungen zu überwachen.

- **Bildung**: Das Cloudgovernanceteam investiert Zeit in die Schulung der Cloudeinführungsteams zu den Governance Journeys, die diese Richtlinien unterstützen.
- **Bereitstellungsprüfungen**: Vor der Bereitstellung einer Ressource überprüft das Cloudgovernanceteam die Governance Journey mit den Cloudeinführungsteams.
