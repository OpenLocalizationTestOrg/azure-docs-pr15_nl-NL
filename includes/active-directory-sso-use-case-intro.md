Organisaties gebruikt meer [als een Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) softwaretoepassingen voor productiviteit omdat cloud technologie en hulpmiddelen worden steeds meer beschikken. Als het aantal SaaS apps omvang groeit, wordt het uitdaging voor de beheerders voor het beheren van accounts en toegangsrechten en voor de gebruikers hun verschillende wachtwoorden onthouden. Beheer van deze toepassingen afzonderlijk Hiermee maakt u extra werk en minder veilig is.


- Werknemers die hebben ontvangen voor veel wachtwoorden meestal minder veilige methoden gebruiken om te onthouden zijn, op opschrijven wachtwoorden of het gebruik van dezelfde wachtwoorden op veel accounts.

- Wanneer een nieuwe werknemer binnenkomt of een weggaat, alle hun accounts afzonderlijk deze is ingericht of buiten gebruik gesteld.

- Daarnaast kunnen werknemers mogelijk aan de slag met SaaS apps voor hun werk zonder tussenkomst IT, wat betekent dat zij hun eigen accounts maken op systemen die de IT-beheerders dat nog niet hebt goedgekeurd en zijn niet bewaken.  

Er is een oplossing voor al deze uitdagingen voor eenmalige aanmelding (SSO). Dit is de eenvoudigste manier voor het beheren van meerdere apps en gebruikers voorzien van een consistente functionaliteit aanmelding. Azure Active Directory (Azure AD) biedt een robuuste oplossing voor eenmalige aanmelding en veel beschikbare vooraf geïntegreerde toepassingen, met zelfstudies voor beheerders kunt u snel een nieuwe app instellen en start de inrichting van gebruikers heeft.


## <a name="how-does-azure-active-directory-integrate-apps"></a>Hoe werkt de Azure Active Directory apps integratie?  

Azure AD kunt u uw apps en ingerichte accounts integreren. Dit kunt doen via een van twee methoden.

- Als de app vooraf geïntegreerde in de galerie-app is, kunt u die portal apps installeren en configureren van de instellingen wilt toestaan dat van eenmalige aanmelding doorlopen. Voor alle Apps galerie, u kunt aan de slag door Volg de eenvoudige stapsgewijze instructies gepresenteerd in de app-galerie en in de portal van Azure voor eenmalige aanmelding inschakelen.

- Als de app niet in de galerie is, kunt u de meeste apps nog steeds instellen in Azure AD als een aangepaste app. U moet hiervoor minder technische expertise om te configureren. U kunt een toepassing die ondersteuning biedt voor SAML 2.0 als een federatieve app of een toepassing met een HTML-e-aanmeldingspagina als een wachtwoord SSO-app toevoegen.

In het geval waar gebruikers hun eigen accounts voor SaaS-apps die niet worden beheerd door hebt gemaakt IT, het hulpmiddel [Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) biedt een oplossing. Dit hulpmiddel controleert de webverkeer om te bepalen welke apps worden gebruikt in de organisatie, en het aantal personen met elk van deze. IT kunnen deze gegevens gebruiken voor meer informatie over welke apps de gebruikers liever om te bepalen welke integreren met Azure AD voor eenmalige aanmelding.  

Wanneer u een app integreren met Azure AD, kunt u de gebruikers waarop deze is opgezet toepassing identiteiten toewijzen aan de bijbehorende Azure AD identiteiten.  
