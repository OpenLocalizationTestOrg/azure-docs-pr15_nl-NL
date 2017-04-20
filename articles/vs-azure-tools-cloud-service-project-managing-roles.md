<properties
   pageTitle="Beheer van rollen in de Azure cloud services-projecten met Visual Studio | Microsoft Azure"
   description="Leer hoe u nieuwe rollen toevoegen aan uw project van de service Azure cloud of bestaande rollen verwijderen uit deze met behulp van Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>Rollen in de Azure cloud services-projecten met Visual Studio beheren

Nadat u uw project Azure cloud-service hebt gemaakt, kunt u nieuwe rollen aan toe te voegen of bestaande rollen verwijderen uit deze. U kunt ook een bestaand project importeren en converteren naar een rol. U kunt bijvoorbeeld een ASP.NET-webtoepassing importeren en aangewezen als een Webrol.

## <a name="adding-or-removing-roles"></a>Toevoegen of verwijderen van rollen

**Een rol toe te voegen**

Open het snelmenu voor het knooppunt **rollen** in uw project cloud-service in **Solution Explorer**en klikt u op **toevoegen**. U kunt de rol van de werknemer van een bestaande Webrol of Selecteer in de huidige oplossing of maak een nieuw web of werknemer rol project. Of u kunt een juiste project, zoals een ASP.NET web application-project selecteren en deze koppelen aan een rol-project.

**Verwijderen van een rolkoppeling**

Open het snelmenu voor de beheerdersrol die u wilt verwijderen en kies **verwijderen**in het knooppunt **rollen** van het project cloud-service in Solution Explorer.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Verwijderen en rollen in uw cloudservice toevoegen

Als u een rol uit uw project cloud-service verwijderen, maar u later besluit om toe te voegen van de rol terug naar het project, worden alleen de rol declaratie en eenvoudige kenmerken, zoals eindpunten en diagnostische informatie worden toegevoegd. Geen extra bronnen of verwijzingen zijn toegevoegd aan het bestand ServiceDefinition.csdef of naar het bestand ServiceConfiguration.cscfg. Als u deze informatie toevoegen wilt, moet u deze handmatig toevoegen terug naar deze bestanden.

Zoals u kunt een Webrol-service verwijderen en u later besluit om toe te voegen van deze rol weer aan uw oplossing. Als u dit doet, wordt een foutbericht weergegeven. Als u wilt voorkomen dat deze fout, u moet toevoegen de `<LocalResources>` element wordt weergegeven in de volgende XML terug naar het bestand ServiceDefinition.csdef. Gebruik de naam van de rol van de web-service die u weer aan het project hebt toegevoegd als onderdeel van het naamkenmerk voor de **<LocalStorage>** element. In dit voorbeeld is de naam van de rol van de service web **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het configureren van rollen in Visual Studio door te [configureren de rollen voor een Azure-Cloudservice met Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md)lezen.
