<properties
   pageTitle="Belangrijke releaseopmerkingen kluis .NET 2.x API | Microsoft Azure"
   description=".NET-ontwikkelaars wordt deze API naar code gebruikt voor Azure-toets kluis"
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="CSharp"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/07/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure belangrijke kluis .NET 2.0 - releaseopmerkingen en Migratiehandleiding

De volgende instructies en notities zijn voor ontwikkelaars die werken met de Azure-toets kluis .NET / C#-bibliotheek. In de veranderd ten opzichte van versie 1.0 naar versie 2.0, zijn een aantal updates doorgevoerd die is vereist migratie werk in de code in voordat u kunt profiteren van de functionele verbeteringen en aanbevelen toevoegingen zoals toets kluis certificaten ondersteuning.

Toets kluis certificaten ondersteuning biedt voor beheer van uw x509 certificaten en de volgende problemen:  

-   Hiermee kunt u de eigenaar van een certificaat om een certificaat via een proces voor het maken van een sleutel kluis of via het importeren van een bestaand certificaat te maken. Dit geldt ook voor beide zelf-ondertekend en certificeringsinstantie certificaten gegenereerd.

- Kunt u de eigenaar van een sleutel kluis certificaat willen implementeren veilige opslag en het beheer van X509 certificaten zonder interactie met persoonlijke sleutel materiaal.  

-   Hiermee kunt u de eigenaar van een certificaat een beleid die wordt u omgeleid toets kluis zodat voor het beheren van de levenscyclus van een certificaat maken.  

-   Eigenaren van het certificaat te leveren contactgegevens voor melding over de levenscyclus gebeurtenissen van verval en verlenging van certificaat kunt.  

-   Automatische verlenging ondersteunt met geselecteerde uitgevers - toets kluis partner X509 providers-certificaat / certificeringsinstanties.
    - Opmerking: niet-samen providers/authorities ook zijn toegestaan, maar geen ondersteuning voor de functie voor automatisch verlengen.


## <a name="net-support"></a>.NET-ondersteuning
- **.NET 4.0** wordt niet ondersteund door de 2.0-versie van Azure-toets kluis .NET / C#-bibliotheek
- **.NET Core** wordt ondersteund door de 2.0-versie van Azure-toets kluis .NET / C#-bibliotheek

## <a name="namespaces"></a>Naamruimten
- De naamruimte voor **modellen** is gewijzigd van **Microsoft.Azure.KeyVault** in **Microsoft.Azure.KeyVault.Models**.
- De naamruimte **Microsoft.Azure.KeyVault.Internal** wordt verwijderd.
- De naamruimte van Azure SDK afhankelijkheden uit **Hyak.Common** en **Hyak.Common.Internals** worden gewijzigd in **Microsoft.Rest** en **Microsoft.Rest.Serialization**


## <a name="type-changes"></a>Type wijzigingen
- *Geheim* gewijzigd in *SecretBundle*
- *Woordenlijst* gewijzigd in *IDictionary*
- *Lijst<T>, [] tekenreeks* gewijzigd in *IList<T> *
- *NextList* gewijzigd in *NextPageLink*


## <a name="return-types"></a>Resultaattypen
- **KeyList** en **SecretList** retourneert *IPage<T> * in plaats van *ListKeysResponseMessage*
- De gegenereerde **BackupKeyAsync** retourneert *BackupKeyResult* die *waarde* (back-ups van blob) bevat. Voordat u de methode is tekstterugloop en alleen de waarde te retourneren.

## <a name="exceptions"></a>Uitzonderingen
- *KeyVaultClientException* wordt gewijzigd in *KeyVaultErrorException*
- De servicefout is gewijzigd van *uitzondering. Fout* *uitzondering. Body.Error.Message*.
- Extra info verwijderd uit het foutbericht wordt weergegeven voor **[JsonExtensionData]**.

## <a name="constructors"></a>Constructors
- In plaats van een *HttpClient* als een argument constructor accepteert, accepteert de constructor alleen *HttpClientHandler* of *DelegatingHandler []*.



## <a name="downloaded-packages"></a>Gedownloade pakketten  
Wanneer een client een afhankelijkheid van toets kluis verwerken van is zijn de volgende gedownload
#### <a name="previous-package-list"></a>Vorige pakketlijst
- versie inpakken id="Hyak.Common" = "1.0.2" targetFramework = "net45"
- versie inpakken id="Microsoft.Azure.Common" = "2.0.4" targetFramework = "net45"
- versie inpakken id="Microsoft.Azure.Common.Dependencies" = "1.0.0" targetFramework = "net45"
- versie inpakken id="Microsoft.Azure.KeyVault" = "1.0.0" targetFramework = "net45"
- versie inpakken id="Microsoft.Bcl" = "1.1.9" targetFramework = "net45"
- versie inpakken id="Microsoft.Bcl.Async" = "1.0.168" targetFramework = "net45"
- versie inpakken id="Microsoft.Bcl.Build" = "1.0.14" targetFramework = "net45"
- versie inpakken id="Microsoft.Net.Http" = "2.2.22" targetFramework = "net45"

#### <a name="current-package-list"></a>Huidige pakketlijst
- versie inpakken id="Microsoft.Azure.KeyVault" = "2.0.0-preview" targetFramework = "net45"
- versie inpakken id="Microsoft.Rest.ClientRuntime" = "2.2.0" targetFramework = "net45"
- versie inpakken id="Microsoft.Rest.ClientRuntime.Azure" = "3.2.0" targetFramework = "net45"


## <a name="class-changes"></a>Klasse wijzigingen

- **UnixEpoch** klasse is verwijderd
- **Base64UrlConverter** klasse is gewijzigd in **Base64UrlJsonConverter**

## <a name="other-changes"></a>Andere wijzigingen

- Ondersteuning voor de configuratie van KV bewerking opnieuw beleid op tijdelijke fouten is toegevoegd aan deze versie van de API.



## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet
- Voor de bewerkingen die een *kluis*geretourneerd, is het retourtype een klasse die deel uitmaakt van een eigenschap kluis. Het retourtype is nu *kluis*.
- *PermissionsToKeys* en *PermissionsToSecrets* zijn nu *Permissions.Keys* en *Permissions.Secrets*
- Enkele van de afzender typen wijzigingen toepassen op het ook control-vlak.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet
- Het pakket werkt niet goed **Microsoft.Azure.KeyVault.Extensions** en **Microsoft.Azure.KeyVault.Cryptography** voor de cryptografische bewerkingen.
