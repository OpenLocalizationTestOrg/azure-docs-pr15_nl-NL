## <a name="scenario"></a>Scenario

In dit document doorlopen een distributie die gebruikmaakt van meerdere NIC in VMs in een specifieke scenario. In dit scenario hebt u een IaaS twee lagen werkbelasting gehost in Azure wordt aangegeven. Elke laag is geïmplementeerd in een eigen subnet in een virtueel netwerk (VNet). De front-end van laag bestaat uit verschillende endwebservers, gegroepeerd in een taakverdeling instellen voor maximale beschikbaarheid. De back-end-laag bestaat uit verschillende databaseservers. Deze databaseservers wordt geïmplementeerd met twee NIC's, één telefoonnummer voor toegang tot de database, de andere voor beheer. Het scenario bevat ook netwerk beveiligingsgroepen (NSGs) om te bepalen welke verkeer is toegestaan met elk subnet en NIC in de implementatie. De onderstaande afbeelding ziet u de eenvoudige architectuur van dit scenario.  

![MultiNIC scenario](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

