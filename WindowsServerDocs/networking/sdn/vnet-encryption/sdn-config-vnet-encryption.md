---
title: Configurar a criptografia para uma rede virtual
description: A criptografia de rede virtual permite a criptografia do tráfego de rede virtual entre máquinas virtuais que se comunicam entre si em sub-redes marcadas como ' criptografia habilitada '.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 40150e312f4776ec093c9230eedb646eec277f49
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405806"
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>Configurar a criptografia para uma sub-rede virtual

>Aplica-se a: Windows Server

A criptografia de rede virtual permite a criptografia de tráfego de rede virtual entre VMs que se comunicam entre si em sub-redes marcadas como ' criptografia habilitada '. Ele também utiliza datagrama Transport Layer Security (DTLS) na sub-rede virtual para criptografar pacotes. O DTLS oferece proteção contra interceptações, falsificação e falsificação por qualquer pessoa com acesso à rede física.

A criptografia de rede virtual requer:
- Certificados de criptografia instalados em cada um dos hosts Hyper-V habilitados para SDN.
- Um objeto de credencial no controlador de rede que faz referência à impressão digital desse certificado.
- A configuração em cada uma das redes virtuais contém sub-redes que exigem Criptografia.

Depois de habilitar a criptografia em uma sub-rede, todo o tráfego de rede dentro dessa sub-rede é criptografado automaticamente, além de qualquer criptografia no nível do aplicativo que também possa ocorrer.  O tráfego que cruza entre sub-redes, mesmo se marcado como criptografado, é enviado sem criptografia automaticamente. Qualquer tráfego que cruzar o limite de rede virtual também é enviado sem criptografia.

>[!NOTE]
>Ao se comunicar com outra VM na mesma sub-rede, se estiver conectada ou conectada posteriormente, o tráfego será criptografado automaticamente.

>[!TIP]
>Se você precisar restringir os aplicativos para se comunicar apenas na sub-rede criptografada, poderá usar ACLs (listas de controle de acesso) somente para permitir a comunicação dentro da sub-rede atual. Para obter mais informações, consulte [usar ACLs (listas de controle de acesso) para gerenciar o fluxo de tráfego de rede do datacenter](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).


## <a name="step-1-create-the-encryption-certificate"></a>Etapa 1. Criar o certificado de criptografia
Cada host deve ter um certificado de criptografia instalado. Você pode usar o mesmo certificado para todos os locatários ou gerar um exclusivo para cada locatário. 

1.  Gerar o certificado  

```
    $subjectName = "EncryptedVirtualNetworks"
    $cryptographicProviderName = "Microsoft Base Cryptographic Provider v1.0";
    [int] $privateKeyLength = 1024;
    $sslServerOidString = "1.3.6.1.5.5.7.3.1";
    $sslClientOidString = "1.3.6.1.5.5.7.3.2";
    [int] $validityPeriodInYear = 5;

    $name = new-object -com "X509Enrollment.CX500DistinguishedName.1"
    $name.Encode("CN=" + $SubjectName, 0)

    #Generate Key
    $key = new-object -com "X509Enrollment.CX509PrivateKey.1"
    $key.ProviderName = $cryptographicProviderName
    $key.KeySpec = 1 #X509KeySpec.XCN_AT_KEYEXCHANGE
    $key.Length = $privateKeyLength
    $key.MachineContext = 1
    $key.ExportPolicy = 0x2 #X509PrivateKeyExportFlags.XCN_NCRYPT_ALLOW_EXPORT_FLAG 
    $key.Create()

    #Configure Eku
    $serverauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $serverauthoid.InitializeFromValue($sslServerOidString)
    $clientauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $clientauthoid.InitializeFromValue($sslClientOidString)
    $ekuoids = new-object -com "X509Enrollment.CObjectIds.1"
    $ekuoids.add($serverauthoid)
    $ekuoids.add($clientauthoid)
    $ekuext = new-object -com "X509Enrollment.CX509ExtensionEnhancedKeyUsage.1"
    $ekuext.InitializeEncode($ekuoids)

    # Set the hash algorithm to sha512 instead of the default sha1
    $hashAlgorithmObject = New-Object -ComObject X509Enrollment.CObjectId
    $hashAlgorithmObject.InitializeFromAlgorithmName( $ObjectIdGroupId.XCN_CRYPT_HASH_ALG_OID_GROUP_ID, $ObjectIdPublicKeyFlags.XCN_CRYPT_OID_INFO_PUBKEY_ANY, $AlgorithmFlags.AlgorithmFlagsNone, "SHA512")


    #Request Certificate
    $cert = new-object -com "X509Enrollment.CX509CertificateRequestCertificate.1"

    $cert.InitializeFromPrivateKey(2, $key, "")
    $cert.Subject = $name
    $cert.Issuer = $cert.Subject
    $cert.NotBefore = (get-date).ToUniversalTime()
    $cert.NotAfter = $cert.NotBefore.AddYears($validityPeriodInYear);
    $cert.X509Extensions.Add($ekuext)
    $cert.HashAlgorithm = $hashAlgorithmObject
    $cert.Encode()

    $enrollment = new-object -com "X509Enrollment.CX509Enrollment.1"
    $enrollment.InitializeFromRequest($cert)
    $certdata = $enrollment.CreateRequest(0)
    $enrollment.InstallResponse(2, $certdata, 0, "")
```

