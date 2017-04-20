<properties
    pageTitle="Zelfstudie: WebApp met een meerdere tenant-database met behulp van entiteit Framework en beveiliging op gebruikersniveau rij"
    description="Informatie over het ontwikkelen van een ASP.NET MVC 5-web-app met een multi-tenant SQL-Database backent, met entiteit Framework en beveiliging op gebruikersniveau rij."
  metaKeywords="azure asp.net mvc entity framework multi tenant row level security rls sql database"
    services="app-service\web"
    documentationCenter=".net"
    manager="jeffreyg"
  authors="tmullaney"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/25/2016"
    ms.author="thmullan"/>

# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Zelfstudie: WebApp met een meerdere tenant-database met behulp van entiteit Framework en beveiliging op gebruikersniveau rij

Deze zelfstudie leert hoe u een meerdere tenant WebApp met een "[gedeelde database, gedeelde schema](https://msdn.microsoft.com/library/aa479086.aspx)" pachtadres model, met entiteit Framework en [Beveiliging op gebruikersniveau rij](https://msdn.microsoft.com/library/dn765131.aspx)maken. In dit model, één database gegevens bevat voor veel tenants en elke rij in elke tabel is gekoppeld aan een '-Tenant." Rij niveau beveiliging (RLS), een nieuwe functie voor Azure SQL-Database wordt gebruikt om te voorkomen dat tenants toegang hebben tot elkaars gegevens. U moet hiervoor slechts één, kleine wijziging aan de toepassing. Door de logica tenant toegang binnen de database zelf centraal, RLS eenvoudiger van de toepassingscode en Hiermee reduceert u de kans op pesterijen gegevens per ongeluk lekkage tussen tenants.

Laten we beginnen met de eenvoudige Contact Manager-toepassing uit [een ASP.NET-MVP-app met auth en SQL DB maken en implementeren naar Azure App-Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Rechts, de toepassing kunt nu alle gebruikers (tenants) om alle contactpersonen weer te geven:

![Contact Manager-toepassing voordat u inschakelt RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Met een paar kleine wijzigingen, wordt we ondersteuning voor meerdere pachtadres, toevoegen, zodat gebruikers kunnen om alleen de contactpersonen die deel uitmaakt van deze weer te geven.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Stap 1: Een klasse Interceptor toevoegen in de toepassing voor het instellen van de SESSION_CONTEXT

Er is een toepassing wijziging die we hoeven te maken. Omdat alle gebruikers van toepassingen verbinden met de database met de dezelfde verbindingsreeks (dat wil zeggen dezelfde SQL aanmelden), is momenteel geen manier om een beleid RLS weten welke gebruiker dit moet hierop wilt filteren. Deze methode werkt veelvoorkomende in webtoepassingen omdat kunt efficiënt groepsgewijze, maar betekent dit dat we een andere manier om aan te geven van de huidige toepassingsgebruiker in de database nodig. De oplossing is dat de toepassing een paar sleutelwaarden instellen voor de huidige gebruikersnaam in het [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) direct na het openen van een verbinding voordat deze query's hebt uitgevoerd. SESSION_CONTEXT is een sleutelwaarde sessie beperkte store en ons beleid RLS de gebruikers-id die zijn opgeslagen in deze wordt gebruikt om de huidige gebruiker te identificeren.

Voeg een [interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (met name, een [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), wordt een nieuwe functie in entiteit Framework (EF) 6, automatisch instellen van de huidige gebruikersnaam in het SESSION_CONTEXT door het uitvoeren van een T-SQL-instructie wanneer EF een verbinding opent.

1.  Open het project ContactManager in Visual Studio.
2.  Met de rechtermuisknop op de map modellen in de Verkenner oplossing en klikt u op toevoegen > Class.
3.  Geef de nieuwe klasse "SessionContextInterceptor.cs" naam en klik op toevoegen.
4.  De inhoud van SessionContextInterceptor.cs vervangen door de volgende code.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }
        
        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

Dit is de enige toepassing wijziging vereist. Verdergaan en maken en publiceren van de toepassing.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Stap 2: Een gebruikers-id-kolom toevoegen aan het databaseschema

Vervolgens moeten we een gebruikers-id-kolom toevoegen aan de tabel Contactpersonen elke rij koppelen aan een gebruiker (tenant). We wordt het schema rechtstreeks in de database, wijzigen en wat u niet hoeft te dit veld in het gegevensmodel van onze EF opnemen.

Verbinding maken met de database rechtstreeks met behulp van SQL Server Management Studio of Visual Studio, en klikt u vervolgens de volgende T-SQL-instructies uitvoeren:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Hiermee voegt u een gebruikers-id-kolom aan de tabel Contactpersonen. We het gegevenstype nvarchar(128) gebruiken om te voldoen aan de gebruikersnamen die zijn opgeslagen in de tabel AspNetUsers en maken we een standaardbeperking die de gebruikers-id voor de nieuw ingevoegde rijen moeten de gebruikers-id die is opgeslagen in SESSION_CONTEXT automatisch worden ingesteld.

De tabel ziet er nu zo uit:

![Tabel SSMS contactpersonen](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Wanneer nieuwe contactpersonen worden gemaakt, wordt worden deze automatisch de juiste gebruikers-id toegewezen. Voor demodoeleinden, maar we toewijzen een paar van deze bestaande contactpersonen aan een bestaande gebruiker.

Als u een paar gebruikers in de toepassing al hebt gemaakt (bijvoorbeeld met een lokale, Google of Facebook accounts), ziet u deze in de tabel AspNetUsers. In de onderstaande schermafbeelding is er slechts één gebruiker dusverre.

![SSMS AspNetUsers-tabel](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Kopieer de Id voor user1@contoso.com, en plak deze in de onderstaande T-SQL-instructie. Deze expressie om de drie van de contactpersonen koppelen aan deze gebruikers-id uitvoeren.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Stap 3: Beveiliging op gebruikersniveau rij-beleid maken in de database

De laatste stap is het opzetten van een beveiligingsbeleid waarin de gebruikers-id in SESSION_CONTEXT voor het automatisch filteren de resultaten van query's wordt gebruikt.

Nog steeds verbinding gemaakt met de database en de volgende T-SQL-instructies uitvoeren:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

Deze code biedt drie items. Eerst wordt een nieuw schema aangeraden voor centraal opslaan en het beperken van toegang tot de objecten RLS gemaakt. Vervolgens wordt een predikaatfunctie waarmee '1' wordt geretourneerd als de gebruikers-id van een rij overeenkomt met de gebruikersnaam in SESSION_CONTEXT gemaakt. Tot slot dat wordt gemaakt met een beveiligingsbeleid dat deze functie toegevoegd als een filter en een blok predicaat op de tabel Contactpersonen. Het filter predikaat worden query's om terug te keren alleen de rijen die deel uitmaakt van de huidige gebruiker en het predikaat blokkeren fungeert als een beschermen om te voorkomen dat de toepassing ooit per ongeluk het invoegen van een rij voor de verkeerde gebruiker.

Nu de toepassing wordt uitgevoerd en meld u aan als user1@contoso.com. Nu ziet deze gebruiker alleen de contactpersonen die we eerder aan deze gebruikers-id toegewezen:

![Contact Manager-toepassing voordat u inschakelt RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Probeer te registreren van een nieuwe gebruiker om te valideren dit verder. Zien zij geen contactpersonen, omdat er geen zijn toegewezen aan deze. Deze worden toegewezen aan deze als ze een nieuwe contactpersoon maken, en alleen zij kunnen zien.

## <a name="next-steps"></a>Volgende stappen

Dat is alles. De eenvoudige Contact Manager-web-app is geconverteerd naar een meerdere tenant een waar elke gebruiker een eigen lijst met contactpersonen heeft. Met behulp van beveiliging op gebruikersniveau rij hebt we de complexiteit van een tenant access logica in onze toepassingscode afdwingen voorkomen. Deze doorzichtigheid kan de toepassing kunt richten op het probleem reële bedrijven waaraan u bezig bent en het vermindert ook de kans op pesterijen gegevens per ongeluk lekkende terwijl de toepassing bevindt zich codebase in omvang groeit.

Deze zelfstudie heeft horen bij elkaar van de mogelijkheden van RLS. Bijvoorbeeld, is het mogelijk te hebben meer geavanceerde of gedetailleerde access logica, waarna ze het opslaan van meer dan alleen de huidige gebruikersnaam in het SESSION_CONTEXT. Het is ook mogelijk om te [integreren RLS met de elastische database extra client-bibliotheken](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) ter ondersteuning van meerdere tenant shards in een gegevenslaag schalen.

Na deze mogelijkheden werken we ook nog beter zodat RLS. Als u vragen hebt, ideeën of dingen die u zien wilt, laat het ons weten van de opmerkingen. Waarderen we uw feedback!
