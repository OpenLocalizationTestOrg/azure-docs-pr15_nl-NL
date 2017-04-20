<properties
    pageTitle="Azure AD Connect meerdere domeinen"
    description="In dit document wordt beschreven instellen en configureren van meerdere domeinen van het hoogste niveau met O365 en Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Ondersteuning voor meerdere domein voor samenbrengen met Azure AD
De volgende documentatie bevat richtlijnen voor het gebruik van meerdere topniveaudomeinen en subdomeinen wanneer samenbrengen met Office 365 of Azure AD-domeinen.

## <a name="multiple-top-level-domain-support"></a>Ondersteuning voor meerdere topniveaudomeinen
Topniveaudomeinen met Azure AD samenbrengen meerdere, is enkele extra configuratie die is niet vereist wanneer samenbrengen met één topniveaudomeinen vereist.

Wanneer u een domein wordt gekoppeld aan Azure AD, worden de verschillende eigenschappen instellen in het domein in Azure wordt aangegeven.  Belangrijke één is IssuerUri.  Dit is een URI die wordt gebruikt door Azure AD identificeren van het domein dat het token is gekoppeld.  De URI hoeft niet omzetten in andere waarde dan dat het moet een geldige URI.  Standaard Azure AD Hiermee stelt u dit naar de waarde van de identificatie van de service Federatie in uw on-premises AD FS configuratie.

>[AZURE.NOTE]De id van de service Federatie is een URI die deze identificeert op een federation-service.  De federation-service is een exemplaar van AD FS die functies als de beveiliging token-service. 

Kunt u vew IssuerUri met behulp van de PowerShell-opdracht `Get-MsolDomainFederationSettings - DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Een probleem treedt op wanneer we wilt toevoegen van meer dan één topniveaudomeinen.  Bijvoorbeeld: Stel dat u setup-federatie tussen Azure AD hebt en uw on-premises omgeving.  Ik gebruik bmcontoso.com voor dit document.  Nu ik een tweede, op het hoogste niveau domein hebt toegevoegd bmfabrikam.com.

![Domeinen](./media/active-directory-multiple-domains/domains.png)

Als we probeert te converteren van onze bmfabrikam.com-domein om te worden gekoppeld, wordt een foutbericht.  De reden hiervoor is Azure AD heeft een beperking die niet zijn toegestaan met de eigenschap IssuerUri naar dezelfde waarde voor meer dan één domein.  
  

![Federatie fout](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain Parameter

Aan de tijdelijke oplossing dit, moeten we toevoegen van een andere IssuerUri die kan worden uitgevoerd met behulp van de `-SupportMultipleDomain` parameter.  Deze parameter wordt gebruikt met de volgende cmdlets uit:
    
- `New-MsolFederatedDomain`
- `Convert-MsolDomaintoFederated`
- `Update-MsolFederatedDomain`

Deze parameter wordt Azure AD de IssuerUri zodanig configureren dat is gebaseerd op de naam van het domein.  Dit is unieke in mappen in Azure AD.  Gebruikt de parameter, kunt de PowerShell-opdracht te voltooien.

![Federatie fout](./media/active-directory-multiple-domains/convert.png)

De instellingen van onze nieuwe bmfabrikam.com domein ziet u het volgende:

![Federatie fout](./media/active-directory-multiple-domains/settings.png)

Houd er rekening mee dat `-SupportMultipleDomain` wordt niet gewijzigd voor de andere eindpunten die nog steeds zodat deze verwijzen naar onze federation-service adfs.bmcontoso.com zijn geconfigureerd.

Een ander object die `-SupportMultipleDomain` bevat is dat dit zorgt ervoor dat de AD FS-systeem in tokens uitgegeven voor Azure AD de juiste uitgever-waarde bevat. Dit doet dit door te nemen van het domeingedeelte van de gebruikers UPN en dit instelt als het domein in de IssuerUri, dat wil zeggen https://{upn achtervoegsel} / adfs/services/vertrouwen. 

Dus tijdens de verificatie naar Azure AD of Office 365, het element IssuerUri in token van de gebruiker wordt gebruikt om te zoeken van het domein in Azure AD.  Als er een overeenkomst wordt gevonden dat de verificatie mislukt. 

Als een gebruiker zich UPN is bijvoorbeeld bsimon@bmcontoso.com, het element IssuerUri in de token AD FS-problemen wordt ingesteld op http://bmcontoso.com/adfs/services/trust. Dit komen overeen met de Azure AD-configuratie en verificatie worden uitgevoerd.

Hieronder ziet u de aangepaste claim regel die deze logica implementeert.

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type =   "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


>[AZURE.IMPORTANT]Pas de schakeloptie - SupportMultipleDomain gebruiken wanneer u probeert te nieuwe toevoegen of al converteren domeinen toegevoegd, moet u beschikken over setup uw federatieve vertrouwensrelatie ter ondersteuning van deze oorspronkelijk.  


## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Het bijwerken van de vertrouwensrelatie tussen AD FS en Azure AD
Als u de federatieve vertrouwensrelatie tussen AD FS en uw exemplaar van Azure AD niet hebt ingesteld, moet u mogelijk opnieuw maken deze vertrouwen.  Dit is omdat, wanneer het oorspronkelijk installatie zonder de `-SupportMultipleDomain` parameter, de IssuerUri is ingesteld met de standaardwaarde.  In de schermopname onder u kunt u dat de IssuerUri is ingesteld op https://adfs.bmcontoso.com/adfs/services/trust.

Dat het geval is nu, als er een nieuw domein in de portal Azure AD hebt toegevoegd en vervolgens probeert te converteren met behulp van `Convert-MsolDomaintoFederated -DomainName <your domain>`, we wordt het volgende foutbericht weergegeven.

![Federatie fout](./media/active-directory-multiple-domains/trust1.png)

Als u probeert toe te voegen de `-SupportMultipleDomain` veranderen, wordt de volgende foutmelding ontvangt:

![Federatie fout](./media/active-directory-multiple-domains/trust2.png)

Gewoon probeert uit te voeren `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` in het oorspronkelijke domein ook resulteert in een fout.

![Federatie fout](./media/active-directory-multiple-domains/trust3.png)

Gebruik de volgende stappen als u een extra op het hoogste niveau domein toe te voegen.  Als u al een domein hebt toegevoegd en geen gebruik de `-SupportMultipleDomain` parameter beginnen met de stappen voor het verwijderen en bijwerken van uw oorspronkelijke domein.  Als u een topniveaudomein niet hebt toegevoegd kunt nog u beginnen met de stappen voor het toevoegen van een domein met PowerShell van Azure AD Connect.

Gebruik de volgende stappen uit om te verwijderen van de Microsoft Online-vertrouwen en bijwerken van uw oorspronkelijke domein.

2.  Klik op de federatieserver AD FS openen **AD FS-Management.** 
2.  Aan de linkerkant uitbreiden **Vertrouwensrelaties** en **Vertrouwensrelaties van derden te vertrouwen**
3.  Aan de rechterkant de vermelding van de **Microsoft Office 365-identiteit Platform** te verwijderen.
![Microsoft Online verwijderen](./media/active-directory-multiple-domains/trust4.png)
1.  Voer de volgende handelingen uit op een computer waarop [Azure Active Directory-Module voor Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) is geïnstalleerd: `$cred=Get-Credential`.  
2.  Voer de gebruikersnaam en wachtwoord van een globale beheerder voor het Azure AD-domein dat u met zijn samenbrengen
2.  Voer in PowerShell`Connect-MsolService -Credential $cred`
4.  Voer in PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Dit is voor het oorspronkelijke domein.  Met behulp van de domeinen van de bovenstaande zou:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`


