<properties
   pageTitle="Logica Apps on-premises implementatie gateway gegevensverbinding | Microsoft Azure"
   description="Informatie over het maken van een verbinding tot de on-premises implementatie data gateway via een logica-app."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Verbinding maken met de on-premises implementatie data gateway voor logica Apps

Ondersteunde logica apps verbindingslijnen kunnen u de verbinding met access on-premises gegevens via de on-premises implementatie data gateway te configureren.  De volgende stappen wordt u begeleid bij het installeren en configureren van de on-premises implementatie data gateway voor gebruik met een logica-app.

## <a name="prerequisites"></a>Vereisten voor

* Moeten worden gebruikt een werk of school e-mailadres in Azure naar de on-premises implementatie data gateway koppelen aan uw account (Azure Active Directory gebaseerd account)
    * Als u een Microsoft-Account gebruikt (bijvoorbeeld @outlook.com, @live.com) kunt u uw Azure-account te maken van een werk of school e-mailadres aan de hand van [de volgende stappen proberen](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal)

> [AZURE.WARNING] Er is momenteel een beperking wilt invoeren dat on-premises implementatie gateway installeren duurt alleen wanneer u een account dat is geregistreerd bij Power BI gebruikt.  Registreer ondertussen een account met 'Power BI gratis' de installatie voltooid.

* Moet de on-premises implementatie gegevens gateway [op een lokale computer zijn geïnstalleerd](app-service-logic-gateway-install.md).
* Gateway moet niet hebt gedaan door een andere Azure on-premises implementatie data gateway ([claimen gebeurt er met het maken van stap 2 onder](#2-create-an-azure-on-premises-data-gateway-resource)): een installatie kan alleen worden gekoppeld aan één gateway resource.

## <a name="installing-and-configuring-the-connection"></a>Installeren en configureren van de verbinding

### <a name="1-install-the-on-premises-data-gateway"></a>1. de on-premises implementatie data gateway installeren

[In dit artikel](app-service-logic-gateway-install.md)vindt u informatie over het installeren van de on-premises implementatie data gateway.  De gateway moet zijn geïnstalleerd op een lokale computer voordat u kunt doorgaan met de volgende stappen uitvoert.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. een resource Azure on-premises implementatie gegevens gateway maken

Zodra geïnstalleerd, moet u uw abonnement op Azure koppelen aan de on-premises implementatie data gateway.

1. Meld u aan bij Azure via de dezelfde werk of school-e-mailadres dat is gebruikt tijdens de installatie van de gateway
1. Klik op de knop resource **Nieuw**
1. Zoek en selecteer de **On-premises gegevensgateway**
1. Vul de gegevens om de gateway koppelen aan uw account - Selecteer de gewenste **Naam van de installatie**

    ![On-Premises Gateway gegevensverbinding][1]
1. Klik op de knop **maken** als u wilt maken van de resource

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3. een logica app-verbinding maken in de ontwerpfunctie

Nu uw Azure-abonnement is gekoppeld aan een exemplaar van de gateway van de gegevens on-premises implementatie, kunt u een verbinding met deze uit binnen een logica-app maken.

1. Open een logica-app en kies een verbindingslijn die ondersteuning biedt voor on-premises implementatie connectivity (schrijven van dit, SQL Server)
1. Schakel het selectievakje in voor **Verbinden via on-premises implementatie gegevensgateway**

    ![Logica App Designer Gateway maken][2]
1. Selecteer de **Gateway** bij naar verbinden en eventuele andere verbindingsinformatie vereist te voltooien
1. Klik op **maken** om de verbinding te maken

De verbinding moet nu worden geconfigureerd voor gebruik in uw app logica.  

## <a name="next-steps"></a>Volgende stappen
- [Algemene voorbeelden en scenario's voor logica-apps](app-service-logic-examples-and-scenarios.md)
- [Onderdelen van de Enterprise-integratie](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG