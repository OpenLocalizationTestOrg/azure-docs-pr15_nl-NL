Azure bepaalt dat uw toepassing gebruikmaakt van Python **Als deze voorwaarden wordt voldaan**:

- Requirements.txt-bestand in de hoofdmap
- .py bestanden in de hoofdmap of een runtime.txt waarmee wordt opgegeven python

Wanneer dit het geval is, wordt een Python specifieke implementatiescript, waarmee de standaard synchronisatie van bestanden, evenals de extra Python bewerkingen zoals gebruikt:

- Automatisch beheer van virtuele omgeving
- Installatie van pakketten in de lijst in requirements.txt met pip
- Het maken van de juiste web.config op basis van de geselecteerde Python-versie.
- Statische bestanden voor Django toepassingen verzamelen

U kunt bepaalde aspecten van de implementatiestappen standaard bepalen zonder dat u moet het script aanpassen.

Als u overslaan van alle stappen van de Python specifieke implementatie wilt, kunt u deze leeg bestand maken:

    \.skipPythonDeployment

Als u overslaan verzameling statische bestanden voor uw toepassing Django wilt:

    \.skipDjango 

Voor meer controle over de implementatie, kunt u de standaard-implementatiescript negeren door het maken van de volgende bestanden:

    \.deployment
    \deploy.cmd

U kunt de [interface van Azure opdrachtregel][] gebruiken om de bestanden te maken.  Gebruik deze opdracht uit de projectmap van uw:

    azure site deploymentscript --python

Wanneer deze bestanden niet bestaat, wordt Azure Hiermee maakt u een tijdelijke implementatiescript en deze wordt uitgevoerd.  Dit is gelijk aan de sectie die u met de bovenstaande opdracht maakt.

[Azure opdrachtregel-interface]: http://azure.microsoft.com/downloads/
