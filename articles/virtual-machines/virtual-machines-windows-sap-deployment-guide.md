<properties
   pageTitle="SAP NetWeaver op Windows virtuele machines (VMs) – Implementatiehandleiding | Microsoft Azure"
   description="SAP NetWeaver op Windows virtuele machines (VMs) – Implementatiehandleiding"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-windows-virtual-machines-vms--deployment-guide"></a>SAP NetWeaver op Windows virtuele machines (VMs) – Implementatiehandleiding

[767598]:https://service.sap.com/sap/support/notes/767598
[773830]:https://service.sap.com/sap/support/notes/773830
[826037]:https://service.sap.com/sap/support/notes/826037
[965908]:https://service.sap.com/sap/support/notes/965908
[1031096]:https://service.sap.com/sap/support/notes/1031096
[1139904]:https://service.sap.com/sap/support/notes/1139904
[1173395]:https://service.sap.com/sap/support/notes/1173395
[1245200]:https://service.sap.com/sap/support/notes/1245200
[1409604]:https://service.sap.com/sap/support/notes/1409604
[1558958]:https://service.sap.com/sap/support/notes/1558958
[1585981]:https://service.sap.com/sap/support/notes/1585981
[1588316]:https://service.sap.com/sap/support/notes/1588316
[1590719]:https://service.sap.com/sap/support/notes/1590719
[1597355]:https://service.sap.com/sap/support/notes/1597355
[1605680]:https://service.sap.com/sap/support/notes/1605680
[1619720]:https://service.sap.com/sap/support/notes/1619720
[1619726]:https://service.sap.com/sap/support/notes/1619726
[1619967]:https://service.sap.com/sap/support/notes/1619967
[1750510]:https://service.sap.com/sap/support/notes/1750510
[1752266]:https://service.sap.com/sap/support/notes/1752266
[1757924]:https://service.sap.com/sap/support/notes/1757924
[1757928]:https://service.sap.com/sap/support/notes/1757928
[1758182]:https://service.sap.com/sap/support/notes/1758182
[1758496]:https://service.sap.com/sap/support/notes/1758496
[1772688]:https://service.sap.com/sap/support/notes/1772688
[1814258]:https://service.sap.com/sap/support/notes/1814258
[1882376]:https://service.sap.com/sap/support/notes/1882376
[1909114]:https://service.sap.com/sap/support/notes/1909114
[1922555]:https://service.sap.com/sap/support/notes/1922555
[1928533]:https://service.sap.com/sap/support/notes/1928533
[1941500]:https://service.sap.com/sap/support/notes/1941500
[1956005]:https://service.sap.com/sap/support/notes/1956005
[1973241]:https://service.sap.com/sap/support/notes/1973241
[1984787]:https://service.sap.com/sap/support/notes/1984787
[1999351]:https://service.sap.com/sap/support/notes/1999351
[2002167]:https://service.sap.com/sap/support/notes/2002167
[2015553]:https://service.sap.com/sap/support/notes/2015553
[2039619]:https://service.sap.com/sap/support/notes/2039619
[2121797]:https://service.sap.com/sap/support/notes/2121797
[2134316]:https://service.sap.com/sap/support/notes/2134316
[2178632]:https://service.sap.com/sap/support/notes/2178632
[2191498]:https://service.sap.com/sap/support/notes/2191498
[2233094]:https://service.sap.com/sap/support/notes/2233094
[2243692]:https://service.sap.com/sap/support/notes/2243692

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription

[dbms-guide]:virtual-machines-windows-sap-dbms-guide.md (SAP NetWeaver op Windows virtuele machines (VMs) – DBMS Implementatiehandleiding) [dbms-guide-2.1]:virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (cache voor VMs en VHD's) [dbms-guide-2.2]:virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software RAID) [dbms-guide-2.3]:virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage) [dbms-guide-2]:virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (structuur van een RDBMS-implementatie) [dbms-guide-3]:virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (beschikbaarheid en herstel met Azure VMs) [dbms-guide-5.5.1]:virtual-machines-windows-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 en hoger) [dbms-guide-5.5.2]:virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 en eerdere versies) [dbms-guide-5.6]:virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (met een SQL Server-afbeeldingen uit Microsoft Azure Marketplace) [dbms-guide-5.8]:virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (SQL Server algemeen voor SAP op Azure overzicht) [dbms-guide-5]:virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (manier naar SQL Server RDBMS) [dbms-guide-8.4.1]:virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (opslagconfiguratie) [dbms-guide-8.4.2]:virtual-machines-windows-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (back-up maken en terugzetten) [dbms-guide-8.4.3]:virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Prestatieoverwegingen voor back-up en herstellen) [dbms-guide-8.4.4]:virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (overige) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-windows-sap-deployment-guide.md (SAP NetWeaver op Windows virtuele machines (VMs) – Implementatiehandleiding) [deployment-guide-2.2]:virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP Resources) [deployment-guide-3.1.2]:virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (implementeert een VM met een aangepaste afbeelding) [deployment-guide-3.2]:virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Scenario 1: een VM afmelden bij de Azure Marketplace voor SAP implementeert) [deployment-guide-3.3]:virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Scenario 2: een VM met een aangepaste afbeelding implementeren voor SAP) [deployment-guide-3.4]:virtual-machines-windows-sap-deployment-guide.md# a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Scenario 3: het verplaatsen van een VM vanuit on-premises met een niet-generalized Azure-VHD met SAP) [deployment-guide-3]:virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (implementatie scenario's van VMs voor SAP op Microsoft Azure) [deployment-guide-4.1]:virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (implementeert Azure PowerShell-cmdlets) [deployment-guide-4.2]:virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (downloaden en importeren SAP relevante PowerShell-cmdlets) [deployment-guide-4.3]:virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (deelnemen aan VM in on-premises domein - alleen Windows) [deployment-guide-4.4.2]:virtual-machines-windows-sap-deployment-guide.md# 6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [deployment-guide-4.4]:virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (downloaden, installeren en activeren Azure VM Agent) [deployment-guide-4.5.1]:virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell) [deployment-guide-4.5.2]:virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI) [deployment-guide-4.5]:virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (configureren Azure Enhanced Monitoring-extensie voor SAP) [deployment-guide-5.1]:virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (gereedheidscontrole voor Azure Enhanced Monitoring voor SAP) [implementatie-handleiding-5,2]: Virtual-machines-Windows-SAP-Deployment-Guide.MD#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Statuscontrole voor Azure Monitoring infrastructuur configuratie) [deployment-guide-5.3]:virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (verder probleemoplossing van Azure Monitoring infrastructuur voor SAP)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:virtual-machines-windows-sap-high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-windows-sap-planning-guide.md (SAP NetWeaver op Windows virtuele machines (VMs) – Planning en implementatiehandleiding) [planning-guide-1.2]:virtual-machines-windows-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Resources) [planning-guide-11]:virtual-machines-windows-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (hoge beschikbaarheid (HA) en noodgevallen herstel (DR) voor SAP NetWeaver uitgevoerd op Azure virtuele Machines) [planning-guide-11.4.1]:virtual-machines-windows-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (beschikbaarheid voor SAP-Servers van toepassing) [planning-guide-11.5]:virtual-machines-windows-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (gebruik starten voor SAP-exemplaren) [planning-guide-2.1]:virtual-machines-windows-sap-planning-guide.md# 1625df66-4CC6-4d60-9202-de8a0b77f803 (alleen de Cloud - VM implementaties in Azure zonder afhankelijkheden op het lokale customer network) [planning-guide-2.2]:virtual-machines-windows-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Cross-premises - implementatie van één of meerdere SAP VMs in Azure met de vereiste van volledig geïntegreerd in de on-premises netwerk) [planning-guide-3.1]:virtual-machines-windows-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure regio's) [planning-guide-3.2.1]:virtual-machines-windows-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (domeinen foutenstructuuranalyse) [planning-guide-3.2.2]:virtual-machines-windows-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (domeinen upgraden) [planning-guide-3.2.3]:virtual-machines-windows-sap-planning-guide.md#18810088- F9BE - 4c 97-958a - 27996255c 665 (Azure beschikbaarheid Sets) [planning-guide-3.2]:virtual-machines-windows-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (het Microsoft Azure Virtual Machine Concept) [planning-guide-3.3.2]:virtual-machines-windows-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium opslag) [planning-guide-5.1.1]:virtual-machines-windows-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (een VM vanuit on-premises naar verplaatst Azure op een dvd niet-generalized) [planning-guide-5.1.2]:virtual-machines-windows-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (implementeert een VM met een bepaalde afbeelding van de klant) [planning-guide-5.2.1]:virtual-machines-windows-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (voorbereiden voor het verplaatsen van een VM vanuit on-premises naar Azure met een niet-generalized schijf) [planning-guide-5.2.2]:virtual-machines-windows-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (voorbereiden voor de implementatie van een VM met een bepaalde afbeelding van de klant voor SAP) [planning-guide-5.2]:virtual-machines-windows-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (voorbereiden VMs met SAP voor Azure) [planning-guide-5.3.1]:virtual-machines-windows-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (verschil tussen een Azure schijf en Azure afbeelding) [planning-guide-5.3.2]:virtual-machines-windows-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (uploaden van een VHD vanuit on-premises naar Azure) [planning-guide-5.4.2]:virtual-machines-windows-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (schijven kopiëren tussen Azure opslag-Accounts) [planning-handleiding-5.5.1]: Virtual-machines-Windows-SAP-Planning-Guide.MD#4efec401-91e0-40c0-8e64-f2dceadff646 (VM/VHD structuur voor SAP-implementaties) [planning-guide-5.5.3]:virtual-machines-windows-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (instelling automount voor gekoppelde schijven) [planning-guide-7.1]:virtual-machines-windows-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (één VM met SAP NetWeaver demo/training scenario) [planning-guide-7]:virtual-machines-windows-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (concepten van Cloud-Only-implementatie van SAP-exemplaren) [planning-guide-9.1]:virtual-machines-windows-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure Monitoring oplossing voor SAP) [planning-guide-azure-premium-storage]:virtual-machines-windows-sap-planning-guide.md# ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium opslag)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:virtual-machines-windows-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-windows-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../storage/storage-premium-storage.md
[storage-redundancy]:../storage/storage-redundancy.md
[storage-scalability-targets]:../storage/storage-scalability-targets.md
[storage-use-azcopy]:../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:virtual-machines-linux-attach-disk-portal.md
[virtual-machines-windows-attach-disk-portal]:virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:virtual-machines-windows-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

