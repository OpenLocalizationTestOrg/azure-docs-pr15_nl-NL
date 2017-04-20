<properties
    pageTitle="Azure AD Java aan de slag | Microsoft Azure"
    description="Het maken van een Java opdrachtregel-app die gebruikers zich aanmeldt bij het openen van een API."
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Java opdrachtregel App gebruiken voor toegang tot een API met Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD kunt u eenvoudige en duidelijke naar uw web-app identiteitsbeheer, uitbesteden leveren één aanmelden en afmelden met slechts een paar regels met code.  In de web-apps Java, kunt u dit doen met Microsoft implementatie van de community op basis van hoeveelheid werk ADAL4J.

  Hier gebruiken we ADAL4J naar:
- Meld u aan de gebruiker in de app met Azure AD als de identiteitsprovider.
- Vindt u informatie over de gebruiker weergeven.
- Meld u aan de gebruiker afmelden bij de app.

Hiervoor moet u:

1. Een toepassing met Azure AD registreren
2. Uw app instellen voor het gebruik van de bibliotheek ADAL4J.
3. De bibliotheek ADAL4J op aanmelden en afmelden aanvragen op Azure AD gebruiken.
4. Gegevens over de gebruiker te drukken.

Om gestart, [de app-skelet downloaden](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) of [het voltooide voorbeeld](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip).  U moet ook een Azure AD-tenant waarin uw toepassing registreren.  Als u geen sjabloon hebt, [leren hoe u een](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1. een toepassing met Azure AD registreren
Als u wilt dat uw app om gebruikers te verifiëren, moet u eerst een nieuwe toepassing registreren in uw tenant.

- Meld u aan bij de Portal Azure Management.
- Klik in de navigatiebalk linkerpagina op **Active Directory**.
- Selecteer de tenant waar u wilt registreren van de toepassing.
- Klik op het tabblad **toepassingen** en klik op toevoegen in de onder-vel.
- Volg de aanwijzingen en maak een nieuwe **webtoepassing en/of WebAPI**.
    - De **naam** van de toepassing wordt uw toepassing voor eindgebruikers beschrijven
    - De **URL voor eenmalige aanmelding** is de basis-URL van uw app.  De standaardwaarde van het skelet is `http://localhost:8080/adal4jsample/`.
    - De **App-ID-URI** is een unieke id voor uw toepassing.  De overeenkomst is via `https://<tenant-domain>/<app-name>`, bijvoorbeeld`http://localhost:8080/adal4jsample/`
- Wanneer u de registratie hebt uitgevoerd, wordt AAD toegewezen uw app een unieke client-id.  U moet deze waarde in de volgende secties, zodat deze kopiëren op het tabblad configureren.

Eenmaal in de portal voor uw app maken van een **Toepassing geheim** voor uw toepassing en kopieer deze naar beneden.  U moet deze binnenkort.


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. instellen van uw app ADAL4J bibliotheek en vereisten voor het gebruik van Maven gebruiken
Hier wordt we ADAL4J voor gebruik van de verificatie OpenID verbinding protocol configureren.  ADAL4J worden gebruikt op aanmelden en afmelden verzoeken en informatie over de gebruiker, onder andere sessie van de gebruiker beheren.

-   In de hoofdmap van uw project, de openen/maken `pom.xml` en zoek naar de `// TODO: provide dependencies for Maven` en vervangen door de volgende handelingen uit:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. het java PublicClient-bestand maken

Zoals hierboven, we gebruiken de API Graph toegang tot gegevens over de gebruiker is aangemeld. Hiermee kunt u gemakkelijk ons moet we maken zowel een bestand voor een **Directory-Object** als een afzonderlijk bestand om aan te geven van de **gebruiker** , zodat het patroon OO Java kan worden gebruikt.

1. Maken van een bestand met de naam `DirectoryObject.java` die we gebruiken om op te slaan eenvoudige gegevens over een DirectoryObject (u kunt je mag rustig dit later gebruiken voor andere grafiek query's kunt u doen). U kunt knippen en plakken dit uit het volgende:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>Compileren en uitvoeren van de steekproef

Wijzig weer uit in de hoofdmap en voer de volgende opdracht voor het maken van de steekproef u plaatsen samenwerken met `maven`. Hiermee wordt gebruikt de `pom.xml` bestand dat u hebt geschreven voor afhankelijkheden.

`$ mvn package`

U hebt nu een `adal4jsample.war` bestand uw `/targets` directory. U kunt implementeren die in de container Tomcat en Ga naar de URL 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
Het is heel eenvoudig een WAR met de meest recente Tomcat-servers implementeren. Eenvoudig navigeren naar `http://localhost:8080/manager/` en volg de instructies over het uploaden van uw '' adal4jsample.war' bestand. Deze autodeploy wordt u met het juiste eindpunt.

##<a name="next-steps"></a>Volgende stappen

Gefeliciteerd! U hebt nu een werkende Java-toepassing met de mogelijkheid om gebruikers te verifiëren, veilig bellen Web API's met OAuth 2.0 en basisinformatie over de gebruiker.  Als u nog niet is gedaan, is nu de tijd aan het vullen van uw tenant met enkele gebruikers.

Voor een verwijzing naar kunt de voltooide steekproef (zonder de configuratiewaarden) [wordt weergegeven als een .zip hier](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip)of u klonen deze uit GitHub:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

