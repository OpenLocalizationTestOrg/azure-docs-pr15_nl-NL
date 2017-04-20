<properties
   pageTitle="Een Azure project te maken met Visual Studio | Microsoft Azure"
   description="Een Azure project te maken met Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="creating-an-azure-project-with-visual-studio"></a>Een Azure Project te maken met Visual Studio

De Azure-Tools voor Visual Studio bieden een sjabloon waarmee u een cloudservice voor Azure maken. De hulpmiddelen voor helpen u configureert, fouten opsporen en implementeren van de cloudservice naar Azure.

Een oplossing Azure cloud-service bevat de volgende typen projecten:

- **Azure project**

    Het Azure project bevat koppelingen naar de rol projecten in de oplossing. Het bevat ook de servicedefinitie en bestanden van de service-configuratie. De definitie servicebestand Hiermee definieert u de runtime-instellingen voor uw toepassing inclusief welke functies zijn vereist, eindpunten en VM grootte. Het configuratiebestand service Hiermee configureert u hoeveel exemplaren van een rol worden uitgevoerd en de waarden van de instellingen voor een rol. Zie voor meer informatie over deze instellingen [hoe: de rollen configureren voor een Azure-Cloudservice met Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

- **Web rol project**

    De rol van een werknemer kunt u de achtergrondverwerking van de uitvoeren. De rol van een werknemer kunt communiceren met storage-services en andere op Internet gebaseerde services. De rol van een werknemer kan een willekeurig aantal HTTP, HTTPS of TCP eindpunten hebben.

    - **ASP.NET Webrol**voor het samenstellen van een ASP.NET-toepassing met een front-end van het web
    - **ASP.NET-MVC5 Webrol**
    - **ASP.NET-MVC4 Webrol**
    - **ASP.NET-MVC3 Webrol**
    - **WCF-Service Webrol**voor het samenstellen van een WCF-service
    - **Silverlight zakelijke toepassing Webrol** (hiervoor Visual Studio 2012)

- **Cache werknemer rol**

    Een rol waarmee een speciale cache in uw toepassing.

- **De rol van de werknemer met Service Bus wachtrij**

    Een service bus wachtrij waarmee de wachtrij plaatsen functionaliteit bericht om te communiceren met het werkproces. Lees [hoe u gebruik Service Bus wachtrijen](http://go.microsoft.com/fwlink/?LinkId=260560)voor meer informatie.

## <a name="to-create-an-azure-cloud-service-project-in-visual-studio"></a>Maken van een project Azure cloud-service in Visual Studio

1. Start Microsoft Visual Studio als beheerder.

1. Kies **bestand**, **Nieuw** **Project**op de menubalk.

1. Kies **Cloud** in de Visual C# of Visual Basic-project sjabloon knooppunten in het deelvenster **Projecttypen** .

1. Kies in het deelvenster **sjablonen** **Azure-Cloudservice**.

1. Opgeven welke versie van het .NET Framework die u gebruiken wilt voor het ontwikkelen van uw project.

1. Voer een naam en locatie voor uw project en een naam voor de oplossing. Kies de knop **OK** .

1. Klik in het dialoogvenster **Nieuw Azure Project** kiest u de rollen die u wilt toevoegen en kies de knop pijl-rechts ze aan uw oplossing toevoegt. U kunt zo veel rollen naar wens toevoegen.

1. De naam van een rol die u hebt toegevoegd aan uw project, plaats de muisaanwijzer op de rol in het dialoogvenster **Nieuw Azure Project** en kies het pictogram **naam** aan de rechterkant van de rol. U kunt ook een rol binnen uw oplossing wijzigen nadat deze is toegevoegd.
