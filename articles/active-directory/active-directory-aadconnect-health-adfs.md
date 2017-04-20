
<properties
    pageTitle="Gebruik van Azure AD status verbinding met AD FS | Microsoft Azure"
    description="Dit is de pagina servicestatus Azure AD verbinding maken met het controleren van uw on-premises AD FS-infrastructuur."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-with-ad-fs"></a>Gebruik van Azure AD status verbinding met AD FS
De volgende documentatie hoort bij de infrastructuur van uw AD FS met Azure AD verbinding servicestatus bewaken. Zie voor informatie over het controleren van Azure AD Connect (-synchronisatie) met Azure AD verbinding status, [Gebruik Azure AD verbinding servicestatus voor synchronisatie](active-directory-aadconnect-health-sync.md). Zie bovendien voor informatie over het controleren van Active Directory Domain Services met Azure AD verbinding status, [Gebruik Azure AD status verbinding maken met AD DS](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Waarschuwingen voor AD FS
De sectie Azure AD verbinding systeemstatus waarschuwingen bevat de lijst met actieve waarschuwingen. Elke melding bevat relevante informatie, resolutie stappen en koppelingen naar gerelateerde documentatie.

Een waarschuwing actief of opgelost, als u wilt een nieuwe blade openen met aanvullende informatie, stappen voor het oplossen van de waarschuwing en koppelingen naar relevante documenten die u kunt uitvoeren, kunt u dubbelklikken. U kunt ook historische gegevens weergeven op waarschuwingen die zijn opgelost in het verleden.

![Azure AD Connect systeemstatus-Portal](./media/active-directory-aadconnect-health/alert2.png)



## <a name="usage-analytics-for-ad-fs"></a>Gebruiksanalyses voor AD FS
Azure AD verbinding systeemstatus gebruiksanalyses analyseert het verificatieverkeer van uw federatieservers. U kunt het vak gebruik analytics om te openen van het blad gebruiksanalyses, waarin u verschillende maatstaven en groeperingen dubbelklikken.

>[AZURE.NOTE] Als u wilt gebruiken gebruiksanalyses met AD FS, moet u ervoor zorgen dat de AD FS controleren is ingeschakeld. Zie de [Controle inschakelen voor AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs)voor meer informatie.

![Azure AD Connect systeemstatus-Portal](./media/active-directory-aadconnect-health/report1.png)

Selecteer extra statistieken, Geef een tijdsbereik of de groepering wijzigen, met de rechtermuisknop op de gebruik analytics-grafiek en selecteer grafiek bewerken. U kunt het tijdsbereik opgeven, selecteer een andere meting en de groepering wijzigen. U kunt de verdeling van de verificatie-verkeer is toegestaan op basis van verschillende "gegevens" weergeven en groeperen van elke metrisch met relevante "groeperen op' parameters die worden beschreven in de volgende tabel:

| Metrisch | Groeperen op | Wat de groepering steekproefgemiddelden en waarom is het nuttig? |
| ------ | -------- | -------------------------------------------- |
| Totaal aanvraagt: Het totale aantal aanvragen verwerkt door de service Federatie | Alle | Bevat het aantal totaal aantal aanvragen zonder groeperen. |
|  | Toepassing | Het totaal aantal aanvragen op basis van de gerichte gebruikmakende partij groepen. Deze groeperen is handig voor meer informatie over welke toepassing hoeveel percentage van de totale verkeer ontvangt. |
|  | Server | Groepen van het totaal aantal aanvragen op basis van de server waarop de aanvraag verwerkt. Deze groeperen is handig voor meer informatie over de verdeling van het laden van de totale verkeer. |
|  | Bedrijf Join | Groepen van het totaal aantal aanvragen op basis van de vraag of ze afkomstig zijn van apparaten die bedrijf die zijn gekoppeld zijn (bekende). Deze groeperen is handig om te begrijpen als uw resources zijn geopend met apparaten die niet kent de infrastructuur identiteit. |
|  | Verificatiemethode | Het totaal aantal aanvragen op basis van de verificatiemethode gebruikt voor verificatie van groepen. Deze groeperen is handig voor meer informatie over de gemeenschappelijke verificatiemethode die wordt gebruikt voor verificatie. Hier volgen de mogelijke verificatiemethoden <ol> <li>Geïntegreerde Windows-verificatie (Windows)</li> <li>Op formulieren gebaseerde verificatie (formulieren)</li> <li>Eenmalige aanmelding (eenmalige aanmelding)</li> <li>X509 certificaat verificatie (certificaat)</li> <br>Als de federatieservers de aanvraag met een Cookie eenmalige aanmelding ontvangen, wordt die aanvraag telt als eenmalige aanmelding (Single Sign On). In dat geval als de cookie geldig is, is de gebruiker wordt niet gevraagd referenties op te geven en naadloze toegang heeft tot de toepassing. Dit probleem geldt als er meerdere gebruikmakende partijen die zijn beveiligd met de federatieservers. |
|  | Netwerklocatie | Groepen van het totaal aantal aanvragen op basis van de locatie van de gebruiker. Het kan zijn dat beide intranet of extranet. Deze groeperen is handig om te weten hoeveel procent van het verkeer is die afkomstig zijn uit het intranet versus extranet. |
| Totaal aantal mislukte aanvragen: Het totale aantal mislukte aanvragen die zijn verwerkt door de service Federatie. <br> (Deze metrisch is alleen beschikbaar op AD FS voor Windows Server 2012 R2)| Foutwaarden van Microsoft Excel | Toont het aantal fouten op basis van vooraf gedefinieerde fouttypen. Deze groeperen is handig voor meer informatie over de voorkomende fouten. <ul><li>Onjuiste gebruikersnaam of wachtwoord: fouten vanwege onjuiste gebruikersnaam of wachtwoord.</li> <li>"Extranet accountvergrendeling": fouten als gevolg van het aanvragen van een gebruiker die is vergrendeld uit extranet ontvangen </li><li> 'Wachtwoord verlopen': fouten vanwege de gebruikers die zich aan met een wachtwoord van verlopen.</li><li>'Uitgeschakeld Account': fouten vanwege gebruikers logboekregistratie met een uitgeschakelde account.</li><li>"Apparaat verificatie": fouten vanwege gebruikers om te verifiëren met apparaat verificatie is verbroken.</li><li>"Certificaat gebruikersverificatie": fouten vanwege gebruikers om te verifiëren vanwege een ongeldige certificaat is verbroken.</li><li>"MFA": fouten vanwege gebruiker om te verifiëren met meervoudige verificatie is verbroken.</li><li>"Andere referentie": "Uitgifte autorisatie": fouten vanwege autorisatie fouten.</li><li>"Uitgifte delegeren": fouten door uitgifte delegeren fouten.</li><li>"Token acceptatie": fouten vanwege ADFS te negeren het token van een identiteitsprovider van derden.</li><li>"Protocol": mislukt vanwege protocolfouten.</li><li>'Onbekend': alle onderschept. Eventuele andere fouten die niet in de gedefinieerde categorieën passen.</li> |
|  | Server | Groepen de fouten op basis van de server. Deze groeperen is handig voor meer informatie over de fout-verdeling voor servers. Ongelijkmatige spreiding mogelijk een aanduiding van een server in een defect staat. |
|  | Netwerklocatie | Groepen de fouten op basis van de locatie van de aanvragen (extranet intranet vs). Deze groeperen is handig voor meer informatie over het type van aanvragen die worden verbroken. |
|  | Toepassing | Groepen de fouten op basis van de gerichte toepassing (gebruikmakende partijen). Deze groeperen is handig voor meer informatie over welke gerichte toepassing meeste aantal fouten ziet. |
| Het aantal gebruikers: kunt u het gemiddelde aantal unieke gebruikers in het systeem actief | Alle | Deze metrisch biedt een telling van het gemiddelde aantal gebruikers met de Federatie-service in het segment geselecteerde tijd. De gebruikers die zijn niet gegroepeerd. <br>Het gemiddelde, is afhankelijk van het tijdsegment is geselecteerd. |
|  | Toepassing | Groepen van het gemiddelde aantal gebruikers op basis van de gerichte toepassing (gebruikmakende partijen). Deze groeperen is handig voor meer informatie over hoeveel gebruikers welke toepassing gebruikt. |


## <a name="performance-monitoring-for-ad-fs"></a>Prestaties controleren voor AD FS
Azure AD verbinding prestaties statuscontrole wordt controleren aandacht besteed aan de doelstellingen. Als u de controle inschakelt, wordt een nieuwe blade geopend met gedetailleerde informatie over het aan de doelstellingen.


![Azure AD Connect systeemstatus-Portal](./media/active-directory-aadconnect-health/perf1.png)


Als u de filteroptie aan de bovenkant van het blad selecteert, kunt u filteren op de server om een afzonderlijke server aan de doelstellingen weer te geven. Als u wilt wijzigen van de doelstellingen, met de rechtermuisknop op de grafiek controleren onder het blad controleren en selecteer grafiek bewerken. Vervolgens gaat u in het nieuwe blad dat wordt geopend, kunt u extra statistieken Selecteer in de vervolgkeuzelijst en geef een tijdsbereik voor het weergeven van de prestatiegegevens.

## <a name="reports-for-ad-fs"></a>Rapporten voor AD FS
Azure AD verbinding systeemstatus biedt rapporten over activiteit en prestaties van AD FS. Deze rapporten waarmee beheerders kunnen inzicht in de activiteiten op hun AD FS-servers.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>Top 50 gebruikers met mislukte gebruikersnaam en wachtwoord aanmeldingen

Een van de enkele veelvoorkomende redenen om een verificatieaanvraag mislukte op een AD FS-server is een aanvraag met ongeldige referenties, dat wil zeggen verkeerde gebruikersnaam of wachtwoord. Meestal gebeurt aan gebruikers vanwege complexe wachtwoorden, vergeten wachtwoorden of typefouten.

Maar er zijn andere redenen hetgeen kunnen resulteren in een onverwacht aantal aanvragen worden verwerkt door uw AD FS-servers, zoals: een toepassing die cache gebruikersreferenties en de referenties verlopen of een kwaadwillende gebruiker probeert u zich aanmeldt bij een account in bij een reeks bekende wachtwoorden. De volgende twee voorbeelden kunnen goede redenen dat tot een toename aanvragen leiden kunnen.

Azure AD verbinding servicestatus voor ADFS biedt een rapport over Actiefste 50 gebruikers met mislukte aanmelding pogingen vanwege een ongeldige gebruikersnaam of wachtwoord. Dit rapport wordt bereikt door het verwerken van de controlegebeurtenissen gegenereerd door de AD FS-servers op de bedrijven

![Azure AD Connect systeemstatus-Portal](./media/active-directory-aadconnect-health-adfs/report1a.png)

U hebt eenvoudig toegang tot de volgende onderdelen van de informatie in de lijst:

- Totaal aantal mislukte aanvragen met verkeerde gebruikersnaam en wachtwoord in de afgelopen 30 dagen
- Gemiddelde aantal gebruikers dat is mislukt met een ongeldige gebruikersnaam en wachtwoord aanmelding per dag.


In dit onderdeel klikken Hiermee, gaat u naar het blad hoofdrapport vindt u meer informatie. Deze blade bevat een grafiek met trending informatie om te helpen stand brengen van een basislijn over aanvragen met verkeerde gebruikersnaam of wachtwoord. Daarnaast biedt dit de lijst met gebruikers van de belangrijkste 50 met het aantal mislukte pogingen.

In de grafiek bevat de volgende informatie:

- Het totale aantal mislukte aanmeldingen vanwege een ongeldige gebruikersnaam en wachtwoord op basis van de per dag.
- Het totale aantal unieke gebruikers die niet kon aanmeldingen op basis van de per dag.
- IP-adres van client van voor de laatste aanvraag

![Azure AD Connect systeemstatus-Portal](./media/active-directory-aadconnect-health-adfs/report3a.png)

Het rapport bevat de volgende informatie:

| Rapportitem | Beschrijving
| ------ | -------- |
|Gebruikers-ID| Ziet u de gebruikers-ID die is gebruikt. Deze waarde is wat de gebruiker heeft getypt, die in sommige gevallen wordt de verkeerde gebruiker-ID die wordt gebruikt.|
|Mislukte pogingen| Ziet u het totaal aantal mislukte pogingen voor die specifieke gebruikers-ID. De tabel is met de meest aantal mislukte pogingen in aflopende volgorde gesorteerd.|
|Laatste is mislukt| Geeft het tijdstempel wanneer de laatste fout is opgetreden.
|Fout bij laatst IP | Ziet u de Client-IP-adres van de meest recente ongeldige aanvraag.|



>[AZURE.NOTE] Dit rapport wordt automatisch bijgewerkt nadat elke twee uur met de nieuwe informatie die worden verzameld in die tijd. Mogelijk login pogingen in de afgelopen twee uur daardoor niet opgenomen in het rapport.



## <a name="related-links"></a>Verwante koppelingen

* [Azure AD Connect systeemstatus](active-directory-aadconnect-health.md)
* [Azure AD Connect systeemstatus Agent installatie](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect systeemstatus bewerkingen](active-directory-aadconnect-health-operations.md)
* [Met behulp van Azure AD verbinding servicestatus voor synchronisatie](active-directory-aadconnect-health-sync.md)
* [Gebruik van Azure AD status verbinding met AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect systeemstatus Veelgestelde vragen](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect systeemstatus versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
