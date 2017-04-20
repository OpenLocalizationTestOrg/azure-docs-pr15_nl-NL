<properties
   pageTitle="SAP NetWeaver op Windows virtuele machines (VMs) - werken met een hoge beschikbaarheid | Microsoft Azure"
   description="Hoge beschikbaarheid handleiding voor het SAP NetWeaver op Windows virtuele machines"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="goraco"
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
   ms.author="goraco"/>

# <a name="sap-netweaver-on-windows-virtual-machines-vms---high-availability-guide"></a>SAP NetWeaver op Windows virtuele machines (VMs) - werken met een hoge beschikbaarheid

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

[sap-installation-guides]:http://service.sap.com/instguides

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

[sap-ha-guide]:virtual-machines-windows-sap-high-availability-guide.md (High-availability SAP NetWeaver on Windows virtual machines)
[sap-ha-guide-1]:virtual-machines-windows-sap-high-availability-guide.md#217c5479-5595-4cd8-870d-15ab00d4f84c (Prerequisites)
[sap-ha-guide-2]:virtual-machines-windows-sap-high-availability-guide.md#42b8f600-7ba3-4606-b8a5-53c4f026da08 (Resources)
[sap-ha-guide-3]:virtual-machines-windows-sap-high-availability-guide.md#42156640c6-01cf-45a9-b225-4baa678b24f1 (High-availability SAP with Azure Resource Manager vs. the classic deployment model)
[sap-ha-guide-3.1]:virtual-machines-windows-sap-high-availability-guide.md#f76af273-1993-4d83-b12d-65deeae23686 (Resource groups)
[sap-ha-guide-3.2]:virtual-machines-windows-sap-high-availability-guide.md#3e85fbe0-84b1-4892-87af-d9b65ff91860 (Clustering with Azure Resource Manager vs. the classic deployment model)
[sap-ha-guide-4]:virtual-machines-windows-sap-high-availability-guide.md#8ecf3ba0-67c0-4495-9c14-feec1a2255b7 (Windows Server Failover Clustering)
[sap-ha-guide-4.1]:virtual-machines-windows-sap-high-availability-guide.md#1a3c5408-b168-46d6-99f5-4219ad1b1ff2 (Quorum modes)
[sap-ha-guide-5]:virtual-machines-windows-sap-high-availability-guide.md#fdfee875-6e66-483a-a343-14bbaee33275 (Windows Server Failover Clustering on-premises)
[sap-ha-guide-5.1]:virtual-machines-windows-sap-high-availability-guide.md#be21cf3e-fb01-402b-9955-54fbecf66592 (Shared storage)
[sap-ha-guide-5.2]:virtual-machines-windows-sap-high-availability-guide.md#ff7a9a06-2bc5-4b20-860a-46cdb44669cd (Networking and name resolution)
[sap-ha-guide-6]:virtual-machines-windows-sap-high-availability-guide.md#2ddba413-a7f5-4e4e-9a51-87908879c10a (Windows Server Failover Clustering in Azure)
[sap-ha-guide-6.1]:virtual-machines-windows-sap-high-availability-guide.md#1a464091-922b-48d7-9d08-7cecf757f341 (Shared disk in Azure with SIOS DataKeeper)
[sap-ha-guide-6.2]:virtual-machines-windows-sap-high-availability-guide.md#44641e18-a94e-431f-95ff-303ab65e0bcb (Name resolution in Azure)
[sap-ha-guide-7]:virtual-machines-windows-sap-high-availability-guide.md#2e3fec50-241e-441b-8708-0b1864f66dfa (SAP NetWeaver beschikbaarheid in Azure-infrastructuur-als-een-service (IaaS)) [sap-ha-guide-7.1]:virtual-machines-windows-sap-high-availability-guide.md#93faa747-907e-440a-b00a-1ae0a89b1c0e (hoge beschikbaarheid SAP-toepassingsservers) [sap-ha-guide-7.2]:virtual-machines-windows-sap-high-availability-guide.md#f559c285-ee68-4eec-add1-f60fe7b978db (hoge beschikbaarheid SAP ASC's / SCS exemplaar) [sap-ha-guide-7.2.1]:virtual-machines-windows-sap-high-availability-guide.md#b5b1fd0b-1db4-4d49-9162-de07a0132a51 (hoge beschikbaarheid SAP ASC's / SCS exemplaar met Windows Server Failoverclustering in Azure wordt aangegeven) [sap-ha-guide-7.3]:virtual-machines-windows-sap-high-availability-guide.md#ddd878a0-9c2f-4b8e-8968-26ce60be1027 ( High-Availability DBMS exemplaar) [sap-ha-guide-7.4]:virtual-machines-windows-sap-high-availability-guide.md#045252ed-0277-4fc8-8f46-c5a29694a816 (End-to-end hoge beschikbaarheid implementatie-scenario's) [sap-ha-guide-8]:virtual-machines-windows-sap-high-availability-guide.md#78092dbe-165b-454c-92f5-4972bdbef9bf (voorbereiden de infrastructuur) [sap-ha-guide-8.1]:virtual-machines-windows-sap-high-availability-guide.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489 (Deploy virtuele machines met bedrijfsnetwerk connectivity (cross-premises) gebruik in productie) [sap-ha-guide-8.2]:virtual-machines-windows-sap-high-availability-guide.md#7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310 (alleen de Cloud implementatie van SAP-exemplaren voor test en demo) [sap-ha-guide-8.3]:virtual-machines-windows-sap-high-availability-guide.md#47d5300a-a830-41d4-83dd-1a0d1ffdbe6a ( Azure virtuele netwerk) [sap-ha-guide-8.4]:virtual-machines-windows-sap-high-availability-guide.md#b22d7b3b-4343-40ff-a319-097e13f62f9e (DNS IP-adressen) [sap-ha-guide-8.5]:virtual-machines-windows-sap-high-availability-guide.md#9fbd43c0-5850-4965-9726-2a921d85d73f (Host names en statische IP-adressen voor de SAP ASC's / SCS gegroepeerd exemplaar en gegroepeerd exemplaar DBMS) [sap-ha-guide-8.6]:virtual-machines-windows-sap-high-availability-guide.md#84c019fe-8c58-4dac-9e54-173efd4b2c30 (Set statische IP-adressen voor de SAP virtuele machines) [sap-ha-guide-8.7]:virtual-machines-windows-sap-high-availability-guide.md#7a8f3e9b-0624-4051-9e41-b73fff816a9e (maken een statische IP-adres van de interne taakverdeling) [sap-ha-guide-8.8]:virtual-machines-windows-sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c (standaard ASC's / SCS laden het verdelen van regels voor de verdeling van de Azure interne laden) [sap-ha-guide-8.9]:virtual-machines-windows-sap-high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716 (veranderen de ASC's / SCS standaard-taakverdeling regels voor de verdeling van de Azure interne laden) [sap-ha-guide-8.10]:virtual-machines-windows-sap-high-availability-guide.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c (virtuele machines Windows toevoegen op het domein) [sap-ha-guide-8.11]:virtual-machines-windows-sap-high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158 (registervermeldingen op beide knooppunten van het SAP ASC's / SCS exemplaar toevoegen) [sap-ha-guide-8.12]:virtual-machines-windows-sap-high-availability-guide.md#0d67f090-7928-43e0-8772-5ccbf8f59aab (instellen op een Windows Server-Failoverclustering cluster voor een SAP ASC's / SCS exemplaar) [sap-ha-handleiding-8.12.1] : virtual-machines-windows-sap-high-availability-guide.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79 (verzamelen knooppunten in een clusterconfiguratie) [sap-ha-guide-8.12.2]:virtual-machines-windows-sap-high-availability-guide.md#e49a4529-50c9-4dcf-bde7-15a0c21d21ca (configureren van een cluster bestandssharewitness) [sap-ha-guide-8.12.2.1]:virtual-machines-windows-sap-high-availability-guide.md#06260b30-d697-4c4d-b1c9-d22c0bd64855 (maken een bestandsshare) [sap-ha-guide-8.12.2.2]:virtual-machines-windows-sap-high-availability-guide.md#4c08c387-78a0-46b1-9d27-b497b08cac3d (het bestand delen getuige quorum in Failoverclusterbeheer ingesteld) [sap-ha-guide-8.12.3]:virtual-machines-windows-sap-high-availability-guide.md#5c8e5482-841e-45e1-a89d-a05c0907c868 (installeren SIOS DataKeeper Cluster versie voor de SAP ASC's / SCS cluster delen schijf) [sap-ha-handleiding-8.12.3.1] : virtual-machines-windows-sap-high-availability-guide.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3 (Add .NET Framework 3.5) [sap-ha-guide-8.12.3.2]:virtual-machines-windows-sap-high-availability-guide.md#dd41d5a2-8083-415b-9878-839652812102 (installeren SIOS DataKeeper) [sap-ha-guide-8.12.3.3]:virtual-machines-windows-sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 (instellen SIOS DataKeeper) [sap-ha-guide-9]:virtual-machines-windows-sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b (installatie het systeem SAP NetWeaver) [sap-ha-guide-9.1]:virtual-machines-windows-sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 (SAP installeren met een hoge beschikbaarheid ASC's / SCS exemplaar) [sap-ha-guide-9.1.1]:virtual-machines-windows-sap-high-availability-guide.md# a97ad604-9094-44fe-a364-f89cb39bf097 (maken de naam van een virtuele host voor de gegroepeerde SAP ASC's / SCS exemplaar) [sap-ha-guide-9.1.2]:virtual-machines-windows-sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 (het eerste knooppunt van SAP installatie) [sap-ha-guide-9.1.3]:virtual-machines-windows-sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 (wijzigen het SAP-profiel van het exemplaar ASC's / SCS) [sap-ha-guide-9.1.4]:virtual-machines-windows-sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 (test poort toevoegen) [sap-ha-guide-9.2]:virtual-machines-windows-sap-high-availability-guide.md#85d78414-b21d-4097-92b6-34d8bcb724b7 (installatie exemplaar van de database) [sap-ha-guide-9.3]:virtual-machines-windows-sap-high-availability-guide.md#8a276e16-f507-4071-b829-cdc0a4d36748 (het tweede clusterknooppunt installatie) [sap-ha-guide-9.4]:virtual-machines-windows-sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a (het type begin van de service-exemplaar van SAP uit Windows wijzigen) [sap-ha-guide-9.5]:virtual-machines-windows-sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 (installatie de primaire toepassingsserver van SAP) [sap-ha-guide-9.6]:virtual-machines-windows-sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 (installatie de SAP extra toepassingsserver) [sap-ha-guide-10]:virtual-machines-windows-sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9 (testen of de SAP ASC's / SCS exemplaar failover en SIOS herhaling) [sap-ha-guide-10.1]:virtual-machines-windows-sap-high-availability-guide.md#65fdef0f-9f94-41f9-b314-ea45bbfea445 (SAP ASC's / SCS exemplaar wordt uitgevoerd op clusterknooppunt A) [ SAP-ha-Guide-10.2]:Virtual-machines-Windows-SAP-High-Availability-Guide.MD#5e959fa9-8fcd-49e5-a12c-37f6ba07b916 (Failover van knooppunt A naar knooppunt B) [sap-ha-guide-10.3]:virtual-machines-windows-sap-high-availability-guide.md#755a6b93-0099-4533-9f6d-5c9a613878b5 (SAP ASC's / SCS exemplaar wordt uitgevoerd op clusterknooppunt B))


