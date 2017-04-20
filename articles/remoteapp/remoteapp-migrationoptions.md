
<properties 
    pageTitle="Opties voor het migreren van afmelden bij Azure RemoteApp | Microsoft Azure" 
    description="Meer informatie over de opties voor het migreren van afmelden bij Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2016" 
    ms.author="elizapo" />

# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Opties voor het migreren van afmelden bij Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Als u het gebruik van Azure RemoteApp vanwege de [buitengebruikstelling aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) zijn gestopt of omdat u klaar bent met uw evaluatie, moet u mensen uit Azure RemoteApp migreren naar een andere app-service. Er zijn twee manieren om deze te migreren: een selfservice beheerde (vaak infrastructuur als een Service [IaaS] genoemd) implementatie of een volledig beheerde (vaak genoemd Platform als een Service) of de Software als Service [PaaS/SaaS] aanbod. 

Selfservice-IaaS is een doe implementatie dat wordt beheerd, dat wordt beheerd en u rechtstreeks worden geïmplementeerd op virtuele machines (VMs) of fysieke systemen eigenaar. Aan het andere uiteinde, een volledig beheerde PaaS/SaaS aanbod is vergelijkbaar met een Azure RemoteApp - een partner met een laag service boven aan een externe-oplossing die omgaat met operationele en onderhoud, terwijl u, als de klant, gaat u enkele afbeelding en de app management.

Lees verder voor meer informatie en voorbeelden van de verschillende hostingprovider opties.    

## <a name="self-managed-iaas-solutions"></a>Selfservice beheerde (IaaS)-oplossingen

### <a name="rds-on-iaas"></a>**RDS op IaaS** 
U kunt een systeemeigen sessie gebaseerde Remote Desktop Services (in Windows Server) implementatie met RemoteApp- of desktopcomputer on-premises implementatie implementeren of in een gehoste omgeving (like op Azure VMs). RDS op IaaS implementaties meest geschikt zijn voor klanten die al kent en die bestaande technische expertise met RDS implementaties hebben. 

> [AZURE.NOTE]
> U moet Volume Licensing met Software Assurance (SA) voor licenties voor clienttoegang RDS deze optie implementatie wilt gebruiken.

