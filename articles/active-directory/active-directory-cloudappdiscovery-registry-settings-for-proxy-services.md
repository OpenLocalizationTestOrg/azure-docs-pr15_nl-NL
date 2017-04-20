<properties 
    pageTitle="App-Discovery-registerinstellingen voor proxyservices cloud | Microsoft Azure" 
    description="Het doel van dit onderwerp is zodat u de stappen die u moet uitvoeren om in te stellen van de vereiste poort op de computers met de Cloud-App Discovery-agent." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud-App Discovery-registerinstellingen voor proxyservices

Standaard is de Cloud-App Discovery-agent geconfigureerd voor gebruik alleen de poorten 80 of 443. Als u van plan bent voor het installeren van Cloud-App Discovery in een omgeving met een proxyserver waarin een aangepaste poort (Ineffectieve 80 en 443) wordt gebruikt, moet u uw agenten om deze poort configureren. De configuratie is gebaseerd op een registersleutel.


Het doel van dit onderwerp is zodat u de stappen die u moet uitvoeren om in te stellen van de vereiste poort op de computers met de Cloud-App Discovery-agent.



**Als u wilt wijzigen van de poort die wordt gebruikt door de computer met de Cloud-App Discovery-agent, moet u de volgende stappen uitvoeren:**


1. Start de Register-editor. <br> ![Uitvoeren](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. Navigeer naar of maken van de volgende registersleutel: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint** 

3. Maak een nieuwe **meerdere** tekenreekswaarde **poorten**genoemd. ![Nieuw](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. Om het dialoogvenster **Met meerdere tekenreeksen bewerken** , dubbelklikt u op de waarde poorten.


5. Typ de volgende waarden in het tekstvak waarde gegevens en alle aangepaste poorten die worden gebruikt door uw organisatie toevoegen: <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**443** <br>
**1110** <br><br>
![Bewerken met meerdere tekenreeksen](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. Klik op **OK** om het dialoogvenster **Met meerdere tekenreeksen bewerken** te sluiten.



**Aanvullende informatie**


* [Hoe kan ik verzoek tot niet toegestande cloud-apps die worden gebruikt Ontdek binnen mijn organisatie](active-directory-cloudappdiscovery-whatis.md) 


