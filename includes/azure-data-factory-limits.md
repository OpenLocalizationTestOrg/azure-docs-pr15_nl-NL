Gegevens factory is een meerdere tenant-service die heeft de volgende standaardlimieten ter plaatse om ervoor te zorgen dat klanten abonnementen zijn beveiligd uit elkaars werkbelastingen. Veel van de limieten kunnen worden eenvoudig verhoogd voor uw abonnement snel aan de maximale limiet contact opnemen met ondersteuning. 

**Resource** | **Standaardlimiet** | **Maximumlimiet**
-------- | ------------- | -------------
bedrijven van de gegevens in een Azure-abonnement | 50 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
pijpleidingen binnen een fabriek gegevens | 2500 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
gegevenssets binnen een fabriek gegevens | 5000 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
gelijktijdige segmenten per gegevensset | 10 | 10
bytes per object voor verkooppijplijn objecten <sup>1</sup> | 200 KB | 2000 KB
bytes per object voor gegevensset en gekoppelde service objecten <sup>1</sup> | 100 KB | 2000 KB
HDInsight op aanvraag cluster cores binnen een abonnement <sup>2</sup> | 48 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Cloud gegevens verkeer eenheid <sup>3</sup> | 8 | [Contact opnemen met ondersteuning](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Probeer tellen voor verkooppijplijn activiteit wordt uitgevoerd | 1000 | MaxInt (32-bits)

<sup>1</sup> verkooppijplijn, gegevensset en gekoppelde serviceobjecten vertegenwoordigen een logische groepering van uw werkzaamheden. Limieten voor deze objecten geen betrekking hebben op hoeveelheid gegevens die u kunt verplaatsen en verwerken met de Azure gegevens Factory-service. Gegevens factory is bedoeld om schaal worden afgehandeld petabytes van gegevens.

<sup>2</sup> op aanvraag HDInsight cores worden toegewezen afmelden bij het abonnement waaraan de fabriek gegevens bevat. De bovenstaande limiet is daardoor de fabriek gegevens core limiet voor op aanvraag HDInsight cores afgedwongen en verschilt van de limiet core is gekoppeld aan uw Azure abonnement.

<sup>3</sup> cloud gegevens verkeer eenheid (DMU) wordt gebruikt in een van de cloud-naar-cloud-kopieerbewerking. Dit is een maateenheid die de kracht (een combinatie van processor en geheugen netwerk resourcetoewijzing) van één eenheid in Data Factory vertegenwoordigt. U kunt gebruikmaken van meer DMUs voor bepaalde scenario's bereiken sneller kopie worden verwerkt. Raadpleeg de sectie [Cloud gegevens verkeer eenheden](../../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) op de pagina details.

**Resource** | **Standaard ondergrens** | **Ondergrens**
-------- | ------------------- | -------------
Interval plannen | 15 minuten | 15 minuten
Interval tussen nieuwe pogingen | 1 seconde | 1 seconde
Probeer time-outwaarde | 1 seconde | 1 seconde


### <a name="web-service-call-limits"></a>Beperkingen voor de oproep van de Web-service

Azure resourcemanager heeft limieten voor API-oproepen. U kunt API-oproepen op een tarief binnen de [limieten van Azure resourcemanager API](../azure-subscription-service-limits.md#resource-group-limits)maken. 