RDS op Azure VMs distribueren is gemakkelijker dan ooit wanneer u implementatie en het corrigeren van sjablonen (voor het lezen van een [Overzicht](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) en klik vervolgens [gaat u deze](https://aka.ms/rdautomation)) gebruikt. U gaat dezelfde elastische schaal mogelijkheden met Azure klassieke implementatie model resources (niet Azure Resource Model resources) binnen Azure RemoteApp met behulp van de die wordt [automatisch schalen script](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), hoewel er meer aanpassingen en configuraties zijn. Wanneer u RDS op Azure VMs implementeert, wordt ondersteund door de [Ondersteuning van Azure](https://azure.microsoft.com/support/plans/), de dezelfde supportmedewerkers die u met Azure RemoteApp ondersteund. U kunt ophalen kostenramingen op basis van uw bestaande gebruik door contact opnemen met [Ondersteuning van Azure](https://azure.microsoft.com/support/plans/)of kunt u berekeningen uitvoeren uzelf met een binnenkort worden uitgebracht kosten Rekenmachine.  Met N-reeks VMs (die momenteel in privé preview) kunt u ook vGPU - toevoegen horen informatie over het toevoegen van vGPU en over het [Gebruik RDS verbeteringen in Windows Server 2016](https://myignite.microsoft.com/videos/2794) te maken in onze Ignite-sessie.   

We hebben stap voor stap implementatie gidsen voor [Windows Server 2012 R2](http://aka.ms/rdsonazure) en [Windows Server 2016](http://aka.ms/rdsonazure2016) om u te helpen met uw implementatie. Bekijk de [Extern bureaublad-blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) voor het laatste nieuws.
 
### <a name="citrix-on-iaas"></a>**Citrix op IaaS** 
Een systeemeigen Citrix implementatie van sessie gebaseerde XenApp of XenDesktop kan worden geïmplementeerd on-premises implementatie of vanuit een gehoste omgeving (zoals op Azure VMs). 

Bekijk de stapsgewijze implementatiehandleiding, [Citrix XA 7.6 op Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), voor meer informatie. Lees meer over [Citrix op Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), inclusief een rekenmachine prijs. U vindt ook een [Citrix Neem contact op met](http://citrix.com/English/contact/index.asp) de opties met bespreken.

## <a name="fully-managed-paassaas-offerings"></a>Volledig beheerde (PaaS/SaaS)-aanbiedingen

### <a name="citrix-cloud"></a>**Citrix Cloud** 
[De bestaande cloud-oplossing Citrix](https://www.citrix.com/products/citrix-cloud/), identieke conceptueel naar Citrix XenApp Express. Citrix biedt een [50% korting aanbieding](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/) voor bestaande klanten van Azure RemoteApp. 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix XenApp Express (in Technical preview)**
[Registreren voor hun Technical preview](http://now.citrix.com/remoteapp), en hun [Ignite sessie](https://myignite.microsoft.com/videos/2792) bekijken (beginnend bij minuut 20:30). XenApp Express conceptueel identiek is aan Citrix Cloud, behalve het vereenvoudigd beheer UI en andere functies en mogelijkheden die vergelijkbaar met Azure RemoteApp zijn bevat. 

Meer informatie over het [Citrix XenApp Express](http://now.citrix.com/remoteapp).   

### <a name="citrix-service-provider-program"></a>**Programma voor Citrix Service Provider** 
Het programma Citrix Service Provider kunt u gemakkelijk voor serviceproviders voor het leveren van de eenvoudig te virtuele cloud computing naar SMB's, aanbod ze de services die ze in een eenvoudige, pay-as-you-go model willen. Citrix serviceproviders hun bedrijf Microsoft SPLA vergroten en hun RDS platform investeringen uitvouwen met elk apparaat, toegang vanaf elke locatie, de langste programmaondersteuning, een uitgebreide ervaring, extra beveiliging en verbeterde schaalbaarheid. Op zijn beurt Citrix serviceproviders trekken meer abonnees, vergroot de klanttevredenheid van de en hun operationele verlagen. [Meer informatie](http://www.citrix.com/products/service-providers.html) of [een partner zoeken](https://www.citrix.com/buy/partnerlocator.html).

### <a name="microsoft-hosted-service-provider"></a>**Microsoft serviceprovider gehost** 
Hostingprovider partners bieden meestal een volledig beheerde gehost Windows-bureaublad en application service, waaronder het Azure resources, besturingssystemen, toepassingen en helpdesk met de partner beheren overeenkomsten met Microsoft en andere providers software samen met een Service Provider licentieovereenkomst toe te staan dat wederverkoop van abonnee Access licentie (SAL) wordt de van licenties. De volgende informatie detailinformatie en contactgegevens voor een deel van de hosters die gespecialiseerd te helpen klanten met hun Azure RemoteApp-migratie. Bekijk [de huidige lijst met serviceproviders die worden gehost](http://aka.ms/rdsonazurecertified) die de RDS op IaaS zal het leren van pad en assessment hebt voltooid.  

#### <a name="aspex"></a>**ASPEX**
Er is een [ASPEX](http://www.aspex.be/en) gespecialiseerd in ISV's overgang naar de Cloud en ISV' wilt optimaliseren van hun huidige cloud-instellingen. ASPEX biedt een groot aantal beheerde services, devops en consultingservices.  

Primaire locatie: Antwerpen, België

Bewerkingsregio: West-Europa

Status van partner: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Microsoft Cloud-aanbieder: Ja

RemoteApp- en bureaublad sessie-oplossingen bieden: Ja, beide

Azure RemoteApp-oplossingen voor migratie: Ja, [voor meer informatie](https://www.aspex.be/en/azure-remote-apps)

**Contactpersoon:**

- Telefoon: +3232202198
- E-mail:[info@aspex.be](mailto:info@aspex.be)
- Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink (Platform naam: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com) is een platform automatisering voor IT-bedrijven te vereenvoudigen, optimaliseren en de schaal van de migratie en de bezorging van externe bureaubladen, externe toepassingen en -infrastructuur in de Cloud met Microsoft Azure aanpassen. 

Het platform MyCloudIT distributietijd 95%, Azure kosten per 30%, en de hele IT-infrastructuur van hun klant overgezet naar de cloud in een kwestie van een paar toetsaanslagen. Partners nu klanten vanuit één globale dashboard kunnen beheren, service eindgebruikers overal ter wereld zoals nooit tevoren en uit te breiden inkomsten zonder extra belasting of uitgebreide Azure training toe te voegen.  

Primaire locatie: Dallas, TX, VS

Bewerkingsregio: wereldwijd

Status van partner: [gouden](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Microsoft Cloud-aanbieder: Ja

RemoteApp- en bureaublad sessie-oplossingen bieden: Ja, beide

Azure RemoteApp-oplossingen voor migratie: Ja, [voor meer informatie](https://mycloudit.com/remote-app-microsoft/)

**Contactpersoon:**
- Brian Garoutte, VP Business Development

   Telefoon: 972-218-0741

   E-mail:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com) is gespecialiseerd in leveren gehoste bureaublad oplossingen, bezorgen volledige bureaublad en toepassingen ISV ervaringen gebaseerd op Microsoft technology bij een globale client grondtal van Azure en hun eigen datacenters.

Primaire locatie: Londen, UK; Singapore; Houston, TX

Bewerkingsregio: wereldwijd

Status van partner: gouden

Microsoft Cloud-aanbieder: Ja

RemoteApp- en bureaublad sessie-oplossingen bieden: Ja, beide

Azure RemoteApp-oplossingen voor migratie: Ja, [voor meer informatie](http://www.acuutech.com/ara-migration/)

**Contactpersoon:**

- Verenigd Koninkrijk:

  5/6 York House, Langston wegen,

  Loughton, Essex IG10 3TQ
  
  Telefoon: + 44 (0) 20 8502 2155
 
- Singapore:

  100 Cecil straat, #09-02, 
  
  De wereldbol, Singapore 069532
  
  Telefoon: + 65 6709 4933
 
- Noord-Amerika: 

  3601 S. Sandman St.
  
  Suite 200, Houston, TX 77098
  
  Telefoon: +1 713 691 0800

## <a name="need-more-help"></a>Meer hulp nodig?
Nog steeds moet kiezen of hebt u hulp verder vragen? Gebruik een van de volgende methoden voor hulp. 

1.  Een e-mail sturen naar [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2.  Neem contact op met [ondersteuning voor Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Begin met het openen van een [Azure ondersteunen hoofdletters/kleine letters](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
3.  Ons bellen. [Zoeken naar een lokaal nummer van de verkoop](https://azure.microsoft.com/overview/sales-number/).
