<properties
    pageTitle="Eenheid album een bal zelfstudie"
    description="Stappen voor het maken van de klassieke eenheid implementeren een bal spel namelijk spelen voor alle Mobile betrokkenheid eenheid zelfstudies"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a id="unity-roll-a-ball"></a>Maak gebruik van de eenheid een spel bal

Deze zelfstudie begeleidt bij de belangrijkste stappen die iets gewijzigd [eenheid album een bal zelfstudie](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). In dit voorbeeld spel bestaat uit een bolvormige speler-object dat wordt beheerd door de app-gebruiker en het doel van het spel is 'verzamelen' verzamelobjecten: objecten met tegenaan van het object speler met deze verzamelobjecten: objecten. Hiermee wordt ervan uitgegaan eenvoudige bekend zijn met eenheid editor-omgeving. Als u problemen ondervindt moet u ook verwijzen naar de volledige zelfstudie. 

### <a name="setting-up-the-game"></a>Bij het instellen van het spel
De onderstaande stappen zijn van de [eenheid zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. **Eenheid Editor** openen en klik op **Nieuw**. 
    
    ![][51] 
    
2. Geef een **Projectnaam** & **locatie**, selecteert u **3D** en klik op **project maken**.
    
    ![][52]

3. Sla de standaard-scène net hebt gemaakt als onderdeel van het nieuwe project als met de naam **MiniGame** binnen een nieuwe ** \_scènes** map onder **activa** map:
    
    ![][53]

4. Maak een- **3D-Object -> vlak** als het veld afspelen en wijzig de naam dit object vlak als **grond**

    ![][1]

5. Opnieuw instellen van het onderdeel transformeren voor dit object **grond** zodat deze op de oorsprong is. 

    ![][3]

6. Schakel **Raster weergeven** in **Gizmos menu** voor het object **grond** .

    ![][4]

7. De **schaal** component voor het object **grond** moeten bijwerken [X = 2, Y = 1, Z = 2]. 

    ![][5]

8. Een nieuwe- **3D-Object-bol >** toevoegen aan het project en de naam van dit object bol als **speler**wijzigen. 

    ![][6]

9. Selecteer het object **Player** en klikt u op **Beginwaarden transformeren** lijkt op het vlak-object. 

10. Update **Transformeren -> positie -> Y-coördinaten** component voor de speler Y als 0,5.  

    ![][7]

11. Maak een nieuwe map genaamd **materialen** in het project waar we het materiaal dat aan de kleur van de speler gaan maken. 

12. Maak een nieuw **materiaal** ( **achtergrond)** in deze map. 

    ![][8]

13. Werk de kleur van het materiaal dat door de eigenschap **Albedo** van deze bij te werken. U kunt de RGB-waarden van [0,32,64] selecteren. 

    ![][9]

14. Dit materiaal naar de weergave van de scène kleur wilt toepassen op het object **grond** slepen. 

    ![][10]

17. Ten slotte bijwerken de **Transformeren -> draaiing Y ->** 60 op het object gericht licht voor duidelijkheid. 

    ![][12]

### <a name="moving-the-player"></a>Windows media player verplaatsen
De onderstaande stappen zijn van de [eenheid zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Een **RigidBody** -onderdeel toevoegen aan het object **Player** . 

    ![][13]

2. Maak een nieuwe map genaamd **Scripts** in het Project. 

3. Klik op **onderdeel toevoegen -> nieuwe Script C#-Script ->**. Noem deze **PlayerController**en klik op **maken en toevoegen**. Hiermee maakt u en een script als bijlage toevoegen aan het object Player.  

    ![][14]

5. Dit script onder de map **Scripts** in het project gaan 

6. Het script om te bewerken in uw favoriete scripteditor openen, de scriptcode bijwerkt met de volgende code en opslaan. 

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
    
8. Houd er rekening mee dat het bovenstaande script gebruikmaakt van een eigenschap **snelheid** . Klik in de editor eenheid bijwerken de eigenschap snelheid tot en met 10.  

    ![][15]

9. Druk op **afspelen** in de eenheid-Editor. Nu u zou moeten kunnen om te bepalen de bal met het toetsenbord en moet deze draaien en te navigeren. 

### <a name="moving-the-camera"></a>De camera verplaatsen
De onderstaande stappen van de [eenheid zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) en wordt het **Hoofdgegeven Camera** op het object **speler** koppelen. 

1. Bijwerken van de **Transform.Position** als X = 0, Y = 10.5, Z =-10.  
2. Bijwerken van de **Transform.Rotation** als X = 45, Y = 0, Z = 0.  

    ![][16]

2. Voeg een nieuw script **CameraController** naar de **MainCamera** genoemd en verplaatst u deze onder de map Scripts. 

    ![][17]

3. Open het script om te bewerken en de volgende code erin hebt toegevoegd:

        using UnityEngine;
        using System.Collections;
        
        public class CameraController : MonoBehaviour {
        
            public GameObject player;
        
            private Vector3 offset;
        
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
            
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
    
5. Sleep de variabele speler in eenheid omgeving - in de speler slot voor het hoofdgegeven Camera-object dat de twee gekoppeld aan elkaar zijn. 

    ![][18]

6. Nu als u afspelen in de editor eenheid raken en u het object speler bal ziet vervolgens u de volgende tekst in het verkeer Camera.  

### <a name="setting-up-the-play-area"></a>Bij het instellen van het gebied afspelen
De onderstaande stappen zijn van de [eenheid zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). We gaan de wanden rond de grond zodat het object speler bal niet uit het gebied afspelen in de verplaatsing neerzetten maken. 

1. Klik op **maken -> lege maken spel Object ->** en noem deze **wanden**

    ![][19]

2. Klik onder dit object wanden - maken een nieuwe- **3D-Object-kubus >** en noem deze "West wand". 

    ![][20]

3. Werk de **Transformeren -> positie** en **Transformeren -> schaal** voor dit object West wand. 

    ![][21]

4. De wand West als u wilt een **Oost wand** maken met de bijgewerkte transformeren positie en de schaal dupliceren. 

    ![][22]

5. De wand Oost als u wilt een **Noord wand** maken met de bijgewerkte transformeren positie & schaal dupliceren. 

    ![][23]

6. Dupliceren van de wand Noord en een **Zuid wand** maken met de bijgewerkte transformeren positie & schaal. 

    ![][24]

### <a name="creating-collectible-objects"></a>Verzamelobjecten: objecten maken
De onderstaande stappen zijn van de [eenheid zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). We gaan sommige fraai ogende objecten die wordt de set verzamelobjecten: objecten die het object speler bal verzamelen moet door conflicten met deze vormen maken. 

1. Maak een nieuw **3D-kubus object** en noem deze ophalen. 

2. Pas de- **transformatie -> draaiing** & **Transformeren -> schaal** van het object ophalen-. 

    ![][25]

3. Maken en een **Nieuwe C# Script** **Rotator** aan het object ophalen met de naam wilt toevoegen. Zorg ervoor dat het script plaatsen onder de map Scripts. 

    ![][26]

4. Open dit script om te bewerken en bijwerken om te worden de volgende: 

        using UnityEngine;
        using System.Collections;
        
        public class Rotator : MonoBehaviour {
        
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }

5. Klik op nu worden de afspeelmodus in de Editor eenheid en de ophalen object te geven op de as draaien.

6. Een nieuwe map genaamd **Prefabs** maken 

    ![][27]

7. Sleep het object **ophalen** en zet dit in de map Prefabs.

    ![][28]

8. Maak een nieuwe **lege spel-object** met de naam van de **overdracht op**. De positie terugzetten naar origin en sleep vervolgens de ophalen object onder dit spel object.  

    ![][29]

9. Dupliceren van het object **ophalen** los en klik op het object **grond** rondom het object **speler** door de waarden **van Transform.Position X & Z** correct bij te werken. 

    ![][30]

10. Maak een **nieuw materiaal** genoemd **ophalen** en bijwerken om te worden rood in kleur door de **eigenschap Albedo** lijkt op wat die we hebben gedaan voor het bijwerken van het object grond bij te werken. 

    ![][31]

11. Het materiaal dat van toepassing op alle 4 ophalen objecten.

    ![][32]

### <a name="collecting-the-pickup-objects"></a>De ophalen objecten verzamelen
De onderstaande stappen zijn van de [eenheid zelfstudie](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). We zullen de speler bijwerken zodat deze kan worden 'verzamelen' de ophalen objecten met conflicten met deze is. 

1. Open het script **PlayerController** is gekoppeld aan het object Player om te bewerken en bijwerken naar het volgende:  

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour {
        
            public float speed;
        
            private Rigidbody rb;
        
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
        
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
        
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
        
                rb.AddForce (movement * speed);
            }
        
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }

2. Een nieuwe **Tag** genoemd **Kies omhoog** (moet overeenkomen met wat staat er in het script) maken  

    ![][33]
    
    ![][34]

3. Deze **markering** toepassen op het object Prefab ophalen. 

    ![][35]

4. **IsTrigger** het selectievakje voor het object Prefab inschakelen.

    ![][36]

5. Een harde hoofdtekst toevoegen aan ophalen Prefab object. We zullen de statische collider die wordt gebruikt bij het optimaliseren bijwerken naar een dynamische collider. 

    ![][37]
  
6. Ten slotte de eigenschap **IsKinematic** voor het prefab object controleren. 

    ![][38]

7. Treffers **afspelen** in de editor eenheid en u kunnen deze spel **een bal implementeren** door de speler-object met de toetsen om invoer richting te bewegen. 

### <a name="updating-the-game-for-mobile-play"></a>Het spel voor mobiele afspelen bijwerken
De bovenstaande secties gesloten de eenvoudige zelfstudie van eenheid. Nu wordt we het spel zodat u deze mobiel apparaat beschrijvende wijzigen. Houd er rekening mee dat we invoer van het toetsenbord voor het spel dusverre voor het testen gebruikt. Nu wijzigt we deze zodat we de speler bepalen kunt met behulp van de bewegingen van de telefoon, dat wil zeggen versnellingsmeter gebruiken als invoer. 

Open het **PlayerController** -script om te bewerken en bijwerken van de methode **FixedUpdate** als u wilt de animatie van de versnellingsmeter gebruiken om de speler-object te verplaatsen. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Deze zelfstudie eindigt maken van een eenvoudige spel met eenheid en kunt u dit op een apparaat van uw keuze en spelen implementeren. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png  
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png  
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png

    
    
    
    
    
    
    
    
    
    
    
    
