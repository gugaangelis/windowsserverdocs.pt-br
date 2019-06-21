---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, instalar, configurar
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: Configuração do servidor do OpenSSH para Windows
ms.openlocfilehash: 7eff3d3e1af67c9daf7a68c67c3609c0ee89fc93
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280031"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configuração do servidor do OpenSSH para Windows 10 1809 e Server # 2019

Este tópico aborda a configuração do Windows específica para OpenSSH Server (sshd). 

OpenSSH mantém a documentação detalhada de opções de configuração on-line na [OpenSSH.com](https://www.openssh.com/manual.html), que não é ser duplicado neste conjunto de documentação. 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Configurando o shell padrão para OpenSSH no Windows

O shell de comando padrão fornece a experiência de que um usuário vê ao se conectar ao servidor usando o SSH. O padrão inicial do Windows é o shell de comando do Windows (cmd.exe). Windows também incluem o PowerShell e Bash e shells de comando de terceiros também estão disponíveis para Windows e podem ser configurados como o shell padrão para um servidor.

Para definir o padrão de shell de comando, confirme que a pasta de instalação do OpenSSH está no caminho do sistema. Para Windows, a pasta de instalação padrão é SystemDrive:WindowsDirectory\System32\openssh. Os comandos a seguir mostra a configuração atual do caminho e adicione a pasta de instalação padrão OpenSSH a ele. 

Shell de comando | Comando a ser usado
------------- | -------------- 
Comando | path
PowerShell | $env:path

Configurando o padrão ssh shell é feita no registro do Windows, adicionando o caminho completo para o executável para Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH no valor de cadeia de caracteres DefaultShell do shell. 

Por exemplo, o seguinte comando do Powershell define o shell padrão ser PowerShell.exe:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshdconfig"></a>Configurações do Windows em sshd_config 

No Windows, sshd lê dados de configuração de %programdata%\ssh\sshd_config por padrão, ou um arquivo de configuração diferente pode ser especificado por iniciando sshd.exe com o parâmetro -f.
Se o arquivo estiver ausente, sshd irá gerar um com a configuração padrão quando o serviço é iniciado.

Os elementos listados abaixo fornecem possíveis de configuração específicas do Windows por meio de entradas no sshd_config. Há outras definições de configuração possíveis em que não estão listadas aqui, pois eles serão discutidos em detalhes no online [OpenSSH Win32 documentação](https://github.com/powershell/win32-openssh/wiki). 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

Controlar quais usuários e grupos podem se conectar ao servidor é feita usando as diretivas AllowGroups, AllowUsers, DenyGroups e DenyUsers. As diretivas de permitir/negar são processadas na seguinte ordem: DenyUsers, AllowUsers, DenyGroups e finalmente AllowGroups. Todos os nomes de conta devem ser especificados em letras minúsculas. Veja os padrões no ssh_config para obter mais informações sobre padrões para caracteres curinga.

Quando a configuração de usuário/grupo com base em regras com um usuário de domínio ou grupo, use o seguinte formato: ``` user?domain* ```.
Windows permite que vários formatos para especificar as entidades de domínio, mas muitos entram em conflito com os padrões de Linux padrão. Por esse motivo, * é adicionado para cobrir os FQDNs. Além disso, essa abordagem usa "?", em vez de @, para evitar conflitos com o username@host formato. 

Grupo de trabalho que os usuários/grupos e contas conectado à internet sempre são resolvidas ao seu nome de conta local (nenhuma parte do domínio, semelhante aos nomes padrão do Unix). Os usuários do domínio e grupos são estritamente resolvidos para [NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format) formato - domain_short_name\user_name. Todos os usuário/grupo com base em regras precisam atender a esse formato de configuração.

Exemplos para usuários de domínio e grupos 

```
DenyUsers contoso\admin@192.168.2.23 : blocks contoso\admin from 192.168.2.23
DenyUsers contoso\* : blocks all users from contoso domain
AllowGroups contoso\sshusers : only allow users from contoso\sshusers group
```

Exemplos para usuários e grupos locais 

```
AllowUsers localuser@192.168.2.23
AllowGroups sshusers
```

### <a name="authenticationmethods"></a>AuthenticationMethods 

Para Windows OpenSSH, os métodos de autenticação apenas disponíveis são "senha" e "publickey".

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

O padrão é ". SSH/authorized_keys. SSH/authorized_keys2". Se o caminho não é absoluto, ele será executado em relação ao diretório base do usuário (ou caminho de imagem de perfil). Ex. c:\users\user.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (suporte adicionado à v7.7.0.0)

Essa diretiva só é compatível com sessões de sftp. Uma sessão remota para cmd.exe não honra isso. Para configurar um servidor chroot somente sftp, defina ForceCommand como interno sftp. Você também pode configurar scp com chroot, Implementando um shell personalizado que permite somente scp e sftp.

### <a name="hostkey"></a>HostKey

Os padrões são % programdata%/ssh/ssh_host_ecdsa_key, %programdata%/ssh/ssh_host_ed25519_key e % programdata%/ssh/ssh_host_rsa_key. Se os padrões não estiverem presentes, sshd gera automaticamente no início de um serviço.

### <a name="match"></a>Correspondência

Observe que o padrão de regras nesta seção. Nomes de usuário e grupo devem estar em letra minúscula.

### <a name="permitrootlogin"></a>PermitRootLogin

Não é aplicável no Windows. Para evitar o logon de administrador, use os administradores com DenyGroups diretiva.

### <a name="syslogfacility"></a>SyslogFacility

Se você precisar de registro em log com base em arquivo, use LOCAL0. Os logs são gerados sob % programdata%\ssh\logs.
Qualquer outro valor, incluindo o valor padrão AUTH direciona o registro em log para o ETW. Para obter mais informações, consulte recursos de registro em log no Windows.

### <a name="not-supported"></a>Sem suporte 

As opções de configuração a seguir não estão disponíveis na versão OpenSSH que acompanha o Windows Server 2019 e Windows 10 1809:

* AcceptEnv
* AllowStreamLocalForwarding
* AuthorizedKeysCommand
* AuthorizedKeysCommandUser
* AuthorizedPrincipalsCommand
* AuthorizedPrincipalsCommandUser
* Compactação
* ExposeAuthInfo
* GSSAPIAuthentication
* GSSAPICleanupCredentials
* GSSAPIStrictAcceptorCheck
* HostbasedAcceptedKeyTypes
* HostbasedAuthentication
* HostbasedUsesNameFromPacketOnly
* IgnoreRhosts
* IgnoreUserKnownHosts
* KbdInteractiveAuthentication
* KerberosAuthentication
* KerberosGetAFSToken
* KerberosOrLocalPasswd
* KerberosTicketCleanup
* PermitTunnel
* PermitUserEnvironment
* PermitUserRC
* PidFile
* PrintLastLog
* RDomain
* StreamLocalBindMask
* StreamLocalBindUnlink
* StrictModes
* X11DisplayOffset
* X11Forwarding
* X11UseLocalhost
* XAuthLocation

