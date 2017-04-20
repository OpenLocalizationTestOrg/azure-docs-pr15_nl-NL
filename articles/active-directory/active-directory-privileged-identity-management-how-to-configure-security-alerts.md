<properties
   pageTitle="Het configureren van beveiligingsmeldingen | Microsoft Azure"
   description="Informatie over het configureren van beveiligingsmeldingen voor bevoegdheden identiteitsbeheer Azure extensie."
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
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Beveiligingsmeldingen configureren in Azure AD bevoegdheden identiteitsbeheer

## <a name="security-alerts"></a>Beveiligingsmeldingen
Azure bevoegdheden identiteit Management (PIM) genereert waarschuwingen als verdacht of onveilige activiteit in uw omgeving is. Als een waarschuwing wordt geactiveerd, wordt deze weergegeven op het dashboard PIM. Selecteer de melding om te zien van een rapport waarin de gebruikers of rollen die de waarschuwing geactiveerd.

![PIM dashboard beveiligingsmeldingen - schermafbeelding][1]



| Waarschuwen | Trigger | Aanbeveling |
| ----- | ------- | -------------- |
| **Rollen worden toegewezen buiten PIM** | Een beheerder is permanent toegewezen aan een rol, buiten de PIM-interface. | Bekijk de nieuwe toewijzing van de rol. Aangezien andere services alleen beheerders van de permanente toewijst kunnen, deze naar een in aanmerking komend toewijzing desgewenst wijzigen. |
| **Rollen worden te vaak geactiveerd** | Zijn er te veel heractiveringen van dezelfde rol binnen de tijd die is toegestaan in de instellingen. | Neem contact op met de gebruiker om te zien waarom ze hebt geactiveerd de rol zo vaak. De tijdslimiet is misschien te kort voor ze hun taken uit te voeren of wellicht ze scripts gebruikt voor een rol automatisch te activeren. |
| **Rollen vereisen niet meervoudige verificatie voor activering** | Er zijn rollen zonder MFA ingeschakeld in de instellingen. | We MFA vereisen voor de meest veel bevoegdheden rollen, maar raden MFA voor de activering van alle rollen in te schakelen. |
| **Beheerders van hun bevoegdheden rollen niet gebruikt** | Er zijn in aanmerking komend beheerders die hun rollen onlangs nog niet hebt geactiveerd. | Een access-revisie om te bepalen de gebruikers die niet meer nodig hebt access starten. |
| **Er worden te veel globale beheerders** | Er zijn meer globale beheerders dan aanbevolen. | Als u een groot aantal globale beheerders hebt, is het waarschijnlijk of gebruikers wel meer machtigingen dan zij nodig hebben. Gebruikers verplaatsen naar minder bevoegdheden rollen, of zorg enkele in aanmerking komen voor de rol in plaats van permanent toegewezen. |

## <a name="configure-security-alert-settings"></a>Waarschuwing beveiligingsinstellingen configureren

U kunt enkele van de beveiligingswaarschuwingen in PIM voor gebruik met uw omgeving en de beveiligingsdoelen aanpassen. Volg deze stappen om het blad instellingen hebt bereikt:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) en selecteer de tegel **Azure AD bevoegdheden identiteitsbeheer** vanuit het dashboard.
2. Selecteer **bevoegdheden rollen beheerde** > **Instellingen** > **Waarschuwingsinstellingen**.

    ![Navigeer naar de beveiligingsinstellingen voor meldingen][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>Melding "Rollen worden geactiveerd te vaak"

Deze waarschuwing activeert wanneer een gebruiker dezelfde bevoegdheden rol meerdere keren binnen een bepaalde termijn te activeren. U kunt zowel de periode en het aantal activeringen configureren.

- **Activering verlenging tijdsbestek**: opgeven in dagen, uren, minuten en de periode die u gebruiken wilt voor het bijhouden van verdachte vernieuwingen seconde.

- **Aantal activering vernieuwingen**: Geef het aantal activeringen, van 2 op 100, waarmee u rekening houden tot slot melding binnen het tijdsbestek u ervoor hebt gekozen. U kunt deze instelling wijzigen door de schuifregelaar verplaatsen of een getal in het tekstvak te typen.


### <a name="there-are-too-many-global-administrators-alert"></a>Melding "Er te veel globale beheerders"

PIM geeft deze waarschuwing als twee verschillende criteria is voldaan, en u ze kunt. Eerst moet u een bepaalde drempel van globale beheerders hebt bereikt. Tweede, moet een bepaald percentage van de totale roltoewijzingen globale beheerders. Als u alleen een van deze metingen overeenkomen, wordt de melding niet weergegeven.  

- **Minimumaantal globale beheerders**: Geef het aantal globale beheerders, 2 op 100, dat u rekening houden met een onveilige bedrag.

- **Percentage van de globale beheerders**: Geef het percentage van beheerders die globale beheerders zijn, van 0% tot 100%, die is onveilig in uw omgeving.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Melding "Beheerders niet hun bevoegdheden rollen gebruikt"

Deze waarschuwing activeert wanneer een gebruiker een bepaalde hoeveelheid tijd gaat zonder het activeren van een rol.

- **Het aantal dagen**: Geef het aantal dagen, van 0 tot 100, die een gebruiker kunt gaan zonder het activeren van een rol.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
