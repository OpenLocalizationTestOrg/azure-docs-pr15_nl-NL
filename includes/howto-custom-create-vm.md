#<a name="how-to-create-a-custom-virtual-machine"></a>Het maken van een aangepaste virtuele Machine

Een *aangepaste* virtuele machine verwijst naar een virtuele machine die u met behulp van de methode **Uit galerie maakt** omdat hiermee u meer opties dan de methode **Snelle maken** configureren. Deze opties zijn:

- Meer mogelijkheden wanneer u de afbeelding wilt gebruiken om te maken van de virtuele machine (VM)
- De VM verbinden door een virtueel netwerk
- De VM toevoegen aan een bestaande cloudservice
- De VM toevoegen aan een set beschikbaarheid

> [AZURE.IMPORTANT] Als u wilt dat uw virtuele machines gebruik van een virtueel netwerk, zodat u verbinding met deze rechtstreeks door hostname of instellen van meerdere lokale verbindingen maken kunt, zorg er dan voor dat u het virtuele netwerk opgeven wanneer u de virtuele machine maakt. Een virtuele machine kan worden geconfigureerd als u wilt deelnemen aan een virtueel netwerk alleen wanneer u de virtuele machine maakt. Zie voor meer informatie over virtuele netwerken, [Azure virtuele netwerk overzicht](http://go.microsoft.com/fwlink/p/?LinkID=294063).

1. Meld u aan bij de [portal van Azure](http://manage.windowsazure.com).

2. Klik op de balk met opdrachten op **Nieuw**.

3. Klik op **berekenen** **VM**op en klik vervolgens op **Uit de galerie**.

4. Kies de afbeelding die u wilt gebruiken en klik vervolgens op de pijl om door te gaan.

5. Als u meerdere versies van de afbeelding zijn beschikbaar, in **De releasedatum versie**, kiest u de versie die u wilt gebruiken.

6. Typ in het vak **VM naam**de naam die u wilt gebruiken voor de virtuele machine.

7. Gebruik de **laag** en **grootte** om de juiste grootte voor de virtuele machine te selecteren. De grootte die u selecteert van invloed is op de maximale configuratie van de virtuele machine, evenals de prijzen. Zie voor meer informatie configuratie [VM en Cloud Service grootten voor Azure](http://go.microsoft.com/fwlink/p/?LinkID=389844).

8. Typ een naam voor de beheerdersaccount die u gebruiken wilt voor het beheren van de server in het vak **Nieuwe gebruikersnaam in te voeren**.

9. Typ in het vak **Nieuw wachtwoord**een sterk wachtwoord voor het beheerdersaccount. Typ het wachtwoord in het vak **Wachtwoord bevestigen**.

10. Klik op de pijl om door te gaan.

11. Voer een van de volgende opties in **Cloudservice binnenkomt**:

    - Als dit de eerste of alleen virtuele machine in de cloudservice, schakelt u **een nieuwe Cloudservice maken**in. In **De naam van de DNS-Service Cloud**, typ een naam die tussen 3 en 24 kleine letters en cijfers worden gebruikt. Deze naam maakt deel uit van de URI die wordt gebruikt voor het contact opnemen met de virtuele machine tot en met de cloudservice.
    - Als deze virtuele machine wordt toegevoegd aan een cloudservice, selecteert u deze in de lijst.

    > [AZURE.NOTE] Zie voor meer informatie over het plaatsen van virtuele machines in dezelfde cloudservice, [hoe u verbinding maakt virtuele machines in een cloudservice](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/).

12. Selecteer in de **Regio/affiniteit groep/virtuele netwerk**, regio, affiniteit groep of virtuele netwerk dat u wilt gebruiken voor de virtuele machine. Zie voor meer informatie over affiniteit groepen, [over affiniteit groepen voor Virtual Network](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

13. Selecteer een bestaand account opslag voor het bestand VHD in **Opslag-Account**, of gebruikt u een account automatisch gegenereerde opslag. Slechts één opslag account per regio wordt automatisch gemaakt. In dit opslag-account bevinden alle andere virtuele machines die u met deze instelling maakt. U zijn beperkt tot 20 opslag-accounts.

14. Als u wilt dat de virtuele machine deel uitmaakt van een set beschikbaarheid, klikt u in het **Beschikbaarheid instellen**, selecteert u **de beschikbaarheid van de maken instellen** of toe te voegen aan een bestaande beschikbaarheid set.

    **Opmerking**: virtuele machines in een set beschikbaarheid zijn geïmplementeerd in verschillende foutenstructuuranalyse domeinen. Meerdere virtuele machines plaatsen in een beschikbaarheid in set zorgt ervoor dat uw toepassing tijdens netwerkstoringen, problemen met de lokale schijf hardware en eventuele geplande downtime beschikbaar is.

15.  Controleer onder **eindpunten**, de nieuwe eindpunten die worden gemaakt als u wilt toestaan dat verbindingen met de virtuele machine, bijvoorbeeld via Extern bureaublad of een client Secure Shell (SSH). U kunt ook eindpunten nu toevoegen, of ze later te maken. Zie voor instructies over het maken van deze later, [het instellen van eindpunten naar een virtuele Machine](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

16.  Klik onder **VM Agent**, beslissen of de VM-Agent installeren. Deze agent biedt de omgeving voor u bij het installeren van extensies waarmee u kunnen werken met de virtuele machine. Zie [Extensies beheren](http://go.microsoft.com/FWLink/p/?LinkID=390493)voor meer informatie.

17. Klik op de pijl om het maken van de virtuele machine.

    ![Aangepaste VM maken is geslaagd](./media/howto-custom-create-vm/VMSuccessWindows.png)

##<a name="next-steps"></a>Volgende stappen##
Nadat de virtuele machine is gemaakt, wordt deze automatisch gestart. Wanneer de portal ziet u de status als voorlopig, kunt u zich aanmelden bij de virtuele machine. Zie voor instructies een van de volgende artikelen:

- [Hoe meld u aan bij een virtuele Machine waarop Linux](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)
- [Hoe meld u aan bij een virtuele Machine met Windows Server](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md)

