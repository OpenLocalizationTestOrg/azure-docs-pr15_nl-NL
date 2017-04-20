<properties
    pageTitle="Hybride identiteit: Vergelijking hulpprogramma's voor Directory-integratie | Microsoft Azure"
    description="Dit is de pagina vindt u een volledig tabel die verschilt van de verschillende hulpprogramma´s voor adreslijstintegratie die kunnen worden gebruikt voor directory-integratie."
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
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Hybride identiteit directory-integratie hulpprogramma's voor vergelijking

De integratie van directory's hebben de jaren gegroeid en die is voortgekomen.  Dit document is te voorzien van een gecombineerde weergave van deze hulpprogramma's en een vergelijking van de functies die beschikbaar in elk zijn.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Azure AD Connect bevat de onderdelen en functionaliteit eerder uitgebracht als Dirsync en AAD-synchronisatie. Deze hulpprogramma's zijn niet meer afzonderlijk wordt gepubliceerd en alle toekomstige verbeteringen worden opgenomen in updates voor Azure AD Connect zodat u altijd waar u de meest recente functionaliteit weet.
>
>DirSync en Azure AD-synchronisatie zijn afgeschaft. Meer informatie vindt u [hier](active-directory-aadconnect-dirsync-deprecated.md)in.


Gebruik de volgende sleutel voor elk van de tabellen.

● = Nu beschikbaar  
Frans = toekomstige Release  
PP = Public Preview  

## <a name="on-premises-to-cloud-synchronization"></a>On-Premises naar Cloud synchronisatie

| Functie  | Azure Active Directory verbinding maken  | Azure Active Directory-Synchronisatieservices (AAD-synchronisatie) | Hulpprogramma Azure Active Directory-synchronisatie (DirSync)| Forefront identiteit Manager 2010 R2 (FIM) |Microsoft identiteitsbeheer 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Verbinding maken met één lokale AD bos | ● | ● | ● | ● |● |
| Verbinding maken met meerdere lokale AD forests |●  | ● |  | ● |● |
| Verbinding maken met meerdere lokale Exchange organisaties | ● |  |  |  | |
| Verbinding maken met één on-premises implementatie LDAP-diagram | FRANS |  |  | ● |● |
| Verbinding maken met meerdere on-premises implementatie LDAP-mappen |FRANS  |  |  | ● |● |
| Verbinding maken met lokale AD en on-premises LDAP-mappen |FRANS  |  |  | ● |● |
| Verbinding maken met aangepaste systemen (dat wil zeggen SQL, Oracle, MySQL, enz.) | FRANS |  |  | ● |● |
| Synchroniseren van de gedefinieerde Klantkenmerken (directory extensions) | ● |  |  |  |  |
|Verbinding maken met on-premises implementatie HR (dat wil zeggen SAP, Oracle eBusiness, PeopleSoft)| FRANS |  |  | ● |● |
|Regels voor FIM-synchronisatie en verbindingslijnen ondersteunt voor het inrichten van tot on-premises implementatie-systemen.|  |  |  | ● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Wolk On-Premises-synchronisatie

| Functie  | Azure Active Directory verbinding maken  | Azure Active Directory-Synchronisatieservices | Hulpprogramma Azure Active Directory-synchronisatie (DirSync) | Forefront identiteit Manager 2010 R2 (FIM) |Microsoft identiteitsbeheer 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Write-backs van apparaten | ● |  | ● |  ||
| Kenmerk write-backs (voor hybride implementatie van Exchange) | ● | ● | ● | ● |● |
| Write-backs van gebruikers en groepen objecten |  ●|  | |  ||
| Write-backs wachtwoorden (van selfservice-wachtwoordherstel (SSPR) en wachtwoord wijzigen) |  ● | ● |  |  ||



## <a name="authentication-feature-support"></a>Ondersteuning van functies voor verificatie

| Functie  | Azure Active Directory verbinding maken | Azure Active Directory-Synchronisatieservices | Hulpprogramma Azure Active Directory-synchronisatie (DirSync) | Forefront identiteit Manager 2010 R2 (FIM) |Microsoft identiteitsbeheer 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Wachtwoordsynchronisatie voor één lokale AD bos | ● | ● | ● |  ||
| Wachtwoordsynchronisatie voor meerdere lokale AD forests |  ●| ● |  |  ||
| Eenmalige aanmelding met Federatie | ● | ● | ● | ● |● |
| Write-backs wachtwoorden (van SSPR en het wachtwoord wijzigen) |●  | ● |  |  ||



## <a name="set-up-and-installation"></a>Installatie

| Functie  | Azure Active Directory verbinding maken  | Azure Active Directory-Synchronisatieservices | Hulpprogramma Azure Active Directory-synchronisatie (DirSync) | Microsoft identiteitsbeheer 2016 (MIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Installatie ondersteunt op een domeincontroller | ● | ● | ● |  |
| Installatie met SQL Express ondersteunt | ● | ● | ● |  |
| Eenvoudig upgrade van DirSync |● |  |  |  |
| Lokalisatie van Admin UX op Windows Server talen | ● | ● | ● |  |
|Lokalisatie van eindgebruikers UX op Windows Server talen| |  |  |● |
| Ondersteuning voor Windows Server 2008 en Windows Server 2008 R2 | ● voor synchronisatie, niet voor Federatie| ● | ●  | ● |
| Ondersteuning voor Windows Server 2012 en Windows Server 2012 R2 | ● | ● | ● | ● |

## <a name="filtering-and-configuration"></a>Filteren en configuratie

Functie  | Azure Active Directory verbinding maken | Azure Active Directory-Synchronisatieservices | Hulpprogramma Azure Active Directory-synchronisatie (DirSync) | Forefront identiteit Manager 2010 R2 (FIM)| Microsoft identiteitsbeheer 2016 (MIM)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
Filteren op domeinen en organisatie-eenheden | ● | ● | ● | ●  | ●
Filteren op objecten kenmerkwaarden | ● | ● | ● | ●| ●
Toestaan dat minimale set kenmerken worden gesynchroniseerd (MinSync) | ● | ● |  ||
Verschillende sjablonen voor het kenmerk loopt toestaan |●  | ● |  ||
Toestaan dat kenmerken van AD door naar Azure AD verwijderen | ● | ● |  |  |
Geavanceerde aanpassingen voor kenmerk overdrachten toestaan | ● | ● |  | ●  | ●

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
