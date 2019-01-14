---
title: Bedingtes Bereitstellen einer Ressource in einer Azure Resource Manager-Vorlage
description: Hier erfahren Sie, wie Sie die Funktionalität von Azure Resource Manager-Vorlagen erweitern, um eine bedingte Bereitstellung einer Ressource in Abhängigkeit vom Wert eines Parameters zu erzielen.
author: petertay
ms.date: 10/30/2018
ms.openlocfilehash: 0e02fbbd130bd6be2fc10173c8466b028d5d61da
ms.sourcegitcommit: 1f4cdb08fe73b1956e164ad692f792f9f635b409
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54113466"
---
# <a name="conditionally-deploy-a-resource-in-an-azure-resource-manager-template"></a><span data-ttu-id="f9eba-103">Bedingtes Bereitstellen einer Ressource in einer Azure Resource Manager-Vorlage</span><span class="sxs-lookup"><span data-stu-id="f9eba-103">Conditionally deploy a resource in an Azure Resource Manager template</span></span>

<span data-ttu-id="f9eba-104">Es gibt Situationen, in denen Sie Ihre Vorlage für die Bereitstellung einer Ressource auf Grundlage einer Bedingung, z.B. des Vorhandenseins eines Parameterwerts, entwerfen müssen.</span><span class="sxs-lookup"><span data-stu-id="f9eba-104">There are some scenarios in which you need to design your template to deploy a resource based on a condition, such as whether or not a parameter value is present.</span></span> <span data-ttu-id="f9eba-105">Ihre Vorlage kann beispielsweise ein virtuelles Netzwerk bereitstellen und Parameter enthalten, die andere virtuelle Netzwerke für das Peering angeben.</span><span class="sxs-lookup"><span data-stu-id="f9eba-105">For example, your template may deploy a virtual network and include parameters to specify other virtual networks for peering.</span></span> <span data-ttu-id="f9eba-106">Wenn Sie keine Parameterwerte für das Peering angegeben haben, möchten Sie nicht, dass Resource Manager die Peeringressource bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="f9eba-106">If you've not specified any parameter values for peering, you don't want Resource Manager to deploy the peering resource.</span></span>

<span data-ttu-id="f9eba-107">Um dies zu erreichen, verwenden Sie das [Element „condition“][azure-resource-manager-condition] in der Ressource, um die Länge des Parameterarrays zu testen.</span><span class="sxs-lookup"><span data-stu-id="f9eba-107">To accomplish this, use the [condition element][azure-resource-manager-condition] in the resource to test the length of your parameter array.</span></span> <span data-ttu-id="f9eba-108">Wenn die Länge 0 (null) ist, geben Sie `false` zurück, um die Bereitstellung zu verhindern. Bei sämtlichen Werten größer 0 geben Sie hingegen `true` zurück, um die Bereitstellung zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="f9eba-108">If the length is zero, return `false` to prevent deployment, but for all values greater than zero return `true` to allow deployment.</span></span>

## <a name="example-template"></a><span data-ttu-id="f9eba-109">Beispielvorlage</span><span class="sxs-lookup"><span data-stu-id="f9eba-109">Example template</span></span>

<span data-ttu-id="f9eba-110">Sehen wir uns eine Beispielvorlage an, die dies veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="f9eba-110">Let's look at an example template that demonstrates this.</span></span> <span data-ttu-id="f9eba-111">Unsere Vorlage verwendet das [Element „condition“][azure-resource-manager-condition] zum Steuern der Bereitstellung der Ressource `Microsoft.Network/virtualNetworks/virtualNetworkPeerings`.</span><span class="sxs-lookup"><span data-stu-id="f9eba-111">Our template uses the [condition element][azure-resource-manager-condition] to control deployment of the `Microsoft.Network/virtualNetworks/virtualNetworkPeerings` resource.</span></span> <span data-ttu-id="f9eba-112">Diese Ressource erstellt ein Peering zwischen zwei virtuellen Azure-Netzwerken in derselben Region.</span><span class="sxs-lookup"><span data-stu-id="f9eba-112">This resource creates a peering between two Azure Virtual Networks in the same region.</span></span>

<span data-ttu-id="f9eba-113">Werfen wir einen Blick auf die einzelnen Abschnitte der Vorlage.</span><span class="sxs-lookup"><span data-stu-id="f9eba-113">Let's take a look at each section of the template.</span></span>

