---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, instalar, instalação
contributor: maertendMSFT
ms.product: w10
author: maertendMSFT
title: Configuração do servidor OpenSSH para Windows
ms.openlocfilehash: ed424c33c4cd2c19a9b5e985ab6083bcbcb9fbdc
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546267"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configuração do servidor OpenSSH para Windows 10 1809 e Server 2019

Este tópico aborda a configuração específica do Windows para o servidor OpenSSH (sshd). 

A OpenSSH mantém a documentação detalhada das opções de configuração online em [OpenSSH.com](https://www.openssh.com/manual.html), que não está duplicada neste conjunto de documentação. 

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Configurando o shell padrão para OpenSSH no Windows

O Shell de comando padrão fornece a experiência que um usuário vê ao se conectar ao servidor usando SSH. O Windows padrão inicial é o Shell de comando do Windows (cmd. exe). O Windows também inclui o PowerShell e o bash, e os shells de comando de terceiros também estão disponíveis para o Windows e podem ser configurados como o shell padrão para um servidor.

Para definir o Shell de comando padrão, primeiro confirme se a pasta de instalação do OpenSSH está no caminho do sistema. Para o Windows, a pasta de instalação padrão é SystemDrive: WindowsDirectory\System32\openssh. Os comandos a seguir mostram a configuração de caminho atual e adicionam a pasta de instalação padrão do OpenSSH a ela. 

Shell de comando | Comando a ser usado
------------- | -------------- 
Comando | path
PowerShell | $env:p Ho

Configurar o shell ssh padrão é feito no registro do Windows adicionando o caminho completo ao executável do Shell para Computer\HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH no valor da cadeia de caracteres DefaultShell. 

Por exemplo, o seguinte comando do PowerShell define o shell padrão como PowerShell. exe:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshd_config"></a>Configurações do Windows no sshd_config 

No Windows, o sshd lê os dados de configuração do%ProgramData%\ssh\sshd_config por padrão ou um arquivo de configuração diferente pode ser especificado Iniciando sshd. exe com o parâmetro-f.
Se o arquivo estiver ausente, o sshd gerará um com a configuração padrão quando o serviço for iniciado.

Os elementos listados abaixo fornecem a configuração específica do Windows possível por meio de entradas no sshd_config. Há outras definições de configuração possíveis no que não estão listadas aqui, pois elas são abordadas em detalhes na [documentação do Win32 OpenSSH](https://github.com/powershell/win32-openssh/wiki)online. 


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers 

O controle de quais usuários e grupos podem se conectar ao servidor é feito usando as diretivas AllowGroups, AllowUsers, DenyGroups e DenyUsers. As diretivas de permissão/negação são processadas na seguinte ordem: DenyUsers, AllowUsers, DenyGroups e finalmente AllowGroups. Todos os nomes de conta devem ser especificados em letras minúsculas. Consulte padrões em ssh_config para obter mais informações sobre padrões para curingas.

Ao configurar regras baseadas em usuário/grupo com um usuário ou grupo de domínio, use o seguinte ``` user?domain* ```formato:.
O Windows permite vários formatos para especificar entidades de segurança de domínio, mas muitos estão em conflito com padrões padrão do Linux. Por esse motivo, * é adicionado para cobrir FQDNs. Além disso, essa abordagem usa "?", em vez de @, para evitar conflitos username@host com o formato. 

Os usuários/grupos do grupo de trabalho e as contas conectadas à Internet são sempre resolvidos para o nome da conta local (sem parte do domínio, semelhante a nomes Unix padrão). Os usuários e grupos de domínio são estritamente resolvidos para [NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format) Format-domain_short_name\user_name. Todas as regras de configuração baseadas em usuário/grupo precisam aderir a esse formato.

Exemplos de usuários e grupos de domínio 

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

Para o Windows OpenSSH, os únicos métodos de autenticação disponíveis são "password" e "PublicKey".

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile 

O padrão é ". ssh/authorized_keys. ssh/authorized_keys2". Se o caminho não for absoluto, ele será levado em relação ao diretório base do usuário (ou caminho da imagem do perfil). Estendi. c:\users\user.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (suporte adicionado em v 7.7.0.0)

Essa diretiva só tem suporte com sessões SFTP. Uma sessão remota em cmd. exe não honraria isso. Para configurar um servidor chroot somente SFTP, defina ForceCommand como interno-SFTP. Você também pode configurar o SCP com chroot, implementando um shell personalizado que permitia apenas o SCP e o SFTP.

### <a name="hostkey"></a>HostKey

Os padrões são% ProgramData%/SSH/ssh_host_ecdsa_key,% ProgramData%/SSH/ssh_host_ed25519_key e% ProgramData%/SSH/ssh_host_rsa_key. Se os padrões não estiverem presentes, o sshd os gerará automaticamente em um início de serviço.

### <a name="match"></a>Correspondência

Observe que as regras de padrão nesta seção. Os nomes de usuários e grupos devem estar em letras minúsculas.

### <a name="permitrootlogin"></a>PermitRootLogin

Não aplicável no Windows. Para impedir o logon de administrador, use administradores com a diretiva DenyGroups.

### <a name="syslogfacility"></a>SyslogFacility

Se você precisar de log baseado em arquivo, use LOCAL0. Os logs são gerados em%ProgramData%\ssh\logs.
Qualquer outro valor, incluindo o valor padrão AUTH, direciona o log para o ETW. Para obter mais informações, consulte recursos de log no Windows.

### <a name="not-supported"></a>Sem suporte 

As seguintes opções de configuração não estão disponíveis na versão OpenSSH fornecida no Windows Server 2019 e no Windows 10 1809:

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

