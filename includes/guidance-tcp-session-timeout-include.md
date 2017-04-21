##<a name="tcp-settings-for-azure-vms"></a>TCP-instellingen voor Azure VMs

Azure VMs communiceren met de openbare Internet met behulp van [NAT] [ nat] (NAT). NAT-apparaten toewijzen een openbare IP-adres en poort aan een VM Azure, krijgt deze VM tot stand brengen van een socket voor communicatie met andere apparaten. Als pakketten die doorloopt tot en met die socket na een bepaalde tijd stoppen, het NAT-apparaat beëindigd de toewijzing en de socket is gratis door andere VMs moet worden gebruikt.

Dit is een algemene NAT-gedrag, waardoor communicatieproblemen op op basis van TCP-toepassingen dat een socket worden bijgehouden na een periode time-out verwacht. Er zijn twee niet-actieve time-outinstellingen aandachtspunten voor sessies in een *verbinding tot stand gebracht* staat:

- **binnenkomende** tot en met de [Azure belasting voor documentconversies][azure-lb-timeout]. Deze time-out standaard 4 minuten en omhoog kan worden aangepast aan 30 minuten.
- **uitgaande** met [SNAT] [ snat] (bron Translation). Deze time-out is ingesteld op 4 minuten en kan niet worden gecorrigeerd.

Om ervoor te zorgen verbindingen niet buiten de kolomgrenzen time-out gaan verloren, moet u controleren of uw toepassing behouden de sessie leven, of u kunt het onderliggende besturingssysteem hiervoor configureren. De instellingen moet worden gebruikt zijn verschillend voor Linux en Windows-systemen, zoals hieronder wordt weergegeven.

Voor [Linux][linux], moet u de onderstaande kernelvariabelen.
NET.IPv4.tcp_keepalive_time = 120 net.ipv4.tcp_keepalive_intvl = 30 net.ipv4.tcp_keepalive_probes = 8
 
Voor [Windows][windows], moet u de onderstaande registerwaarden.
KeepAliveInterval = 30 KeepAliveTime = 120 TcpMaxDataRetransmissions = 8


De bovenstaande instellingen te garanderen voor een leven-pakket is verzonden, na 2 minuten (120 seconden) van niet-actieve tijd, en klik vervolgens verzonden elke 30 seconden. En als 8 van deze pakketten mislukt, de sessie wordt verwijderd.

<!-- links -->
[nat]: http://computer.howstuffworks.com/nat.htm
[snat]: ../load-balancer/load-balancer-overview.md/#source-nat
[linux]: http://tldp.org/HOWTO/TCP-Keepalive-HOWTO/usingkeepalive.html
[windows]: http://blogs.technet.com/b/nettracer/archive/2010/06/03/things-that-you-may-want-to-know-about-tcp-keepalives.aspx
[azure-lb-timeout]: ../load-balancer/load-balancer-tcp-idle-timeout.md