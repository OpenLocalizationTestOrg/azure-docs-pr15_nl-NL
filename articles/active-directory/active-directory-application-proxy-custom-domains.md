<properties
    pageTitle="Werken met aangepaste domeinen in Azure AD-toepassingsproxy | Microsoft Azure"
    description="Voorbladen worden hoe met aangepaste domeinen in Azure AD-toepassingsproxy werken."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Werken met aangepaste domeinen in Azure AD-toepassingsproxy

Gebruik van een standaarddomein, kunt u dezelfde URL instellen als de interne en externe-URL voor toegang tot de toepassing zodat uw gebruikers slechts één URL moet denken om toegang tot de app, ongeacht waar ze toegang krijgen vanuit tot hebben. Hiermee kunt u ook de snelkoppeling voor een enkele maken in het deelvenster toegang voor de toepassing. Als u het standaarddomein die is verstrekt door Azure AD-toepassingsproxy gebruikt, is er geen aanvullende configuratie, moet u uw domein inschakelen. In het geval dat u een aangepast domein gebruiken, zijn er enkele dingen die u doen om ervoor te zorgen moet dat toepassingsproxy herkent van uw domein en de certificaten is gevalideerd.

## <a name="selecting-your-custom-domain"></a>Uw aangepaste domein selecteren

1. Uw toepassing volgens de instructies in [de toepassingen publiceren met toepassingsproxy](active-directory-application-proxy-publish.md)publiceren.
2. Nadat de toepassing in de lijst met toepassingen wordt weergegeven, selecteert u deze en klik op **configureren**.
3. Voer onder **Externe URL**, uw aangepaste domein.
4. Als de URL van uw externe https is, wordt u gevraagd een certificaat uploaden zodat deze Azure de URL van de toepassing kunt valideren. U kunt ook een jokerteken-certificaat dat overeenkomt met de externe URL van de toepassing uploaden. Dit domein moeten in de lijst met uw [Azure geverifieerd domeinen](https://msdn.microsoft.com/library/azure/jj151788.aspx). Azure moet een certificaat hebben voor de URL van het domein van de toepassing of een jokerteken-certificaat dat overeenkomt met de externe URL voor de toepassing.
5. Zorg ervoor dat u een DNS-record toevoegen die stuurt de interne URL naar de toepassing waarmee u kunt de URL voor interne en externe toegang en een enkel snelkoppeling naar de toepassing in de lijst met toepassingen van de gebruiker hebt.

## <a name="frequently-asked-questions-about-working-with-custom-domains"></a>Veelgestelde vragen over het werken met aangepaste domeinen

V: kan ik een certificaat al geüpload selecteren zonder deze opnieuw te uploaden?  
A: eerder geüploade certificaten automatisch zijn gekoppeld aan een toepassing en er is precies een certificaat dat overeenkomt met de hostnaam van de toepassing.  

V: hoe voeg ik een certificaat en welke indeling het geëxporteerde certificaat moeten worden geüpload?  
A: het certificaat moet worden geüpload vanaf de pagina van de configuratie van toepassing. Het certificaat moet een PFX-bestand.  

V: kunnen ECC certificaten worden gebruikt?  
A: Er is geen expliciete beperking op handtekening methoden.  

V: kunnen SAN certificaten worden gebruikt?  
A: Ja.  

V: kunnen ik jokertekens certificaten gebruiken?  
A: Ja.  

V: kan een ander certificaat worden gebruikt op elke toepassing?  
A: Ja, tenzij de twee toepassingen dezelfde externe host delen.  

V: als ik een nieuw domein hebt geregistreerd, kan ik dat domein gebruiken?  
A: Ja, de lijst met domeinen is afkomstig uit de gecontroleerde domeinenlijst van de tenant.  

V: Wat gebeurt er wanneer een verloopt?  
A: u krijgt een waarschuwing in de sectie certificaat in de pagina van de configuratie van toepassing. Wanneer een gebruiker probeert te krijgen tot de toepassing, wordt een beveiligingswaarschuwing weergegeven.  

V: wat moet ik doen als ik wil een certificaat voor een bepaalde app vervangen?  
A: upload een nieuw certificaat van de pagina van de configuratie van toepassing.  

V: kan ik een certificaat verwijderen en vervangen?  
A: wanneer u een nieuw certificaat uploaden als het oude certificaat niet door een andere toepassing wordt gebruikt is, worden deze automatisch verwijderd.  

V: Wat gebeurt er wanneer een certificaat is ingetrokken?  
A: intrekkingsreferenties controles worden niet uitgevoerd voor certificaten. Wanneer een gebruiker probeert te krijgen tot de toepassing, afhankelijk van de browser, mogelijk een beveiligingswaarschuwing weergegeven.  

V: kan ik een zelfondertekend certificaat gebruiken?  
A: Ja, zelfondertekende certificaten zijn toegestaan. Houd er rekening mee dat als u een persoonlijke certificeringsinstantie gebruikt, de CDP (certificaat intrekkingsreferenties punt verdeling punt) voor het certificaat moet openbare.  

V: is er een plaats om te zien van alle certificaten voor mijn tenant?  
A: dit wordt niet ondersteund in de huidige versie.  


## <a name="see-also"></a>Zie ook

- [Toepassingen met toepassingsproxy publiceren](active-directory-application-proxy-publish.md)
- [Eenmalige aanmelding inschakelen](active-directory-application-proxy-sso-using-kcd.md)
- [Voorwaardelijke toegang inschakelen](active-directory-application-proxy-conditional-access.md)
- [Uw aangepaste domeinnaam toevoegen aan Azure AD](active-directory-add-domain.md)

Voor het laatste nieuws en updates, raadpleegt u de [toepassingsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)