Microsoft Azure kunnen bedrijven in het bezit van berekeningscluster en opslagruimte resources in minimale tijd zonder lange aanschaf maal. Azure virtuele Machines kunnen bedrijven implementeren klassieke toepassingen, zoals SAP NetWeaver op basis van Azure van toepassingen en hun betrouwbaarheid en beschikbaarheid weergeven zonder verder resources beschikbaar on-premises implementatie uitbreiden. Microsoft Azure ondersteunt ook cross-premises connectivity kan bedrijven actief Azure virtuele Machines integreren met de domeinen van hun on-premises implementatie, hun privé-wolken en hun liggend SAP-systeem.

Dit artikel wordt beschreven stap voor stap hoe Azure virtuele machines is voorbereid voor de implementatie van SAP NetWeaver op basis van toepassingen. Dit wordt ervan uitgegaan dat de gegevens in [Planning en implementatiehandleiding opgenomen] [-Planningshandleiding] bekend is. Als dit niet het geval is, wordt het desbetreffende document eerst worden gelezen.

Het papier is een aanvulling op de documentatie van SAP-installatie en SAP-notities die staan voor de primaire bronnen voor installaties en implementaties van SAP-software op de opgegeven platforms.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="introduction"></a>Inleiding
Een groot aantal wereldwijd bedrijven SAP NetWeaver op basis van toepassingen – meest prominent het SAP Business Suite – gebruiken om uit te voeren hun opdracht kritieke bedrijfsprocessen. Systeemstatus is dus een essentieel activa en de mogelijkheid om de enterprise-ondersteuning in geval van een storing, inclusief prestaties incidenten, wordt een belangrijke vereiste.
Microsoft Azure biedt bovenliggende platform instrumentation zodat de vereisten van de ondersteuning van alle kritieke bedrijfstoepassingen. Deze handleiding zorgt ervoor dat een Microsoft Azure Virtual Machine bedoeld voor de implementatie van SAP-Software is geconfigureerd dat enterprise-ondersteuning kan worden aangeboden, ongeacht welke manier de virtuele Machine wordt gemaakt, deze die u afmelden bij de Azure Marketplace hebt gemaakt worden of door een bepaalde afbeelding van de klant.
Bij het instellen van de volgende, alle benodigde worden stappen beschreven in detail.

## <a name="prerequisites-and-resources"></a>Vereisten en bronnen
### <a name="prerequisites"></a>Vereisten voor
Voordat u begint, zorg dat de vereisten die de beschreven in de volgende hoofdstukken zijn wordt voldaan.

#### <a name="local-personal-computer"></a>Lokale pc
De instellingen van een Azure Virtual Machine voor SAP-Software-implementatie omvat van enkele stappen uitvoeren. Als u wilt beheren Windows VMs of Linux VMs, moet u een PowerShell-script en de Portal van Microsoft Azure gebruiken. Voor die is een lokale pc met Windows 7 of hoger nodig. Als u alleen wilt Linux VMs beheren en een Linux-computer gebruiken voor deze taak wilt, kunt u ook de Azure opdrachtregelinterface (Azure).

#### <a name="internet-connection"></a>Internetverbinding
Als u wilt downloaden en uitvoeren van de vereiste hulpmiddelen en scripts, is een internetverbinding vereist. Microsoft Azure Virtual Machine met de extensie cmdlets voor controle van Azure Enhanced moet bovendien toegang tot Internet. Als deze VM Azure deel van een Azure Virtual Network of on-premises domein uitmaakt, zorg dat de relevante proxy-instellingen zijn ingesteld, zoals beschreven in hoofdstuk [Proxy configureren] [ deployment-guide-configure-proxy] in dit document.

#### <a name="microsoft-azure-subscription"></a>Microsoft Azure-abonnement
Er bestaat al een Azure-account en volgens aanmeldingsreferenties bekend zijn.

#### <a name="topology-consideration-and-networking"></a>Topologie aandacht en netwerken
De topologie en de architectuur van de SAP-implementatie in Azure moet deze velden worden gedefinieerd. Architectuur met betrekking tot:

* Microsoft Azure Storage (s) moet worden gebruikt
* Virtuele netwerk om te implementeren in het SAP-systeem
* Resourcegroep om te implementeren in het SAP-systeem
* Azure regio om te implementeren van het SAP-systeem
* SAP-configuratie (2-laag of lagen 3)
* VM/maten en het aantal aanvullende VHD's moet worden gekoppeld aan de VM(s)
* SAP Transport en correctie systeemconfiguratie

Azure opslag Accounts of Azure virtuele netwerken moet als zodanig zijn gemaakt en al geconfigureerd. Het maken en configureren van deze valt [Planning en implementatiehandleiding] [-Planningshandleiding].

#### <a name="sap-sizing"></a>SAP formaatgrepen
* De werklast van de verwachte SAP is bepaald, bijvoorbeeld met behulp van het SAP-Quicksizer, en het volgens SAP's getal is bekend 
* De vereiste resource en geheugen processorgebruik van het SAP-systeem had moet worden
* De vereiste i/o-bewerkingen per seconde had moeten worden
* De vereiste netwerkbandbreedte in eventuele communicatie tussen verschillende VMs in Azure bekend is
* De vereiste netwerkbandbreedte tussen de on-premises implementatie-activa en de Azure geïmplementeerd SAP systemen bekend is

#### <a name="resource-groups"></a>Resourcegroepen
Resourcegroepen zijn een nieuwe concept dat alle bronnen die u hebt de levenscyclus van dezelfde zoals ze zijn gemaakt en verwijderde op hetzelfde moment bevatten. Lees [dit artikel] [ resource-group-overview] voor meer informatie over resourcegroepen. 

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP-Resources
Tijdens de configuratiewerk, zijn de volgende bronnen nodig:

* SAP notitie [1928533]
    * de lijst met Azure virtuele machines grootten, die worden ondersteund voor de implementatie van SAP-Software 
    * informatie over belangrijke capaciteit per Azure virtuele machines grootte
    * ondersteunde SAP-software en combinatie van besturingssysteem en DB
* SAP: notitie [2015553] vermelding vereisten voor het SAP worden ondersteund, bij het implementeren van SAP-software in Microsoft Azure.
* SAP: notitie [1999351] met aanvullende informatie over probleemoplossing voor de Enhanced Azure controle voor SAP.
* SAP: notitie [2178632] met gedetailleerde informatie over alle beschikbare controleren doelstellingen voor SAP op Microsoft Azure. 
* SAP: notitie [1409604] met de vereiste SAP Host-Agent-versie voor Windows op Microsoft Azure bij het distribueren van de nieuwe Azure Resource Manager.
* SAP: notitie [2191498] met de vereiste SAP Host-Agent-versie voor Linux op Microsoft Azure bij het distribueren van de nieuwe Azure Resource Manager.
* SAP notitie [2243692] met informatie over licenties voor SAP op Linux op Azure
* SAP: notitie [1984787] die bevat algemene informatie over SUSE LINUX Enterprise Server 12
* SAP: notitie [2002167] met algemene informatie Red rol Enterprise Linux 7.x
* [SCN](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) waarin alle vereist SAP notities voor Linux
* SAP specifieke PowerShell-cmdlets die deel uitmaken van de [Azure PowerShell][azure-ps]
* Het specifieke Azure CLI SAP, die deel uitmaken van de [Azure CLI][azure-cli]
* [Microsoft Azure-Portal][azure-portal]

[comment]: <> (MSSedusch taak toevoegen ARM patchniveau voor SAP-Host Agent in SAP notitie 1409604)
 
Als u ook het onderwerp van SAP op Microsoft Azure betrekking hebben op de volgende handleidingen:

* [SAP NetWeaver op Windows virtuele machines (VMs) – Planning en implementatiehandleiding] [-Planningshandleiding]
* [SAP NetWeaver op Windows virtuele machines (VMs) – Implementatiehandleiding (in dit document)] [Implementatiehandleiding]
* [SAP NetWeaver op Windows virtuele machines (VMs) – DBMS Implementatiehandleiding] [dbms-handleiding]
* [SAP NetWeaver op Windows virtuele Machines (VMs) – Implementatiehandleiding van beschikbaarheid][ha-guide]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Scenario's voor implementatie van VMs voor SAP op Microsoft Azure
In dit hoofdstuk leert u de verschillende manieren van implementatie en de enkele stappen voor elk implementatietype.

### <a name="deployment-of-vms-for-sap"></a>Implementatie van VMs van SAP
Microsoft Azure biedt meerdere manieren om te implementeren VMs en bijbehorende schijven. Daarmee ook is het belangrijk is voor meer informatie over de verschillen, aangezien voorbereidingen van de VMs afhankelijk van de manier van implementatie verschillen kunnen. We kijken in het algemeen, in de volgende scenario's:

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Een VM afmelden bij de Azure Marketplace implementeren
Wilt u een Microsoft of 3e partijen afbeelding uit de Azure Marketplace opgegeven om te implementeren van uw VM. Nadat u uw VM op Microsoft Azure geïmplementeerd, volgt u dezelfde richtlijnen en hulpmiddelen voor het installeren van de software SAP binnen uw VM zoals u in een on-premises omgeving doen zou. Voor de installatie van de software SAP binnen de VM Azure, raden SAP en Microsoft voor uploaden en bewaren van de SAP-installatiemedia in Azure VHD's of maken van een VM Azure werken als een 'bestandsserver' waarin alle de benodigde SAP installatiemedia.

[comment]: <> (MSSedusch TODO waarom hebben we nodig om aan te raden een Bestandsbeheer bijvoorbeeld bestandsserver of VHD? Waarin verschilt die dus vanuit on-premises?)

Voor meer informatie raadpleegt hoofdstuk [Scenario 1: een VM afmelden bij de Azure Marketplace voor SAP implementeert] [implementatie-handleiding-3,2].

#### <a name="3688666f-281f-425b-a312-a77e7db2dfab"></a>Een VM met een aangepaste afbeelding implementeren
De meegeleverde afbeeldingen uit de Azure Marketplace mogelijk vanwege specifieke patch vereisten met betrekking tot uw OS of DBMS-versie, niet aan uw wensen voldoet. Daarom moet u mogelijk een VM door uw eigen 'Privé' OS/DB VM afbeelding waarop kan worden geïmplementeerd enkele malen achteraf maken.
De stappen voor het maken van een privé verschillen tussen een Windows Phone en de afbeelding van een Linux.

