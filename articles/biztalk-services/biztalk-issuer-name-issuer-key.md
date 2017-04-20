<properties 
    pageTitle="De naam van de uitgever en uitgever sleutel in BizTalk Services | Microsoft Azure" 
    description="Informatie over het ophalen van de naam van de uitgever en uitgever sleutel voor Service Bus of Access Control (ACS) in BizTalk-Services. MAB, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>




# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk Services: De naam van de uitgever en de uitgever sleutel

Azure BizTalk Services wordt gebruikt voor de naam van de Service Bus uitgever uitgever toets heeft, en Access besturingselement uitgever en de naam uitgever-toets. Name:

Taak | Welke uitgever naam en de uitgever-toets
--- | ---
De toepassing Visual Studio implementeren | Toegang tot de naam van besturingselement uitgever en uitgever-toets
De Portal van Azure BizTalk-Services configureren | Toegang tot de naam van besturingselement uitgever en uitgever-toets
LOB-relais maken met de Services BizTalk Adapter in Visual Studio | Uitgever van de servicenaam Bus en uitgever sleutel

In dit onderwerp vindt u de stappen voor het ophalen van de naam van de uitgever en uitgever-toets. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Toegang tot de naam van besturingselement uitgever en uitgever-toets
Het Access-besturingselementnaam uitgever en de uitgever sleutel worden gebruikt door het volgende:

- Uw Azure BizTalk-Service-toepassing die is gemaakt in Visual Studio: als u wilt installeren uw BizTalk Service-toepassing in Visual Studio naar Azure, u de naam van Access besturingselement uitgever en uitgever sleutel invoeren. 
- De Azure BizTalk Services-Portal: Wanneer u een BizTalk-Service maken en de Portal van de Services BizTalk Open, uw Access-besturingselementnaam uitgever en de uitgever sleutel worden automatisch geregistreerd voor uw implementaties met dezelfde waarde beheren in Access.

### <a name="to-copy-and-paste-the-access-control-issuer-name-and-issuer-key"></a>Kopiëren en plakken van het Access-besturingselementnaam uitgever en de uitgever sleutel

1. Meld u aan bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het linkernavigatiedeelvenster **BizTalk-Services**.
3. Selecteer uw BizTalk-Service. 
4. Selecteer de **Verbindingsgegevens** in de taakbalk. De Access besturingselement Namespace, standaard uitgever (uitgever naam) en standaard toets (uitgever) worden weergegeven en kan worden gekopieerd en geplakt.  

Samenvatting:  
De naam van de uitgever = standaard uitgever  
Uitgever toets = standaard-toets


U kunt ook **Openen ACS Management Portal** om de toegangsbeheer waarden selecteren:

1. Meld u aan bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het linkernavigatiedeelvenster **BizTalk-Services**.
3. Selecteer uw BizTalk-Service.
4. Selecteer de knop informatie over de databaseverbinding en selecteer **Openen ACS-beheerportal**.
5. Selecteer de **Service-identiteiten**in de Portal onder **Service-instellingen**. Hiermee worden uw identiteit Service, dat wil de waarde van uw Access-besturingselementnaam uitgever zeggen weergegeven. Selecteer uw identiteit van de Service-koppeling om het wachtwoord, dat wil de uitgever sleutelwaarde zeggen weer te geven. De waarden kunnen worden gekopieerd.<br/><br/>
In de **Service-identiteiten**ziet u bijvoorbeeld 'eigenaar'. "Eigenaar" is de naam van uw Access-besturingselement-uitgever. Wanneer u op de koppeling "eigenaar" klikt, ziet u het **wachtwoord**. Wanneer u op de koppeling "Wachtwoord" klikt, ziet u de waarde. Deze waarde wachtwoord is uw toegangstoets van het besturingselement-uitgever.  

Samenvatting:  
De naam van de uitgever = naam van de Service-identiteit  
Uitgever toets = wachtwoordwaarde

U kunt ook **Active Directory** om op te halen de toegangsbeheer waarden selecteren in het linkernavigatiedeelvenster. 

> [AZURE.IMPORTANT]Als een Access-besturingselement Namespace wordt gemaakt wordt met behulp van **Active Directory**, de identiteit van een Service **niet** automatisch gemaakt. Wanneer u een BizTalk Service, een Access-besturingselement-Namespace, inrichten Service identiteit met de naam "eigenaar" (uitgever naam), wachtwoord (uitgever toetsen), en Symmetric sleutel worden automatisch gemaakt.<br /> 
[Hoe: gebruik ACS Management-Service configureren Service identiteiten](http://go.microsoft.com/fwlink/p/?LinkID=303942) vindt u meer informatie over Access besturingselement Service-identiteiten.


## <a name="service-bus-issuer-name-and-issuer-key"></a>Uitgever van de servicenaam Bus en uitgever sleutel
Uitgever van de servicenaam Bus en uitgever sleutel worden gebruikt door BizTalk Adapter Services. In uw project BizTalk-Services in Visual Studio gebruikt u de BizTalk Adapter Services verbinding maken met een on-premises Line-of-Business (LOB)-systeem. Als u verbinding wilt maken van de Relay LOB en geef de details van uw LOB-systeem. Wanneer u dit doet, Voer u ook de Service Bus uitgever naam en de uitgever-toets.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Om op te halen van de Service Bus uitgever naam en de uitgever-toets

1. Meld u aan bij de [portal van Azure klassieke](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het linkernavigatiedeelvenster **Service Bus**.
3. Selecteer uw naamruimte. Selecteer de **Verbindingsgegevens**in de taakbalk. Hiermee worden weergegeven in de **Standaard uitgever** (uitgever naam) en de **Toets standaard** (uitgever toets). De waarden kunnen worden gekopieerd.  

Samenvatting:  
De naam van de uitgever = standaard uitgever  
Uitgever toets = standaard-toets

## <a name="next"></a>Volgende
Aanvullende Azure BizTalk Services onderwerpen:

-  [Installatie van de Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Zelfstudies: Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Hoe kan ik gebruiken de SDK van Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## <a name="see-also"></a>Zie ook
-  [Hoe u: ACS Management-Service voor het configureren van de Service-identiteiten gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [BizTalk Services: Ontwikkelaars, Basic, Standard en Premium edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk Services: Inrichting klassieke portal Azure gebruiken](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk Services: Inrichting Status grafiek](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk Services: De tabbladen Dashboard, Monitor en schaal](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk Services: Back-up maken en deze herstellen](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk-Services: beperken](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 
