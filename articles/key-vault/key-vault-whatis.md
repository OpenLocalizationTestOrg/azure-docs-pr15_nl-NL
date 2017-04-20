<properties
    pageTitle="Wat is Azure-toets kluis? | Microsoft Azure"
    description="Azure-toets kluis helpt beschermen cryptografische sleutels en geheimen die worden gebruikt door de cloud-toepassingen en -services. Met behulp van Azure-toets kluis, klanten kunnen worden versleuteld toetsen en geheimen (zoals verificatiesleutels, opslag account toetsen, gegevens versleuteling toetsaanslagen. PFX-bestanden en wachtwoorden) met behulp van de toetsen die zijn beveiligd met hardware beveiligingsmodules (HSM's)."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="cabailey"/>



# <a name="what-is-azure-key-vault"></a>Wat is Azure-toets kluis?

Azure-toets kluis is beschikbaar in de meeste regio's. Zie de [sleutel kluis prijzen pagina](https://azure.microsoft.com/pricing/details/key-vault/)voor meer informatie.

## <a name="introduction"></a>Inleiding

Azure-toets kluis helpt beschermen cryptografische sleutels en geheimen die worden gebruikt door de cloud-toepassingen en -services. Met behulp van toets kluis, kunt u coderen toetsen en geheimen (zoals verificatiesleutels, opslag account toetsen, gegevens versleuteling toetsaanslagen. PFX-bestanden en wachtwoorden) met behulp van de toetsen die zijn beveiligd met hardware beveiligingsmodules (HSM's). U kunt importeren of toetsen in HSM's genereren voor toegevoegde assurance. Als u dit doet, Microsoft-processen uw sleutels in FIPS 140-2 niveau 2 gevalideerde HSM's (hardware en firmware).  

Toets kluis stroomlijnt het proces key management en kunt u het beheer van sleutels die openen en versleutelen van uw gegevens. Ontwikkelaars kunnen maken van sneltoetsen voor het ontwikkelen en testen in minuten en vervolgens naadloos migreren naar productie toetsen. Beveiligingsbeheerders van de kunnen verlenen (en intrekken) machtiging voor sleutels, indien nodig.

Gebruik de volgende tabel beter te begrijpen hoe toets kluis kan helpen om te voldoen aan de behoeften van ontwikkelaars en Beveiligingsbeheerders.





| Rol        | Probleembeschrijving           | Kan worden opgelost aan Azure belangrijke kluis  |
| ------------- |-------------|-----|
| Ontwikkelaars voor een Azure-toepassing      | "Ik wil een toepassing schrijven voor Azure die sleutels voor ondertekenen en versleutelen wordt gebruikt, maar ik wil dat deze toetsen moeten buiten mijn-toepassing, zodat de oplossing is geschikt voor een toepassing die geografisch is verdeeld. <br/><br/>Ik wil ook deze toetsen en geheimen te beschermen, zonder dat u moet de programmacode schrijven zelf. Ik ook wil deze toetsen en geheimen eenvoudig te gebruiken in mijn toepassingen, met optimale prestaties." | √ Sleutels zijn opgeslagen in een kluis en aangeroepen door URI als dat nodig is.<br/><br/> √ Sleutels zijn gewaarborgd door Azure, met gestandaardiseerde algoritmen, sleutels en hardware beveiligingsmodules (HSM's).<br/><br/> √ Sleutels worden verwerkt in HSM's die zich in de dezelfde Azure datacenters als de toepassingen bevinden. Dit biedt betere betrouwbaarheid en verminderde latentie dan als de toetsen bevinden zich in een andere locatie, zoals on-premises implementatie.|
| Ontwikkelaars voor Software als Service (SaaS)      |"Ik niet wilt dat de verantwoordelijkheid of mogelijke aansprakelijkheid voor mijn klanten tenant sleutels en geheimen. <br/><br/>Ik wil de klanten de eigenaar bent en het beheren van hun toetsen zodat ik vestigen kunt op doen wat ik doen best, waarin de core biedt software-functies." | √ Klanten kunnen hun eigen toetsen importeren in Azure en beheren. Wanneer een toepassing voor SaaS cryptografische bewerkingen uitvoeren moet met behulp van hun klanten toetsen, gebeurt toets kluis deze bewerkingen namens de toepassing. De toepassing niet de klanten toetsen te zien.|
| Hoofd beveiligingsbeambte (BBF) | "Ik wil dat onze toepassingen akkoord met FIPS 140-2 niveau 2 HSM's voor secure key management gaat. <br/><br/>Ik wil om ervoor te zorgen dat mijn organisatie controle over de levenscyclus van de belangrijkste is en gebruik van de sleutel kunt controleren. <br/><br/>En hoewel we verschillende Azure services en resources gebruiken, ik wil de toetsen beheren vanaf één locatie in Azure wordt aangegeven."     |√ HSM's zijn FIPS 140-2 niveau 2 gevalideerd.<br/><br/>√ Sleutel kluis is zo ontworpen dat Microsoft niet zien of uw sleutels extraheren.<br/><br/>√ in de buurt van realtime logboekregistratie van belangrijke gebruik.<br/><br/>√ De kluis biedt één interface, ongeacht het aantal kluizen u in Azure wordt aangegeven, welke regio's ze ondersteuning en welke toepassingen gebruiken. |


Iedereen met een Azure-abonnement kunt maken en gebruiken van belangrijke kluizen. Hoewel toets kluis ontwikkelaars en Beveiligingsbeheerders voordelen, kan deze kan worden geïmplementeerd en beheerd door de beheerder van een organisatie die andere Azure services voor een organisatie beheert. Deze beheerder zou Meld u aan met een Azure-abonnement maken, bijvoorbeeld een kluis voor de organisatie waarin toetsen opslaan en vervolgens belast operationele taken, zoals:

+ Maken of importeren van een sleutel of geheim
+ Intrekken of een toets of geheim verwijderen
+ Autoriseer gebruikers of toepassingen toegang tot de belangrijkste kluis, zodat ze vervolgens kunnen beheren of gebruik de toetsen en geheimen
+ Gebruik van de sleutel configureren (bijvoorbeeld, meld u aan of versleutelen)
+ Het belangrijkste gebruik controleren

Deze beheerder zou vervolgens ontwikkelaars URI's om te bellen vanuit hun toepassingen, en bieden hun beveiligingsbeheerder met gebruik van de sleutel logboekinformatie. 

   ![Overzicht van Azure belangrijke kluis][1]

Ontwikkelaars kunnen ook beheren de toetsen rechtstreeks met behulp van API's. Zie [de sleutel kluis handleiding voor ontwikkelaars](key-vault-developers-guide.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Zie [Aan de slag met Azure toets kluis](key-vault-get-started.md)voor een ophalen gestart zelfstudie voor een beheerder.

Zie [Azure toets kluis logboekregistratie](key-vault-logging.md)voor meer informatie over het gebruik van logboekregistratie voor sleutel kluis.

Zie voor meer informatie over het gebruik van toetsen en geheimen met Azure-toets kluis [over sleutels, geheimen, en certificaten](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
