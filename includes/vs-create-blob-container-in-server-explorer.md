U kunt Azure opslag wachtrijen maken met behulp van Visual Studio **Server Explorer**.

![Server Explorer BLOB 's][Image1]

1. Kies op het menu **Beeld** **Server Explorer**.
2. In Server Explorer Vouw het knooppunt **Azure** voor uw abonnement en uit het knooppunt **opslag** en het knooppunt voor de opslag-account dat u hebt opgegeven in de verbonden Azure-Storage-service.
3. Selecteer het knooppunt **wachtrijen** en kies **Wachtrij maken** in het snelmenu.
4. Voer een naam voor de container en kies **OK**.   

Standaard de nieuwe container privé is en moet u uw opslagruimte toegangstoets wilt BLOB's downloaden van deze container. Als u de bestanden in de container openbaar maken wilt, selecteert u de container in **Server Explorer** en druk op `F4` om **het eigenschappenvenster** weer te geven. Stel de **openbare leestoegang** op **Blob**. Iedereen op Internet BLOB's in een openbare container kunt zien, maar u kunt wijzigen of ze alleen verwijderen als u de juiste toegangstoets hebt.


[Image1]: ./media/vs-create-blob-container-in-server-explorer/vs-storage-create-blob-containers-in-Server-Explorer.png