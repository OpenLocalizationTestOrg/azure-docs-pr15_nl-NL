Uw IaaS virtuele machines (VMs) en PaaS rol exemplaren in een virtueel netwerk kunt u automatisch een persoonlijk IP-adres van een bereik dat u hebt opgegeven, op basis van het subnet ze zijn verbonden met ontvangen. Dit adres wordt bewaard door de VMs en rol exemplaren, totdat ze buiten gebruik gesteld. U schaft een exemplaar van de rol van of VM door het stoppen van PowerShell, de CLI Azure of de Azure-portal. In dat geval zodra het VM of rol exemplaar wordt gestart nogmaals ontvangt deze een beschikbaar IP-adres van de Azure-infrastructuur die inhoud mogelijk niet hetzelfde voorheen. Als u de rol of VM exemplaar van het gastbesturingssysteem afsluit, worden het IP-adres had behouden.  

In sommige gevallen wilt u een rol of VM exemplaar heeft een statische IP-adres, bijvoorbeeld als uw VM dient te DNS uitvoert of wordt een domeincontroller. U kunt dit doen door in te stellen van een statische privé IP-adres.