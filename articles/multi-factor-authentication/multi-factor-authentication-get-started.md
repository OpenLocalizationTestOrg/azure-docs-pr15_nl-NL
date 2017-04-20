<properties
    pageTitle="Azure MFA cloud tegenover server | Microsoft Azure"
    description="Kies de meervoudige verificatie secutiry-oplossing die rechts om door te vragen welke am i probeert te beveiligen en waar zijn mijn gebruikers die zich bevindt.  Kies cloud, MFA Server of AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Kies de oplossing Azure meervoudige verificatie voor u

Omdat er zijn verschillende typen van Azure meervoudige verificatie MFA (), moeten we enkele vragen om te bepalen welke versie het hulpmiddel is gebruik beantwoorden.  Deze vragen zijn:

-   [Wat heb ik probeert te beveiligen](#what-am-i-trying-to-secure)
-   [Waar bevinden de gebruikers](#where-are-the-users-located)
- [Welke functies heb ik nodig?](#what-featured-do-i-need)

De volgende secties geven richtlijnen voor het bepalen van elk van deze antwoorden.

## <a name="what-am-i-trying-to-secure"></a>Wat heb ik probeert te beveiligen?

Om te bepalen de juiste twee stappen verificatie-oplossing, eerst we de vraag moet beantwoorden van wat wilt u probeert te beveiligen met een tweede methode voor verificatie.  Is het een toepassing in Azure?  Of een RAS-systeem?  Door te bepalen wat we wilt beveiligen, kunnen we de vraag van waar meervoudige verificatie moet worden ingeschakeld.  


Wat wilt u beveiligen| Meervoudige verificatie in de cloud|Meervoudige verificatie-Server
------------- | :-------------: | :-------------: |
Directe Microsoft apps|● |● |
SaaS apps in de app-galerie|● |● |
IIS-toepassingen die zijn gepubliceerd via Azure AD-App-Proxy|● |● |
IIS-toepassingen die niet zijn gepubliceerd via Azure AD-App-Proxy | |● |
Externe toegang zoals VPN, RDG| |● |



## <a name="where-are-the-users-located"></a>Waar bevinden de gebruikers

Vervolgens helpt bekijkt onze gebruikers bij het bepalen van de juiste oplossing wilt gebruiken in de cloud of on-premises met de Server MFA.



Gebruikerslocatie| Meervoudige verificatie in de cloud|Meervoudige verificatie-Server
------------- | :-------------: | :-------------: |
Azure Active Directory|● | |
Azure AD en on-premises Federatie met AD FS met AD|● |● |
Azure AD en on-premises AD met DirSync, Azure AD-synchronisatie, Azure AD Connect - niet Wachtwoordsynchronisatie|● |● |
Azure AD en on-premises AD DirSync, Azure AD-synchronisatie, Azure AD Connect - gebruikt met Wachtwoordsynchronisatie|● | |
On-premises Active Directory| |● |

## <a name="what-features-do-i-need"></a>Welke functies heb ik nodig?

De volgende tabel wordt een vergelijking van de functies die beschikbaar met meervoudige verificatie in de cloud en met de meervoudige verificatie-Server zijn.

 | Meervoudige verificatie in de cloud | Meervoudige verificatie-Server
------------- | :-------------: | :-------------: |
Melding van de mobiele app als een tweede factor | ● | ● |
De mobiele app verificatiecode in die als een tweede factor | ● | ●
Telefoongesprek als tweede factor | ● | ●
Enkelvoudige SMS als tweede factor | ● | ●
Twee richtingen SMS als tweede factor |  | ●
Hardware-Tokens als tweede factor |  | ●
Appwachtwoorden voor clients die geen ondersteuning MFA bieden | ● |  
Beheerder controle over verificatiemethoden | ● | ●
PINCODE-modus |  | ●
Waarschuwing | ● | ●
MFA rapporten | ● | ●
Eenmalige overslaan |  | ●
Aangepaste begroeting voor telefoongesprekken | ● | ●
Aanpasbare Nummerweergave voor telefoongesprekken | ● | ●
Vertrouwde IP-adressen | ● | ●
MFA onthouden voor vertrouwde apparaten  | ● |  
Voorwaardelijke toegang | ● | ●
Cache |  | ●

Nu dat we hebt vastgesteld of cloud meervoudige verificatie of de MFA on-premises servers wilt gebruiken, we aan de slag kunt instellen en gebruiken van Azure meervoudige verificatie. **Selecteer het pictogram waarmee uw scenario!**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
