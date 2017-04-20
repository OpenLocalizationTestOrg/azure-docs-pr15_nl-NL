<properties
  pageTitle="Azure IoT-Suite en Azure Active Directory | Microsoft Azure"
  description="Wordt beschreven hoe Azure IoT Suite Azure Active Directory gebruikt om machtigingen te beheren."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="10/24/2016"
  ms.author="araguila"/>
  
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Machtigingen voor de site azureiotsuite.com

## <a name="what-happens-when-you-sign-in"></a>Wat gebeurt er wanneer u zich aanmeldt

Wanneer u eerst aanmelden bij [azureiotsuite.com][lnk-azureiotsuite], bepaalt de machtigingsniveaus die u hebt op basis van de momenteel geselecteerde Azure Active Directory (AAD) tenant en Azure abonnement van de site.

1.  De site eerst vindt u informatie van Azure welke AAD-tenants waartoe u behoort om aan te vullen van de lijst met tenants gezien naast uw gebruikersnaam is aangemeld. Op dit moment kan aanvragen de site alleen gebruikerstokens voor één tenant per keer. Hierdoor wanneer u naar een ander-tenant met behulp van de vervolgkeuzelijst in de rechterbovenhoek overstapt, de site opnieuw meldt zich in aan die tenant voor de tokens voor die tenant.

2.  De site vindt vervolgens af van Azure welke abonnementen u hebt gekoppeld aan de geselecteerde tenant. De beschikbare abonnementen wordt weergegeven als u een nieuwe vooraf geconfigureerde oplossing maakt.

3.  Ten slotte de site worden alle opgehaald de resources in de abonnementen en resourcegroepen gelabeld als vooraf geconfigureerde oplossingen en vult de tegels op de startpagina.

De volgende secties worden de rollen die toegang tot de vooraf geconfigureerde oplossingen.

## <a name="aad-roles"></a>AAD rollen

De rollen AAD de mogelijkheid inrichten vooraf geconfigureerde oplossingen beheren en beheren van gebruikers in een vooraf geconfigureerde oplossing.

U vindt meer informatie over beheerdersrollen in AAD in [beheerdersrollen toewijzen in Azure AD][lnk-aad-admin], maar de nadruk in dit artikel ligt op de **Globale beheerder** en de **Gebruiker/lid van het domein** rollen zoals gebruikt door de vooraf geconfigureerde oplossingen.

**Hoofdbeheerder:** Kunnen er veel globale beheerders per pachter AAD zijn. Wanneer u een AAD-tenant maakt, bent u al dan niet standaard de globale beheerder van die tenant. De globale beheerder kunt inrichten van een vooraf geconfigureerde oplossing en **een beheerdersrol voor de toepassing binnen hun AAD-tenant** is toegewezen. Als u een andere gebruiker in dezelfde AAD tenant maakt een toepassing, is de standaardrol die de globale beheerder wordt verleend echter **Alleen impliciete lezen**. Globale beheerders kunnen toewijzen rollen voor toepassingen met behulp van de [Azure klassieke portal][lnk-classic-portal].

**Gebruiker/lid van het domein:** Kunnen er veel gebruikers/leden van het domein per pachter AAD zijn. Een gebruiker een vooraf geconfigureerde oplossing tot en met de [azureiotsuite.com] kunt inrichten[ lnk-azureiotsuite] site. De standaardrol die ze zijn verleend voor de toepassing die ze inrichten is **beheerder**. Ze kunnen een toepassing maken met het script build.cmd in de [azure-iot-remote-cmdlets voor controle] [ lnk-rm-github-repo] of [azure-iot-blog-onderhoud] [ lnk-pm-github-repo] opslagplaats, maar de standaardrol ze zijn verleend is **Impliciet alleen-lezen**, zoals ze bent niet gemachtigd rollen toewijzen. Als u een andere gebruiker in de tenant AAD maakt een toepassing, krijgen ze de rol **Impliciete alleen-lezen** al dan niet standaard voor de toepassing. Er geen de mogelijkheid om toe te wijzen rollen voor toepassingen. Daarom toevoegen niet gebruikers of rollen voor gebruikers voor een toepassing zelfs als ze deze is ingericht.

**Gast gebruiker/Gast:** Kunnen er veel Gast gebruikers/gasten per pachter AAD zijn. Gastgebruikers hebben een beperkt aantal rights in de tenant AAD. Daardoor kunnen geen gastgebruikers inrichten van een vooraf geconfigureerde oplossing in de tenant AAD.

Zie de volgende bronnen voor meer informatie:

