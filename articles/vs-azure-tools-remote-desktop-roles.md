<properties 
   pageTitle="Gebruik van extern bureaublad met Azure rollen | Microsoft Azure"
   description="Gebruik van extern bureaublad met Azure rollen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-remote-desktop-with-azure-roles"></a>Gebruik van extern bureaublad met Azure rollen

U kunt met behulp van Azure SDK en Remote Desktop Services, Azure rollen en virtuele machines die worden gehost door Azure openen. U kunt in Visual Studio Remote Desktop Services configureren uit een Azure project. Als u wilt inschakelen Remote Desktop Services, moet u een project werken met een of meer rollen maken en vervolgens publiceren naar Azure.

>[AZURE.IMPORTANT] U moet een Azure beheerdersrol voor probleemoplossing of ontwikkeling alleen openen. Het doel van elke virtuele machine wordt uitgevoerd van een specifieke rol in uw Azure-toepassing, niet voor andere clienttoepassingen. Als u Azure wilt voor het hosten van een virtuele machine die u voor doeleinden gebruiken kunt gebruikt, raadpleegt u toegang tot Azure virtuele Machines vanuit Server Explorer.

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Inschakelen en gebruiken van extern bureaublad voor een Azure-beheerdersrol

1. Open het snelmenu voor uw project in Solution Explorer en kies **publiceren**.

    De wizard **Publiceren Azure-toepassing** wordt weergegeven.

    ![Opdracht voor een Cloudservice project publiceren](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)

1. Selecteer het **Extern bureaublad inschakelen** voor alle rollen selectievakje onderaan in de **Publicatie-instellingen van Microsoft Azure** -pagina van de wizard. 

    Het dialoogvenster **Configuratie voor extern bureaublad** wordt weergegeven.

1. Kies onder in het dialoogvenster **Configuratie voor extern bureaublad** , de knop **Meer opties** . 
 
    Hiermee worden weergegeven voor een vervolgkeuzelijst waarmee u maken en kies een certificaat kunt, zodat u van referenties gegevens coderen kunt wanneer u verbinding maakt via Extern bureaublad.

1. Kies in de vervolgkeuzelijst ** &lt;maken >**, of kiest u een bestaande eigenschap in de lijst. 

    Als u een bestaand certificaat kiest, moet u de volgende stappen overslaan.

    >[AZURE.NOTE] De certificaten die u nodig voor een verbinding met extern bureaublad hebt verschillen van de certificaten die u voor andere Azure bewerkingen gebruikt. Het certificaat RAS moet een persoonlijke sleutel hebben.

    Het dialoogvenster **Certificaat maken** wordt weergegeven.

    1. Geef een beschrijvende naam voor het nieuwe certificaat en klik op **OK** . Het nieuwe certificaat wordt weergegeven in de vervolgkeuzelijst.

    1. Geef een gebruikersnaam en wachtwoord in het dialoogvenster **Configuratie voor extern bureaublad** .
    
        U kunt een bestaand account niet gebruiken. Geen beheerder opgeven als de gebruikersnaam voor het nieuwe account.

        >[AZURE.NOTE] Als het wachtwoord niet voldoet aan de complexiteitsvereisten, verschijnt er een rode pictogram naast het wachtwoordvak. Het wachtwoord moet bevatten hoofdletters, kleine letters, en getallen of symbolen.

    1. Kies een datum in waarop de account verloopt en na welke verbindingen met extern bureaublad worden geblokkeerd.

    1. Nadat u de vereiste gegevens hebt opgegeven, klikt u op de knop **OK** .
    
        Verschillende instellingen waarmee Remote Access Services worden toegevoegd aan de .cscfg en .csdef-bestanden.

1. Kies in de wizard **Microsoft Azure Publish Settings** de knop **OK** wanneer u klaar bent om te publiceren van uw cloudservice.

    Als u niet wilt publiceren, klikt u op de knop **Annuleren** . De configuratie-instellingen worden opgeslagen en u uw cloudservice later kunt publiceren.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Verbinding maken met een beheerdersrol Azure via Extern bureaublad

Nadat u uw cloudservice op Azure publiceert, kunt u Server Explorer aan te melden bij de virtuele machines die Azure hosts. 

1. Vouw het knooppunt **Azure** in Server Explorer en vouw het knooppunt voor een cloudservice en een van de rollen voor het weergeven van de exemplaren.

1. Open het snelmenu voor een exemplaar knooppunt en kiest u **Verbinding maken met behulp van extern bureaublad**.

    ![Verbinding maken via Extern bureaublad](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)

1. Voer de gebruikersnaam en wachtwoord dat u eerder hebt gemaakt. U bent nu aangemeld bij uw externe sessie.


