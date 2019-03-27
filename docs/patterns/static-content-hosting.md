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
# <a name="static-content-hosting-pattern"></a>Muster für das Hosten von statischen Inhalten

Stellen Sie statische Inhalte in einem cloudbasierten Speicherdienst bereit, der die Inhalte direkt an den Client übermitteln kann. Dies kann den Bedarf an potenziell kostspieligen Compute-Instanzen reduzieren.

## <a name="context-and-problem"></a>Kontext und Problem

Webanwendungen enthalten i.d.R. auch statische Inhalte. Diese statischen Inhalte umfassen möglicherweise HTML-Seiten und andere Ressourcen, z.B. Bilder und Dokumente, die für den Client verfügbar sind, entweder als Teil einer HTML-Seite (z.B. Inlinebilder, Formatvorlagen und clientseitige JavaScript-Dateien) oder als eigene Downloads (z.B. PDF-Dokumente).

Webserver sind zwar für dynamisches Rendering und Ausgabezwischenspeicherung optimiert, müssen aber dennoch Anforderungen für das Herunterladen statischer Inhalte verarbeiten. Dies beansprucht Verarbeitungszyklen, die häufig besser genutzt werden können.

## <a name="solution"></a>Lösung

In den meisten Cloudhostingumgebungen kann ein Teil der Ressourcen und statischen Seiten einer Anwendung in einem Speicherdienst platziert werden. Die Speicherdienst kann Anforderungen für diese Ressourcen bedienen, um die Computeressourcen für die Verarbeitung anderer Webanforderungen zu entlasten. Die Kosten für in der Cloud gehosteten Speicher sind in der Regel weitaus geringer als die Kosten für Compute-Instanzen.

Wenn einige Teile einer Anwendung in einem Speicherdienst gehostet werden, beziehen sich die wichtigsten Überlegungen auf die Bereitstellung der Anwendung und das Sichern der Ressourcen, die nicht für anonyme Benutzer verfügbar sein sollen.

## <a name="issues-and-considerations"></a>Probleme und Überlegungen

Beachten Sie die folgenden Punkte bei der Entscheidung, wie dieses Muster implementiert werden soll:

- Der gehostete Speicherdienst muss einen HTTP-Endpunkt verfügbar machen, auf den Benutzer zugreifen können, um die statischen Ressourcen herunterzuladen. Einige Speicherdienste unterstützen auch HTTPS, daher können Ressourcen in Speicherdiensten gehostet werden, die SSL erfordern.

- Für maximale Leistung und Verfügbarkeit empfiehlt es sich, ein Content Delivery Network (CDN) zu verwenden, um die Inhalte des Speichercontainers in mehreren Datencentern auf der ganzen Welt zwischenzuspeichern. Wahrscheinlich ist CDN jedoch für Sie kostenpflichtig.

- Speicherkonten werden häufig standardmäßig georepliziert, um Resilienz im Fall von Ereignissen zu bieten, die sich auf das Datencenter auswirken können. Dies bedeutet, dass sich die IP-Adresse ändern kann, die URL bleibt aber die gleiche.

- Wenn sich einige Inhalte in einem Speicherkonto und andere Inhalte in einer gehosteten Compute-Instanz befinden, wird es schwieriger, die Anwendung bereitzustellen und zu aktualisieren. Möglicherweise müssen Sie getrennte Bereitstellungen durchführen und Versionen der Anwendung und Inhalte erstellen, um die Verwaltung zu vereinfachen – insbesondere, wenn die statischen Inhalte Skriptdateien oder UI-Komponenten umfassen. Wenn jedoch nur statische Ressourcen aktualisiert werden müssen, können sie einfach in das Speicherkonto hochgeladen werden, ohne das Anwendungspaket erneut bereitzustellen.

- Speicherdienste unterstützen möglicherweise nicht die Verwendung von benutzerdefinierten Domänennamen. In diesem Fall muss in Links die vollständige URL der Ressourcen angegeben werden, da sie sich in einer anderen Domäne als der dynamisch generierte Inhalt befinden, der die Links enthält.

- Die Speichercontainer müssen für öffentlichen Lesezugriff konfiguriert werden. Um das Hochladen von Inhalten durch Benutzer zu verhindern, muss jedoch unbedingt sichergestellt werden, dass sie nicht für den öffentlichen Schreibzugriff konfiguriert sind.

- Verwenden Sie ggf. einen Valetschlüssel oder ein Token, um den Zugriff auf Ressourcen zu steuern, die nicht anonym verfügbar sein sollen. Weitere Informationen finden Sie unter [Muster „Valetschlüssel“](./valet-key.md).

## <a name="when-to-use-this-pattern"></a>Verwendung dieses Musters

Dieses Muster ist hilfreich:

- Zum Minimieren der Hostingkosten für Websites und Anwendungen, die statische Ressourcen enthalten.

- Zum Minimieren der Hostingkosten für Websites, die nur aus statischen Inhalten und Ressourcen bestehen. Abhängig von den Funktionen des Speichersystems des Hostinganbieters kann ggf. eine gesamte vollständig statische Website in einem Speicherkonto gehostet werden.

