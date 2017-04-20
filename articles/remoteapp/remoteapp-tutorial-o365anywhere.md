<properties
   pageTitle="Krijgen van de dezelfde Office 365-stijl op elk apparaat met Azure RemoteApp | Microsoft Azure"
   description="Leer hoe u alle Office 365-Apps delen met gebruikers met behulp van Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="guscatal;elizapo"/>


# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Krijgen van de dezelfde Office 365-stijl op elk apparaat met Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

In dit artikel wordt uitgelegd hoe u Office 365 implementeren op elk apparaat in uw bedrijf. Uw gebruikers kunnen krijgen de dezelfde mogelijkheden en UI ervaring met Android, appel- en Windows.

We doen als deze met Azure RemoteApp door Office 365 hosting op schaal kunt virtuele machines in Azure wordt aangegeven dat gebruikers verbinding kunnen maken. Deze waarde van virtuele machines verwijst naar een verzameling"cloud".

## <a name="create-a-cloud-collection"></a>Een cloud-verzameling maken

Eerst nadat u een Azure-account hebt gemaakt, navigeer naar **RemoteApp** door te klikken op de koppeling aan de linkerkant.
![Azure RemoteApp bevinden zich op de Portal van Azure](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Vervolgens gaat u verder door te klikken op **nieuwe** aan de onderkant en 'snelle maken' een siteverzameling. Geef een naam, de regio, het abonnement, het abonnement en de afbeelding 'Office Proffesional 2013' die wij bieden.
![Dialoogvenster maken](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Wanneer u klaar bent met het formulier dat een proces voor het maken van de siteverzameling moet beginnen. Dit kan duren naar een uur of om.

![In afwachting van](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Als het proces is ingevuld, wordt deze er ongeveer zo uitzien. Als we op **publicerende** zien we dat de meeste Office-toepassingen zijn gepubliceerd voor ons al.
![Siteverzamelingen die zijn gemaakt](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Gepubliceerde apps](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

U kunt nu ook meer gebruikers die toegang tot deze verzameling hebben door te klikken op **De toegang van gebruikers**toevoegen.
![De toegang van gebruikers configureren](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Nu we uitproberen verbinding met Office 365!

## <a name="connect-to-office-365"></a>Verbinding maken met Office 365

We onderweg via naar [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), schuif omlaag en klik op **clients downloaden** als u wilt de Azure RemoteApp-client installeren op het apparaat dat u zich op. De onderstaande schermafbeeldingen zijn voor Windows.

Zodra de toepassing wel wordt gestart wordt u gevraagd naar Meld u aan met uw Microsoft-account (voorheen een "Live-ID"), gebruikt u hetzelfde als uw Azure-account voor nu. Wanneer u bent aangemeld in moet u ziet een melding over de nieuwe uitnodigingen, klikt u er op en ziet u een lijst, zoals een hieronder. Accepteer de uitnodiging die overeenkomt met uw e-mail van de eigenaar van Azure-account.

![Uitnodiging voor de nieuwe](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Hoe dit eruitziet wanneer er nieuwe uitnodigingen zijn.

![Een toepassing accepteren](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Nadat u de uitnodiging hebben geaccepteerd ziet u alle Office-apps in de Azure RemoteApp-client.

![Lijst met apps](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Na het klikken op moet een van deze die de toepassing op de Azure virtuele machine en u beginnen moet alle set! Te rekenen op!

![starten](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![PowerPoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)
