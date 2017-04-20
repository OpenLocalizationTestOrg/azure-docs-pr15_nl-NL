<properties 
   pageTitle="Mobile-Services toevoegen met behulp van verbonden Services in Visual Studio | Microsoft Azure"
   description="Mobile-Services toevoegen met behulp van het dialoogvenster Visual Studio verbonden Services toevoegen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="mlhoop"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="12/16/2015"
   ms.author="mlearned" />

# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Mobile-Services toevoegen met behulp van Visual Studio verbonden Services

Met Visual Studio-2015 verlengt, kunt u een verbinding maken met Azure Mobile Services met het dialoogvenster **Verbonden Service toevoegen** . U kunt verbinding maken vanaf een C#-client-app, een JavaScript-app of meerdere platforms Cordova app. Als u verbinding hebt gemaakt, kunt u maken en toegang tot gegevens, aangepaste API's en geplande taken maken of ondersteuning voor pushmeldingen toevoegen.  De bewerking verbonden services wordt toegevoegd voor alle gewenste verwijzingen en verbinding code. U kunt ook profiteren van de ingebouwde ondersteuning voor verificatie met een verscheidenheid aan populaire identiteit schema, zoals Azure AD, Facebook, Twitter en Microsoft-Accounts.

## <a name="supported-project-types"></a>Ondersteunde projecttypen

>[AZURE.NOTE] In Visual Studio-2015 verlengt, wordt Azure Mobile Services toevoegen aan een Windows universele (Windows 10) projecten met behulp van het dialoogvenster verbonden Services toevoegen niet ondersteund. U kunt Azure Mobile Services toevoegen door te installeren de juiste pakketten met behulp van de NuGet Package Manager voor uw project.

Verbinding maken met Azure Mobile Services in de volgende projecttypen kunt u het dialoogvenster verbonden Services.

- .NET Windows 8.1 Store, telefoon en universele App projecten

- JavaScript Windows 8.1 Store, telefoon en universele App projecten

- Projecten die zijn gemaakt met behulp van Visual Studio Tools voor Apache Cordova


## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Verbinding maken met Azure Mobile Services met het dialoogvenster verbonden Services toevoegen

1. Zorg ervoor dat u een Azure-account hebt. Als u niet een Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Open het dialoogvenster **Verbonden Services toevoegen** .
 - .NET-apps voor uw project openen in Visual Studio, het snelmenu voor het knooppunt **verwijzingen** in Solution Explorer openen en kies vervolgens **Verbonden Service toevoegen**
 
        ![Connecting to Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)

 - Voor Apache Cordova app projecten, uw project openen in Visual Studio, het snelmenu voor het projectknooppunt openen in Verkenner oplossing en kies vervolgens **Verbonden Service toevoegen**.

1. Kies **Azure Mobile Services**in het dialoogvenster **Verbonden Service toevoegen** en kies vervolgens de knop **configureren** . Mogelijk wordt u gevraagd aan te melden bij Azure als u dat nog niet had gedaan.

    ![Het toevoegen van een Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)

1. Klik in het dialoogvenster **Azure Mobile Services** kiest u een bestaande mobiele service als er een. Als u nodig hebt om een nieuwe Azure mobiele service te maken, volgt u de volgende kunt doen procedure. Ga anders verder met de volgende stap.

    Een nieuwe mobile service-account maken:
    1. Kies de koppeling **Maken Service **onderaan in het dialoogvenster.
        ![Nieuwe mobiele verbonden service toevoegen](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)




    2. Klik in het dialoogvenster **Maken Mobile Service** kunt u een mobiele JavaScript backend-service of een mobiele .NET backend-service kiezen uit de vervolgkeuzelijst **Runtime** . 
  
        ![Een mobiele service maken](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)

        Een JavaScript backend-service is eenvoudig en krachtige. Als u een mobiele JavaScript backend-service maakt, de serverzijde JavaScript-code is opgeslagen in de cloud, maar u kunt server-scripts bewerken met behulp van de Server Explorer of de Azure beheerportal. 

        Een mobiele .NET backend-service biedt de volledige kracht en flexibiliteit van Web API en entiteit Framework. Als u een mobiele .NET backend-service maakt, wordt een project voor u gemaakt en toegevoegd aan uw oplossing. 

    1. Kies de **regio** waar u de mobiele service en voer vervolgens een gebruikersnaam en wachtwoord voor de server.
 
    1. Nadat u de vereiste gegevens hebt ingevoerd, klikt u op de knop **maken** om de mobiele service te maken.
    2. De nieuwe mobiele service moet worden weergegeven in de lijst met Services in het dialoogvenster **Azure Mobile Services** . Kies de nieuwe mobiele service in de lijst en kies vervolgens de knop **toevoegen** aan de service toevoegen aan uw project.
    

1. Controleer de ophalen gestart pagina die verschijnt en ontdek hoe uw project is gewijzigd. Een pagina aan de slag weergegeven in uw browser wanneer u een verbonden service toevoegen. U kunt de voorgestelde Vervolgstappen en codevoorbeelden bekijken of overschakelen naar de pagina Wat is er gebeurd om te zien welke verwijzingen zijn toegevoegd aan uw project en hoe uw code en configuratie-bestanden zijn gewijzigd.

1. Start met behulp van de voorbeelden van de code als een hulplijn schrijven van code voor toegang tot uw telefoonprovider!

## <a name="how-your-project-is-modified"></a>Hoe uw project is gewijzigd

Hoe Visual Studio uw project wijzigt, is afhankelijk van het projecttype. Zie [Wat is er gebeurd – C# projecten](http://go.microsoft.com/fwlink/p/?LinkId=513119)C#-client-apps voor. Zie [Wat is er gebeurd – JavaScript-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513120)voor JavaScript-clienttoepassingen. Zie [Wat is er gebeurd – Cordova projecten](http://go.microsoft.com/fwlink/p/?LinkId=513116)voor Cordova apps.


##<a name="next-steps"></a>Volgende stappen

Vragen kunt stellen en Help-informatie opvragen: 

 - [MSDN-Forum: Azure mobiele Services](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)

 - [Azure mobiele Services op de Microsoft Azure-teamblog](https://azure.microsoft.com/blog/topics/mobile/)

 - [Azure mobiele Services op azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)

 - [Azure mobiele Services documentatie op azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)