<span data-ttu-id="f9eba-114">Das `parameters`-Element definiert einen einzelnen Parameter mit dem Namen `virtualNetworkPeerings`:</span><span class="sxs-lookup"><span data-stu-id="f9eba-114">The `parameters` element defines a single parameter named `virtualNetworkPeerings`:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "virtualNetworkPeerings": {
      "type": "array",
      "defaultValue": []
    }
  },
```

<span data-ttu-id="f9eba-115">Unser `virtualNetworkPeerings`-Parameter ist ein `array` mit dem folgenden Schema:</span><span class="sxs-lookup"><span data-stu-id="f9eba-115">Our `virtualNetworkPeerings` parameter is an `array` and has the following schema:</span></span>

```json
"virtualNetworkPeerings": [
    {
      "name": "firstVNet/peering1",
      "properties": {
          "remoteVirtualNetwork": {
              "id": "[resourceId('Microsoft.Network/virtualNetworks','secondVNet')]"
          },
          "allowForwardedTraffic": true,
          "allowGatewayTransit": true,
          "useRemoteGateways": false
      }
    }
]
```

<span data-ttu-id="f9eba-116">Die Eigenschaften in unserem Parameter geben die [Einstellungen im Zusammenhang mit dem Peering virtueller Netzwerke][vnet-peering-resource-schema] an.</span><span class="sxs-lookup"><span data-stu-id="f9eba-116">The properties in our parameter specify the [settings related to peering virtual networks][vnet-peering-resource-schema].</span></span> <span data-ttu-id="f9eba-117">Wir geben Werte für diese Eigenschaften an, wenn wir die Ressource `Microsoft.Network/virtualNetworks/virtualNetworkPeerings` im Abschnitt `resources` angeben:</span><span class="sxs-lookup"><span data-stu-id="f9eba-117">We'll provide the values for these properties when we specify the `Microsoft.Network/virtualNetworks/virtualNetworkPeerings` resource in the `resources` section:</span></span>

```json
"resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2017-05-10",
      "name": "[concat('vnp-', copyIndex())]",
      "condition": "[greater(length(parameters('virtualNetworkPeerings')), 0)]",
      "dependsOn": [
        "firstVNet", "secondVNet"
      ],
      "copy": {
          "name": "iterator",
          "count": "[length(variables('peerings'))]",
          "mode": "serial"
      },
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
            {
              "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
              "apiVersion": "2016-06-01",
              "location": "[resourceGroup().location]",
              "name": "[variables('peerings')[copyIndex()].name]",
              "properties": "[variables('peerings')[copyIndex()].properties]"
            }
          ],
          "outputs": {
          }
        }
      }
    }
]
```

<span data-ttu-id="f9eba-118">Es gibt mehrere Dinge, die in diesem Teil der Vorlage passieren.</span><span class="sxs-lookup"><span data-stu-id="f9eba-118">There are a couple of things going on in this part of our template.</span></span> <span data-ttu-id="f9eba-119">Zunächst einmal ist die eigentliche Ressource, die bereitgestellt wird, eine Inlinevorlage vom Typ `Microsoft.Resources/deployments`, die eine eigene Vorlage enthält, die `Microsoft.Network/virtualNetworks/virtualNetworkPeerings` eigentlich bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="f9eba-119">First, the actual resource being deployed is an inline template of type `Microsoft.Resources/deployments` that includes its own template that actually deploys the `Microsoft.Network/virtualNetworks/virtualNetworkPeerings`.</span></span>

<span data-ttu-id="f9eba-120">Unser `name` für die Inlinevorlage wird durch Anhängen der aktuellen Iteration von `copyIndex()` mit dem Präfix `vnp-` eindeutig gemacht.</span><span class="sxs-lookup"><span data-stu-id="f9eba-120">Our `name` for the inline template is made unique by concatenating the current iteration of the `copyIndex()` with the prefix `vnp-`.</span></span>

<span data-ttu-id="f9eba-121">Das `condition`-Element gibt an, dass unsere Ressource verarbeitet werden sollte, wenn die `greater()`-Funktion `true` ergibt.</span><span class="sxs-lookup"><span data-stu-id="f9eba-121">The `condition` element specifies that our resource should be processed when the `greater()` function evaluates to `true`.</span></span> <span data-ttu-id="f9eba-122">Hier testen wir, ob für das `virtualNetworkPeerings`-Parameterarray gilt: `greater()` als 0 (null).</span><span class="sxs-lookup"><span data-stu-id="f9eba-122">Here, we're testing if the `virtualNetworkPeerings` parameter array is `greater()` than zero.</span></span> <span data-ttu-id="f9eba-123">Wenn dies der Fall ist, lautet das Ergebnis `true`, und die `condition` ist erfüllt.</span><span class="sxs-lookup"><span data-stu-id="f9eba-123">If it is, it evaluates to `true` and the `condition` is satisfied.</span></span> <span data-ttu-id="f9eba-124">Andernfalls ist das Ergebnis `false`.</span><span class="sxs-lookup"><span data-stu-id="f9eba-124">Otherwise, it's `false`.</span></span>

<span data-ttu-id="f9eba-125">Als Nächstes geben wir unsere `copy`-Schleife an.</span><span class="sxs-lookup"><span data-stu-id="f9eba-125">Next, we specify our `copy` loop.</span></span> <span data-ttu-id="f9eba-126">Es ist eine `serial`-Schleife, das bedeutet, dass die Schleife nacheinander ausgeführt wird, wobei jede Ressource wartet, bis die letzte Ressource bereitgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="f9eba-126">It's a `serial` loop that means the loop is done in sequence, with each resource waiting until the last resource has been deployed.</span></span> <span data-ttu-id="f9eba-127">Die `count`-Eigenschaft gibt an, wie oft die Schleife durchlaufen wird.</span><span class="sxs-lookup"><span data-stu-id="f9eba-127">The `count` property specifies the number of times the loop iterates.</span></span> <span data-ttu-id="f9eba-128">Hier legen wir normalerweise die Länge des `virtualNetworkPeerings`-Arrays fest, da es die Parameterobjekte enthält, die die Ressource, die wir bereitstellen möchten, angeben.</span><span class="sxs-lookup"><span data-stu-id="f9eba-128">Here, normally we'd set it to the length of the `virtualNetworkPeerings` array because it contains the parameter objects specifying the resource we want to deploy.</span></span> <span data-ttu-id="f9eba-129">Dabei tritt jedoch ein Fehler bei der Überprüfung auf, wenn das Array leer ist, da Resource Manager bemerkt, dass wir versuchen, auf Eigenschaften zuzugreifen, die nicht vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="f9eba-129">However, if we do that, validation will fail if the array is empty because Resource Manager notices that we are attempting to access properties that do not exist.</span></span> <span data-ttu-id="f9eba-130">Wir können dies jedoch umgehen.</span><span class="sxs-lookup"><span data-stu-id="f9eba-130">We can work around this, however.</span></span> <span data-ttu-id="f9eba-131">Werfen wir einen Blick auf die Variablen, die wir benötigen:</span><span class="sxs-lookup"><span data-stu-id="f9eba-131">Let's take a look at the variables we'll need:</span></span>

```json
  "variables": {
    "workaround": {
       "true": "[parameters('virtualNetworkPeerings')]",
       "false": [{
           "name": "workaround",
           "properties": {}
       }]
     },
     "peerings": "[variables('workaround')[string(greater(length(parameters('virtualNetworkPeerings')), 0))]]"
  },
