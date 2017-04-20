
1. In **Projectverkenner** in Android Studio, opent u het bestand ToDoActivity.java en voeg de volgende importinstructies.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. De volgende methode toevoegen aan de klasse **ToDoActivity** : 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    Hiermee maakt u een nieuwe methode voor het verwerken van het verificatieproces. De gebruiker is geverifieerd met behulp van een Google-aanmelding. Een dialoogvenster weergegeven waarin de ID van de geverifieerde gebruiker. U kunt niet doorgaan zonder een positief verificatie.

    > [AZURE.NOTE] Als u een identiteitsprovider dan Google gebruikt, wijzigt u de waarde doorgegeven aan de bovenstaande methode **login** naar een van de volgende handelingen uit: _MicrosoftAccount_, _Facebook_, _Twitter_of _windowsazureactivedirectory_.

3. In de methode **onCreate** , voegt u de volgende regel met code na de code die wordt de `MobileServiceClient` object.

        authenticate();

    In dit gesprek begint het verificatieproces.

4. Verplaatst u de resterende code na `authenticate();` in de methode **onCreate** naar een nieuwe **createTable** -methode, die er zo uitziet:

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9. Vanuit het menu **uitvoeren** en klik vervolgens op **app uitvoeren** als u wilt start de app en meld u aan met uw door u gekozen identiteitsprovider. 

    Wanneer u zich heeft aangemeld-in, de app moet worden uitgevoerd zonder fouten en u moet kunnen query de backend-service en updates in de gegevens.