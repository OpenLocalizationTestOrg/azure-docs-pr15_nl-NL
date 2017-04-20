<properties
    pageTitle="Aanmeldingsproblemen ervaringen met Azure AD identiteit beveiliging | Microsoft Azure"
    description="Biedt een overzicht van de gebruikerservaring als identiteitsbeveiliging is beperkt of verholpen van een gebruiker of als meervoudige verificatie is vereist door een beleid."
    services="active-directory"
    keywords="beveiliging in de Azure active directory-identiteit, cloud-app discovery,-toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid beheren"
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
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Aanmeldingsproblemen ervaringen met Azure AD identiteit-beveiliging

Met Azure Active Directory identiteitsbeveiliging, kunt u het volgende doen:

- gebruikers kunnen registreren voor meervoudige verificatie vereisen

- risicogevoelig gevonden aanmeldingen en getroffen gebruikers verwerken

Het antwoord van het systeem op deze problemen invloed is op de aanmeldervaring van een gebruiker omdat alleen rechtstreeks aanmelden door op te geven van een gebruikersnaam in te voeren en een wachtwoord zijn niet mogelijk meer. Er zijn extra stappen vereist om een gebruiker veilig weer aan bedrijven.

In dit onderwerp krijgt u een overzicht van de aanmeldervaring van een gebruiker voor alle gevallen die zich kunnen voordoen.

**Meervoudige verificatie**

- Registratie voor meervoudige verificatie



**Aanmelden op risico**

- Risicogevoelig gevonden aanmeldingsproblemen herstel

- Risicogevoelig gevonden aanmeldingsproblemen geblokkeerd

- Meervoudige verificatie registratie tijdens een risicogevoelig gevonden aanmelden
 

**Gebruiker risico**

- Beschadigde accountherstel

- Beschadigde account geblokkeerd




## <a name="multi-factor-authentication-registration"></a>Registratie voor meervoudige verificatie

De beste gebruikerservaring voor beide, de stroom van de herstel beschadigde account en de risicogevoelig gevonden stroom aanmeldingsproblemen is wanneer de gebruiker kan zelf herstellen. Als er gebruikers zijn geregistreerd voor meervoudige verificatie, al ze een telefoonnummer dat is gekoppeld aan hun account die kan worden gebruikt om de Beveiligingsuitdagingen doorgeven. Geen help helpdesk of beheerder betrokkenheid is herstellen uit de account compromissen nodig. Het is dus ten zeerste aanbevolen om te zorgen dat uw gebruikers die zijn geregistreerd voor meervoudige verificatie. 

Beheerders kunnen:

- een beleid instellen dat, moeten gebruikers hun accounts voor extra beveiliging verificatie instellen. 
- toestaan dat meervoudige verificatie registratie overslaan voor maximaal 30 dagen, geval ze gebruikers per respijtperiode willen voordat u zich registreert geven.

**De registratie meervoudige verificatie heeft drie stappen:**

1. In de eerste stap van ontvangt de gebruiker een melding over de vereiste het account voor meervoudige verificatie instellen. 

    ![Remediation] (./media/active-directory-identityprotection-flows/140.png "Remediation")


2. Meervoudige verificatie als u wilt instellen, moet u laten weten hoe u wilt contact met u opgenomen.

    ![Remediation] (./media/active-directory-identityprotection-flows/141.png "Remediation")
 
3. Het systeem een vraag aan verstuurt u en u moet reageren.

    ![Remediation] (./media/active-directory-identityprotection-flows/142.png "Remediation")

 



## <a name="risky-sign-in-recovery"></a>Risicogevoelig gevonden aanmeldingsproblemen herstel

Wanneer u een beheerder heeft een beleid voor aanmelden risico's geconfigureerd, wordt de betreffende gebruikers worden gesteld wanneer ze wil aanmelden. 

**De stroom voor risicogevoelig gevonden aanmeldingsproblemen heeft twee stappen:** 

