---
title: "Gerenciamento de protocolos SSL/TLS e pacotes de codificação do AD FS"
description: Perguntas frequentes para o AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 23055a1b727e4f1a6b1ccafdea9a410c6021c432
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/13/2018
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>Gerenciamento de protocolos SSL/TLS e pacotes de codificação do AD FS
A documentação a seguir fornece informações sobre como desabilitar e habilitar determinados protocolos TLS/SSL e pacotes que são usados pelo AD FS de codificação

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS/SSL, SChannel e pacotes de codificação no AD FS

O Transport Layer Security (TLS) e Secure Sockets Layer (SSL) são protocolos que fornecem para comunicação segura.  Serviços de Federação do Active Directory usa esses protocolos para comunicações.  Hoje existem várias versões desses protocolos.

Schannel é um Security Support Provider (SSP) que implementa os protocolos de autenticação padrão SSL, TLS e DTLS Internet. A Interface de provedor de suporte de segurança (SSPI) é uma API usada pelos sistemas do Windows para executar funções relacionadas à segurança, incluindo a autenticação. A SSPI funciona como uma interface comum para vários provedores de suporte de segurança (SSPs), incluindo o Schannel SSP.

Um conjunto de codificação é um conjunto de algoritmos criptográficos. A implementação de SSP schannel dos protocolos TLS/SSL use os algoritmos de um conjunto de codificação para criar chaves e criptografar informações. Um conjunto de codificação Especifica um algoritmo para cada uma das seguintes tarefas:

- Troca de chaves
- Criptografia em massa
- Autenticação de mensagem

AD FS usa Schannel para executar suas interações de comunicação seguro.  No momento do AD FS dá suporte a todos os protocolos e pacotes de codificação que são suportados pelo Schannel.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Gerenciando os protocolos TLS/SSL e pacotes de codificação
> [!IMPORTANT]
> Esta seção contém etapas que descrevem como modificar o registro. No entanto, problemas sérios poderão ocorrer se você modificar o Registro incorretamente. Portanto, certifique-se de que você siga estas etapas com cuidado. 
> 
> Lembre-se de que alterar as configurações de segurança padrão para SCHANNEL pode quebrar ou impedir as comunicações entre determinados clientes e servidores.  Isso ocorrerá se comunicação segura é necessária e não têm um protocolo negociar comunicações com.
> 
> Se você estiver aplicando essas alterações, elas devem ser aplicadas a todos os servidores do AD FS em seu farm.  Depois de aplicar essas alterações uma reinicialização é necessária.

No dia de hoje e idade, hardening seus servidores e removendo mais antigas ou pacotes de codificação fracas está se tornando das principais prioridades para muitas organizações.  Pacotes de software estão disponíveis que irá testar seus servidores e fornecem informações detalhadas sobre esses protocolos e pacotes.  Para continuar compatível ou obter classificações seguras, removendo ou desabilitando protocolos mais fracos ou pacotes de codificação tornou essencial.  O restante deste documento fornece orientação sobre como habilitar ou desabilitar determinados protocolos e pacotes de codificação.

As chaves do Registro abaixo estão localizadas no mesmo local: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  Usar regedit ou o PowerShell para habilitar ou desabilitar esses protocolos e pacotes de codificação.

