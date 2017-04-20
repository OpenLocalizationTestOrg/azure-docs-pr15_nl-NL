<properties 
    pageTitle="Azure RemoteApp - testen van uw netwerk bandbreedtegebruik met enkele veelvoorkomende scenario's | Microsoft Azure"
    description="Informatie over veelvoorkomende scenario's u kunnen bepalen hoe zit de behoeften van uw netwerk bandbreedte voor Azure RemoteApp."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp - testen van uw netwerk bandbreedtegebruik met enkele veelvoorkomende scenario 's

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Wat is behandeld in [schatting Azure RemoteApp netwerkbandbreedte gebruikt](remoteapp-bandwidth.md), wordt de beste manier om te bepalen wat de invloed van Azure RemoteApp met uw netwerk om uit te voeren sommige gebruik is getest. Deze tests voor een gegeven periode uitvoeren en de bandbreedte die u nodig hebt voor elk scenario meten. Als u de mogelijkheid hebt, kunt u ook het pakket verlies en netwerk netwerkstoringen begrip van de patronen netwerk die worden gemaakt in uw omgeving meten.

    
Vergeet niet dat gebruik tussen verschillende gebruikers binnen uw bedrijf varieert om te bepalen of het bandbreedtegebruik. Bijvoorbeeld bandbreedte tekstlezers en schrijvers meestal kleiner is dan de gebruikers die met video werken. Voor de beste resultaten bestuderen van uw eigen gebruikersbehoeften en maak een combinatie van de volgende scenario's die gebruikers in uw bedrijf het beste wordt aangeduid. Onthoud dat moet worden [controleren van de factoren die van invloed bandbreedtegebruik en gebruiker ervaart](remoteapp-bandwidthexperience.md) - waarmee u kunt de ideaal tests identificeren.

Lees eerst de tests, kies uw mix en voert u deze. Gebruik de onderstaande tabel kunt u helpt bij het bijhouden van prestaties.

>[AZURE.NOTE] Als u niet uw eigen netwerk testen of er geen de tijd kunt doen, raadpleegt u onze [Basisnetwerk bandbreedte maakt een schatting/aanbevelingen](remoteapp-bandwidthguidelines.md). Uw reiskosten variëren, maar als u *kunt* uw eigen tests uitvoeren, moet u.


## <a name="the-usage-tests"></a>De tests gebruik
Elk van deze tests uitvoeren voor verschillende hoeveelheden tijd en andere functies/functies die netwerk bandbreedte testen. Onthoud dat de combinatie van test kiezen beste past bij uw voor individuele bedrijfsgebruikers.
 
### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Executive/complex PowerPoint - uitvoeren voor 900-1000 seconden

Een gebruiker worden weergegeven tussen 45 tot 50 hoogwaardige dia's met behulp van Microsoft Office PowerPoint in de modus volledig scherm. De dia's mogen bevatten afbeeldingen, overgangen (met animaties) en achtergronden met kleurovergang met kleuren die typische voor uw bedrijf zijn. De gebruiker moet ten minste 20 seconden besteedt aan elke dia.
    
Dit scenario gemaakt bursty-verkeer is toegestaan, wanneer een dia naar de volgende dia in de presentatie overgaat.
    
### <a name="simple-powerpoint---run-for-620-seconds"></a>Eenvoudige PowerPoint - uitvoeren voor ~ 620 seconden

Een gebruiker biedt een eenvoudige PowerPoint-bestand ongeveer 30 dia's met behulp van Microsoft Office PowerPoint in de modus volledig scherm. De dia's meer tekst bevatten dan in het Executive/complex PowerPoint-scenario zijn en eenvoudiger achtergronden en afbeeldingen (zwart diagrammen). 
    
### <a name="internet-explorer---run-for-250-seconds"></a>Internet Explorer - uitvoeren voor 250 seconden

