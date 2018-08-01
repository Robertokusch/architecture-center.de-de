---
title: 'Einführung von Azure: Grundlegende Phase'
description: Hier wird der grundlegende Wissensstand beschrieben, den ein Unternehmen für die Einführung von Azure benötigt.
author: petertay
ms.openlocfilehash: b5a0a4a2c4ed1d06c97774b0eca643a89a5a2110
ms.sourcegitcommit: c704d5d51c8f9bbab26465941ddcf267040a8459
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39229166"
---
# <a name="adopting-microsoft-azure-foundational"></a>Einführung von Microsoft Azure: Grundlegende Phase

Für eine Organisation, die noch keine Erfahrung mit Cloudtechnologie hat, kann die Wahl des richtigen Ausgangspunkts für die Einführung schwierig sein. Das Ziel der grundlegenden Einführungsphase besteht darin, einen Ausgangspunkt festzulegen. Nachdem alle Personen der Organisation diese Phase durchgearbeitet haben, verfügen sie über das Wissen und die Fähigkeiten, die zum Bereitstellen der Computeressourcen für eine einfache Workload in Azure benötigt werden. 

> [!NOTE]
> Die Anwendungsentwicklung wird in diesem Leitfaden nicht behandelt. Weitere Informationen zur Entwicklung von Anwendungen in Azure finden Sie im [Azure-Anwendungsarchitekturleitfaden](/azure/architecture/guide/).

Die Zielgruppe für diese Phase des Handbuchs umfasst folgende Personas in Ihrer Organisation:

- *Finanzen:* Zuständig für die finanzielle Verpflichtung gegenüber Azure und verantwortlich für die Entwicklung von Richtlinien und Verfahren zur Überwachung der Kosten für die Ressourcennutzung (einschließlich Abrechnung und verbrauchsbasierte Kostenzuteilung).
- *Zentrale IT:* Zuständig für die Steuerung der Cloudressourcen Ihrer Organisation (einschließlich Ressourcenverwaltung und -zugriff) sowie für Workloadintegrität und -überwachung.
- *Workloadbesitzer:* Alle Entwicklungsrollen, die an der Bereitstellung von Workloads für Azure beteiligt sind, z.B. Entwickler, Tester und Buildmanager.

## <a name="section-1-azure-basics"></a>Abschnitt 1: Azure-Grundlagen

Dieser einführende Abschnitt richtet sich an die Mitarbeiter aus den Bereichen *Finanzen* und *Zentrale IT*. Der Schwerpunkt dieses Abschnitts ist die Vermittlung von Grundkenntnissen in Bezug auf die [Funktionsweise von Azure](azure-explainer.md). Dies dient als Vorbereitung auf den Abschnitt zum [Konzept der Cloud-Governance](governance-explainer.md). Es kann für die *Workloadbesitzer* in Ihrer Organisation auch hilfreich sein, sich diese Informationen anzusehen, um die Verwaltung des Ressourcenzugriffs zu verstehen.

## <a name="section-2-governance-design-guide"></a>Abschnitt 2: Governance-Entwurfshandbuch

Nachdem Sie sich mit der Funktionsweise von Azure und den Grundlagen der Cloud-Governance vertraut gemacht haben, besteht der erste Schritt zur Einführung von Azure darin, sich über die [Ressourcenzugriffsverwaltung](azure-resource-access.md) in Azure zu informieren. In diesem Artikel werden die Azure-Dienste zum Senden von Zugriffsanforderungen für Ressourcen und die Kontrollen beschrieben, die zum Überprüfen dieser Anforderungen verwendet werden.

Im nächsten Schritt erfahren Sie dann, wie Sie [ein Governance-Modell für ein einzelnes Team entwerfen](governance-how-to.md). In diesem Artikel wird beschrieben, wie Sie die Dienste und Kontrollen für die Ressourcenzugriffsverwaltung konfigurieren, die weiter oben beschrieben wurden.

## <a name="section-3-implementing-a-basic-resource-access-management-model"></a>Abschnitt 3: Implementieren eines einfachen Modells für die Ressourcenzugriffsverwaltung

Im letzten Schritt zur Einführung erfahren Sie, wie Sie das zuvor entworfene Governance-Modell implementieren. 

