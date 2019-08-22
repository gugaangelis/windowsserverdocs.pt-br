---
title: Gerenciando protocolos SSL/TLS e conjuntos de codificação para AD FS
description: Perguntas frequentes sobre o AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 951e7d74a3370d9863d747e349d7fe701615e225
ms.sourcegitcommit: 2e38b26742f3b16c153170d6f5219c020a8e9383
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69896814"
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>Gerenciando protocolos SSL/TLS e conjuntos de codificação para AD FS
A documentação a seguir fornece informações sobre como desabilitar e habilitar determinados protocolos TLS/SSL e conjuntos de codificação que são usados pelo AD FS

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>Pacotes TLS/SSL, SChannel e Cipher no AD FS

A segurança de camada de transporte (TLS) e o protocolo SSL (SSL) são protocolos que fornecem comunicações seguras.  O Serviços de Federação do Active Directory (AD FS) usa esses protocolos para comunicações.  Atualmente, existem várias versões desses protocolos.

Schannel é um SSP (Provedor de Suporte de Segurança) que implementa os protocolos de autenticação padrão da Internet SSL, TLS e DTLS. A Interface SSPI é uma API usada por sistemas Windows para executar funções relacionadas à segurança, incluindo autenticação. A SSPI funciona como uma interface comum para vários SSPS, incluindo o SSP Schannel.

Um pacote de criptografia é um pacote de algoritmos criptográficos. A implementação do SSP do Schannel dos protocolos TLS/SSL usam algoritmos de um conjunto de codificação para criar chaves e criptografar informações. Um pacote de criptografia especifica um algoritmo para cada uma das seguintes tarefas:

- Troca de chaves
- Criptografia em massa
- Autenticação de mensagem

AD FS usa Schannel. dll para executar suas interações de comunicações seguras.  Atualmente AD FS dá suporte a todos os protocolos e conjuntos de codificação que têm suporte do Schannel. dll.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Gerenciando os protocolos TLS/SSL e conjuntos de codificação
> [!IMPORTANT]
> Esta seção contém etapas que explicam como modificar o registro. No entanto, problemas sérios podem ocorrer se você modificar o registro incorretamente. Portanto, certifique-se de seguir estas etapas com cuidado. 
> 
> Lembre-se de que a alteração das configurações de segurança padrão para SCHANNEL pode interromper ou impedir a comunicação entre determinados clientes e servidores.  Isso ocorrerá se a comunicação segura for necessária e não tiver um protocolo para negociar comunicações com o.
> 
> Se você estiver aplicando essas alterações, elas deverão ser aplicadas a todos os seus servidores de AD FS em seu farm.  Depois de aplicar essas alterações, uma reinicialização é necessária.

No dia atual e na idade, a proteção dos servidores e a remoção de pacotes de criptografia antigos ou fracos estão se tornando uma prioridade importante para muitas organizações.  Os pacotes de software estão disponíveis para testar seus servidores e fornecer informações detalhadas sobre esses protocolos e conjuntos de pacotes.  Para permanecer em conformidade ou obter classificações seguras, remover ou desabilitar protocolos mais fracos ou pacotes de codificação se tornou um necessário.  O restante deste documento fornecerá orientações sobre como habilitar ou desabilitar determinados protocolos e conjuntos de codificação.

As chaves do registro abaixo estão localizadas no mesmo local:  HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  Use o regedit ou o PowerShell para habilitar ou desabilitar esses protocolos e conjuntos de codificação.

![Local do Registro](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>Habilitar e desabilitar SSL 2,0
Use as seguintes chaves do registro e seus valores para habilitar e desabilitar o SSL 2,0.

### <a name="enable-ssl-20"></a>Habilitar SSL 2,0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ servidor] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ servidor] "DisabledByDefault" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ cliente] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ cliente] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-ssl-20"></a>Desabilitar SSL 2,0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ servidor] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ servidor] "DisabledByDefault" = DWORD: 00000001 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ cliente] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ cliente] "DisabledByDefault" = DWORD: 00000001

### <a name="using-powershell-to-disable-ssl-20"></a>Usando o PowerShell para desabilitar o SSL 2,0

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>Habilitar e desabilitar SSL 3,0
Use as seguintes chaves do registro e seus valores para habilitar e desabilitar o SSL 3,0.

### <a name="enable-ssl-30"></a>Habilitar o SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ servidor] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ servidor] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ cliente] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ cliente] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-ssl-30"></a>Desabilitar SSL 3,0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ servidor] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ servidor] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ cliente] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ cliente] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>Usando o PowerShell para desabilitar o SSL 3,0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>Habilitar e desabilitar o TLS 1,0
Use as seguintes chaves do registro e seus valores para habilitar e desabilitar o TLS 1,0.

> [!IMPORTANT]
> Desabilitar o TLS 1,0 interromperá o WAP para AD FS confiança.  Se você desabilitar o TLS 1,0, deverá habilitar a autenticação forte para seus aplicativos.  Consulte [habilitar autenticação forte](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>Habilitar TLS 1,0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ servidor] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ servidor] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ cliente] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ cliente] "DisabledByDefault" = DWORD: 00000000 

