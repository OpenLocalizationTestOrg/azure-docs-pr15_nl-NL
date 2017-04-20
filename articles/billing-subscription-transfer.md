<properties
   pageTitle="De overdracht van eigendom van een abonnement op Azure | Microsoft Azure"
   description="Hoe u een abonnement op Azure vaak overzet naar een andere gebruiker, en enkele gevraagd Veelgestelde vragen over het proces"
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing,top-support-issue"/>

<tags
   ms.service="billing"
   ms.workload="na"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/10/2016"
   ms.author="genli"/>

# <a name="transferring-ownership-of-an-azure-subscription"></a>Eigendom van een abonnement op Azure overdragen

U volgt te werk:

- Moet u overgeven eigendom van uw Azure-abonnement aan iemand anders facturering?
- Wilt u het account dat is gebruikt om u te registreren voor Azure wijzigen? Misschien u uw Microsoft-Account gebruikt maar bedoeld om het gebruiken van uw werk- of schoolaccount in plaats daarvan weer?
- Wilt u uw abonnement op Azure vanuit een map verplaatsen naar een andere?
- Azure en Office 365 in verschillende tenants en wilt samenvoegen?

U kunt nu deze stap herhalen eenvoudig in de Microsoft Azure-Account beheercentrum - voor Pay-As-You-Go, MSDN, actie Pack of BizSpark abonnementen.  We hebt kunnen uw abonnement omzetten in een andere gebruiker toegevoegd. Met andere woorden, kunt u de beheerder van het account van elke Pay-As-You-Go, MSDN, actie Pack of BizSpark-abonnement dat u eigenaar bent, ongeacht welk land u werkt in nu wijzigen. We nu ondersteuning de overdracht van Azure Marketplace aankopen voor deze soorten abonnementen ook.

