<properties
   pageTitle="Probleemoplossing: 'Active Directory' item is ontbreekt of is niet beschikbaar | Microsoft Azure "
   description="Wat moet u doen als u niet in de beheerportal van Azure Active Directory-menu-item wordt weergegeven."
   services="active-directory"
   documentationCenter="na"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Probleemoplossing: 'Active Directory' item is ontbreekt of is niet beschikbaar

Veel van de instructies voor het gebruik van de Azure Active Directory-functies en services beginnen met 'Gaat u naar de beheerportal Azure en klik op **Active Directory**.' Maar wat moet u doen als het Active Directory-extensie of menu-item niet wordt weergegeven of als deze is gemarkeerd als **Niet beschikbaar**? In dit onderwerp is bedoeld om te helpen. Hierin worden de voorwaarden waaronder **Active Directory** niet wordt weergegeven of is niet beschikbaar en wordt uitgelegd hoe verder te gaan.

## <a name="active-directory-is-missing"></a>Active Directory ontbreekt

Meestal een **Active Directory** -item wordt weergegeven in het linkernavigatiemenu. De instructies in Azure Active Directory-procedures wordt ervan uitgegaan dat dit item in uw weergave.

![Schermafbeelding: Active Directory in Azure wordt aangegeven](./media/active-directory-troubleshooting/typical-view.png)

Het Active Directory-item wordt weergegeven in het linkernavigatiemenu wanneer een van de volgende voorwaarden wordt voldaan. Anders het item niet wordt weergegeven.

* De huidige gebruiker die zijn aangemeld met een Microsoft-account (voorheen bekend als een Windows Live ID).

    OF-BEWERKING

* De Azure tenant heeft een map en de huidige rekening is een directory-beheerder.

    OF-BEWERKING

* De Azure tenant heeft ten minste één Azure AD-toegangsbeheer (ACS) naamruimte. Zie [Access besturingselement Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx)voor meer informatie.

    OF-BEWERKING

* De Azure tenant heeft ten minste één Azure meervoudige verificatie-provider. Zie [Azure meervoudige Verificatieproviders beheren](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)voor meer informatie.

Als u wilt maken van een naamruimte beheren in Access of een meervoudige verificatie-provider, klikt u op **+ Nieuw** > **App Services** > **Active Directory**.

Als u beheerdersrechten naar een map, hebt u een beheerder een beheerdersrol toewijzen aan uw account. Zie [beheerdersrollen toewijzen](active-directory-assign-admin-roles.md)voor meer informatie.

## <a name="active-directory-is-not-available"></a>Active Directory is niet beschikbaar

Wanneer u klikt op **+ Nieuw** > **App Services**, een **Active Directory** -item wordt weergegeven. Het Active Directory-item wordt specifiek, weergegeven wanneer er een van de Active Directory-functies, zoals de map, toegangsbeheer of meervoudige Auth Provider, beschikbaar voor de huidige gebruiker zijn.

Echter terwijl de pagina wordt geladen, het item grijs wordt weergegeven en is gemarkeerd als **Niet beschikbaar**. Dit is een tijdelijke status. Als u een paar seconden wachten, wordt het item beschikbaar. Als u de vertraging wordt verlengd, vaak vernieuwen van de pagina met webonderdelen het probleem is opgelost.

![Schermafbeelding: Active Directory is niet beschikbaar](./media/active-directory-troubleshooting/not-available.png)
