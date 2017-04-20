## <a name="peering-across-subscriptions"></a>Peering in abonnementen

In dit scenario maakt u een peering tussen twee VNets die deel uitmaken van verschillende abonnementen.

![cross sub scenario](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

VNet peering, is afhankelijk van rollen gebaseerd toegangsbeheer (RBAC) voor autorisatie. Scenario's met een cross-abonnementen moet u eerst voldoende toestemming verlenen aan gebruikers die de peering koppeling maakt:

> [AZURE.NOTE] Als dezelfde gebruiker de bevoegdheden voor beide abonnementen heeft, kunt u stap 1-4 onderstaande overslaan.
