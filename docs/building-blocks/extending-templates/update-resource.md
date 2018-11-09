---
title: Aktualisieren einer Ressource in einer Azure Resource Manager-Vorlage
description: Beschreibt das Erweitern der Funktionalität der Azure Resource Manager-Vorlagen für das Aktualisieren einer Ressource.
author: petertay
ms.date: 10/31/2018
ms.openlocfilehash: dc97534e658c9728ac617b4e52031e2553600458
ms.sourcegitcommit: e9eb2b895037da0633ef3ccebdea2fcce047620f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50251820"
---
# <a name="update-a-resource-in-an-azure-resource-manager-template"></a><span data-ttu-id="904e4-103">Aktualisieren einer Ressource in einer Azure Resource Manager-Vorlage</span><span class="sxs-lookup"><span data-stu-id="904e4-103">Update a resource in an Azure Resource Manager template</span></span>

<span data-ttu-id="904e4-104">Es gibt einige Szenarien, in denen Sie eine Ressource während der Bereitstellung aktualisieren müssen.</span><span class="sxs-lookup"><span data-stu-id="904e4-104">There are some scenarios in which you need to update a resource during a deployment.</span></span> <span data-ttu-id="904e4-105">Dieses Szenario kann eintreten, wenn Sie nicht alle Eigenschaften für eine Ressource angeben können, solange nicht andere, abhängige Ressourcen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="904e4-105">You might encounter this scenario when you cannot specify all the properties for a resource until other, dependent resources are created.</span></span> <span data-ttu-id="904e4-106">Wenn Sie z.B einen Back-End-Adresspool für einen Load Balancer erstellen, könnten Sie vielleicht die Netzwerkschnittstellen (NICs) auf Ihren virtuellen Computern (VMs) aktualisieren, um sie in den Back-End-Pool einzubeziehen.</span><span class="sxs-lookup"><span data-stu-id="904e4-106">For example, if you create a backend pool for a load balancer, you might update the network interfaces (NICs) on your virtual machines (VMs) to include them in the backend pool.</span></span> <span data-ttu-id="904e4-107">Obwohl Resource Manager das Aktualisieren von Ressourcen während der Bereitstellung unterstützt, müssen Sie die Vorlage ordnungsgemäß entwerfen, um Fehler zu vermeiden und sicherzustellen, dass die Bereitstellung als Update behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="904e4-107">And while Resource Manager supports updating resources during deployment, you must design your template correctly to avoid errors and to ensure the deployment is handled as an update.</span></span>

<span data-ttu-id="904e4-108">Zunächst müssen Sie einmal in der Vorlage auf die Ressource verweisen, um sie zu erstellen, und dann müssen Sie mit dem gleichen Namen auf die Ressource verweisen, um sie zu einem späteren Zeitpunkt zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="904e4-108">First, you must reference the resource once in the template to create it and then reference the resource by the same name to update it later.</span></span> <span data-ttu-id="904e4-109">Wenn jedoch zwei Ressourcen in einer Vorlage den gleichen Namen besitzen, löst der Resource Manager eine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="904e4-109">However, if two resources have the same name in a template, Resource Manager throws an exception.</span></span> <span data-ttu-id="904e4-110">Um diesen Fehler zu vermeiden, geben Sie die aktualisierte Ressource mit dem Ressourcentyp `Microsoft.Resources/deployments` in einer zweiten Vorlage an, die entweder verknüpft oder als Untervorlage enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="904e4-110">To avoid this error, specify the updated resource in a second template that's either linked or included as a subtemplate using the `Microsoft.Resources/deployments` resource type.</span></span>

<span data-ttu-id="904e4-111">Als Zweites müssen Sie in der geschachtelten Vorlage den Namen der vorhandenen zu ändernden Eigenschaft oder einen neuen Namen für eine hinzuzufügende Eigenschaft angeben.</span><span class="sxs-lookup"><span data-stu-id="904e4-111">Second, you must either specify the name of the existing property to change or a new name for a property to add in the nested template.</span></span> <span data-ttu-id="904e4-112">Sie müssen auch die ursprünglichen Eigenschaften und ihre ursprünglichen Werte angeben.</span><span class="sxs-lookup"><span data-stu-id="904e4-112">You must also specify the original properties and their original values.</span></span> <span data-ttu-id="904e4-113">Wenn Sie die ursprünglichen Eigenschaften und Werte nicht angeben, setzt Resource Manager voraus, dass Sie eine neue Ressource erstellen möchten, und löscht die ursprüngliche Ressource.</span><span class="sxs-lookup"><span data-stu-id="904e4-113">If you fail to provide the original properties and values, Resource Manager assumes you want to create a new resource and deletes the original resource.</span></span>

