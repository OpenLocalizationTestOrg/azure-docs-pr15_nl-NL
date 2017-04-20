### <a name="app-service-plan"></a>App-abonnement

Hiermee maakt u het abonnement dat voor het hosten van de web-app. U opgeven de naam van het abonnement via de parameter **hostingPlanName** . De locatie van het abonnement dat is gebruikt voor de resourcegroep dezelfde locatie. De prijzen laag en werknemer grootte zijn opgegeven in de parameters **sku** en **workerSize**

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