Gebruik van de volgende stappen om toe te voegen van de nieuwe topniveaudomeinen via PowerShell

1.  Voer de volgende handelingen uit op een computer waarop [Azure Active Directory-Module voor Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) is geïnstalleerd: `$cred=Get-Credential`.  
2.  Voer de gebruikersnaam en wachtwoord van een globale beheerder voor het Azure AD-domein dat u met zijn samenbrengen
2.  Voer in PowerShell`Connect-MsolService -Credential $cred`
3.  Voer in PowerShell`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Gebruik de volgende stappen uit de nieuwe op het hoogste niveau domein met Azure AD Connect toe te voegen.

1.  Azure AD Connect vanaf het bureaublad starten of het menu start
2.  Kies 'Een extra Azure AD-domein toevoegen' ![toevoegen een extra Azure AD-domein](./media/active-directory-multiple-domains/add1.png)
3.  Voer uw Azure AD en Active Directory-referenties
4.  Selecteer het tweede domein dat u wilt configureren voor Federatie.
![Een extra toevoegen Azure AD-domein](./media/active-directory-multiple-domains/add2.png)
5.  Klik op installeren


### <a name="verify-the-new-top-level-domain"></a>Controleer het nieuwe domein op het hoogste niveau
Met behulp van de PowerShell-opdracht `Get-MsolDomainFederationSettings - DomainName <your domain>`kunt u de bijgewerkte IssuerUri weergeven.  De volgende schermafbeelding ziet u de federatie instellingen zijn bijgewerkt op onze oorspronkelijke domein http://bmcontoso.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

En de IssuerUri in onze nieuwe domein is ingesteld op https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)


##<a name="support-for-sub-domains"></a>Ondersteuning voor subdomeinen
Wanneer u een subdomein vanwege de domeinen van de manier Azure AD verwerkt toevoegt, worden de instellingen van de bovenliggende overgenomen.  Dit betekent dat de IssuerUri moet overeenkomen met de bovenliggende items.

Kan dus zeggen bijvoorbeeld dat ik heb bmcontoso.com en voeg corp.bmcontoso.com toe.  Dit betekent dat de IssuerUri voor een gebruiker van corp.bmcontoso.com moeten worden **http://bmcontoso.com/adfs/services/trust.**  Maar de standaard regel geïmplementeerd boven voor Azure AD, genereert een token met een uitgever als **http://corp.bmcontoso.com/adfs/services/trust.** waarde waaraan komen niet overeen met van het domein vereist en verificatie mislukt.

### <a name="how-to-enable-support-for-sub-domains"></a>Ondersteuning voor subdomeinen inschakelen
Om dit probleem oplossen door de AD FS moet gebruikmakende partij vertrouwen voor Microsoft Online worden bijgewerkt.  Hiervoor moet u een regel aangepaste claim configureren zodat deze uit een subdomeinen uit van de gebruiker UPN-achtervoegsel Oprollen wanneer het bouwen van de aangepaste uitgever-waarde. 

De volgende claim wordt als volgt:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));

Gebruik de volgende stappen uit om toe te voegen een aangepaste claim ter ondersteuning van subdomeinen.

1.  Open AD FS-Management.
2.  Klik met de rechtermuisknop op de optie Microsoft Online RP en kiest u bewerken claimen regels
3.  Selecteer de regel van het derde claimen en vervangen ![claimen bewerken](./media/active-directory-multiple-domains/sub1.png)
4.  De huidige claimen vervangen:
    
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
        
    met
    
        `c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));`
    
![Claim vervangen](./media/active-directory-multiple-domains/sub2.png)
5.  Klik op Ok.  Klik op toepassen.  Klik op Ok.  Sluit de AD FS-Management.

