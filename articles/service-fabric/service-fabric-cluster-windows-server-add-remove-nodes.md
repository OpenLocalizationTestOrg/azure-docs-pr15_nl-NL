<properties
   pageTitle="Toevoegen of verwijderen van knooppunten aan een zelfstandige Service stof cluster | Microsoft Azure"
   description="Informatie over het toevoegen of verwijderen van knooppunten aan een Azure-Service stof cluster in een fysieke of virtuele machine met Windows Server, die on-premises implementatie kan zijn of in een cloud."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Toevoegen of verwijderen van knooppunten met een zelfstandig product Service stof cluster op Windows Server

Nadat u [uw cluster zelfstandige Service stof op computers met Windows Server gemaakt hebt](service-fabric-cluster-creation-for-windows-server.md), wordt uw zakelijke behoeften mogelijk wijzigen zodat u moet mogelijk toevoegen of verwijderen van meerdere knooppunten aan uw cluster. In dit artikel vindt u gedetailleerde stappen om te realiseren.


## <a name="add-nodes-to-your-cluster"></a>Knooppunten toevoegen aan uw cluster

1. Voorbereiden het VM/machine die u toevoegen aan uw cluster wilt volgens de stappen die worden genoemd in de sectie met [de computers om te voldoen aan de vereisten voor de implementatie van cluster voorbereiden](service-fabric-cluster-creation-for-windows-server.md#preparemachines) .
2. Welke foutenstructuuranalyse domein en een upgrade domein dat u wilt toevoegen van deze VM/computer om te plannen.
3. Extern bureaublad (RDP) in de VM/de computer waarop u wilt toevoegen aan het cluster.
4. Kopiëren of [de zelfstandige downloaden voor Service stof voor Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) naar deze VM/computer en pak het pakket.
5. Powershell uitvoeren als beheerder en Ga naar de locatie van het uitgepakt pakket.
6. *AddNode.ps1* Powershell uitvoeren met de parameters met een beschrijving van het nieuwe knooppunt om toe te voegen. Het volgende voorbeeld wordt een nieuw knooppunt aangeroepen VM5, met type NodeType0, IP-adres 182.17.34.52 in UD1 en FD1 toegevoegd. De *ExistingClusterConnectionEndPoint* is een verbindingseindpunt naar het knooppunt al in de bestaande cluster. Voor dit eindpunt, kunt u het IP-adres van *knooppunten* in het cluster.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## <a name="remove-nodes-from-your-cluster"></a>Knooppunten verwijderen uit uw cluster

1. Afhankelijk van het Reliablity niveau gekozen voor het cluster, u kunt de eerste n (3/5/7/9) knooppunten van het type primaire knooppunt niet verwijderen
2. RemoveNode opdracht uitvoeren op een cluster ontwikkelaar wordt niet ondersteund.
2. Extern bureaublad (RDP) in de VM/de computer waarop u wilt verwijderen uit het cluster.
2. Kopiëren of [gedownload van de zelfstandige Inpakken voor Service stof voor Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) en pak het pakket met deze VM/computer.
3. Powershell uitvoeren als beheerder en Ga naar de locatie van het uitgepakt pakket.
4. *RemoveNode.ps1* worden uitgevoerd in PowerShell. Het volgende voorbeeld wordt het huidige knooppunt verwijderd uit het cluster. De *ExistingClientConnectionEndpoint* is een client verbindingseindpunt voor elk knooppunt dat in het cluster blijft. Kies het IP-adres en de poort van het eindpunt van * **andere knooppunt** * in het cluster. Dit **andere knooppunt** werkt om de configuratie van het cluster voor het knooppunt verwijderd. 

```
.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000
```

Houd er rekening mee dat zich na het verwijderen van knooppunten, deze weergegeven mogelijk als wordt ingedrukt in query's en SFX. Dit is een bekende defect en wordt opgelost in een nieuwe versie. 


## <a name="next-steps"></a>Volgende stappen
- [Configuratie-instellingen voor de zelfstandige versie van Windows cluster](service-fabric-cluster-manifest.md)
- [Secure een zelfstandige cluster op Windows met behulp van Windows-beveiliging](service-fabric-windows-cluster-windows-security.md)
- [Een zelfstandige cluster op Windows met behulp van X509 Secure certificaten](service-fabric-windows-cluster-x509-security.md)
- [Maak een zelfstandige Service stof cluster met Azure VMs waarop Windows wordt uitgevoerd](service-fabric-cluster-creation-with-windows-azure-vms.md)
