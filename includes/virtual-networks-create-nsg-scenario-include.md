## <a name="scenario"></a>Scenario

Om te beter illustreren hoe u NSGs maakt, wordt dit document gebruikt in het onderstaande scenario.

![VNet scenario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

In dit scenario maakt u een NSG voor elk subnet in de virtuele **TestVNet** -netwerk, zoals hieronder beschreven: 

- **NSG-FrontEnd**. De front-end NSG worden toegepast op het subnet *FrontEnd* en twee regels bevatten:  
    - **rdp-regel**. Deze regel wordt RDP-verkeer naar het subnet *FrontEnd* toestaan.
    - **web-regel**. Deze regel wordt HTTP-verkeer naar het subnet *FrontEnd* toestaan.
- **NSG-BackEnd**. De back-end NSG wordt toegepast op de *BackEnd* -subnet en twee regels bevatten: 
    - **sql-regel**. Deze regel staat SQL-verkeer alleen uit het subnet *FrontEnd* .
    - **web-regel**. Deze regel al dan niet toestaan van dat alle internet verkeer afhankelijk van de *BackEnd* -subnet.

De combinatie van deze regels een DMZ-achtige-scenario, waar het back-end-subnet kan alleen binnenkomende verkeer ontvangen voor SQL uit het subnet front-end en heeft geen toegang tot Internet, terwijl de front-end van subnet kunt met Internet communiceren, en ontvangen van binnenkomende HTTP-aanvragen maken.
 
