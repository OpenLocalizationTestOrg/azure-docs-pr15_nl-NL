<properties
   pageTitle="Azure gegevenscatalogus vereisten | Microsoft Azure"
   description="Azure gegevenscatalogus-voorwaarden - wat u moet aan de slag met Azure-gegevenscatalogus."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-prerequisites"></a>Azure gegevenscatalogus vereisten

## <a name="what-do-i-need-to-get-started-with-azure-data-catalog"></a>Wat moet ik aan de slag met Azure-gegevenscatalogus?

Er zijn enkele dingen die u verrichten moet voordat u **Azure-gegevenscatalogus**kunt instellen. U hoeft niet – ze won't lang duren!

## <a name="azure-subscription"></a>Azure-abonnement
Als u wilt de gegevenscatalogus Azure hebt ingesteld, moet u de eigenaar of de mede-eigenaar van een abonnement op Azure.

Azure abonnementen waarmee u toegang tot cloud serviceresources zoals Azure-gegevenscatalogus kunt organiseren. Ze ook help u hoe de weergave Resourcegebruik wordt aangegeven bepalen, gefactureerd en dat is betaald. Elk abonnement kan verschillende facturering en betalingsgegevens instellingen, hebben, zodat u kunt verschillende-abonnementen en andere abonnementen door afdeling, project, regionale office en dergelijke hebben. Elke cloudservice behoort tot een abonnement, en moet u beschikken over een abonnement vóór het instellen van Azure-gegevenscatalogus. Zie [Accounts beheren, abonnementen, en beheerdersrollen](../active-directory/active-directory-assign-admin-roles.md)meer informatie.

## <a name="azure-active-directory"></a>Azure Active Directory
Als u wilt instellen Azure-gegevenscatalogus, moet u zijn aangemeld met een Azure Active Directory-gebruikersaccount.

Azure Active Directory (Azure AD) vindt u een eenvoudige manier om uw bedrijf voor het beheren van identiteit en open, zowel in de cloud en on-premises implementatie. Gebruikers kunnen één werk- of schoolaccount gebruiken voor eenmalige aanmelding in een cloud en on-premises implementatie-webtoepassing. Azure AD Azure gegevenscatalogus gebruikt om te verifiëren aanmelding. Zie [Wat Azure Active Directory is](../active-directory/active-directory-whatis.md)meer informatie.

> [AZURE.NOTE] De [portal van Azure](http://portal.azure.com/) kunnen gebruikers zich aan met een persoonlijk Microsoft-Account of een Azure Active Directory-werk- of schoolaccount. Instellen van de catalogus van Azure-gegevens met behulp van de Azure portal of met behulp van de [gegevenscatalogus portal](http://www.azuredatacatalog.com) moet u zijn aangemeld met een Azure Active Directory-account, niet een persoonlijk account.

## <a name="active-directory-policy-configuration"></a>Configuratie van Active Directory-beleid

In sommige gevallen, kunnen gebruikers een situatie waar ze zich aanmelden kunnen bij de portal Azure-gegevenscatalogus, maar als zij proberen te Meld u aan bij de data bron registratie tool een foutbericht dat voorkomt dat deze aanmelden die ze ondervinden optreden. Dit probleem kan gebeuren wanneer de gebruiker zich op het netwerk van het bedrijf of optreden alleen wanneer de gebruiker verbinding van buiten het bedrijfsnetwerk maakt.

De gegevensbron registratie hulpmiddel maakt gebruik van formulierverificatie voor het valideren van gebruikers die zich aanmelden met Active Directory. Voor een succesvolle aanmelding, moet formulierverificatie zijn ingeschakeld in de globale verificatie-beleid door een Active Directory-beheerder.

De globale verificatie-beleid kan verificatiemethoden afzonderlijk worden ingeschakeld voor intranet en extranet verbindingen, Zie de volgende afbeelding. Aanmeldingsfouten kunnen optreden als formulierverificatie is niet ingeschakeld voor het netwerk van waaruit de gebruiker verbinding maakt.

 ![Active Directory globale verificatie-beleid](./media/data-catalog-prerequisites/global-auth-policy.png)

Zie [Beleidsregels voor verificatie configureren](https://technet.microsoft.com/library/dn486781.aspx)voor meer informatie.