[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png


[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../resource-group-overview.md
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
[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../resource-manager-deployment-model.md#benefits-of-using-resource-manager-and-resource-groups
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
[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:virtual-machines-windows-portal-sql-alwayson-int-listener.md
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


Azure virtuele Machines is de oplossing voor organisaties die de berekeningscluster, opslag- en mogelijkheden, nodig in minimale tijd en zonder lange aanschaf maal. U kunt Azure virtuele Machines klassieke toepassingen zoals SAP NetWeaver gebaseerde ABAP, Java en een stapel ABAP + Java implementeren. Betrouwbaarheid en beschikbaarheid zonder extra on-premises implementatie resources uitbreiden. Omdat Azure virtuele Machines meerdere lokale connectivity ondersteunt, kunt uw organisatie integreren met Azure virtuele Machines uw on-premises implementatie domeinen, persoonlijke wolken en SAP-systeem Liggend.

In dit artikel behandeld de stappen die u ondernemen kunt om het implementeren van hoge beschikbaarheid SAP-systemen in Azure wordt aangegeven, met behulp van het nieuwe implementatiemodel van de resourcemanager Azure. We begeleiden u tot en met de volgende belangrijke taken kunnen uitvoeren:

- De juiste SAP-installatie hulplijnen en notities, weergegeven in de [Resources] vinden[ sap-ha-guide-2] sectie. In dit artikel is een aanvulling op de documentatie van SAP-installatie en SAP-opmerkingen zijn de primaire resources waarmee u kunt installeren en implementeren van SAP-software op bepaalde platformen.

- Ontdek de verschillen tussen het model Azure klassieke-implementatie en het implementatiemodel resourcemanager Azure.

- Meer informatie over Windows Server-Failoverclustering quorum modi, zodat u het model die geschikt is voor uw Azure-implementatie kunt selecteren.

- Meer informatie over Windows Server Failoverclustering gedeeld opslagruimte in Azure services.

- Meer informatie over het beveiligen van één-punt-van-fout onderdelen, zoals ABAP SAP centraal Services (ASC's) / SAP centraal Services (SCS) en systemen (DBMS) en redundante onderdelen zoals SAP-toepassing-servers in Azure wordt aangegeven.

- Volg een stapsgewijs voorbeeld van een installatie en configuratie van een SAP-systeem van hoge beschikbaarheid in een cluster Failoverclustering van Windows Server met behulp van Azure als een platform, met Azure Resource Manager.

- Meer informatie over het gebruik van Windows Server Failoverclustering in Azure, maar die niet in een on-premises implementatie nodig zijn vereist.

Als u wilt vereenvoudigen installatie en configuratie, in dit artikel, gebruiken we de nieuwe SAP drie lagen hoge beschikbaarheid resourcemanager sjablonen. De sjablonen automatiseren implementatie van de hele infrastructuur die u nodig voor een SAP-systeem van hoge beschikbaarheid hebt. De infrastructuur ondersteunt ook SAP's formaatgrepen van het SAP-mailsysteem.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Vereisten voor

Controleer voordat u begint, of dat u voldoet aan de vereisten die in de volgende secties worden beschreven. Ook, moet u alle resources in de [Resources] controleren[ sap-ha-guide-2] sectie.

In dit artikel gebruiken we Azure resourcemanager sjablonen voor [Drie lagen SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/). Zie voor een handig overzicht van sjablonen, [SAP Azure resourcemanager sjablonen](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).

## <a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Resources

In deze handleidingen begeleidende ook SAP-implementaties in Azure wordt aangegeven:

- [SAP NetWeaver op Windows virtuele machines (VMs) – Planning en implementatiehandleiding] [-Planningshandleiding]
- [SAP NetWeaver op Windows virtuele machines (VMs) – Implementatiehandleiding] [Implementatiehandleiding]
- [SAP NetWeaver op Windows virtuele machines (VMs) – DBMS Implementatiehandleiding] [dbms-handleiding]
- [SAP NetWeaver op Windows virtuele machines (VMs) – hoge beschikbaarheid handleiding [deze handleiding]][sap-ha-guide]

> [AZURE.NOTE] Indien mogelijk, geven wij u een koppeling tot de waarnaar wordt verwezen SAP-installatiehandleiding (Zie [SAP-installatiehandleidingen][sap-installation-guides]). Voor de vereisten en informatie over het installatieproces is dit een goed idee om de installatie-gidsen van SAP NetWeaver zorgvuldig lezen. In dit artikel behandelt alleen specifieke taken voor NetWeaver SAP-systemen die u met Azure virtuele Machines kunt gebruiken.

Deze notities SAP hebben betrekking op het onderwerp van SAP in Azure wordt aangegeven:

| Nootnummer   | Titel                                                    
| ------------- |----------------------------------------------------------
| [1928533]       | SAP-toepassingen op Azure: ondersteunde producten en formaatgrepen
| [2015553]       | SAP op Microsoft Azure: vereisten voor ondersteuning         
| [1999351]       | Enhanced Azure Monitoring voor SAP                        
| [2178632]       | Sleutel aan de doelstellingen voor SAP op Microsoft Azure bewaken        
| [1999351]       | Virtualization in Windows: Enhanced bewaken      


Meer informatie over de [beperkingen van Azure abonnementen][azure-subscription-service-limits-subscription], inclusief algemene standaardbeperkingen en beperkingen voor maximale.

## <a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Hoge beschikbaarheid SAP met Azure resourcemanager versus het klassieke implementatiemodel

De resourcemanager Azure en klassieke implementatiemodellen zijn twee manieren:

- Resourcegroepen
- Cluster vereisten

### <a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Resourcegroepen
In Azure resourcemanager, kunt u resourcegroepen voor het beheren van alle resources in de toepassing van uw Azure-abonnement. In een geïntegreerde aanpak hebben alle resources in een resourcegroep, de levenscyclus van de dezelfde. Alle resources zijn bijvoorbeeld gemaakt op hetzelfde moment en op hetzelfde moment verwijderd. U kunt meer informatie over [resourcegroepen](azure-resource-manager/resource-group-overview.md#resource-groups)krijgen.

### <a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Cluster met Azure resourcemanager versus het klassieke implementatiemodel

In het model Azure resourcemanager hoeft u niet een cloudservice Azure interne taakverdeling beschikbaarheid gebruiken.

Als u wilt gebruiken het Azure klassieke model, volgt u de procedures in [SAP NetWeaver in Azure wordt aangegeven: cluster SAP ASC's / SCS exemplaren met behulp van Windows Server-Failoverclustering in Azure wordt aangegeven met SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).

> [AZURE.NOTE] Het is raadzaam dat u het implementatiemodel van Azure Resource Manager voor uw SAP-installaties gebruiken. Deze optie biedt vele voordelen die niet beschikbaar in het implementatiemodel klassieke zijn. U kunt meer informatie over Azure [implementatiemodellen][virtual-machines-azure-resource-manager-architecture-benefits-arm].   

## <a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server failoverclusters

Windows Server-Failoverclustering is de basis van een hoge beschikbaarheid SAP ASC's / SCS installatie en DBMS in Windows.

Een failovercluster is een groep van 1 + n onafhankelijke servers (knooppunten) die samenwerken om de beschikbaarheid van toepassingen en services te vergroten. Als een knooppunt een fout optreedt, berekent Failoverclustering van Windows Server het aantal fouten die kunnen optreden en voor het behoud van een correct cluster de gedefinieerde toepassingen en services bieden. U kunt kiezen uit verschillende quorum modi hiervoor.

### <a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Quorum modi

U kunt kiezen uit vier quorum modi wanneer u de Failoverclustering van Windows Server gebruikt:

- **De meeste knooppunt**. Elk knooppunt van de cluster kunt stemmen. Het cluster werkt alleen met een meeste stemmen, dat wil zeggen met meer dan de helft van de stemmen. U wordt aangeraden deze optie voor clusters met een oneven aantal knooppunten. Bijvoorbeeld drie knooppunten in een cluster zeven kunnen mislukken en de cluster tussen beelden realiseert een meeste en doorgaan om uit te voeren.  

- **Knooppunt en de meeste schijf**. Elk knooppunt en een aangewezen schijf (een schijfwitness) in de opslagruimte cluster kunnen stemmen wanneer ze beschikbaar zijn en in communicatie. Het cluster werkt alleen met een grootste deel van de stemmen, dat wil zeggen met meer dan de helft van de stemmen. Deze modus relevant is in een clusteromgeving met een even getal van knooppunten. Als de helft van de knooppunten en de schijf online zijn, wordt het cluster blijft in orde zijn.

- **Knooppunt en bestandssharemeerderheid**. Elk knooppunt plus een aangewezen bestandsshare (een bestandssharewitness) die de beheerder maakt kunt stemmen, ongeacht of deze beschikbaar zijn en in communicatie. Het cluster werkt alleen met een grootste deel van de stemmen, dat wil zeggen met meer dan de helft van de stemmen. Deze modus relevant is in een clusteromgeving met een even getal van knooppunten. Is vergelijkbaar met de modus knooppunt en schijf meeste, maar het een bestandsshare getuige gebruikt in plaats van een schijf witness. Deze modus is eenvoudig te implementeren, maar als het bestand wilt delen zelf niet ten zeerste beschikbaar is, kan dit een potentieel risico worden.

- **Geen meeste: alleen schijf**. Het cluster heeft een quorum als één knooppunt alleen beschikbaar en communicatie met een specifieke schijf in de opslagruimte cluster is. Alleen de knooppunten die ook communicatie met die schijf kunnen deelnemen aan het cluster. Het is raadzaam dat u deze modus niet gebruiken.
 
## <a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server failoverclusters on-premises
Het voorbeeld in afbeelding 1 ziet een cluster van twee knooppunten. Als de netwerkverbinding tussen de knooppunten mislukt en zowel knooppunten blijven omhoog en actief is, een quorumschijf of bestand wilt delen, bepaalt welke knooppunt blijft de toepassingen en services van het cluster op te geven. Het knooppunt dat toegang tot de schijf of bestand quorumshare heeft is het knooppunt die ervoor zorgt dat dat services blijven.

Omdat in dit voorbeeld een cluster met twee knooppunten wordt, gebruiken we het knooppunt en bestandssharemeerderheid quorum-modus. Het knooppunt en de meeste schijf is ook een geldige optie. In een productieomgeving, is het raadzaam een quorumschijf te gebruiken. U kunt netwerk- en opslag technologie gebruiken om deze ten zeerste beschikbaar maken.

![Afbeelding 1: Voorbeeld van een Windows Server-Failoverclustering configuratie voor SAP ASC's / SCS in Azure wordt aangegeven][sap-ha-guide-figure-1000]

_**Afbeelding 1:** Voorbeeld van een Windows Server-Failoverclustering configuratie voor SAP ASC's / SCS in Azure wordt aangegeven_

### <a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Gedeelde opslag

Afbeelding 1 ziet u ook een cluster twee knooppunten gedeelde opslag. In een gedeelde opslag on-premises implementatie cluster opsporen alle knooppunten in het cluster gedeelde opslag. Een vergrendelingsmechanisme beveiligt de gegevens uit de beschadigde bestanden. Alle knooppunten kunnen detecteren wanneer een ander knooppunt mislukt. Als een van de knooppunten mislukt, wordt de resterende knooppunt eigenaar van de opslagbronnen voor de en zorgt ervoor dat de beschikbaarheid van services.

> [AZURE.NOTE] U hoeft geen gedeelde schijven voor maximale beschikbaarheid met sommige DBMS-toepassingen, zoals met SQL Server. SQL Server altijd op wordt overgenomen door DBMS gegevens- en logbestanden bestanden vanaf de lokale schijf van één clusterknooppunt de lokale schijf van een ander clusterknooppunt. De configuratie van het Windows-cluster nodig niet in dat geval een gedeelde schijf.

### <a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Pagina over netwerken en mailnamen omzetten

Het cluster bereiken clientcomputers via een virtueel IP-adres en de naam van een virtuele host vindt u de DNS-server. De knooppunten on-premises implementatie en de DNS-server kunnen meerdere IP-adressen verwerken.

In een standaardinstallatie, moet u twee of meer netwerkverbindingen gebruiken:

- Een speciale verbinding met de opslag
- Een cluster interne netwerkverbinding voor de heartbeat
- Een openbare netwerk waarmee clients verbinding maken met de cluster

## <a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server failoverclusters in Azure wordt aangegeven

Azure virtuele Machines vergeleken met metalen of persoonlijke cloud implementaties b, zijn extra stappen voor het configureren van Windows Server-Failoverclustering vereist. Als u een clusterschijf gedeelde samenstelt, moet u diverse IP-adressen en namen van virtuele host voor het exemplaar SAP ASC's / SCS instellen.

In dit artikel bespreken we belangrijke concepten en de extra stappen zijn nodig wanneer u een SAP-services met hoge beschikbaarheid centraal cluster in Azure wordt aangegeven maken. We zien u hoe u het hulpprogramma derden SIOS DataKeeper instelt en hoe u het configureren van de Azure interne taakverdeling. U kunt deze hulpprogramma's voor het maken van een Windows-failovercluster waarbij een bestandssharewitness in Azure wordt aangegeven.


![Afbeelding 2: Windows Server-Failoverclustering configuratie in Azure zonder een gedeelde schijf][sap-ha-guide-figure-1001]

_**Afbeelding 2:** Windows Server-Failoverclustering configuratie in Azure zonder een gedeelde schijf_


### <a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>Schijf in Azure wordt aangegeven met SIOS DataKeeper gedeeld

U moet gedeelde opslag voor een exemplaar van de SAP ASC's / SCS hoge beschikbaarheid cluster. Vanaf September 2016 bieden niet Azure gedeelde opslag die u gebruiken kunt om te maken van een gedeelde opslag cluster. U kunt software van derden SIOS DataKeeper Cluster Edition maken een gespiegeld opslag die gedeeld clusteropslag simuleert. De oplossing SIOS biedt synchroon realtimegegevens herhaling. Dit is hoe u een resource gedeelde schijf voor een cluster kunt maken:

1. Een extra Azure virtuele harde schijf (VHD) toevoegen aan elk van de virtuele machines in een Windows clusterconfiguratie.
2. Voer SIOS DataKeeper Cluster Edition op beide knooppunten VM.
3. SIOS DataKeeper Cluster Edition zodanig configureren dat deze komt overeen met de inhoud van de aanvullende VHD gekoppeld volume uit de bron virtuele machine naar de extra VHD gekoppeld volume van de doelsite virtuele machine. SIOS DataKeeper abstracts van de bronsite en doelsites lokale hoeveelheden en vervolgens gepresenteerd aan Windows Server Failoverclustering als één gedeelde schijf.

Meer informatie over [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/)krijgen.

 ![Afbeelding 3: Failoverclustering van Windows Server configuration in Azure wordt aangegeven met behulp van SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Afbeelding 3:** Configuratie van Windows Server-Failoverclustering in Azure met SIOS DataKeeper_

> [AZURE.NOTE] U hoeft geen gedeelde schijven voor maximale beschikbaarheid met sommige DBMS-producten, zoals SQL Server. SQL Server altijd op wordt overgenomen door DBMS gegevens- en logbestanden bestanden vanaf de lokale schijf van één clusterknooppunt de lokale schijf van een ander clusterknooppunt. De configuratie van het Windows-cluster nodig niet in dit geval een gedeelde schijf.

### <a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>Naamresolutie in Azure wordt aangegeven

 Het platform Azure cloud niet mogelijk is de optie voor het configureren van virtuele IP-adressen, zoals zwevende IP-adressen. Reden moet u een alternatieve oplossing voor het instellen van een virtueel IP-adres naar de resource cluster in de cloud hebt bereikt.
Azure heeft een interne taakverdeling in de taakverdeling Azure-service. Met de interne taakverdeling bereikt clients het cluster via het cluster virtuele IP-adres.
Moet u de interne taakverdeling in de resourcegroep knooppunten met implementeren. Configureer vervolgens alle benodigde poort doorstuurregels met de test poorten van de interne taakverdeling.
De clients kunnen verbinding maken via de virtuele hostnaam. De DNS-server lost u het IP-adres van de cluster en de interne laden verdeling grepen poort doorsturen naar het actieve knooppunt van het cluster.

## <a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>Hoge beschikbaarheid SAP NetWeaver in Azure-infrastructuur-als-een-service (IaaS)

Als u wilt bereiken van SAP-toepassing beschikbaarheid, zoals voor onderdelen van SAP-software, moet u de volgende onderdelen beveiligen. Hiermee wordt beschreven in [SAP NetWeaver op Windows virtuele machines (VMs) – Planning en implementatiehandleiding] uitgebreider [planning-handleiding-11].

- SAP-toepassing-servers
- SAP ASC's / SCS exemplaar
- DBMS-server

### <a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Hoge beschikbaarheid SAP-toepassingsservers

Hoeft u niet een specifieke hoge beschikbaarheid-oplossing voor de servers voor SAP-toepassing en het dialoogvenster exemplaren. U bereiken beschikbaarheid redundantie en configureert u meerdere exemplaren van het dialoogvenster in verschillende exemplaren van Azure virtuele Machines. U moet ten minste twee SAP-toepassingsexemplaren geïnstalleerd in twee instanties van Azure virtuele Machines hebben.

![Afbeelding 4: Hoge beschikbaarheid SAP-toepassingsservers][sap-ha-guide-figure-2000]

_**Afbeelding 4:** Hoge beschikbaarheid SAP-toepassingsservers_

Alle virtuele machines die host SAP-toepassing-servers in de dezelfde Azure beschikbaarheid instellen, moet u plaatsen. Een set beschikbaarheid van Azure zorgt ervoor dat:

- Alle virtuele machines maken deel uit van de upgrade hetzelfde domein. Een upgrade-domein, bijvoorbeeld wordt gecontroleerd of dat de virtuele machines op hetzelfde moment tijdens gepland onderhoud downtime worden niet bijgewerkt.
- Alle virtuele machines maken deel uit van hetzelfde foutenstructuuranalyse domein. Een domein foutenstructuuranalyse, bijvoorbeeld ervoor zorgt dat virtuele machines zijn geïmplementeerd zodat er geen potentieel risico van invloed is op de beschikbaarheid van alle virtuele machines.

Meer informatie over het [beheren van de beschikbaarheid van virtuele machines][virtual-machines-manage-availability].

Omdat de opslag van Azure-account een mogelijke potentieel risico is, is het belangrijk dat ten minste twee Azure opslag-accounts, waarin ten minste twee virtuele machines zijn verdeeld. In een ideaal instelling, zou elke virtuele machine met een SAP-dialoogvenster exemplaar worden geïmplementeerd in een andere opslag-account.

### <a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Hoge beschikbaarheid SAP ASC's / SCS exemplaar

![Afbeelding 5: Hoge beschikbaarheid SAP ASC's / SCS exemplaar][sap-ha-guide-figure-2001]

_**Afbeelding 5:** Hoge beschikbaarheid SAP ASC's / SCS exemplaar_


#### <a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>Hoge beschikbaarheid SAP ASC's / SCS exemplaar met Windows Server Failoverclustering in Azure wordt aangegeven

Azure virtuele Machines vergeleken met metalen of persoonlijke cloud implementaties b, zijn extra stappen voor het configureren van Windows Server-Failoverclustering vereist. Als u een Windows-failovercluster, moet u een gedeelde clusterschijf, verschillende IP-adressen, verschillende virtuele hostnamen en een Azure interne taakverdeling voor een SAP ASC's / SCS exemplaar cluster.

Wordt hiermee later in het artikel uitvoeriger besproken.

![Afbeelding 6: Windows Server Failoverclustering voor een SAP ASC's / SCS-configuratie in Azure wordt aangegeven met behulp van SIOS DataKeeper][sap-ha-guide-figure-1002]

_**Afbeelding 6:** Windows Server Failoverclustering voor een SAP ASC's / SCS-configuratie in Azure wordt aangegeven met SIOS DataKeeper_


### <a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Hoge beschikbaarheid DBMS exemplaar

De DBMS is ook een potentieel contactpersoon van een SAP-systeem. U moet deze beveiligen met een hoge beschikbaarheid-oplossing. Afbeelding 7 toont een voorbeeld van een SQL Server altijd op hoge beschikbaarheid-oplossing in Azure wordt aangegeven met behulp van Windows Server-Failoverclustering en de verdeling van de Azure interne laden. SQL Server altijd op wordt overgenomen door DBMS gegevens- en logbestanden bestanden met behulp van een eigen herhaling DBMS. In dit geval nodig hebt u niet cluster gedeelde schijven, die de hele instelling eenvoudiger.

![Afbeelding 7: Voorbeeld van een hoge beschikbaarheid SAP-DBMS: SQL Server altijd op][sap-ha-guide-figure-2003]

_**Afbeelding 7:** Voorbeeld van een hoge beschikbaarheid SAP-DBMS: SQL Server altijd op_


Raadpleeg de volgende artikelen voor meer informatie over het cluster van SQL Server in Azure wordt aangegeven met behulp van het implementatiemodel resourcemanager Azure:

- [Groep altijd op beschikbaarheid in Azure virtuele Machines handmatig configureren met behulp van bronbeheer][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
- [Een interne taakverdeling voor een groep altijd op beschikbaarheid in Azure configureren][virtual-machines-windows-portal-sql-alwayson-int-listener]

### <a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>End-to-end hoge beschikbaarheid implementatie scenario 's

Figuur 8 toont een voorbeeld van een SAP NetWeaver hoge beschikbaarheid architectuur in Azure wordt aangegeven. In dit scenario gebruiken we een speciale cluster voor het exemplaar SAP ASC's / SCS en een andere voor het DBMS.

![Afbeelding 8: SAP HA architectuur sjabloon 1, met een speciale cluster voor ASC's / SCS en een speciale cluster voor het exemplaar DBMS][sap-ha-guide-figure-2004]

_**Figuur 8:** SAP HA architectuur sjabloon 1: Speciale clusters voor ASC's / SCS en DBMS_

## <a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>De infrastructuur voorbereiden

Azure resourcemanager sjablonen voor SAP vereenvoudigen implementatie van vereiste resources.

De drie lagen-sjablonen ondersteunen ook hoge beschikbaarheid-scenario's, zoals architectuur sjabloon 1, waarin twee clusters heeft. Elk cluster is een SAP potentieel risico voor SAP ASC's / SCS en DBMS.

Hier ziet u waar u Azure resourcemanager sjablonen kunt ophalen voor Scenario 1:

- [Afbeelding van Azure Marketplace](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
- [Aangepaste afbeelding](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

Wanneer u de afbeelding van SAP drie lagen Marketplace selecteert, wordt dit scherm wordt weergegeven in de portal van Azure:

![Afbeelding 9: SAP hoge beschikbaarheid Azure resourcemanager parameters opgeeft][sap-ha-guide-figure-3000]

_**Afbeelding 9:** SAP hoge beschikbaarheid Azure resourcemanager parameters opgeeft_


Selecteer in **SYSTEMAVAILABILITY**, **HA**.

De sjablonen maken:

- **Virtuele machines**:
    - SAP toepassingsserver virtuele machines: <*SAPSystemSID*> - di - <*getal*>
    - ASC's / SCS cluster virtuele machines: <*SAPSystemSID*> - ASC's - <*getal*>
    - Een cluster DBMS: <*SAPSystemSID*> - db - <*getal*>
- **Netwerkkaarten voor alle virtuele machines, met de bijbehorende IP-adressen**:
    - <*SAPSystemSID*> - nic - di - <*getal*>
    - <*SAPSystemSID*> - nic - ASC's - <*getal*>
    - <*SAPSystemSID*> - nic - db - <*getal*>
- **Azure opslag-accounts**
- **Beschikbaarheid van de groepen** voor:
    - SAP toepassingsserver virtuele machines: di <*SAPSystemSID*> - avset -
    - SAP ASC's / SCS cluster virtuele machines: <*SAPSystemSID*> - avset - ASC's
    - DBMS cluster virtuele machines: <*SAPSystemSID*> - avset - db
- **Azure interne belasting voor documentconversies**:
  - Met alle poorten voor de ASC's / SCS adres exemplaar en IP-<*SAPSystemSID*> - kg - ASC 's
  - Met alle poorten voor de SQL Server DBMS en IP-adres <*SAPSystemSID*> - kg - db
- **Beveiligingsgroep netwerk**: <*SAPSystemSID*> - nsg - ASC's-0  
    - Met een open externe Remote Desktop Protocol (RDP) poort op de <*SAPSystemSID*> - ASC's - 0 virtuele machine


> [AZURE.NOTE] Alle IP-adressen van de netwerkkaarten en Azure interne netwerktaakverdelers zijn **dynamische** al dan niet standaard. Wijzig deze in **statische** IP-adressen. We dit later in dit artikel wordt beschreven.


### <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Virtuele machines met bedrijfsnetwerk connectivity (cross-premises) gebruik in productie implementeren

Implementeren voor SAP systemen Azure virtuele machines met bedrijfsnetwerk connectiviteit (cross-premises) [planning-handleiding-2.2] met behulp van Azure naar website VPN of Azure ExpressRoute.

> [AZURE.NOTE] U kunt uw exemplaar van de Azure Virtual Network gebruiken. Het virtuele netwerk en subnet zijn al gemaakt en voorbereid.

Selecteer in **NEWOREXISTINGSUBNET**, de optie **bestaande**.

In **SUBNETID**, voegt u de volledige tekenreeks van uw voorbereide Azure netwerk SubnetID, waar u van plan bent om te implementeren van uw Azure virtuele machines.

Deze PowerShell-opdracht om te zien van alle Azure netwerksubnetten uitvoeren:

```powershell
(Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
```

Het veld **ID** bevat de **SUBNETID**.

U kunt een lijst met alle **SUBNETID** waarden ophalen met behulp van deze PowerShell-opdracht:

```PowerShell
(Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
```

De **SUBNETID** ziet er zo uit:

```
/subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
```

### <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>Alleen de cloud implementatie van SAP-exemplaren voor test en demo

U kunt ook uw SAP-systeem van hoge beschikbaarheid in een implementatiemodel alleen de cloud implementeren.

Dit soort implementatie is vooral handig voor demo of test gebruik gevallen. Het niet geschikt voor productiecases gebruiken.

Selecteer in **NEWOREXISTINGSUBNET**, de optie **Nieuw**. Laat het veld **SUBNETID** leeg.

De sjabloon SAP Azure resourcemanager wordt automatisch gemaakt het Azure virtuele netwerk en subnet.

> [AZURE.NOTE] Ook moet u ten minste één speciale VM implementeren voor Active Directory en DNS in dezelfde Azure Virtual Network-sessie. De sjabloon niet gemaakt in deze virtuele machines.

### <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Azure Virtual Network

In ons voorbeeld is de adresruimte van het Azure virtuele netwerk 10.0.0.0/16. Er is één subnet **Subnet**, met een adresbereiken van 10.0.0.0/24 genoemd. Alle virtuele machines en interne netwerktaakverdelers zijn geïmplementeerd in deze virtuele netwerk.

> [AZURE.NOTE] Geen wijzigingen aanbrengen de netwerkinstellingen binnen het gastbesturingssysteem. Dit geldt ook voor IP-adressen, DNS-servers en subnet. Uw netwerkinstellingen configureren in Azure wordt aangegeven. De service Dynamic Host Configuration Protocol (DHCP) geeft uw instellingen.

### <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS IP-adressen

Zorg dat uw virtuele netwerk de optie **DNS-Servers** is ingesteld op **Aangepaste DNS**.
Selecteer uw instellingen op basis van het type netwerk dat u hebt:

- Bedrijfsnetwerk connectivity (cross-premises) [planning-handleiding-2.2]: Voeg het IP-adressen van de lokale DNS-servers.  

    U kunt de lokale DNS-servers de virtuele machines die worden uitgevoerd in Azure uitbreiden. In dit scenario kunt u het IP-adressen van de Azure virtuele machines waarop u de DNS-service uitvoert toevoegen.

-   [Alleen de cloud-implementatie] [planning-handleiding-2.1]: een extra VM in dezelfde Virtual Network instantie die als een DNS-server fungeert implementeren. Voeg het IP-adressen van de Azure virtuele machines die u hebt ingesteld voor het uitvoeren van DNS-service.


![Afbeelding 10: DNS-servers configureren voor Azure Virtual Network][sap-ha-guide-figure-3001]

_**Afbeelding 10:** DNS-servers configureren voor Azure Virtual Network_

> [AZURE.NOTE] Als u het IP-adressen van de DNS-servers wijzigt, moet u opnieuw starten van de Azure virtuele machines als u wilt toepassen van de wijziging en doorgeven van de nieuwe DNS-servers.

In ons voorbeeld wordt de DNS-service is geïnstalleerd en geconfigureerd op deze Windows virtuele machines:

| VM rol        | VM hostnaam | Kaart netwerknaam  | Statisch IP-adres  
| ---------------|-------------|--------------------|-------------------
| Eerste DNS-server | domcontr-0  | PR1-nic-domcontr-0 | 10.0.0.10         
| Tweede DNS-server | domcontr-1  | PR1-nic-domcontr-1 | 10.0.0.11         


### <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Hostnamen en statische IP-adressen voor de SAP ASC's / SCS gegroepeerd exemplaar en gegroepeerd exemplaar DBMS

Voor on-premises implementatie moet u deze gereserveerde hostnamen en IP-adressen:

| Virtuele host naam rol                                                       | Virtuele hostnaam | Virtuele statische IP-adres
| ----------------------------------------------------------------------------|------------------|---------------------------
| SAP ASC's / SCS eerste cluster virtuele hostnaam (voor cluster management) | PR1-ASC's-vir     | 10.0.0.42                 
| SAP ASC's / SCS exemplaar virtuele hostnaam                                  | PR1-ASC's-sap     | 10.0.0.43             
| SAP DBMS tweede cluster virtuele hostnaam (cluster management)     | PR1-dbms-vir     | 10.0.0.32                 

Wanneer u het cluster maakt, maakt u de namen van virtuele host **pr1-ASC's-vir** en **pr1-dbms-vir** en de bijbehorende IP-adressen die het cluster zelf te beheren. Het proces wordt beschreven in [verzamelen knooppunten in clusterconfiguratie] [sap-ha-handleiding-8.12.1].

U kunt handmatig de andere twee virtuele hostnamen, **pr1-ASC's-sap** en **pr1-dbms-sap**en de bijbehorende IP-adressen, op de DNS-server maken. Gebruik deze informatiebronnen het geclusterde SAP ASC's / SCS exemplaar en de gegroepeerde DBMS exemplaar. Hiermee wordt beschreven in [maken de naam van een virtuele host voor gegroepeerd SAP-ASCS/SCS][sap-ha-guide-9.1.1].

### <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Statische IP-adressen voor de SAP virtuele machines instellen

Nadat u de virtuele machines gebruiken in uw cluster implementeert, moet u statische IP-adressen voor alle virtuele machines instellen. Deze stap herhalen in Azure virtuele netwerkconfiguratie en niet in het gastbesturingssysteem.

Er is een manier om in te stellen van een statische IP-adres met behulp van de Azure-portal. Ga in de portal van Azure naar **Resourcegroep** > **Netwerkkaart** > **Instellingen** > **IP-adres**.

Schakel onder **toewijzing**, **statische**in. Voer in het veld **IP-adres van** het IP-adres dat u wilt gebruiken.

> [AZURE.NOTE] Als u het IP-adres van de netwerkkaart wijzigt, moet u opnieuw starten van de Azure virtuele machines als de wijziging wilt toepassen.  


![Afbeelding 11: Statische IP-adressen voor de netwerkkaart van elke virtuele machine instellen][sap-ha-guide-figure-3002]

_**Afbeelding 11:** Statische IP-adressen voor de netwerkkaart van elke virtuele machine instellen_

Herhaal deze stap voor alle netwerkinterfaces, die is, voor alle virtuele machines, inclusief virtuele machines die u wilt gebruiken voor uw Active Directory/DNS-service.

In ons voorbeeld hebben we deze virtuele machines en statische IP-adressen:

| VM rol                                 | VM hostnaam  | Kaart netwerknaam  | Statisch IP-adres  
| ----------------------------------------|--------------|--------------------|-------------------
| Eerste SAP-toepassingsserver              | PR1-di-0     | PR1-nic-di-0       | 10.0.0.50         
| Tweede SAP-toepassingsserver              | PR1-di-1     | PR1-nic-di-1       | 10.0.0.51         
| ...                                     | ...          | ...                | ...               
| Laatste SAP-toepassingsserver             | PR1-di-5     | PR1-nic-di-5       | 10.0.0.55         
| Eerste knooppunt voor ASC's / SCS exemplaar  | PR1-ASC's-0   | PR1-nic-ASC's-0     | 10.0.0.40         
| Tweede clusterknooppunt voor ASC's / SCS exemplaar  | PR1-ASC's-1   | PR1-nic-ASC's-1     | 10.0.0.41         
| Eerste knooppunt voor DBMS exemplaar      | PR1-db-0     | PR1-nic-db-0       | 10.0.0.30         
| Tweede clusterknooppunt voor DBMS exemplaar      | PR1-db-1     | PR1-nic-db-1       | 10.0.0.31         

### <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Een statische IP-adres voor de interne taakverdeling instellen

De sjabloon SAP Azure resourcemanager Hiermee maakt u een Azure interne taakverdeling die wordt gebruikt voor het SAP ASC's / SCS exemplaar cluster en het cluster DBMS.

De eerste installatie Hiermee stelt u de interne laden de verdeling van IP-adres op **dynamische**. Het is belangrijk om te wijzigen van het IP-adres in **statische**.

In ons voorbeeld hebben we twee Azure interne netwerktaakverdelers die deze statische IP-adressen:

| Functie van de verdeling van Azure interne laden             | De naam van de verdeling van Azure interne laden | Statisch IP-adres
| ---------------------------|----------------|-------------------
| SAP ASC's / SCS exemplaar interne taakverdeling  | PR1-kg-ASC 's    | 10.0.0.43         
| SAP DBMS interne belasting voor documentconversies               | PR1-kg-dbms    | 10.0.0.33         


> [AZURE.NOTE] Het IP-adres van de virtuele host-naam van het SAP ASC's / SCS is hetzelfde als het IP-adres van de SAP ASC's / SCS interne laden de verdeling van pr1-kg-ASC's.
Het IP-adres van de virtuele naam van het DBMS is hetzelfde als het IP-adres van de DBMS interne laden de verdeling van pr1-kg-dbms.

In ons voorbeeld stellen we het IP-adres van de interne laden de verdeling van **pr1-kg-ASC's** op het IP-adres van de virtuele hostnaam van het SAP ASC's / SCS-exemplaar (in dit voorbeeld **10.0.0.43**).

![Figuur 12: Statische IP-adressen voor de verdeling van de interne belasting voor het exemplaar SAP ASC's / SCS instellen][sap-ha-guide-figure-3003]

_**Figuur 12:** Statische IP-adressen voor de verdeling van de interne belasting voor het exemplaar SAP ASC's / SCS instellen_

Stel het IP-adres van de interne laden de verdeling van **pr1-kg-dbms** naar het IP-adres van de virtuele hostnaam van het exemplaar DBMS (in dit voorbeeld **10.0.0.33**).

### <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Regels voor de verdeling van de Azure interne belasting van taakverdeling ASC's / SCS standaard

De sjabloon SAP Azure resourcemanager Hiermee maakt u de poorten die u nodig hebt:

- Een exemplaar ABAP ASCS, klikt u met de standaardexemplaar **00** nummeren
- Een exemplaar Java SCS, klikt u met de standaardexemplaar **01** nummeren

Wanneer u uw exemplaar van SAP ASC's / SCS installeert, moet u het exemplaar standaardaantal 00 voor uw exemplaar ABAP ASCS en het exemplaar standaardaantal 01 voor uw exemplaar Java SCS.

Maak vervolgens deze vereiste interne taakverdeling eindpunten voor de poorten SAP NetWeaver ABAP ASC's:

| Taakverdeling regelnaam service/laden           | Standaardpoortnummers | Betonnen poorten voor (ASC's exemplaar met exemplaarnummer 00) (klanten met 10)  
| ---------------------------------------------|-----------------------|--------------------------------------------------------------------------
| Wachtrij plaatsen Server / _lbrule3200_                | 32 <*InstanceNumber*>  | 3200                                                                     
| ABAP berichtenserver / _lbrule3600_           | 36 <*InstanceNumber*>  | 3600                                                                     
| Interne ABAP bericht / _lbrule3900_         | 39 <*InstanceNumber*>  | 3900                                                                     
| Bericht van Server HTTP / _Lbrule8100_           | 81 <*InstanceNumber*>  | 8100                                                                     
| SAP: Start Service ASC's HTTP / _Lbrule50013_  | 5 <*InstanceNumber*> 13 | 50013                                                                    
| SAP: Start Service ASC's HTTPS / _Lbrule50014_ | 5 <*InstanceNumber*> 14 | 50014                                                                    
| Wachtrij plaatsen herhaling / _Lbrule50016_          | 5 <*InstanceNumber*> 16 | 50016                                                                    
| SAP: Start Service uit HTTP _Lbrule51013_     | 5 <*InstanceNumber*> 13 | 51013                                                                    
| SAP: Start Service uit HTTP _Lbrule51014_     | 5 <*InstanceNumber*> 14 | 51014                                                                    
| Win RM _Lbrule5985_                          |                       | 5985                                                                     
| Bestand delen _Lbrule445_                       |                       | 445                                                                      

_**Tabel 1:** Poortnummers van de exemplaren SAP NetWeaver ABAP ASC 's_


Maak deze vereiste interne taakverdeling eindpunten voor de poorten SAP NetWeaver Java SCS:

| Taakverdeling regelnaam service/laden           | Standaardpoortnummers | Betonnen poorten voor (SCS exemplaar met een getal dat exemplaar 01) (klanten met 11)  
| ---------------------------------------------|-----------------------|--------------------------------------------------------------------------
| Wachtrij plaatsen Server / _lbrule3201_                | 32 <*InstanceNumber*>  | 3201                                                                     
| Gateway-Server / _lbrule3301_                | 33 <*InstanceNumber*>  | 3301                                                                     
| Java berichtenserver / _lbrule3900_           | 39 <*InstanceNumber*>  | 3901                                                                     
| Bericht van Server HTTP / _Lbrule8101_           | 81 <*InstanceNumber*>  | 8101                                                                     
| SAP: Start Service SCS HTTP / _Lbrule50113_   | 5 <*InstanceNumber*> 13 | 50113                                                                    
| SAP: Start Service SCS HTTPS / _Lbrule50114_  | 5 <*InstanceNumber*> 14 | 50114                                                                    
| Wachtrij plaatsen herhaling / _Lbrule50116_          | 5 <*InstanceNumber*> 16 | 50116                                                                    
| SAP: Start Service uit HTTP _Lbrule51113_     | 5 <*InstanceNumber*> 13 | 51113                                                                    
| SAP: Start Service uit HTTP _Lbrule51114_     | 5 <*InstanceNumber*> 14 | 51114                                                                    
| Win RM _Lbrule5985_                          |                       | 5985                                                                     
| Bestand delen _Lbrule445_                       |                       | 445                                                                      

_**Tabel 2:** Poortnummers van de exemplaren SAP NetWeaver Java SCS_


![Afbeelding 13: Standaard ASC's / SCS taakverdeling regels voor de verdeling van de Azure interne laden][sap-ha-guide-figure-3004]

_**Afbeelding 13:** Regels voor de verdeling van de Azure interne belasting van taakverdeling ASC's / SCS standaard_

Stel het IP-adres van de belasting voor gelijkmatige verdeling **pr1-kg-dbms** naar het IP-adres van de virtuele hostnaam van het exemplaar DBMS (in dit voorbeeld **10.0.0.33**).

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Wijzigen van de ASC's / SCS standaard taakverdeling regels voor de verdeling van de Azure interne laden

Als u verschillende getallen gebruiken voor de SAP ASC's of SCS exemplaren wilt, moet u de namen en waarden van deze poorten bijwerken.

Een manier om bij te werken exemplaar getallen is met behulp van de Azure-portal:

Ga naar * *<*beveiligings-id*> - kg - ASC's de belasting voor documentconversies** > **laden verdelen van regels **.

Voor alle taakverdeling regels die deel uitmaakt van het SAP ASC's of SCS exemplaar, deze waarden te wijzigen:

- Naam
- Poort
- Back-enddatabase poort

Als u wijzigen van het standaardaantal ASC's in het exemplaar van 00 tot en met 31 wilt, moet u bijvoorbeeld, breng de wijzigingen voor alle poorten in de lijst in de tabel 1.

Hier volgt een voorbeeld van een update voor de poort _lbrule3200_.

![Afbeelding 14: Wijzig de ASC's / SCS standaard taakverdeling regels voor de verdeling van de Azure interne laden][sap-ha-guide-figure-3005]

_**Figuur 14:** Wijzigen van de ASC's / SCS standaard taakverdeling regels voor de verdeling van de Azure interne laden_

### <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Windows virtuele machines toevoegen aan het domein

Nadat u een statische IP-adres aan de virtuele machines toewijzen, moet u de virtuele machines toevoegen aan het domein.

![Afbeelding 15: Een virtuele machine aan een domein toevoegen][sap-ha-guide-figure-3006]

_**Figuur 15:** Een virtuele machine aan een domein toevoegen_

### <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Registervermeldingen op beide knooppunten van het SAP ASC's / SCS exemplaar toevoegen

Azure taakverdeling heeft een interne taakverdeling waarmee wordt gesloten verbindingen wanneer de verbindingen actief voor een bepaalde zijn tijd (een inactiviteit). SAP processen in dialoogvenster exemplaren openen verbindingen met het SAP-plaatsen verwerken zodra de eerste in wachtrij plaatsen/wachtrij aanvragen nodig heeft om te worden verzonden. Deze verbindingen meestal blijven waarop deze is opgezet totdat het werkproces of het proces plaatsen opnieuw opstarten. Als de verbinding niet actief voor een periode is, sluit de Azure interne taakverdeling echter de verbindingen. Dit is een probleem niet, omdat het SAP-werkproces herstelt u de verbinding met het proces plaatsen als deze niet langer bestaat. Deze activiteiten worden beschreven in de traces ontwikkelaars van SAP-processen, maar ze een grote hoeveelheid extra inhoud in deze sporen maken. Het is een goed idee om te wijzigen van het TCP/IP `KeepAliveTime` en `KeepAliveInterval` op beide clusterknooppunten. Combineer deze wijzigingen in de TCP/IP-parameters met SAP-profiel parameters, die later in dit artikel worden beschreven.

Deze Windows-registervermeldingen op beide Windows clusterknooppunten voor SAP ASC's / SCS toevoegen:

| Pad                  | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters   
| ----------------------|-----------------------------------------------------------
| De naam van de variabele         | `KeepAliveTime`                                              
| Type variabele         | REG_DWORD (decimaal)                                        
| Waarde                 | 120000                                                     
| Koppeling naar documentatie | [https://technet.Microsoft.com/en-us/library/cc957549.aspx](https://technet.microsoft.com/en-us/library/cc957549.aspx)


_**Tabel 3:** De eerste TCP/IP-parameter wijzigen_


| Pad                  | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters   
| ----------------------|-----------------------------------------------------------
| De naam van de variabele         | `KeepAliveInterval`                                          
| Type variabele         | REG_DWORD (decimaal)                                        
| Waarde                 | 120000                                                     
| Koppeling naar documentatie | [https://technet.Microsoft.com/en-us/library/cc957548.aspx](https://technet.microsoft.com/en-us/library/cc957548.aspx)


_**Tabel 4:** De tweede TCP/IP-parameter wijzigen_

Start opnieuw beide knooppunten om de wijzigingen van kracht.

### <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Een cluster Failoverclustering van Windows Server voor een SAP ASC's / SCS exemplaar instellen

#### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Knooppunten in een clusterconfiguratie verzamelen

De eerste stap is failoverclusters aan beide knooppunten toevoegen. Gebruik de functie en de Wizard onderdelen toevoegen.

De tweede stap is voor het instellen van het failovercluster met behulp van Failoverclusterbeheer.

In Failoverclusterbeheer, selecteer **Cluster maken**en voegt u alleen de naam van het eerste knooppunt A. Voeg bijvoorbeeld **pr1-ASC's-0**. Het tweede knooppunt niet nog opgeteld. U voegt het tweede knooppunt in een volgende stap.

![Afbeelding 16: De naam van de server of VM van het eerste knooppunt toevoegen][sap-ha-guide-figure-3007]

_**Afbeelding 16:** De naam van de server of VM van het eerste knooppunt toevoegen_

Vervolgens wordt u gevraagd voor de netwerknaam (virtual hostnaam) van het cluster.

![Afbeelding 17: De clusternaam van de opgeven][sap-ha-guide-figure-3008]

_**Afbeelding 17:** De clusternaam van de opgeven_


Nadat u het cluster hebt gemaakt, voer een cluster validatie-toets.

![Figuur 18: De controle van de validatie cluster uitvoeren][sap-ha-guide-figure-3009]

_**Figuur 18:** De cluster validatiecontrole uitvoeren_

![Figuur 19: Geen quorumschijf is gevonden.][sap-ha-guide-figure-3010]

_**Figuur 19:** Geen quorumschijf is gevonden_

Waarschuwingen met betrekking tot schijven op dit punt in het proces, kunt u negeren. U voegt dat een bestandssharewitness de SIOS gedeelde en schijven later. In dit stadium moet u niet bang te zijn met een quorum.

![Figuur 20: Core cluster resource moet een nieuw IP-adres][sap-ha-guide-figure-3011]

_**Figuur 20:** Core cluster resource moet een nieuw IP-adres_

Het cluster starten niet omdat het IP-adres van de server naar een van de knooppunten VM verwijst. U moet het IP-adres van het cluster core wijzigen.

Bijvoorbeeld, moeten we een IP-adres (in dit voorbeeld **10.0.0.42**) toewijzen voor de cluster virtuele host naam **pr1-ASC's-vir**. Deze stap herhalen op de eigenschappenpagina van de core cluster-service IP-resource, weergegeven in figuur 21.

![Figuur-21: In de ** eigenschappen ** dialoogvenster vak, wijzigt u het IP-adres][sap-ha-guide-figure-3012]

_**Figuur 21:** Wijzig het IP-adres in het dialoogvenster **Eigenschappen**_

![Afbeelding van 22: Het IP-adres dat is gereserveerd voor het cluster toewijzen][sap-ha-guide-figure-3013]

_**Figuur 22:** Het IP-adres dat is gereserveerd voor het cluster toewijzen_

Nadat u het IP-adres hebt gewijzigd, geef de cluster virtuele hostnaam online.

![Figuur 23: Cluster core-service is gestart en uitgevoerd, en met het juiste IP-adres][sap-ha-guide-figure-3014]

_**Figuur 23:** Cluster core-service is gestart en wordt uitgevoerd, en met het juiste IP-adres_

Nu de core cluster-service actief is, kunt u het tweede clusterknooppunt toevoegen.

![Figuur 24: Het tweede clusterknooppunt toevoegen][sap-ha-guide-figure-3015]

_**Figuur 24:** Het tweede clusterknooppunt toevoegen_

![Afbeelding 25: Toevoegen de tweede cluster knooppunt hostnaam, bijvoorbeeld pr1-ASC's-1][sap-ha-guide-figure-3016]

_**Afbeelding 25:** De tweede cluster knooppunt hostnaam, bijvoorbeeld **pr1-ASC's-1** toevoegen_

![Afbeelding 26: Schakel het selectievakje][sap-ha-guide-figure-3017]

_**Afbeelding 26:** Kan *niet* selecteren het selectievakje_

> [AZURE.IMPORTANT] Zorg dat het selectievakje **alle in aanmerking komend opslagruimte aan het cluster toevoegen** *niet* is ingeschakeld.  

![Afbeelding 27: Waarschuwingen over de schijf quorum negeren][sap-ha-guide-figure-3018]

_**Afbeelding 27:** Waarschuwingen over de schijf quorum negeren_

U kunt waarschuwingen over quorum en schijven negeren. U gaat het quorum instellen en delen van de schijf later, zoals wordt beschreven in [installatie SIOS DataKeeper Cluster Edition voor SAP ASC's / SCS cluster delen schijf] [sap-ha-handleiding-8.12.3].

#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>Een cluster bestandssharewitness configureren

##### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Een bestandsshare maken

Selecteer een bestandssharewitness in plaats van een quorumschijf. SIOS DataKeeper ondersteunt deze optie.

In de voorbeelden in dit artikel worden de bestandssharewitness is op de Active Directory/DNS-server die wordt uitgevoerd in Azure wordt aangegeven. De bestandssharewitness heet **domcontr-0**. Omdat u zou een VPN (VPN)-verbinding met Azure (via VPN van Site-naar-Site of Azure ExpressRoute) hebt geconfigureerd, wordt in uw Active Directory/DNS service wordt on-premises en is niet geschikt is voor het uitvoeren van een bestand getuige delen.

> [AZURE.NOTE] Als uw Active Directory/DNS-service wordt uitgevoerd alleen on-premises implementatie, niet uw bestandssharewitness configureren op de Active Directory en DNS-Windows-besturingssysteem met on-premises implementatie. Netwerklatentie tussen knooppunten met Azure en Active Directory/DNS on-premises implementatie wellicht te groot zijn en verbindingsproblemen veroorzaken. Zorg ervoor dat de bestandssharewitness configureren op een Azure virtuele machine die dicht bij het knooppunt wordt uitgevoerd.  

Het quorumstation moet ten minste 1.024 MB beschikbare ruimte. U wordt aangeraden 2048 MB beschikbare ruimte.

De eerste stap is het cluster naam object toevoegen.

![Afbeelding 28: De machtigingen voor het delen voor het object met cluster naam toewijzen][sap-ha-guide-figure-3019]

_**Afbeelding 28:** De machtigingen voor het delen voor het object met cluster naam toewijzen_

Zorg ervoor dat die de machtigingen bevat de instantie om gegevens in de optie voor delen voor het object cluster naam (in dit voorbeeld **pr1-ASC's-vir$**) te wijzigen. Als u wilt het cluster naam object toevoegen aan de lijst, selecteert u **toevoegen**. Het filter wilt controleren op computerobjecten, die wordt weergegeven in de afbeelding 29 wijzigen:

![Afbeelding 29: Het objecttype als u wilt opnemen computerobjecten wijzigen][sap-ha-guide-figure-3020]

_**Afbeelding 29:** Het objecttype om op te nemen computerobjecten wijzigen_

![Afbeelding 30: Schakel het selectievakje voor computerobjecten][sap-ha-guide-figure-3021]

_**Afbeelding 30:** Schakel het selectievakje voor computerobjecten_

Voer vervolgens het cluster name-object, zoals wordt weergegeven in de afbeelding 29. Omdat de record al is gemaakt, kunt u de machtigingen, zoals wordt weergegeven in de afbeelding 28 is ingesteld.

Vervolgens selecteert u het tabblad **beveiliging** van de optie voor delen en vervolgens meer gedetailleerde machtigingen instellen voor het object cluster naam.

![Afbeelding 31: De beveiligingskenmerken voor het object met cluster naam instellen voor het bestand delen quorum][sap-ha-guide-figure-3022]

_**Afbeelding 31:** De beveiligingskenmerken voor het object met cluster naam instellen voor het bestand delen quorum_

##### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Het bestand delen getuige quorum in Failoverclusterbeheer instellen

Configuratie van het cluster in Failoverclusterbeheer wijzigen in een bestandssharewitness.

![Afbeelding 32: Start de Wizard configureren Cluster Quorum-instelling][sap-ha-guide-figure-3023]

_**Afbeelding 32:** Start de Wizard configureren Cluster Quorum-instelling_

![Afbeelding 33: Quorumconfiguraties die kunt u kiezen uit][sap-ha-guide-figure-3024]

_**Afbeelding 33:** U kunt kiezen uit Quorumconfiguraties_

Selecteer **de witness quorum selecteren**.

![Afbeelding 34: Selecteer de bestandssharewitness][sap-ha-guide-figure-3025]

_**Afbeelding 34:** Selecteer de bestandssharewitness_

Selecteer **configureren een bestand delen witness**.

![Afbeelding 35: De locatie van het delen voor het delen van getuige definiëren][sap-ha-guide-figure-3026]

_**Afbeelding 35:** De locatie van het delen voor het delen van getuige definiëren_

Geef het UNC-pad naar het bestand delen (in ons voorbeeld \\domcontr-0\FSW).

Selecteer **volgende** voor een overzicht van de wijzigingen die u kunt aanbrengen. Selecteer de gewenste wijzigingen en selecteer vervolgens **volgende**.

![Afbeelding 36: Bevestigen dat u hebt geconfigureerd het cluster][sap-ha-guide-figure-3027]

_**Afbeelding 36:** Bevestigen dat u hebt het cluster geconfigureerd_

In deze laatste stap moet u succes configureren, de configuratie van het cluster, zoals wordt weergegeven in de afbeelding 36.  

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>SIOS DataKeeper Cluster Edition installeren voor de schijf SAP ASC's / SCS cluster delen

U hebt nu een werkende Failoverclustering van Windows Server configuration in Azure wordt aangegeven. Maar als u wilt een exemplaar van SAP ASC's / SCS installeert, moet u een resource gedeelde schijf. U kunt de gedeelde schijfbronnen die u nodig hebt in Azure maken. SIOS DataKeeper Cluster Edition is een derde partij-oplossing die kunt u gedeelde schijf resources maken.

#### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>.NET Framework 3.5 toevoegen

Microsoft .NET Framework 3.5 is niet automatisch geactiveerd of geïnstalleerd op Windows Server 2012 R2. Maar SIOS DataKeeper is .NET Framework moet zijn op alle knooppunten die u DataKeeper op installeren. Reden, moet u .NET Framework 3.5 installeren vanaf het gastbesturingssysteem van alle virtuele machines in het cluster.

Er zijn twee manieren om toe te voegen .NET Framework 3.5. Eén manier is het gebruik van de Wizard van de functies en functies toevoegen in Windows, weergegeven in de afbeelding 37.

![Afbeelding 37: .NET Framework 3.5 installeren met behulp van de Wizard van de functies en functies toevoegen][sap-ha-guide-figure-3028]

_**Afbeelding 37:** .NET Framework 3.5 installeren met behulp van de Wizard van de functies en functies toevoegen_

![Afbeelding 38: Installatie voortgangsbalk wanneer u .NET Framework 3.5 installeren met behulp van de Wizard van de functies en functies toevoegen][sap-ha-guide-figure-3029]

_**Afbeelding 38:** Installatie voortgangsbalk bij het installeren van .NET Framework 3.5 tot en met de Wizard van de functies en functies toevoegen_

De tweede optie voor het activeren van de functie .NET Framework 3.5 is via het hulpprogramma voor de opdrachtregel dism.exe. Voor dit type installatie moet u toegang tot de SxS-map op de Windows-installatiemedia. Deze opdracht uitvoert op een opdrachtprompt met verhoogde bevoegdheid:

```
Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
```

#### <a name="dd41d5a2-8083-415b-9878-839652812102"></a>SIOS DataKeeper installeren

SIOS DataKeeper Cluster Edition op elk knooppunt in de cluster geïnstalleerd. Gedeeld met SIOS DataKeeper, voor het maken van virtuele gedeelde opslag, een gesynchroniseerde gespiegelde maken en vervolgens simuleren cluster opslag.

Voordat u de software SIOS hebt geïnstalleerd, kunt u de domeingebruiker **DataKeeperSvc**maken.

> [AZURE.NOTE] De gebruiker **DataKeeperSvc** toevoegen aan de **Lokale** beheerdersgroep op beide clusterknooppunten.

Installeer de software SIOS op beide clusterknooppunten.

![SIOS installer][sap-ha-guide-figure-3030]

![Afbeelding 39: Eerste scherm van de installatie SIOS DataKeeper][sap-ha-guide-figure-3031]

_**Afbeelding 39:** Eerste scherm van de installatie SIOS DataKeeper_

![Afbeelding 40: DataKeeper wordt gemeld dat een service wordt uitgeschakeld][sap-ha-guide-figure-3032]

_**Afbeelding 40:** DataKeeper wordt gemeld dat een service wordt uitgeschakeld_

Selecteer **Ja**in het dialoogvenster wordt weergegeven in de afbeelding 40.

![Afbeelding 41: Selectie van gebruiker voor SIOS DataKeeper][sap-ha-guide-figure-3033]

_**Afbeelding 41:** De selectie van de gebruiker voor SIOS DataKeeper_


Klik op het scherm wordt weergegeven in de afbeelding 41, is het raadzaam dat u **domein of Server-account selecteert**.

![Afbeelding 42: Voer het domein, gebruikersnaam en wachtwoord voor de installatie SIOS DataKeeper][sap-ha-guide-figure-3034]

_**Afbeelding 42:** Voer in het domein, gebruikersnaam en wachtwoord voor de installatie SIOS DataKeeper_

Voer de gebruikersnaam van de domein-account en wachtwoorden opnieuw instellen die u hebt gemaakt voor SIOS DataKeeper.

![Afbeelding 43: Voert u uw licentie SIOS DataKeeper][sap-ha-guide-figure-3035]

_**Afbeelding 43:** Voer uw licentie SIOS DataKeeper_

Installeer de toets licentie voor uw exemplaar SIOS DataKeeper zoals wordt weergegeven in de afbeelding 43. Aan het einde van de installatie, wordt u gevraagd om de virtuele machine opnieuw te starten.

#### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>SIOS DataKeeper instellen

Nadat u op beide knooppunten SIOS DataKeeper hebt geïnstalleerd, moet u de configuratie starten. Het doel van de configuratie is synchroon gegevensreplicatie tussen de extra VHD's die zijn bijgevoegd bij elk van de virtuele machines hebt. Hierna ziet u de stappen voor het configureren van beide knooppunten.

![Afbeelding van 44: SIOS DataKeeper Management en configuratiehulpprogramma][sap-ha-guide-figure-3036]

_**Afbeelding 44:** Hulpmiddel SIOS DataKeeper beheer en de configuratie_

Het hulpprogramma DataKeeper beheer en de configuratie starten en selecteer vervolgens **Connect-Server**. (In de afbeelding 44, deze optie is met een rode cirkel.)

![Afbeelding 45: De naam invoegen of TCP/IP-adres van het eerste knooppunt het beheer en configuratiehulpprogramma moeten verbinding naar en klik in een tweede stap, het tweede knooppunt][sap-ha-guide-figure-3037]

_**Afbeelding 45:** De naam of TCP/IP-adres van het eerste knooppunt dat het hulpmiddel voor beheer en de configuratie moet verbinding naar en klik in een tweede stap, het tweede knooppunt invoegen_

De volgende stap is het opzetten van de taak tussen de twee knooppunten.

![Afbeelding 46: Een taak maken][sap-ha-guide-figure-3038]

_**Afbeelding 46:** Een taak maken_

Een wizard begeleidt u door het proces van het maken van een taak.

![Afbeelding 47: De naam van de taak definiëren][sap-ha-guide-figure-3039]

_**Afbeelding 47:** De naam van de taak definiëren_

![Afbeelding 48: De basisgegevens voor het knooppunt de huidige bronknooppunt moet definiëren][sap-ha-guide-figure-3040]

_**Afbeelding 48:** De basisgegevens voor het knooppunt de huidige bronknooppunt moet definiëren_

In de eerste stap moet u de naam, het TCP/IP-adres en het volume Schijfopruiming van het bronknooppunt definiëren. De tweede stap is het doelknooppunt definiëren. Zoals eerder beschreven, moet u de naam, TCP/IP-adres en schijfvolume van het doelknooppunt definiëren.

![Afbeelding 49: De basisgegevens voor het knooppunt het huidige doelknooppunt moet definiëren][sap-ha-guide-figure-3041]

_**Afbeelding 49:** De basisgegevens voor het knooppunt het huidige doelknooppunt moet definiëren_

Definieer vervolgens de compressie-algoritmen. In ons voorbeeld is het raadzaam dat u in de stream herhaling worden gecomprimeerd. Met name in het opnieuw situaties minder de compressie van de stream herhaling veel tijd voor het opnieuw. Houd er rekening mee dat compressie van processor en RAM resources van een virtuele machine gebruikt. Als het tarief weer dat compressie toeneemt, loopt u het volume van CPU resources die worden gebruikt. U kunt aanpassen om deze instelling later.

Een andere instelling die u nodig hebt om te controleren is of de replicatie synchroon of asynchroon plaatsvindt. *Wanneer u beveiligen met een SAP ASC's / SCS configuraties, moet u synchroon herhaling*.  

![Afbeelding 50: Herhaling details definiëren][sap-ha-guide-figure-3042]

_**Afbeelding 50:** Replicatie details definiëren_

De laatste stap is om te definiëren of het volume dat is gerepliceerd door de taak moet worden weergegeven in een cluster Failoverclustering van Windows Server configuratie als een gedeelde schijf. Voor de configuratie van SAP ASC's / SCS, selecteert u **Ja** zodat het Windows-cluster ziet het gerepliceerde volume als een gedeelde schijf die kan worden gebruikt als een clustervolume.

![Afbeelding 51: Selecteer ** Ja ** voor het instellen van het gerepliceerde volume als een clustervolume][sap-ha-guide-figure-3043]

_**Afbeelding 51:** Selecteer **Ja** om in te stellen van de gerepliceerde volume als een clustervolume_

Nadat het volume is gemaakt, ziet u het hulpprogramma DataKeeper Management en configuratie dat de taak actief is.

![Afbeelding 52: Synchroon spiegelen van DataKeeper voor de SAP ASC's / SCS delen schijf actief is][sap-ha-guide-figure-3044]

_**Afbeelding 52:** Synchroon spiegelen van DataKeeper voor de SAP ASC's / SCS delen schijf is actief_

Failoverclusterbeheer ziet nu de schijf als een schijf DataKeeper, zoals wordt weergegeven in de afbeelding 53.

![Afbeelding 53: Failovercluster Manager ziet u de schijf die DataKeeper gerepliceerd][sap-ha-guide-figure-3045]

_**Afbeelding 53:** Failoverclusterbeheer ziet u de schijf dat DataKeeper gerepliceerd_


## <a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Het systeem SAP NetWeaver installeren

De configuratie DBMS won't worden beschreven omdat instellingen varieert afhankelijk van het DBMS systeem dat u gebruikt. We echter uitgegaan dat hoge beschikbaarheid bezwaren met het DBMS worden besproken met de functies van die de verschillende DBMS leveranciers ondersteuning voor Azure. Bijvoorbeeld altijd in- of spiegelen van SQL Server en Oracle gegevens beveiliging voor Oracle. In het scenario dat we in dit artikel gebruiken toevoegen niet we meer beveiliging aan het DBMS.

Er geen speciale overwegingen wanneer de verschillende services van DBMS interactie met dit type gegroepeerde SAP ASC's / SCS configuratie in Azure wordt aangegeven.


> [AZURE.NOTE] De procedures voor de installatie van SAP NetWeaver ABAP systemen, Java-systemen en ABAP + Java-systemen zijn bijna identiek. Het belangrijkste verschil is dat een SAP-ABAP-systeem eenmalig ASC's heeft. SAP Java is één SCS exemplaar. SAP ABAP + Java is één exemplaar van de ASC's en één SCS exemplaar uitgevoerd in de dezelfde Microsoft cluster overnamegroep. Een installatie verschillen voor elke SAP NetWeaver installatie-stack worden expliciet vermeld. U kunt ervan uitgegaan dat alle andere onderdelen hetzelfde zijn.  

### <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>SAP installeren met een hoge beschikbaarheid ASC's / SCS exemplaar

> [AZURE.IMPORTANT] Zorg ervoor dat u niet het paginabestand op DataKeeper gespiegeld weergegeven volumes plaatsen. DataKeeper biedt geen ondersteuning voor mirrorvolumes. U kunt het paginabestand op de tijdelijke station D van een Azure virtuele machine, laat de standaardinstelling. Als deze niet al aanwezig is, verplaatst u het bestand van de pagina Windows op station D van uw Azure virtuele machine.

#### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Een virtuele host-naam voor de gegroepeerde SAP ASC's / SCS exemplaar te maken

Maak eerst een DNS-vermelding voor de virtuele hostnaam van het exemplaar ASC's / SCS in de Windows-DNS-beheer. Definieer vervolgens, het IP-adres dat is toegewezen aan de virtuele hostnaam.

> [AZURE.NOTE]  
Houd er rekening mee dat het IP-adres die u aan de virtuele hostnaam van het exemplaar ASC's / SCS toewijst moet zijn hetzelfde als het IP-adres die u hebt toegewezen aan Azure taakverdeling (<*beveiligings-id*> - kg - ASC's).  

Het IP-adres van de virtuele SAP ASC's / SCS hostnaam (pr1-ASC's-sap) is hetzelfde als het IP-adres van taakverdeling Azure (pr1-kg-ASC's).

Slechts één SAP failover cluster rol kan worden uitgevoerd in één Windows Server-failovercluster in Azure wordt aangegeven. Dit betekent bijvoorbeeld eenmalig ASC's voor het systeem ABAP en één SCS exemplaar voor de Java-systeem. Voor ABAP + Java worden deze eenmalig ASC's en één SCS exemplaar.

> [AZURE.NOTE] Op dit moment multi-beveiligings-id cluster zoals is beschreven in de hulplijnen SAP-installatie (Zie [SAP-installatiehandleidingen][sap-installation-guides]) werkt niet in Azure wordt aangegeven.

![Afbeelding 54: De DNS-vermelding voor SAP ASC's / SCS cluster virtuele naam en TCP/IP-adres definiëren][sap-ha-guide-figure-3046]

_**Afbeelding 54:** De DNS-vermelding voor SAP ASC's / SCS cluster virtuele naam en TCP/IP-adres definiëren_

Het item is in DNS-beheer, onder het domein, zoals wordt weergegeven in de afbeelding 55.

![55 afbeelding: De nieuwe virtuele naam en het TCP/IP-adres voor de configuratie van het SAP ASC's / SCS cluster][sap-ha-guide-figure-3047]

_**Afbeelding 55:** Nieuwe virtuele naam en het TCP/IP-adres voor SAP ASC's / SCS clusterconfiguratie_

#### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Het eerste knooppunt van SAP installeren

Uitvoeren om het eerste cluster van SAP hebt geïnstalleerd, de eerste cluster knooppunt optie op clusterknooppunt A. Bijvoorbeeld op de host **pr1-ASC's-0** .

Als u wilt behouden de standaard-poorten voor de verdeling van de Azure interne laden, selecteert u:

- **ABAP systeem**: **ASC's** exemplaar getal **00**
- **Java-systeem**: **SCS** exemplaar getal **01**
- **ABAP + JAVA systeem**: **ASC's** exemplaar getal **00** en **SCS** exemplaar getal **01**

Als u exemplaar getallen dan 00 voor het exemplaar ABAP ASCS en 01 voor het exemplaar Java SCS gebruiken wilt, moet u eerst wijzigen van de Azure interne laden verdeling standaard taakverdeling regels zoals is beschreven in [wijzigen de ASC's / SCS standaard-taakverdeling regels voor de verdeling van de Azure interne laden] [sap-ha-handleiding-8,9].

Ga op een paar stappen die niet worden beschreven in de gebruikelijke documentatie voor SAP-installatie.

> [AZURE.NOTE] De SAP-installatie-documentatie wordt beschreven hoe u het eerste ASC's / SCS knooppunt installeert.

#### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Wijzig het SAP-profiel van de ASC's / SCS ik '''powershellnstance

U moet een nieuw profiel parameter toevoegen. De parameter profiel kunt u voorkomen dat verbindingen tussen SAP processen en de server plaatsen af te sluiten wanneer deze actief zijn te lang zijn. Gezegd het probleem scenario in [registervermeldingen op beide knooppunten van het SAP ASC's / SCS exemplaar toevoegen] [sap-ha-handleiding-8.11] in dit artikel. In deze sectie kennis we twee wijzigingen ook met enkele eenvoudige TCP/IP-verbindingsparameters. In een tweede stap moet u de server plaatsen **keep_alive** signaal verzenden zodat de verbindingen niet van de verdeling van de Azure interne belasting inactief drempel raken instellen.

Deze parameter profiel toevoegen aan het SAP ASC's / SCS exemplaar profiel:
```
enque/encni/set_so_keepalive = true
```
In ons voorbeeld is het pad:

`<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

Bijvoorbeeld naar de SAP SCS exemplaar profiel en de bijbehorende pad:

`<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`


#### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Een test-poort toevoegen

De interne taakverdeling test-functionaliteit gebruiken om de configuratie van het hele cluster werken met taakverdeling. De Azure interne meestal taakverdeling worden de binnenkomende werklast gelijkmatig tussen de deelnemende virtuele machines. Echter werkt dit niet in sommige clusterconfiguraties omdat er slechts één exemplaar actief is. Het andere exemplaar is passieve en een van de werklast niet accepteren. Een test-functionaliteit helpt wanneer de Azure interne taakverdeling werk alleen aan een actieve exemplaar wordt toegewezen. Met de functionaliteit voor test, kan de interne taakverdeling welke processen actief zijn en vervolgens doelgerichte alleen het exemplaar met de werklast detecteren.

Controleer eerst de huidige instelling **ProbePort** met deze PowerShell-opdracht. Deze in een van de virtuele machines in de configuratie van het cluster uitvoeren:

```PowerShell
Get-ClusterResource „SAP PR1 IP" | Get-ClusterParameter
```

![Afbeelding 56: De poort cluster configuratie-test is aan 0 al dan niet standaard][sap-ha-guide-figure-3048]

_**Afbeelding 56:** De poort cluster configuratie-test is aan 0 al dan niet standaard_

Definieer vervolgens, een poort test. Het poortnummer van de standaard-test is aan 0. In ons voorbeeld gebruiken we test **62300**poort.

Het poortnummer is gedefinieerd in SAP Azure resourcemanager sjablonen. U kunt het poortnummer in PowerShell toewijzen.

Eerst de host van SAP-virtuele-naam cluster resource **SAP WAC IP**krijgen.

```PowerShell
$var = Get-ClusterResource | Where-Object {  $_.name -eq "SAP PR1 IP"  }
```

Vervolgens stelt u de test-poort op **62300**.

```PowerShell
$var | Set-ClusterParameter -Multiple @{"Address"="10.0.0.43";"ProbePort"=62300;"Subnetmask"="255.255.255.0";"Network"="Cluster Network 1";"OverrideAddressMatch"=1;"EnableDhcp"=0}  
```

De wijzigingen te activeren, stoppen en vervolgens starten de rol van **SAP PR1** cluster.

Nadat u de **SAP PR1** cluster rol online naar voren haalt, controleert u of of **ProbePort** is ingesteld op de nieuwe waarde:

```PowerShell
Get-ClusterResource „SAP PR1 IP" | Get-ClusterParameter
```

![Afbeelding 57: Zoeken de poort cluster nadat u de nieuwe waarde instellen][sap-ha-guide-figure-3049]

_**Afbeelding 57:** De poort cluster onderzoeken nadat u de nieuwe waarde instellen_

De **ProbePort** is ingesteld op **62300**. Nu u toegang hebt tot de bestandsshare ** \\\ascsha-clsap\sapmnt** met andere hosts, zoals **ascsha-dbas**.

### <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Exemplaar van de database installeren

Als u wilt het exemplaar van de database hebt geïnstalleerd, volgt u het proces beschreven in de documentatie van SAP-installatie.

### <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Het tweede clusterknooppunt installeren

Als u wilt het tweede cluster hebt geïnstalleerd, volg de stappen in de installatiehandleiding SAP.

### <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Het type begin van de service-exemplaar van SAP uit Windows wijzigen

Wijzig het type begin van de SAP plaatsen herhaling Server go-Windows-service in **automatisch (vertraagd starten)** op beide clusterknooppunten.

![Afbeelding 58: Het servicetype voor het exemplaar SAP-gebruikers op vertraagde automatisch wijzigen][sap-ha-guide-figure-3050]

_**Afbeelding 58:** De service voor het exemplaar SAP-gebruikers op vertraagde automatisch wijzigen_

### <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>De primaire SAP-toepassingsserver installeren

Het primaire toepassing Server (PAS) exemplaar <*beveiligings-id*> - di - 0 op de virtuele machine die u hebt opgegeven voor het hosten van de PAS installeren. Er zijn geen afhankelijkheden op Azure of DataKeeper manier.

### <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>De extra SAP-toepassingsserver installeren

Een SAP extra toepassing Server (AAS) op alle de virtuele machines die u hebt opgegeven voor het hosten van een SAP-toepassingsserver installeren. Bijvoorbeeld op <*beveiligings-id*> - di - 1 <*beveiligings-id*> - di -<n>.

## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Test de SAP ASC's / SCS exemplaar failover en SIOS herhaling

Het is eenvoudig om te testen en een SAP ASC's / SCS exemplaar failover en SIOS schijf herhaling controleren met behulp van Failoverclusterbeheer en de gebruikersinterface van de DataKeeper SIOS.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>SAP ASC's / SCS exemplaar wordt uitgevoerd op clusterknooppunt A

De groep met **SAP-WAC** cluster wordt uitgevoerd op clusterknooppunt A. Bijvoorbeeld op **ascsha-clna**. De gedeelde schijf S, die deel uitmaakt van het **SAP WAC** clustergroep en het exemplaar ASC's / SCS waarbij gebruik wordt, toewijzen aan clusterknooppunt A.

![Afbeelding 59: Failoverclusterbeheer: het SAP < * beveiligings-id * > clustergroep wordt uitgevoerd op clusterknooppunt A][sap-ha-guide-figure-5000]

_**Afbeelding 59:** Failoverclusterbeheer: het SAP <*beveiligings-id*> clustergroep wordt uitgevoerd op clusterknooppunt A_

Met behulp van de gebruikersinterface van de DataKeeper SIOS, kunt u zien dat de gedeelde schijfgegevens worden synchroon gerepliceerd uit de bron volume station S op clusterknooppunt A naar het doel volume-station S op clusterknooppunt b te drukken. Bijvoorbeeld van **ascsha-clna [10.0.0.41]** naar **ascsha-clnb [10.0.0.42]**.

![Afbeelding 60: In SIOS DataKeeper, repliceren het lokale volume van clusterknooppunt A knooppunt cluster B][sap-ha-guide-figure-5001]

_**Afbeelding 60:** Repliceren in SIOS DataKeeper, het lokale volume van clusterknooppunt A knooppunt cluster B_


### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Failover van knooppunt A naar knooppunt B

U kunt een van deze opties gebruiken om een overname van de groep SAP <*beveiligings-id*> cluster van clusterknooppunt A knooppunt cluster B:

- Failoverclusterbeheer gebruiken  
- Failovercluster PowerShell gebruiken
  ```powershell
  Move-ClusterGroup -Name "SAP WAC"
  ```
- Start opnieuw cluster knooppunt A binnen het besturingssysteem van de Windows gast (Start een automatische overname van de groep SAP <*beveiligings-id*> cluster van knooppunt A naar knooppunt B)  
- Start opnieuw cluster knooppunt A van de Azure-portal (Start een automatische overname van de groep SAP <*beveiligings-id*> cluster van knooppunt A naar knooppunt B)  
- Cluster knooppunt A opnieuw met behulp van Azure PowerShell (Start een automatische overname van de groep SAP <*beveiligings-id*> cluster van knooppunt A naar knooppunt B)

  ```powershell
  Restart-AzureVM -Name ascsha-clna -ServiceName ascsha-cluster
  ```

Na een failover wordt de SAP <*beveiligings-id*> clustergroep uitgevoerd op clusterknooppunt b te drukken. Bijvoorbeeld op **ascsha-clnb**.

![Afbeelding 61: In Failoverclusterbeheer, het SAP < * beveiligings-id * > clustergroep wordt uitgevoerd op clusterknooppunt B][sap-ha-guide-figure-5002]

_**Afbeelding 61**: In Failoverclusterbeheer de SAP <*beveiligings-id*> clustergroep wordt uitgevoerd op clusterknooppunt B_

Nu de gedeelde schijf is gekoppeld op clusterknooppunt wordt B. SIOS DataKeeper gegevens uit de bron volume station S op clusterknooppunt B gerepliceerd naar doel volume station S op clusterknooppunt A. Bijvoorbeeld van **ascsha-clnb [10.0.0.42]** naar **ascsha-clna [10.0.0.41]**.


![Afbeelding 62: SIOS DataKeeper wordt overgenomen door het lokale volume van clusterknooppunt B naar cluster knooppunt A][sap-ha-guide-figure-5003]

_**Afbeelding 62:** SIOS DataKeeper wordt overgenomen door het lokale volume van clusterknooppunt B naar cluster knooppunt A_
