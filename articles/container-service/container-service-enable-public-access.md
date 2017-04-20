<properties
   pageTitle="Openbare toegang inschakelen aan een ACS-app | Microsoft Azure"
   description="Het inschakelen van openbare toegang tot een Service van de Container Azure."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="timlt"/>

# <a name="enable-public-access-to-an-azure-container-service-application"></a>Openbare toegang tot een servicetoepassing Azure Container inschakelen

Een container domeincontroller/OS in de ACS- [agent openbare groep](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) wordt automatisch blootgesteld aan internet. Standaard, poorten **80**, **443**, **8080** worden geopend en eventuele (openbaar) container luisteren op deze poorten toegankelijk zijn. In dit artikel leest u hoe u meer poorten voor uw toepassingen opent in Azure Container-Service.

## <a name="open-a-port-portal"></a>Open een poort (portal) 

Eerst moeten we de poort die we wilt openen.

1. Meld u aan bij de portal.
2. Zoek de resourcegroep waarvoor u de Container Azure Service geïmplementeerd.
3. Selecteer de agent taakverdeling (dat heet vergelijkbaar met **XXXX-agent-kg-XXXX**).

    ![Azure container service taakverdeling](media/container-service-dcos-agents/agent-load-balancer.png)

4. Klik op **controleert** en vervolgens op **toevoegen**.

    ![Controleert of taakverdeling Azure container-service](media/container-service-dcos-agents/add-probe.png)

5. Vul het formulier test en klik op **OK**.

  	| Veld | Beschrijving |
  	| ----- | ----------- |
  	| Naam  | Een beschrijvende naam van de test. |
  	| Poort  | De poort van de container om te testen. |
  	| Pad  | (Wanneer in de modus voor HTTP) Het pad relatief website om te zoeken. HTTPS niet ondersteund. |
  	| Interval | De hoeveelheid tijd tussen test probeert, in seconden. |
  	| Drempelwaarde voor beschadigd | Dit is het aantal opeenvolgende test pogingen voordat de container beschadigd. | 
    

6. Op de eigenschappen van de verdeling van de agent belasting, klikt u op **laden van taakverdeling regels** en klik vervolgens op **toevoegen**.

    ![Azure container service laden de verdeling van regels](media/container-service-dcos-agents/add-balancer-rule.png)

7. Vul het formulier van de verdeling van laden en klik op **OK**.

  	| Veld | Beschrijving |
  	| ----- | ----------- |
  	| Naam  | Een beschrijvende naam van de taakverdeling. |
  	| Poort  | De openbare inkomende poort. |
  	| Back-end poort | De interne-openbare-poort van de container voor het routeren van verkeer naar. |
  	| Back-end-toepassingen | De containers in deze groep is het doel voor deze taakverdeling. |
  	| Zoeken | De test gebruikt om te bepalen of een doel in de **groep Backend** orde. |
  	| Permanente sessie | Bepaalt hoe verkeer van een client voor de duur van de sessie moet worden verwerkt.<br><br>**Geen**: opeenvolgende aanmeldingsaanvragen van dezelfde client door een container kunnen worden verwerkt.<br>**Client-IP-**: opeenvolgende aanvragen van de dezelfde client-IP worden verwerkt door de dezelfde container.<br>**Client-IP- en protocol**: opeenvolgende aanmeldingsaanvragen van de dezelfde client IP-protocol combinatie en zijn afgehandeld door de dezelfde container. |
  	| Inactiviteit | (Alleen TCP) De tijd als u een TCP/HTTP-client open in minuten, zonder te vertrouwen op *permanente* berichten. |

## <a name="add-a-security-rule-portal"></a>Een regel (portal) toevoegen

Vervolgens moet een regel die verkeer door van onze geopende poort via de firewall stuurt toevoegen.

1. Meld u aan bij de portal.
2. Zoek de resourcegroep waarvoor u de Container Azure Service geïmplementeerd.
3. Selecteer de **openbare** agent netwerk beveiligingsgroep (dat heet vergelijkbaar met **XXXX-agent-openbare-nsg-XXXX**).

    ![Azure container service netwerk-beveiligingsgroep](media/container-service-dcos-agents/agent-nsg.png)

4. Selecteer **Beveiligingsregels inkomende** en klik vervolgens op **toevoegen**.

    ![Azure container service netwerk beveiligingsregels-groep](media/container-service-dcos-agents/add-firewall-rule.png)

5. Vul de firewallregel naar uw openbare poort toestaan en klik op **OK**.

  	| Veld | Beschrijving |
  	| ----- | ----------- |
  	| Naam  | Een beschrijvende naam van de firewallregel. |
  	| Prioriteit | De rang van de prioriteit voor de regel. Hoe lager het nummer hoe hoger de prioriteit. |
  	| Bron | De binnenkomende IP-adresbereiken om te worden toegestaan of geweigerd door deze regel beperken. Met **een** kunt u niet opgeven een beperking. |
  	| Service | Selecteer een set vooraf gedefinieerde services die deze regel is bedoeld voor. Anders gebruiken **aangepast** aan uw eigen kleurenschema samenstellen. |
  	| Protocol | Beperken op basis van **TCP** of **UDP-**verkeer is toegestaan. Met **een** kunt u niet opgeven een beperking. |
  	| Poortbereik | Wanneer de **Service** is **aangepast**, geeft het bereik van poorten die deze regel geldt voor. U kunt een enkele poort, zoals **80**of een bereik zoals **1024-1500**gebruiken. |
  	| Actie | Toestaan of weigeren verkeer die voldoet aan de criteria. |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het verschil tussen [openbare en persoonlijke domeincontroller/OS agenten](container-service-dcos-agents.md).

Lees meer informatie over het [beheren van uw containers domeincontroller/OS](container-service-mesos-marathon-ui.md).