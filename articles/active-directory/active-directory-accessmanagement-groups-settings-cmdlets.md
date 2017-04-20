<properties
    pageTitle="Azure Active Directory-cmdlets voor het configureren van instellingen voor | Microsoft Azure"
    description="Hoe de instellingen voor groepen met Azure Active Directory-cmdlets beheren."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory-cmdlets voor het configureren van de groepsinstellingen

De volgende instellingen voor collectieve groepen kunnen worden geconfigureerd in uw adreslijst:

1.  Classificaties: de door komma's gescheiden lijst met categorieën die gebruikers kunnen worden ingesteld op een groep. Voorbeelden zou "Krant", "Geheim" en "Bovenste geheim."

2.  Gebruik richtlijnen URL: een URL die gebruikers naar de gebruiksvoorwaarden voor het gebruik van Unified groepen, verwijst zoals gedefinieerd door uw organisatie. Deze URL wordt weergegeven in de gebruikersinterface waar gebruikers hierin groepen wilt gebruiken.

3.  Groep maken die zijn ingeschakeld: of geen, enkele of alle gebruikers mogen Unified groepen maken. Wanneer een ingesteld op aan, kunnen alle gebruikers groepen maken. Als de waarde op uit, kunnen geen gebruikers groepen maken. Wanneer uit, kunt u ook opgeven een beveiligingsgroep waarvan gebruikers die nog steeds kunnen maken van groepen.

Deze instellingen zijn geconfigureerd met een instellingen en SettingsTemplate objecten. In eerste instantie ziet niet u de voor instellingenobjecten in uw adreslijst. Dit betekent dat uw map is geconfigureerd met de standaardinstellingen. Als u wilt wijzigen van de standaardinstellingen, maakt u een nieuwe instellingenobject met een sjabloon instellingen. Instellingen-sjablonen zijn gedefinieerd door Microsoft.

U kunt de module met de cmdlets die worden gebruikt voor deze bewerkingen van de [site van Microsoft Connect](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)downloaden.

## <a name="create-settings-at-the-directory-level"></a>Instellingen op het mapniveau van de maken

Deze stappen maken instellingen op het mapniveau van de waarin op alle Office-groepen in de adreslijst toepassen.

1. Als u niet welke SettingTemplate weet gebruiken, wordt deze cmdlet de lijst met instellingen voor sjablonen:

    `Get-MsolAllSettingTemplate`

    ![Lijst met sjablonen voor instellingen](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. Als u wilt een URL van de richtlijn gebruik toevoegt, moet u eerst het SettingsTemplate-object dat de waarde van de URL in de gebruik richtlijn; definieert ophalen dat wil zeggen de sjabloon Group.Unified:

    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. Maak vervolgens een nieuwe instellingenobject op basis van die sjabloon:

    `$setting = $template.CreateSettingsObject()`

4. Vervolgens de waarde van de richtlijn gebruik bijwerken:

    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. Ten slotte de instellingen van toepassing:

    `New-MsolSettings –SettingsObject $setting`

    ![De URL van een gebruik richtlijn toevoegen](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Hier vindt u de instellingen in de Group.Unified SettingsTemplate.

 **Instelling**                          | **Beschrijving**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>Type: tekenreeks<li>Standaard: ""                  | Een door komma's gescheiden lijst met geldige classificatie-waarden die kunnen worden toegepast op Unified groepen.                
 <ul><li>EnableGroupCreation<li>Type: Booleaanse<li>Standaard: waar              | De vlag die aangeeft of het maken van groepen geïntegreerde is toegestaan in de adreslijst.                               
 <ul><li>GroupCreationAllowedGroupId<li>Type: tekenreeks<li>Standaard: ""         | GUID van de beveiligingsgroep dat is toegestaan om Unified groepen te maken, zelfs wanneer EnableGroupCreation == ONWAAR.
 <ul><li>UsageGuidelinesUrl<li>Type: tekenreeks<li>Standaard: ""                  | Een koppeling naar richtlijnen voor het gebruik van de groep.                                                                       

## <a name="read-settings-at-the-directory-level"></a>Instellingen op het mapniveau van de lezen

Deze stappen Lees instellingen op het mapniveau van de die van toepassing op alle Office-groepen in de adreslijst.

1. Lees alle bestaande directoryinstellingen:

    `Get-MsolAllSettings`

2. Alle instellingen voor een specifieke groep lezen:

    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. Instellingen voor specifieke map instellen, met SettingId GUID lezen:

    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

    ![Instellingen-ID-GUID](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>Instellingen op mapniveau bijwerken

Deze stappen bijwerken instellingen op het mapniveau van de waarin op alle Office-groepen in de adreslijst toepassen.

1. Het bestaande instellingen-object ophalen:

    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. Haal de waarde die u wilt bijwerken:

    `$value = $Setting.GetSettingsValue()`

3. De waarde bijwerken:

    `$value["AllowToAddGuests"] = "false"`

4. De instelling bijwerken:

    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>Instellingen op het mapniveau van de verwijderen

Deze stap verwijdert u instellingen op het mapniveau van de die van toepassing op alle Office-groepen in de adreslijst.

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Referentie voor kql cmdlet

U vindt meer Azure Active Directory PowerShell-documentatie bij [Azure Active Directory-Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260).

## <a name="settingstemplate-object-reference-groupunified-settingstemplate-object"></a>SettingsTemplate objectverwijzing (Group.Unified SettingsTemplate object)

- "naam": "EnableGroupCreation", "type": "System.Boolean", "defaultValue": "true", "Beschrijving": "Een Boole-vlag die aangeeft dat u als de functie geïntegreerd groep maken ingeschakeld is."

- "naam": "GroupCreationAllowedGroupId", "type": "System.Guid", "defaultValue": "", "Beschrijving": "GUID van de beveiligingsgroep die is whitelisted om geïntegreerde groepen te maken."

- "naam": "ClassificationList", "type": "System.String", "defaultValue": "", "Beschrijving": "Een door komma's gescheiden lijst met geldige classificatie-waarden die kunnen worden toegepast op groepen voor geïntegreerde."

- "naam": "UsageGuidelinesUrl", "type": "System.String", "defaultValue": "", "Beschrijving": "Een koppeling naar richtlijnen voor het gebruik van de groep."

naam | type | Standaardwaarde | Beschrijving
----------  | ----------  | ---------  | ----------
"EnableGroupCreation"  | "System.Boolean"  | "true"  | "Een Boole-vlag die aangeeft dat u als de functie geïntegreerd groep maken ingeschakeld is."
"GroupCreationAllowedGroupId"  | "System.Guid"  | ""  | "GUID van de beveiligingsgroep die is whitelisted maken van groepen Unified."
"ClassificationList"  | "System.String"  | ""  | "Een door komma's gescheiden lijst met geldige classificatie-waarden die kunnen worden toegepast op groepen Unified."
"UsageGuidelinesUrl"  | "System.String"  | ""  | "Een koppeling naar richtlijnen voor het gebruik van de groep."

## <a name="next-steps"></a>Volgende stappen

U vindt meer Azure Active Directory PowerShell-documentatie bij [Azure Active Directory-Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260).

Aanvullende instructies van Microsoft-programmamanager Rob de Jong is beschikbaar op [Van Flip groepen Blog](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad).

* [Toegang tot resources met Azure Active Directory-groepen beheren](active-directory-manage-groups.md)

* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