Ihre Organisation benötigt ein Azure-Konto, um beginnen zu können. Wenn Ihre Organisation über ein vorhandenes [Microsoft Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx) verfügt, in dem Azure nicht enthalten ist, kann Azure hinzugefügt werden, indem Sie vorab eine Zahlungsverpflichtung eingehen. Weitere Informationen finden Sie unter [Lizenzierung von Azure für das Unternehmen](https://azure.microsoft.com/pricing/enterprise-agreement/). 

Beim Erstellen Ihres Azure-Kontos geben Sie eine Person in Ihrer Organisation als Azure-**Kontobesitzer** an. Anschließend wird standardmäßig ein Azure AD-Mandant (Azure Active Directory) erstellt. Ihr Azure-**Kontobesitzer** muss die [Erstellung des Benutzerkontos](/azure/active-directory/add-users-azure-active-directory) für die Person in Ihrer Organisation durchführen, die als **Workloadbesitzer** fungiert. 

Als Nächstes muss Ihr Azure-**Kontobesitzer** ein [Abonnement erstellen](https://docs.microsoft.com/partner-center/create-a-new-subscription) und diesem [den Azure AD-Mandanten zuordnen](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory).

Nachdem das Abonnement erstellt und Ihr Azure AD-Mandant zugeordnet wurde, können Sie [den **Workloadbesitzer** dem Abonnement mit der integrierten Rolle **Besitzer** hinzufügen](/azure/billing/billing-add-change-azure-subscription-administrator#add-an-rbac-owner-for-a-subscription-in-azure-portal).

## <a name="section-4-deploy-a-basic-workload-architecture-to-azure"></a>Abschnitt 4: Bereitstellen einer einfachen Workloadarchitektur in Azure

Die Zielgruppe für diesen Abschnitt sind die *Workloadbesitzer*. *Workloadbesitzer* definieren die Compute- und Netzwerkanforderungen für ihre Workloads, wählen die richtigen Ressourcen zum Erfüllen dieser Anforderungen aus und stellen sie in Azure bereit. 

Für die grundlegende Einführungsphase kann der Workloadbesitzer entweder eine einfache Webanwendung oder ein virtuelles Netzwerk (VNET) und einen virtuellen Computer (VM) wählen. Weitere Informationen zu diesen unterschiedlichen Arten von Computeoptionen finden Sie in der [Übersicht über Azure-Computeoptionen](/azure/architecture/guide/technology-choices/compute-overview?toc=/azure/architecture/cloud-adoption-guide/toc.json).

Unabhängig von der gewählten Computeoption ist für diese Bereitstellungen jeweils eine **Ressourcengruppe** erforderlich. Ihr **Workloadbesitzer** muss [die Ressourcengruppe erstellen](/azure/azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy). Im Rahmen der Bereitstellung gibt der **Workloadbesitzer** einen Namen für die Ressourcengruppe an. Dieser Name wird in den folgenden Abschnitten verwendet.

### <a name="basic-web-application-paas"></a>Einfache Webanwendung (PaaS)

Wählen Sie für eine einfache Webanwendung eine der 5-Minuten-Schnellstartanleitungen der [Dokumentation zu Web-Apps](/azure/app-service?toc=/azure/architecture/cloud-adoption-guide/toc.json) aus, und führen Sie die jeweiligen Schritte aus. 

> [!NOTE]
> In einigen Schnellstartanleitungen wird standardmäßig eine Ressourcengruppe bereitgestellt. In diesem Fall muss der **Workloadbesitzer** nicht explizit eine Ressourcengruppe erstellen. Stellen Sie die Webanwendung andernfalls in der oben erstellten Ressourcengruppe bereit.

Nachdem Ihre einfache Workload bereitgestellt wurde, können Sie sich weiter über die bewährten Methoden zur Bereitstellung einer [einfachen Webanwendung](/azure/architecture/reference-architectures/app-service-web-app/basic-web-app?toc=/azure/architecture/cloud-adoption-guide/toc.json) in Azure informieren.

### <a name="single-windows-or-linux-vm-iaas"></a>Einzelne Windows- oder Linux-VM (IaaS)

Für eine einfache Workload, die auf einem virtuellen Computer ausgeführt wird, ist der erste Schritt das Bereitstellen eines virtuellen Netzwerks. Für alle IaaS-Ressourcen in Azure, z.B. virtuelle Computer, Lastenausgleichsmodule und Gateways, ist ein virtuelles Netzwerk erforderlich. Erfahren Sie mehr zu [virtuellen Azure-Netzwerken](/azure/virtual-network/virtual-networks-overview?toc=/azure/architecture/cloud-adoption-guide/toc.json), und führen Sie die Schritte zum [Bereitstellen eines virtuellen Netzwerks in Azure mit dem Portal](/azure/virtual-network/quick-create-portal?toc=/azure/architecture/cloud-adoption-guide/toc.json) aus. Geben Sie den Namen der oben erstellten Ressourcengruppe an, wenn Sie die Einstellungen für das virtuelle Netzwerk im Azure-Portal festlegen.

Der nächste Schritt ist die Entscheidung, ob eine einzelne Windows- oder Linux-VM bereitgestellt werden soll. Führen Sie für eine Windows-VM die Schritte zum [Bereitstellen eines virtuellen Windows-Computers in Azure mit dem Portal](/azure/virtual-machines/windows/quick-create-portal?toc=/azure/architecture/cloud-adoption-guide/toc.json) aus. Geben Sie wieder den Namen der oben erstellten Ressourcengruppe an, wenn Sie die Einstellungen für den virtuellen Computer im Azure-Portal festlegen.

Nachdem Sie die Schritte ausgeführt und die VM bereitgestellt haben, können Sie sich über die [bewährten Methoden für das Ausführen eines virtuellen Windows-Computers in Azure](/azure/architecture/reference-architectures/virtual-machines-windows/single-vm?toc=/azure/architecture/cloud-adoption-guide/toc.json) informieren. Führen Sie für eine Linux-VM die Schritte zum [Bereitstellen eines virtuellen Linux-Computers in Azure mit dem Portal](/azure/virtual-machines/linux/quick-create-portal?toc=/azure/architecture/cloud-adoption-guide/toc.json) aus. Außerdem können Sie sich über die [bewährten Methoden für das Ausführen eines virtuellen Linux-Computers in Azure](/azure/architecture/reference-architectures/virtual-machines-linux/single-vm?toc=/azure/architecture/cloud-adoption-guide/toc.json) informieren.

## <a name="next-steps"></a>Nächste Schritte

Der nächste Schritte zur Cloud Readiness ist die [**mittlere Phase**](../intermediate-stage/overview.md). In der mittleren Phase erfahren Sie, wie Sie Ihr lokales Netzwerk erweitern, um mehrere Workloads und Governance-Modelle für mehrere Teams auszuführen.