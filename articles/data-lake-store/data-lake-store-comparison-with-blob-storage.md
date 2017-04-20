<properties
   pageTitle="Azure Lake gegevensopslag vergelijking met Azure opslag Blob | Microsoft Azure"
   description="Azure Lake gegevensopslag vergelijking met Azure opslag Blob"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Vergelijking van Azure Lake gegevensopslag en Azure-blobopslag

De tabel in dit artikel worden de verschillen tussen Azure Lake gegevensopslag en Azure-blobopslag langs enkele belangrijke aspecten van groot gegevensverwerking. Azure-blobopslag is een algemene scalable object store die is bedoeld voor een groot aantal opslag scenario's. Azure Lake gegevensopslag is een hyper-schaal-opslagplaats die is geoptimaliseerd voor groot gegevens analytics werkbelasting.

|    | Azure Lake gegevensopslag  | Azure-blobopslag  |
|----|-----------------------|--------------------|
| Doel  | Geoptimaliseerde opslag voor groot gegevens analytics werkbelasting                                                                          | Algemene object opslaan voor een groot aantal opslag scenario 's                                                                                |
| Zaken gebruiken                                   | Batch, interactieve, analyses en machine learning-gegevens, zoals streaming logboekbestanden, IoT gegevens, klikt u op streams, grote gegevenssets | Een willekeurig type tekst of binaire gegevens, zoals toepassing een back-end, back-upgegevens, Mediaopslag voor streaming en algemene doel-gegevens |
| Belangrijke concepten                                | Gegevensopslag Lake account bevat mappen, daarin op zijn beurt gegevens die zijn opgeslagen als bestanden | Opslag-account heeft containers, die op zijn beurt gegevens in de vorm van BLOB's bevat |
| Structuur | Hiërarchische bestandssysteem                                                                                                    | Object store met plat naamruimte  |
| API   | REST API via HTTPS | REST API via HTTP/HTTPS                                                                                                                            |
| Aan de clientzijde-API                             | [WebHDFS-compatibele REST API](https://msdn.microsoft.com/library/azure/mt693424.aspx)                                                                                                 | [Azure-blobopslag REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)                                                                                                                         |
| Hadoop-bestand systeem-Client                   | Ja                                                                                                                         | Ja                                                                                                                                                 |
| Gegevensbewerkingen - verificatie            | Op basis van [Azure Active Directory-identiteiten](../active-directory/active-directory-authentication-scenarios.md) | Op basis van gedeelde geheimen - [Account toegangstoetsen](../storage/storage-create-storage-account.md#manage-your-storage-account) en [Gedeelde toegangstoetsen handtekening](../storage/storage-dotnet-shared-access-signature-part-1.md).                                                                       |
| Gegevensbewerkingen - verificatieprotocol     | OAuth 2.0. Oproepen moeten bevatten een geldige JWT (JSON Web Token) uitgegeven door Azure Active Directory                     | Bericht hash gebaseerde verificatie-Code (HMAC). Oproepen moeten een hash SHA-256 Base64-codering boven een deel van de HTTP-aanvraag bevatten. |
| Gegevensbewerkingen - autorisatie               | POSIX toegangsbeheerlijsten (ACL's).  ACL's op basis van Azure Active Directory identiteiten kunnen worden ingesteld als bestands- en niveau. | Voor account niveau autorisatie – gebruiken [Toegangstoetsen Account](../storage/storage-create-storage-account.md#manage-your-storage-account)<br>Voor het account, container of blob autorisatie - gebruiken [Toegangstoetsen handtekening gedeeld](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| Gegevensbewerkingen - controle                    | Beschikbaar. Zie [hier](data-lake-store-diagnostic-logs.md) voor informatie.                                                                                                                   | Beschikbaar                                                                                                                                           |
| Versleuteling gegevens in rust                     | Transparante, serverzijde (binnenkort)<ul><li>Met toetsen service worden beheerd</li><li>Met toetsen klant worden beheerd in Azure KeyVault</li></ul>| <ul><li>Transparante, serverzijde</li> <ul><li>Met toetsen service worden beheerd</li><li>Met toetsen klant worden beheerd in Azure KeyVault (binnenkort)</li></ul><li>Aan de clientzijde versleuteling</li></ul> |
| Beheertaken uit te voeren (bijvoorbeeld Account maken) | [Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) Azure (RBAC) gekregen om uw account te beheren                                                                       | [Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) Azure (RBAC) gekregen om uw account te beheren                                                                                               |
| Ontwikkelaars SDK 's                              | .NET, Java, Node.js                                                                                                         | .NET, Java, Python, Node.js, C++, Ruby                                                                                                              |
| Analytics werkbelasting prestaties              | Geoptimaliseerde prestaties voor parallelle analytics werkbelasting. Hoge doorvoer en IO's / s.                                           | Niet geoptimaliseerd voor analytics werkbelasting                                                                                                               |
| Maximale grootte                                 | Geen beperkingen ten aanzien van het formaat van de account, bestandsgrootten of aantal bestanden                                                                   | Specifieke limieten beschreven [hier](../azure-subscription-service-limits.md#storage-limits)                                                                                                                     |
| Geografische redundantie                              | Lokaal-redundante (meerdere exemplaren van gegevens in één Azure regio)                                                             | Lokaal overtollige (LRS), globaal overtollige (GRS), leestoegang globaal overtollige (AB-GRS). Zie [hier](../storage/storage-redundancy.md) voor meer informatie |
| Servicestatus                               | Openbare Preview                                                                                                              | Algemeen beschikbaar                                                                                                                                 |
| Regionale beschikbaarheid  | Zie [hier](https://azure.microsoft.com/regions/#services)| Zie [hier](https://azure.microsoft.com/regions/#services) |
| Prijs                                       |     Zie [prijzen](https://azure.microsoft.com/pricing/details/data-lake-store/)| Zie [prijzen](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure Lake gegevensopslag](data-lake-store-overview.md)
- [Aan de slag met Lake gegevensopslag](data-lake-store-get-started-portal.md)