```

<span data-ttu-id="f9eba-132">Unsere `workaround`-Variable enthält die beiden Eigenschaften `true` und `false`.</span><span class="sxs-lookup"><span data-stu-id="f9eba-132">Our `workaround` variable includes two properties, one named `true` and one named `false`.</span></span> <span data-ttu-id="f9eba-133">Die `true`-Eigenschaft wird anhand des Werts des `virtualNetworkPeerings`-Parameterarrays ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="f9eba-133">The `true` property evaluates to the value of the `virtualNetworkPeerings` parameter array.</span></span> <span data-ttu-id="f9eba-134">Die `false`-Eigenschaft wird in ein leeres Objekt ausgewertet. Dies schließt auch die benannten Eigenschaften ein, die Resource Manager erwartet – beachten Sie, dass `false` tatsächlich ein Array ist (ebenso wie unser `virtualNetworkPeerings`-Parameter), sodass die Überprüfung erfüllt wird.</span><span class="sxs-lookup"><span data-stu-id="f9eba-134">The `false` property evaluates to an empty object including the named properties that Resource Manager expects to see &mdash; note that `false` is actually an array, just as our `virtualNetworkPeerings` parameter is, which will satisfy validation.</span></span>

<span data-ttu-id="f9eba-135">Unsere `peerings`-Variable verwendet unsere `workaround`-Variable, indem sie erneut testet, ob die Länge des `virtualNetworkPeerings`-Parameterarrays größer als 0 (null) ist.</span><span class="sxs-lookup"><span data-stu-id="f9eba-135">Our `peerings` variable uses our `workaround` variable by once again testing if the length of the `virtualNetworkPeerings` parameter array is greater than zero.</span></span> <span data-ttu-id="f9eba-136">Wenn dies der Fall, wird `string` als `true` und die `workaround`-Variable als `virtualNetworkPeerings`-Parameterarray ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="f9eba-136">If it is, the `string` evaluates to `true` and the `workaround` variable evaluates to the `virtualNetworkPeerings` parameter array.</span></span> <span data-ttu-id="f9eba-137">Andernfalls lautet das Ergebnis `false`, und die `workaround`-Variable wird zu einem leeren Objekt ausgewertet, das das erste Element des Arrays ist.</span><span class="sxs-lookup"><span data-stu-id="f9eba-137">Otherwise, it evaluates to `false` and the `workaround` variable evaluates to our empty object in the first element of the array.</span></span>

<span data-ttu-id="f9eba-138">Nachdem wir das Überprüfungsproblem umgangen haben, können wir einfach die Bereitstellung der `Microsoft.Network/virtualNetworks/virtualNetworkPeerings`-Ressource in der geschachtelten Vorlage angeben, indem wir `name` und `properties` aus unserem `virtualNetworkPeerings`-Parameterarray übergeben.</span><span class="sxs-lookup"><span data-stu-id="f9eba-138">Now that we've worked around the validation issue, we can simply specify the deployment of the `Microsoft.Network/virtualNetworks/virtualNetworkPeerings` resource in the nested template, passing the `name` and `properties` from our `virtualNetworkPeerings` parameter array.</span></span> <span data-ttu-id="f9eba-139">Sie können dies auch im `template`-Element erkennen, das im `properties`-Element unserer Ressource geschachtelt ist.</span><span class="sxs-lookup"><span data-stu-id="f9eba-139">You can see this in the `template` element nested in the `properties` element of our resource.</span></span>

## <a name="try-the-template"></a><span data-ttu-id="f9eba-140">Testen der Vorlage</span><span class="sxs-lookup"><span data-stu-id="f9eba-140">Try the template</span></span>

<span data-ttu-id="f9eba-141">Eine Beispielvorlage finden Sie [auf GitHub][github].</span><span class="sxs-lookup"><span data-stu-id="f9eba-141">An example template is available on [GitHub][github].</span></span> <span data-ttu-id="f9eba-142">Führen Sie zum Bereitstellen der Vorlage die folgenden Befehle der [Azure-Befehlszeilenschnittstelle][cli] aus:</span><span class="sxs-lookup"><span data-stu-id="f9eba-142">To deploy the template, run the following [Azure CLI][cli] commands:</span></span>

```bash
az group create --location <location> --name <resource-group-name>
az group deployment create -g <resource-group-name> \
    --template-uri https://raw.githubusercontent.com/mspnp/template-examples/master/example2-conditional/deploy.json
```

## <a name="next-steps"></a><span data-ttu-id="f9eba-143">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="f9eba-143">Next steps</span></span>

* <span data-ttu-id="f9eba-144">Verwenden Sie anstelle von Skalarwerten Objekte als Vorlagenparameter.</span><span class="sxs-lookup"><span data-stu-id="f9eba-144">Use objects instead of scalar values as template parameters.</span></span> <span data-ttu-id="f9eba-145">Informationen finden Sie unter [Verwenden eines Objekts als Parameter in einer Azure Resource Manager-Vorlage](./objects-as-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="f9eba-145">See [Use an object as a parameter in an Azure Resource Manager template](./objects-as-parameters.md)</span></span>

<!-- links -->
[azure-resource-manager-condition]: /azure/azure-resource-manager/resource-manager-templates-resources#condition
[azure-resource-manager-variable]: /azure/azure-resource-manager/resource-group-authoring-templates#variables
[vnet-peering-resource-schema]: /azure/templates/microsoft.network/virtualnetworks/virtualnetworkpeerings
[cli]: /cli/azure/?view=azure-cli-latest
[github]: https://github.com/mspnp/template-examples
