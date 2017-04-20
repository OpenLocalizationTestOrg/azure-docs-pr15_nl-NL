<properties 
    pageTitle="Hoe u fouten opsporen in SAML-gebaseerde eenmalige aanmelding naar toepassingen in Azure Active Directory | Microsoft Azure" 
    description="Leer hoe u fouten opsporen in SAML-gebaseerde eenmalige aanmelding naar toepassingen in Azure Active Directory " 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Hoe u foutopsporing op SAML gebaseerde eenmalige aanmelding naar toepassingen in Azure Active Directory

Wanneer de toepassingsintegratie van een op SAML gebaseerde foutopsporing, is het meestal handig dat u op een hulpmiddel zoals [Fiddler](http://www.telerik.com/fiddler) gebruiken om het verzoek SAML, het antwoord op SAML en het werkelijke SAML-token dat is uitgegeven aan de toepassing weer te geven. Door het SAML-token gecontroleerd, kunt u ervoor zorgen dat alle de vereiste kenmerken, de gebruikersnaam in het onderwerp SAML en de uitgever URI binnenkort via zoals verwacht.

![][1]

De reactie van Azure AD waarin het SAML-token is meestal de fase die plaatsvindt nadat een HTTP 302 redirect vanaf https://login.windows.net en wordt verzonden naar de geconfigureerde **Antwoord URL** van de toepassing. 
 
U kunt het SAML-token weergeven met deze regel selecteren en selecteer de **controles > webformulieren** tabblad in het rechterdeelvenster. Hierin met de rechtermuisknop op de waarde **SAMLResponse** en selecteer **verzenden naar TextWizard**. Selecteer **Uit Base64** in het menu **Transformeren** het token decoderen en ziet u de inhoud ervan.
 
**Opmerking**: als u wilt zien van de inhoud van deze HTTP-aanvraag, Fiddler u mogelijk gevraagd te configureren decoderen van HTTPS-verkeer, waardoor u moet doen.

## <a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Eenmalige aanmelding naar toepassingen die niet zijn opgenomen in de galerie met Azure Active Directory-toepassing configureren](active-directory-saas-custom-apps.md)
- [Het aanpassen van Claims uitgegeven in het SAML-Token voor vooraf geïntegreerde Apps](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png
