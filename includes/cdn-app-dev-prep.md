## <a name="prerequisites"></a>Vereisten voor

Voordat we CDN management code schrijven kunt, moeten we voorbereiding onze code om te communiceren met de resourcemanager van Azure inschakelen.  Hiervoor moet u:

* Een resourcegroep aanduiding van het we in deze zelfstudie maken CDN-profiel maken
* Azure Active Directory voor de verificatie voor de toepassing configureren
* Machtigingen toepassen op de resourcegroep, zodat alleen bevoegde gebruikers van onze Azure AD-tenant met onze CDN-profiel werken kunnen

### <a name="creating-the-resource-group"></a>De resourcegroep maken

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com).

2. Klik op de knop **Nieuw** in de linkerbovenhoek, en vervolgens **Management**en **Resourcegroep**.
    
    ![Een nieuwe resourcegroep maken](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)

3. Bel de resourcegroep *CdnConsoleTutorial*.  Selecteer uw abonnement en kies een locatie in de buurt.  Als u wilt, kunt u klikken op het selectievakje **vastmaken aan dashboard** als u wilt vastmaken aan het dashboard in de portal.  Hierdoor wordt u later gemakkelijk te vinden.  Nadat u uw keuze hebt gemaakt, klikt u op **maken**.

    ![Naamgeving van de resourcegroep](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)

4. Nadat de resourcegroep is gemaakt, als u geen aan uw dashboard vastmaken, kunt u deze kunt vinden door te klikken op **Bladeren**en vervolgens **Resourcegroepen**.  Klik op de resourcegroep om deze te openen.  Maak een notitie voor uw **Abonnements-ID**.  Moet u deze later.

    ![Naamgeving van de resourcegroep](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Maken van de Azure AD-toepassing en het toekennen van machtigingen

Er zijn twee mogelijkheden voor verificatie van de app met Azure Active Directory: individuele gebruikers of een service principal. Een service principal is vergelijkbaar met een serviceaccount in Windows.  In plaats van een bepaalde gebruiker toewijzen van machtigingen om te communiceren met de CDN-profielen, wordt in plaats daarvan de machtigingen verlenen voor de service principal.  Service principes worden gewoonlijk gebruikt voor geautomatiseerde, niet-interactieve processen.  Hoewel deze zelfstudie een interactieve console-app schrijven is, richten we ons op de hoofdsom service-methode.

Maken van een service principal bestaat uit meerdere stappen, waaronder een Azure Active Directory-toepassing maken.  Klik hiertoe gaan we [deze zelfstudie](../articles/resource-group-create-service-principal-portal.md)te volgen.

> [AZURE.IMPORTANT] Zorg ervoor dat u de stappen in de [gekoppelde zelfstudie](../articles/resource-group-create-service-principal-portal.md)te volgen.  Het is *zeer belangrijk* dat u deze precies zoals wordt beschreven voltooit.  Zorg ervoor dat uw **tenant-ID**, **de domeinnaam tenant** Opmerking (meestal een *. onmicrosoft.com* domein tenzij u een aangepast domein hebt opgegeven), **client-ID**en **client verificatie-toets**heeft, zoals we deze later moeten.  Let zeer beschermen uw **klant-ID** en de **client verificatie-toets**heeft, zoals deze referenties kunnen worden gebruikt door iedereen bewerkingen uitvoeren als de service-principal. 
>   
> Wanneer u toegang hebt tot de stap die naam [configureren met meerdere tenant application](../articles/resource-group-create-service-principal-portal.md#configure-multi-tenant-application), selecteert u **Nee**.
> 
> Wanneer u toegang hebt tot de stap [toepassing aan rol toewijzen](../articles/resource-group-create-service-principal-portal.md#assign-application-to-role), gebruikt u de resourcegroep die we eerder hebt gemaakt, *CdnConsoleTutorial*, maar in plaats van de rol van de **lezer** , toewijzen de rol **Inzender voor CDN-profiel** .  Nadat u de toepassing de **CDN profiel Inzender** rol op de resourcegroep toewijzen, terug naar deze zelfstudie. 

Nadat u uw service principal hebt gemaakt en toegewezen de rol **CDN profiel Inzender** , het blad **gebruikers** voor de resourcegroep ziet er ongeveer als volgt.

![Gebruikers blade](./media/cdn-app-dev-prep/cdn-service-principal-include.png)


### <a name="interactive-user-authentication"></a>Interactieve gebruikersverificatie

Als u liever interactieve afzonderlijke gebruikersverificatie in plaats van een service-principal, hebben wilt, is het proces is vergelijkbaar met die voor een service principal.  U moet in feite Voer dezelfde procedure uit, maar een paar kleine wijzigingen kunt aanbrengen.

> [AZURE.IMPORTANT] Volg deze stappen alleen als u kiest voor het gebruik van afzonderlijke gebruikersverificatie in plaats van een service principal.

1. Bij het maken van uw toepassing, in plaats van de **Webtoepassing**, kiest u de **bijbehorende toepassing**. 
    
    ![Oorspronkelijke toepassing](./media/cdn-app-dev-prep/cdn-native-application-include.png)
    
2. Op de volgende pagina, wordt u gevraagd voor een **URI omleiden**.  De URI won't worden gevalideerd, maar onthoud wat u hebt ingevoerd.  U moet deze later. 

3. Er is geen hoeft te maken van een **client verificatie-toets**.

4. In plaats van voor meer informatie over het toewijzen van een service principal aan de rol **Inzender voor CDN-profiel** gaan we individuele gebruikers of groepen toewijzen.  In dit voorbeeld ziet u dat ik *CDN Demo gebruiker* hebt toegewezen aan de rol **Inzender voor CDN-profiel** .  
    
    ![De toegang van individuele gebruikers](./media/cdn-app-dev-prep/cdn-aad-user-include.png)

