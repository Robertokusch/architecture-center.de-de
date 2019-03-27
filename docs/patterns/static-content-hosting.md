---
title: Muster für das Hosten von statischen Inhalten
titleSuffix: Cloud Design Patterns
description: Stellen Sie statische Inhalte in einem cloudbasierten Speicherdienst bereit, der die Inhalte direkt an den Client übermitteln kann.
keywords: Entwurfsmuster
author: dragon119
ms.date: 01/04/2019
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: 719f0221ecc8d52267cba3136eec20dadef30b99
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58249305"
---
# <a name="static-content-hosting-pattern"></a><span data-ttu-id="ee53b-104">Muster für das Hosten von statischen Inhalten</span><span class="sxs-lookup"><span data-stu-id="ee53b-104">Static Content Hosting pattern</span></span>

<span data-ttu-id="ee53b-105">Stellen Sie statische Inhalte in einem cloudbasierten Speicherdienst bereit, der die Inhalte direkt an den Client übermitteln kann.</span><span class="sxs-lookup"><span data-stu-id="ee53b-105">Deploy static content to a cloud-based storage service that can deliver them directly to the client.</span></span> <span data-ttu-id="ee53b-106">Dies kann den Bedarf an potenziell kostspieligen Compute-Instanzen reduzieren.</span><span class="sxs-lookup"><span data-stu-id="ee53b-106">This can reduce the need for potentially expensive compute instances.</span></span>

## <a name="context-and-problem"></a><span data-ttu-id="ee53b-107">Kontext und Problem</span><span class="sxs-lookup"><span data-stu-id="ee53b-107">Context and problem</span></span>

<span data-ttu-id="ee53b-108">Webanwendungen enthalten i.d.R. auch statische Inhalte.</span><span class="sxs-lookup"><span data-stu-id="ee53b-108">Web applications typically include some elements of static content.</span></span> <span data-ttu-id="ee53b-109">Diese statischen Inhalte umfassen möglicherweise HTML-Seiten und andere Ressourcen, z.B. Bilder und Dokumente, die für den Client verfügbar sind, entweder als Teil einer HTML-Seite (z.B. Inlinebilder, Formatvorlagen und clientseitige JavaScript-Dateien) oder als eigene Downloads (z.B. PDF-Dokumente).</span><span class="sxs-lookup"><span data-stu-id="ee53b-109">This static content might include HTML pages and other resources such as images and documents that are available to the client, either as part of an HTML page (such as inline images, style sheets, and client-side JavaScript files) or as separate downloads (such as PDF documents).</span></span>

<span data-ttu-id="ee53b-110">Webserver sind zwar für dynamisches Rendering und Ausgabezwischenspeicherung optimiert, müssen aber dennoch Anforderungen für das Herunterladen statischer Inhalte verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="ee53b-110">Although web servers are optimized for dynamic rendering and output caching, they still have to handle requests to download static content.</span></span> <span data-ttu-id="ee53b-111">Dies beansprucht Verarbeitungszyklen, die häufig besser genutzt werden können.</span><span class="sxs-lookup"><span data-stu-id="ee53b-111">This consumes processing cycles that could often be put to better use.</span></span>

## <a name="solution"></a><span data-ttu-id="ee53b-112">Lösung</span><span class="sxs-lookup"><span data-stu-id="ee53b-112">Solution</span></span>

<span data-ttu-id="ee53b-113">In den meisten Cloudhostingumgebungen kann ein Teil der Ressourcen und statischen Seiten einer Anwendung in einem Speicherdienst platziert werden.</span><span class="sxs-lookup"><span data-stu-id="ee53b-113">In most cloud hosting environments, you can put some of an application's resources and static pages in a storage service.</span></span> <span data-ttu-id="ee53b-114">Die Speicherdienst kann Anforderungen für diese Ressourcen bedienen, um die Computeressourcen für die Verarbeitung anderer Webanforderungen zu entlasten.</span><span class="sxs-lookup"><span data-stu-id="ee53b-114">The storage service can serve requests for these resources, reducing load on the compute resources that handle other web requests.</span></span> <span data-ttu-id="ee53b-115">Die Kosten für in der Cloud gehosteten Speicher sind in der Regel weitaus geringer als die Kosten für Compute-Instanzen.</span><span class="sxs-lookup"><span data-stu-id="ee53b-115">The cost for cloud-hosted storage is typically much less than for compute instances.</span></span>

