## <a name="send-messages-to-event-hubs"></a>Berichten verzenden naar gebeurtenis Hubs

In deze sectie schrijft we een C-app als u wilt verzenden gebeurtenissen op uw Hub gebeurtenis. We gebruiken de bibliotheek Proton AMQP uit het [Apache Qpid project](http://qpid.apache.org/). Dit is vergelijkbaar met het gebruik van Service Bus wachtrijen en onderwerpen via AMQP uit C als weergegeven [hier](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Zie [Qpid Proton documentatie](http://qpid.apache.org/proton/index.html)voor meer informatie.

1. Klik op de koppeling **Qpid Proton installeren** vanaf de [pagina Qpid AMQP Messenger](http://qpid.apache.org/components/messenger/index.html), en volg de instructies afhankelijk van uw omgeving. Wordt ervan uitgegaan een Linux-omgeving. bijvoorbeeld een [Azure Linux VM](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md) met Ubuntu 14.04.

2. De bibliotheek Proton compileren, installeert u de volgende pakketten:

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. De bibliotheek [Qpid Proton bibliotheek](http://qpid.apache.org/proton/index.html) downloaden en extraheren, bijvoorbeeld:

    ```
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. Maak een opbouwen directory, compileren en installeren:

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. In het telefoonboek van uw werk, maak een nieuw bestand **sender.c** met de volgende inhoud genoemd. Vergeet niet te vervangen door de waarde voor de naam van de gebeurtenis Hub en de naamruimtenaam van de (meestal de laatste is `{event hub name}-ns`). U moet ook vervangen door een URL-codering versie van de toets voor de **SendRule** eerder hebt gemaakt. U kunt URL-codering deze [hier](http://www.w3schools.com/tags/ref_urlencode.asp).

    ```
    #include "proton/message.h"
    #include "proton/messenger.h"

    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>

    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }  

    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  

    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }

    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";

        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();

        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);

        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));

        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);

        pn_message_free(message);
    }

    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");

        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);

        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }

        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);

        return 0;
    }
    ```

6. Het bestand, ervan uitgaande dat er **gcc**samenstellen:

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] In deze code gebruiken we een uitgaande-venster van 1 om de uitgaande berichten zo snel mogelijk. In het algemeen, moet uw toepassing probeert te batch berichten om uit te breiden doorvoer. Zie [Qpid AMQP Messenger-pagina](http://qpid.apache.org/components/messenger/index.html) voor meer informatie over het gebruik van de bibliotheek Qpid Proton in dit en andere omgevingen en van platforms waarvoor bindingen worden verstrekt (momenteel Perl, PHP, Python en Ruby).
