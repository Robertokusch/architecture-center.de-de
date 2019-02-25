<!-- TEMPLATE FILE - DO NOT ADD METADATA -->

### <a name="governance-of-resources"></a>Governance von Ressourcen

Die abonnementübergreifende Erzwingung von Governance erfolgt über Azure Blueprints und die zugehörigen Ressourcen innerhalb der Blaupause.

1. Erstellen Sie eine Azure-Blaupause namens „Governance-MVP“.
    1. Erzwingen Sie die Verwendung von Azure-Standardrollen.
    2. Erzwingen Sie, dass Benutzer sich nur bei einer vorhandenen RBAC-Implementierung authentifizieren können.
    3. Wenden Sie diese Blaupause auf alle Abonnements in der Verwaltungsgruppe an.
2. Erstellen Sie eine Azure Policy-Instanz, um Folgendes anzuwenden oder zu erzwingen:
    1. Für die Ressourcenmarkierungen müssen Werte für die Geschäftsfunktion, die Datenklassifizierung, die Wichtigkeit, die SLA, die Umgebung und die Anwendung angefordert werden.
    2. Der Wert der Anwendungsmarkierung muss dem Namen der Ressourcengruppe entsprechen.
    3. Überprüfen Sie die Rollenzuweisungen für jede Ressourcengruppe und Ressource.
3. Veröffentlichen Sie die Azure-Blaupause „Governance-MVP“, und wenden Sie sie auf die einzelnen Verwaltungsgruppen an.

Anhand dieser Muster können die Ressourcen ermittelt und nachverfolgt werden. Zudem wird dadurch eine grundlegende Rollenverwaltung erzwungen.

### <a name="demilitarized-zone-dmz"></a>Demilitarisierte Zone (DMZ)

Für bestimmte Abonnements ist es üblich, eine bestimmte Zugriffsebene für lokale Ressourcen anzufordern. Dies kann bei Migrationsszenarien oder Entwicklungsszenarien der Fall sein, wenn sich einige abhängige Ressourcen weiterhin im lokalen Rechenzentrum befinden. In diesem Fall fügt das Governance-MVP die folgenden bewährten Methoden hinzu:

1. Richten Sie eine Cloud-DMZ ein.
    1. Die [Cloud-DMZ-Referenzarchitektur](/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) richtet ein Muster und ein Bereitstellungsmodell zur Erstellung einer VPN Gateway-Instanz in Azure ein.
    2. Stellen Sie sicher, dass die richtige DMZ-Konnektivität und Sicherheitsanforderungen für ein lokales Edgegerät im lokalen Rechenzentrum eingerichtet sind.
    3. Stellen Sie sicher, dass das lokale Edgegerät mit den Azure VPN Gateway-Anforderungen kompatibel ist.
    4. Sobald die Verbindung mit dem lokalen VPN überprüft wurde, erfassen Sie die Resource Manager-Vorlage, die von dieser Referenzarchitektur erstellt wurde.
2. Erstellen Sie eine zweite Blaupause namens „DMZ“.
    1. Fügen Sie der Blaupause die Resource Manager-Vorlage für die VPN Gateway-Instanz hinzu.
3. Wenden Sie die DMZ-Blaupause auf alle Abonnements an, die lokale Konnektivität benötigen. Diese Blaupause muss zusätzlich zu der Governance-MVP-Blaupause angewendet werden.

Eines der größten Probleme, die vom IT-Sicherheitsteam und von herkömmlichen Governanceteams angemerkt werden, ist das Risiko, dass vorhandene Ressourcen durch eine frühe Phase der Cloudeinführung kompromittiert werden. Der oben beschriebene Ansatz ermöglicht Cloudeinführungsteams das Erstellen und Migrieren von Hybridlösungen mit verringertem Risiko für lokale Ressourcen. In der späteren Entwicklung wird diese temporäre Lösung entfernt.

> [!NOTE]
> Der obige Ansatz ist ein Ausgangspunkt für eine schnelle Erstellung eines Baselinegovernance-MVP. Dies ist erst der Anfang des Governancevorgangs. Eine Weiterentwicklung ist erforderlich, wenn das Unternehmen die Cloudeinführung fortsetzt und in folgenden Bereichen höhere Risiken eingeht:
>
> - Unternehmenskritische Workloads
> - Datenschutz
> - Kostenverwaltung
> - Szenarien mit mehreren Clouds
>
>Darüber hinaus basieren die besonderen Details dieses MVP auf dem Beispielvorgang eines fiktiven Unternehmens, der in den nachfolgenden Artikeln beschrieben wird. Vor der Implementierung dieser bewährten Methode wird dringend empfohlen, sich mit den anderen Artikeln dieser Serie vertraut zu machen.