<span data-ttu-id="ee53b-116">Wenn einige Teile einer Anwendung in einem Speicherdienst gehostet werden, beziehen sich die wichtigsten Überlegungen auf die Bereitstellung der Anwendung und das Sichern der Ressourcen, die nicht für anonyme Benutzer verfügbar sein sollen.</span><span class="sxs-lookup"><span data-stu-id="ee53b-116">When hosting some parts of an application in a storage service, the main considerations are related to deployment of the application and to securing resources that aren't intended to be available to anonymous users.</span></span>

## <a name="issues-and-considerations"></a><span data-ttu-id="ee53b-117">Probleme und Überlegungen</span><span class="sxs-lookup"><span data-stu-id="ee53b-117">Issues and considerations</span></span>

<span data-ttu-id="ee53b-118">Beachten Sie die folgenden Punkte bei der Entscheidung, wie dieses Muster implementiert werden soll:</span><span class="sxs-lookup"><span data-stu-id="ee53b-118">Consider the following points when deciding how to implement this pattern:</span></span>

- <span data-ttu-id="ee53b-119">Der gehostete Speicherdienst muss einen HTTP-Endpunkt verfügbar machen, auf den Benutzer zugreifen können, um die statischen Ressourcen herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="ee53b-119">The hosted storage service must expose an HTTP endpoint that users can access to download the static resources.</span></span> <span data-ttu-id="ee53b-120">Einige Speicherdienste unterstützen auch HTTPS, daher können Ressourcen in Speicherdiensten gehostet werden, die SSL erfordern.</span><span class="sxs-lookup"><span data-stu-id="ee53b-120">Some storage services also support HTTPS, so it's possible to host resources in storage services that require SSL.</span></span>

- <span data-ttu-id="ee53b-121">Für maximale Leistung und Verfügbarkeit empfiehlt es sich, ein Content Delivery Network (CDN) zu verwenden, um die Inhalte des Speichercontainers in mehreren Datencentern auf der ganzen Welt zwischenzuspeichern.</span><span class="sxs-lookup"><span data-stu-id="ee53b-121">For maximum performance and availability, consider using a content delivery network (CDN) to cache the contents of the storage container in multiple datacenters around the world.</span></span> <span data-ttu-id="ee53b-122">Wahrscheinlich ist CDN jedoch für Sie kostenpflichtig.</span><span class="sxs-lookup"><span data-stu-id="ee53b-122">However, you'll likely have to pay for using the CDN.</span></span>

- <span data-ttu-id="ee53b-123">Speicherkonten werden häufig standardmäßig georepliziert, um Resilienz im Fall von Ereignissen zu bieten, die sich auf das Datencenter auswirken können.</span><span class="sxs-lookup"><span data-stu-id="ee53b-123">Storage accounts are often geo-replicated by default to provide resiliency against events that might affect a datacenter.</span></span> <span data-ttu-id="ee53b-124">Dies bedeutet, dass sich die IP-Adresse ändern kann, die URL bleibt aber die gleiche.</span><span class="sxs-lookup"><span data-stu-id="ee53b-124">This means that the IP address might change, but the URL will remain the same.</span></span>

