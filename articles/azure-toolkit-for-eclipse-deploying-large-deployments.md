<properties
    pageTitle="Grote implementaties implementeren"
    description="Leer hoe u grote implementaties met behulp van de Azure-Toolkit voor Eclips implementeren."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# <a name="deploying-large-deployments"></a>Grote implementaties implementeren #

Als uw implementatie te groot is moeten worden opgenomen in de standaardmap voor approot, kunt u een resource lokale opslag gebruiken als de implementatie-hoofdmap voor uw JDK en toepassingsserver.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>Een lokale opslag-bron gebruikt als de implementatie-hoofdmap voor grote implementaties ##

1. Maak een nieuwe lokale opslag resource. De naam van de resource maakt niet uit. Opslag resources zijn gedefinieerd op het niveau van de rol. De snelste manier voor toegang tot het dialoogvenster lokale opslag configuratie, waaruit u een nieuwe lokale opslag resource, kan maken is met behulp van de volgende stappen uit: met de rechtermuisknop op de rol in de weergave **Projectverkenner** (het projectknooppunt Azure uitvouwen als u de rol niet ziet,) **Azure**op en klik vervolgens op **Lokale opslag**. Klik op **toevoegen** om te maken van een nieuwe lokale opslag resource in het dialoogvenster **Lokale opslag** .
1. Stel de gewenste grootte op ten minste 2048 MB (minder problemen kan veroorzaken hetzelfde bestand grootte zoals u zou optreden in de approot).
1. Zorg ervoor dat **de inhoud als het exemplaar van de rol herhaald wordt wissen** is ingeschakeld; Dit helpt voorkomen dat de implementatie van opstarten logica optreden conflicten met bestaande bestanden in de resource wanneer het exemplaar van de rol herhaald wordt.
1. Zorg ervoor dat de **omgevingsvariabele opslaan het pad van de resource na implementatie** -waarde is ingesteld op de tekenreeks **DEPLOYROOT**. Uw lokale opslag resource dialoogvenster ziet er ongeveer als volgt uit.
    ![][ic667943]

U kunt ook als u **DEPLOYROOT** als de *naam* van uw lokale resource gebruiken en u niet de automatisch gegenereerde naam van de omgevingsvariabele wijzigen beter (dat wordt ingesteld op **DEPLOYROOT_PATH** in dat geval), die geschikt voor uw toepassing ook.

Aanvullende informatie over het maken van een resource lokale opslag kan worden gevonden op [Eigenschappen van de lokale opslag][].

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Maken van een toepassing van de wereld Hallo voor Azure in Eclips][]

[Installatie van de Azure Toolkit voor Eclips][] 

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Maken van een toepassing van de wereld Hallo voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[Eigenschappen van de lokale opslag]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png