Een gebruiker navigeert het web met Internet Explorer. De gebruiker bladert en schuift u via een combinatie van tekst, natuurlijke afbeeldingen en sommige schema's. De webpagina's die zijn opgeslagen op de lokale harde schijf van de server extern bureaublad (sessiehost) als een. MHT-bestand. De gebruiker schuift Page Up, Page Down, omhoog en omlaag toetsaanslagen gebruikt met verschillende intervallen voor elk sleutel/type schuifbalk:
    
    - Pijl - omlaag 250 toetsaanslagen helemaal 500 ms
    - Page Up - 36 toetsaanslagen elke 1000 ms
    - Pijl omlaag - 75 toetsaanslagen elke 100 ms
    - Page Down - 20 toetsaanslagen elke 500 ms
    - Omhoog - toetsaanslagen 120 door elke 300 ms
    
### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF-document - eenvoudige - uitvoeren voor ~ 610 seconden
Een gebruiker leest en Hiermee wordt gezocht in een PDF-document op verschillende manieren met behulp van Adobe Acrobat Reader. Het document moet bestaan uit de tabellen, eenvoudige grafieken en meerdere tekstlettertypen. Het document is 35-40 pagina's lang. De gebruiker door schuift op twee verschillende tarieven, achterwaartse en doorstuurt, op vier verschillende zoomen grootten (aanpassen aan pagina's, aanpassen aan de breedte, 100%, en een andere van uw keuze). De zoomen zorgt ervoor dat de tekst (lettertype) wordt weergegeven in verschillende grootten. Schuiven is niet beschikbaar voor het gebruik van de Page Up, Page Down, omhoog en omlaag toetsen, met verschillende intervallen voor elke scroll.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>PDF - gemengde - uitvoeren voor ~ 320 seconden document
Een gebruiker leest en Hiermee wordt gezocht in een PDF-document op verschillende manieren met behulp van Adobe Acrobat Reader. Het document bestaat uit de afbeeldingen van hoge kwaliteit (met inbegrip van foto's), tabellen, eenvoudige grafieken en meerdere tekstlettertypen. De gebruiker door schuift op twee verschillende tarieven, achterwaartse en doorstuurt, op vier verschillende zoomen grootten (aanpassen aan pagina's, aanpassen aan de breedte, 100%, en een andere van uw keuze). De zoomen zorgt ervoor dat de tekst (lettertype) wordt weergegeven in verschillende grootten. Schuiven is niet beschikbaar voor het gebruik van de Page Up, Page Down, omhoog en omlaag toetsen, met verschillende intervallen voor elke scroll.

### <a name="flash-video-playback---run-for-180-seconds"></a>Flash video's afspelen - uitvoeren voor ~ 180 seconden
Een gebruiker weergeeft een Adobe Flash-codering-video ingesloten in een pagina met webonderdelen. De pagina met webonderdelen is opgeslagen in de lokale harde schijf van de sessiehost-server. De video wordt afgespeeld in Internet Explorer door een ingesloten speler invoegtoepassing.

Dit scenario emuleert gebruikers uitgebreide inhoud webpagina's met multimedia bekijken. De meeste van de gegevens moet bo tot en met VOBR.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Externe typen voor Word - uitgevoerd voor ~ 1800 seconden
Een gebruiker een document via een RDP-sessie is getypt. Toetsaanslagen worden verzonden vanuit de client via de RDP-sessie aan een document in Microsoft Word uitgevoerd in de externe sessie. Het tarief weer dat Access is één teken elke 250 ms (totale 7050 tekens). 

Dit is een van de meest voorkomende scenario's voor een werknemer knowledge. Dit scenario Hiermee test u de reactiesnelheid van een gebruiker te typen in een processor moderne werk. Dit scenario is gevoelige even kleine wijzigingen in bandbreedtegebruik.

## <a name="tracking-the-test-results"></a>Het bijhouden van de testresultaten

