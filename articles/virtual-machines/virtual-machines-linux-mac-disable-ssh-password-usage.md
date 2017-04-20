<properties
    pageTitle="Wachtwoorden voor SSH op uw VM Linux uitschakelen door te configureren SSHD | Microsoft Azure"
    description="Uw VM Linux op Azure beveiligen door het wachtwoord aanmeldingen uitschakelen voor SSH."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="v-livech"/>

# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Wachtwoorden voor SSH op uw VM Linux uitschakelen door te SSHD configureren

In dit artikel bevat informatie over hoe u de beveiliging van de aanmelding van uw VM Linux vergrendelen.  Zodra de SSH poort 22 wordt geopend aan de wereld bots start probeert te raden wachtwoorden aanmelden.  Wat wij doen in dit artikel is het wachtwoord aanmeldingen via SSH uitschakelen.  Door te volledig verwijderen de mogelijkheid om te gebruiken van wachtwoorden beveiligen we de VM Linux van dit type brutekrachtaanval.  De toegevoegde bonus is dat we Linux SSHD als u wilt dat alleen aanmeldingen via SSH openbare en persoonlijke sleutels, veel de meest veilige manier om te Meld u aan bij Linux configureert.  Mogelijke combinaties van deze vereist te raden de persoonlijke sleutel is zeer grote en kunnen daarom ontmoedigt bots uit even proberen alles om te proberen te felle SSH toetsen.


## <a name="goals"></a>Doelstellingen

- Configureer SSHD om te weigeren:
  - Wachtwoord aanmeldingen
  - Gebruikersaanmelding hoofdsite
  - Vraag-antwoord-verificatie
- Configureer SSHD toe te staan dat:
  - alleen SSH belangrijke aanmeldingen
- Start opnieuw SSHD terwijl u nog steeds bent aangemeld
- Test de configuratie van de nieuwe SSHD

## <a name="introduction"></a>Inleiding

[SSH gedefinieerd](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD is de SSH Server die wordt uitgevoerd op de VM Linux.  SSH is een client die wordt uitgevoerd vanuit een shell op uw werkstation MacBook of Linux.  SSH is ook het protocol dat is gebruikt om te beveiligen en de communicatie tussen uw werkstation en de VM Linux versleutelen.

Eén Meld u aan bij uw VM Linux openen voor in dit artikel is het belangrijk om te houden voor de hele doorlopen.  Daarom wordt we twee overstapstations en SSH voor Linux VM uit beide geopend.  De eerste terminal gebruiken we de wijzigingen aanbrengen in het configuratiebestand SSHDs en opnieuw starten van de service SSHD.  We gebruiken de tweede terminal test deze wijzigingen zodra de service opnieuw is opgestart.  Omdat we SSH wachtwoorden wordt uitgeschakeld en wordt te vertrouwen strikt op SSH toetsen, als uw sleutels SSH niet juist zijn en u de verbinding met de VM sluiten, de VM permanent vergrendeld en niemand kunnen Meld u aan bij het vereisen van deze worden verwijderd en opnieuw ingesteld.

## <a name="prerequisites"></a>Vereisten voor

- [SSH toetsen op Linux en Mac maken voor Linux VMs in Azure wordt aangegeven](virtual-machines-linux-mac-create-ssh-keys.md)
- Azure-account
  - [gratis proefabonnement aanmelden](https://azure.microsoft.com/pricing/free-trial/)
  - [Azure-portal](http://portal.azure.com)
- Linux VM waarop azure
- SSH openbare en persoonlijke sleutels paar in`~/.ssh/`
- Openbare sleutel SSH in `~/.ssh/authorized_keys` op de VM Linux
- Sudo-rechten op de VM
- Poort 22 openen

## <a name="quick-commands"></a>Snelle opdrachten

_Doorgewinterde Linux Admins die de versie TLDR Begin hier.  Voor alle andere wil wordt dit gedeelte overslaan door de gedetailleerde toelichting op en doorlopen._

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no

# On the Debian Family
username@macbook$ sudo service ssh restart

# On the RedHat Family
username@macbook$ sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Gedetailleerde doorlopen

Meld u aan de VM Linux op terminal 1 (t 1).  Meld u aan de VM Linux op terminal 2 (t 2).

We gaan het configuratiebestand SSHD bewerken op t 2.  

```
username@macbook$ sudo vim /etc/ssh/sshd_config
```

Hier wordt we alleen de instellingen om te schakelen van wachtwoorden en inschakelen van de belangrijkste aanmeldingen SSH bewerken.  Zijn er veel instellingen in het bestand dat u moet onderzoeken en wijzigen zodat Linux & SSH zo veilig als u nodig hebt.

#### <a name="disable-password-logins"></a>Wachtwoord aanmeldingen uitschakelen

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Verificatie met openbare sleutel inschakelen

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Hoofdmap aanmelding uitschakelen

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Vraag-antwoord-verificatie uitschakelen

```
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Start opnieuw SSHD

Controleer of dat u nog steeds worden geregistreerd in van de shell t 1.  Dit is kritieke zodat u kan niet worden vergrendeld afmelden bij uw VM als uw SSH sleutels onjuist zijn aangezien wachtwoorden nu zijn uitgeschakeld.  Als een instelling kloppen niet op uw Linux VM kunt u t 1 sshd_config corrigeren terwijl u nog steeds worden geregistreerd in en SSH de verbinding leven tijdens de SSHD-service dat, opnieuw starten.

Uit t 2 uitvoeren:

##### <a name="on-the-debian-family"></a>Klik op het Debian gezin

```
username@macbook$ sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>Klik op het gezin RedHat

```
username@macbook$ sudo service sshd restart
```

Wachtwoorden zijn nu uitgeschakeld op uw VM deze beveiligen tegen felle wachtwoord login pogingen.  Toegestaan zult u sneller aanmelden en veel beter kunt beveiligen met alleen SSH toetsen.
