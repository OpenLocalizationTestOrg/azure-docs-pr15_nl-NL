<properties
   pageTitle="Azure AD Connect: Functies in de proefversie van | Microsoft Azure"
   description="In dit onderwerp wordt beschreven in meer detail-functies die in het voorbeeld in Azure AD Connect."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/27/2016"
   ms.author="billmath"/>

# <a name="more-details-about-features-in-preview"></a>Meer informatie over de functies in de Preview-versie
In dit onderwerp wordt beschreven hoe het momenteel gebruik van functies in de proefversie.

## <a name="group-writeback"></a>Groep write-backs
De optie voor groep write-backs in optionele onderdelen, kunt u naar write-backs **Office 365-groepen** aan een bos met Exchange is geïnstalleerd. Dit is een groep die altijd is gemaakt in de cloud. Als u lokale Exchange hebt en u deze groepen naar on-premises implementatie terugschrijven kunt zodat gebruikers met een on-premises Exchange-postvak kunnen verzenden en ontvangen van e-mailberichten van deze groepen.

Meer informatie over Office 365-groepen en hoe u ze kunt gebruiken vindt u [hier](http://aka.ms/O365g).

Deze groep worden weergegeven als een distributiegroep on-premises AD DS. Uw Exchange-server on-premises implementatie moet zich op Exchange 2013 met de cumulatieve update 8 (uitgebracht in maart 2015) of Exchange-2016 te herkennen dit nieuwe groepstype.

**Notities in de voorbeeldweergave**

- Het kenmerk adres adresboek is momenteel niet in de voorbeeldversie ingevuld. Zonder dat dit kenmerk, de groep worden niet zichtbaar zijn in de GAL. De eenvoudigste manier om dit kenmerk te vullen, is het gebruik van de Exchange-PowerShell-cmdlet `update-recipient`.
- Alleen forests met de Exchange-schema zijn geldig doelen voor groepen. Als er geen Exchange is gevonden, is klikt u vervolgens groep write-backs niet mogelijk om in te schakelen.
- Alleen enkele verzameling organisatie implementaties van Exchange worden momenteel ondersteund. Als u meer dan één Exchange organisatie on-premises implementatie hebt, moet u een on-premises implementatie GALSync-oplossing voor deze groepen wordt weergegeven in uw andere forests.
- De functie Group write-backs wordt momenteel niet verwerkt beveiligingsgroepen of distributiegroepen.

>[AZURE.NOTE] Een abonnement op Azure AD Premium is vereist voor de groep write-backs.

## <a name="user-writeback"></a>Gebruiker write-backs
> [AZURE.IMPORTANT] De functie write-backs preview is verwijderd in de bijwerken augustus 2015 Azure AD Connect. Als u deze hebt ingeschakeld, moet u deze functie uitschakelen.

## <a name="next-steps"></a>Volgende stappen
Doorgaan met uw [aangepaste installatie van Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
