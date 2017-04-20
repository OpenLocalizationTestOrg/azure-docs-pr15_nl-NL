<properties 
    pageTitle="Media Services PlayReady licentie sjabloon overzicht" 
    description="Dit onderwerp biedt een overzicht van een sjabloon van PlayReady licentie waarmee PlayReady licenties configureren." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="media-services-playready-license-template-overview"></a>Media Services PlayReady licentie sjabloon overzicht

Azure Media Services biedt nu een service om Microsoft PlayReady licenties te bieden. Wanneer de eindgebruiker-speler (bijvoorbeeld Silverlight) probeert uw PlayReady beveiligde inhoud af te spelen, wordt een aanvraag verzonden naar de bezorgingsservice licentie om een licentie te verkrijgen. Als de service licentie de aanvraag goedkeurt, oplossen deze de licentie die wordt verzonden naar de client en u ontsleutelt en het afspelen van de opgegeven inhoud kan worden gebruikt.

Media Services biedt ook API's waarmee u uw licenties PlayReady configureren. Licenties bevatten rechten en beperkingen die u voor PlayReady DRM runtime afdwingen wilt wanneer een gebruiker te afspelen beveiligde inhoud probeert.
Hieronder vindt u enkele voorbeelden van PlayReady licentiebeperkingen die u kunt opgeven:

