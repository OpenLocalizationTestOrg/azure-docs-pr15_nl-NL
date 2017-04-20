
1. Klik in Visual Studio Solution Explorer met de rechtermuisknop op de Windows Store-app-project, klikt u op **Store** > **App koppelen in de Store...**.

    ![Windows Store app koppelen](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. In de wizard op **volgende**, meld u aan met uw Microsoft-account, typ een naam voor de app in **reserveren een nieuwe appnaam**en klik op **reserveren**.

3. Nadat de registratie van de app is gemaakt, selecteert u de naam van de nieuwe app, klik op **volgende**en klik vervolgens op **koppelen**. Hiermee wordt de vereiste gegevens van de Windows Store-registratie toegevoegd aan manifest van de toepassing.

7. Herhaal stap 1 en 3 voor de Windows Phone Store-app-project met behulp van de dezelfde registratie die u eerder hebt gemaakt voor de Windows Store-app.  

7. Ga naar het [Windows ontwikkelaar Center](https://dev.windows.com/en-us/overview), aanmelden met uw Microsoft-account, op de nieuwe app registratie in **Mijn apps**en **Services**uitvouwen > **Push-meldingen**.

8. **Push-meldingen** op de pagina **Services Live-site** op onder **Windows Push-meldingen Services (WNS) en Microsoft Azure Mobile-Apps**en noteer de waarden van het **Pakket beveiligings-id** en de *huidige* waarde in de **Toepassing geheim**. 

    ![App-instelling in het developer center](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] De toepassing geheim en pakket beveiligings-id zijn belangrijk beveiligingsreferenties. Deze waarden met iedereen delen of verdelen met uw app niet.
