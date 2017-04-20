<properties 
    pageTitle="Azure mobiele betrokkenheid - backend-integratie" 
    description="Azure Mobile betrokkenheid verbinding met een SharePoint-end maken van campagnes van SharePoint" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement---api-integration"></a>Azure mobiele betrokkenheid - API-integratie

In een geautomatiseerde marketing-systeem, maken en de marketingcampagnes activeren ook worden automatisch uitgevoerd. Voor dit doel - Azure Mobile betrokkenheid kunt maken van dergelijke geautomatiseerde marketingcampagnes API's ook gebruiken. 

Klanten gebruiken meestal de interface van de front-end van Mobile betrokkenheid aankondigingen/polls enzovoort als onderdeel van hun marketingcampagnes maken. Echter zodra de marketingcampagnes volwaardige, is een nodig om te profiteren van de gegevens in de backend-systemen (zoals een CRM of CMS-systeem, zoals SharePoint) vergrendeld, zodat een volledig geautomatiseerde pijplijn kan worden gemaakt waarin campagnes in Mobile-betrokkenheid dynamisch op basis van de gegevens die doorloopt in van de backend-systems maakt. 

![][5]

Deze zelfstudie doorloopt naar een scenario waarbij een SharePoint-zakelijke gebruiker wordt gevuld met marketing gegevens in een SharePoint-lijst en een geautomatiseerde proces items uit de lijst wordt opgehaald en maakt een verbinding met het mobiele betrokkenheid-systeem met alle beschikbare REST API's een marketingcampagne maken van de SharePoint-gegevens. 
 
> [AZURE.IMPORTANT] U kunt in het algemeen, in dit voorbeeld gebruiken als uitgangspunt voor informatie over hoe u een mobiele betrokkenheid REST API bellen terwijl deze een gedetailleerd overzicht van de twee belangrijkste aspecten van het aanroepen van de API's - verificatie en doorgeven van parameters. 

## <a name="sharepoint-integration"></a>SharePoint-integratie
1. Hier ziet u hoe de SharePoint-lijst steekproef eruitziet. **Titel**, **categorie**, **NotificationTitle**, **bericht** en de **URL** worden voor het maken van de aankondiging gebruikt. Er is een kolom met de naam van **IsProcessed** die wordt gebruikt door het proces van de steekproef automatisering in de vorm van een consoleprogramma. U kunt uitvoeren deze console programma als een WebJob Azure zodat u deze kunt plannen of u kunt rechtstreeks naar programma maken en de aankondiging activeren wanneer een item in de SharePoint-lijst is ingevoegd in de SharePoint-werkstroom. In dit voorbeeld gebruiken we het consoleprogramma welke doorloopt de items in de SharePoint-lijst en aankondiging maken in Azure Mobile betrokkenheid voor elk van deze en klik ten slotte Hiermee markeert u de vlag **IsProcessed** succesvolle aankondiging maken waar zijn.

    ![][1]

2. We gebruiken de code uit de voorbeeld- *externe verificatie in SharePoint Online via het objectmodel Client* [hier](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) om te verifiëren met de SharePoint-lijst.
 
3. Als geverifieerd, we aan de items in de lijst om vast te stellen gemaakte items doorlopen (waarin **IsProcessed** heeft = ONWAAR). 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Mobile betrokkenheid-integratie
1.  Als we een item dat is vereist verwerking - gevonden we de gegevens die zijn vereist voor het maken van een aankondiging van het lijstitem en gesprek extraheren `CreateAzMECampaign` moet maken en vervolgens `ActivateAzMECampaign` te activeren. Hierna ziet u in feite REST API oproepen inbelt om de betrokkenheid van de Mobile-end. 

2.  De mobiele betrokkenheid REST API's vereisen een **eenvoudige auth kleurenschema autorisatie HTTP kop** die bestaan uit de `ApplicationId` en de `ApiKey` waarvoor u lid van de Azure-portal. Zorg ervoor dat u de sleutel in de sectie met **api toetsen** worden gebruikt en *niet* in de sectie met **sdk toetsen** . 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. Voor het maken van de aankondiging type campagne - verwijzen naar de [documentatie](https://msdn.microsoft.com/library/azure/mt683750.aspx). U nodig hebt om ervoor te zorgen dat u de campagne opgeeft `kind` als *aankondiging* en de [nettolading](https://msdn.microsoft.com/library/azure/mt683751.aspx) en deze wordt doorgegeven als FormUrlEncodedContent. 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. Nadat u de aankondiging die is gemaakt hebt, worden er ongeveer als volgt te werk op de betrokkenheid van de Mobile-portal (Houd er rekening mee dat de status concept en geactiveerd = = n/b)

    ![][3]

5. `CreateAzMECampaign`Hiermee maakt u een aankondiging campagne en geeft als resultaat de Id tot de beller. `ActivateAzMECampaign`deze-Id vereist als de parameter voor het activeren van de campagne. 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. Nadat u de aankondiging geactiveerd hebt, worden er ongeveer als volgt te werk op de betrokkenheid van de Mobile-portal:

    ![][4]

7. Zodra de campagne wordt geactiveerd, begint de apparaten die voldoen aan het criterium van de campagne meldingen te zien. 

8. U ziet ook dat het lijstitem gemarkeerd met IsProcessed = ONWAAR is ingesteld op waar nadat de campagne aankondiging is gemaakt.  

In dit voorbeeld is gemaakt van een eenvoudige aankondiging campagne voornamelijk de vereiste eigenschappen opgeven. U kunt dit aanpassen zoveel u via de portal kunt met behulp van de informatie [hier](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 
