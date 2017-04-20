<properties
    pageTitle="Synchronisatie van Azure AD Connect: begrijpen en aanpassen van synchronisatie | Microsoft Azure"
    description="Dit artikel wordt uitgelegd hoe Azure AD Connect synchroniseren werkt en hoe om aan te passen."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Synchronisatie van Azure AD Connect: begrijpen en synchronisatie aanpassen
De Azure Active Directory verbinden met Synchronisatieservices (Azure AD Connect-synchronisatie) is een belangrijke onderdeel van Azure AD Connect. Dit zorgt voor alle bewerkingen die zijn gerelateerd aan de identiteitsgegevens tussen uw on-premises omgeving en Azure AD synchroniseren. Azure AD Connect-synchronisatie is de opvolgende taak van DirSync, Azure AD-synchronisatie en Forefront identiteitsbeheer met Azure Active Directory Connector geconfigureerd.

In dit onderwerp is van de startpagina voor **synchronisatie van Azure AD Connect** (ook wel **synchronisatie-engine**genoemd) en vindt u koppelingen naar andere onderwerpen die zijn gerelateerd aan deze. Zie de [integratie van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md)voor koppelingen naar Azure AD Connect.

De synchronisatieservice bestaat uit twee onderdelen, het onderdeel van de **synchronisatie van Azure AD Connect** on-premises implementatie en de service-kant in Azure AD **Azure AD Connect synchronisatieservice**genoemd. De service is algemene voor DirSync, Azure AD-synchronisatie en Azure AD Connect.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Connect synchronisatie onderwerpen

Onderwerp | Wat deze bedekt en wanneer te lezen
----- | -----
**Grondbeginselen van Azure AD Connect-synchronisatie** |
[Inzicht in de architectuur](active-directory-aadconnectsync-understanding-architecture.md) | Voor mensen die nog niet eerder met de synchronisatie-engine en wilt u meer informatie over de architectuur en de voorwaarden die worden gebruikt.
[Technische concepten](active-directory-aadconnectsync-technical-concepts.md) | Een korte versie van het onderwerp van de architectuur en kort wordt uitgelegd dat de voorwaarden die worden gebruikt.
[Topologieën voor Azure AD verbinding maken](active-directory-aadconnect-topologies.md) | Beschrijft de verschillende topologieën en scenario's voor die de synchronisatie-engine ondersteunt.
**Aangepaste configuratie** |
[De installatiewizard opnieuw uit te voeren](active-directory-aadconnectsync-installation-wizard.md) | Dit artikel wordt uitgelegd wat u opties er beschikbaar wanneer u de installatiewizard Azure AD Connect opnieuw uitvoeren.
[Informatie over declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| Beschrijving van de configuratiemodel ' declaratieve provisioning ' genoemd.
[Lidmaatschap declaratieve inrichten expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | De syntaxis voor de expressietaal die wordt gebruikt in de inrichting van declaratieve wordt beschreven.
[Informatie over de standaardconfiguratie](active-directory-aadconnectsync-understanding-default-configuration.md)| Beschrijving van de kant-en-klare regels en de standaardconfiguratie. Ook wordt beschreven hoe de regels samenwerken voor de kant-en-klare scenario's om te werken.
[Wat gebruikers en contactpersonen](active-directory-aadconnectsync-understanding-users-and-contacts.md) | Klik op het vorige onderwerp blijft en wordt beschreven hoe de configuratie voor gebruikers en contactpersonen werkt samen, met name in een omgeving met meerdere bos.
[Hoe u een wijziging in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md) | Begeleidt u bij het maken van een algemene configuratie omzetten in kenmerk loopt.
[Aanbevolen procedures voor het wijzigen van de standaardconfiguratie](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | Beperkingen voor de ondersteuning en voor wijzigingen aanbrengt in de configuratie kant-en-klare.
[Filtering configureren](active-directory-aadconnectsync-configure-filtering.md) | Beschrijft de verschillende opties voor de realisatie beperken welke objecten worden gesynchroniseerd met Azure AD en stapsgewijze het configureren van deze opties.
**Functies en scenario 's** |
[Niet per ongeluk verwijderen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | Beschrijving van de functie *niet per ongeluk verwijderen* en hoe u configureert u deze.
[Planner](active-directory-aadconnectsync-feature-scheduler.md) | Beschrijving van de ingebouwde scheduler, die is, synchroniseren, gegevens importeren en exporteren.
[Wachtwoordsynchronisatie implementeren](active-directory-aadconnectsync-implement-password-synchronization.md) | Wordt beschreven hoe Wachtwoordsynchronisatie werkt, hoe willen implementeren, werken en problemen met.
[Apparaat write-backs](active-directory-aadconnect-feature-device-writeback.md) | Beschrijving van de werking van apparaat write-backs in Azure AD Connect.
[Directory-extensies](active-directory-aadconnectsync-feature-directory-extensions.md) | Beschreven hoe u het schema Azure AD met uw eigen aangepaste kenmerken uitbreiden.
**Synchronisatieservice** |
[Azure AD Connect synchronisatie service-functies](active-directory-aadconnectsyncservice-features.md) | Beschrijving van de kant van de service synchroniseren en hoe u synchronisatie-instellingen wijzigt in Azure AD.
[Dubbel kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | Wordt beschreven hoe u inschakelt en **userPrincipalName** en **proxyAddresses** -kenmerk dubbele waarden tolerantie gebruiken.
**Bewerkingen en UI** |
[Synchronisatie-servicebeheer](active-directory-aadconnectsync-service-manager-ui.md) | Beschrijving van de synchronisatie-servicebeheer UI, inclusief [bewerkingen](active-directory-aadconnectsync-service-manager-ui-operations.md), [verbindingslijnen](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)en tabbladen [Metaverse zoeken](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) .
[Operationele taken en overwegingen](active-directory-aadconnectsync-operations.md) | Operationele bezwaren, zoals herstel beschreven.
**Procedures...** |
[De Azure AD-account opnieuw instellen](active-directory-aadconnectsync-howto-azureadaccount.md) | Hoe de referenties van de serviceaccount waarmee verbinding van Azure AD Connect synchroniseren met Azure AD opnieuw in te stellen.
**Meer informatie en verwijzingen** |
[Poorten](active-directory-aadconnect-ports.md) | Welke poorten die u wilt openen tussen de synchronisatie-engine en uw on-premises implementatie mappen en Azure AD-lijsten.
[Kenmerken die zijn gesynchroniseerd met Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) | Hier worden alle kenmerken worden gesynchroniseerd tussen on-premises implementatie AD en Azure AD.
[Overzicht van functies](active-directory-aadconnectsync-functions-reference.md) | Bevat alle functies beschikbaar in de inrichting van declaratieve.

## <a name="additional-resources"></a>Aanvullende informatie

* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
