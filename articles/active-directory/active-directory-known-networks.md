<properties 
    pageTitle="Bekende netwerken | Microsoft Azure" 
    description="Bekende netwerken configureren, kunt u hoeft niet IP-adressen die door uw organisatie die is opgenomen in de ins Aanmeldingsadres uit verschillende gebieden en aanmelding ins van IP-adressen met verdachte activiteitsrapporten eigendom zijn." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"  
    editor=""/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markvi"/>

# <a name="known-networks"></a>Bekende-netwerken te gebruiken


U kunt toegang tot Azure Active Directory- en gebruiksrapporten krijgen inzicht in de integriteit en de beveiliging van de adreslijst van uw organisatie. Met deze informatie kunt een directory-beheerder beter bepalen waar mogelijke beveiligingsrisico's mogelijk liggen zodat ze voldoende plannen kunnen deze risico's te verminderen.

Het is mogelijk dat de rapporten "*Aanmeldingsadres ins uit verschillende gebieden*" en "*Aanmeldingsadres ins van IP-adressen met verdachte activiteit*" markeert IP-adressen die het eigendom zijn door uw organisatie. 

Dit kan, bijvoorbeeld gebeuren wanneer: 

- Een gebruiker in uw office heeft extern aangemeld bij uw datacenter in San Francisco Boston activeert het rapport "Meld u aan de invoegtoepassingen uit verschillende gebieden" 

- Een gebruiker van uw organisatie wil aanmelding enkele malen met een onjuist wachtwoord triggers het rapport "Zich invoegtoepassingen voor IP-adressen met verdachte activiteit" 

Als u wilt voorkomen dat deze gevallen misleidende beveiligingsrapporten genereren, moet u bekende IP-adresbereiken toevoegen aan de lijst met het openbare IP-adres van uw organisatie.    


###<a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Als u wilt toevoegen van uw organisatie openbare IP-adresbereiken, moet u de volgende stappen uitvoeren: 

1.  Eenmalige aanmelding bij de [portal van Azure management](https://manage.windowsazure.com).

2.  Klik in het linkerdeelvenster op **Active Directory**. <br><br>![De werking van Cloud-App Discovery](./media/active-directory-known-networks/known-netwoks-01.png)

3.  Selecteer de map in het tabblad **map** .

4.  In het menu aan de bovenkant, klikt u op **configureren**. <br><br>![De werking van Cloud-App Discovery](./media/active-directory-known-networks/known-netwoks-02.png)

5.  Klik op het tabblad configureren, gaat u naar **uw organisaties openbare IP-adresbereiken** <br><br>![De werking van Cloud-App Discovery](./media/active-directory-known-networks/known-netwoks-03.png)

6.  Klik op **bekende IP-adresbereiken toevoegen**.

7.  Uw adresbereiken toevoegen in het dialoogvenster dat wordt weergegeven, en klik op de knop controleren als u klaar bent. <br><br>![De werking van Cloud-App Discovery](./media/active-directory-known-networks/known-netwoks-04.png)


**Aanvullende informatie**


* [Uw toegang en gebruik rapporten weergeven](active-directory-view-access-usage-reports.md)
* [Meld u aan ins van IP-adressen met verdachte activiteiten](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Meld u invoegtoepassingen uit verschillende gebieden](active-directory-reporting-sign-ins-from-multiple-geographies.md)


