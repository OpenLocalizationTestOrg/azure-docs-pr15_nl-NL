## <a name="scenario"></a>Scenario

Om te beter illustreren hoe u UDRs maakt, wordt dit document gebruikt in het onderstaande scenario.

![BESCHRIJVING VAN AFBEELDING](./media/virtual-network-create-udr-scenario-include/figure1.png)

In dit scenario maakt u één UDR voor de *voorgrond einde subnet* en een andere UDR voor het *Back-end-subnet* , zoals hieronder beschreven: 

- **UDR-FrontEnd**. De front-end UDR worden toegepast op het subnet *FrontEnd* en één route bevatten:  
    - **RouteToBackend**. Deze route stuurt al het verkeer naar de back-end subnet, de **FW1** virtuele machine.
- **UDR-BackEnd**. De back-end UDR wordt toegepast op de *BackEnd* -subnet en één route bevatten: 
    - **RouteToFrontend**. Deze route stuurt al het verkeer naar de front-end van subnet, de **FW1** virtuele machine.

De combinatie van deze routes zorgt ervoor dat dat al het verkeer bestemd uit één subnet naar een andere worden doorgestuurd naar de **FW1** virtuele machine, die wordt gebruikt als een virtuele toestel. U moet ook inschakelen IP-doorsturen voor die VM, zodat het verkeer naar andere VMs kan ontvangen.