> [AZURE.NOTE] Zie voor meer informatie over het wijzigen van uw abonnement op een ander voorstel [schakeloptie uw Azure abonnement op een andere aanbieding](billing-how-to-switch-azure-offer.md) voor meer informatie. Als u meer hulp op een willekeurige plaats in dit artikel nodig hebt, neemt [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel opgelost.

## <a name="how-to-transfer-ownership-of-an-azure-subscription"></a>Hoe u het eigendom van een Azure-abonnement

> [AZURE.VIDEO transfer-an-azure-subscription]

1.  Meld u aan bij <https://account.windowsazure.com/Subscriptions>. U moet de beheerder van de account om uit te voeren de overdracht van een eigendom. Zie de [Veelgestelde vragen](#faq)voor meer informatie over hoe u erachter wie de accountbeheerder van het van het abonnement dat is.

2.  Selecteer het abonnement om over te brengen.

3.  Klik op de optie **Abonnement overbrengen** .

    ![Tabblad voor abonnementen van Azure-account](./media/billing-subscription-transfer/image1.png)

4.  Volg de aanwijzingen voor het opgeven van de geadresseerde.

    ![Dialoogvenster abonnement overbrengen](./media/billing-subscription-transfer/image2.PNG)

5.  De geadresseerde krijgen automatisch een e-mailbericht met een koppeling acceptatie.

    ![Abonnement doorverbinden e-mail naar geadresseerde](./media/billing-subscription-transfer/image3.png)

6.  De geadresseerde op de koppeling klikt en volgt de instructies, inclusief hun betalingsgegevens invoeren.

    ![Eerste pagina van het overdracht web abonnement](./media/billing-subscription-transfer/image4.png)

    ![Tweede pagina van het overdracht web abonnement](./media/billing-subscription-transfer/image5.png)

7. Succes! Het abonnement wordt nu worden overgedragen.

<a id="faq"></a>
## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen (FAQ)

-   **Hoe weet ik wie de accountbeheerder van het van het abonnement dat is?**

    U kunt controleren wie de accountbeheerder van het van het abonnement als volgt is:

    1. Meld u aan bij de [portal van Azure](https://portal.azure.com).
    2. Selecteer in het menu Hub **abonnement**.
    3. Selecteer het abonnement dat u wilt controleren en selecteer **Instellingen**.
    4. Selecteer **Eigenschappen**. De accountbeheerder van het van het abonnement dat wordt weergegeven in het vak **Account-beheerder** .  

-   **Resulteert de overdracht van een abonnement in elke service uitvaltijd?**

    Er is geen gevolgen voor de service. Dit effectief afdrukbewerking wordt het abonnement onder de huidige Account beheerder en maakt een nieuwe record onder account van de geadresseerde, maar de onderliggende Azure services worden gekoppeld aan het nieuwe abonnement. De abonnements-ID blijft ongewijzigd.

-   **Hoe gebruik ik deze om de map voor een abonnement wijzigen?**-   
    Een Azure-abonnement is gemaakt in de map die de beheerder van het Account behoort. Zo is, om te wijzigen van de map, net overbrengen het abonnement dat aan een gebruikersaccount in de doelmap. Wanneer die gebruiker voltooit de stappen voor het doorverbinden accepteren, wordt het abonnement wordt automatisch verplaatsen naar de doelmap.

-   **Als ik billing eigendom van een abonnement op een andere organisatie overnemen, blijft ze toegang hebben tot mijn resources?**

    Als het abonnement dat is overgebracht naar de andere tenant, gaan de gebruikers die zijn gekoppeld aan de vorige tenant toegang tot het abonnement verloren. Zelfs als een gebruiker niet een Service-beheerder of een Co-beheerders meer is, mogelijk ze nog steeds toegang tot het abonnement via andere regelingen beveiliging. Hierbij:
    - Beheer van certificaten die de gebruiker beheerder machtigen voor het abonnement resources Verleen. Zie voor meer informatie [maken en uploaden van Management certificaten van Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   Toegangstoetsen voor services zoals opslag. Zie voor meer informatie, [weergave, kopiëren en opnieuw genereren opslag-toegangstoetsen](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   Externe toegangsreferenties voor services zoals Azure virtuele Machines

    Dit is niet voor een volledige lijst. De geadresseerde rekening moet houden geen geheimen die is gekoppeld aan de service als ze nodig hebt voor het beperken van toegang tot hun bronnen bijwerken. De meeste resources kunnen als volgt worden bijgewerkt:

    1.   Ga naar de portal van Azure: [ *https://portal.azure.com*](https://portal.azure.com)

    2.    Klik op Bladeren All -&gt; alle Resources

    3.    Selecteer de resource. Hiermee opent u het blad voor de resource.

    4.    Klik in het blad resource op **Instellingen**. Hier kunt u bekijken en bijwerken van bestaande geheimen.


-   **Als ik het abonnement in het midden van de factureringscyclus overbrengt, de geadresseerde betaald wordt voor de hele facturering dan terugbladeren?**

    De afzender is verantwoordelijk voor betaling voor een gebruik die is gerapporteerd tot aan het moment dat de overdracht is voltooid. De geadresseerde is verantwoordelijk voor gebruik gerapporteerd vanaf het moment van hoger doorverbinden. Er zijn mogelijk enkele gebruik dat heeft plaatsgevonden vóór overdracht maar achteraf werd gemeld. Deze oplossing wordt opgenomen in van de geadresseerde factuur.

-   **Beschikt over de geadresseerde toegang tot gebruik en factureringsgeschiedenis?**

    Op dit moment is de enige informatie weergegeven aan de geadresseerde het bedrag van de laatste factuur (of het huidige saldo, als het abonnement dat is overgebracht voordat de eerste factuur is gegenereerd). De rest van het gebruik en factureringsgeschiedenis wordt niet overdragen aan het abonnement.

-   **Kan de aanbieding tijdens een transfer worden gewijzigd?**

    De aanbieding moet hetzelfde blijven. Als u wilt wijzigen van uw voorstel, moet u [contact opnemen met ondersteuning](http://go.microsoft.com/fwlink/?LinkID=619338).

-   **Kan ik een abonnement overzetten naar een gebruikersaccount in een ander land?**

    Nee, op dit moment die dit wordt niet ondersteund. Het gebruikersaccount van de geadresseerde moet zich in het hetzelfde land.

-   **Kan de geadresseerde de om van een andere betalingsoptie gebruiken?**

    Ja. Er zijn beperkingen hier: nu het abonnement factureringsgeschiedenis wordt gesplitst tussen twee accounts. Maar het voordeel dat u dit doen kunt zonder dat u moet [contact opnemen met ondersteuning](http://go.microsoft.com/fwlink/?LinkID=619338).

-   **De betalingsmethode worden beïnvloed nadat ik een abonnement op Azure overgebracht?**

    Om te kunnen accepteren de overdracht van een abonnement, moet een creditcard of een vergelijkbare betalingsmethode te betalen voor het abonnement worden opgegeven. Bijvoorbeeld als Stefan een abonnement naar Jane brengt en Jane de overdracht accepteert, moet Jane Daarnaast bieden een betalingsmethode die zij wordt gebruikt om te betalen voor het abonnement. Wanneer de overdracht voltooid is, moet niet langer Stefan betalen voor het abonnement dat hij naar Jane overgebracht.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Volgende stappen na het accepteren van eigendom van een abonnement

1. U bent nu de beheerder van het Account. Bekijken en bijwerken van de Service-beheerder en CO-beheerders. Beheerders in de [klassieke Azure-portal](https://manage.windowsazure.com) beheren door te gaan naar instellingen. [Meer informatie](http://go.microsoft.com/fwlink/?LinkID=533293).
2. U kunt ook Rolgebaseerd toegangsbeheer (RBAC) gebruiken voor uw abonnement en -services. Ga naar [meer informatie over RBAC](http://go.microsoft.com/fwlink/?LinkID=544802) [Azure-portal](https://portal.azure.com)
3. Referenties die zijn gekoppeld aan dit abonnement van services bijwerken. Hierbij:
    - Beheer van certificaten die de gebruiker beheerder machtigen voor het abonnement resources Verleen. Zie voor meer informatie [maken en uploaden een management-certificaat voor Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   Toegangstoetsen voor services zoals opslag. Zie voor meer informatie, [weergave, kopiëren en opnieuw genereren opslag-toegangstoetsen](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   Externe toegangsreferenties voor services zoals Azure virtuele Machines
4. Waarschuwingen voor dit abonnement, voor[meer informatie](http://go.microsoft.com/fwlink/?LinkID=533292) [Azure Account beheercentrum](https://account.windowsazure.com/Subscriptions)facturering bijwerken  
5.  Als u met een partner werkt, kunt u het bijwerken van de partner-ID op dit abonnement. U kunt dit doen in het [Beheercentrum van Azure-Account](https://account.windowsazure.com/Subscriptions).

> [AZURE.NOTE] Als u nog verdere vragen hebben, neemt [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om het probleem opgelost snel.
