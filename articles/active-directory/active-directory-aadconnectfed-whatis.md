<properties
    pageTitle="Azure AD Connect en Federatie | Microsoft Azure"
    description="Deze pagina is de centrale locatie voor alle documentatie met betrekking tot AD FS-bewerkingen met Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect en Federatie

Azure AD Connect kunt u federatie configureren met de lokale AD FS en Azure AD. Met Federatie aanmeldingsgegevens kunt u gebruikers aan te melden met Azure AD op basis van services met hun wachtwoorden on-premises implementatie en, terwijl u op het bedrijfsnetwerk bevinden, zonder te hoeven hun wachtwoord opnieuw invoeren. De optie Federatie met AD FS kunt u een nieuwe implementeren of een bestaande AD FS opgeven in Windows Server 2012 R2-farm.

Dit onderwerp is bedoeld voor de start voor meer informatie over Federatie gerelateerde functies voor Azure AD Connect en lijsten koppelingen naar andere onderwerpen die zijn gerelateerd aan deze. Voor koppelingen naar Azure AD Connect, raadpleegt u uw on-premises implementatie identiteiten integreren met Azure Active Directory.

## <a name="azure-ad-connect---federation-topics"></a>Azure AD Connect - Federatie onderwerpen

| Onderwerp | Wat deze bedekt en wanneer te lezen |
|:------|:-----------|
| **Azure AD Connect aanmeldingsproblemen gebruikersopties** ||
| [Informatie over aanmelden gebruikersopties](active-directory-aadconnect-user-signin.md) | Beschrijving van verschillende aanmeldingsproblemen gebruikersopties en de invloed gebruikerservaring voor Azure aanmelden |
| **AD FS-installatie met Azure AD Connect**||
| [Minimumvereisten](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | Minimumvereisten voor een succesvolle AD FS-installatie via Azure AD Connect|
| [AD FS-farm configureren](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | Het installeren van een nieuwe AD FS-farm met Azure AD Connect |
| **Wijzigen van de AD FS-configuratie** | |
| [De optie herstellen](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Het herstellen van huidige vertrouwensrelatie tussen on-premises implementatie AD FS- en Office 365 / Azure |
| [Een nieuwe AD FS-server toevoegt](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | Gegevensniveaus uitvouwen AD FS-farm met extra AD FS-server bericht eerste installatie |
| [Een nieuwe WAP van AD FS-server toevoegt](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | Gegevensniveaus uitvouwen AD FS-farm met extra WAP server bericht eerste installatie |
| [Een nieuwe gefedereerd domein toevoegen](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Een ander domein toevoegen om te worden gekoppeld aan Azure AD |
|**Bericht installatietaken**||
| [Aangepaste bedrijfslogo toevoegen / afbeelding](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| De aanmeldervaring wijzigen door het opgeven van het aangepaste logo die wordt weergegeven op de aanmeldingspagina van AD FS |
| [Aanmeldingsproblemen beschrijving toevoegen](active-directory-aadconnect-federation-management.md#add-sign-in-description) | Beschrijving aanmelden op de aanmeldingspagina van AD FS wijzigen | 
| [Het wijzigen van AD FS claimen regels](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Wijzigen / claimen regels toevoegen in AD FS overeenkomt met de configuratie van de Azure AD Connect-synchronisatie |


## <a name="additional-resources"></a>Aanvullende informatie

* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
* [AD FS-implementatie in Azure wordt aangegeven](active-directory-aadconnect-azure-adfs.md)
* [Beschikbaarheid cross-geografische AD FS-implementatie in Azure wordt aangegeven met Azure verkeer Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


