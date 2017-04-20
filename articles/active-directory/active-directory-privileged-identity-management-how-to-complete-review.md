<properties
   pageTitle="Het voltooien van een access-controleren | Microsoft Azure"
   description="Nadat u een access-controleren in Azure AD bevoegdheden identiteitsbeheer gestart, Leer hoe u deze voltooit en de resultaten bekijken"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Het voltooien van een access te bekijken in Azure AD bevoegdheden identiteitsbeheer


Beheerders van de bevoegdheden rol kunnen bekijken bevoegdheden access één keer een [beveiliging controleren is gestart](active-directory-privileged-identity-management-how-to-start-security-review.md). Azure AD rechten identiteitsbeheer (PIM) stuurt automatisch een e-mailbericht waarin u wordt gevraagd gebruikers hun toegang bekijken. Als een gebruiker niet een e-mailbericht hebt, kunt u ze de instructies verzenden in [het uitvoeren van een onderzoek](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Nadat de periode van beveiliging controleren of alle gebruikers klaar bent met hun selfservice controleren, volg de stappen in dit artikel voor het beheren van uw beoordeling en de resultaten weer te geven.

## <a name="manage-security-reviews"></a>Beveiliging beoordelingen beheren

1. Ga naar de [Azure-portal](https://portal.azure.com/) en selecteer de toepassing **Azure AD bevoegdheden identiteitsbeheer** op uw dashboard.
2. Selecteer de sectie **toegang reviseert** van het dashboard.
3. Selecteer de access-bekijken die u wilt beheren.

Er zijn een getal opties voor het beheren van die controleren op het access-controleren detail blade.

![PIM-knoppen voor het controleren van access - schermafbeelding][1]

### <a name="remind"></a>Eraan herinneren dat er

Als u een access-controleren is ingesteld zodat de gebruikers zelf bekijken, verzendt de knop **herinnering sturen** een melding. 

### <a name="stop"></a>Stoppen

Een einddatum voor alle access-beoordelingen hebben, maar u kunt de knop **stoppen** vroeg wilt voltooien. Als u alle gebruikers dit nog niet hebt door ditmaal onderzocht, ze niet mogelijk te nadat u uw beoordeling stoppen. U kunt een beoordeling niet opnieuw starten nadat deze gestopt.

### <a name="apply"></a>Toepassen

Nadat een access-controleren is voltooid, implementeert ofwel omdat u de einddatum bereikt of deze handmatig gestopt **toepassen** klikt, wordt het resultaat van de controle. Als u de toegang van gebruikers in uw beoordeling is geweigerd, is dit de stap die hun roltoewijzing worden verwijderd.  

### <a name="export"></a>Exporteren

Als u de resultaten van de evaluatie van de beveiliging handmatig toepassen wilt, kunt u uw beoordeling exporteren. De knop **exporteren** wordt gestart met het downloaden van een CSV-bestand. De resultaten in Excel of andere programma's die CSV-bestanden openen, kunt u beheren.

### <a name="delete"></a>Verwijderen

Als u niet geïnteresseerd in de recensie verder bent, dit verwijderen. De knop **verwijderen** Hiermee verwijdert u het controleren van de configuratietoepassing PIM.

> [AZURE.IMPORTANT] U wordt niet wordt er een waarschuwing weergegeven voordat verwijdering plaatsvindt, dus zeker weet dat u wilt verwijderen die controleren.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