___

> ![Windows][Logo_Windows] Windows
>
> Als u wilt voorbereiden op een Windows-afbeelding die kan worden gebruikt om meerdere virtuele machines implementeren, moet de Windows-instellingen (zoals Windows beveiligings-id en hostname) op de on-premises implementatie VM geabstraheerd/generalized. U kunt dit doen met sysprep zoals beschreven op <https://technet.microsoft.com/library/cc721940.aspx>.
>
> ![Linux][Logo_Linux] Linux
>
> Als u wilt een Linux-afbeelding die kan worden gebruikt om te implementeren van meerdere virtuele machines voorbereiden, moet enkele Linux-instellingen op de on-premises implementatie VM geabstraheerd/generalized. U kunt dit doen met waagent-deprovision zoals is beschreven in [dit artikel] [ virtual-machines-linux-capture-image] of in [dit artikel][virtual-machines-linux-agent-user-guide-command-line-options].

___

U kunt de inhoud van uw database instellen met behulp van de Manager inrichten van SAP Software installeren van een nieuwe SAP-systeem, een back-up van database terugzetten uit een VHD die is gekoppeld aan de virtuele machine of een back-up van database rechtstreeks herstellen uit Azure opslag als het DBMS dit ondersteunt. (Zie de [DBMS implementatie Guide][dbms-guide]). Als u al een SAP-systeem in uw on-premises implementatie VM (met name voor 2 niveaus systemen) hebt geïnstalleerd, kunt u de instellingen van het SAP-systeem aanpassen na de implementatie van Azure VM tot en met de naam van systeem procedure wordt ondersteund door de SAP Software inrichting Manager (SAP notitie [1619720]). Anders kunt u de SAP-software installeren later na de implementatie van Azure VM.

Voor meer informatie raadpleegt hoofdstuk [Scenario 2: een VM met een aangepaste afbeelding implementeren voor SAP] [implementatie-handleiding-3.3].

#### <a name="moving-a-vm-from-on-premises-to-microsoft-azure-with-a-non-generalized-disk"></a>Een VM verplaatsen vanuit on-premises implementatie naar de Microsoft Azure op een dvd niet-generalized
U van plan bent een specifieke SAP-systeem van on-premises implementatie naar Microsoft Azure verplaatsen. Dit kan worden uitgevoerd door de VHD waarin het besturingssysteem, de binaire bestanden van SAP en eventuele DBMS binaire bestanden plus de VHD's met de bestanden die gegevens- en logbestanden van de DBMS naar Microsoft Azure uploaden. In omgekeerde het scenario wordt beschreven in hoofdstuk [implementeert een VM met een aangepaste afbeelding] [implementatie-handleiding-3.1.2] hierboven, kunt u het behouden de hostname, SAP beveiligings-id en SAP-gebruikersaccounts in Azure VM zoals deze zijn geconfigureerd in de on-premises omgeving. Noemt van het besturingssysteem is dus niet nodig. In dit geval wordt hoofdzakelijk toepassen voor cross-premises scenario's waarin een deel van het landschap SAP on-premises en delen op Microsoft Azure wordt uitgevoerd.

Voor meer informatie raadpleegt hoofdstuk [Scenario 3: het verplaatsen van een VM vanuit on-premises met een niet-generalized Azure-VHD met SAP] [implementatie-handleiding-3.4].

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>Scenario 1: Een VM afmelden bij de Azure Marketplace voor SAP implementeren
Microsoft Azure biedt de mogelijkheid om te implementeren van een exemplaar VM afmelden bij de Azure Marketplace, dat sommige standaardafbeeldingen OS van Windows Server en andere Linux onderzoeken bevat. Het is ook mogelijk om te implementeren van een afbeelding met DBMS SKU's zoals SQL Server. Raadpleeg voor meer informatie het gebruik van deze afbeeldingen met DBMS SKU's de [DBMS Implementatiehandleiding] [dbms-handleiding]

De SAP specifieke reeks stappen een VM afmelden bij de Azure Marketplace implementeert eruit als:

![Stroomdiagram van VM implementatie van SAP-systemen met behulp van de afbeelding van een VM van Azure Marketplace][deployment-guide-figure-100]

Na het stroomdiagram moeten de volgende stappen worden uitgevoerd:

#### <a name="create-virtual-machine-using-the-azure-portal"></a>Virtuele machine met behulp van de Azure-Portal maken
De eenvoudigste manier om te maken van een nieuwe virtuele machine gebruik van een afbeelding van Azure Marketplace is via de Portal Azure. Navigeer naar <https://portal.azure.com/#create>. Voer het type besturingssysteem dat u wilt implementeren in het zoekveld, bijvoorbeeld Windows, SLES of RHEL en selecteer de versie. Zorg ervoor dat de implementatiemodel "Resourcemanager Azure" selecteren en klik op maken.

De wizard begeleidt u bij de vereiste parameters voor het maken van de virtuele machine samen met alle vereiste resources zoals netwerkinterfaces of opslag-accounts. Sommige van deze parameters zijn:

1. Basisbeginselen
    1. Naam: De naam van de resource dat wil zeggen de naam van de virtuele machines
    1. Gebruikersnaam en wachtwoord/SSH openbare sleutel: Geef de gebruikersnaam en wachtwoord van de gebruiker die is gemaakt tijdens de inrichting van. Voor een Linux virtuele machine, kunt u ook de openbare SSH-sleutel die u wilt gebruiken invoeren om aan te melden bij de computer met SSH.
    1. Abonnement: Selecteer het abonnement dat u gebruiken wilt voor het inrichten van de nieuwe virtuele machine.
    1. Resourcegroep: De naam van het resourceveld groep. U kunt de naam van een nieuwe resourcegroep of van een resourcegroep die al bestaat invoegen
    1. Locatie: Selecteer de locatie waar de nieuwe virtuele machine moet worden geïmplementeerd. Als u wilt de virtuele machine verbinden met uw netwerk on-premises implementatie, zorgt u ervoor dat u selecteert u de locatie van het virtuele netwerk maakt die Azure verbinding met uw on-premises netwerk. Zie voor meer informatie hoofdstuk [Microsoft Azure toegang] [ planning-guide-microsoft-azure-networking] in de [Planningshandleiding] [-Planningshandleiding].
1. Grootte: Lees SAP notitie [1928533] voor een lijst met ondersteunde VM typen. Ook Zorg ervoor dat het juiste type als u wilt gebruiken Premium opslag selecteert. Niet alle ondersteund VM Premium opslag. Zie hoofdstuk [opslag: Microsoft Azure Storage en gegevensschijven] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] en [Azure Premium opslagruimte] [planning-handleiding-azure-premium-opslagruimte] in de [Planningshandleiding] [-Planningshandleiding] voor meer informatie.
1. Instellingen
    1. Opslag-Account: Die u kunt selecteren van een bestaand opslag-account of een nieuw account te maken. Lees hoofdstuk [Microsoft Azure Storage] [dbms-handleiding-2.3] van [DBMS handleiding] [dbms-handleiding] voor meer informatie over de verschillende opslagtypen. Houd er rekening mee dat niet alle typen van de opslagruimte voor het uitvoeren van SAP-toepassingen worden ondersteund.
    1. Virtuele netwerk- en Subnet: Selecteer het virtuele netwerk die is gekoppeld aan uw on-premises netwerk als u wilt de virtuele machine integreren met uw intranet.
    1. Openbare IP-adres: Selecteer het openbare IP-adres dat u wilt gebruiken of de parameters invoeren om te maken van een nieuwe openbare IP-adres. U kunt een openbare IP-adres gebruiken voor toegang tot uw VM via internet. Zorg ervoor dat u ook een beveiligingsgroep netwerk als u wilt filteren toegang tot uw VM maken.
    1. Beveiligingsgroep netwerk: Zie [Wat is een netwerk beveiliging groep (NSG)] [ virtual-networks-nsg] voor meer informatie.
    1. Voor controle: U kunt de instelling diagnostische hulpprogramma's uitschakelen. Deze automatisch worden ingeschakeld als u de opdrachten om in te schakelen van de Azure Enhanced bewaken zoals beschreven in hoofdstuk [Configureren voor controle]uitvoeren[deployment-guide-configure-monitoring-scenario-1].
    1. Beschikbaarheid: Selecteer een beschikbaarheid instellen, of Voer de parameters om een nieuwe beschikbaarheid Set te maken. Zie voor meer informatie hoofdstuk [Azure beschikbaarheid Sets] [planning-handleiding-3.2.3].
1. Overzicht: De informatie op de overzichtspagina valideren en klik op OK.

Nadat u klaar bent met de wizard, wordt uw VM geïmplementeerd in de resourcegroep die u hebt geselecteerd.

#### <a name="create-virtual-machine-using-a-template"></a>Virtuele machine met een sjabloon maken
U kunt ook een implementatie met een van de SAP-sjablonen die zijn gepubliceerd in de [bibliotheek van azure-quickstart-sjablonen github]maken[azure-quickstart-templates-github]. U kunt maken met behulp van de [Portal van Azure]virtuele machines[virtual-machines-windows-tutorial], [PowerShell] [ virtual-machines-ps-create-preconfigure-windows-resource-manager-vms] of [Azure CLI] [ virtual-machines-linux-tutorial] handmatig.

* [2-laag (slechts één virtuele machine) configuratiesjabloon] [ sap-templates-2-tier-marketplace-image] Gebruik deze sjabloon als u wilt maken van een 2-laag systeem met slechts één virtuele machine.
* [3-laag (meerdere virtuele machines) configuratiesjabloon] [ sap-templates-3-tier-marketplace-image] Gebruik deze sjabloon als u wilt een 3-laag systeem met meerdere virtuele machines maken.

Nadat u een van de bovenstaande sjablonen hebt geopend, wordt de Portal Azure navigeert naar het deelvenster Parameters bewerken. Voer de volgende gegevens:

* **sapSystemId**: SAP-systeem-ID
* **waardereeksen**: besturingssysteem dat u implementeren wilt, bijvoorbeeld Windows Server 2012 R2, SLES 12 of RHEL 7.2
    * De lijst bevat alleen versies die worden ondersteund door SAP op Microsoft Azure
