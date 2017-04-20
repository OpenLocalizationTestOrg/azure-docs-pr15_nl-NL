<properties
    pageTitle="Azure Active Directory identiteitsbeveiliging - hoe u gebruikers deblokkeren | Microsoft Azure"
    description="Informatie over hoe gebruikers die zijn geblokkeerd door een beleid Azure Active Directory identiteitsbeveiliging deblokkeren."
    services="active-directory"
    keywords="beveiliging van Azure active directory-identiteit, gebruiker de blokkering opheffen"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory identiteitsbeveiliging - hoe u gebruikers de blokkering opheffen

Met Azure Active Directory identiteitsbeveiliging, kunt u beleidsregels als u wilt gebruikers blokkeren als de geconfigureerde voorwaarden is voldaan. Meestal een geblokkeerde gebruiker contactpersonen helpdesk te worden opgeheven. In dit onderwerp worden de stappen beschreven die u kunt uitvoeren om de blokkering van een geblokkeerde gebruiker te.


## <a name="determine-the-reason-for-blocking"></a>De reden voor het blokkeren van bepalen

Als eerste stap om de blokkering van een gebruiker te, moet u bepalen welk beleid die de gebruiker heeft geblokkeerd omdat de volgende stappen zijn afhankelijk van het type. Met Azure Active Directory identiteitsbeveiliging, kan een gebruiker ofwel worden geblokkeerd door een beleid aanmeldingsproblemen risico of een gebruikersbeleid risico. 

U kunt het type van beleid die een gebruiker van de kop in het dialoogvenster die aan de gebruiker is gepresenteerd tijdens een poging aanmeldingsproblemen heeft geblokkeerd opvragen:

|Beleid | Gebruikersdialoogvenster|
|--- | --- |
|Aanmeldingsproblemen risico | ![Geblokkeerde aanmelden](./media/active-directory-identityprotection-unblock-howto/02.png) |
|Gebruiker risico | ![Geblokkeerde account](./media/active-directory-identityprotection-unblock-howto/104.png) |


Een gebruiker die is geblokkeerd door:

- Een beleid aanmeldingsproblemen risico wordt ook wel bekend als verdacht aanmelden
- Een risico gebruikersbeleid wordt ook wel een account risico

 
## <a name="unblocking-suspicious-sign-ins"></a>Blokkering verdachte aanmeldingen

Als u wilt deblokkeren een verdachte aanmeldingsproblemen, hebt u de volgende opties:

1. **Aanmelden vanuit een vertrouwde locatie of apparaat** - een reden voor geblokkeerde verdachte aanmeldingen zijn aanmeldingsproblemen pogingen van onbekende locaties of apparaten. Uw gebruikers bepalen snel of dit de blokkering reden door te gaan aanmelden van een vertrouwde locatie of het apparaat.


3. **Uitsluiten van beleid** - als u denkt dat de huidige configuratie van uw beleid aanmeldingsproblemen problemen met voor specifieke gebruikers veroorzaakt, kunt u de gebruikers uitsluiten van deze. Zie [aanmeldingsproblemen risico beleid](active-directory-identityprotection.md#sign-in-risk-policy) voor meer informatie.
 
4. **Beleid uitschakelt** - als u denkt dat de configuratie van uw beleid problemen met voor alle gebruikers veroorzaakt, kunt u het beleid uitschakelen. Zie [aanmeldingsproblemen risico beleid](active-directory-identityprotection.md#sign-in-risk-policy) voor meer informatie.


## <a name="unblocking-accounts-at-risk"></a>Blokkering accounts risico

Als u wilt deblokkeren een account risico, hebt u de volgende opties:

1. **Wachtwoord opnieuw** - kunt u de gebruikerswachtwoord opnieuw instellen. Zie [handmatige secure wachtwoord opnieuw instellen](active-directory-identityprotection.md#manual-secure-password-reset) voor meer informatie.

2. **Alle risico gebeurtenissen sluiten** - het risico gebruikersbeleid kan een gebruiker als het niveau van de risico's geconfigureerde gebruiker voor het blokkeren van access is bereikt. Kunt u een gebruiker verkleinen bevindt zich risiconiveau door te handmatig sluiten gerapporteerd risico's. Zie voor meer informatie, [risico's handmatig te sluiten](active-directory-identityprotection.md#closing-risk-events-manually).

3. **Uitsluiten van beleid** - als u denkt dat de huidige configuratie van uw beleid aanmeldingsproblemen problemen met voor specifieke gebruikers veroorzaakt, kunt u de gebruikers uitsluiten van deze. Zie [risico gebruikersbeleid](active-directory-identityprotection.md#user-risk-policy) voor meer informatie.
 
4. **Beleid uitschakelt** - als u denkt dat de configuratie van uw beleid problemen met voor alle gebruikers veroorzaakt, kunt u het beleid uitschakelen. Zie [risico gebruikersbeleid](active-directory-identityprotection.md#user-risk-policy) voor meer informatie.




## <a name="next-steps"></a>Volgende stappen

 Wilt u meer weten over de beveiliging van Azure AD identiteit? Bekijk de [Azure Active Directory identiteitsbeveiliging](active-directory-identityprotection.md).
 

