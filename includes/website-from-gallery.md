De Azure Marketplace beschikbaar een breed scala van populaire web-apps die zijn ontwikkeld door Microsoft en derden bedrijven bron openen software initiatieven. Installatie van software dan de browser waarmee verbinding met de [Portal van Azure Preview](http://go.microsoft.com/fwlink/?LinkId=529715)nodig niet is gemaakt op basis van de Azure Marketplace-Webapps. 

In deze zelfstudie leert u hoe:

- Het maken van een nieuwe WebApp via de Azure Marketplace.

- Het implementeren van de web-app via de Portal van Azure Preview.
 
U kunt een WordPress-blog waarin een standaardsjabloon maken. De volgende afbeelding ziet u de voltooide toepassing:


![WordPress-blog][13]

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="create-a-web-app-in-the-portal"></a>Een web-app in de beheerportal maken

1. Meld u aan bij de Portal Azure Preview.

2. Open de Azure Marketplace door te klikken op het pictogram **Marketplace** :

    ![Marketplace-pictogram][marketplace]

    Of door te klikken op het pictogram **Nieuw** in de rechterbovenhoek van het dashboard en **Marketplace** bij de bottow van de lijst te selecteren.
    
    ![Nieuwe maken][5]
    
3. Selecteer **Web & Mobile**. Zoeken naar **WordPress** en klik op het pictogram **WordPress** .

    ![WordPress in lijst][7]
    
5. Lees de beschrijving van de app WordPress en selecteer **maken**.

6. Klik op **WebApp**en geef de vereiste waarden voor het configureren van uw web-app.
    
    ![uw app configureren][8]

7. Klik op **Database**en geef de vereiste waarden voor het configureren van uw MySQL-database. 

    ![database configureren][database]

8. Geef een naam voor een nieuwe resourcegroep.

    ![Resourcegroep instellen][groupname]

8. Zo nodig op **abonnement**en geef het abonnement te gebruiken. 

7. Wanneer u klaar bent met het definiëren van de web-app, klikt u op **maken**en wacht tot de nieuwe web-app is gemaakt.

   Wanneer de app is gemaakt, ziet u de resourcegroep met WebApp en database.

   ![de groep weergeven][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Starten en beheren van uw WordPress web-app
    
1. Klik op de nieuwe WebApp informatie over uw app wilt weergeven.

    ![dashboard starten][10]

2. Klik op de pagina **Essentials** **Bladeren** of de koppeling onder de **Url** naar de welkomstpagina van de web-app te openen.

    ![site-URL][browse]

3. Als u WordPress niet hebt geïnstalleerd, voert u de gegevens van de juiste configuratie vereist WordPress en **WordPress installeren** als u wilt voltooien configuratie en open de web-app login pagina klikt u op.

4. Klik op **aanmelden** en voert u uw referenties.  

5. Er wordt een nieuwe WordPress web-app, dat op de onderstaande web-app lijkt.    

    ![uw site WordPress][13]






[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png
