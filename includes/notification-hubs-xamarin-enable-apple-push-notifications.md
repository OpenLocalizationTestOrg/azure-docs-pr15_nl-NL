

Als u wilt de app voor push-meldingen tot en met Apple Push Notification Service (APNS) hebt geregistreerd, moet u een nieuw push-certificaat, App-ID en inrichten profiel voor het project op van Apple developer portal maken. De App-ID bevat de configuratie-instellingen waarmee uw app verzenden en ontvangen van push-meldingen inschakelen. Deze instellingen bevat het push-meldingen certificaat nodig om te verifiëren met Apple Push Notification Service (APNS) bij het verzenden en ontvangen van push-meldingen. Zie de officiële [Apple Push Notification-Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) -documentatie voor meer informatie over deze concepten.


####<a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Het bestand certificaat ondertekening aanvragen voor het certificaat push genereren

Deze stappen begeleiden u bij het maken van de aanvraag voor Certificaatondertekening. Hiermee worden gebruikt om een push-certificaat moet worden gebruikt met APNS te genereren.

1. Op uw Mac, voert u het hulpprogramma Sleutelhangertoegang. Dit kan worden geopend vanuit het **hulpprogramma's** of de **andere** map in het pad starten.

2. Klik op **Sleutelhangertoegang**, uitvouwen **Assistent certificaat**en klik op **een certificaat aanvragen bij een certificeringsinstantie...**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. Selecteer uw **E-mailadres gebruiker** en **De naam van de algemene** , zorg dat **opgeslagen op de schijf** is ingeschakeld en klik vervolgens op **Doorgaan**. Laat het veld **E-mailadres CA** leeg, als dit niet nodig is.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Typ een naam voor het certificaat ondertekening aanvragen (CSR)-bestand in het vak **OpslaanAls**, selecteert u de locatie **waar**in en klik op **Opslaan**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Hiermee wordt het CSR-bestand opgeslagen in de geselecteerde locatie; de standaardlocatie is op het bureaublad. Onthoud dat de locatie voor dit bestand gekozen.


####<a name="register-your-app-for-push-notifications"></a>Uw app voor push-meldingen hebt geregistreerd

Een nieuwe expliciete App-ID voor uw toepassing maken met Apple en configureer deze ook voor push-meldingen.  

1. Navigeer naar de [iOS Provisioning-Portal](http://go.microsoft.com/fwlink/p/?LinkId=272456) op het Apple Developer Center Meld u aan met uw Apple-ID, klikt u op **id's**, en vervolgens op **App-id's**, en ten slotte op op de **+** meldt u zich bij het registreren van een nieuwe app.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Pas de volgende drie velden voor de nieuwe app en klik vervolgens op **Doorgaan**:

    * **Naam**: Typ een beschrijvende naam voor de app in het veld **naam** in de sectie **Beschrijving van de App-ID** .

    * **Bundel-id**: Voer onder de sectie **Expliciete App-ID** een **Bundel-id** in het formulier `<Organization Identifier>.<Product Name>` zoals vermeld in de [Handleiding voor App-verdeling](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Dit moet overeenkomen met wat wordt ook gebruikt in het project XCode, Xamarin of Cordova voor de app.

    * **Pushmeldingen**: Schakel de optie **Push-meldingen** in het gedeelte **App-Services** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

3.  Klik op de pagina bevestig uw App-ID-scherm, bekijk de instelling en nadat u hebt gecontroleerd op deze **verzenden**

4.  Zodra u de nieuwe App-ID hebt verzonden, ziet u het **volledige** scherm. Klik op **Gereed**.

5. Zoek de app-ID die u zojuist hebt gemaakt en klik op de rij in het Developer Center, onder de App-id's. Klikken op de app-ID-rij, wordt de appdetails weergegeven. Klik op de knop **bewerken** onderaan.

6. Schuif naar de onderkant van het scherm en klik op de knop **Certificaat maken** onder de sectie **Ontwikkeling Push SSL-certificaat**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    De assistent 'IOS certificaat toevoegen' wordt weergegeven.

    > [AZURE.NOTE] Deze zelfstudie gebruikt een certificaat ontwikkeling. Hetzelfde proces wordt gebruikt wanneer een productiecertificaat registreren. Zorg ervoor dat u hetzelfde certificaattype gebruiken bij het verzenden van meldingen.

7. Klik op **Bestand kiezen**, blader naar de locatie waar u de CSR voor uw push-certificaat opgeslagen. Klik vervolgens op **genereren**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

8. Nadat het certificaat is gemaakt met de portal, klikt u op de knop **downloaden** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Dit downloads van het handtekeningcertificaat en slaat het naar uw computer in uw map Downloads.

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] Standaard het gedownloade bestand een certificaat ontwikkeling heet **aps_development.cer**.

9. Dubbelklik op de gedownloade push certificate **aps_development.cer**. Hiermee installeert u het nieuwe certificaat in de sleutelhanger, zoals hieronder wordt weergegeven:

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] De naam in het certificaat kan afwijken, maar deze wordt voorafgegaan door **ontwikkeling van Apple iOS Push Services:**.

10. Sleutelhangertoegang, met de rechtermuisknop op het nieuwe push-certificaat dat u zojuist hebt gemaakt in de categorie **certificaten** . Klik op **exporteren**, geef het bestand een naam, de **.p12** selecteren en klik vervolgens op **Opslaan**.

    Onthoud dat de bestandsnaam en de locatie van het geëxporteerde .p12 push-certificaat. Deze worden gebruikt met APNS-verificatie inschakelen te uploaden naar de klassieke Azure-Portal.



####<a name="create-a-provisioning-profile-for-the-app"></a>Een inrichten profiel voor de app maken

1. Terug in de <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning-Portal</a>, selecteert u **Inrichting profielen**, selecteert u **alle**en klik vervolgens op de **+** om te maken van een nieuw profiel. Hiermee wordt de Wizard **toevoegen iOS Provisiong profiel** gestart

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. **IOS ontwikkelen van apps** in **ontwikkeling** selecteert als het type van de profiel provisiong en klikt u op **Doorgaan**.


3. Vervolgens selecteert u de app-ID die u zojuist hebt gemaakt in de vervolgkeuzelijst van de **App-ID** en klikt u op **Doorgaan**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4. Selecteer uw development-certificaat dat is gebruikt voor het ondertekenen van code in het scherm **selecteren van certificaten** en klikt u op **Doorgaan**. Dit is een ondertekend digitaal certificaat, niet de push-certificaat dat wordt u zojuist hebt gemaakt.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. Selecteer vervolgens de **apparaten** wilt gebruiken voor het testen van, en klikt u op **Doorgaan**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. Ten slotte, een naam voor het profiel kiezen in de **Naam van een profiel**, klik op **genereren**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
