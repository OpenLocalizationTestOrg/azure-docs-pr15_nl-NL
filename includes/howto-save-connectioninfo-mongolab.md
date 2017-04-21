Het is mogelijk een MongoLab URI in uw code plakken, wordt u aangeraden configureren in de omgeving voor eenvoudige van management. Op deze manier als de URI wijzigt, kunt u bijwerken deze via de Portal Azure zonder naar de code te gaan.


1. Selecteer in de Portal Azure **Web Apps**.
1. Klik op de naam van de web-app in de lijst met Web Apps.  
![WebAppEntry][entry-website]  
Het dashboard Web App wordt weergegeven.

1. Klik op **configureren** op de menubalk.  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. Schuif omlaag naar de sectie tekenreeksen met de verbinding.  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. Voer voor de **naam**MONGOLAB_URI.
1. Plak de verbindingsreeks die we in de vorige sectie verkregen voor **waarde**.
1. Selecteer **aangepast** in de vervolgkeuzelijst **Type** (in plaats van de standaardinstelling **SQLAzure**).
1. Klik op **Opslaan** op de werkbalk.  
![SaveWebApp][button-website-save]

**Notitie:** Azure voegt de **CUSTOMCONNSTR\_ ** naar deze variabele, dat wil waarom zeggen aanduiding voor de bovenstaande verwijzingen code **CUSTOMCONNSTR\_MONGOLAB_URI.**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png
