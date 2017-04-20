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


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups---public-preview"></a>Toepassingen op afzonderlijke netwerken en locaties verbindingslijn groepen - Public Preview met publiceren

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure klassieke portal](active-directory-application-proxy-connectors.md)


Connector groepen zijn handig om verschillende scenario's, waaronder:

- Sites met meerdere, onderling verbonden datacenters. In dit geval wilt u behouden zoveel verkeer binnen het datacenter mogelijk omdat cross-datacenter koppelingen dure en traag zijn. U kunt implementeren verbindingslijnen in elke datacenter moet fungeren alleen de toepassingen die zich binnen het datacenter bevinden. Deze methode wordt geminimaliseerd cross-datacenter koppelingen en beschikt u over een volledig doorzichtig ervaring aan uw gebruikers.
- Het beheren van toepassingen op geïsoleerd netwerken die geen deel uitmaken van de belangrijkste bedrijfsnetwerk is geïnstalleerd. U kunt verbindingslijn groepen speciale verbindingslijnen installeren op geïsoleerd netwerken te isoleren ook toepassingen bij het netwerk.
- Connector groepen bieden voor toepassingen geïnstalleerd op IaaS voor cloudtoegang, een algemene service beveiligde toegang tot alle apps. Connector groepen niet extra afhankelijkheid op uw bedrijfsnetwerk maken of de app-ervaring fragment. Verbindingslijnen kunnen worden geïnstalleerd op elke cloud-datacenter en alleen de toepassingen die zich in dit netwerk bevinden fungeren. U kunt verschillende verbindingslijnen als u wilt bereiken beschikbaarheid installeren.
- Ondersteuning voor meerdere bos omgevingen waarin specifieke verbindingslijnen kunnen worden geïmplementeerd per bos en ingesteld op specifieke toepassingen fungeren.
- Connector groepen kunnen worden gebruikt in herstel sites ofwel failover wordt gedetecteerd of als back-up maken voor de hoofdsite.
- Connector groepen kunnen ook worden gebruikt voor meerdere bedrijven van een enkel tenant.

## <a name="prerequisite-create-your-connectors"></a>Let op: Uw verbindingslijnen maken
Als u wilt groeperen uw verbindingslijnen, die u hebt om te controleren of u [meerdere verbindingslijnen geïnstalleerd](active-directory-application-proxy-enable.md). Wanneer u een nieuwe verbindingslijn installeert, verbindt deze automatisch de verbindingslijn **standaard** groep.

## <a name="step-1-create-connector-groups"></a>Stap 1: Verbindingslijn groepen maken
U kunt zo veel verbindingslijn groepen als u wilt maken. Connector voor het maken van groepen wordt in de [portal van Azure](https://portal.azure.com)bereikt.

1. Selecteer **Azure Active Directory** om naar het dashboard management voor uw adreslijst te gaan. Selecteer daar **bedrijfstoepassingen** > **toepassingsproxy**.

2. Selecteer de knop **Verbindingslijn groepen** . Het blad nieuwe verbindingslijn groep wordt weergegeven.

3. Geef een naam op voor de nieuwe groep voor de verbindingslijn en gebruik het vervolgkeuzemenu selecteren welke verbindingslijnen toevoegen in deze groep.

4. Selecteer **Opslaan** wanneer u de verbindingslijn groep is voltooid.

## <a name="step-2-assign-applications-to-your-connector-groups"></a>Stap 2: Toepassingen toewijzen aan uw groepen verbindingslijn
De laatste stap is om in te stellen van elke toepassing aan de groep verbindingslijn die wordt deze dienen.

1. Selecteer in het dashboard management voor uw adreslijst, **bedrijfstoepassingen** > **alle toepassingen** > de toepassing die u wilt toewijzen aan een groep verbindingslijn > **Toepassingsproxy**.
2. Gebruik het vervolgkeuzemenu om te selecteren van de groep die u wilt dat de toepassing gebruik onder **verbindingslijn groep**.
3. Selecteer **Opslaan** om toe te passen van de wijziging.


## <a name="see-also"></a>Zie ook

- [Toepassingsproxy inschakelen](active-directory-application-proxy-enable.md)
- [Inschakelen voor eenmalige aanmelding](active-directory-application-proxy-sso-using-kcd.md)
- [Voorwaardelijke toegang inschakelen](active-directory-application-proxy-conditional-access.md)
- [Problemen die u met toepassingsproxy ondervindt](active-directory-application-proxy-troubleshoot.md)

Voor het laatste nieuws en updates, raadpleegt u de [toepassingsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)