## <a name="example-template"></a><span data-ttu-id="904e4-114">Beispielvorlage</span><span class="sxs-lookup"><span data-stu-id="904e4-114">Example template</span></span>

<span data-ttu-id="904e4-115">Sehen wir uns eine Beispielvorlage an, die dies veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="904e4-115">Let's look at an example template that demonstrates this.</span></span> <span data-ttu-id="904e4-116">Unsere Vorlage stellt das virtuelle Netzwerk `firstVNet` bereit, das ein Subnetz mit dem Namen `firstSubnet` enthält.</span><span class="sxs-lookup"><span data-stu-id="904e4-116">Our template deploys a virtual network  named `firstVNet` that has one subnet named `firstSubnet`.</span></span> <span data-ttu-id="904e4-117">Anschließend stellt sie die virtuelle Netzwerkschnittstelle (NIC) `nic1` bereit und ordnet sie unserem Subnetz zu.</span><span class="sxs-lookup"><span data-stu-id="904e4-117">It then deploys a virtual network interface (NIC) named `nic1` and associates it with our subnet.</span></span> <span data-ttu-id="904e4-118">Damit enthält die Bereitstellungsressource `updateVNet` eine geschachtelte Vorlage, die unsere `firstVNet`-Ressource durch Hinzufügen eines zweiten Subnetzes namens `secondSubnet` aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="904e4-118">Then, a deployment resource named `updateVNet` includes a nested template that updates our `firstVNet` resource by adding a second subnet named `secondSubnet`.</span></span> 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
      {
      "apiVersion": "2016-03-30",
      "name": "firstVNet",
      "location":"[resourceGroup().location]",
      "type": "Microsoft.Network/virtualNetworks",
      "properties": {
          "addressSpace":{"addressPrefixes": [
              "10.0.0.0/22"
          ]},
          "subnets":[              
              {
                  "name":"firstSubnet",
                  "properties":{
                    "addressPrefix":"10.0.0.0/24"
                  }
              }
            ]
      }
    },
    {
        "apiVersion": "2015-06-15",
        "type":"Microsoft.Network/networkInterfaces",
        "name":"nic1",
        "location":"[resourceGroup().location]",
        "dependsOn": [
            "firstVNet"
        ],
        "properties": {
            "ipConfigurations":[
                {
                    "name":"ipconfig1",
                    "properties": {
                        "privateIPAllocationMethod":"Dynamic",
                        "subnet": {
                            "id": "[concat(resourceId('Microsoft.Network/virtualNetworks','firstVNet'),'/subnets/firstSubnet')]"
                        }
                    }
                }
            ]
        }
    },
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "updateVNet",
      "dependsOn": [
          "nic1"
      ],
      "properties": {
        "mode": "Incremental",
        "parameters": {},
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [
              {
                  "apiVersion": "2016-03-30",
                  "name": "firstVNet",
                  "location":"[resourceGroup().location]",
                  "type": "Microsoft.Network/virtualNetworks",
                  "properties": {
                      "addressSpace": "[reference('firstVNet').addressSpace]",
                      "subnets":[
                          {
                              "name":"[reference('firstVNet').subnets[0].name]",
                              "properties":{
                                  "addressPrefix":"[reference('firstVNet').subnets[0].properties.addressPrefix]"
                                  }
                          },
                          {
                              "name":"secondSubnet",
                              "properties":{
                                  "addressPrefix":"10.0.1.0/24"
                                  }
                          }
                     ]
                  }
              }
          ],
          "outputs": {}
          }
        }
    }
  ],
  "outputs": {}
}
```

<span data-ttu-id="904e4-119">Werfen wir zunächst einen Blick auf das Ressourcenobjekt für unsere `firstVNet`-Ressource.</span><span class="sxs-lookup"><span data-stu-id="904e4-119">Let's take a look at the resource object for our `firstVNet` resource first.</span></span> <span data-ttu-id="904e4-120">Beachten Sie, dass wir neue Einstellungen für `firstVNet` in einer geschachtelten Vorlage&mdash; angeben, da Resource Manager keine gleichnamige Bereitstellung innerhalb einer Vorlage erlaubt und geschachtelte Vorlagen als eine andere Vorlage gelten.</span><span class="sxs-lookup"><span data-stu-id="904e4-120">Notice that we respecify the settings for our `firstVNet` in a nested template&mdash;this is because Resource Manager doesn't allow the same deployment name within the same template and nested templates are considered to be a different template.</span></span> <span data-ttu-id="904e4-121">Durch die neuen Angaben für die Werte unserer `firstSubnet`-Ressource weisen wir Resource Manager an, die vorhandene Ressource zu aktualisieren, anstatt sie zu löschen und erneut bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="904e4-121">By respecifying our values for our `firstSubnet` resource, we are telling Resource Manager to update the existing resource instead of deleting it and redeploying it.</span></span> <span data-ttu-id="904e4-122">Abschließend werden unsere neuen Einstellungen für `secondSubnet` während des Updates übernommen.</span><span class="sxs-lookup"><span data-stu-id="904e4-122">Finally, our new settings for `secondSubnet` are picked up during this update.</span></span>

## <a name="try-the-template"></a><span data-ttu-id="904e4-123">Testen der Vorlage</span><span class="sxs-lookup"><span data-stu-id="904e4-123">Try the template</span></span>

<span data-ttu-id="904e4-124">Eine Beispielvorlage finden Sie [auf GitHub][github].</span><span class="sxs-lookup"><span data-stu-id="904e4-124">An example template is available on [GitHub][github].</span></span> <span data-ttu-id="904e4-125">Führen Sie zum Bereitstellen der Vorlage die folgenden Befehle der [Azure-Befehlszeilenschnittstelle][cli] aus:</span><span class="sxs-lookup"><span data-stu-id="904e4-125">To deploy the template, run the following [Azure CLI][cli] commands:</span></span>

```bash
az group create --location <location> --name <resource-group-name>
az group deployment create -g <resource-group-name> \
    --template-uri https://raw.githubusercontent.com/mspnp/template-examples/master/example1-update/deploy.json