- <span data-ttu-id="ee53b-125">Wenn sich einige Inhalte in einem Speicherkonto und andere Inhalte in einer gehosteten Compute-Instanz befinden, wird es schwieriger, die Anwendung bereitzustellen und zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ee53b-125">When some content is located in a storage account and other content is in a hosted compute instance, it becomes more challenging to deploy and update the application.</span></span> <span data-ttu-id="ee53b-126">Möglicherweise müssen Sie getrennte Bereitstellungen durchführen und Versionen der Anwendung und Inhalte erstellen, um die Verwaltung zu vereinfachen – insbesondere, wenn die statischen Inhalte Skriptdateien oder UI-Komponenten umfassen.</span><span class="sxs-lookup"><span data-stu-id="ee53b-126">You might have to perform separate deployments, and version the application and content to manage it more easily&mdash;especially when the static content includes script files or UI components.</span></span> <span data-ttu-id="ee53b-127">Wenn jedoch nur statische Ressourcen aktualisiert werden müssen, können sie einfach in das Speicherkonto hochgeladen werden, ohne das Anwendungspaket erneut bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ee53b-127">However, if only static resources have to be updated, they can simply be uploaded to the storage account without needing to redeploy the application package.</span></span>

- <span data-ttu-id="ee53b-128">Speicherdienste unterstützen möglicherweise nicht die Verwendung von benutzerdefinierten Domänennamen.</span><span class="sxs-lookup"><span data-stu-id="ee53b-128">Storage services might not support the use of custom domain names.</span></span> <span data-ttu-id="ee53b-129">In diesem Fall muss in Links die vollständige URL der Ressourcen angegeben werden, da sie sich in einer anderen Domäne als der dynamisch generierte Inhalt befinden, der die Links enthält.</span><span class="sxs-lookup"><span data-stu-id="ee53b-129">In this case it's necessary to specify the full URL of the resources in links because they'll be in a different domain from the dynamically-generated content containing the links.</span></span>

- <span data-ttu-id="ee53b-130">Die Speichercontainer müssen für öffentlichen Lesezugriff konfiguriert werden. Um das Hochladen von Inhalten durch Benutzer zu verhindern, muss jedoch unbedingt sichergestellt werden, dass sie nicht für den öffentlichen Schreibzugriff konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="ee53b-130">The storage containers must be configured for public read access, but it's vital to ensure that they aren't configured for public write access to prevent users being able to upload content.</span></span>

- <span data-ttu-id="ee53b-131">Verwenden Sie ggf. einen Valetschlüssel oder ein Token, um den Zugriff auf Ressourcen zu steuern, die nicht anonym verfügbar sein sollen.</span><span class="sxs-lookup"><span data-stu-id="ee53b-131">Consider using a valet key or token to control access to resources that shouldn't be available anonymously.</span></span> <span data-ttu-id="ee53b-132">Weitere Informationen finden Sie unter [Muster „Valetschlüssel“](./valet-key.md).</span><span class="sxs-lookup"><span data-stu-id="ee53b-132">See the [Valet Key pattern](./valet-key.md) for more information.</span></span>

## <a name="when-to-use-this-pattern"></a><span data-ttu-id="ee53b-133">Verwendung dieses Musters</span><span class="sxs-lookup"><span data-stu-id="ee53b-133">When to use this pattern</span></span>

<span data-ttu-id="ee53b-134">Dieses Muster ist hilfreich:</span><span class="sxs-lookup"><span data-stu-id="ee53b-134">This pattern is useful for:</span></span>

- <span data-ttu-id="ee53b-135">Zum Minimieren der Hostingkosten für Websites und Anwendungen, die statische Ressourcen enthalten.</span><span class="sxs-lookup"><span data-stu-id="ee53b-135">Minimizing the hosting cost for websites and applications that contain some static resources.</span></span>

- <span data-ttu-id="ee53b-136">Zum Minimieren der Hostingkosten für Websites, die nur aus statischen Inhalten und Ressourcen bestehen.</span><span class="sxs-lookup"><span data-stu-id="ee53b-136">Minimizing the hosting cost for websites that consist of only static content and resources.</span></span> <span data-ttu-id="ee53b-137">Abhängig von den Funktionen des Speichersystems des Hostinganbieters kann ggf. eine gesamte vollständig statische Website in einem Speicherkonto gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="ee53b-137">Depending on the capabilities of the hosting provider's storage system, it might be possible to entirely host a fully static website in a storage account.</span></span>