### <a name="disable-tls-10"></a>Desabilitar TLS 1,0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ servidor] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ servidor] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ cliente] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ cliente] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-tls-10"></a>Usando o PowerShell para desabilitar o TLS 1,0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>Habilitar e desabilitar o TLS 1,1
Use as seguintes chaves do registro e seus valores para habilitar e desabilitar o TLS 1,1.

### <a name="enable-tls-11"></a>Habilitar TLS 1,1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ servidor] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ servidor] "DisabledByDefault" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ cliente] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ cliente] "DisabledByDefault" = DWORD: 00000000

### <a name="disable-tls-11"></a>Desabilitar TLS 1,1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ servidor] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ servidor] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ cliente] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ cliente] "DisabledByDefault" = DWORD: 00000001 

### <a name="using-powershell-to-disable-tls-11"></a>Usando o PowerShell para desabilitar o TLS 1,1

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>Habilitar e desabilitar o TLS 1,2

Use as seguintes chaves do registro e seus valores para habilitar e desabilitar o TLS 1,2.

### <a name="enable-tls-12"></a>Habilitar TLS 1,2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ servidor] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ servidor] "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ cliente] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ cliente] "DisabledByDefault" = DWORD: 00000000

### <a name="disable-tls-12"></a>Desabilitar TLS 1,2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ servidor] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ servidor] "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ cliente] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ cliente] "DisabledByDefault" = DWORD: 00000001

### <a name="using-powershell-to-disable-tls-12"></a>Usando o PowerShell para desabilitar o TLS 1,2

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="enable-and-disable-rc4"></a>Habilitar e desabilitar o RC4 

Use as seguintes chaves do registro e seus valores para habilitar e desabilitar o RC4.  As chaves do registro do conjunto de codificação estão localizadas aqui:

- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![Local do Registro](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>Habilitar RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled" = DWORD: 00000001 

### <a name="disable-rc4"></a>Desabilitar RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Habilitado" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Habilitado" = DWORD: 00000000 

### <a name="using-powershell"></a>Usando o PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>Habilitando ou desabilitando conjuntos de codificação adicionais

Você pode desabilitar determinadas codificações específicas removendo-as de HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\00010002 

![Local do Registro](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

Para habilitar um conjunto de codificação, adicione seu valor de cadeia de caracteres à chave de valor de várias cadeias de caracteres do functions.  Por exemplo, se quisermos habilitar TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521, vamos adicioná-lo à cadeia de caracteres.

Para obter uma lista completa de pacotes de criptografia com suporte, consulte [pacotes de codificação em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).  Este documento fornece uma tabela de pacotes que são habilitados por padrão e os que têm suporte, mas não são habilitados por padrão.  Para priorizar os conjuntos de codificação, consulte [priorizando conjuntos de codificação Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx).

## <a name="enabling-strong-authentication-for-net-applications"></a>Habilitando a autenticação forte para aplicativos .NET
Os aplicativos .NET Framework 3.5/4.0/4.5. x podem alternar o protocolo padrão para TLS 1,2 habilitando a chave do registro SchUseStrongCrypto.  Essa chave do registro forçará os aplicativos .NET a usarem o TLS 1,2.

> [!IMPORTANT]
> Para AD FS no Windows Server 2016 e no Windows Server 2012 R2, você precisa usar a chave .NET Framework 4.0/4.5. x:  HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\. NETFramework\v4.0.30319


Para o .NET Framework 3,5, use a seguinte chave do registro:

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\\. NETFramework\v2.0.50727] "SchUseStrongCrypto" = DWORD: 00000001

Para o .NET Framework 4.0/4.5. x, use a seguinte chave do registro: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\. NETFramework\v4.0.30319 "SchUseStrongCrypto" = DWORD: 00000001

![Autenticação forte](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>Informações adicionais

- [Conjuntos de codificação em TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)
- [Conjuntos de codificação TLS no Windows 8.1](https://msdn.microsoft.com/library/windows/desktop/mt767781.aspx)
- [Priorizando conjuntos de codificação Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx)
- [Falando em codificações e outras enigmáticos de língua](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
