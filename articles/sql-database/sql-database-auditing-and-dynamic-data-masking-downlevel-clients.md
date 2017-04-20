<properties
    pageTitle="Ondersteuning voor SQL-Database downlevel-clients en IP-eindpunt gewijzigd voor controle | Microsoft Azure"
    description="Meer informatie over ondersteuning voor SQL-Database voor downlevel clients en IP-eindpunt wijzigingen voor controle."
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/10/2016"
    ms.author="ronitr"/>

# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-auditing"></a>SQL-Database - ondersteuning voor Downlevel-clients en IP-eindpunt wijzigingen voor controle


[Controle](sql-database-auditing-get-started.md) werkt automatisch met SQL-clients die ondersteuning biedt voor omleiding TDS.


##<a id="subheading-1"></a>Ondersteuning voor downlevel-clients

Een client die wordt geïmplementeerd TDS 7.4 moet ook ondersteuning biedt voor omleiding. Uitzonderingen op dit opnemen JDBC 4.0 waarin de functie omleiding wordt niet volledig ondersteund en Tedious voor Node.JS in welke omleiding is niet geïmplementeerd.

Voor 'Downlevel-clients', moet dat wil zeggen welke ondersteuning TDS versie 7.3 en onder - de FQDN-naam van de server in de verbinding tekenreeks worden gewijzigd:

Oorspronkelijke FQDN-naam server in de verbindingstekenreeks: <*servernaam*>. database.windows.net

Gewijzigde FQDN-naam server in de verbindingstekenreeks: <*servernaam*> .database. **Secure**. windows.net

Een gedeeltelijke lijst met "Downlevel-clients" bevat:

- .NET 4.0 en lager,
- ODBC-10.0 en lager.
- JDBC (terwijl JDBC TDS 7.4 ondersteunt, de TDS omleiding functie wordt niet volledig ondersteund)
- Vervelende (voor Node.JS)

**Opmerking:** De bovenstaande server FULLY wijziging mogelijk nuttige ook voor het toepassen van een beleid controle van SQL Server op zonder een voor een configuratiestap in elke database (tijdelijke risicobeperking).

##<a id="subheading-2"></a>IP-eindpunt wijzigingen bij het inschakelen van controle

Houd er rekening mee dat wanneer u een controle inschakelt, het IP-eindpunt van uw database wordt gewijzigd. Als er strikte firewallinstellingen, werk deze firewallinstellingen dienovereenkomstig gewijzigd.

Het nieuwe database IP-eindpunt, is afhankelijk van de databaseregio:

| Databaseregio | Mogelijke IP-eindpunten |
|----------|---------------|
| China Noord  | 139.217.29.176, 139.217.28.254 |
| China Oost  | 42.159.245.65, 42.159.246.245 |
| Australië Oost  | 104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Australië Zuidoost | 191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Brazilië Zuid  | 104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| Central US  | 104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Oost-Azië   | 23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| Oost-Amerikaanse 2 | 104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Oost-VS   | 23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| Centraal India  | 104.211.98.219, 104.211.103.71 |
| Zuid-India   | 104.211.227.102, 104.211.225.157 |
| West India  | 104.211.161.152, 104.211.162.21 |
| Japan Oost   | 104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Japan West    | 104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Centraal Noord-Amerikaanse  | 191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Noord-Europa  | 104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Zuid-Central US  | 191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Zuidoost-Azië  | 104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| West Europa  | 104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| West VS  | 191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Canada centraal  | 13.88.248.106 |
| Canada Oost  |  40.86.227.82 |
