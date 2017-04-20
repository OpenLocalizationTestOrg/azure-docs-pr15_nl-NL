<properties
   pageTitle="SSMS ondersteuning voor Azure AD MFA met SQL-Database en SQL Data Warehouse | Microsoft Azure"
   description="Gebruik meerdere meegenomen verificatie met SSMS voor SQL-Database en SQL datawarehouse."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>SSMS ondersteuning voor Azure AD MFA met SQL-Database en SQL Data Warehouse

Azure SQL-Database en Azure SQL Data Warehouse nu ondersteuning voor verbindingen van SQL Server Management Studio (SSMS) met *Active Directory universele verificatie*. Active Directory-universele verificatie is een interactieve werkstroom die ondersteuning biedt voor *Azure meervoudige verificatie* (MFA). Azure MFA helpt beschermen toegang tot gegevens en toepassingen tijdens de vergadering gebruiker aanvraag voor een eenvoudig proces aanmelden. Sterke verificatie met een aantal eenvoudige controleopties levert, telefoongesprek, SMS-bericht, smartcards met vastmaken of vanuit de melding van de mobiele app, zodat gebruikers kunnen kiezen hoe ze liever. Zie voor een beschrijving van meervoudige verificatie [Meervoudige verificatie](../multi-factor-authentication/multi-factor-authentication.md).

SSMS ondersteunt nu:

- Interactieve MFA met Azure AD met de mogelijkheden voor pop-upvenster vak validatie.
- Niet-interactieve Active Directory-wachtwoord en geïntegreerde verificatie van Active Directory methoden die kunnen worden gebruikt in veel verschillende toepassingen (ADO.NET, JDBC, ODBC, enzovoort). Pop-dialoogvensters resultaat nooit deze twee methoden.

Wanneer u het gebruikersaccount is geconfigureerd voor MFA moet de werkstroom interactieve verificatie gebruikers niets via pop-dialoogvensters, smartcard gebruiken, enzovoort. Wanneer het account is geconfigureerd voor MFA, moet de gebruiker Azure universele verificatie om verbinding te selecteren. Als het gebruikersaccount MFA niet nodig, kan de gebruiker nog steeds de andere twee Azure Active Directory-verificatie-opties gebruiken.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Universele verificatie beperkingen voor SQL-Database en SQL Data Warehouse

- SSMS is de enige hulpprogramma momenteel ingeschakeld voor MFA via Active Directory-universele verificatie.
- Alleen een enkele Azure Active Directory-account kunt aanmelden voor een exemplaar van SSMS met universele verificatie. Als u zich hebt aangemeld als een andere Azure AD-account, wilt moet u een ander exemplaar van SSMS gebruiken. (Deze beperking is beperkt tot de verificatie van Active Directory-universele; u kunt aanmelden bij verschillende servers met Active Directory-wachtwoordverificatie geïntegreerde verificatie van Active Directory of SQL Server-verificatie).
- SSMS ondersteunt Active Directory-universele verificatie voor Object Explorer, Query-Editor en Query Store visualisatie.
- Ondersteuning voor universele verificatie DacFx noch de ontwerpfunctie voor Schema's.
- MSA accounts worden niet ondersteund voor universele verificatie van Active Directory.
- Active Directory-universele verificatie wordt niet ondersteund in SSMS voor gebruikers die zich in de huidige Active Directory van andere Active Directory van Azure's zijn geïmporteerd. Deze gebruikers worden niet ondersteund, omdat het vereist een tenant-ID voor het valideren van de accounts en er is geen manier die.
- Er zijn geen extra softwarevereisten voor universele verificatie van Active Directory, behalve dat u een ondersteunde versie van SSMS moet gebruiken.

## <a name="configuration-steps"></a>Volgende configuratiestappen uit

Meervoudige verificatie implementeren, moet vier eenvoudige stappen.

1. **Een Azure Active Directory configureren** : Zie voor meer informatie, [integratie van uw on-premises implementatie identiteiten met Azure Active Directory](../active-directory/active-directory-aadconnect.md), [de naam van uw eigen domein naar Azure AD toevoegen](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure nu ondersteunt Federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [beheer van het telefoonboek van uw Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx)en [Manage Azure AD via Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).

2. **MFA configureren** – Zie [Azure meervoudige verificatie configureren](../multi-factor-authentication/multi-factor-authentication-whats-next.md)voor stapsgewijze instructies. 

3. **SQL-Database configureren of SQL Data Warehouse voor Azure AD-verificatie** – voor stapsgewijze instructies Zie [verbinding maken met SQL-Database of SQL Data Warehouse met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md).

4. **Download SSMS** – op de clientcomputer, downloadt u de meest recente SSMS (ten minste augustus 2016), van [Downloaden SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Verbinding maken met behulp van universele verificatie met SSMS

De volgende stappen hoe verbinding maken met SQL-Database of SQL Data Warehouse met behulp van de meest recente SSMS.

1. Selecteer voor gebruik van universele authenticatie, in het dialoogvenster **verbinding maken met de Server** , **Active Directory-universele verificatie**.
![1mfa universal verbinden][1]

2. Zoals u gewend bent voor SQL-Database en SQL Data Warehouse moet u klikt u op **Opties** en het opgeven van de database in het dialoogvenster **Opties** . Klik op **verbinding maken**.
3. Wanneer het dialoogvenster **aanmelden bij uw account** wordt weergegeven, geef het account en wachtwoord van uw identiteit Azure Active Directory.
![2mfa-aanmelden][2]

    > [AZURE.NOTE] Voor universele verificatie met een account die niet voor MFA vereist is u verbinding maakt op dit punt. Voor gebruikers vereisen van MFA, gaat u verder met de volgende stappen uit.
 
4. Twee MFA setup-dialoogvensters kunnen worden weergegeven. Dit één keer bewerking is afhankelijk van de beheerder van het MFA instellen, en daarom mogelijk optioneel. Voor een domein MFA ingeschakeld deze stap is het soms vooraf gedefinieerde (bijvoorbeeld, het domein moet gebruikers met een smartcard en pincode).  
![3mfa-instelling][3]

5. De tweede mogelijke eenmalig dialoogvenster kunt u de details van uw verificatiemethode selecteren. De mogelijke opties worden geconfigureerd door uw beheerder.
![4mfa-controleren-1][4]
 
6. De Azure Active Directory stuurt waarin informatie aan u. Wanneer u de verificatiecode ontvangt, voert u het in het vak **Voer de verificatiecode** en klikt u op **aanmelden**.
![5mfa-verifiëren-2][5]

Als de verificatie is voltooid, verbindt SSMS normaal ervan uitgaande dat geldige referenties en firewalltoegang.

##<a name="next-steps"></a>Volgende stappen  

Anderen toegang verlenen tot uw database: [SQL Database-verificatie en autorisatie: toegang verlenen](sql-database-manage-logins.md)  
Zorg ervoor dat anderen verbinding kunnen maken via de firewall: [een regel voor het niveau van de server firewall van Azure SQL-Database met behulp van de Azure portal configureren](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

