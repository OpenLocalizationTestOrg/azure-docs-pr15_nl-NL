
<!--
includes/sql-database-include-connection-string-40-config.md

Latest Freshness check:  2015-09-04 , GeneMi.

## Connection string
-->


### <a name="example-config-file-for-connection-string-security"></a>Voorbeeld configuratiebestand voor verbinding tekenreeks waardepapier


Ongezond moeten worden geplaatst de verbindingsreeks als letterlijke waarden in uw C#-code is. Is het beter de verbindingsreeks in een config-bestand hebt opgeslagen. Er kunt u de tekenreeks elk gewenst moment zonder dat u compileren.

Stel dat uw gecompileerd C#-programma heet **ConsoleApplication1.exe**, en dat deze .exe bevindt zich in een **bin\debug\* * directory.

In dit voorbeeld worden de meeste onderdelen van de verbindingsreeks opgeslagen in een config-bestand met de naam exact **ConsoleApplication1.exe.config**. In dit configuratiebestand, moet u ook zich bevinden **bin\debug\**.

U ziet een verbindingsreeks **ConnectionString4NoUserIDNoPassword**met de naam in het XML-bestand van de volgende configuratiebestand. De C#-code Hiermee wordt gezocht naar deze tekenreeks.

Reële namen in de tijdelijke aanduidingen, moet u bewerken:

- {your_serverName_here}
- {your_databaseName_here}



        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
        
            <connectionStrings>
                <clear />
                <add name="ConnectionString4NoUserIDNoPassword"
                providerName="System.Data.ProviderName"
        
                connectionString=
                "Server=tcp:{your_serverName_here}.database.windows.net,1433;
                Database={your_databaseName_here};
                Connection Timeout=30;
                Encrypt=True;
                TrustServerCertificate=False;" />
            </connectionStrings>
        </configuration>



In dit voorbeeld we hebben gekozen twee parameters weglaten:

- Gebruikers-ID = {your_userName_here};
- Wachtwoord = {your_password_here};


Kunt u deze opnemen, maar soms is het beter om het programma dat deze waarden krijgen van de invoer van het toetsenbord door de gebruiker. Dit hangt af.



<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
