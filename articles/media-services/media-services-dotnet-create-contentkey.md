<properties 
    pageTitle="ContentKeys met .NET maken" 
    description="Informatie over het maken van inhoud toetsen die beveiligde toegang tot activa bieden." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="create-contentkeys-with-net"></a>ContentKeys met .NET maken

> [AZURE.SELECTOR]
- [REST](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)

Media-Services kunt u maken en geven van versleutelde activa. Een **ContentKey** biedt beveiligde toegang tot uw **activa**s. 

Wanneer u een nieuw activum (bijvoorbeeld voordat u [bestanden uploaden](media-services-dotnet-upload-files.md)) maakt, kunt u de volgende versleutelingsopties: **StorageEncrypted**, **CommonEncryptionProtected**of **EnvelopeEncryptionProtected**. 

Als u activa wilt op uw klanten overbrengen, kunt u [configureren voor activa dynamisch worden versleuteld](media-services-dotnet-configure-asset-delivery-policy.md) met een van de volgende twee coderingen: **DynamicEnvelopeEncryption** of **DynamicCommonEncryption**.

Versleutelde activa moeten zijn gekoppeld aan **ContentKey**s. In dit artikel wordt beschreven hoe een inhoud sleutel maakt.

>[AZURE.NOTE] Wanneer u een nieuw **StorageEncrypted** activum met behulp van de Media Services .NET SDK maakt, wordt de **ContentKey** automatisch gemaakt en die zijn gekoppeld aan de activa.

##<a name="contentkeytype"></a>ContentKeyType

Een van de waarden dat u wanneer instellen moet een inhoud maken sleutel is het belangrijkste inhoudstype. Kies een van de volgende waarden. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

##<a id="envelope_contentkey"></a>Envelop type ContentKey maken

Het volgende codefragment een inhoud sleutel van de envelop versleutelingstype gemaakt. Vervolgens wordt de toets met het opgegeven activum gekoppeld.

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

bellen

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>Algemene type ContentKey maken    

Het volgende codefragment Hiermee maakt u een inhoud sleutel van de algemene coderingstype. Vervolgens wordt de toets met het opgegeven activum gekoppeld.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
bellen

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