- <span data-ttu-id="ee53b-138">Zum Verfügbarmachen statischer Ressourcen und Inhalte für Anwendungen, die in anderen Hostumgebungen oder auf lokalen Servern ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ee53b-138">Exposing static resources and content for applications running in other hosting environments or on-premises servers.</span></span>

- <span data-ttu-id="ee53b-139">Zum Suchen von Inhalten in mehreren geografischen Regionen mithilfe eines Content Delivery Network, das die Inhalte des Speicherkontos in mehreren Datencentern auf der ganzen Welt zwischenspeichert.</span><span class="sxs-lookup"><span data-stu-id="ee53b-139">Locating content in more than one geographical area using a content delivery network that caches the contents of the storage account in multiple datacenters around the world.</span></span>

- <span data-ttu-id="ee53b-140">Zum Überwachen von Kosten und Bandbreitennutzung.</span><span class="sxs-lookup"><span data-stu-id="ee53b-140">Monitoring costs and bandwidth usage.</span></span> <span data-ttu-id="ee53b-141">Durch die Verwendung eines eigenen Speicherkontos für einige oder alle statischen Inhalte lassen sich die Hosting- und Laufzeitkosten leichter trennen.</span><span class="sxs-lookup"><span data-stu-id="ee53b-141">Using a separate storage account for some or all of the static content allows the costs to be more easily separated from hosting and runtime costs.</span></span>

<span data-ttu-id="ee53b-142">Dieses Muster ist in den folgenden Situationen eventuell nicht hilfreich:</span><span class="sxs-lookup"><span data-stu-id="ee53b-142">This pattern might not be useful in the following situations:</span></span>

- <span data-ttu-id="ee53b-143">Die Anwendung muss statische Inhalte vor der Übermittlung an den Client verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="ee53b-143">The application needs to perform some processing on the static content before delivering it to the client.</span></span> <span data-ttu-id="ee53b-144">Beispielsweise kann es erforderlich, einem Dokument einen Zeitstempel hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="ee53b-144">For example, it might be necessary to add a timestamp to a document.</span></span>

- <span data-ttu-id="ee53b-145">Die Menge statischer Inhalte ist sehr gering.</span><span class="sxs-lookup"><span data-stu-id="ee53b-145">The volume of static content is very small.</span></span> <span data-ttu-id="ee53b-146">Der Mehraufwand für das Abrufen dieser Inhalte aus einem eigenen Speicherkonto überwiegt möglicherweise den Kostenvorteil durch die Trennung von der Compute-Ressource.</span><span class="sxs-lookup"><span data-stu-id="ee53b-146">The overhead of retrieving this content from separate storage can outweigh the cost benefit of separating it out from the compute resource.</span></span>

## <a name="example"></a><span data-ttu-id="ee53b-147">Beispiel</span><span class="sxs-lookup"><span data-stu-id="ee53b-147">Example</span></span>

