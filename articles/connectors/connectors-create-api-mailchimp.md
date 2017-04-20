<properties
pageTitle="MailChimp | Microsoft Azure"
description="Logica apps maken met de App Azure-service. MailChimp is een SaaS-service waarmee bedrijven om te beheren en de marketingactiviteiten e-mail, waaronder het verzenden van e-mailberichten marketing, geautomatiseerde berichten en gerichte campagnes automatiseren."
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-mailchimp-connector"></a>Aan de slag met de verbindingslijn MailChimp

MailChimp is een SaaS-service waarmee bedrijven om te beheren en de marketingactiviteiten e-mail, waaronder het verzenden van e-mailberichten marketing, geautomatiseerde berichten en gerichte campagnes automatiseren.


>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie.

U kunt aan de slag met het maken van een app logica nu, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De verbindingslijn MailChimp kan worden gebruikt als een actie; trigger(s) heeft. Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen.

 De verbindingslijn MailChimp heeft de volgende of meerdere acties en/of trigger(s) beschikbaar:

### <a name="mailchimp-actions"></a>MailChimp acties
U kunt deze of meerdere acties uitvoeren:

|Actie|Beschrijving|
|--- | ---|
|[newcampaign](connectors-create-api-mailchimp.md#newcampaign)|Maak een nieuwe campagne op basis van een Type campagne, lijst met geadresseerden en campagne instellingen (onderwerpregel, titel, from_name en reply_to)|
|[newlist](connectors-create-api-mailchimp.md#newlist)|Maak een nieuwe lijst in uw account MailChimp|
|[AddMember](connectors-create-api-mailchimp.md#addmember)|Toevoegen of bijwerken van een lid van de|
|[RemoveMember](connectors-create-api-mailchimp.md#removemember)|Een lid uit een lijst verwijderen.|
|[updatemember](connectors-create-api-mailchimp.md#updatemember)|Bijwerkgegevens voor een specifieke lijst-lid|
### <a name="mailchimp-triggers"></a>MailChimp-triggers
U kunt luisteren voor deze gebeurtenis(sen):

|Trigger | Beschrijving|
|--- | ---|
|Wanneer een lid is toegevoegd aan een lijst|Een werkstroom voor de gebeurtenis wanneer een nieuw lid is toegevoegd aan een lijst|
|Wanneer een nieuwe lijst wordt gemaakt|Een werkstroom voor de gebeurtenis wanneer een nieuwe lijst wordt gemaakt|


## <a name="create-a-connection-to-mailchimp"></a>Een verbinding maken met MailChimp
Als u wilt maken logica apps met MailChimp, moet u eerst een **verbinding** maken en de details bieden voor de volgende eigenschappen:

|Eigenschap| Vereist|Beschrijving|
| ---|---|---|
|Token|Ja|MailChimp referenties opgeven|

>[AZURE.INCLUDE [Steps to create a connection to MailChimp](../../includes/connectors-create-api-mailchimp.md)]

>[AZURE.TIP] U kunt deze verbinding gebruiken in andere apps logica.

## <a name="reference-for-mailchimp"></a>Overzicht van MailChimp
Dit geldt voor versie: 1.0

## <a name="newcampaign"></a>newcampaign
Nieuwe campagne: Maak een nieuwe campagne op basis van een Type campagne, lijst met geadresseerden en campagne instellingen (onderwerpregel, titel, from_name en reply_to)

```POST: /campaigns```

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|newCampaignRequest| |Ja|hoofdtekst|geen|JSON object verzenden in de hoofdtekst met parameters voor de nieuwe campagne-aanvraag|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="newlist"></a>newlist
Nieuwe lijst: Maak een nieuwe lijst in uw account MailChimp

```POST: /lists```

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|newListRequest| |Ja|hoofdtekst|geen|JSON object verzenden in de hoofdtekst met parameters voor de nieuwe campagne-aanvraag|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="addmember"></a>AddMember
Lid toevoegen aan lijst: toevoegen of bijwerken van een lid van de

```POST: /lists/{list_id}/members```

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|tekenreeks|Ja|pad|geen|De unieke id voor de lijst|
|newMemberInList| |Ja|hoofdtekst|geen|JSON object verzenden in de hoofdtekst met de nieuwe lid informatie|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="removemember"></a>RemoveMember
Lid verwijderen uit lijst: een lid uit een lijst verwijderen.

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|tekenreeks|Ja|pad|geen|De unieke id voor de lijst|
|member_email|tekenreeks|Ja|pad|geen|Het e-mailadres van het lid verwijderen|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="updatemember"></a>updatemember
Bijwerken van gegevens: gegevens voor een specifieke lijstlid bijwerken

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|tekenreeks|Ja|pad|geen|De unieke id voor de lijst|
|member_email|tekenreeks|Ja|pad|geen|De unieke e-mailadres van het lid wilt bijwerken|
|updateMemberInListRequest| |Ja|hoofdtekst|geen|JSON object verzenden in de hoofdtekst met de bijgewerkte lid informatie|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="onmembersubscribed"></a>OnMemberSubscribed
Wanneer een lid is toegevoegd aan een lijst: een werkstroom voor de gebeurtenis wanneer een nieuw lid is toegevoegd aan een lijst

```GET: /trigger/lists/{list_id}/members```

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|list_id|tekenreeks|Ja|pad|geen|De unieke id voor de lijst|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="oncreatelist"></a>OnCreateList
Wanneer een nieuwe lijst wordt gemaakt: een werkstroom voor de gebeurtenis wanneer een nieuwe lijst wordt gemaakt

```GET: /trigger/lists```

Er zijn geen parameters voor dit gesprek
#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|OK|
|202|Geaccepteerd|
|400|Ongeldige aanvraag|
|401|Onbevoegde|
|403|Verboden|
|404|Niet gevonden|
|500|Interne serverfout. Onbekende fout|
|Standaard|Bewerking is mislukt.|


## <a name="object-definitions"></a>Objectdefinities

### <a name="newcampaignrequest"></a>NewCampaignRequest


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|type|tekenreeks|Ja |
|geadresseerden|niet gedefinieerd|Ja |
|Instellingen|niet gedefinieerd|Ja |
|variate_settings|niet gedefinieerd|Nee |
|bijhouden|niet gedefinieerd|Nee |
|rss_opts|niet gedefinieerd|Nee |
|social_card|niet gedefinieerd|Nee |



### <a name="recipient"></a>Geadresseerde


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|list_id|tekenreeks|Ja |
|segment_opts|niet gedefinieerd|Nee |



### <a name="settings"></a>Instellingen


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|subject_line|tekenreeks|Ja |
|titel|tekenreeks|Nee |
|from_name|tekenreeks|Ja |
|reply_to|tekenreeks|Ja |
|use_conversation|Booleaanse waarde|Nee |
|to_name|tekenreeks|Nee |
|folder_id|geheel getal|Nee |
|verifiëren|Booleaanse waarde|Nee |
|auto_footer|Booleaanse waarde|Nee |
|inline_css|Booleaanse waarde|Nee |
|auto_tweet|Booleaanse waarde|Nee |
|auto_fb_post|matrix|Nee |
|fb_comments|Booleaanse waarde|Nee |



### <a name="variatesettings"></a>Variate_Settings


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|winner_criteria|tekenreeks|Nee |
|wachttijd|geheel getal|Nee |
|test_size|geheel getal|Nee |
|subject_lines|matrix|Nee |
|send_times|matrix|Nee |
|from_names|matrix|Nee |
|reply_to_addresses|matrix|Nee |



### <a name="tracking"></a>Bijhouden


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|wordt geopend|Booleaanse waarde|Nee |
|html_clicks|Booleaanse waarde|Nee |
|text_clicks|Booleaanse waarde|Nee |
|goal_tracking|Booleaanse waarde|Nee |
|ecomm360|Booleaanse waarde|Nee |
|google_analytics|tekenreeks|Nee |
|clicktale|tekenreeks|Nee |
|SalesForce|niet gedefinieerd|Nee |
|highrise|niet gedefinieerd|Nee |
|Capsule|niet gedefinieerd|Nee |



### <a name="rssopts"></a>RSS_Opts


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|feed_url|tekenreeks|Nee |
|frequentie|tekenreeks|Nee |
|constrain_rss_img|tekenreeks|Nee |
|planning|niet gedefinieerd|Nee |



### <a name="socialcard"></a>Social_Card


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|image_url|tekenreeks|Nee |
|Beschrijving|tekenreeks|Nee |
|titel|tekenreeks|Nee |



### <a name="segmentopts"></a>Segment_Opts


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|saved_segment_id|geheel getal|Nee |
|de zoekwaarde|tekenreeks|Nee |



### <a name="salesforce"></a>SalesForce


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|campagne|Booleaanse waarde|Nee |
|notities|Booleaanse waarde|Nee |



### <a name="highrise"></a>Highrise


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|campagne|Booleaanse waarde|Nee |
|notities|Booleaanse waarde|Nee |



### <a name="capsule"></a>Capsule


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|notities|Booleaanse waarde|Nee |



### <a name="schedule"></a>Planning


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|uur|geheel getal|Nee |
|daily_send|niet gedefinieerd|Nee |
|weekly_send_day|tekenreeks|Nee |
|monthly_send_date|getal|Nee |



### <a name="dailysend"></a>Daily_Send


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|zondag|Booleaanse waarde|Nee |
|maandag|Booleaanse waarde|Nee |
|Dinsdag|Booleaanse waarde|Nee |
|woensdag|Booleaanse waarde|Nee |
|donderdag|Booleaanse waarde|Nee |
|vrijdag|Booleaanse waarde|Nee |
|zaterdag|Booleaanse waarde|Nee |



### <a name="campaignresponsemodel"></a>CampaignResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Nee |
|type|tekenreeks|Nee |
|create_time|tekenreeks|Nee |
|archive_url|tekenreeks|Nee |
|status|tekenreeks|Nee |
|emails_sent|geheel getal|Nee |
|send_time|tekenreeks|Nee |
|CONTENT_TYPE|tekenreeks|Nee |
|geadresseerde|matrix|Nee |
|Instellingen|niet gedefinieerd|Nee |
|variate_settings|niet gedefinieerd|Nee |
|bijhouden|niet gedefinieerd|Nee |
|rss_opts|niet gedefinieerd|Nee |
|ab_split_opts|niet gedefinieerd|Nee |
|social_card|niet gedefinieerd|Nee |
|report_summary|niet gedefinieerd|Nee |
|delivery_status|niet gedefinieerd|Nee |
|_links|matrix|Nee |



### <a name="absplitopts"></a>AB_Split_Opts


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|split_test|tekenreeks|Nee |
|pick_winner|tekenreeks|Nee |
|wait_units|tekenreeks|Nee |
|wachttijd|geheel getal|Nee |
|split_size|geheel getal|Nee |
|from_name_a|tekenreeks|Nee |
|from_name_b|tekenreeks|Nee |
|reply_email_a|tekenreeks|Nee |
|reply_email_b|tekenreeks|Nee |
|subject_a|tekenreeks|Nee |
|subject_b|tekenreeks|Nee |
|send_time_a|tekenreeks|Nee |
|send_time_b|tekenreeks|Nee |
|send_time_winner|tekenreeks|Nee |



### <a name="reportsummary"></a>Report_Summary


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|wordt geopend|geheel getal|Nee |
|unique_opens|geheel getal|Nee |
|open_rate|getal|Nee |
|klikken|geheel getal|Nee |
|subscriber_clicks|getal|Nee |
|click_rate|getal|Nee |



### <a name="deliverystatus"></a>Delivery_Status


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ingeschakeld|Booleaanse waarde|Nee |
|can_cancel|Booleaanse waarde|Nee |
|status|tekenreeks|Nee |
|emails_sent|geheel getal|Nee |
|emails_canceled|geheel getal|Nee |



### <a name="link"></a>Koppeling


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ver|tekenreeks|Nee |
|href|tekenreeks|Nee |
|methode|tekenreeks|Nee |
|targetSchema|tekenreeks|Nee |
|schema|tekenreeks|Nee |



### <a name="newlistrequest"></a>NewListRequest


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|naam|tekenreeks|Ja |
|Neem contact op met|niet gedefinieerd|Ja |
|permission_reminder|tekenreeks|Ja |
|use_archive_bar|Booleaanse waarde|Nee |
|campaign_defaults|niet gedefinieerd|Ja |
|notify_on_subscribe|tekenreeks|Nee |
|notify_on_unsubscribe|tekenreeks|Nee |
|email_type_option|Booleaanse waarde|Ja |
|zichtbaarheid|tekenreeks|Nee |



### <a name="contact"></a>Contactpersoon


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|bedrijf|tekenreeks|Ja |
|adres1|tekenreeks|Ja |
|adres2|tekenreeks|Nee |
|plaats|tekenreeks|Ja |
|de staat|tekenreeks|Ja |
|Postcode|tekenreeks|Ja |
|land|tekenreeks|Ja |
|telefoon|tekenreeks|Ja |



### <a name="campaigndefaults"></a>Campaign_Defaults


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|from_name|tekenreeks|Ja |
|from_email|tekenreeks|Ja |
|Onderwerp|tekenreeks|Nee |
|taal|tekenreeks|Ja |



### <a name="createnewlistresponsemodel"></a>CreateNewListResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Ja |
|naam|tekenreeks|Ja |
|Neem contact op met|niet gedefinieerd|Ja |
|permission_reminder|tekenreeks|Ja |
|use_archive_bar|Booleaanse waarde|Nee |
|campaign_defaults|niet gedefinieerd|Ja |
|notify_on_subscribe|tekenreeks|Nee |
|notify_on_unsubscribe|tekenreeks|Nee |
|Date_Created|tekenreeks|Nee |
|list_rating|geheel getal|Nee |
|email_type_option|Booleaanse waarde|Ja |
|subscribe_url_short|tekenreeks|Nee |
|subscribe_url_long|tekenreeks|Nee |
|beamer_address|tekenreeks|Nee |
|zichtbaarheid|tekenreeks|Nee |
|modules|matrix|Nee |
|Stat|niet gedefinieerd|Nee |
|_links|matrix|Nee |



### <a name="stats"></a>Stat


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|member_count|geheel getal|Nee |
|unsubscribe_count|geheel getal|Nee |
|cleaned_count|geheel getal|Nee |
|member_count_since_send|geheel getal|Nee |
|unsubscribe_count_since_send|geheel getal|Nee |
|cleaned_count_since_send|geheel getal|Nee |
|campaign_count|geheel getal|Nee |
|campaign_last_sent|geheel getal|Nee |
|merge_field_count|geheel getal|Nee |
|avg_sub_rate|getal|Nee |
|avg_unsub_rate|getal|Nee |
|target_sub_rate|getal|Nee |
|open_rate|getal|Nee |
|click_rate|getal|Nee |
|last_sub_date|tekenreeks|Nee |
|last_unsub_date|tekenreeks|Nee |



### <a name="getlistsresponsemodel"></a>GetListsResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|lijsten|matrix|Nee |
|total_items|geheel getal|Nee |



### <a name="newmemberinlistrequest"></a>NewMemberInListRequest


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|email_type|tekenreeks|Nee |
|status|tekenreeks|Ja |
|merge_fields|niet gedefinieerd|Nee |
|interesses|tekenreeks|Nee |
|taal|tekenreeks|Nee |
|VIP|Booleaanse waarde|Nee |
|locatie|niet gedefinieerd|Nee |
|mailadres|tekenreeks|Ja |



### <a name="firstandlastname"></a>FirstAndLastName


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|FNAME|tekenreeks|Nee |
|LNAME|tekenreeks|Nee |



### <a name="location"></a>Locatie


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|breedtegraad|getal|Nee |
|lengtegegevens|getal|Nee |



### <a name="memberresponsemodel"></a>MemberResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Nee |
|mailadres|tekenreeks|Nee |
|unique_email_id|tekenreeks|Nee |
|email_type|tekenreeks|Nee |
|status|tekenreeks|Nee |
|merge_fields|niet gedefinieerd|Nee |
|interesses|tekenreeks|Nee |
|Stat|niet gedefinieerd|Nee |
|ip_signup|tekenreeks|Nee |
|timestamp_signup|tekenreeks|Nee |
|ip_opt|tekenreeks|Nee |
|timestamp_opt|tekenreeks|Nee |
|member_rating|geheel getal|Nee |
|last_changed|tekenreeks|Nee |
|taal|tekenreeks|Nee |
|VIP|Booleaanse waarde|Nee |
|email_client|tekenreeks|Nee |
|locatie|niet gedefinieerd|Nee |
|last_note|niet gedefinieerd|Nee |
|list_id|tekenreeks|Nee |
|_links|matrix|Nee |



### <a name="lastnote"></a>Last_Note


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|note_id|geheel getal|Nee |
|created_at|tekenreeks|Nee |
|created_by|tekenreeks|Nee |
|Opmerking|tekenreeks|Nee |



### <a name="getallmembersresponsemodel"></a>GetAllMembersResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|leden|matrix|Nee |
|list_id|tekenreeks|Nee |
|total_items|geheel getal|Nee |



### <a name="object"></a>Object






### <a name="updatememberinlistrequest"></a>UpdateMemberInListRequest


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|mailadres|tekenreeks|Nee |
|email_type|tekenreeks|Nee |
|status|tekenreeks|Ja |
|merge_fields|niet gedefinieerd|Nee |
|interesses|tekenreeks|Nee |
|taal|tekenreeks|Nee |
|VIP|Booleaanse waarde|Nee |
|locatie|niet gedefinieerd|Nee |



### <a name="getmembersresponsemodel"></a>GetMembersResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|leden|matrix|Nee |
|list_id|tekenreeks|Nee |
|total_items|geheel getal|Nee |



### <a name="adduserresponsemodel"></a>AddUserResponseModel


| Naam van eigenschap | Gegevenstype | Vereist |
|---|---|---|
|ID|tekenreeks|Ja |
|mailadres|tekenreeks|Ja |
|unique_email_id|tekenreeks|Nee |
|email_type|tekenreeks|Nee |
|status|tekenreeks|Nee |
|merge_fields|niet gedefinieerd|Ja |
|interesses|tekenreeks|Nee |
|Stat|niet gedefinieerd|Nee |
|ip_signup|tekenreeks|Nee |
|timestamp_signup|tekenreeks|Nee |
|ip_opt|tekenreeks|Nee |
|timestamp_opt|tekenreeks|Nee |
|member_rating|geheel getal|Nee |
|last_changed|tekenreeks|Nee |
|taal|tekenreeks|Nee |
|VIP|Booleaanse waarde|Nee |
|email_client|tekenreeks|Nee |
|locatie|niet gedefinieerd|Nee |
|last_note|niet gedefinieerd|Nee |
|list_id|tekenreeks|Nee |
|_links|matrix|Nee |


## <a name="next-steps"></a>Volgende stappen
[Een logica-app maken](../app-service-logic/app-service-logic-create-a-logic-app.md)
