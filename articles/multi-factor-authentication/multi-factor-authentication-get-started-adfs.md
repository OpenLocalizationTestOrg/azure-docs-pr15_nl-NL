<properties
    pageTitle="Azure MFA en AD FS | Microsoft Azure"
    description="Dit is de pagina van de Azure meervoudige verificatie die wordt beschreven hoe u aan de slag met Azure MFA en AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na" ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Aan de slag met Azure meervoudige verificatie en Active Directory Federation Services



<center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Als uw organisatie heeft federatieve uw lokale Active Directory met Azure Active Directory AD FS gebruikt, zijn er twee opties voor het gebruik van Azure meervoudige verificatie.

- Secure cloud resources met Azure meervoudige verificatie of Active Directory Federation Services
- Cloud en on-premises bronnen met Azure meervoudige verificatieserver beveiligen

De volgende tabel worden de verificatie-ervaring tussen resources met Azure meervoudige verificatie en AD FS beveiligen

|Verificatie-ervaring - Apps op basis van bladeren|Verificatie-ervaring - Apps niet op Browser gebaseerde
:------------- | :------------- | :------------- |
Beveiligen van Azure AD-bronnen met Azure meervoudige verificatie |<li>De eerste verificatiestap is uitgevoerd lokale AD FS gebruikt.</li> <li>De tweede stap is een methode telefoongebaseerde is uitgevoerd met de cloud-verificatie.</li>|Eindgebruikers kunt appwachtwoorden gebruiken om aan te melden.
Beveiligen van Azure AD-bronnen met Active Directory Federation Services |<li>De eerste verificatiestap is uitgevoerd lokale AD FS gebruikt.</li><li>De tweede stap wordt uitgevoerd on-premises door behouden blijft het claimen.</li>|Eindgebruikers kunt appwachtwoorden gebruiken om aan te melden.

Als u aanvullende opmerkingen met de appwachtwoorden voor federatieve gebruikers:

- Appwachtwoorden zijn gecontroleerd met de cloud-verificatie, zodat ze Federatie overslaan. Federatie alleen actief wordt gebruikt bij het instellen van een appwachtwoord.
- On-premises implementatie beheren in Access-Client-instellingen worden niet herkend door de appwachtwoorden.
- Er verloren on-premises implementatie de mogelijkheid verificatie-logboekregistratie voor appwachtwoorden.
- Account uitschakelen/verwijdering duurt maximaal drie uren voor directory-synchronisatie, vertragen uitschakelen/verwijdering van appwachtwoorden in de cloud-identiteit.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor informatie over het instellen van Azure meervoudige verificatie of de Server Azure meervoudige verificatie met AD FS:

- [Cloud resources met Azure meervoudige verificatie en AD FS verzamelen](multi-factor-authentication-get-started-adfs-cloud.md)
- [Cloud en on-premises bronnen Azure meervoudige verificatieserver gebruikt met Windows Server 2012 R2 AD FS beveiligen](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Cloud en on-premises bronnen Azure meervoudige verificatieserver gebruikt met AD FS 2.0 beveiligen](multi-factor-authentication-get-started-adfs-adfs2.md)