![Local do registro](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>Habilitar e desabilitar SSL 2.0
Use as seguintes chaves do registro e seus valores para habilitar e desabilitar o SSL 2.0.

### <a name="enable-ssl-20"></a>Habilitar SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-ssl-20"></a>Desabilitar SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SL 2.0\Server] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault" = DWORD: 00000001 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault" = DWORD: 00000001

### <a name="using-powershell-to-disable-ssl-20"></a>Usando o PowerShell para desabilitar o SSL 2.0

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>Habilitar e desabilitar SSL 3.0
Use as seguintes chaves do registro e seus valores para habilitar e desabilitar SSL 3.0.

### <a name="enable-ssl-30"></a>Habilitar SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-ssl-30"></a>Desabilitar SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>Usando o PowerShell para desabilitar SSL 3.0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>Habilitar e desabilitar TLS 1.0
Use as seguintes chaves do registro e seus valores para habilitar e desabilitar TLS 1.0.

> [!IMPORTANT]
> Desabilitar TLS 1.0 romperá o WAP com confiança do AD FS.  Se você desabilitar TLS 1.0, você deve habilitar a autenticação forte para seus aplicativos.  Consulte [habilitar a autenticação forte](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>Habilitar TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-tls-10"></a>Desabilitar TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-tls-10"></a>Usando o PowerShell para desabilitar TLS 1.0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>Habilitar e desabilitar TLS 1.1
Use as seguintes chaves do registro e seus valores para habilitar e desabilitar o protocolo TLS 1.1.

### <a name="enable-tls-11"></a>Habilitar TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "DisabledByDefault" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "DisabledByDefault" = DWORD: 00000000

### <a name="disable-tls-11"></a>Desabilitar TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-tls-11"></a>Usando o PowerShell para desabilitar o protocolo TLS 1.1

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>Habilitar e desabilitar TLS 1.2

Use as seguintes chaves do registro e seus valores para habilitar e desabilitar TLS 1.2.

### <a name="enable-tls-12"></a>Habilitar TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault" = DWORD: 00000000

### <a name="disable-tls-12"></a>Desabilitar TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault" = DWORD: 00000001

### <a name="using-powershell-to-disable-tls-12"></a>Usando o PowerShell para desabilitar TLS 1.2

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="enable-and-disable-rc4"></a>Habilitar e desabilitar RC4 

Use as seguintes chaves do registro e seus valores para habilitar e desabilitar RC4.  Chaves de registro desse pacote de codificação estão localizadas aqui:

- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![Local do registro](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>Habilitar RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled" = DWORD: 00000001 

### <a name="disable-rc4"></a>Desabilitar RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled" = DWORD: 00000000 

### <a name="using-powershell"></a>Usando o PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>Habilitando ou desabilitando pacotes de codificação adicionais

Você pode desabilitar determinadas criptografias específicas removendo-os da HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\0010002 

![Local do registro](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

Para habilitar um conjunto de codificação, nisso é o valor de cadeia de caracteres para a chave de valor de cadeia de caracteres de várias funções.  Por exemplo, se quisermos habilitar TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521, em seguida, nós seria adicioná-lo à cadeia de caracteres.

Para uma lista completa de codificação com suporte, veja pacotes [pacotes de codificação em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx).  Este documento fornece uma tabela de pacotes que são habilitados por padrão e aqueles que são compatíveis, mas não é habilitado por padrão.  Priorizar os pacotes de codificação veja [priorizando pacotes de codificação Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx).

## <a name="enabling-strong-authentication-for-net-applications"></a>Habilitar a autenticação de alta segurança para aplicativos .NET
Os aplicativos do .NET Framework 3.5/4.0/4.5.x podem alternar o protocolo padrão para TLS 1.2, permitindo a chave de registro SchUseStrongCrypto.  Essa chave do registro forçará aplicativos .NET para usar TLS 1.2.

> [!IMPORTANT]
> Do AD FS no Windows Server 2012 R2 e no Windows Server 2016 você precisa usar a chave do .NET Framework 4.0/4.5: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\. NETFramework\v4.0.30319


Para o .NET Framework 3.5, use a seguinte chave do registro:

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\. NETFramework\v2.0.50727] "SchUseStrongCrypto" = DWORD: 00000001

Para o .NET Framework 4.0/4.5 use a seguinte chave do registro: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\. NETFramework\v4.0.30319 "SchUseStrongCrypto" = DWORD: 00000001

![Autenticação forte](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>Informações adicionais

- [Pacotes de codificação em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx)
- [Pacotes de codificação TLS no Windows 8.1](https://msdn.microsoft.com/en-us/library/windows/desktop/mt767781.aspx)
- [Priorizando pacotes de codificação Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx)
- [Falando em criptografias e outros tongues enigmático](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
