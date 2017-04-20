<properties 
    pageTitle="Hoe u TLS onderlinge verificatie configureren voor WebApp" 
    description="Informatie over het configureren van uw web-app als u wilt gebruiken verificatie via clientcertificaat op TLS." 
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/08/2016" 
    ms.author="naziml"/>    

# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>Het configureren van onderlinge TLS-verificatie voor WebApp

## <a name="overview"></a>Overzicht ##
U kunt toegang tot uw Azure WebApp doordat verschillende soorten verificatie voor deze beperken. Er is een manier om te doen om te verifiëren met een clientcertificaat wanneer de aanvraag afgelopen TLS/SSL is. Deze methode wordt onderlinge TLS-verificatie of clientcertificaat verificatie en in dit artikel wordt beschreven hoe u het instellen van uw web-app als u wilt gebruiken, verificatie via clientcertificaat genoemd.

> **Notitie:** Als u toegang hebt tot uw site via HTTP en niet HTTPS, ontvangt u geen eventuele clientcertificaat. Dus als clientcertificaten vereist is voor uw toepassing mag u niet worden aanvragen in uw toepassing via HTTP.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="configure-web-app-for-client-certificate-authentication"></a>WebApp configureren voor verificatie via clientcertificaat ##
Voor het instellen van uw web-app om te vereisen clientcertificaten moet u de instelling van de site clientCertEnabled voor uw web-app toevoegen en stel deze in op waar. Deze instelling is momenteel niet beschikbaar via de management-ervaring in de Portal en de REST API moet worden gebruikt om dit te doen.

U kunt het [ARMClient hulpmiddel](https://github.com/projectkudu/ARMClient) gebruiken zodat u deze eenvoudig worden opgesteld van de REST API-oproep. Nadat u zich aan met het hulpmiddel moet u de volgende opdracht:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose
    
Alles in {} worden vervangen door de gegevens voor uw web-app verplaatst maken van een bestand naar enableclientcert.json met de volgende JSON inhoud:

> {"locatie": "Mijn App weblocatie"   
>   "eigenschappen": {  
>     "clientCertEnabled": waar}}  

Zorg ervoor dat om te wijzigen in de waarde van "locatie" afhankelijk van waar uw web-app zich bevindt bijvoorbeeld Noord centraal ons of West Amerikaans enzovoort.

> **Notitie:** Als u ARMClient uit Powershell uitvoert, moet u druk op ESC om de @ symbool voor de JSON-bestand met een vorige maatstreepjes '.

## <a name="accessing-the-client-certificate-from-your-web-app"></a>Toegang krijgen tot het clientcertificaat in uw Web-App ##
Als u ASP.NET gebruikt en configureren van uw app als u wilt gebruiken, verificatie via clientcertificaat, is het certificaat is beschikbaar via de eigenschap **HttpRequest.ClientCertificate** . Voor andere stapels toepassing is de client-certificaat beschikbaar in uw app via een base64-gecodeerde waarde in de koptekst van de aanvraag "X-ARR-ClientCert". Uw toepassing kunt een digitaal certificaat maken van deze waarde en deze vervolgens gebruiken voor verificatie en machtiging doeleinden in uw toepassing.

## <a name="special-considerations-for-certificate-validation"></a>Aandachtspunten voor certificaatverificatie ##
Het clientcertificaat dat wordt verzonden naar de toepassing verder door het platform Azure Web Apps niet een validatie uit te voeren. Valideren van dit certificaat is de verantwoordelijkheid van de web-app. Hier ziet u een voorbeeld ASP.NET-code die de eigenschappen van het certificaat voor verificatiedoeleinden is gevalideerd.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;
                
                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;
                
                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
