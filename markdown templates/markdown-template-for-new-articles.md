<properties
   pageTitle="De titel van de pagina die wordt weergegeven in de browser tabblad en zoekresultaten"
   description="Beschrijving van het artikel die wordt weergegeven op te lossen, pagina's en klik in de meeste zoekresultaten"
   services="service-name"
   documentationCenter="dev-center-name"
   authors="GitHub-alias-of-only-one-author"
   manager="manager-alias"
   editor=""/>

<tags
   ms.service="required"
   ms.devlang="may be required"
   ms.topic="article"
   ms.tgt_pltfrm="may be required"
   ms.workload="required"
   ms.date="mm/dd/yyyy"
   ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

# <a name="markdown-template-article-heading-1-or-h1-heading-at-the-top-of-the-article"></a>Korting-sjabloon (artikel Kop1 of H1): kop boven aan het artikel

Als u wilt kopiëren de korting op basis van deze sjabloon, het artikel kopiëren in uw lokale cessies‑retrocessies, of klik op de knop onbewerkte in de GitHub UI en kopieer de korting.

  ![Alternatieve tekst; laat niet leeg. Afbeelding wordt beschreven.][8]

Inleidende alinea: Lorem dolor amet, adipiscing elit. Phasellus interdum nulla risus, lacinia Portal nisl imperdiet sed. Mauris dolor mauris, tempus sed lacinia nec, euismod niet felis. Nunc semper Portal ultrices. Maecenas neque nulla, condimentum vitae ipsum zitten amet, dignissim aliquet nisi.

## <a name="heading-2-h2"></a>Kop 2 (H2)

Aenean gaan zitten amet leo nec purus placerat fermentum ac gravida odio. Aenean tellus lectus, faucibus in rhoncus in faucibus sed urna.  volutpat mi-id purus ultrices iaculis nec niet neque. [Koppelingstekst voor de koppeling naar buiten azure.microsoft.com](http://weblogs.asp.net/scottgu). Nullam dictum dolor bij aliquam pharetra. Vivamus WC hendrerit mauris [voorbeeld koppelingstekst voor de koppeling naar een artikel in een servicemap](../articles/expressroute/expressroute-bandwidth-upgrade.md).

Ik krijg meer 10 keer verkeer van [Google]  [ gog] dan uit [Yahoo]  [ yah] - of [MSN] [msn].

> [AZURE.NOTE] Ingesprongen tekst.  Het woord 'notitie' wordt toegevoegd tijdens de publicatie. UT Europese Unie pretium lacus. Nullam purus schat, iaculis sed est vel, euismod vehicula odio. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi, Europese Unie condimentum turpis nisi een purus.

1. Aenean gaan zitten amet leo nec **Purus** placerat fermentum ac gravida odio.

2. Aenean tellus lectus, faucius in **Rhoncus** in faucibus sed urna. Suspendisse volutpat mi-id purus ultrices iaculis nec niet neque.

    ![Alternatieve tekst; laat niet leeg. Auto verzamelen in het rood racing.][5]

3. Nullam dictum dolor bij aliquam pharetra. Vivamus WC hendrerit mauris. Sed dolor dui, condimentum en varius per vehicula bij nisl.

    ![Alternatieve tekst; laat niet leeg][6]


Suspendisse volutpat mi-id purus ultrices iaculis nec niet neque. Nullam dictum dolor bij aliquam pharetra. Vivamus WC hendrerit mauris. Otrus informatus: [1 van de koppeling naar een andere azure.microsoft.com documentatie onderwerp](virtual-machines-windows-hero-tutorial.md)

## <a name="heading-h2"></a>Kop (H2)

UT Europese Unie pretium lacus. Nullam purus schat, iaculis sed est vel, euismod vehicula odio.

1. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi, Europese Unie condimentum turpis nisi een purus.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:
        (NSDictionary *)launchOptions
        {
            // Register for remote notifications
            [[UIApplication sharedApplication] registerForRemoteNotificationTypes:
            UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
            return YES;
        }

2. Vestibulum vooraf ipsum primis in faucibus orci luctus en ultrices posuere cubilia.

        // Because toast alerts don't work when the app is running, the app handles them.
        // This uses the userInfo in the payload to display a UIAlertView.
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
        (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
            [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
            @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }


    > [AZURE.NOTE] Duis sed diam niet <i>nisl molestie</i> pharetra eget een est. [Koppeling 2 naar een ander azure.microsoft.com documentatie onderwerp](web-sites-custom-domain-name.md)


Quisque commodo eros vel lectus euismod auctor eget gaan zitten amet leo. Proin faucibus suscipit tellus dignissim ultrices.

## <a name="heading-2-h2"></a>Kop 2 (H2)

1. Maecenas sed condimentum nisi. Suspendisse potenti.

  + Fusce
  + Malesuada
  + SEM

2. Nullam in massa Europese Unie tellus tempus hendrerit.

    ![Alternatieve tekst; laat niet leeg. Voorbeeld van een kanaal 9-video.][7]

3. Quisque felis enim, fermentum ut aliquam nec, pellentesque hoog pulvinar magna.




<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Vestibul vooraf ipsum primis in faucibus orci luctus en ultrices posuere cubilia Curae; Nullam ultricies, ipsum vitae volutpat hendrerit purus diam pretium eros, vitae tincidunt nulla lorem sed turpis: [3 van de koppeling naar een andere azure.microsoft.com documentatie onderwerp](storage-whatis-account.md).

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    
