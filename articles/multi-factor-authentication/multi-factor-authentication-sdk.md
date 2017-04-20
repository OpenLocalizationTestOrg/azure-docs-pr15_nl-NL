<properties 
    pageTitle="Uw on-premises implementatie identiteiten integreren met Azure Active Directory."
    description="Dit is de Azure AD Connect waarmee wordt beschreven wat is en waarom u deze wilt gebruiken."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Building meervoudige verificatie in aangepaste Apps (SDK)

> [AZURE.IMPORTANT]  Als u de SDK downloaden wilt, moet u een Provider van Azure meervoudige Auth maken, zelfs als er Azure MFA, AAD Premium of EMS licenties.  Als u een Provider van Azure meervoudige Auth voor dit doel maken en licenties al hebt, is verplicht voor het maken van de Provider met het model **Per gebruiker is ingeschakeld** en de Provider koppelen aan de map waarin de licenties Azure MFA, Azure AD Premium of EMS.  Dit zorgt ervoor dat dat u bent niet gefactureerd tenzij u hebt meer unieke gebruikers met de SDK dan het aantal licenties dat u eigenaar bent.

De Azure meervoudige verificatie Software Development Kit (SDK) kunt u bouwen telefoongesprek en tekst bericht verificatie rechtstreeks in de processen aanmelden of transactie van toepassingen in uw Azure AD-tenant.

De meervoudige verificatie SDK is beschikbaar voor C#, Visual Basic (.NET), Java, Perl, PHP en Ruby. De SDK biedt een dunne Wikkel rond meervoudige verificatie. Het bevat alles wat die u nodig hebt om uw code, inclusief bestanden met opmerkingen broncode, voorbeeldbestanden en een gedetailleerde Leesmij-bestand te schrijven. Elke SDK bevat ook een certificaat en persoonlijke sleutel voor het coderen van transacties die uniek is voor uw Provider van meervoudige verificatie. Zo lang maken als u een provider hebt, kunt u de SDK in zo veel talen en indelingen downloaden als u nodig hebt.

De structuur van de API in de meervoudige verificatie SDK is heel eenvoudig. U één functie belt u bij een API met de optie voor meervoudige parameters, zoals de verificatie-modus en gebruikersgegevens, zoals het telefoonnummer bellen of de pincode om deze te valideren. De API's vertalen de oproep door functie in serviceaanvragen web naar de cloud gebaseerde Azure meervoudige verificatie-Service. Alle gesprekken moeten een verwijzing naar het persoonlijk certificaat dat is opgenomen in elke SDK bevatten.

Omdat de API's geen toegang tot de gebruikers die zijn geregistreerd in Azure Active Directory, moet u gebruikersgegevens, zoals telefoonnummers en PIN-codes in een bestand of de database opgeven. Bovendien bieden de API's geen beheerfuncties voor registratie of de gebruiker, dus u hoeft te maken van deze processen in uw toepassing.






## <a name="download-the-azure-multi-factor-authentication-sdk"></a>Downloaden van de Azure meervoudige verificatie SDK

Downloaden van de Azure meervoudige SDK, is een [Azure meervoudige Auth Provider](multi-factor-authentication-get-started-auth-provider.md)vereist.  Hiervoor een volledige Azure abonnement, zelfs als het eigendom van Azure MFA, Azure AD Premium of Enterprise mobiliteit Suite licenties zijn.  Als u wilt downloaden van de SDK moet u naar de beheerportal meervoudige navigeren door de Provider voor meervoudige Auth rechtstreeks beheren of door te klikken op de koppeling **'Ga naar de portal'** op de instellingenpagina van MFA-service.


### <a name="to-download-the-azure-multi-factor-authentication-sdk-from-the-azure-portal"></a>Om te downloaden van de SDK van Azure meervoudige verificatie van de Azure-portal


1. Meld u aan bij de Azure-Portal als beheerder.
2. Selecteer aan de linkerkant, Active Directory.
3. Klik op de pagina Active Directory boven op **Meervoudige Auth Providers**
4. Klik onderaan op **beheren**
5. Hiermee opent u een nieuwe pagina.  Aan de linkerkant, onder, klikt u op SDK.
<center>![Downloaden](./media/multi-factor-authentication-sdk/download.png)</center>
6. Selecteer de gewenste taal en klik op een van de downloadkoppelingen gekoppeld.
7. Sla het downloaden.



### <a name="to-download-the-azure-multi-factor-authentication-sdk-via-the-service-settings"></a>Downloaden van de SDK Azure meervoudige verificatie via de service-instellingen


1. Meld u aan bij de Azure-Portal als beheerder.
2. Selecteer aan de linkerkant, Active Directory.
3. Dubbelklikt u op uw exemplaar van Azure AD.
4. Klik op boven **configureren**
5. Selecteer **de service-instellingen beheren**onder meervoudige verificatie
![downloaden](./media/multi-factor-authentication-sdk/download2.png)
6. Klik op de servicepagina-instellingen onder aan het scherm op **Ga naar de portal**.
![Downloaden](./media/multi-factor-authentication-sdk/download3a.png)
7. Hiermee opent u een nieuwe pagina.  Aan de linkerkant, onder, klikt u op SDK.
8. Selecteer de gewenste taal en klik op een van de downloadkoppelingen gekoppeld.
9. Sla het downloaden.

