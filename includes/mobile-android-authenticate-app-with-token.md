
Het vorige voorbeeld blijkt een standaard-aanmelden, waarvoor de client contact opnemen met zowel de identiteitsprovider als de backend Azure-service elke keer dat de app wordt gestart. Niet alleen is deze methode niet efficiënt, dat u kunt uitvoeren in gebruik problemen moeten veel klanten probeert te starten u app op hetzelfde moment. Een betere benadering is het Autorisatietoken die het resultaat van de Azure-service in cache en probeert u het gebruik van deze eerste vóór het gebruik van een provider gebaseerde aanmelden. 

>[AZURE.NOTE]U kunt het token uitgegeven door de backend Azure-service ongeacht of u van client beheerde of service-beheerd verificatie gebruikmaakt cache. In deze zelfstudie wordt verificatie service worden beheerd.


1. Open het bestand ToDoActivity.java en de volgende importinstructies toevoegen:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. Toevoegen het de volgende leden die u wilt de `ToDoActivity` klasse.

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. Klik in het document ToDoActivity.java toevoegen het de volgende definitie voor de `cacheUserToken` methode.
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    Deze methode slaat de gebruikers-id en de token in een voorkeursbestand dat is gemarkeerd als privé. Dit moet toegang tot de cache beveiligen zodat andere apps op het apparaat geen toegang tot het token omdat de voorkeur sandbox voor de app. Echter als iemand toegang tot het apparaat krijgt, is het mogelijk dat mogelijk ze toegang hebben tot de token cache andere manier. 

    >[AZURE.NOTE]U kunt het token met versleuteling verder beveiligen als token toegang tot uw gegevens gevoelige en iemand mogelijk toegang krijgen tot het apparaat. Een volledig veilige oplossing is echter buiten het bereik van deze zelfstudie en afhankelijk van uw beveiligingsvereisten die zijn.


4. Klik in het document ToDoActivity.java toevoegen het de volgende definitie voor de `loadUserTokenCache` methode.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. Vervang in het bestand *ToDoActivity.java* , de `authenticate` methode met de volgende methode die gebruikmaakt van een token cache. De provider login wijzigen als u wilt een ander account dan Google gebruiken.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. De app en test verificatie via een geldige account maken. Ten minste twee keer uitvoeren. Tijdens het eerst wordt uitgevoerd, moet u wordt gevraagd om zich aanmeldt en de cache voor token te maken. Daarna elke uitvoeren wordt geprobeerd de token cache voor verificatie wilt laden en zijn niet verplicht voor het aanmelden.



