####<a name="configuring-the-ios-project-in-xamarin-studio"></a>Het project iOS configureren in Xamarin Studio

1. Xamarin.Studio, open **Info.plist**en bijwerken van de **Bundel-id** aan de bundel-ID die u eerder hebt gemaakt met uw nieuwe App-ID.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)

2. Schuif omlaag naar de **Achtergrond modi** en controleer het vak **Achtergrond modi inschakelen** en het **externe berichten** in. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)

3. Dubbelklik in uw project in het deelvenster oplossing om **Project-opties**te openen.

4.  Kies **iOS bundel ondertekening** onder **maken**en selecteer de bijbehorende **identiteit** en **Provisioning profiel** u zojuist hebt ingesteld voor dit project. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

    Dit zorgt ervoor dat het project wordt gebruikt voor het nieuwe profiel voor het ondertekenen van de code. Zie de [Inrichting van Xamarin-apparaat]voor het officiële Xamarin apparaat inrichting documentatie.

####<a name="configuring-the-ios-project-in-visual-studio"></a>Het project iOS configureren in Visual Studio

1. Visual Studio, met de rechtermuisknop op het project en klik vervolgens op **Eigenschappen**.

2. Eigenschappen van pagina's, klik op het tabblad **iOS toepassing** en de **id** bijwerken aan de ID die u eerder hebt gemaakt.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)

3. Selecteer in het tabblad **iOS bundel ondertekening** de bijbehorende **identiteit** en **Provisioning profiel** u zojuist hebt ingesteld voor dit project. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Dit zorgt ervoor dat het project wordt gebruikt voor het nieuwe profiel voor het ondertekenen van de code. Zie de [Inrichting van Xamarin-apparaat]voor het officiële Xamarin apparaat inrichting documentatie.

4. Dubbelklikken Info.plist te openen en vervolgens **RemoteNotifications** onder achtergrond modi inschakelen. 



[Xamarin apparaat inrichting]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/