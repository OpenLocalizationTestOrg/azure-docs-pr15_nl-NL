<properties
    pageTitle="Azure AD SAML Protocol verwijzing | Microsoft Azure"
    description="Dit artikel bevat een overzicht van de profielen eenmalige aanmelding en één Sign-Out SAML in Azure Active Directory."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Het gebruik van de SAML-protocol met Azure Active Directory

Gebruik de SAML 2.0-protocol van de Azure Active Directory (Azure AD) naar het mogelijk dat toepassingen een functionaliteit voor eenmalige aanmelding aan hun gebruikers te verstrekken. De [Eenmalige aanmelding](active-directory-single-sign-on-protocol-reference.md) en [één afmelden](active-directory-single-sign-out-protocol-reference.md) SAML profielen van Azure AD wordt uitgelegd hoe SAML bevestigingen, protocollen en bindingen worden gebruikt in de identiteit provider-service.

Op SAML-Protocol is vereist voor de identiteitsprovider (Azure AD) en de serviceprovider (de toepassing) om informatie over zichzelf te wisselen.

Wanneer een toepassing is geregistreerd met Azure Active Directory, registreert de app-ontwikkelaar gegevens die betrekking hebben op Federatie met Azure AD. Dit geldt ook voor de **URI omleiden** en **Metagegevens URI** van de toepassing.

Azure AD gebruikt de **Metagegevens URI** van de cloudservice om op te halen, de bijbehorende sleutel en het afmelden URI van de cloudservice. Als de toepassing biedt geen ondersteuning voor een metagegevens URI, de ontwikkelaar moet contact op met Microsoft ondersteuning bieden van de URI Meld u af en ondertekening toets.

Azure Active Directory beschrijft tenant / regiospecifieke en algemene (tenant ongeacht) eenmalige aanmelding en één afmelden eindpunten. Deze URL's vertegenwoordigen gebruikt locaties--zijn ze niet alleen een identificaties--, zodat u kunt gaan naar het eindpunt te lezen van de metagegevens.

 - Het eindpunt van de Tenant / regiospecifieke bevindt zich op `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  De <TenantDomainName> tijdelijke aanduiding voor een geregistreerde domeinnaam of TenantID GUID van een Azure AD-tenant vertegenwoordigt. De metagegevens Federatie van de tenant contoso.com is bijvoorbeeld op: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- Het eindpunt van de Tenant-onafhankelijke bevindt zich op `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. In dit eindpuntadres **algemene** wordt weergegeven, in plaats van een tenant-domeinnaam of -ID.

Zie voor informatie over de Federatie metagegevens-documenten die Azure AD publiceert, [Federatie metagegevens](active-directory-federation-metadata.md).