* **sapSystemSize**: de grootte van het SAP-systeem
    * Het bedrag van SAP's u in het nieuwe systeem vindt. Als u niet zeker weet hoeveel SAP's worden het systeem moet, vraag uw SAP technologie Partner of System Integrator
* **systemAvailability**: (alleen 3 laag sjabloon) beschikbaarheid van het systeem 
    * Selecteer HA voor een configuratie die geschikt is voor een HA-installatie. Twee servers webdatabase en twee servers voor de ASC's wordt gemaakt.
* storageType: (alleen 2 niveaus sjabloon) Type opslag die moet worden gebruikt 
    * Het gebruik van de Premium-opslag wordt ten zeerste aanbevolen voor groter systemen. Lees voor meer informatie over de van verschillende opslagtypen 
        * [Microsoft Azure opslag] [dbms-handleiding-2.3] van [DBMS handleiding] [dbms-handleiding]
        * [Premium-opslag: High-Performance opslag voor Azure virtuele machines werkbelasting][storage-premium-storage-preview-portal]
        * [Inleiding tot Microsoft Azure-opslag][storage-introduction]
* **adminUsername** en **beheerderswachtwoord**: gebruikersnaam en wachtwoord
    * Een nieuwe gebruiker wordt gemaakt die kan worden gebruikt voor aanmelding bij de computer.
* **newOrExistingSubnet**: bepaalt of een nieuw virtueel netwerk en subnet moeten worden gemaakt of een bestaand subnet moet worden gebruikt. Als u al een virtueel netwerk die is gekoppeld aan uw on-premises netwerk, selecteert u de bestaande.
* **subnetId**: de ID van het subnet waaraan de virtuele machines moet worden aangesloten. Selecteer het subnet van VPN Express Route virtuele netwerk van uw of de virtuele machine verbinden met uw on-premises netwerk. /Subscriptions/ de ID meestal eruit`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Nadat u alle parameters hebt ingevoerd, selecteert u het abonnement en de resourcegroep die u wilt gebruiken. U kunt een bestaande resourcegroep selecteren of een nieuwe record door te selecteren 'en nieuwe' maken in de vervolgkeuzelijst. Als u een nieuwe resourcegroep maakt, moet u ook selecteren de regio waar de resourcegroep en de virtuele machine worden gemaakt.

Bekijk de juridische voorwaarden, accepteren en klik op maken.

Houd er rekening mee dat de Azure VM-Agent al dan niet standaard wordt geïmplementeerd bij gebruik van een afbeelding van Azure Marketplace.

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren
Afhankelijk van uw on-premises implementatie-netwerkconfiguratie, is het mogelijk dat deze verplicht voor het configureren van de proxy op uw VM als deze is verbonden met uw on-premises netwerk via een VPN- of Express doorsturen. Anders de virtuele machine mogelijk geen toegang krijgen tot internet en kunnen daarom niet kunt downloaden van de vereiste extensies of controlegegevens verzamelen. Zie hoofdstuk [Proxy configureren] [ deployment-guide-configure-proxy] van dit document.

#### <a name="join-domain-windows-only"></a>Deelnemen aan domein (alleen Windows)
In het geval dat de implementatie in Azure is verbonden met het lokale AD/DNS via Azure Site-naar-Site of Express Route (ook waarnaar wordt verwezen als Cross-premises implementatie in de [Planning en implementatie Guide][planning-guide]), wordt naar verwachting dat de VM lid van een on-premises domein worden. Overwegingen van deze stap worden beschreven in hoofdstuk [deelnemen aan VM in on-premises domein (alleen Windows)] [implementatie-handleiding-4,3] van dit document.

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Cmdlets voor controle configureren
De Azure Enhanced Monitoring-extensie voor SAP configureren, zoals beschreven in hoofdstuk [configureren Azure Enhanced Monitoring-extensie voor SAP] [implementatie-handleiding-4.5] van dit document.

Controleer de vereisten voor het controleren van SAP voor vereiste minimale versies van SAP Kernel en SAP Host-Agent in de resources in hoofdstuk SAP Resources [implementatie-handleiding-2.2] van dit document.

#### <a name="monitoring-check"></a>Selectievakje bewaken
Controleren of de cmdlets voor controle werkt zoals beschreven in hoofdstuk [Hiermee wordt gecontroleerd en probleemoplossing voor End-to-End controle-instellingen voor SAP op Azure][deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Stappen voor post-implementatie
Na het maken van de VM, deze wordt geïmplementeerd en klik vervolgens op u bij het installeren van alle benodigde software-onderdelen naar de VM is. Dit soort VM implementatie moet dus beide de software moeten geïnstalleerde beeing al beschikbaar in Microsoft Azure in enkele andere VM of als de schijf die kan worden gekoppeld. Of we op zoek bent naar meerdere lokale scenario's waarin de connectiviteit met de on-premises implementatie activa (installeren waarden voor aandelen) is een opgegeven.

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>Scenario 2: Een VM met een aangepaste afbeelding implementeren voor SAP
Zoals is beschreven in [Planning en implementatiehandleiding] [-Planningshandleiding] al in gedetailleerde stappen zijn er is een manier om te bereiden en een aangepaste afbeelding maken en hiermee meerdere nieuwe VMs maken. De volgorde van stappen in het stroomdiagram eruit als:
 
![Stroomdiagram van VM implementatie van SAP-systemen met een afbeelding VM in privé Marketplace][deployment-guide-figure-300]

Na het stroomdiagram moeten de volgende stappen worden uitgevoerd:

#### <a name="create-virtual-machine"></a>Virtuele machine maken
Als u wilt een installatie die door een privé OS afbeelding via de Portal Azure maakt, gebruikt u een van de SAP-sjablonen die zijn gepubliceerd in de [bibliotheek van azure-quickstart-sjablonen github][azure-quickstart-templates-github].
U kunt ook een virtuele machine via de [PowerShell] maken[ virtual-machines-upload-image-windows-resource-manager] handmatig. 

* [2-laag (slechts één virtuele machine) configuratiesjabloon] [ sap-templates-2-tier-user-image] Gebruik deze sjabloon als u wilt maken van een laag 2-systeem met slechts één virtuele machine en uw eigen afbeelding OS.
* [3-laag (meerdere virtuele machines) configuratiesjabloon] [ sap-templates-3-tier-user-image] Gebruik deze sjabloon als u wilt maken van een 3-laag systeem met meerdere virtuele machines en uw eigen afbeelding OS.

Nadat u een van de bovenstaande sjablonen hebt geopend, wordt de Portal Azure navigeert naar het deelvenster Parameters bewerken. Voer de volgende gegevens:

* **sapSystemId**: SAP-systeem-ID
* **waardereeksen**: besturingssysteemtype u implementeren wilt, Windows of Linux
* **sapSystemSize**: de grootte van het SAP-systeem
    * Het bedrag van SAP's u in het nieuwe systeem vindt. Als u niet zeker weet hoeveel SAP's worden het systeem moet, vraag uw SAP technologie Partner of System Integrator
* **systemAvailability**: (alleen 3 laag sjabloon) beschikbaarheid van het systeem 
    * Selecteer HA voor een configuratie die geschikt is voor een HA-installatie. Twee servers webdatabase en twee servers voor de ASC's wordt gemaakt.
* **storageType**: (alleen 2 niveaus sjabloon) Type opslag die moet worden gebruikt 
    * Het gebruik van de Premium-opslag wordt ten zeerste aanbevolen voor groter systemen. Lees voor meer informatie over de van verschillende opslagtypen 
        * [Microsoft Azure opslag] [dbms-handleiding-2.3] van [DBMS handleiding] [dbms-handleiding]
        * [Premium-opslag: High-Performance opslag voor Azure virtuele machines werkbelasting][storage-premium-storage-preview-portal]
        * [Inleiding tot Microsoft Azure-opslag][storage-introduction]
* **adminUsername** en **beheerderswachtwoord**: gebruikersnaam en wachtwoord
    * Een nieuwe gebruiker wordt gemaakt die kan worden gebruikt voor aanmelding bij de computer.
* **userImageVhdUri**: URI van de privé OS afbeelding vhd bijvoorbeeld https://`<accountname`>.blob.core.windows.net/vhds/userimage.vhd
* **userImageStorageAccount**: naam van het opslag-account waarin de privé OS afbeelding bijvoorbeeld is opgeslagen `<accountname`> in het voorbeeld hierboven URI
* **newOrExistingSubnet**: bepaalt of een nieuw virtueel netwerk en subnet moeten worden gemaakt of een bestaand subnet moet worden gebruikt. Als u al een virtueel netwerk die is gekoppeld aan uw on-premises netwerk, selecteert u de bestaande.
* **subnetId**: de ID van het subnet waaraan de virtuele machines moet worden aangesloten. Selecteer het subnet van VPN Express Route virtuele netwerk van uw of de virtuele machine verbinden met uw on-premises netwerk. /Subscriptions/ de ID meestal eruit`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Nadat u alle parameters hebt ingevoerd, selecteert u het abonnement en de resourcegroep die u wilt gebruiken. U kunt een bestaande resourcegroep selecteren of een nieuwe record door te selecteren 'en nieuwe' maken in de vervolgkeuzelijst. Als u een nieuwe resourcegroep maakt, moet u ook selecteren de regio waar de resourcegroep en de virtuele machine worden gemaakt.

Bekijk de juridische voorwaarden, accepteren en klik op maken.

#### <a name="install-vm-agent-linux-only"></a>VM Agent (alleen Linux) installeren
De Linux-Agent moet al zijn geïnstalleerd in de afbeelding van de gebruiker als u wilt de bovenstaande sjablonen gebruiken. Anders de implementatie, mislukt. Download en installeer de VM-Agent in de afbeelding van de gebruiker, zoals beschreven in hoofdstuk [downloaden, installeren en inschakelen van Azure VM Agent] [implementatie-handleiding-4.4] van dit document.
Als u de bovenstaande sjablonen niet gebruikt, kunt u de VM-Agent ook achteraf installeren.

