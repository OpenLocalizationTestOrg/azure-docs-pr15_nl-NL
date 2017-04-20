<properties 
    pageTitle="Windows-verificatie en Azure meervoudige verificatie-Server"
    description="Dit is de pagina van de Azure meervoudige verificatie die helpt bij het Windows-verificatie en Azure meervoudige verificatieserver implementeren."
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
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windows-verificatie en Azure meervoudige verificatie-Server

De sectie Windows-verificatie kan de beheerder inschakelen en configureren van Windows-verificatie voor een of meer toepassingen.  Hier volgt een lijst met items waaraan u moet denken vóór het instellen van Windows-verificatie.

-  opnieuw opstarten nodig is voordat de Azure meervoudige verificatie voor Terminal Services toegepast worden.
-  Als 'Azure meervoudige verificatie vereisen gebruiker vergelijken' is ingeschakeld en u zich niet in de lijst met gebruikers, is het niet mogelijk aan te melden bij de computer na opnieuw opstarten.
-  Vertrouwde IP-adressen is afhankelijk van of de toepassing de client-IP-met de verificatie kan bieden. Momenteel alleen Terminal Services wordt ondersteund.  







>[AZURE.NOTE]Deze functie wordt niet ondersteund voor secure Terminal Services op Windows Server 2012 R2.




## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Als u wilt beveiligen een toepassing met Windows-verificatie, gebruikt u de volgende procedure.

1. Klik op het Windows-verificatie-pictogram op de Server Azure meervoudige verificatie.
![Windows-verificatie](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Schakel het selectievakje inschakelen Windows-verificatie. Dit selectievakje is standaard uitgeschakeld.
3. Het tabblad toepassingen kan de beheerder voor het configureren van een of meer toepassingen voor Windows-verificatie.
4. Selecteer een server of -toepassing: Geef aan of de servertoepassing is ingeschakeld. Klik op OK.
5. Klik op toevoegen... knop.
6. Het tabblad vertrouwde IP's kunt u Azure meervoudige verificatie voor Windows-sessies die afkomstig zijn van specifieke IP-adressen overslaan. Bijvoorbeeld als werknemers gebruikt u de toepassing van de office- en thuis, besluiten u dat u niet wilt dat hun telefoon overgaan voor Azure meervoudige verificatie terwijl u op kantoor. Hiervoor moet u het office-subnet opgeven als vertrouwde IP-adressen-fragment.
7. Klik op toevoegen... knop.
8. Selecteer één IP als u wilt overslaan één IP-adres.
9. Selecteer IP-bereik als u wilt overslaan gehele IP-bereik. Voorbeeld 10.63.193.1-10.63.193.100.
10. Selecteer Subnet als u een bereik wilt van IP-adressen met behulp van subnet notatie opgeven. Voer de begindatum van het subnet-IP en kiest u het juiste netmasker uit de vervolgkeuzelijst.
11. Klik op de knop OK.
