<properties
   pageTitle="CSV-indeling voor Azure Active Directory B2B samenwerking preview | Microsoft Azure"
   description="Azure Active Directory-B2B ondersteunt uw relaties intern doordat zakenpartners selectief toegang krijgen tot uw zakelijke toepassingen"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Voorbeeld van de samenwerking Azure AD B2B: CSV-indeling

De preview-versie van Azure AD B2B samenwerking vereist een CSV-bestand partner gebruikersgegevens om te worden geüpload via de portal Azure AD opgeven. Het CSV-bestand moet bevatten de vereiste labels onderstaande en optionele velden indien nodig. Wijzig het CSV-voorbeeldbestand (hieronder) zonder te wijzigen van de spelling van de etiketten in de eerste rij.

>[AZURE.NOTE] De eerste rij met labels (zoals E-mail, weergavenaam, enzovoort) is vereist voor het CSV-bestand is verdeeld. De spelling moet overeenkomen met de velden hieronder. Deze labels Geef aan dat de inhoud eronder. Voor optionele velden die niet meer nodig hebt, kunnen de bijbehorende labels worden verwijderd uit het CSV-bestand. De hele kolom kan leeg zijn.

## <a name="required-fields-br"></a>Vereiste velden: <br/>
**E-mail:** E-mailadres van de uitgenodigde gebruiker. <br/>
**Weergavenaam:** De weergavenaam voor uitgenodigde gebruiker (meestal eerste en laatste naam).<br/>


## <a name="optional-fields-br"></a>Optionele velden: <br/>

**InvitationText:** Pas e-mailuitnodiging na app huisstijl en voor de koppeling aflossingsprijs.<br/>
**InvitedToApplications:** Bewerkingswaarde tot bedrijfstoepassingen gebruikers toe te wijzen. Bewerkingswaarde zijn opvraagbare in PowerShell door te bellen`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** Extensies voor groepen gebruiker toevoegen. Extensies zijn opvraagbare in PowerShell door te bellen`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:** De URL voor een uitgenodigde gebruiker rechtstreekse na acceptatie uitnodigen. Dit moet de URL van een specifiek voor het bedrijf (zoals [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). Als dit optioneel veld niet is opgegeven, wordt de uitgenodigde gebruiker doorgestuurd naar het deelvenster aan de Access-App waar ze naar de door u gekozen bedrijfs-apps navigeren kunnen. De App Access Configuratiescherm-URL van het formulier is `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: E-mailadres kunt u via e-mail verzonden uitnodiging kopiëren. Als het veld CcEmailAddress wordt gebruikt, kan deze uitnodiging niet kan worden gebruikt voor gebruiker e-mail geverifieerd of de aanmaakdatum van de tenant.<br/>
**Taal:** De taal voor de uitnodiging aflossingsprijs ervaring voor e-mail en, met 'en' (Engelstalig) als het standaardapparaat wanneer niet opgegeven. De andere 10 ondersteunde taal codes zijn:<br/>
1. de: Duits<br/>
2. ES: Spaans<br/>
3. Frans: Frans<br/>
4. Deze: Italiaans<br/>
5. Ja: Japans<br/>
6. Ko: Koreaans<br/>
7. pt-BR: Portugees (Brazilië)<br/>
8. RU: Russisch<br/>
9. zh-HANS: vereenvoudigd Chinees<br/>
10. zh-HANT: Chinees (Traditioneel)<br/>

## <a name="sample-csv-file"></a>CSV-voorbeeldbestand
Hier volgt een voorbeeld CSV die u kunt wijzigen.

>[AZURE.NOTE] Kopieer en plak deze in Kladblok en deze opslaan onder een bestandsextensie. Deze vervolgens bewerken in Excel. Deze worden gestructureerd in een tabel met labels in de eerste rij.

> Optionele velden toevoegen in Excel door te geven van het etiket en vullen met de kolom eronder.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Verwante artikelen
Onze andere artikelen op Azure AD B2B samenwerking bladeren

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Werkwijze](active-directory-b2b-how-it-works.md)
- [Gedetailleerd overzicht](active-directory-b2b-detailed-walkthrough.md)
- [Externe gebruiker token opmaken](active-directory-b2b-references-external-user-token-format.md)
- [Wijzigingen van de externe gebruiker object kenmerken](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Huidige preview beperkingen](active-directory-b2b-current-preview-limitations.md)
- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