- [Gebruikers maken of bewerken in Azure AD][lnk-create-edit-users]
- [App-rollen in AAD toewijzen][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Beheerdersrollen Azure-abonnement

De Azure beheerdersrollen bepalen de mogelijkheid een Azure-abonnement toewijzen aan een AD-tenant.

U vindt meer informatie over de rollen Azure collega beheerder, Service-beheerder en accountbeheerder in het artikel [over toevoegen of wijzigen van Azure collega beheerder, de Service-beheerder en de accountbeheerder][lnk-admin-roles].

## <a name="application-roles"></a>Toepassingsrollen

De rollen van de toepassing beheren toegang tot apparaten in de vooraf geconfigureerde oplossing.

Er zijn twee gedefinieerd en één impliciete rol die zijn gedefinieerd in de toepassing dat wordt gemaakt wanneer u een vooraf geconfigureerde oplossing inrichten.

-   **Beheerder:** Volledig beheer kunt toevoegen, beheren en verwijder apparaten heeft

-   **Alleen-lezen:** Mogelijkheid om weer te geven van apparaten heeft

-   **Impliciete gelezen alleen:** Dit is dezelfde als alleen-lezen, maar wordt verleend voor alle gebruikers van uw AAD-tenant. Dit is gebeurd uitkomt tijdens de ontwikkeling. U kunt deze rol verwijderen doordat de [RolePermissions.cs] [ lnk-resource-cs] bronbestand.

### <a name="changing-application-roles-for-a-user"></a>Toepassingsrollen van de voor een gebruiker wijzigen

U kunt de volgende procedure om te maken van een gebruiker in uw Active Directory een beheerder van de vooraf geconfigureerde oplossing.

U moet een globale beheerder van de AAD rollen voor een gebruiker wijzigen:

1. Ga naar de [portal van Azure klassieke][lnk-classic-portal].

2. Selecteer **Active Directory**.

3. Klik op de naam van uw AAD-tenant (dit is de map die u hebt gekozen op azureiotsuite.com wanneer u uw oplossing deze is ingericht).

4. Klik op **toepassingen**.

5. Klik op de naam van de toepassing die overeenkomt met de oplossingsnaam van de vooraf geconfigureerde. Als uw toepassing in de lijst niet wordt weergegeven, veranderen de decoratieve **weergeven** naar beneden af op **Mijn bedrijf eigenaar is van toepassingen** en klik op het vinkje.

7. Klik op **gebruikers**.

8. Selecteer de gebruiker die u wilt overstappen van rollen.

9. Klik op **toewijzen** en selecteer de rol (zoals **beheerder**) u wilt toewijzen aan de gebruiker, klik op het vinkje.

## <a name="faq"></a>FAQ

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-do-this"></a>Ik ben een service-beheerder en ik wil graag wijzigen van de toewijzing directory van mijn abonnement op een specifieke AAD-tenant. Hoe moet ik dit doen?

1. Ga naar de [portal van Azure klassieke][lnk-classic-portal], klik in de lijst met services aan de linkerkant op **Instellingen** .

2. Selecteer het abonnement dat u wilt wijzigen van de toewijzing wordt directory.

3. Klik op **Adreslijst bewerken**.

4. Selecteer de **map** die u wilt gebruiken in de vervolgkeuzelijst. Klik op de pijl naar rechts.

5. Bevestig de directory-toewijzing en beïnvloed CO-beheerders. Houd er rekening mee dat als u van een andere map verplaatst, alle CO-beheerders van de oorspronkelijke map zijn verwijderd.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Ik ben gebruiker/lid van een domein op de AAD-tenant en ik heb een vooraf geconfigureerde oplossing hebt gemaakt. Hoe krijg ik een rol toegewezen voor de toepassing?

Vraag een globale beheerder om toe te wijzen u als een globale beheerder in de tenant AAD voor machtigingen rollen toewijzen aan gebruikers zelf of vraag een globale beheerder aan u een rol toewijzen. Als u wilt wijzigen van de tenant AAD uw vooraf geconfigureerde oplossing is om te, zien de volgende vraag geïmplementeerd.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Hoe schakel ik de AAD-tenant die mijn externe controle vooraf geconfigureerde oplossing en de toepassing aan zijn toegewezen?

U kunt een cloud-implementatie uitvoeren vanaf <https://github.com/Azure/azure-iot-remote-monitoring> en implementeer deze opnieuw met een nieuw gemaakte AAD-tenant. Aangezien u al dan niet standaard een globale beheerder wanneer u een nieuwe AAD-tenant maakt, hebt u toegang voor het toevoegen van gebruikers en rollen toewijzen aan gebruikers.

1. Een nieuwe AAD-map maken in de [beheerportal van Azure][lnk-classic-portal].

2. Ga naar <https://github.com/Azure/azure-iot-remote-monitoring>.

3. Uitvoeren `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (bijvoorbeeld `build.cmd cloud debug myRMSolution`)

4. Wanneer u wordt gevraagd, stel de **tenantid** moeten de nieuw aangemaakte tenant in plaats van uw vorige tenant.


### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Ik wil een Service-beheerder of een collega beheerder wanneer aangemeld met een organisatorische account wijzigen

Zie het ondersteuningsartikel [servicebeheerder wijzigen en collega beheerder wanneer aangemeld met een organisatorische account][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Waarom zie ik deze fout? 'Uw account beschikt niet over de juiste machtigingen om een oplossing te maken. Neem contact op met uw accountbeheerder of probeer met een ander account.'

Bekijk het onderstaande diagram:

![][img-flowchart]

> [AZURE.NOTE] Als u nog steeds wordt weergegeven de fout na het valideren van u zijn een globale beheerder in de tenant AAD en de beheerder van een collega het abonnement, moet de beheerder van uw account de gebruiker verwijderen en opnieuw toewijzen onvoldoende machtigingen in deze volgorde: de gebruiker als een globale beheerder en voeg gebruiker toe als de beheerder van een collega van de Azure-abonnement. Als zich problemen blijven voordoen, neemt u contact [Help en ondersteuning voor][lnk-help-support].

**Waarom zie ik deze fout wanneer ik een Azure-abonnement heb?** *Een Azure-abonnement is vereist om vooraf geconfigureerde oplossingen te maken. U kunt een gratis proefabonnement-account maken in een paar minuten.*

Als u zeker weet dat u Azure-abonnement hebt, valideren van de tenant toewijzen voor uw abonnement en controleer of dat de juiste tenant is ingeschakeld in de vervolgkeuzelijst. Als u hebt gevalideerd de gewenste tenant juist is, volgt u de bovenstaande diagram en de toewijzing van uw abonnement en deze tenant AAD valideren.

## <a name="next-steps"></a>Volgende stappen

Als u wilt blijven leren over IoT Suite, Zie hoe u kunt met het [aanpassen van een vooraf geconfigureerde oplossing][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://azure.microsoft.com/documentation/articles/active-directory-application-manifest/
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
