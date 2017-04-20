<properties
    pageTitle="Verbinding maken met SQL-Database via Python | Microsoft Azure"
    description="Geeft een Python voorbeeld die kunt u verbinding maakt met Azure SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-python"></a>Verbinding maken met SQL-Database via Python


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Dit onderwerp wordt uitgelegd hoe u verbinding maken en een Azure SQL-Database met Python query. U kunt dit voorbeeld uitvoeren van Windows, Ubuntu Linux of Mac-platforms.


## <a name="step-1-create-a-sql-database"></a>Stap 1: Een SQL-database maken

Zie de [pagina aan de slag](sql-database-get-started.md) voor meer informatie over het maken van een voorbeelddatabase.  Het is belangrijk dat u volgt de richtlijnen voor het maken van een **sjabloon voor AdventureWorks-database**. In de voorbeelden hieronder werken alleen met het **schema voor AdventureWorks**. Nadat u uw database Zorg ervoor dat maken inschakelen u toegang naar uw IP-adres doordat de firewallregels zoals is beschreven in de [pagina aan de slag](sql-database-get-started.md)

## <a name="step-2-configure-development-environment"></a>Stap 2: Configureer ontwikkelomgeving

### <a name="mac-os"></a>**Mac OS**   
### <a name="install-the-required-modules"></a>De vereiste modules installeren
Open uw terminal en installeren

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Open uw terminal en navigeer naar een map waarin u van plan bent over het maken van uw python script. Voer de volgende opdrachten voor het installeren **FreeTDS** en **pymssql**. pymssql maakt FreeTDS verbinding met SQL-Databases.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1
    
### <a name="windows"></a>**Windows**

Installeer pymssql vanaf [**hier**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql). 

Zorg ervoor dat u het juiste whl-bestand kiezen. Bijvoorbeeld: als u Python 2.7 op een computer 64-bits gebruikt kiezen: pymssql‑2.1.1‑cp27‑none‑win_amd64.whl. Zodra u downloaden het bestand .whl deze in de map C:/Python27 plaatsen.

Nu het stuurprogramma pymssql pip vanaf de opdrachtregel gebruiken. cd in C:/Python27 en uitvoeren van de volgende handelingen uit
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Instructies voor het inschakelen van de pip gebruiken vindt u [hier](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="step-3-run-sample-code"></a>Stap 3: Voorbeeldcode uitvoeren

Maak een bestand met de naam van **sql_sample.py** en plak de volgende code binnen de vorm. U kunt dit uitvoeren vanaf de opdrachtregel gebruiken:
    
    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>Verbinding maken met uw SQL-Database

De functie [pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) wordt gebruikt voor het verbinding maken met SQL-Database.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>Een SQL SELECT-instructie uitvoeren

De functie [cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) kan worden gebruikt om op te halen een resultatenset uit een query ten opzichte van de SQL-Database. Deze functie is in principe er een query geaccepteerd en geeft als resultaat het die resultaat van een die instellen kunnen worden iteratie met het gebruik van [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone).


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Een rij invoegen, parameters doorgeven en de gegenereerde primaire sleutel ophalen

In SQL-Database kunnen de eigenschap [identiteit](https://msdn.microsoft.com/library/ms186775.aspx) en de [SEQUENTIE](https://msdn.microsoft.com/library/ff878058.aspx) -object worden gebruikt om waarden van de [primaire sleutel](https://msdn.microsoft.com/library/ms179610.aspx) automatisch te genereren. 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Transacties


In dit codevoorbeeld wordt het gebruik van transacties waarin u:

* Begin een transactie
* Een rij met gegevens invoegen
* Terugdraaien uw transactie ongedaan maken van de invoegen 

Plak de volgende code binnen sql_sample.py.
    
    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Volgende stappen

* Bekijk het [SQL-Database ontwikkelen-overzicht](sql-database-develop-overview.md)
* Meer informatie over het [Microsoft Python stuurprogramma voor SQL Server](https://msdn.microsoft.com/library/mt652092.aspx)
* Bezoek het [Python Developer Center](/develop/python/).

## <a name="additional-resources"></a>Aanvullende informatie 

* [Ontwerppatronen voor meerdere tenant SaaS toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Kennismaken met alle [functies van de SQL-Database](https://azure.microsoft.com/services/sql-database/)
