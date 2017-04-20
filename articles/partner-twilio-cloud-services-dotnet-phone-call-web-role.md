<properties 
    pageTitle="Hoe u een telefoongesprek starten vanuit Twilio (.NET) | Microsoft Azure" 
    description="Leer hoe u een telefoongesprek starten en het verzenden van een SMS-bericht met de Twilio API-service op Azure. Voorbeelden van de code in .NET geschreven." 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/04/2016" 
    ms.author="microsofthelp@twilio.com"/>




# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Hoe u een telefoongesprek starten Twilio gebruiken in een Webrol op Azure

Deze handleiding wordt aangegeven hoe Twilio gebruiken om een gesprek te vanaf een webpagina die wordt gehost in Azure wordt aangegeven. De resulterende toepassing wordt de gebruiker voor telefoongesprek waarden, zoals wordt weergegeven in de volgende schermafbeelding.

![Azure gesprek formulier met Twilio en ASP.NET][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Vereisten voor

U moet als volgt de code gebruiken in dit onderwerp:

1. In het bezit van een Twilio-account en verificatie token. Als u wilt beginnen met Twilio, moet u zich aanmeldt bij [https://www.twilio.com/try-twilio][try_twilio]. U kunt evalueren prijzen bij [http://www.twilio.com/pricing][twilio_pricing]. Zie voor informatie over de API die is verstrekt door Twilio [http://www.twilio.com/voice/api][twilio_api].
2. De bibliotheek Twilio .NET toevoegen aan uw Webrol. Zie ' de bibliotheken Twilio toevoegen aan uw project web rol"verderop in dit onderwerp.

U moet vertrouwd zijn met het maken van een eenvoudige Webrol op Azure.

## <a name="howtocreateform"></a>Hoe u: een webformulier voor het maken van een gesprek maken

<a id="use_nuget"></a>De bibliotheken Twilio toevoegen aan uw project van de rol web:

1.  Open uw oplossing in Visual Studio.
2.  Met de rechtermuisknop op **verwijzingen**.
3.  Klik op **NuGet-pakketten beheren**.
4.  Klik op **Online**.
5.  Typ in het zoekvak online *twilio*.
6.  Klik op het pakket Twilio op **installeren** .

De volgende code ziet hoe u een webformulier om op te halen gebruikersgegevens voor u een oproep plaatst maken. In dit voorbeeld wordt een ASP.NET-web-functie met de naam **TwilioCloud** gemaakt.

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a id="howtocreatecode"></a>Hoe u: de code als u het gesprek wilt maken
De volgende code die wordt aangeroepen wanneer de gebruiker het formulier is voltooid, maakt het oproep-bericht en genereert het gesprek. In dit voorbeeld wordt de code in de gebeurtenis-handler onclick van de knop uitvoeren op het formulier. (Gebruik uw Twilio-account en verificatie in plaats van de tijdelijke aanduiding voor waarden die zijn toegewezen aan **accountSID** en **authToken** in de onderstaande code token.)

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

De oproep wordt uitgevoerd en het eindpunt Twilio, API-versie, en de status van het gesprek worden weergegeven. De volgende schermafbeelding ziet u uitvoer van een steekproef uitvoeren.

![Azure gesprek antwoord met Twilio en ASP.NET][twilio_dotnet_basic_form_output]

Meer informatie over TwiML kunt vinden op [http://www.twilio.com/docs/api/twiml][twiml]. Meer informatie over &lt;zeg&gt; en andere bewerkingen Twilio kunnen u vinden op [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Volgende stappen
Deze code is opgegeven om aan te geven basisfunctionaliteit Twilio gebruiken in een ASP.NET-web-beheerdersrol op Azure. Voordat u de implementatie in Azure in productie, kunt u meer foutafhandeling of andere onderdelen toevoegen. Bijvoorbeeld:

* In plaats van een webformulier gebruikt, kunt u Azure-blobopslag of een exemplaar van de Azure SQL-Database kunt gebruiken voor het opslaan van telefoonnummers en tekst te bellen. Zie voor informatie over het gebruik van BLOB's in Azure [het gebruik van de Azure Blob storage-service in .NET][howto_blob_storage_dotnet]. Zie voor informatie over het gebruik van de SQL-Database [het gebruik van Azure SQL-Database in .NET-toepassingen][howto_sql_azure_dotnet].
* U kunt voor het ophalen de Twilio account-ID en verificatietoken RoleEnvironment.getConfigurationSettings uit van uw implementatie configuratie-instellingen, in plaats van de harde codering de waarden in een formulier. Zie voor informatie over de klasse RoleEnvironment [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Lees de richtlijnen van de beveiliging Twilio bij [https://www.twilio.com/docs/security][twilio_docs_security].
* Meer informatie over Twilio bij [https://www.twilio.com/docs][twilio_docs].

##<a name="seealso"></a>Zie ook
* [Het gebruik van Twilio voor spraak- en SMS mogelijkheden van Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
