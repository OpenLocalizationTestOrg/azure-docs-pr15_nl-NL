<properties
    pageTitle="Weergave SAML die het resultaat van de Access-Service van het besturingselement (Java)"
    description="Informatie over het weergeven van SAML geretourneerd door de Service voor het beheer van Access in Java-toepassingen die worden gehost op Azure."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>Het weergeven van SAML die het resultaat van de Azure-Service voor het beheer van Access

Deze handleiding leert u hoe u de onderliggende beveiliging bevestiging Markup Language (SAML) terug naar de toepassing door de Azure Access Control Service (ACS) kunt bekijken. De gids is gebaseerd op het onderwerp [hoe u webgebruikers verifiëren met Azure toegang besturingselement Service gebruiken Eclips][] doordat code waarin de SAML-gegevens worden weergegeven. De voltooide toepassing ziet er ongeveer als volgt uit.

![Voorbeeld SAML uitvoer][saml_output]

Zie voor meer informatie over ACS, de sectie van de [volgende stappen](#next_steps) .

> [AZURE.NOTE]
> Het Filter Azure Access Services besturingselement is een voorbeeld van de technologie community. Als bètasoftware, wordt deze niet formeel ondersteund door Microsoft.

## <a name="prerequisites"></a>Vereisten voor

De taken in deze handleiding uitvoeren, voert u de steekproef hoe [u webgebruikers verifiëren met Azure toegang besturingselement Service gebruiken Eclips][] en deze gebruiken als uitgangspunt voor deze zelfstudie.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>De bibliotheek JspWriter toevoegen aan uw opbouwen pad en de implementatie constructie

De bibliotheek waarin de klas **javax.servlet.jsp.JspWriter** naar uw opbouwen pad en de implementatie constructie toevoegen. Als u Tomcat gebruikt, is de bibliotheek **jsp-api.jar**, dat in de map Apache- **bibliotheek bevindt zich** .

1. In de Eclips Projectverkenner met de rechtermuisknop op **MyACSHelloWorld**op **Pad maken**, klikt u op **Bouwen pad configureren**, klik op het tabblad **bibliotheken** , en klik vervolgens op **Externe potten toevoegen**.
2. In het dialoogvenster **JAR selecteren** , Ga naar het benodigde oppervlak, selecteert u deze en klik vervolgens op **openen**.
3. Klik op **Implementatie verzameling**met het dialoogvenster **Eigenschappen voor MyACSHelloWorld** nog steeds openen.
4. Klik in het dialoogvenster **Web implementatie constructie** op **toevoegen**.
5. Klik in het dialoogvenster **Nieuwe constructie richtlijn** op **Java bouwen pad vermeldingen** en klik vervolgens op **volgende**.
6. Selecteer de juiste bibliotheek en klik op **Voltooien**.
7. Klik op **OK** om te sluiten van het dialoogvenster **Eigenschappen voor MyACSHelloWorld** .

## <a name="modify-the-jsp-file-to-display-saml"></a>Het bestand JSP om weer te geven op SAML wijzigen

Wijzig **index.jsp** als u wilt gebruiken van de volgende code.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {
        
            try
            {
                String nodeName;
                int nChild, i;
                
                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
                   
                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }
                   
                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                   {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    
    
                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        
                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }
                                            
                                     }
                                     out.println("<br>");
                                     
                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>
    
        <%
        try 
        {
            String data  = (String) request.getAttribute("ACSSAML");
            
            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();
            
            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();
            
            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();
    
            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e) 
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }
        
        %>
    </body>
    </html>

## <a name="run-the-application"></a>Voer de toepassing

1. Uw toepassing uitvoeren in de computer emulator of implementeren naar Azure, met de stappen beschreven hoe [u webgebruikers verifiëren met Azure toegang besturingselement Service gebruiken Eclips][].
2. Starten van een browser en open de webtoepassing. Nadat u zich bij uw toepassing aanmeldt, ziet u SAML-informatie, inclusief de bevestiging beveiliging is verstrekt door de identiteitsprovider.

## <a name="next-steps"></a>Volgende stappen

Verder te verkennen van ACS functionaliteit en om te experimenteren met meer geavanceerde scenario's, Zie [Access besturingselement Service 2.0][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access Control-Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Hoe webgebruikers worden geverifieerd met Azure Access Control-Service Eclips gebruiken]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 