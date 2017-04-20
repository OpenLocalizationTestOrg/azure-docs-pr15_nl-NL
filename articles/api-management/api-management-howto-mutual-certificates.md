<properties 
    pageTitle="Back-enddatabase services client met verificatie via clientcertificaat in Azure API Management beveiligen" 
    description="Informatie over het beveiligen van back-enddatabase services met verificatie via clientcertificaat in Azure API Management." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Back-enddatabase services client met verificatie via clientcertificaat in Azure API Management beveiligen

API Management biedt de mogelijkheid om te beveiligen toegang tot de back-enddatabase-service van een API clientcertificaten gebruiken. Deze handleiding ziet u hoe u de certificaten in de API publisher-portal beheren en hoe u een API voor het gebruik van een certificaat voor toegang tot de back-end-service configureren.

Zie voor informatie over het beheren van certificaten met behulp van de API Management REST API [Azure API Management REST API certificaat entiteit][].

## <a name="prerequisites"> </a>Vereisten

Deze handleiding ziet u hoe u uw exemplaar van de service API Management om deze te verificatie via clientcertificaat gebruiken voor toegang tot de back-enddatabase-service voor een API configureren. Voordat u de stappen in dit onderwerp, moet uw back-end-service geconfigureerd voor verificatie via clientcertificaat ([Als u wilt configureren verificatie via clientcertificaat in Azure WebSites verwijzen naar dit artikel][]), en dat u hebt toegang tot het certificaat en het wachtwoord voor het certificaat voor het uploaden in de publisher-API-beheerportal.

## <a name="step1"> </a>Een clientcertificaat uploaden

Als u wilt beginnen, klikt u op **beheren** in de klassieke Azure-Portal voor uw service API Management. Hiermee gaat u naar de publisher-API-beheerportal.

![API Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

**Beveiliging** in het menu **API beheer** aan de linkerkant op en klik op **certificaten**.

![Clientcertificaten][api-management-security-client-certificates]

Klik op **certificaat uploaden**om een nieuw certificaat.

![Certificaat uploaden][api-management-upload-certificate]

Blader naar het certificaat en voer vervolgens het wachtwoord voor het certificaat.

>Het certificaat moet **.pfx** -indeling. Zelfondertekende certificaten zijn toegestaan.

![Certificaat uploaden][api-management-upload-certificate-form]

Klik op **uploaden** om te uploaden van het certificaat.

>Het certificaatwachtwoord is gevalideerd op dit moment. Als het niet juist wordt een foutbericht wordt weergegeven.

![Certificaat hebt geüpload][api-management-certificate-uploaded]

Als het certificaat is geüpload, wordt deze weergegeven op het tabblad **certificaten** . Als u meerdere certificaten hebt, controleert u een notitie van het onderwerp of de laatste vier tekens van de vingerafdruk, die worden gebruikt voor het selecteren van het certificaat tijdens het configureren van een API voor het gebruik van certificaten, zoals beschreven in de volgende sectie voor het [configureren een API voor het gebruik van een clientcertificaat voor verificatie van de gateway][] .

>Volg de stappen in deze Veelgestelde vragen over het [item](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)uit te schakelen ketting certificaatverificatie gebruikt, bijvoorbeeld een zelfondertekend certificaat.

## <a name="step1a"> </a>Verwijderen van een clientcertificaat

Als u wilt een certificaat verwijderen, klikt u op **verwijderen** naast het gewenste certificaat.

![Certificaat verwijderen][api-management-certificate-delete]

Klik op **Ja, verwijdert u deze** om te bevestigen.

![Verwijderen bevestigen][api-management-confirm-delete]

Als het certificaat door een API wordt gebruikt is, wordt klikt u vervolgens een waarschuwing-scherm weergegeven. Als u wilt verwijderen van het certificaat moet u eerst het certificaat verwijderen uit een API's die zijn geconfigureerd dat deze gebruiken.

![Verwijderen bevestigen][api-management-confirm-delete-policy]

## <a name="step2"> </a>Een API voor het gebruik van een clientcertificaat voor verificatie van de gateway configureren

Klik op **API's** in het menu **API beheer** aan de linkerkant, klik op de naam van de gewenste API en klik op het tabblad **beveiliging** .

![API-beveiliging][api-management-api-security]

Selecteer **clientcertificaten** in de vervolgkeuzelijst **met referenties** .

![Clientcertificaten][api-management-mutual-certificates]

Selecteer het gewenste certificaat in de vervolgkeuzelijst **clientcertificaat** . Als er meerdere certificaten kunt u het onderwerp of de laatste vier tekens van de vingerafdruk zoals beschreven in de vorige sectie om te bepalen van het juiste certificaat kijken.

![Selecteer certificaat][api-management-select-certificate]

Klik op **Opslaan** om op te slaan wijzigen van de configuratie tot de API.

>Deze wijziging is onmiddellijk van kracht en oproepen naar bewerkingen van die API wordt het certificaat gebruiken om te verifiëren op de server back-enddatabase.

![API wijzigingen opslaan][api-management-save-api]

>Wanneer een certificaat is opgegeven voor de verificatie van de gateway voor de back-enddatabase-service van een API, maakt deel uit van het beleid voor die API, en kunnen worden weergegeven in de editor.

![Certificaatbeleid][api-management-certificate-policy]

## <a name="next-steps"></a>Volgende stappen

Zie de volgende video voor meer informatie over andere manieren om te beveiligen van uw backend-service, zoals HTTP-eenvoudige of gedeelde geheime verificatie.

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Aan de slag met Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance

[Azure API Management REST API certificaat entiteit]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Als u wilt configureren certificaatverificatie in Azure WebSites verwijzen naar dit artikel]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Een API voor het gebruik van een clientcertificaat voor verificatie van de gateway configureren]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 