Depois de executar o script, um novo certificado aparecerá no meu repositório:

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

2. Exporte o certificado para um arquivo.<p>Você precisa de duas cópias do certificado, uma com a chave privada e outra sem.

```
   $subjectName = "EncryptedVirtualNetworks"
   $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"}
   [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret"))
   Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert
```

3. Instalar os certificados em cada um de seus hosts Hyper-v 

   PS c:\> dir c:\$SubjectName. *


~~~
    Directory: C:\


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer
-a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx
~~~

4. Instalando em um host Hyper-V

```
   $server = "Server01"

   $subjectname = "EncryptedVirtualNetworks"
   copy c:\$SubjectName.* \\$server\c$
   invoke-command -computername $server -ArgumentList $subjectname,"secret" {
       param (
           [string] $SubjectName,
           [string] $Secret
       )
       $certFullPath = "c:\$SubjectName.cer"

       # create a representation of the certificate file
       $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
       $certificate.import($certFullPath)

       # import into the store
       $store = new-object System.Security.Cryptography.X509Certificates.X509Store("Root", "LocalMachine")
       $store.open("MaxAllowed")
       $store.add($certificate)
       $store.close()

       $certFullPath = "c:\$SubjectName.pfx"
       $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
       $certificate.import($certFullPath, $Secret, "MachineKeySet,PersistKeySet")

       # import into the store
       $store = new-object System.Security.Cryptography.X509Certificates.X509Store("My", "LocalMachine")
       $store.open("MaxAllowed")
       $store.add($certificate)
       $store.close()

       # Important: Remove the certificate files when finished
       remove-item C:\$SubjectName.cer
       remove-item C:\$SubjectName.pfx
   }
```

5. Repita para cada servidor em seu ambiente.<p>Depois de repetir para cada servidor, você deve ter um certificado instalado na raiz e meu repositório de cada host Hyper-V. 

6. Verifique a instalação do certificado.<p>Verifique os certificados verificando o conteúdo dos meus repositórios de certificados My e root:

   PS C:\> Enter-PSSession Server1

~~~
[Server1]: PS C:\> get-childitem cert://localmachine/my,cert://localmachine/root | ? {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

Thumbprint                                Subject
----------                                -------
5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks


PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\root

Thumbprint                                Subject
----------                                -------
5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks
~~~

7. Anote a impressão digital.<p>Você deve anotar a impressão digital porque precisa dela para criar o objeto de credencial do certificado no controlador de rede.

## <a name="step-2-create-the-certificate-credential"></a>Etapa 2. Criar a credencial do certificado

Depois de instalar o certificado em cada um dos hosts Hyper-V conectados ao controlador de rede, agora você deve configurar o controlador de rede para usá-lo.  Para fazer isso, você deve criar um objeto de credencial contendo a impressão digital do certificado do computador com os módulos do PowerShell do controlador de rede instalados. 

```
    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  

    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller

    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force
```
>[!TIP]
>Você pode reutilizar essa credencial para cada rede virtual criptografada ou pode implantar e usar um certificado exclusivo para cada locatário.


## <a name="step-3-configuring-a-virtual-network-for-encryption"></a>Etapa 3. Configurando uma rede virtual para criptografia

Esta etapa pressupõe que você já criou um nome de rede virtual "minha rede" e que ele contém pelo menos uma sub-rede virtual.  Para obter informações sobre como criar redes virtuais, consulte [criar, excluir ou atualizar redes virtuais de locatário](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md).

>[!NOTE]
>Ao se comunicar com outra VM na mesma sub-rede, se estiver conectada ou conectada posteriormente, o tráfego será criptografado automaticamente.

1.  Recuperar a rede virtual e os objetos de credencial do controlador de rede
```
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork"
    $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"
```
2.  Adicionar uma referência à credencial de certificado e habilitar a criptografia em sub-redes individuais
```
    $vnet.properties.EncryptionCredential = $certcred

    # Replace the Subnets index with the value corresponding to the subnet you want encrypted.  
    # Repeat for each subnet where encryption is needed
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true
```
3.  Colocar o objeto de rede virtual atualizado no controlador de rede
```
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force
```

_**Congratula!**_ Quando concluir essas etapas, você terá concluído. 


## <a name="next-steps"></a>Próximas etapas



