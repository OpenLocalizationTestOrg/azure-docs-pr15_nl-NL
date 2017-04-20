<properties
   pageTitle="Verificatie met Amazon webservices configureren | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe maken en valideren van een referentie AWS voor runbooks in Azure automatisering AWS bronnen beheren."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="AWS authenticatie, aws configureren"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>

# <a name="authenticate-runbooks-with-amazon-web-services"></a>Runbooks met Amazon webservices verifiëren
Algemene taken automatiseren met resources in Amazon Web Services (AWS) kan worden uitgevoerd met automatisering runbooks in Azure wordt aangegeven.  U kunt veel taken in AWS met automatisering runbooks net als bij de resources in Azure kunt automatiseren.  Alle die is vereist zijn twee dingen:

* Een abonnement AWS en een set referenties.  Specifiek uw AWS toegangstoets en een geheime sleutel.  Lees het artikel [AWS-referenties gebruiken](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html)voor meer informatie.
* Een Azure-abonnement en automatisering-account.  Lees het artikel [Azure uitvoeren als Account configureren](../automation/automation-sec-configure-azure-runas-account.md)voor meer informatie over het instellen van een automatisering Azure-account.  

Als u wilt verifiëren met AWS, moet u een reeks AWS referenties voor de verificatie van uw runbooks vanaf Azure automatisering worden uitgevoerd. Als u al een automatisering-account hebt gemaakt en u gebruiken die wilt verificatie met AWS, kunt u de stappen in de volgende sectie.  Als u een account voor runbooks gericht AWS resources toegewezen wilt, moet u eerst een nieuw [account automatisering uitvoeren als](../automation/automation-sec-configure-azure-runas-account.md) (de optie voor het maken van een service principal overslaan) maken en volg de onderstaande stappen.

## <a name="configure-automation-account"></a>Automatisering-account configureren
Voor Azure automatisering om te communiceren met AWS, moet u eerst naar uw referenties AWS ophalen en ze opslaan als activa in Azure automatisering.  De volgende stappen beschreven in het document AWS [Toegangstoetsen beheren voor uw Account AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) toegangstoets maken en kopiëren van de **Access-sleutel-ID** en **Geheim toegangstoets** uitvoeren (desgewenst uw belangrijkste bestand om op te slaan deze ergens veilige downloaden).

Nadat u hebt gemaakt en uw sleutels AWS gekopieerd, moet u een referentie-activa maken met een account Azure automatisering veilig ze hebt opgeslagen en ze verwijzen naar met uw runbooks.  Volg de stappen in de sectie **een nieuwe referentie maken** in het artikel [referentie activa in Azure automatisering](../automation/automation-certificates.md/###To create a new credential with the Azure portal) en voer de volgende gegevens:

1. Voer in het vak **naam** **AWScred** of een geschikte waarde uw naming standaarden te volgen.  
2. Typ uw **Access-ID** en uw **Geheim toegangstoets** in het vak **wachtwoord** en **Bevestig het wachtwoord** in het vak **gebruikersnaam** .   

## <a name="next-steps"></a>Volgende stappen

- Bekijken het artikel oplossing [automatiseren implementatie van een VM in Amazon Web Services](../automation/automation-scenario-aws-deployment.md) voor meer informatie over het maken van runbooks om taken in AWS te automatiseren.
