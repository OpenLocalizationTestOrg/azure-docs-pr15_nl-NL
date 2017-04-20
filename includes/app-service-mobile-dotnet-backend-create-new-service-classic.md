1. Meld u aan bij de [Portal van Azure].

2. Klik op **+ Nieuw** > **Web + Mobile** > **Mobile-App**, Geef een naam voor uw backend Mobile-App.

3. Selecteer een bestaande resourcegroep voor de **Resourcegroep**, of maak een nieuwe (met dezelfde naam als uw app). 
 
    U kunt een andere App serviceplan selecteren of een nieuw account te maken. Voor meer informatie over de App Services plannen en het maken van een nieuw abonnement in een verschillende prijzen trapsgewijs en op de gewenste locatie, Zie [uitgebreide overzicht van Azure-Service voor App-abonnementen](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. Voor het **App-abonnement**, is de standaard-abonnement (in de [standaard laag](https://azure.microsoft.com/pricing/details/app-service/)) ingeschakeld. U kunt ook een ander abonnement of [Maak een nieuw](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan)selecteren. Instellingen van de planning van de App Service bepalen de [locatie, functies, kosten en berekenen van resources](https://azure.microsoft.com/pricing/details/app-service/) die zijn gekoppeld aan uw app. 

    Nadat u hebt besloten op het abonnement, klikt u op **maken**. Hiermee wordt de Mobile-App-end gemaakt. 
    
6. Klik in het blad **Instellingen** voor de nieuwe Mobile-App-end, op **snel aan de slag** > uw client app-platform > **verbinding maken met een database**. 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. Klik in het blad **gegevensverbinding toevoegen** op **SQL-Database** > **een nieuwe database maken**, typt u de **naam**van de database, kiest u een prijzen laag en klik op **Server**.  U kunt deze nieuwe database opnieuw gebruiken. Als u al een database op dezelfde locatie hebt, kunt u in plaats daarvan **een bestaande database gebruiken**. Het gebruik van een database op een andere locatie wordt niet aanbevolen vanwege bandbreedtekosten en hoger latentie.
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. Typ een unieke naam in het veld **naam van de Server** in het blad **nieuwe server** , bieden een aanmelden en een wachtwoord, **toestaan azure-services voor toegang tot server**controleren en klik op **OK**. Hiermee wordt de nieuwe database gemaakt.

9. Terug in het blad **gegevensverbinding toevoegen** , klikt u op **verbindingsreeks**, typt u de waarden aanmelden en wachtwoord voor uw database en klik op **OK**. Wacht enkele minuten duren voordat de database moet worden geïmplementeerd voordat u verdergaat.

<!-- URLs. -->
[Azure-Portal]: https://portal.azure.com/
