# <a name="pull-request-comment-automation"></a>Aanvraag Opmerking automatisering halen

We gebruiken Opmerking automatisering in halen verzoek om opmerkingen toe te staan dat inzenders en auteurs labels die het verzoek halen station toewijzen Controleer proces.

| Opmerking | Beschrijving | Beschikbaarheid|
| -------- |-------------|-------------|
|#afmelden | Wanneer de auteur van een artikel typt de opmerking **#sign-uitschakelen** in de stream opmerking, wordt het label **bij de hand-naar-samenvoegen** is toegewezen. | Openbare en persoonlijke|
|#afmelden | Als een inzender die niet de vermelde auteur pogingen doen om te afmelden op een openbare halen-aanvraag met de opmerking **sign uit #** , wordt een bericht is geschreven naar de halen verzoek waarin wordt aangegeven dat het label alleen door de auteur kan worden toegewezen. | Openbare |
|#wachtstand uitschakelen | Als u **#hold-uitschakelen** in een opmerking van de aanvraag halen typt, wordt het label **bij de hand-naar-samenvoegen** - verwijderd geval u van gedachten verandert of een fout maakt. In de persoonlijke cessies‑retrocessies, wordt dit het label **not samenvoegen** toegewezen. | Openbare en persoonlijke |
| #Geef / slot | Auteurs kunnen u de opmerking **#please / slot** typen in de stream Opmerking van een aanvraag halen om dit te sluiten als u besluit om niet te laten de wijzigingen samengevoegd. | Openbare |

##<a name="troubleshooting-sign-offs-in-the-public-repo"></a>Teken-en nadelen in de openbare cessies‑retrocessies probleemoplossing

De openbare cessies‑retrocessies afmelden automatisering is kan alleen de auteur afmelden. Sommige verwerking handmatige uitzondering mogelijk zijn er nodig:

- **Artikel auteurs**: als u wilt gebruiken in de openbare opslagplaats Opmerking automatisering, uw eigen GitHub-account moet PRECIES overeenkomen met het GitHub-account wordt vermeld in het artikel-metagegevens. Het hoofdlettergebruik van uw account is van belang. Als u van ondertekening uitschakelen vanwege dit probleem worden geblokkeerd, een e-mail naar de alias azdocprs versturen.

- **Andere werknemers**: als u een werknemer die uitschakelen aanmeldt zich namens de auteur en u worden geblokkeerd door de automatisering, neemt u contact azdocprs met de koppeling van de aanvraag halen. Aangeven wie u zijn--PMs op het dezelfde productteam, collega's op het team schrijven en teammanagers schrijven worden beschouwd als vertrouwde bronnen.



## <a name="related"></a>Gerelateerd

- [Halen verzoek mailetiquette en aanbevolen procedures voor het Microsoft-inzenders](contributor-guide-pull-request-etiquette.md)

- [Kwaliteitscriteria voor halen aanvraag controleren](contributor-guide-pr-criteria.md)
