<properties
   pageTitle="Azure automatisering beveiliging | Microsoft Azure"
   description="Dit artikel bevat een overzicht van automatisering beveiligings- en andere verificatiemethoden die beschikbaar zijn voor automatisering-Accounts in Azure automatisering."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="automatisering beveiliging en beveiligde automatisering" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/29/2016"
   ms.author="magoedte" />

# <a name="azure-automation-security"></a>Azure automatisering-beveiliging
Azure automatisering kunt u taken ten opzichte van resources in Azure wordt aangegeven, on-premises en met andere cloud-providers zoals Amazon Web Services (AWS) automatiseren.  In de volgorde voor een runbook de vereiste acties wilt uitvoeren, moet deze machtigingen veilig toegang krijgen tot de resources met de minimale rechten vereist binnen het abonnement hebben.  
In dit artikel wordt uitgelegd hoe de verschillende verificatie-scenario's die worden ondersteund door Azure automatisering en wordt uitgelegd hoe u aan de slag op basis van de omgeving of omgevingen die u nodig hebt om te beheren.  

## <a name="automation-account-overview"></a>Automatisering Accountoverzicht
Wanneer u Azure automatisering voor het eerst start, moet u ten minste één automatisering-account maken. Automatisering accounts kunnen u uw resources automatisering (runbooks, activa, configuraties) isoleren uit de bronnen die zich bevinden in andere automatisering-accounts. U kunt automatisering accounts kunt scheiden van resources in afzonderlijke logische omgevingen. U kunt bijvoorbeeld één account gebruiken voor de ontwikkeling, een andere voor productie en een andere voor uw on-premises omgeving.  Een account Azure automatisering verschilt van uw Microsoft-account of de accounts van uw Azure-abonnement hebt gemaakt.

De bronnen automatisering voor elk account automatisering zijn gekoppeld aan één Azure regio, maar automatisering accounts resources in een regio kunnen beheren. De belangrijkste reden om Automation-accounts maken in verschillende regio's zou zijn als u beleid waarvoor u de gegevens en bronnen die u wilt worden geïsoleerd voor een bepaalde regio.

>[AZURE.NOTE]Automatisering accounts en de resources die ze bevatten die zijn gemaakt in de portal van Azure, kunnen niet worden gebruikt in de portal van Azure klassieke. Als u deze accounts of hun resources met Windows PowerShell beheren wilt, moet u de resourcemanager Azure-modules.

Alle taken die u uitvoert ten opzichte van resources Azure resourcemanager en de Azure cmdlets gebruiken in Azure automatisering moet worden geverifieerd bij Azure met Azure Active Directory organisatie-id referentie gebaseerde verificatie.  Certificaatverificatie was de oorspronkelijke verificatiemethode met de modus voor beheer van Azure-Service, maar deze was ingewikkeld Setup.  Geverifieerd bij Azure met Azure AD-gebruiker is geïntroduceerd terug in 2014 niet alleen vereenvoudigen het proces voor het configureren van een verificatieaccount, maar ook ondersteuning voor niet-interactief worden geverifieerd bij Azure met één gebruikersaccount die met zowel Azure resourcemanager en klassieke resources gewerkt.   

Momenteel als u een nieuw account voor automatisering in de portal van Azure maakt, wordt automatisch gemaakt:

-  Als de account die Hiermee maakt u een nieuwe service principal in Azure Active Directory, een certificaat, en wordt toegewezen het Inzender Rolgebaseerd toegangsbeheer RBAC (), die worden gebruikt voor het beheren van resourcemanager resources met runbooks worden uitgevoerd.
-  Klassieke uitvoeren als account door te uploaden van een certificaat management dat wordt gebruikt voor het beheer van Azure-Service of klassieke resources met runbooks beheren.  

Rolgebaseerd toegangsbeheer is beschikbaar met Azure resourcemanager toestaan toegestane acties in een Azure AD-gebruikersaccount en uitvoeren als account en de hoofdsom van die service wordt geverifieerd.  Lees [Rolgebaseerd toegangsbeheer in Azure automatisering artikel](../automation/automation-role-based-access-control.md) voor meer informatie om u te helpen met het ontwikkelen van uw model voor het beheren van automatisering machtigingen.  

Runbooks een hybride Runbook werknemer in uw datacenter of tegen computing services in AWS waarop de dezelfde methode dat meestal wordt gebruikt voor het verifiëren van runbooks naar Azure bronnen niet gebruiken.  Dit is omdat deze resources worden uitgevoerd buiten Azure en daarom u hun eigen beveiligingsreferenties die is gedefinieerd in automatisering moet worden geverifieerd bij bronnen die ze lokale toegang tot.  

## <a name="authentication-methods"></a>Methoden voor gebruikersverificatie

De volgende tabel worden de verschillende verificatiemethoden voor elke omgeving ondersteund door Azure automatisering en het artikel wordt beschreven hoe u verificatie in voor uw runbooks instellen.

Methode  |  Omgeving  | Artikel
----------|----------|----------
Azure AD-gebruikersaccount | Azure resourcemanager en Azure servicebeheer | [Runbooks verifiëren met Azure AD-gebruikersaccount](../automation/automation-sec-configure-aduser-account.md)
Als de Account Azure uitvoeren | Azure resourcemanager | [Verificatie Runbooks met Azure uitvoeren als account](../automation/automation-sec-configure-azure-runas-account.md)
Als de Account Azure klassieke uitvoeren | Azure servicebeheer | [Runbooks verifiëren met Azure uitvoeren als account](../automation/automation-sec-configure-azure-runas-account.md)
Windows-verificatie | On-Premises Datacenter | [Runbooks verifiëren voor hybride Runbook werknemers](../automation/automation-hybrid-runbook-worker.md)
AWS referenties | Amazon-webservices | [Verifiëren Runbooks met Amazon-webservices (AWS)](../automation/automation-sec-configure-aws-account.md)