- De datum-/ waaruit de licentie geldig is.
- De DateTime-waarde als de licentie verloopt. 
- Voor de licentie die moet worden opgeslagen in permanente opslag op de client. Permanente licenties worden meestal gebruikt voor het toestaan van offline afspelen van de inhoud.
- De minimale beveiligingsniveau die een speler hebt nodig om af te spelen van uw inhoud. 
- Het niveau van de bescherming uitvoer voor de uitvoer besturingselementen voor audio\video inhoud. 
- Zie de sectie uitvoer besturingselementen (3,5) in het document [PlayReady naleving van regels](https://www.microsoft.com/playready/licensing/compliance/) voor meer informatie.

>[AZURE.NOTE]U kunt op dit moment alleen de PlayRight van de PlayReady-licentie (dit recht is vereist) configureren. De PlayRight biedt de client de mogelijkheid tot de inhoud afspelen. De PlayRight kunt ook het configureren van beperkingen die specifiek zijn voor afspelen. Zie [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight)voor meer informatie.

Als u wilt configureren PlayReady licenties met Media-Services, moet u de sjabloon van de licentie Media Services PlayReady configureren. De sjabloon is gedefinieerd in XML-bestand.

Het volgende voorbeeld ziet u de eenvoudigste (en meest voorkomende) sjabloon waarmee u kunt een eenvoudige streaming licentie configureren. Met deze licentie, uw klanten zou kunnen afspelen uw PlayReady beveiligde inhoud.
    
    <?xml version="1.0" encoding="utf-8"?>
    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" 
                                      xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <PlayRight />
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

De XML voldoet aan het PlayReady licentie sjabloon XML-schema gedefinieerd in de PlayReady licentie sjabloon XML-schema-sectie.

Media-Services ook definieert een reeks .NET-klassen waarmee kan worden omgezet en gedeserialiseerd naar en vanuit het XML. Zie [Media Services .NET klassen](media-services-playready-license-template-overview.md#classes)voor beschrijving van de belangrijkste klassen. die worden gebruikt voor het configureren van de licentie sjablonen.

Voor een end-to-end-voorbeeld waarin .NET klassen voor het configureren van de sjabloon PlayReady-licentie wordt gebruikt, raadpleegt u [dynamische versleuteling PlayReady gebruiken en licentie bezorgingsservice](https://msdn.microsoft.com/library/azure/dn783467.aspx).

##<a id="classes"></a>Media Services .NET-klassen die worden gebruikt voor het configureren van de licentie-sjablonen

Hier volgen dat de belangrijkste .NET-klassen worden gebruikt voor het configureren van Media Services PlayReady licentie sjablonen. Deze klassen toewijzen aan de typen die is gedefinieerd in [PlayReady licentie sjabloon XML-schema](media-services-playready-license-template-overview.md#schema).

De klasse [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) wordt serialiseren en terugconverteren naar en vanuit de Media Services licentie sjabloon XML-gebruikt.

###<a name="playreadylicenseresponsetemplate"></a>PlayReadyLicenseResponseTemplate

[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) - deze klasse staat voor de sjabloon voor het antwoord dat terug naar de eindgebruiker worden verzonden. De presentatie bevat een veld voor een aangepaste gegevenstekenreeks tussen de licentieserver en de toepassing (mogelijk handig voor aangepaste app logica), evenals een lijst met sjablonen voor een of meer licentie.

Dit is de klasse 'hoofdniveau' in de Sjabloonhiërarchie. Wat betekent dat de sjabloon antwoord een lijst met sjablonen van de licentie bevat en de licentie sjablonen opnemen (direct of indirect) alle andere klassen waaruit de sjabloongegevens moet worden toegepast.


###<a name="playreadylicensetemplate"></a>PlayReadyLicenseTemplate

[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) - de klas vertegenwoordigt een licentie-sjabloon voor het maken van PlayReady licenties moeten worden geretourneerd voor eindgebruikers. De gegevens op de inhoud sleutel in de licentie en rechten of beperkingen wordt afgedwongen via de runtime PlayReady DRM bij gebruik van de inhoud toets bevat.


###<a id="PlayReadyPlayRight"></a>PlayReadyPlayRight

[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) - deze klasse staat voor het PlayRight van een PlayReady-licentie. Deze verstrekt de gebruiker de mogelijkheid om te afspelen de inhoud onderhevig aan de nul of meer beperkingen geconfigureerd in de licentie en klik op de PlayRight zelf (voor afspelen specifieke beleid). Veel van het beleid op het PlayRight heeft te maken met uitvoer-beperkingen die welke typen uitvoer van die de inhoud kan worden afgespeeld bepalen via en welke beperkingen moeten worden ingevoerd bij gebruik van een bepaald uitvoer. Bijvoorbeeld als de DigitalVideoOnlyContentRestriction is ingeschakeld, worden klikt u vervolgens de runtime DRM mag alleen de video moet worden weergegeven via digitale uitvoer (analoge video uitvoer won't worden toegestaan om door te geven van de inhoud).

>[AZURE.IMPORTANT]Typen beperkingen kunnen zijn zeer krachtig, maar ook invloed kunnen zijn op de consumenten-ervaring. Als de uitvoer bescherming hebt geconfigureerd te beperkend, is de inhoud mogelijk niet afgespeeld op sommige clients. Zie het document [PlayReady naleving van regels](https://www.microsoft.com/playready/licensing/compliance/) voor meer informatie.

Zie voor een voorbeeld van wat beveiliging worden geëffend Silverlight ondersteunt,: [Silverlight ondersteuning voor uitvoer bescherming](http://go.microsoft.com/fwlink/?LinkId=617318).

##<a id="schema"></a>PlayReady licentie sjabloon XML-schema
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:ser="http://schemas.microsoft.com/2003/10/Serialization/" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />
      <xs:complexType name="AgcAndColorStripeRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction" />
      <xs:simpleType name="ContentType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Unspecified" />
          <xs:enumeration value="UltravioletDownload" />
          <xs:enumeration value="UltravioletStreaming" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="ContentType" nillable="true" type="tns:ContentType" />
      <xs:complexType name="ExplicitAnalogTelevisionRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="BestEffort" type="xs:boolean" />
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ExplicitAnalogTelevisionRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction" />
      <xs:complexType name="PlayReadyContentKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="PlayReadyContentKey" nillable="true" type="tns:PlayReadyContentKey" />
      <xs:complexType name="ContentEncryptionKeyFromHeader">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence />
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromHeader" nillable="true" type="tns:ContentEncryptionKeyFromHeader" />
      <xs:complexType name="ContentEncryptionKeyFromKeyIdentifier">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence>
              <xs:element minOccurs="0" name="KeyIdentifier" type="ser:guid" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromKeyIdentifier" nillable="true" type="tns:ContentEncryptionKeyFromKeyIdentifier" />
      <xs:complexType name="PlayReadyLicenseResponseTemplate">
        <xs:sequence>
          <xs:element name="LicenseTemplates" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
          <xs:element minOccurs="0" name="ResponseCustomData" nillable="true" type="xs:string">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseResponseTemplate" nillable="true" type="tns:PlayReadyLicenseResponseTemplate" />
      <xs:complexType name="ArrayOfPlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfPlayReadyLicenseTemplate" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
      <xs:complexType name="PlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AllowTestDevices" type="xs:boolean" />
          <xs:element minOccurs="0" name="BeginDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element name="ContentKey" nillable="true" type="tns:PlayReadyContentKey" />
          <xs:element minOccurs="0" name="ContentType" type="tns:ContentType">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExpirationDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="GracePeriod" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="LicenseType" type="tns:PlayReadyLicenseType" />
          <xs:element minOccurs="0" name="PlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
      <xs:simpleType name="PlayReadyLicenseType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Nonpersistent" />
          <xs:enumeration value="Persistent" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="PlayReadyLicenseType" nillable="true" type="tns:PlayReadyLicenseType" />
      <xs:complexType name="PlayReadyPlayRight">
        <xs:sequence>
          <xs:element minOccurs="0" name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AllowPassingVideoContentToUnknownOutput" type="tns:UnknownOutputPassingOption">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AnalogVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="DigitalVideoOnlyContentRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExplicitAnalogTelevisionOutputRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="FirstPlayExpiration" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComponentVideoRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComputerMonitorRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyPlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
      <xs:simpleType name="UnknownOutputPassingOption">
        <xs:restriction base="xs:string">
          <xs:enumeration value="NotAllowed" />
          <xs:enumeration value="Allowed" />
          <xs:enumeration value="AllowedWithVideoConstriction" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="UnknownOutputPassingOption" nillable="true" type="tns:UnknownOutputPassingOption" />
      <xs:complexType name="ScmsRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction" />
    </xs:schema>



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