## <a name="contents-of-the-azure-multi-factor-authentication-sdk"></a>Inhoud van de Azure meervoudige verificatie SDK
In de SDK vindt u de volgende items:

- **Leesmij voor**. Wordt uitgelegd hoe u de meervoudige verificatie-API's in een nieuw of bestaand-toepassing.
- **Bron-bestanden die** voor meervoudige verificatie
- **Clientcertificaat** dat u gebruikt om te communiceren met de meervoudige verificatie-service
- **Persoonlijke sleutel** voor het certificaat
- **Bel de resultaten.** Een lijst met gesprek resultaatcodes. Als u wilt dit bestand niet openen, een toepassing met tekst met opmaak, zoals WordPad te gebruiken. De oproep resultaatcodes gebruiken om te testen en problemen met de implementatie van meervoudige verificatie in uw toepassing. Ze zijn niet verificatie statuscodes.
- **Voorbeelden.** Voorbeeld van de code voor een eenvoudige werken-implementatie van meervoudige verificatie.


>[AZURE.WARNING]Dit certificaat is een unieke privé-certificaat dat is gegenereerd met name voor u. Delen of gaan verloren dit bestand niet. Is uw sleutel om te zorgen dat de beveiliging van uw communicatie met de meervoudige verificatie-service.

## <a name="code-sample-standard-mode-phone-verification"></a>Codevoorbeeld: Verificatie via de telefoon van standaardmodus

In dit voorbeeld ziet u het gebruik van de API's in de SDK van Azure meervoudige verificatie standaardmodus voice gesprek verificatie toevoegen aan uw toepassing. Standaardmodus is een telefoongesprek die de gebruiker moet reageren op door de #-toets te drukken.

In dit voorbeeld wordt de C# .NET 2.0 meervoudige verificatie SDK in een eenvoudige ASP.NET-toepassing met C# serverzijde logica, maar het proces is vergelijkbaar voor eenvoudige implementaties in andere talen. Omdat de SDK bronbestanden, niet uitvoerbare bestanden bevat, kunt u de bestanden maken en ze verwijzen naar of deze rechtstreeks in uw toepassing opnemen.

>[AZURE.NOTE]Bij het implementeren van meervoudige verificatie, gebruikt u de extra factoren als secundaire of tertiaire verificatie aanvulling op uw primaire verificatiemethode. Deze methoden zijn niet ontworpen om te worden gebruikt als primaire verificatiemethoden.

### <a name="code-sample-overview"></a>Overzicht van de steekproef code
In dit voorbeeldcode voor een zeer eenvoudige webtoepassing demo wordt een telefoongesprek met een reactie # gebruikt om te voltooien van de gebruiker verificatie. Deze factor telefoongesprek wordt genoemd in meervoudige verificatie standaardmodus.

De code aan de clientzijde is niet inbegrepen voor de elementen van een meervoudige verificatie / regiospecifieke. Omdat de aanvullende verificatievragen factoren onafhankelijk van de primaire verificatie, kunt u deze toevoegen zonder te wijzigen van de bestaande interface voor eenmalige aanmelding. De API's in de meervoudige-SDK kunt u de gebruikerservaring aanpassen, maar moet u mogelijk niet op alle wijzigingen aanbrengen.

De code aan de clientzijde worden standaard-verificatiemodus in stap 2 opgeteld. Deze Hiermee maakt u een object PfAuthParams met de parameters die vereist voor de verificatie van de standaard-modus zijn: gebruikersnaam, getal, en modus en het pad naar het clientcertificaat (CertFilePath), die is vereist in elk gesprek telefoon. Zie voor een demonstratie van alle parameters in PfAuthParams, het voorbeeld van een bestand in de SDK.

De code wordt vervolgens het object PfAuthParams doorgegeven aan de functie pf_authenticate(). De retourwaarde geeft aan het slagen of mislukken van de verificatie. De out parameters, callStatus en errorID, bevatten aanvullende gesprek resultaat informatie. De oproep resultaatcodes worden beschreven in het gesprek resultatenbestand in de SDK.

Deze minimale implementatie kan worden geschreven in een slechts een paar regels. In productiecode, zou u bevatten echter nog fraaiere foutafhandeling, aanvullende databasecode en een verbeterde gebruikerservaring.

### <a name="web-client-code"></a>Web Client-Code

Hierna volgt web client-code voor een demo-pagina.


    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Aan de clientzijde Code

Meervoudige verificatie is geconfigureerd en uitvoeren in stap 2 in de volgende code servers. Standaardmodus (MODE_STANDARD) is een telefoongesprek waarop de gebruiker door op de toets # reageert.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "9134884271";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = " Multi-Factor Authentication failed.";
                }
            }

        }
    }
