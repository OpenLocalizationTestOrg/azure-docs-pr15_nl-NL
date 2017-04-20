Als u een Azure-abonnement gebruikt waar u niet de beheerder/eigenaar bent, zoals een bedrijf eigendom van abonnement, moet u het volgende voordat u de stappen in dit document controleren:

* Uw Azure aanmelding moet ten minste beschikken __Inzender__ toegang tot de Azure resourcegroep die u gebruikt bij het maken van HDInsight (en andere resources Azure.)

* Iemand met bij minimaal __Inzender__ toegang tot het Azure abonnement moet hebben eerder hebt geregistreerd de provider voor de resource die u gebruikt. Registratie van provider gebeurt wanneer een gebruiker met Inzender toegang tot het abonnement maakt een resource voor het eerst het abonnement. Dit kan ook worden bereikt zonder te maken van een resource door [een provider met REST registreren](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Zie de volgende documenten voor meer informatie over het werken met toegangsbeheer:

* [Aan de slag met toegangsbeheer in de portal van Azure](../articles/active-directory/role-based-access-control-what-is.md)
* [Roltoewijzingen voor het beheren van toegang tot uw Azure abonnement resources gebruiken](../articles/active-directory/role-based-access-control-configure.md)