1. De gebruiker geïnformeerd dat vestigen over hun aanmelden, zoals uit een nieuwe locatie, apparaat of app aanmelden is gevonden. 

    ![Remediation] (./media/active-directory-identityprotection-flows/120.png "Remediation")

2. De gebruiker is vereist voor hun identiteit bewijzen door het oplossen van een uitdaging beveiliging. Als de gebruiker is geregistreerd voor meervoudige verificatie vereist voor interactie een beveiligingscode naar hun telefoonnummer. Aangezien dit alleen maar een risicogevoelig gevonden aanmelden en niet een beschadigde account, wordt de gebruiker hoeft te wijzigen van het wachtwoord in deze stroom. 

    ![Remediation] (./media/active-directory-identityprotection-flows/121.png "Remediation")



 
## <a name="risky-sign-in-blocked"></a>Risicogevoelig gevonden aanmeldingsproblemen geblokkeerd
Beheerders kunt ook een beleid voor aanmelden risico voorkomen dat gebruikers bij het aanmelden afhankelijk van het risiconiveau instellen. Als u opgeheven, eindgebruikers moet contact opnemen met een netwerkbeheerder of de helpdesk of ze kunnen probeer aan te melden uit een vertrouwde locatie of het apparaat. Selfservice herstellen door het oplossen van meervoudige verificatie is niet een optie in dit geval.

![Remediation] (./media/active-directory-identityprotection-flows/200.png "Remediation")




## <a name="compromised-account-recovery"></a>Beschadigde accountherstel

Wanneer een gebruiker risico-beveiligingsbeleid is geconfigureerd, gebruikers die voldoen aan de gebruiker niveau die zijn opgegeven in het beleid risico (en dus wordt uitgegaan van is gehackt) moet gaan door de gebruiker compromissen herstel stroom voordat ze zich kunnen aanmelden. 

**De gebruiker compromissen herstel stroom heeft drie stappen:**

1. De gebruiker geïnformeerd dat hun accountbeveiliging risico vanwege verdachte activiteiten of meer van de referenties.

    ![Remediation] (./media/active-directory-identityprotection-flows/101.png "Remediation")

2.  De gebruiker is vereist voor hun identiteit bewijzen door het oplossen van een uitdaging beveiliging. Als de gebruiker is geregistreerd voor meervoudige verificatie kunnen ze zelf kunnen worden achterhaald herstellen. Moeten deze interactie een beveiligingscode naar hun telefoonnummer. 

    ![Remediation] (./media/active-directory-identityprotection-flows/110.png "Remediation")


3.  Ten slotte de gebruiker gedwongen naar hun wachtwoord wijzigen aangezien iemand anders toegang tot hun-account hebt. Schermafbeeldingen van deze ervaring zijn onder.
 
    ![Remediation] (./media/active-directory-identityprotection-flows/111.png "Remediation")



## <a name="compromised-account-blocked"></a>Beschadigde account geblokkeerd 

Als u een gebruiker die is geblokkeerd door een gebruiker risico beveiligingsbeleid voor niet-geblokkeerde, moet de gebruiker contact op met een beheerder of helpdesk. Selfservice herstellen door het oplossen van meervoudige verificatie is niet een optie in dit geval.


![Remediation] (./media/active-directory-identityprotection-flows/104.png "Remediation")



 
## <a name="reset-password"></a>Wachtwoord opnieuw instellen

Als getroffen gebruikers worden geblokkeerd niet aanmelden, kan een beheerder een tijdelijk wachtwoord genereren voor hen. De gebruikers hebben hun wachtwoord moet wijzigen tijdens een volgende aanmeldingsproblemen.

![Remediation] (./media/active-directory-identityprotection-flows/160.png "Remediation")


 




 

## <a name="see-also"></a>Zie ook

- [Beveiliging van Azure Active Directory-identiteit](active-directory-identityprotection.md) 