#### <a name="join-domain-windows-only"></a>Deelnemen aan domein (alleen Windows)
In het geval dat de implementatie in Azure is verbonden met het lokale AD/DNS via Azure Site-naar-Site of Express Route (ook waarnaar wordt verwezen als Cross-premises implementatie in de [Planning en implementatie Guide][planning-guide]), wordt naar verwachting dat de VM lid van een on-premises domein worden. Overwegingen van deze stap worden beschreven in hoofdstuk [deelnemen aan VM in on-premises domein (alleen Windows)] [implementatie-handleiding-4,3] van dit document.

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren
Afhankelijk van uw on-premises implementatie-netwerkconfiguratie, is het mogelijk dat deze verplicht voor het configureren van de proxy op uw VM als deze is verbonden met uw on-premises netwerk via een VPN- of Express doorsturen. Anders de virtuele machine mogelijk geen toegang krijgen tot internet en kunnen daarom niet kunt downloaden van de vereiste extensies of controlegegevens verzamelen. Zie hoofdstuk [Proxy configureren] [ deployment-guide-configure-proxy] van dit document.

#### <a name="configure-monitoring"></a>Cmdlets voor controle configureren
Cmdlets voor controle Azure-extensie voor SAP configureren, zoals beschreven in hoofdstuk [configureren Azure Enhanced Monitoring-extensie voor SAP] [implementatie-handleiding-4.5] van dit document.
Controleer de vereisten voor het controleren van SAP voor vereiste minimale versies van SAP Kernel en SAP Host-Agent in de resources in hoofdstuk SAP Resources [implementatie-handleiding-2.2] van dit document.

#### <a name="monitoring-check"></a>Selectievakje bewaken
Controleren of de cmdlets voor controle werkt zoals beschreven in hoofdstuk [Hiermee wordt gecontroleerd en probleemoplossing voor End-to-End controle-instellingen voor SAP op Azure][deployment-guide-troubleshooting-chapter].

### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>Scenario 3: Een VM vanuit on-premises met een niet-generalized Azure-VHD met SAP verplaatsen
Dit scenario is het geval van een SAP-systeem gewoon naar wordt verplaatst in de huidige formulier en de vorm vanuit on-premises Azure adressering. Gemiddelden geen naam wijzigen van de Windows- of Linux hostname en SAP beveiligings-id of een ander nummer NET als dat wordt uitgevoerd. In dit geval de VHD niet als een afbeelding wordt verwezen tijdens de implementatie maar rechtstreeks wordt gebruikt als de schijf OS. Met betrekking tot de implementatie verschilt deze zaak van de twee voormalige gevallen door het feit dat de VM-Agent automatisch kan niet worden geïnstalleerd tijdens de implementatie. Daarom de Azure VM-Agent moet worden gedownload van Microsoft en moet worden geïnstalleerd en ingeschakeld in de VM handmatig achteraf. Nadat deze taak is voltooid, kunt u doorgaan naar de SAP Host Monitoring Azure extensie en de configuratie starten. Controleer in dit artikel voor meer informatie over de functie van de Azure VM-Agent:

[comment]: <> (Onderstaande MSSedusch doen Update Windows-koppeling) 

___

> ![Windows][Logo_Windows] Windows
>
> <http://blogs.msdn.com/b/wats/Archive/2014/02/17/bginfo-Guest-agent-Extension-for-Azure-VMS.aspx>
>
> ![Linux][Logo_Linux] Linux
>
> [De gebruikershandleiding Azure Linux-Agent][virtual-machines-linux-agent-user-guide]

___

De werkstroom van de verschillende stappen ziet er zo:
 
![Stroomdiagram van VM implementatie van SAP-systemen met behulp van een schijf VM][deployment-guide-figure-400]

Aangenomen dat de schijf is al hebt geüpload en gedefinieerd in Azure (Zie [Planning en implementatie Guide][planning-guide]), als volgt te werk