U kunt de volgende tabel de scenario's in uw omgeving wordt berekend. De onderstaande gegevens is alleen betrekking heeft op afbeelding - kan zijn sterk anders uit dan wat u toekijken. 

Volgt de procedure waarmee we wordt ervan uitgegaan dat alle scenario's worden getest met dezelfde 1920 x 1080 pixels schermresolutie en TCP-transport in een netwerk met een latentie (vertraging) onder 200 ms en netwerk jitter in de markering 120 ms + van ongeveer 1%.

Over de tabel:
- **Gemiddelde ervaring** bevat de netwerkbandbreedte waar productiviteit van de gebruiker wordt niet aanzienlijk beïnvloed, maar niet incidentele video of audio voren uitsluit. Het systeem kan snel herstellen door te profiteren van de dynamische logica. De netwerk bandbreedte maakt een schatting poging om u te garanderen de kwaliteit van de gebruikerservaring.
 - **Noticeable problemen (einde punt)** bevat de netwerkbandbreedte waar gebruikers ervaart mogelijk aanzienlijk problemen in hun ervaring en hun productiviteit wordt beïnvloed voor gemeten perioden. Op dit moment de RDP-algoritmen hebben moeite en de kwaliteit van de gebruiker van ervaring vanwege onvoldoende netwerkbandbreedte kunnen niet garanderen.
 - **Aanbevolen** bevat de netwerkbandbreedte voor goed of uitstekend aanbevolen. Dit is meestal één stap hoger zijn dan de waarde in de bijbehorende **gemiddelde ervaring** kolom.
 - **Notities** zijn opmerkingen en opmerkingen.
 
| Test                  | Gemiddelde ervaring | Opvallend problemen (einde punt) | Aanbevolen netwerkbandbreedte | Notities                                                              |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| Executive/complex in PowerPoint | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s voorkeur    | Bij 1 MB/s gaan veel animaties verloren                                   |
| Eenvoudige in PowerPoint            | 5 MB/s              | 256 KB/s                         | 10 MB/s                        | Om 256 KB/s laden de dia's met opvallend vertraging                   |
| Internet Explorer     | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s voorkeur    | 1 MB/s web video's zijn onscherp en problemen schokkerig, snelle schuiven heeft |
| Eenvoudige PDF-bestand            | 1 MB/s              | 256 KB/s                         | 5 MB/s                         | Om 256 KB/s duurt het tijd om het laden van de pagina                       |
| Gemengde PDF-bestand             | 1 MB/s             | 256 KB/s                         | 5 MB/s                         | Op 256 KB/s neemt de pagina een aanzienlijke hoeveelheid tijd om te laden    |
| Flash video's afspelen  | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s voorkeur    | Bij 1 MB/s de video grof is en enkele frames worden niet weergegeven           |
| Externe typen voor Word    | 256 KB/s            | 128 KB/s                         | 1 MB/s                         | Gebruiker mogelijk om 256 KB/s de tijd tussen toetsaanslagen             |

Als u wilt evalueren de netwerkbandbreedte per gebruiker, maakt u een combinatie van de bovenstaande scenario's en de corresponderende gedeelte van de vereiste netwerkbandbreedte. Kies op het hoogste nummer dat u nodig hebt voor uw scenario's. Aangezien het systeem alleen wordt vrijwel nooit gebruikt door gebruikers, kunt u sommige reserveren voor gebruikers die zich werk tegelijkertijd aan hetzelfde netwerk samen.
     
## <a name="learn-more"></a>Meer informatie
- [Schatten van de Azure RemoteApp netwerkbandbreedte gebruikt](remoteapp-bandwidth.md)

- [Azure RemoteApp - hoe doet netwerkbandbreedte en de kwaliteit van zich werk samen?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp netwerkbandbreedte - algemene richtlijnen (als u niet kunt uw eigen testen)](remoteapp-bandwidthguidelines.md)