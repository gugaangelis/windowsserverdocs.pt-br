---
title: Configurar a criptografia para uma rede Virtual
description: Criptografia de rede virtual permite a criptografia de tráfego de rede virtual entre máquinas virtuais que se comunicam entre si em sub-redes marcadas como 'A criptografia habilitada.'
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 90fb33eb4c4b63fdd5c84bf3ffc2447fd52a809b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845487"
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>Configurar a criptografia para uma sub-rede Virtual

>Aplica-se a: Windows Server

Permite a criptografia de rede virtual para a criptografia de tráfego de rede virtual entre as VMs que se comunicam entre si em sub-redes marcadas como 'A criptografia habilitada.' Ele também utiliza datagrama Transport Layer Security (DTLS) na sub-rede virtual para criptografar pacotes. O DTLS oferece proteção contra interceptações, falsificação e falsificação por qualquer pessoa com acesso à rede física.

Requer criptografia de rede virtual:
- Certificados de criptografia instalados em cada um dos hosts Hyper-V SDN habilitado.
- Um objeto de credencial no controlador de rede, fazendo referência a impressão digital do certificado.
- A configuração em cada uma das redes virtuais contém sub-redes que exigem criptografia.

Depois que você habilitar a criptografia em uma sub-rede, todo o tráfego de rede dentro dessa sub-rede é criptografado automaticamente, além de qualquer criptografia no nível do aplicativo que pode ocorrer também.  Tráfego que atravessa entre sub-redes, mesmo se marcado como criptografadas, é automaticamente enviado descriptografado. Qualquer tráfego que cruza o limite de rede virtual também obtém enviado descriptografado.

>[!NOTE]
>Ao se comunicar com outra VM na mesma sub-rede, se seu atualmente conectada ou conectado em um momento posterior, o tráfego é criptografado automaticamente.

>[!TIP]
>Se você deve restringir os aplicativos se comuniquem apenas na sub-rede criptografada, você pode usar listas de controle de acesso (ACLs) apenas para permitir a comunicação dentro da sub-rede atual. Para obter mais informações, consulte [uso Access Control Lists (ACLs) para gerenciar o data center rede tráfego fluir](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).


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

Depois de executar o script, um novo certificado aparece no meu repositório:

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

2.  Exporte o certificado para um arquivo.<p>Você precisa de duas cópias do certificado, uma com a chave privada e outra sem.

    $subjectName = "EncryptedVirtualNetworks" $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"} [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret")) Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert

3.  Instalar os certificados em cada um dos seus hosts hyper-v 

    PS c:\> dir c:\$subjectname.*


        Directory: C:\


    Nome do modo LastWriteTime comprimento
    ----                -------------         ------ ----
    -a---9/22/2017 às 16H: 54 EncryptedVirtualNetworks.cer 543 - a---9/22/2017 às 16H: 54 1706 EncryptedVirtualNetworks.pfx

4.  Instalando em um host Hyper-V

    $server = "Server01"

    $subjectname = "EncryptedVirtualNetworks" copy c:\$SubjectName.* \\$server\c$ invoke-command - computername $server - ArgumentList $subjectname, "segredo" {param ([string] $SubjectName, [string] $Secret) $certFullPath = "c: \$SubjectName.cer "

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

5.  Repita para cada servidor em seu ambiente.<p>Depois de repetição para cada servidor, você deve ter um certificado instalado na raiz e meu repositório de cada host Hyper-V. 

6.  Verifique se a instalação do certificado.<p>Verificar os certificados, verificando o conteúdo do meu e repositórios de certificados de raiz:

    PS C:\> pssession insira Server1

    [Server1]: PS C:\> cert://localmachine/my de get-childitem, cert://localmachine/root |? {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Assunto da impressão digital
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\root

    Assunto da impressão digital
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

7.  Anote a impressão digital.<p>Você deve Anote a impressão digital pois você precisará dela para criar o objeto de credencial de certificado no controlador de rede.

## <a name="step-2-create-the-certificate-credential"></a>Etapa 2. Criar a credencial de certificado

Depois de instalar o certificado em cada um dos hosts do Hyper-V conectados ao controlador de rede, agora você deve configurar o controlador de rede para usá-lo.  Para fazer isso, você deve criar um objeto de credencial que contém a impressão digital do certificado do computador com os módulos do PowerShell no controlador de rede instalados. 


    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  
    
    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller
    
    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force

>[!TIP]
>Você pode reutilizar essa credencial para cada rede virtual criptografada, ou você pode implantar e usar um certificado exclusivo para cada locatário.


## <a name="step-3-configuring-a-virtual-network-for-encryption"></a>Etapa 3. Configurar uma rede Virtual para criptografia

Essa etapa pressupõe que você já tiver criado um nome de rede virtual "Minha rede" e contém pelo menos uma sub-rede virtual.  Para obter informações sobre como criar redes virtuais, consulte [Create, Delete ou Update redes virtuais de locatário](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md).

>[!NOTE]
>Ao se comunicar com outra VM na mesma sub-rede, se seu atualmente conectada ou conectado em um momento posterior, o tráfego é criptografado automaticamente.

1.  Recuperar os objetos de rede Virtual e as credenciais do controlador de rede

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork" $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"

2.  Adicione uma referência para as credenciais de certificado e habilitar a criptografia em sub-redes individuais

    $vnet.properties.EncryptionCredential = $certcred

    # <a name="replace-the-subnets-index-with-the-value-corresponding-to-the-subnet-you-want-encrypted"></a>Substitua o índice de sub-redes com o valor correspondente para a sub-rede que você deseja que seja criptografado.  
    # <a name="repeat-for-each-subnet-where-encryption-is-needed"></a>Repita para cada sub-rede em que a criptografia é necessária
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true

3.  Colocar o objeto de rede Virtual atualizado no controlador de rede

    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force


_**Parabéns!**_ Quando terminar depois de concluir essas etapas. 


## <a name="next-steps"></a>Próximas etapas



