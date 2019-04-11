---
title: 'CAF: Verschlüsselung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Erfahren Sie mehr über die Verschlüsselung als Kerndienst bei Azure-Migrationen.
author: rotycenh
ms.openlocfilehash: 8735800fe481a0e0292a6a0c7555e883e29f8ee2
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241761"
---
# <a name="encryption-decision-guide"></a>Leitfaden zur Entscheidungsfindung für die Verschlüsselung

Das Verschlüsseln von Daten schützt vor nicht autorisiertem Zugriff. Eine ordnungsgemäß implementierte Verschlüsselung bietet zusätzliche Ebenen der Sicherheit für Ihre cloudbasierten Workloads und dient als Schutz vor Angreifern und anderen nicht autorisierten Benutzern – innerhalb und außerhalb Ihrer Organisation und Ihrer Netzwerke.

![Abbildung der Verschlüsselungsoptionen mit zunehmender Komplexität entsprechend den nachstehenden weiterführenden Links](../../_images/discovery-guides/discovery-guide-encryption.png)

Wechseln Sie zu: [Schlüsselverwaltung](#key-management) | [Datenverschlüsselung](#data-encryption) | [Weitere Informationen](#learn-more)

Bei einer cloudbasierten Verschlüsselungsstrategie stehen die Richtlinien- und Compliancevorgaben des Unternehmens im Mittelpunkt. Die Verschlüsselung von Ressourcen ist generell wünschenswert und in vielen Azure-Diensten (etwa in Azure Storage und in Azure SQL-Datenbank) standardmäßig aktiviert. Die Verschlüsselung stellt jedoch einen Zusatzaufwand dar, der zu einer höheren Wartezeit sowie zu einer allgemein höheren Ressourcennutzung führen kann.

Bei anspruchsvollen Workloads muss daher die richtige Balance zwischen Verschlüsselung und Leistung gefunden werden. Ein weiterer entscheidender Faktor ist die Art der Verschlüsselung von Daten und Datenverkehr. Verschlüsselungsmechanismen können sich in Sachen Kosten und Komplexität unterscheiden, und sowohl technische als auch richtlinienbedingte Anforderungen können Ihre Entscheidungen im Zusammenhang mit der Anwendung der Verschlüsselung sowie mit der Speicherung und Verwaltung kritischer Geheimnisse und Schlüssel beeinflussen.

Unternehmensrichtlinien und Konformität mit Drittanbietern sind die wichtigsten Aspekte bei der Planung einer Verschlüsselungsstrategie. Azure bietet mehrere Standardmechanismen zur Erfüllung allgemeiner Datenverschlüsselungsanforderungen für ruhende und übertragene Daten. Für Richtlinien und Complianceanforderungen, die eine strengere Kontrolle erfordern (etwa eine standardisierte Geheimnis- und Schlüsselverwaltung, eine Verschlüsselung während der Verwendung oder eine datenspezifische Verschlüsselung), ist dagegen eine komplexere Verschlüsselungsstrategie erforderlich.

## <a name="key-management"></a>Schlüsselverwaltung

Moderne Schlüsselverwaltungssysteme sollten für einen besseren Schutz Unterstützung für die Speicherung von Schlüsseln mithilfe von Hardwaresicherheitsmodulen (HSMs) bieten. Daher ist ein Schlüsselverwaltungssystem enorm wichtig, damit Ihre Organisation kryptografische Schlüssel, wichtige Kennwörter, Verbindungszeichenfolgen und andere vertrauliche IT-Informationen erstellen und speichern kann.

Die folgende Tabelle für die Planung einer Cloudmigration zeigt, wie Sie Verschlüsselungsschlüssel, Zertifikate und Geheimnisse, die zum Erstellen sicherer und verwaltbarer Cloudbereitstellungen kritisch sind, speichern und verwalten können:

| Frage | Cloudnativ | Hybrid | Lokal |
|---------------------------------------------------------------------------------------------------------------------------------------|--------------|--------|-------------|
| Fehlt in Ihrer Organisation eine zentralisierte Schlüssel- und Geheimnisverwaltung?                                                                    | Ja          | Nein      | Nein           |
| Muss die Erstellung von Schlüsseln und Geheimnissen für Geräte auf lokale Hardware beschränkt werden, wenn diese Schlüssel in der Cloud verwendet werden? | Nein            | Ja    | Nein           |
| Verfügt Ihre Organisation über Regeln oder Richtlinien, die eine externe Speicherung von Schlüsseln verhindern?                | Nein            | Nein      | Ja         |

### <a name="cloud-native"></a>Cloudbasiert

Bei der cloudbasierten Schlüsselverwaltung werden alle Schlüssel und Geheimnisse in einem cloudbasierten Tresor (beispielsweise in Azure Key Vault) generiert, verwaltet und gespeichert. Dieser Ansatz vereinfacht viele IT-Aufgaben im Zusammenhang mit der Schlüsselverwaltung – etwa das Sichern, Speichern und Verlängern von Schlüsseln.

Für die Verwendung eines cloudnativen Schlüsselverwaltungssystems wird Folgendes angenommen:

- Sie vertrauen der Cloudlösung für die Schlüsselverwaltung beim Erstellen, Verwalten und Hosten der Geheimnisse und Schlüssel Ihres Unternehmens.
- Sie ermöglichen allen lokalen Anwendungen und Diensten, die auf den Zugriff auf Verschlüsselungsdienste oder Geheimnisse angewiesen sind, den Zugriff auf das Schlüsselverwaltungssystem in der Cloud.

### <a name="hybrid-bring-your-own-key"></a>Hybrid (Bring Your Own Key)

Bei diesem Ansatz generieren Sie die Schlüssel auf dedizierter HSM-Hardware in Ihrer lokalen Umgebung und übertragen die Schlüssel dann in einen sicheren Schlüsseltresor in der Cloud, damit sie für Cloudressourcen verwendet werden können.

Annahmen für die hybride Schlüsselverwaltung: Für die Verwendung eines hybriden Schlüsselverwaltungssystems wird Folgendes angenommen:

- Sie vertrauen der zugrunde liegenden Sicherheits- und Zugriffssteuerungsinfrastruktur der Cloudplattform beim Hosting und der Verwendung Ihrer Schlüssel und Geheimnisse.
- Ihre in der Cloud gehosteten Anwendungen und Dienste können sicher und zuverlässig auf Schlüssel und Geheimnisse zugreifen und sie entsprechend verwenden.
- Sie sind durch gesetzliche Vorgaben oder Organisationsrichtlinien gezwungen, die Erstellung und Verwaltung der Geheimnisse und Schlüssel Ihrer Organisation lokal durchzuführen.

### <a name="on-premises-hold-your-own-key"></a>Lokal (Hold-Your-Own-Key)

In bestimmten Szenarien liegen möglicherweise gesetzliche Vorschriften, Richtlinien oder technische Gründe vor, aus denen Sie Schlüssel nicht in einem Schlüsselverwaltungssystem, das von einem Dienst in der Public Cloud bereitgestellt wird, speichern dürfen. In diesen Fällen müssen Sie die Schlüssel mithilfe lokaler Hardware verwalten und ein Verfahren bereitstellen, mit dem cloudbasierte Ressourcen für die Verschlüsselung auf diese Schlüssel zugreifen können. Beachten Sie aber, dass die Speicherung der eigenen Schlüssel möglicherweise nicht mit allen Clouddiensten kompatibel ist.

Annahmen für die lokale Schlüsselverwaltung: Für die Verwendung eines lokalen Schlüsselverwaltungssystems wird Folgendes angenommen:

- Sie sind durch gesetzliche Vorgaben oder Organisationsrichtlinien gezwungen, Erstellung, Verwaltung und Hosting der Geheimnisse und Schlüssel Ihrer Organisation lokal durchzuführen.
- Alle cloudbasierten Anwendungen und Dienste, die auf den Zugriff auf Verschlüsselungsdienste oder Geheimnisse angewiesen sind, können auf das lokale Schlüsselverwaltungssystem zugreifen.

## <a name="data-encryption"></a>Datenverschlüsselung

Es gibt verschiedene Zustände von Daten mit unterschiedlichen Anforderungen an die Verschlüsselung, die bei der Planung Ihrer Verschlüsselungsrichtlinie berücksichtigt werden müssen:

| Datenzustand | Daten |
|-----|-----|
| Daten während der Übertragung | Interner Netzwerkdatenverkehr, Internetverbindungen, Verbindungen zwischen Rechenzentren oder virtuellen Netzwerken |
| Ruhende Daten    | Datenbanken, Dateien, virtuelle Laufwerke, PaaS-Speicher |
| Daten in Gebrauch     | Daten, die in den Arbeitsspeicher oder in CPU-Caches geladen wurden |

### <a name="data-in-transit"></a>Daten während der Übertragung

Dies sind Daten, die zwischen internen Ressourcen, zwischen Rechenzentren oder externen Netzwerken oder über das Internet verschoben werden.

Die Verschlüsselung von Daten während der Übertragung erfolgt normalerweise durch das Erzwingen der SSL/TLS-Protokolle für den Datenverkehr. Datenverkehr zwischen Ihren in der Cloud gehosteten Ressourcen und externen Netzwerken oder dem öffentlichen Internet sollte immer verschlüsselt werden. PaaS-Ressourcen erzwingen in der Regel ebenfalls standardmäßig die SSL/TLS-Verschlüsselung für Datenverkehr. Ob Sie die Verschlüsselung für Datenverkehr zwischen IaaS-Ressourcen, die in Ihren virtuellen Netzwerken gehostet werden, erzwingen, ist eine Entscheidung Ihres Cloudeinführungsteams und des Workloadbesitzers, es wird jedoch im Allgemeinen empfohlen.

**Annahmen für die Verschlüsselung von Daten während der Übertragung:** Für die Implementierung einer geeigneten Verschlüsselungsrichtlinie für Daten während der Übertragung wird Folgendes angenommen:

- Alle öffentlich zugänglichen Endpunkte in Ihrer Cloudumgebung kommunizieren mit dem öffentlichen Internet über SSL/TLS-Protokolle.
- Wenn Cloudnetzwerke über das öffentliche Internet mit einem lokalen oder anderen externen Netzwerk verbunden werden, verwenden Sie verschlüsselte VPN-Protokolle.
- Werden Cloudnetzwerke über eine dedizierte WAN-Verbindung wie ExpressRoute mit lokalen oder anderen externen Netzwerken verbunden, verwenden Sie eine VPN- oder andere lokale Verschlüsselungsappliance, die mit einer entsprechenden virtuellen VPN- oder Verschlüsselungsappliance in Ihrem Cloudnetzwerk gekoppelt ist.
- Wenn Sie über vertrauliche Daten verfügen, die nicht in Datenverkehrsprotokolle oder andere Diagnoseberichte, die für IT-Mitarbeiter einsehbar sind, eingeschlossen werden sollten, verschlüsseln Sie sämtlichen Datenverkehr zwischen den Ressourcen in Ihrem virtuellen Netzwerk.

### <a name="data-at-rest"></a>Ruhende Daten

Ruhende Daten sind alle Daten, die nicht aktiv verschoben oder verarbeitet werden, einschließlich Dateien, Datenbanken, Laufwerken virtueller Computer, PaaS-Speicherkonten oder ähnlicher Ressourcen. Das Verschlüsseln gespeicherter Daten schützt virtuelle Geräte oder Dateien vor nicht autorisiertem Zugriff durch externes Eindringen in das Netzwerk, böswillige interne Benutzer oder versehentliche Veröffentlichung.

PaaS-Speicher und Datenbankressourcen erzwingen Verschlüsselung in der Regel standardmäßig. Virtuelle IaaS-Ressourcen können durch Verschlüsselung virtueller Datenträger mithilfe kryptografischer Schlüssel, die in Ihrem Schlüsselverwaltungssystem gespeichert sind, geschützt werden.

Die Verschlüsselung ruhender Daten umfasst auch erweiterte Methoden zur Datenbankverschlüsselung, z.B. auf Spalten- und Zeilenebene. Diese bieten deutlich mehr Kontrolle darüber, welche Daten genau geschützt werden.

Ihre allgemeinen Richtlinien und Complianceanforderungen, die Vertraulichkeit der gespeicherten Daten und die Leistungsanforderungen Ihrer Workloads legen fest, für welche Ressourcen eine Verschlüsselung erforderlich ist.

**Annahmen für die Verschlüsselung von ruhenden Daten:** Für die Verschlüsselung von ruhenden Daten wird Folgendes angenommen:

- Sie speichern Daten, die nicht für die öffentliche Nutzung vorgesehen sind.
- Ihre Workloads vertragen die zusätzliche Latenz durch die Datenträgerverschlüsselung.

### <a name="data-in-use"></a>Daten in Gebrauch

Die Verschlüsselung von Daten in Gebrauch umfasst das Schützen von Daten in nicht beständigem Speicher, z.B. Arbeitsspeicher oder CPU-Caches. Dies setzt die Nutzung von Technologien wie der Verschlüsselung des gesamten Arbeitsspeichers und Enclave-Technologien, z.B. Secure Guard Extensions (SGX) von Intel, voraus. Außerdem umfasst dieser Bereich kryptografische Techniken, wie die homomorphe Verschlüsselung, die zum Erstellen von sicheren, vertrauenswürdigen Ausführungsumgebungen verwendet werden kann.

**Annahmen für die Verschlüsselung von Daten in Gebrauch:** Für die Verschlüsselung von Daten in Gebrauch wird Folgendes angenommen:

- Sie sollen den Besitz der Daten jederzeit von der zugrunde liegenden Cloudplattform trennen – selbst auf den Ebenen von Arbeitsspeicher und CPU.

## <a name="learn-more"></a>Weitere Informationen

Nachstehend finden Sie weitere Informationen zur Verschlüsselung und der Schlüsselverwaltung auf der Azure-Plattform.

- [Übersicht über die Azure-Verschlüsselung:](/azure/security/security-azure-encryption-overview) Eine ausführliche Beschreibung zur Verwendung der Verschlüsselung in Azure zum Schutz sowohl ruhender Daten als auch von Daten bei der Übertragung.
- [Azure Key Vault](/azure/key-vault/key-vault-overview). Key Vault ist das primäre Schlüsselverwaltungssystem zum Speichern und Verwalten von kryptografischen Schlüsseln, Geheimnissen und Zertifikaten in Azure.
- [Confidential Computing in Azure:](/solutions/confidential-compute) Die Confidential Computing-Initiative von Azure stellt Tools und Technologien zum Erstellen von vertrauenswürdigen Ausführungsumgebungen und anderen Verschlüsselungsmechanismen zum Schützen von Daten in Gebrauch bereit.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie virtualisierte Netzwerkfunktionen für Cloudbereitstellungen von Softwaredefinierten Netzwerken bereitgestellt werden.

> [!div class="nextstepaction"]
> [Welches Muster für softwaredefinierte Netzwerke eignet sich für meine Bereitstellung am besten?](../software-defined-network/overview.md)