#### <a name="create-virtual-machine"></a>Virtuele machine maken
Maakt u een implementatie een privé OS schijf via de Portal Azure gebruikt met de SAP-sjabloon die zijn gepubliceerd in de [bibliotheek van azure-quickstart-sjablonen github][azure-quickstart-templates-github]. U kunt ook maken een virtuele machine met de [PowerShell of Azure CLI handmatig.

* [2-laag (slechts één virtuele machine)-configuratiesjabloon][sap-templates-2-tier-os-disk]
    * Gebruik deze sjabloon als u wilt maken van een 2-laag systeem met slechts één virtuele machine.

Nadat u de bovenstaande sjabloon hebt geopend, wordt de Portal Azure navigeert naar het deelvenster Parameters bewerken. Voer de volgende gegevens:

* **sapSystemId**: SAP-systeem-ID
* **waardereeksen**: besturingssysteemtype u implementeren wilt, Windows of Linux
* **sapSystemSize**: de grootte van het SAP-systeem
    * Het bedrag van SAP's u in het nieuwe systeem vindt. Als u niet zeker weet hoeveel SAP's worden het systeem moet, vraag uw SAP technologie Partner of System Integrator
* **storageType**: (alleen 2 niveaus sjabloon) Type opslag die moet worden gebruikt 
    * Het gebruik van de Premium-opslag wordt ten zeerste aanbevolen voor groter systemen. Lees voor meer informatie over de van verschillende opslagtypen 
        * [Microsoft Azure opslag] [dbms-handleiding-2.3] van [DBMS handleiding] [dbms-handleiding]
        * [Premium-opslag: High-Performance opslag voor Azure virtuele machines werkbelasting][storage-premium-storage-preview-portal]
        * [Inleiding tot Microsoft Azure-opslag][storage-introduction]
* **osDiskVhdUri**: URI van de privé OS schijf bijvoorbeeld https://`<accountname`>.blob.core.windows.net/vhds/osdisk.vhd
* **newOrExistingSubnet**: bepaalt of een nieuw virtueel netwerk en subnet moeten worden gemaakt of een bestaand subnet moet worden gebruikt. Als u al een virtueel netwerk die is gekoppeld aan uw on-premises netwerk, selecteert u de bestaande.
* **subnetId**: de ID van het subnet waaraan de virtuele machines moet worden aangesloten. Selecteer het subnet van VPN Express Route virtuele netwerk van uw of de virtuele machine verbinden met uw on-premises netwerk. /Subscriptions/ de ID meestal eruit`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

Nadat u alle parameters hebt ingevoerd, selecteert u het abonnement en de resourcegroep die u wilt gebruiken. U kunt een bestaande resourcegroep selecteren of een nieuwe record door te selecteren 'en nieuwe' maken in de vervolgkeuzelijst. Als u een nieuwe resourcegroep maakt, moet u ook selecteren de regio waar de resourcegroep en de virtuele machine worden gemaakt.

Bekijk de juridische voorwaarden, accepteren en klik op maken.

#### <a name="install-vm-agent"></a>VM Agent installeren
De VM-Agent moet al zijn geïnstalleerd op de schijf OS als u wilt de bovenstaande sjablonen gebruiken. Anders de implementatie, mislukt. Download en installeer de VM-Agent in VM zoals beschreven in hoofdstuk [downloaden, installeren en inschakelen van Azure VM Agent] [implementatie-handleiding-4.4] van dit document.

Als u de bovenstaande sjablonen niet gebruikt, kunt u de VM-Agent ook achteraf installeren.

#### <a name="join-domain-windows-only"></a>Deelnemen aan domein (alleen Windows)
In het geval dat de implementatie in Azure is verbonden met het lokale AD/DNS via Azure Site-naar-Site of Express Route (ook waarnaar wordt verwezen als Cross-premises implementatie in de [Planning en implementatie Guide][planning-guide]), wordt naar verwachting dat de VM lid van een on-premises domein worden. Overwegingen van deze stap worden beschreven in hoofdstuk [deelnemen aan VM in on-premises domein (alleen Windows)] [implementatie-handleiding-4,3] van dit document.

#### <a name="configure-proxy-settings"></a>Proxy-instellingen configureren
Afhankelijk van uw on-premises implementatie-netwerkconfiguratie, is het mogelijk dat deze verplicht voor het configureren van de proxy op uw VM als deze is verbonden met uw on-premises netwerk via een VPN- of Express doorsturen. Anders de virtuele machine mogelijk geen toegang krijgen tot internet en kunnen daarom niet kunt downloaden van de vereiste extensies of controlegegevens verzamelen. Zie hoofdstuk [Proxy configureren] [ deployment-guide-configure-proxy] van dit document.

#### <a name="configure-monitoring"></a>Cmdlets voor controle configureren
Azure Enhanced Monitoring-extensie voor SAP configureren, zoals beschreven in hoofdstuk [configureren Azure Enhanced Monitoring-extensie voor SAP] [implementatie-handleiding-4.5] van dit document.

Controleer de vereisten voor het controleren van SAP voor vereiste minimale versies van SAP kernel en SAP Host-Agent in de resources in hoofdstuk SAP Resources [implementatie-handleiding-2.2] van dit document.

#### <a name="monitoring-check"></a>Selectievakje bewaken
Controleren of de cmdlets voor controle werkt zoals beschreven in hoofdstuk [Hiermee wordt gecontroleerd en probleemoplossing voor End-to-End controle-instellingen voor SAP op Azure][deployment-guide-troubleshooting-chapter].

### <a name="scenario-4-updating-the-monitoring-configuration-for-sap"></a>Scenario 4: De controle configuratie voor SAP bijwerken
Er zijn zaken waar moet u de controle configuratie bijwerken:

* Het gemeenschappelijke MS/SAP-team de controlefuncties uitgebreid en willen meer items toevoegen of verwijderen van sommige items. 
* Microsoft maakt u kennis met een nieuwe versie van de onderliggende Azure infrastructuur geven van de controlegegevens en deze wijzigingen van de Azure Enhanced Monitoring-extensie voor SAP aanpassing.
* U aanvullende VHD's die zijn gekoppeld aan uw VM Azure toevoegen of verwijderen van een VHD. In dit geval u wilt bijwerken van de verzameling opslag gerelateerde gegevens worden weergegeven. Als u de configuratie door toevoegen of verwijderen van eindpunten of IP-adressen toewijzen aan een VM wijzigt, wordt dit geen gevolgen voor de controle configuratie.
* U wijzigen de grootte van uw VM Azure bijvoorbeeld uit A5 naar een andere grootte van VM.
* U toevoegen nieuwe netwerkinterfaces aan uw VM Azure

Om bij te werken de controle configuratie, gaat u als volgt:

* De controle-infrastructuur bijwerken door de stappen beschreven in hoofdstuk [configureren Azure Enhanced Monitoring-extensie voor SAP] [implementatie-handleiding-4.5] van dit document. Een opnieuw uitvoeren van het script in dit hoofdstuk beschreven wordt gedetecteerd dat een controle configuratie wordt geïmplementeerd en wordt de noodzakelijke wijzigingen aan de controle configuratie uitvoeren. 

___

> ![Windows][Logo_Windows] Windows
>
> Zonder tussenkomst is voor de update van de Azure VM-Agent is vereist. VM Agent automatisch bijgewerkt en niet vereist is voor een VM opnieuw opstarten.
>
> ![Linux][Logo_Linux] Linux
>
> Volg de stappen in [dit artikel] [ virtual-machines-linux-update-agent] bijwerken van de Azure Linux-Agent. 

___

## <a name="detailed-single-deployment-steps"></a>Gedetailleerde implementatiestappen voor één

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Implementatie van Azure PowerShell-cmdlets
* Ga naar <https://azure.microsoft.com/downloads/>
* Onder de sectie ' opdrachtregel Tools"Er is een sectie 'Windows PowerShell' genoemd. Volg de koppeling 'Installeren'.
* Microsoft Download Manager wordt pop-upvenster met een lijn-item dat eindigt .exe. Selecteer de optie 'Uitvoeren'.
* Een pop-upvenster bovenkomen waarin wordt gevraagd of u Microsoft Web Platform Installer uitvoeren. Druk op Ja
* Een scherm zoals dit wordt weergegeven:
 
![Installatiescherm voor Azure PowerShell-cmdlets][deployment-guide-figure-500]
<a name="figure-5"></a>

* Druk op installeren en accepteer de gebruiksrechtovereenkomst.

Controleer regelmatig of de PowerShell-cmdlets al zijn bijgewerkt. Er zijn meestal updates op een punt maandelijks. Het gemakkelijkst hiervoor de installatiestappen te volgen zoals hierboven is beschreven snel aan de installatiescherm wordt weergegeven in [deze] [ deployment-guide-figure-5] afbeelding. In dit scherm wordt de releasedatum van de cmdlets evenals de werkelijke release getal weergegeven. Tenzij anders in een SAP-notities [1928533] of [2015553]vermeld, wordt het wordt aanbevolen voor gebruik met de nieuwste versie van Azure PowerShell-cmdlets.

De huidige geïnstalleerde versie van de Azure cmdlets op het bureaublad/laptop kan worden gecontroleerd met de opdracht PS:

```powershell
Import-Module Azure
(Get-Module Azure).Version
```

Het resultaat moet worden weergegeven, zoals hieronder wordt weergegeven in [deze] [ deployment-guide-figure-6] afbeelding.

![Resultaat van Azure PS cmdlet versie controleren][deployment-guide-figure-600]
<a name="figure-6"></a>

Als de versie Azure-cmdlet op het bureaublad/laptop de huidige rij is, het eerste scherm nadat het installatieprogramma van Microsoft Web Platform is gestart er enigszins anders vergeleken met zoals wordt weergegeven in [deze] [ deployment-guide-figure-5] afbeelding.

Neem ziet u de rode cirkel onder in de [afbeelding] [ deployment-guide-figure-7] hieronder.
 
![Installatiescherm voor Azure PowerShell-cmdlets die aangeeft dat de meest recente versie van Azure PS cmdlets zijn geïnstalleerd][deployment-guide-figure-700]
<a name="figure-7"></a>

Als het scherm ziet er als [hierboven][deployment-guide-figure-7], die aangeeft dat de meest recente versie van Azure-cmdlet voor het al is geïnstalleerd, hoeft u wilt doorgaan met de installatie. In dit geval kunt u 'verlaten' de installatie in deze fase.

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Azure CLI implementeren
* Ga naar <https://azure.microsoft.com/downloads/>
* Onder de sectie ' opdrachtregel Tools"Er is een sectie 'Een interface van Azure opdrachtregel' genoemd. Volg de installatie-koppeling voor uw besturingssysteem.

Controleer regelmatig of de CLI Azure al zijn bijgewerkt. Er zijn meestal updates op een punt maandelijks. De eenvoudigste manier hiervoor is naar Volg de installatiestappen zoals hierboven is beschreven.

De huidige geïnstalleerde versie van Azure CLI op het bureaublad/laptop kan worden gecontroleerd met de opdracht:

```
azure --version
```

Het resultaat moet worden weergegeven, zoals hieronder wordt weergegeven in [deze] [ deployment-guide-figure-azure-cli-version] afbeelding.

![Resultaat van Azure CLI versie controleren][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>VM deelnemen in de on-premises domein (alleen Windows)
In gevallen waarin u SAP VMs in een scenario voor meerdere lokale implementeren waar lokale AD en DNS wordt verlengd naar Azure, wordt naar verwachting dat de VMs zijn gekoppeld in een on-premises domein. De gedetailleerde stappen voor het deelnemen aan een VM in een on-premises domein en een extra software vereist moeten dat lid zijn van een on-premises domein is afhankelijke klant. Meestal deelnemen aan een VM naar een on-premises domein betekent dat er extra software zoals beveiliging tegen malware software of verschillende agenten van back-up of controle-software installeren.

U moet ook om ervoor te zorgen voor situaties waarin proxy-instellingen voor Internet gedwongen wanneer u deelneemt aan een domein dat de lokale systeem voor Windows Account(S-1-5-18) in Gast VM heeft de volgende instellingen als u ook. Easiest is het afdwingen van de proxy met Groepsbeleid van het domein, die van toepassing op systemen binnen het domein.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Downloaden, installeren en inschakelen van Azure VM Agent
De volgende stappen zijn nodig wanneer een VM voor SAP wordt geïmplementeerd vanuit een OS-afbeeldingen die niet generalized bijvoorbeeld niet syspreped voor Windows. Het is niet nodig is voor het installeren van de Agent voor virtuele machines geïmplementeerd van Azure marketplace. Deze afbeeldingen bevatten al de Azure-Agent.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows

* Download de Agent Azure VM:
    * Download het installatiepakket Azure VM Agent van: <https://go.microsoft.com/fwlink/?LinkId=394789>
    * Het VM Agent MSI-pakket lokaal opslaan op de laptop of een server
* Installeer de Agent Azure VM:
    * Verbinding maken met de uitgevouwen Azure VM met Terminal Services (RDP)
    * Open een Windows Verkenner-venster op de VM en open een doelmap voor het MSI-bestand van de VM-Agent
    * Slepen en neerzetten van de Azure VM Agent Installer MSI-bestand van uw lokale laptop/server in de doelmap van de VM-Agent in VM
    * Dubbelklikt u op het MSI-bestand in de VM
    * Voor VM lid domeinen van de on-premises implementatie, zorg ervoor dat eventuele Internet proxy-instellingen voor het lokale systeem van Windows-account (S-1-5-18) in de VM toepassen, evenals beschreven in hoofdstuk [Proxy configureren][deployment-guide-configure-proxy]. De VM-Agent wordt uitgevoerd in deze context en moet kunnen verbinding maken met Azure.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux
Installeer de VM-Agent voor Linux met de volgende opdracht

- **SLES**

```
sudo zypper install WALinuxAgent
```
- **RHEL**

```
sudo yum install WALinuxAgent
```

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Proxy configureren
De stappen voor het configureren van de proxy verschillen tussen Windows en Linux.

#### <a name="windows"></a>Windows
Deze instellingen moeten ook zijn geldig voor het systeemaccount voor toegang tot Internet. Als uw proxy-instellingen worden niet door Groepsbeleid, kunt u ze configureren voor de lokale systeemaccount als volgt te werk om deze te configureren.

1.  Gpedit.msc openen
1.  Navigeer naar de configuratie van de Computer –> administratieve sjablonen -> onderdelen van Windows Internet Explorer -> en inschakelen Hiermee "maakt proxy instellingen per computer (en niet per gebruiker)
1.  Open het Configuratiescherm en navigeer naar Internetopties-netwerk en Internet >
1.  Open het tabblad verbindingen en klik op LAN-instellingen
1.  'Instellingen automatisch detecteren' uitschakelen
1.  "Een proxyserver gebruiken voor uw LAN" inschakelen en voer de proxyhost en poort

#### <a name="linux"></a>Linux
Configureer de juiste proxy in het configuratiebestand van Microsoft Azure Gast Agent, waar zich bevindt op /etc/waagent.conf. Moet u de volgende parameters instellen:

```
HttpProxy.Host=<proxy host e.g. proxy.corp.local>
HttpProxy.Port=<port of the proxy host e.g. 80>
```

Start de agent opnieuw nadat u de configuratie met hebt gewijzigd

```
sudo service waagent restart
```

De proxy-instellingen in /etc/waagent.conf zijn ook van toepassing voor de vereiste VM extensies. Als u wilt gebruiken van de Azure opslagplaatsen: Zorg ervoor dat het verkeer naar de volgende opslagplaatsen niet worden verzonden via het intranet on-premises implementatie. Als u de gebruiker gedefinieerde waarmee u wordt gedwongen Tunneling inschakelen, moet een route toevoegen gemaakt stuurt die verkeer door naar de opslagplaatsen rechtstreeks met Internet en niet via de verbinding van uw site-naar-site.

- **SLES** U moet ook routes voor de IP-adressen in /etc/regionserverclnt.cfg toevoegen. Een voorbeeld wordt weergegeven in de onderstaande schermafbeelding. 

- **RHEL** U moet ook routes voor het IP-adressen van de hosts vermeld in /etc/yum.repos.d/rhui-load-balancers toevoegen. Een voorbeeld wordt weergegeven in de onderstaande schermafbeelding.

Zie [dit artikel]voor meer informatie over gebruiker gedefinieerd Routes,[virtual-networks-udr-overview].

![U wordt gedwongen Tunneling][deployment-guide-figure-50]

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Azure uitgebreide controle extensie configureren voor SAP
Als de VM is voorbereid zoals beschreven in hoofdstuk [implementatie scenario's van VMs voor SAP op Microsoft Azure] [implementatie-handleiding-3], de Azure VM-Agent is geïnstalleerd in de computer. De volgende belangrijke stap wordt geïmplementeerd van de Azure Enhanced Monitoring-extensie voor SAP, dat beschikbaar in de bibliotheek van de extensie Azure in de globale datacenters van Microsoft Azure is. Voor meer informatie, Controleer de [Planning en implementatiehandleiding] [planning-handleiding-9.1]. 

U kunt Azure PowerShell of Azure CLI gebruiken voor het installeren en configureren van de Azure Enhanced Monitoring-extensie voor SAP. Lees hoofdstuk Azure PowerShell [implementatie-handleiding-4.5.1] als u wilt de extensie installeren op een Windows- of Linux VM met een Windows-computer. Voor het installeren van de extensie op een Linux VM gebruik een Linux Lees bureaublad hoofdstuk Azure CLI [implementatie-handleiding-4.5.2].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>Azure PowerShell voor Linux en Windows VMs
Om te kunnen uitvoeren van de taak voor het installeren van de Azure Enhanced Monitoring-extensie voor SAP, moet u de volgende stappen uitvoeren:

* Zorg ervoor dat u de nieuwste versie van de Microsoft Azure PowerShell-cmdlet hebt geïnstalleerd. Zie hoofdstuk [implementeert Azure PowerShell-cmdlets] [implementatie-handleiding-4.1] van dit document.  
* De volgende PowerShell-cmdlet uitvoeren. Voor een lijst met beschikbare omgevingen, voert u commandlet Get-AzureRmEnvironment. Als u openbare Azure gebruikt wilt, is uw omgeving AzureCloud. Selecteer AzureChinaCloud voor Azure in China.

```powershell
    $env = Get-AzureRmEnvironment -Name <name of the environment>
    Login-AzureRmAccount -Environment $env
    Set-AzureRmContext -SubscriptionName <subscription name>
    
    Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
```

Nadat u de accountgegevens van uw en de Azure virtuele Machine hebt opgegeven, wordt het script implementeren van de vereiste extensies en de vereiste functies inschakelen. Dit kan enkele minuten duren.
Lees [dit artikel MSDN] [ msdn-set-azurermvmaemextension] voor meer informatie over de Set-AzureRmVMAEMExtension.
  
![Resultaat scherm van SAP specifieke Azure-cmdlet Set-AzureRmVMAEMExtension is uitgevoerd][deployment-guide-figure-900]

Een juist is uitgevoerd van Set-AzureRmVMAEMExtension doet alle noodzakelijke stappen voor het configureren van de controle-functionaliteit voor SAP-host. 

De uitvoer die het script afgeleverd ziet er zo:

* Bevestigen dat de controle configuratie voor de Base VHD (met het besturingssysteem) plus alle aanvullende VHD's die zijn gekoppeld aan de VM is geconfigureerd.
* De volgende twee berichten zijn de configuratie van metrische opslaggegevens voor een specifieke opslag-account bevestigen. 
* Eén witregel uitvoer krijgt de status van de werkelijke bijwerken van de controle configuratie.
* Een ander wordt weergegeven de acceptatie van dat de configuratie is geïmplementeerd of bijgewerkt.
* De laatste regel van de uitvoer is informatieve met de mogelijkheid om te testen van de controle configuratie.
* Als u wilt controleren, dat alle stappen van de Azure Enhanced Monitoring is uitgevoerd en dat de infrastructuur Azure beschikt u over de benodigde gegevens, gaat u verder met de gereedheidscontrole voor Azure Enhanced Monitoring-extensie voor SAP, zoals beschreven in hoofdstuk [gereedheidscontrole voor Azure Enhanced Monitoring voor SAP] [implementatie-handleiding-5.1] in dit document. 
* Als u wilt doorgaan hierdoor, wacht u 15-30 minuten totdat de Azure diagnostische hulpprogramma's de relevante gegevens die worden verzameld hebben.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Azure CLI voor Linux VMs

Om te kunnen uitvoeren van de taak voor het installeren van de Azure Enhanced Monitoring-extensie voor SAP Azure CLI gebruiken, moet u de volgende stappen uitvoeren:

1. Azure CLI installeren, zoals wordt beschreven in [Dit] [ azure-cli] artikel
1. Meld u aan met uw Azure-account

    ```
    azure login
    ```
1. Resourcemanager Azure-modus

    ```
    azure config mode arm
    ```
1. Azure uitgebreide controle inschakelen

    ```
    azure vm enable-aem <resource-group-name> <vm-name>
    ```  
1. Controleer of de Azure Enhanced cmdlets voor controle op de Azure Linux VM actief. Controleer of het bestand /var/lib/AzureEnhancedMonitor/PerfCounters aanwezig. Als bestaat, weergavegegevens die worden verzameld door AEM met:

    ```
    cat /var/lib/AzureEnhancedMonitor/PerfCounters
    ```
    Vervolgens krijgt u uitvoer zoals:
    
    ```
    2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
    2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
    …
    …
    ```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Controles en probleemoplossing voor End-to-End cmdlets voor controle-instellingen voor SAP op Azure
Nadat u hebt uw VM Azure geïmplementeerd en de relevante Azure controleren infrastructuur instellen, moet u controleren of alle onderdelen van de Azure Enhanced cmdlets voor controle in een goede manier werkt. 

Daarom de gereedheidscontrole voor de Azure Enhanced Monitoring-extensie voor SAP uitvoeren, zoals beschreven in hoofdstuk [gereedheidscontrole voor Azure Enhanced Monitoring voor SAP] [implementatie-handleiding-5.1]. Als het resultaat van deze controle positief is en ziet u alle relevante prestatie-items, is het Azure monitoring ingesteld met succes. In dit geval kunt u doorgaan met de installatie van SAP Host Agent zoals is beschreven in de SAP-notities in hoofdstuk SAP Resources [implementatie-handleiding-2.2] van dit document weergegeven. Als het resultaat van de gereedheidscontrole ontbrekende items aangeeft, gaat u verder de statuscontrole voor de infrastructuur voor het controleren van Azure uitvoeren, zoals beschreven in hoofdstuk [Statuscontrole voor Azure Monitoring infrastructuur configuratie] [implementatie-handleiding-5,2]. Controleer in geval van problemen met de configuratie van Azure Monitoring, hoofdstuk [verder probleemoplossing van Azure Monitoring infrastructuur voor SAP] [implementatie-handleiding-5.3] voor verdere hulp bij probleemoplossing.

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Gereedheidscontrole voor Azure uitgebreide controle voor SAP
Met deze controle ervoor u zorgen dat de cijfers die worden weergegeven in uw SAP-toepassing volledig worden verstrekt door de onderliggende Monitoring-infrastructuur van Azure. 

#### <a name="execute-the-readiness-check-on-a-windows-vm"></a>De gereedheidscontrole op een Windows-VM uitvoeren
Om te kunnen uitvoeren van de gereedheidscontrole, aanmelding naar de (beheerdersaccount is niet nodig) Azure virtuele Machine en de volgende stappen uitvoeren:

* Open een opdrachtprompt van Windows en wijzigen in de map voor de installatie van de Azure Monitoring-extensie voor SAP-C:\Packages\Plugins\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\`<version`> \drop

Het deel achter de versie die beschikbaar zijn in het pad naar de bovenstaande controleren extensie kan variëren. Als u meerdere mappen van de controle extensie-versie in de installatiemap ziet, Controleer de configuratie van de Windows-service 'AzureEnhancedMonitoring' en Ga naar de map aangegeven als 'Pad naar uitvoerbaar'.
 
![Eigenschappen van de Azure Enhanced Monitoring-extensie voor SAP-Service][deployment-guide-figure-1000]

* Azperflib.exe in het opdrachtvenster zonder parameters worden uitgevoerd.

> [AZURE.NOTE] De azperflib.exe wordt uitgevoerd in een lus en updates van de verzamelde items om de 60 seconden. Om te voltooien missen, sluit u het opdrachtvenster.

Als de extensie cmdlets voor controle van Azure Enhanced niet is geïnstalleerd of de service 'AzureEnhancedMonitoring' niet wordt uitgevoerd, heeft de extensie niet correct zijn geconfigureerd. In dit geval Volg hoofdstuk [verder probleemoplossing van Azure Monitoring infrastructuur voor SAP] [implementatie-handleiding-5.3] voor gedetailleerde instructies hoe u het implementeren van de extensie.

##### <a name="check-the-output-of-azperflibexe"></a>De uitvoer van azperflib.exe controleren
De uitvoer van azperflib.exe ziet u dat alle prestatiemeteritems van Azure voor SAP zijn ingevuld. Onderaan in de lijst met verzamelde items, door een overzicht en een statusindicator, wat de status van de Azure cmdlets voor controle betekent op te zoeken. 
 
![Uitvoer van statuscontrole door te voeren azperflib.exe dat aangeeft dat er geen problemen aanwezig][deployment-guide-figure-1100]
<a name="figure-11"></a>

Controleer het resultaat voor de uitvoer van de hoeveelheid 'Items totaal', die worden gerapporteerd als leeg en voor de statuscontrole zoals hierboven in de afbeelding [boven][deployment-guide-figure-11].

U kunt als volgt de resultaatwaarden interpreteren:

| Azperflib.exe resultaatwaarden | Azure gereedheid Status controleren |
| ------------------------------|----------------------------------- |
| **Items in totaal: lege** | De volgende 2 Azure opslag items kunnen leeg zijn: <ul><li>Opslag lezen Op latentie Server msec</li><li>Opslag lezen Op latentie E2E msec</li></ul>Alle andere items bevatten waarden. |
| **Statuscontrole** | Alleen ziet OK als afzender status u OK |

Als u niet beide teruggaan waarden van azperflib.exe weergeven dat alle ingevulde items correct worden geretourneerd, volgt u de instructies van de statuscontrole voor de configuratie van Azure Monitoring infrastructuur zoals beschreven in hoofdstuk [Statuscontrole voor Azure Monitoring infrastructuur configuratie] [implementatie-handleiding-5,2] onder.

#### <a name="execute-the-readiness-check-on-a-linux-vm"></a>De gereedheidscontrole op een VM Linux uitvoeren
Als u wilt de Gereedheidscontrole uitvoeren, verbinding maken met SSH naar de Azure virtuele Machine en de volgende stappen uitvoeren:

* De uitvoer van de extensie cmdlets voor controle van Azure Enhanced controleren
    * meer /var/lib/AzureEnhancedMonitor/PerfCounters
        * De database geeft u een lijst met items. Het bestand mogen niet leeg zijn
    * CAT /var/lib/AzureEnhancedMonitor/PerfCounters | GREP fout
        * Eén regel waar de fout geen bijvoorbeeld 3 is; config; moeten worden geretourneerd Fout; 0; 0; **geen**; 0; 1456416792; tst-servercs;
    * meer /var/lib/AzureEnhancedMonitor/LatestErrorRecord
        * Moet leeg zijn of niet aanwezig
* Als de bovenstaande eerste controleren mislukt is, kunt u deze extra onderzoeken:
    * Zorg ervoor dat de waagent is geïnstalleerd en gestart
        * sudo ls-al/var/lib/waagent /
            * de inhoud van de map waagent moet worden weergeven
        * PS-ax | GREP waagent
            * een vermelding strekking moet weergeven ' python /usr/sbin/waagent-daemon'
    * Zorg ervoor dat de extensie Linux Diagnostic is geïnstalleerd en gestart
        * sudo v - c ' ls-al /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-*'
            * de inhoud van de map Linux diagnostische extensie moet worden weergeven
        * PS-ax | GREP diagnostische gegevens
            * een vermelding strekking moet weergeven ' python /var/lib/waagent/Microsoft.OSTCExtensions.LinuxDiagnostic-2.0.92/diagnostic.py-daemon'
    * Zorg ervoor dat de extensie cmdlets voor controle van Azure Enhanced is geïnstalleerd en gestart
        * sudo v - c ' ls-al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/'
            * de inhoud van de map met de extensie cmdlets voor controle van Azure Enhanced moet lijst
        * PS-ax | GREP AzureEnhanced
            * een vermelding moet lijkt op 'python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon' weergeven
* De SAP-Agent Host installeert, zoals wordt beschreven in de notitie SAP [1031096] en controleert u de uitvoer van saposcol
    * /Usr/sap/hostctrl/exe/saposcol -d uitvoeren
    * Dump ccm uitvoeren
    * Controleren of de meetwaarde 'Virtualization_Configuration\Enhanced Monitoring Access' waar is
* Als u al een SAP NetWeaver ABAP toepassingsserver is geïnstalleerd, opent u transactie ST06 en selectievakje als de uitgebreide controle is ingeschakeld.

Als een van de controles hierboven fail, volgt u hoofdstuk [verder probleemoplossing van Azure Monitoring infrastructuur voor SAP] [implementatie-handleiding-5.3] voor gedetailleerde instructies hoe u het implementeren van de extensie.

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Statuscontrole voor Azure Monitoring infrastructuur configuratie
Als een deel van de controle correct zoals aangegeven met de beschrijvingen in hoofdstuk [gereedheidscontrole voor Azure Enhanced Monitoring voor SAP] test gegevens niet is afgeleverd [implementatie-handleiding-5.1] bovenaan uitvoeren de cmdlet Test-AzureRmVMAEMExtension om te testen of de huidige configuratie van de infrastructuur Azure cmdlets voor controle en de controle-extensie voor SAP correct is.

Om te controleren configuratie testen, voert u de volgende handelingen uit:

* Zorg ervoor dat u de nieuwste versie van de Microsoft Azure PowerShell-cmdlet hebt geïnstalleerd zoals beschreven in hoofdstuk [implementeert Azure PowerShell-cmdlets] [implementatie-handleiding-4.1] van dit document.
* De volgende PowerShell-cmdlet uitvoeren. Voor een lijst met beschikbare omgevingen, voert u commandlet Get-AzureRmEnvironment. Als u openbare Azure gebruikt wilt, is uw omgeving AzureCloud. Selecteer AzureChinaCloud voor Azure in China.

```powershell
$env = Get-AzureRmEnvironment -Name <name of the environment>
Login-AzureRmAccount -Environment $env
Set-AzureRmContext -SubscriptionName <subscription name>
Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
```

* Nadat u de accountgegevens van uw en de Azure virtuele Machine hebt opgegeven, wordt het script de configuratie van de virtuele machine die u kiest testen.

 
![Invoer scherm van SAP specifieke Azure-cmdlet Test-VMConfigForSAP_GUI][deployment-guide-figure-1200]

Nadat u de gegevens over uw account en de Azure virtuele Machine hebt ingevoerd, wordt het script de configuratie van de virtuele machine die u kiest testen.
 
![Uitvoer van geslaagde test van Azure Monitoring infrastructuur voor SAP][deployment-guide-figure-1300]

Zorg ervoor dat elk selectievakje is gemarkeerd met OK. Als aantal controles niet ok, voer de cmdlet update zoals beschreven in hoofdstuk [configureren Azure Enhanced Monitoring-extensie voor SAP] [implementatie-handleiding-4.5] van dit document. Wacht een ander 15 minuten en uitvoeren van de controles die worden beschreven in hoofdstuk [gereedheidscontrole voor Azure Enhanced Monitoring voor SAP] [implementatie-handleiding-5.1] en [Statuscontrole voor Azure Monitoring infrastructuur configuratie] [implementatie-handleiding-5,2] opnieuw. Als de controles nog steeds een probleem met sommige of alle items aanduiden, gaat u naar hoofdstuk [verder probleemoplossing van Azure Monitoring infrastructuur voor SAP] [implementatie-handleiding-5.3].

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Verder probleemoplossing van Azure Monitoring infrastructuur voor SAP

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] Azure prestatiemeteritems worden niet weergegeven op alle
Het vastleggen van de prestatiegegevens op Azure wordt uitgevoerd door de Windows-service 'AzureEnhancedMonitoring'. Als de service is niet goed geïnstalleerd of als deze niet op uw VM wordt uitgevoerd, kunt u helemaal geen prestatiegegevens verzameld.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>De installatiemap van de extensie Azure Enhanced Monitoring is leeg 