```

<span data-ttu-id="904e4-126">Nachdem die Bereitstellung abgeschlossen ist, öffnen Sie die Ressourcengruppe, die Sie im Verwaltungsportal angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="904e4-126">Once deployment has finished, open the resource group you specified in the portal.</span></span> <span data-ttu-id="904e4-127">Sie sehen ein virtuelles Netzwerk mit dem Namen `firstVNet` und eine NIC mit dem Namen `nic1`.</span><span class="sxs-lookup"><span data-stu-id="904e4-127">You see a virtual network named `firstVNet` and a NIC named `nic1`.</span></span> <span data-ttu-id="904e4-128">Klicken Sie auf `firstVNet` und dann auf `subnets`.</span><span class="sxs-lookup"><span data-stu-id="904e4-128">Click `firstVNet`, then click `subnets`.</span></span> <span data-ttu-id="904e4-129">Sie sehen das ursprünglich erstellte `firstSubnet` und das `secondSubnet`, das in der `updateVNet`-Ressource hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="904e4-129">You see the `firstSubnet` that was originally created, and you see the `secondSubnet` that was added in the `updateVNet` resource.</span></span> 

![Ursprüngliches Subnetz und aktualisiertes Subnetz](../_images/firstVNet-subnets.png)

<span data-ttu-id="904e4-131">Gehen Sie dann zurück zur Ressourcengruppe, und klicken Sie auf `nic1` und dann auf `IP configurations`.</span><span class="sxs-lookup"><span data-stu-id="904e4-131">Then, go back to the resource group and click `nic1` then click `IP configurations`.</span></span> <span data-ttu-id="904e4-132">Im `IP configurations`-Abschnitt ist `subnet` auf `firstSubnet (10.0.0.0/24)` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="904e4-132">In the `IP configurations` section, the `subnet` is set to `firstSubnet (10.0.0.0/24)`.</span></span> 

![IP-Konfigurationseinstellungen für nic1](../_images/nic1-ipconfigurations.png)

<span data-ttu-id="904e4-134">Das ursprüngliche `firstVNet` wurde nicht neu erstellt, sondern aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="904e4-134">The original `firstVNet` has been updated instead of recreated.</span></span> <span data-ttu-id="904e4-135">Wäre `firstVNet` neu erstellt worden, wäre `nic1` nicht `firstVNet` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="904e4-135">If `firstVNet` had been recreated, `nic1` would not be associated with `firstVNet`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="904e4-136">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="904e4-136">Next steps</span></span>

* <span data-ttu-id="904e4-137">Lesen Sie, wie Sie eine Ressource bedingungsbasiert bereitstellen (also beispielsweise in Abhängigkeit davon, ob ein Parameterwert vorhanden ist).</span><span class="sxs-lookup"><span data-stu-id="904e4-137">Learn how deploy a resource based on a condition, such as whether a parameter value is present.</span></span> <span data-ttu-id="904e4-138">Informationen finden Sie unter [Bedingtes Bereitstellen einer Ressource in einer Azure Resource Manager-Vorlage](./conditional-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="904e4-138">See [Conditionally deploy a resource in an Azure Resource Manager template](./conditional-deploy.md).</span></span>

[cli]: /cli/azure/?view=azure-cli-latest
[github]: https://github.com/mspnp/template-examples