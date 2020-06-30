---
title: Configuração do servidor OpenSSH para Windows
description: Informações de configuração sobre o servidor OpenSSH para Windows 10 1809 e Server 2019.
ms.date: 09/27/2018
ms.topic: conceptual
contributor: maertendMSFT
ms.product: windows-server
author: maertendmsft
ms.openlocfilehash: abd156936bbd26479b0fe6bb7ffb98c1dd122f8e
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469751"
---
# <a name="openssh-server-configuration-for-windows-10-1809-and-server-2019"></a>Configuração do servidor OpenSSH para Windows 10 1809 e Server 2019

Este tópico aborda a configuração específica do Windows para o Servidor OpenSSH (sshd).

O OpenSSH mantém a documentação detalhada das opções de configuração online em [OpenSSH.com](https://www.openssh.com/manual.html), que não está duplicada neste conjunto de documentação.

## <a name="configuring-the-default-shell-for-openssh-in-windows"></a>Como configurar o shell padrão para OpenSSH no Windows

O Shell de comando padrão proporciona a experiência que um usuário vê ao se conectar ao servidor usando SSH.
O Windows padrão inicial é o Shell de Comando do Windows (cmd.exe).
O Windows também inclui o PowerShell e o bash, e os shells de comando de terceiros também estão disponíveis para o Windows e podem ser configurados como o shell padrão para um servidor.

Para definir o shell de comando padrão, primeiro confirme se a pasta de instalação do OpenSSH está no caminho do sistema.
Para o Windows, a pasta de instalação padrão é SystemDrive:WindowsDirectory\System32\openssh.
Os comandos a seguir mostram a configuração de caminho atual e adicionam a pasta de instalação padrão do OpenSSH a ela.

Shell de comando | Comando a ser usado
------------- | --------------
Comando | path
PowerShell | $env:path

A configuração do shell ssh padrão é feita no Registro do Windows, adicionando o caminho completo ao executável do shell a Computer\HKEY_LOCAL_MACHINE \SOFTWARE\OpenSSH no valor da cadeia de caracteres DefaultShell.

Por exemplo, o seguinte comando do PowerShell define o shell padrão como PowerShell.exe:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## <a name="windows-configurations-in-sshd_config"></a>Windows Configurations no sshd_config

No Windows, sshd lê os dados de configuração de %programdata%\ssh\sshd_config por padrão ou um arquivo de configuração diferente pode ser especificado iniciando sshd.exe com o parâmetro-f.
Se o arquivo estiver ausente, o sshd gerará um com a configuração padrão quando o serviço for iniciado.

Os elementos listados abaixo fornecem a configuração específica do Windows possível por meio de entradas no sshd_config.
Há outras definições de configuração possíveis que não estão listadas aqui, pois são abordadas detalhadamente na documentação do [Win32 OpenSSH online](https://github.com/powershell/win32-openssh/wiki).


### <a name="allowgroups-allowusers-denygroups-denyusers"></a>AllowGroups, AllowUsers, DenyGroups, DenyUsers

O controle de quais usuários e grupos podem se conectar ao servidor é feito usando as diretivas AllowGroups, AllowUsers, DenyGroups e DenyUsers.
As diretivas allow/deny são processadas na seguinte ordem: DenyUsers, AllowUsers, DenyGroups e, por fim, AllowGroups.
Todos os nomes de conta devem ser especificados em letras minúsculas.
Veja PATTERNS em ssh_config para obter mais informações sobre padrões para curingas.

Ao configurar regras baseadas em usuário/grupo com um usuário ou grupo de domínio, use o seguinte formato: ``` user?domain* ```.
O Windows permite vários formatos para especificar entidades de segurança de domínio, mas muitos estão em conflito com os padrões do Linux.
Por esse motivo, * é adicionado para cobrir FQDNs.
Além disso, essa abordagem usa "?", em vez de @, para evitar conflitos com o formato username@host.

Os usuários/grupos do grupo de trabalho e as contas conectadas à Internet são sempre resolvidos para o nome da conta local (sem parte do domínio, semelhante a nomes Unix padrão).
Os usuários e grupos de domínio são estritamente resolvidos para o formato [NameSamCompatible](https://docs.microsoft.com/windows/desktop/api/secext/ne-secext-extended_name_format) – domain_short_name\user_name.
Todas as regras de configuração baseadas em usuário/grupo precisam aderir a esse formato.

Exemplos de usuários e grupos de domínio

```
DenyUsers contoso\admin@192.168.2.23 : blocks contoso\admin from 192.168.2.23
DenyUsers contoso\* : blocks all users from contoso domain
AllowGroups contoso\sshusers : only allow users from contoso\sshusers group
```

Exemplos de usuários e grupos de local

```
AllowUsers localuser@192.168.2.23
AllowGroups sshusers
```

### <a name="authenticationmethods"></a>AuthenticationMethods

Para o Windows OpenSSH, os únicos métodos de autenticação disponíveis são "password" e "publickey".

### <a name="authorizedkeysfile"></a>AuthorizedKeysFile

O padrão é ".ssh/authorized_keys .ssh/authorized_keys2". Se o caminho não for absoluto, ele será considerado em relação ao diretório base do usuário (ou o caminho da imagem do perfil). Ex. c:\users\user. Observe que, se o usuário pertencer ao grupo de administradores, %programdata%/ssh/administrators_authorized_keys será usado.

### <a name="chrootdirectory-support-added-in-v7700"></a>ChrootDirectory (suporte adicionado na v7.7.0.0)

Essa diretiva só tem suporte com sessões sftp. Uma sessão remota em cmd.exe não honraria isso. Para configurar um servidor chroot somente sftp, defina ForceCommand como internal-sftp. Você também pode configurar o scp com chroot implementando um shell personalizado que permita apenas scp e sftp.

### <a name="hostkey"></a>HostKey

Os padrões são %programdata%/ssh/ssh_host_ecdsa_key, %programdata%/ssh/ssh_host_ed25519_key, %programdata%/ssh/ssh_host_dsa_key e %programdata%/ssh/ssh_host_rsa_key. Se os padrões não estiverem presentes, o sshd os gerará automaticamente no início do serviço.

### <a name="match"></a>Correspondência

Observe essas regras de padrão nesta seção. Os nomes de usuários e grupos devem estar em letras minúsculas.

### <a name="permitrootlogin"></a>PermitRootLogin

Não aplicável no Windows. Para impedir o logon de administrador, use administradores com a diretiva DenyGroups.

### <a name="syslogfacility"></a>SyslogFacility

Se você precisar de registro em log baseado em arquivo, use LOCAL0. Os logs são gerados em %programdata%\ssh\logs.
Qualquer outro valor, incluindo o valor padrão AUTH, direciona o log para o ETW. Para obter mais informações, confira Recursos de registro em log no Windows.

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

