Azure wordt de versie van Python wilt gebruiken voor de virtuele omgeving met de prioriteit van de volgende bepalen:

1. versie die zijn opgegeven in runtime.txt in de hoofdmap
1. versie die is opgegeven door de instelling Python in de configuratie van de web-app (de **Instellingen** > **Toepassingsinstellingen** blade voor uw web-app in de Portal Azure)
1. Python 2.7 is de standaardwaarde als geen van de bovenstaande zijn opgegeven

Geldige waarden voor de inhoud van 

    \runtime.txt

zijn:

- Python 2.7
- Python-3,4

Als de micro versie (derde cijfer) is opgegeven, wordt deze genegeerd.
