<properties
    pageTitle="Werken met Azure AD-toepassingsproxy verbindingslijnen | Microsoft Azure"
    description="Wordt uitgelegd hoe u groepen van verbindingslijnen in Azure AD-toepassingsproxy maken en beheren."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Toepassingen op afzonderlijke netwerken en locaties verbindingslijn groepen met publiceren

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure klassieke portal](active-directory-application-proxy-connectors.md)


Connector groepen zijn handig om verschillende scenario's, waaronder:

- Sites met meerdere, onderling verbonden datacenters. In dit geval wilt u behouden zoveel verkeer binnen het datacenter mogelijk omdat cross-datacenter koppelingen meestal dure en traag zijn. U kunt implementeren verbindingslijnen in elke datacenter moet fungeren alleen de toepassingen die zich binnen het datacenter bevinden. Deze methode wordt geminimaliseerd cross-datacenter koppelingen en beschikt u over een volledig doorzichtig ervaring aan uw gebruikers.
- Het beheren van toepassingen op geïsoleerd netwerken die geen deel uitmaken van de belangrijkste bedrijfsnetwerk is geïnstalleerd. U kunt verbindingslijn groepen speciale verbindingslijnen installeren op geïsoleerd netwerken te isoleren ook toepassingen bij het netwerk.
- Connector groepen bieden voor toepassingen geïnstalleerd op IaaS voor cloudtoegang, een algemene service beveiligde toegang tot alle apps. Connector groepen niet extra afhankelijkheid op uw bedrijfsnetwerk maken of de app-ervaring fragment. Verbindingslijnen kunnen worden geïnstalleerd op elke cloud-datacenter en alleen de toepassingen die zich in dit netwerk bevinden fungeren. U kunt verschillende verbindingslijnen als u wilt bereiken beschikbaarheid installeren.
- Ondersteuning voor meerdere bos omgevingen waarin specifieke verbindingslijnen kunnen worden geïmplementeerd per bos en ingesteld op specifieke toepassingen fungeren.
- Connector groepen kunnen worden gebruikt in herstel sites ofwel failover wordt gedetecteerd of als back-up maken voor de hoofdsite.
- Connector groepen kunnen ook worden gebruikt voor meerdere bedrijven van een enkel tenant.

## <a name="prerequisite-create-your-connectors"></a>Let op: Uw verbindingslijnen maken
Om te kunnen uw verbindingslijnen groepeert, moet u hebt met zorg ervoor dat u hebt [meerdere verbindingslijnen geïnstalleerd](active-directory-application-proxy-enable.md)en dat u de naam ze en vervolgens groeperen. U moet ten slotte toewijzen aan specifieke apps.

## <a name="step-1-create-connector-groups"></a>Stap 1: Verbindingslijn groepen maken
U kunt zo veel verbindingslijn groepen als u wilt maken. Connector voor het maken van groepen wordt in de portal van Azure klassieke bereikt.

1. Selecteer de map en klikt u op **configureren**.  
    ![Toepassingsproxy, configureren schermafbeelding - Klik op de verbindingslijn groepen beheren](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)

2. Klik onder toepassingsproxy, klikt u op **Verbindingslijn groepen beheren** en een nieuwe verbindingslijn-groep maken door middel van een naam voor de groep.  
    ![Schermafbeelding verbindingslijn groepen in de toepassing proxy - naam nieuwe groep](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Stap 2: Verbindingslijnen toewijzen aan uw groepen
Als de verbindingslijn groepen worden gemaakt, verplaatst u de verbindingslijnen aan de juiste groep.

1. Klik onder **Toepassingsproxy**, klikt u op **Verbindingslijnen beheren**.
2. Selecteer de groep die u wilt gebruiken voor elke verbindingslijn die onder de **groep**. Het duurt maximaal 10 minuten actief in de groep nieuw de verbindingslijnen.  
    ![Schermafbeelding in de toepassing proxy-connectors - bepaalde groep in vervolgkeuzelijst](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Stap 3: Toepassingen toewijzen aan uw groepen verbindingslijn
De laatste stap is om in te stellen van elke toepassing aan de groep verbindingslijn die wordt deze dienen.

1. Klik in de Azure klassieke portal in uw adreslijst, selecteert u de toepassing die u wilt toewijzen aan de groep en klik op **configureren**.
2. Selecteer onder **verbindingslijn groep**, de groep die u wilt dat de toepassing te gebruiken. Deze wijziging wordt onmiddellijk toegepast.  
    ![Schermafbeelding verbindingslijn groep in de toepassing proxy - bepaalde groep in vervolgkeuzelijst](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)


## <a name="see-also"></a>Zie ook

- [Toepassingsproxy inschakelen](active-directory-application-proxy-enable.md)
- [Inschakelen voor eenmalige aanmelding](active-directory-application-proxy-sso-using-kcd.md)
- [Voorwaardelijke toegang inschakelen](active-directory-application-proxy-conditional-access.md)
- [Problemen die u met toepassingsproxy ondervindt](active-directory-application-proxy-troubleshoot.md)

Voor het laatste nieuws en updates, raadpleegt u de [toepassingsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)