###### <a name="issue"></a>Probleem
De installatiemap C:\Packages\Plugins\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\`<version`> \drop leeg is.

###### <a name="solution"></a>Oplossing
De extensie is niet geïnstalleerd. Controleer of als dit een probleem met de proxy is (zoals beschreven voordat). Mogelijk moet u de computer opnieuw opstarten en/of het script in de configuratie-script Set-AzureRmVMAEMExtension opnieuw uitvoeren

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Service voor het controleren van Azure Enhanced bestaat niet 

###### <a name="issue"></a>Probleem
Windows-service 'AzureEnhancedMonitoring' bestaat niet. Azperflib.exe: De uitvoer azperlib.exe genereert een fout zoals wordt weergegeven in de [onderstaande afbeelding][deployment-guide-figure-14].
 
![Uitvoering van azperflib.exe wordt aangegeven dat de service van de extensie cmdlets voor controle van Azure Enhanced voor SAP niet wordt uitgevoerd][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Oplossing
Als de service niet bestaat zoals weergegeven in de [bovenstaande afbeelding][deployment-guide-figure-14], de Azure Monitoring-extensie voor SAP niet juist is geïnstalleerd. Implementeer deze opnieuw de extensie volgens de stappen beschreven voor uw implementatiescenario in hoofdstuk [implementatie scenario's van VMs voor SAP op Microsoft Azure] [implementatie-handleiding-3]. 

Opnieuw controleren of de Azure items binnen de VM Azure worden verstrekt na 1 uur na de installatie van de extensie.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-to-start"></a>Service voor het controleren van Azure Enhanced bestaat, maar kan niet worden gestart 

###### <a name="issue"></a>Probleem
Windows-service 'AzureEnhancedMonitoring' bestaat en is ingeschakeld, maar kan niet worden gestart. Controleer het gebeurtenislogboek van de toepassing voor meer informatie.

###### <a name="solution"></a>Oplossing
Ongeldige configuratie. De controle extensie opnieuw inschakelen voor VM zoals beschreven in hoofdstuk [configureren Azure Enhanced Monitoring-extensie voor SAP] [implementatie-handleiding-4.5].

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] Sommige prestatiemeteritems Azure ontbreken
Het vastleggen van de prestatiegegevens op Azure wordt uitgevoerd door de Windows-service 'AzureEnhancedMonitoring', waarin gegevens uit verschillende bronnen krijgt. Sommige configuratiegegevens lokaal zijn verzameld, prestatiegegevens van Azure diagnostische gegevens worden gelezen en opslag items worden gebruikt in uw logboekregistratie op opslagniveau van abonnement.

Als u problemen oplossen met SAP-notitie hebt [1999351] niet helpen voorkomen, voer de configuratiescript Set-AzureRmVMAEMExtension opnieuw. Mogelijk moet u een uur wachten omdat opslag analytics of diagnostische gegevens items kunnen niet worden gemaakt onmiddellijk nadat deze zijn ingeschakeld. Als het probleem blijft optreden, opent u een SAP-klant ondersteuning weergegeven op de component BC-OP-NT-AZR.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Azure prestatiemeteritems worden niet weergegeven op alle

Het vastleggen van de prestatiegegevens op Azure wordt uitgevoerd door een deamon. Als de deamon niet actief is, kunnen geen prestatiegegevens helemaal worden verzameld.

##### <a name="the-installation-directory-of-the-azure-enhanced-monitoring-extension-is-empty"></a>De installatiemap van de extensie Azure Enhanced Monitoring is leeg 

###### <a name="issue"></a>Probleem
De map/var/bibliotheek/waagent/bevat geen een submap voor de extensie Azure Enhanced bewaken.

###### <a name="solution"></a>Oplossing
De extensie is niet geïnstalleerd. Controleer of als dit een probleem met de proxy is (zoals beschreven voordat). Mogelijk moet u de computer opnieuw opstarten en/of de configuratiescript Set-AzureRmVMAEMExtension opnieuw uitvoeren

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Sommige prestatiemeteritems Azure ontbreken

Het vastleggen van de prestatiegegevens op Azure wordt uitgevoerd door een daemon, welke gegevens worden opgehaald uit verschillende bronnen. Sommige configuratiegegevens lokaal zijn verzameld, prestatiegegevens van Azure diagnostische gegevens worden gelezen en opslag items worden gebruikt in uw logboekregistratie op opslagniveau van abonnement.

Voor een complete en up-to-date Zie lijst met bekende problemen SAP notitie [1999351] met aanvullende informatie over probleemoplossing voor de Enhanced Azure controle voor SAP.

Als u problemen oplossen met SAP-notitie [1999351] niet heeft geholpen, opnieuw Voer de configuratiescript Set-AzureRmVMAEMExtension zoals beschreven in hoofdstuk [configureren Azure Enhanced Monitoring-extensie voor SAP] [implementatie-handleiding-4.5]. Mogelijk moet u een uur wachten omdat opslag analytics of diagnostische gegevens items kunnen niet worden gemaakt onmiddellijk nadat deze zijn ingeschakeld. Als het probleem blijft optreden, opent u een SAP-klant ondersteuning bericht op de component BC-OP-NT-AZR voor Windows of BC-OP-LNX-AZR voor een Linux virtuele machine.