<span data-ttu-id="ee53b-148">Azure Storage unterstützt die direkte Bereitstellung statischer Inhalte über einen Speichercontainer.</span><span class="sxs-lookup"><span data-stu-id="ee53b-148">Azure Storage supports serving static content directly from a storage container.</span></span> <span data-ttu-id="ee53b-149">Dateien werden über anonyme Zugriffsanforderungen bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="ee53b-149">Files are served through anonymous access requests.</span></span> <span data-ttu-id="ee53b-150">Standardmäßig besitzen Dateien eine URL in einer Unterdomäne von `core.windows.net` (etwa `https://contoso.z4.web.core.windows.net/image.png`).</span><span class="sxs-lookup"><span data-stu-id="ee53b-150">By default, files have a URL in a subdomain of `core.windows.net`, such as `https://contoso.z4.web.core.windows.net/image.png`.</span></span> <span data-ttu-id="ee53b-151">Sie können einen benutzerdefinierten Domänennamen konfigurieren und Azure CDN verwenden, um über HTTPS auf die Dateien zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="ee53b-151">You can configure a custom domain name, and use Azure CDN to access the files over HTTPS.</span></span> <span data-ttu-id="ee53b-152">Weitere Informationen finden Sie unter [Hosten von statischen Websites in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="ee53b-152">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

![Direktes Bereitstellen statischer Komponenten einer Anwendung über einen Speicherdienst](./_images/static-content-hosting-pattern.png)

<span data-ttu-id="ee53b-154">Beim Hosten statischer Websites werden die Dateien für anonymen Zugriff verfügbar.</span><span class="sxs-lookup"><span data-stu-id="ee53b-154">Static website hosting makes the files available for anonymous access.</span></span> <span data-ttu-id="ee53b-155">Wenn Sie den Zugriff auf die Dateien steuern möchten, können Sie Dateien in Azure-Blobspeicher speichern und [Shared Access Signatures (SAS)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1) generieren, um den Zugriff einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="ee53b-155">If you need to control who can access the files, you can store files in Azure blob storage and then generate [shared access signatures](/azure/storage/common/storage-dotnet-shared-access-signature-part-1) to limit access.</span></span>

<span data-ttu-id="ee53b-156">Die Links auf den Seiten, die an den Client übermittelt werden, müssen die vollständige URL der Ressource enthalten.</span><span class="sxs-lookup"><span data-stu-id="ee53b-156">The links in the pages delivered to the client must specify the full URL of the resource.</span></span> <span data-ttu-id="ee53b-157">Wenn die Ressourcen durch einen Valetschlüssel (beispielsweise eine SAS) geschützt sind, muss diese Signatur in die URL eingeschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="ee53b-157">If the resource is protected with a valet key, such as a shared access signature, this signature must be included in the URL.</span></span>

<span data-ttu-id="ee53b-158">Auf [GitHub][sample-app] ist eine Beispiellösung verfügbar, die das Verwenden von externem Speicher für statische Ressourcen veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="ee53b-158">A sample application that demonstrates using external storage for static resources is available on [GitHub][sample-app].</span></span> <span data-ttu-id="ee53b-159">Dieses Beispiel enthält Konfigurationsdateien, die das Speicherkonto und den Container angeben, in dem sich die statischen Inhalte befinden.</span><span class="sxs-lookup"><span data-stu-id="ee53b-159">This sample uses configuration files to specify the storage account and container that holds the static content.</span></span>

```xml
<Setting name="StaticContent.StorageConnectionString"
         value="UseDevelopmentStorage=true" />
<Setting name="StaticContent.Container" value="static-content" />
```

<span data-ttu-id="ee53b-160">Die `Settings`-Klasse in der Datei „Settings.cs“ des Projekts „StaticContentHosting.Web“ enthält Methoden zum Extrahieren dieser Werte und zum Erstellen eines Zeichenfolgenwerts, der die URL des Cloudspeicherkonto-Containers enthält.</span><span class="sxs-lookup"><span data-stu-id="ee53b-160">The `Settings` class in the file Settings.cs of the StaticContentHosting.Web project contains methods to extract these values and build a string value containing the cloud storage account container URL.</span></span>

```csharp
public class Settings
{
  public static string StaticContentStorageConnectionString {
    get
    {
      return RoleEnvironment.GetConfigurationSettingValue(
                              "StaticContent.StorageConnectionString");
    }
  }

  public static string StaticContentContainer
  {
    get
    {
      return RoleEnvironment.GetConfigurationSettingValue("StaticContent.Container");
    }
  }

  public static string StaticContentBaseUrl
  {
    get
    {
      var account = CloudStorageAccount.Parse(StaticContentStorageConnectionString);

      return string.Format("{0}/{1}", account.BlobEndpoint.ToString().TrimEnd('/'),
                                      StaticContentContainer.TrimStart('/'));
    }
  }
}
```

<span data-ttu-id="ee53b-161">Die `StaticContentUrlHtmlHelper`-Klasse in der Datei „StaticContentUrlHtmlHelper.cs“ macht die Methode `StaticContentUrl` verfügbar. Diese generiert eine URL, die den Pfad zu dem Cloudspeicherkonto enthält, wenn die an sie übergebene URL mit dem ASP.NET-Zeichen für das Stammverzeichnis (~) beginnt.</span><span class="sxs-lookup"><span data-stu-id="ee53b-161">The `StaticContentUrlHtmlHelper` class in the file StaticContentUrlHtmlHelper.cs exposes a method named `StaticContentUrl` that generates a URL containing the path to the cloud storage account if the URL passed to it starts with the ASP.NET root path character (~).</span></span>

```csharp
public static class StaticContentUrlHtmlHelper
{
  public static string StaticContentUrl(this HtmlHelper helper, string contentPath)
  {
    if (contentPath.StartsWith("~"))
    {
      contentPath = contentPath.Substring(1);
    }

    contentPath = string.Format("{0}/{1}", Settings.StaticContentBaseUrl.TrimEnd('/'),
                                contentPath.TrimStart('/'));

    var url = new UrlHelper(helper.ViewContext.RequestContext);

    return url.Content(contentPath);
  }
}
```

<span data-ttu-id="ee53b-162">Die Datei „Index.cshtml“ im Ordner „Views\Home“ enthält ein image-Element, das mit der `StaticContentUrl`-Methode die URL für das `src`-Attribut erstellt.</span><span class="sxs-lookup"><span data-stu-id="ee53b-162">The file Index.cshtml in the Views\Home folder contains an image element that uses the `StaticContentUrl` method to create the URL for its `src` attribute.</span></span>

```html
<img src="@Html.StaticContentUrl("~/media/orderedList1.png")" alt="Test Image" />
```

## <a name="related-patterns-and-guidance"></a><span data-ttu-id="ee53b-163">Zugehörige Muster und Anleitungen</span><span class="sxs-lookup"><span data-stu-id="ee53b-163">Related patterns and guidance</span></span>

- <span data-ttu-id="ee53b-164">[Beispiel für das Hosten statischer Inhalte][sample-app].</span><span class="sxs-lookup"><span data-stu-id="ee53b-164">[Static Content Hosting sample][sample-app].</span></span> <span data-ttu-id="ee53b-165">Eine Beispielanwendung zur Veranschaulichung dieses Musters.</span><span class="sxs-lookup"><span data-stu-id="ee53b-165">A sample application that demonstrates this pattern.</span></span>
- <span data-ttu-id="ee53b-166">[Valet-Schlüssel-Muster](./valet-key.md).</span><span class="sxs-lookup"><span data-stu-id="ee53b-166">[Valet Key pattern](./valet-key.md).</span></span> <span data-ttu-id="ee53b-167">Wenn die Zielressourcen nicht für anonyme Benutzer verfügbar sein sollen, verwenden Sie dieses Muster, um den Direktzugriff einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="ee53b-167">If the target resources aren't supposed to be available to anonymous users, use this pattern to restrict direct access.</span></span>
- <span data-ttu-id="ee53b-168">[Serverlose Webanwendung in Azure](../reference-architectures/serverless/web-app.md).</span><span class="sxs-lookup"><span data-stu-id="ee53b-168">[Serverless web application on Azure](../reference-architectures/serverless/web-app.md).</span></span> <span data-ttu-id="ee53b-169">Eine Referenzarchitektur, die das Hosten statischer Websites mit Azure Functions verwendet, um eine serverlose Web-App zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="ee53b-169">A reference architecture that uses static website hosting with Azure Functions to implement a serverless web app.</span></span>

[sample-app]: https://github.com/mspnp/cloud-design-patterns/tree/master/static-content-hosting
