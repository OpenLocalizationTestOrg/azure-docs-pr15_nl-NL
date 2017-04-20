<properties
   pageTitle="Azure analyseservices beheren | Microsoft Azure"
   description="Informatie over het beheren van een Analysis Services-server in Azure wordt aangegeven."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="manage-analysis-services"></a>Analyseservices beheren

Als u een Analysis Services-server hebt gemaakt in Azure wordt aangegeven, kunnen er enkele beheerprogramma taken die u wilt uitvoeren direct af of ergens omlaag onderweg. Bijvoorbeeld uitvoeren verwerken met de gegevens vernieuwen, bepalen wie toegang de modellen op de server of van de server servicestatus bewaken. Bepaalde beheertaken kunnen alleen worden uitgevoerd in Azure-portal anderen in SQL Server Management Studio (SSMS), en sommige taken kunnen worden uitgevoerd in een.

## <a name="azure-portal"></a>Azure-portal
De [Azure-portal](http://portal.azure.com/) is waar u kunt maken en verwijderen van servers, bewaken serverbronnen grootte wijzigen en beheren wie toegang heeft tot uw servers.  Als u bepaalde problemen ondervindt, kunt u ook een verzoek voor ondersteuning verzenden.

![Servernaam ophalen in Azure wordt aangegeven](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Verbinding maken met uw mailserver in Azure lijkt op verbinding maken met een server-instantie in uw eigen organisatie. Van SSMS, kunt u veel van dezelfde taken zoals procesgegevens uitvoeren of maken van een verwerkingsscript, rollen beheren en PowerShell gebruiken.

![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

 Een van de verschillen groter te maken is de verificatie die u kunt verbinding maken met uw server. Als u wilt verbinden met uw Azure Analysis Services-server, moet u **Active Directory-wachtwoordverificatie**selecteren.

### <a name="to-connect-with-ssms"></a>Verbinding maken met SSMS
1. Voordat u verbinding maakt, moet u de naam van de server. In **de portal van Azure** > server > **Overzicht** > **servernaam**de naam van de server kopiëren.

    ![Servernaam ophalen in Azure wordt aangegeven](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

2. In SSMS > **Object Explorer**, klikt u op **verbinding maken met** > **Analysis Services**.

3. Klik in het dialoogvenster **verbinding maken met de Server** in de naam van de server en klik in plakken in **verificatie**, kiest u een van de volgende opties:

    **Geïntegreerde verificatie van active Directory** met eenmalige aanmelding met Active Directory Azure Active Directory federation.

    **Active Directory-wachtwoordverificatie** gebruik van een organisatie-account. Bijvoorbeeld wanneer verbinding maakt in een niet-domein samengevoegd computer.

    Opmerking: Als u Active Directory-verificatie niet ziet, moet u mogelijk [Azure Active Directory-verificatie inschakelen](#enable-azure-active-directory-authentication) in SSMS.

    ![Verbinding maken in SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

Aangezien het beheren van uw server in Azure wordt aangegeven met behulp van SSMS vergelijkbaar met een on-premises server beheren is, gaan we niet dieper op hier meer informatie. Alle hulp u nodig hebt vindt u in [Analysis Services-exemplaar Management](https://msdn.microsoft.com/library/hh230806.aspx) op MSDN.

## <a name="server-administrators"></a>Serverbeheerders van de
U kunt **Analysis Services-beheerders** in het blad beheer voor uw server in Azure portal of SSMS serverbeheerders beheren. Analysis Services-beheerders zijn beheerders van de database-server met rechten voor algemene database beheertaken zoals toevoegen en verwijderen van databases en gebruikers beheren. Standaard wordt automatisch de gebruiker die wordt gemaakt van de server in Azure-portal toegevoegd als een Analysis Services-beheerder.

U moet ook weten:

-   Windows Live ID is niet van een identiteitstype ondersteunde voor Azure Analysis Services.  
-   Analysis Services-beheerders moeten geldige Azure Active Directory-gebruikers.
-   Als een Azure Analysis Services-server via Azure resourcemanager sjablonen maakt, gaat Analysis Services-beheerders een matrix JSON van gebruikers die moet worden toegevoegd als beheerders.

Analysis Services-beheerders kunnen afwijken van Azure resource beheerders, die resources voor Azure-abonnementen kunnen beheren. Op deze manier compatibiliteit met bestaande XMLA en TSML gedrag in Analysis Services beheren en zodat u kunt scheiden van rechten tussen Azure resourcebeheer en Analysis Services-database management gehandhaafd.

Als u wilt weergeven van alle functies en toegang tot typen voor de resource Azure Analysis Services, gebruikt u toegangsbeheer (IAM) op het blad besturingselement.

## <a name="database-users"></a>Databasegebruikers
Azure Analysis Services-model databasegebruikers moeten zich in uw Azure Active Directory. Gebruikersnamen die zijn opgegeven voor de modeldatabase moet door de organisatie-e-mailadres of UPN. Dit verschilt van on-premises implementatie model databases die ondersteuning bieden voor gebruikers met Windows domein gebruikersnamen.

U kunt gebruikers toevoegen met behulp van [roltoewijzingen in Azure Active Directory](../active-directory/role-based-access-control-configure.md) of met behulp van [Tabellaire Model uitvoeren van scripttaal](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) in SQL Server Management Studio.

**Voorbeeldscript voor TMSL**

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Users"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@contoso.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="enable-azure-active-directory-authentication"></a>Azure Active Directory-verificatie inschakelen
De functie voor het Azure Active Directory-verificatie inschakelen voor SSMS in het register, maak een tekstbestand met de naam EnableAAD.reg, en vervolgens kopiëren en plak de volgende handelingen uit:


```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\Microsoft Analysis Services\Settings]
"AS AAD Enabled"="True"
```

Opslaan en voer vervolgens het bestand.



## <a name="next-steps"></a>Volgende stappen
Als u dit nog niet hebt al een tabelmodel geïmplementeerd naar uw nieuwe server, is een goed moment. Meer informatie raadpleegt u [Deploy met Azure Analysis Services](analysis-services-deploy.md).

Als u hebt een model geïmplementeerd in uw server, bent u gereed om te verbinden met behulp van een client of de browser. Zie meer informatie, [gegevens ophalen van Azure Analysis Services-server](analysis-services-connect.md).
