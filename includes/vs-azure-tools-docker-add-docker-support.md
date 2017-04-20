1. Klik in de Visual Studio **Solution Explorer**met de rechtermuisknop op het project en selecteer **toevoegen > Docker ondersteuning** in het snelmenu.

    ![Contextmenu voor Docker ondersteuning toevoegen](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Ondersteuning voor Docker toevoegen aan een ASP.NET-Core levert webproject de toevoeging van verschillende Docker-gerelateerde bestanden die worden toegevoegd aan het project, inclusief bestanden Docker opstellen, implementatie Windows PowerShell-scripts en Docker eigenschap bestanden. 

    ![Docker-bestanden die zijn toegevoegd aan project](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE]Als de [Docker voor de bètaversie van de Windows](https://beta.docker.com)gebruikt, Properties\Docker.props openen, de standaardwaarde verwijderen en opnieuw Visual Studio voor de waarde te activeren.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