- Zum Verfügbarmachen statischer Ressourcen und Inhalte für Anwendungen, die in anderen Hostumgebungen oder auf lokalen Servern ausgeführt werden.

- Zum Suchen von Inhalten in mehreren geografischen Regionen mithilfe eines Content Delivery Network, das die Inhalte des Speicherkontos in mehreren Datencentern auf der ganzen Welt zwischenspeichert.

- Zum Überwachen von Kosten und Bandbreitennutzung. Durch die Verwendung eines eigenen Speicherkontos für einige oder alle statischen Inhalte lassen sich die Hosting- und Laufzeitkosten leichter trennen.

Dieses Muster ist in den folgenden Situationen eventuell nicht hilfreich:

- Die Anwendung muss statische Inhalte vor der Übermittlung an den Client verarbeiten. Beispielsweise kann es erforderlich, einem Dokument einen Zeitstempel hinzuzufügen.

- Die Menge statischer Inhalte ist sehr gering. Der Mehraufwand für das Abrufen dieser Inhalte aus einem eigenen Speicherkonto überwiegt möglicherweise den Kostenvorteil durch die Trennung von der Compute-Ressource.

## <a name="example"></a>Beispiel

Azure Storage unterstützt die direkte Bereitstellung statischer Inhalte über einen Speichercontainer. Dateien werden über anonyme Zugriffsanforderungen bereitgestellt. Standardmäßig besitzen Dateien eine URL in einer Unterdomäne von `core.windows.net` (etwa `https://contoso.z4.web.core.windows.net/image.png`). Sie können einen benutzerdefinierten Domänennamen konfigurieren und Azure CDN verwenden, um über HTTPS auf die Dateien zuzugreifen. Weitere Informationen finden Sie unter [Hosten von statischen Websites in Azure Storage](/azure/storage/blobs/storage-blob-static-website).

![Direktes Bereitstellen statischer Komponenten einer Anwendung über einen Speicherdienst](./_images/static-content-hosting-pattern.png)

Beim Hosten statischer Websites werden die Dateien für anonymen Zugriff verfügbar. Wenn Sie den Zugriff auf die Dateien steuern möchten, können Sie Dateien in Azure-Blobspeicher speichern und [Shared Access Signatures (SAS)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1) generieren, um den Zugriff einzuschränken.

Die Links auf den Seiten, die an den Client übermittelt werden, müssen die vollständige URL der Ressource enthalten. Wenn die Ressourcen durch einen Valetschlüssel (beispielsweise eine SAS) geschützt sind, muss diese Signatur in die URL eingeschlossen werden.

Auf [GitHub][sample-app] ist eine Beispiellösung verfügbar, die das Verwenden von externem Speicher für statische Ressourcen veranschaulicht. Dieses Beispiel enthält Konfigurationsdateien, die das Speicherkonto und den Container angeben, in dem sich die statischen Inhalte befinden.

```xml
<Setting name="StaticContent.StorageConnectionString"
         value="UseDevelopmentStorage=true" />
<Setting name="StaticContent.Container" value="static-content" />
```

Die `Settings`-Klasse in der Datei „Settings.cs“ des Projekts „StaticContentHosting.Web“ enthält Methoden zum Extrahieren dieser Werte und zum Erstellen eines Zeichenfolgenwerts, der die URL des Cloudspeicherkonto-Containers enthält.

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

Die `StaticContentUrlHtmlHelper`-Klasse in der Datei „StaticContentUrlHtmlHelper.cs“ macht die Methode `StaticContentUrl` verfügbar. Diese generiert eine URL, die den Pfad zu dem Cloudspeicherkonto enthält, wenn die an sie übergebene URL mit dem ASP.NET-Zeichen für das Stammverzeichnis (~) beginnt.

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

Die Datei „Index.cshtml“ im Ordner „Views\Home“ enthält ein image-Element, das mit der `StaticContentUrl`-Methode die URL für das `src`-Attribut erstellt.

```html
<img src="@Html.StaticContentUrl("~/media/orderedList1.png")" alt="Test Image" />
```

## <a name="related-patterns-and-guidance"></a>Zugehörige Muster und Anleitungen

- [Beispiel für das Hosten statischer Inhalte][sample-app]. Eine Beispielanwendung zur Veranschaulichung dieses Musters.
- [Valet-Schlüssel-Muster](./valet-key.md). Wenn die Zielressourcen nicht für anonyme Benutzer verfügbar sein sollen, verwenden Sie dieses Muster, um den Direktzugriff einzuschränken.
- [Serverlose Webanwendung in Azure](../reference-architectures/serverless/web-app.md). Eine Referenzarchitektur, die das Hosten statischer Websites mit Azure Functions verwendet, um eine serverlose Web-App zu implementieren.

[sample-app]: https://github.com/mspnp/cloud-design-patterns/tree/master/static-content-hosting
