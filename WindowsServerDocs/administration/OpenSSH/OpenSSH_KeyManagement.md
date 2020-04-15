---
ms.date: 09/27/2018
ms.topic: conceptual
contributor: maertendMSFT
author: maertendmsft
title: Configuração do servidor OpenSSH para Windows
ms.product: windows-server
ms.openlocfilehash: defb8875ca73c0d08fb0fa0764ed3ddf9003e09c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852039"
---
# <a name="openssh-key-management"></a>Gerenciamento de chaves OpenSSH

A maioria das autenticações em ambientes Windows é feita com um par nome de usuário-senha.
Isso funciona bem para sistemas que compartilham um domínio comum. Ao trabalhar entre domínios, como entre sistemas locais e hospedados na nuvem, torna-se mais difícil.

Por comparação, os ambientes do Linux geralmente usam pares chave pública/chave privada para direcionar a autenticação.
O OpenSSH inclui ferramentas para ajudar a dar suporte a isso, especificamente:

* __ssh-keygen__ para gerar chaves seguras
* __ssh-agent__ e __SSH-add__ para armazenar chaves privadas com segurança
* __scp__ e __sftp__ para copiar arquivos de chave pública com segurança durante o uso inicial de um servidor

Este documento apresenta uma visão geral de como usar essas ferramentas no Windows para começar a usar a autenticação de chave com o SSH. Se você não está familiarizado com o gerenciamento de chaves SSH, é altamente recomendável examinar o [documento do NIST IR 7966](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf) intitulado "Security of Interactive and Automated Access Management Using Secure Shell (SSH)" (Segurança de gerenciamento de acesso interativo e automatizado usando SSH (Secure Shell)).

## <a name="about-key-pairs"></a>Sobre pares de chaves

Os pares de chaves referem-se aos arquivos de chave pública e privada usados por determinados protocolos de autenticação. 

A autenticação de chave pública SSH usa algoritmos de criptografia assimétrica para gerar dois arquivos de chave, um "privado" e outro "público". Os arquivos de chave privada são equivalentes a uma senha e devem ser protegidos em todas as circunstâncias. Se alguém adquirir sua chave privada, poderá fazer logon como você para qualquer servidor SSH ao qual você tenha acesso. A chave pública é o que é colocado no servidor SSH e pode ser compartilhada sem comprometer a chave privada.

Ao usar a autenticação de chave com um servidor SSH, o servidor SSH e o cliente comparam a chave pública ao nome de usuário fornecido em relação à chave privada. Se a chave pública não puder ser validada em relação à chave privada do lado do cliente, a autenticação falhará. 

A autenticação multifator pode ser implementada com pares de chaves, exigindo que uma frase secreta seja fornecida quando o par de chaves for gerado (confira a geração de chave abaixo). Durante a autenticação, o usuário é solicitado a informar a senha, que é usada junto com a presença da chave privada no cliente SSH para autenticar o usuário. 

## <a name="host-key-generation"></a>Geração de chave de host

As chaves públicas têm requisitos específicos de ACL que, no Windows, são equivalentes a permitir somente o acesso a administradores e sistemas. Para tornar isso mais fácil: 

* O módulo OpenSSHUtils do PowerShell foi criado para definir as ACLs de chave corretamente e deve ser instalado no servidor
* No primeiro uso de sshd, o par de chaves do host será gerado automaticamente. Se o ssh-agent estiver em execução, as chaves serão adicionadas automaticamente ao repositório local. 

Para facilitar a autenticação de chave com um servidor SSH, execute os seguintes comandos em um prompt do PowerShell com privilégios elevados:

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

Como não há nenhum usuário associado ao serviço sshd, as chaves de host são armazenadas em \ProgramData\ssh.


## <a name="user-key-generation"></a>Geração de chave do usuário

Para usar a autenticação baseada em chave, primeiro você precisa gerar alguns pares de chaves pública/privada para seu cliente. No PowerShell ou cmd, use ssh-keygen para gerar alguns arquivos de chave.

```powershell
cd ~\.ssh\
ssh-keygen
```

Isso deve exibir algo semelhante ao seguinte (em que "username" é substituído pelo seu nome de usuário)

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

Você pode pressionar Enter para aceitar o padrão ou especificar um caminho em que você deseja que suas chaves sejam geradas. Neste ponto, você será solicitado a usar uma frase secreta para criptografar seus arquivos de chave privada.
A frase secreta funciona com o arquivo de chave para fornecer autenticação de 2 fatores. Para este exemplo, estamos deixando a frase secreta vazia. 

```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in C:\Users\username\.ssh\id_ed25519.
Your public key has been saved in C:\Users\username\.ssh\id_ed25519.pub.
The key fingerprint is: 
SHA256:OIzc1yE7joL2Bzy8!gS0j8eGK7bYaH1FmF3sDuMeSj8 username@server@LOCAL-HOSTNAME

The key's randomart image is:
+--[ED25519 256]--+
|        .        |
|         o       |
|    . + + .      |
|   o B * = .     |
|   o= B S .      |
|   .=B O o       |
|  + =+% o        |
| *oo.O.E         |
|+.o+=o. .        |
+----[SHA256]-----+
```

Agora você tem um par de chaves pública/privada ED25519 (os arquivos .pub são chaves públicas e o restante são chaves privadas):

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

Lembre-se de que os arquivos de chave privada são o equivalente a uma senha e devem ser protegido da mesma maneira que você protege sua senha.
Para ajudar com isso, use ssh-agent para armazenar com segurança as chaves privadas em um contexto de segurança do Windows, associado ao seu logon do Windows. Para fazer isso, inicie o serviço ssh-agent como administrador e use ssh-adicionar para armazenar a chave privada. 

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

Depois de concluir essas etapas, sempre que uma chave privada for necessária para autenticação desse cliente, o ssh-agent recuperará automaticamente a chave privada local e a passará para o cliente SSH.

> [!NOTE]
> É altamente recomendável fazer backup da chave privada em um local seguro e, em seguida, excluí-la do sistema local *depois* de adicioná-la ao ssh-agent.
> A chave privada não pode ser recuperada do agente.
> Se você perder o acesso à chave privada, precisará criar um novo par de chaves e atualizar a chave pública em todos os sistemas com os quais interage.

## <a name="deploying-the-public-key"></a>Como implantar a chave pública

Para usar a chave de usuário criada acima, a chave pública precisa ser colocada no servidor em um arquivo de texto chamado *authorized_keys* em users\username\.ssh\. As ferramentas OpenSSH incluem SCP, que é um utilitário de transferência de arquivo seguro, para ajudar com isso.

Para mover o conteúdo da sua chave pública (~\.ssh\id_ed25519.pub) para um arquivo de texto chamado authorized_keys em ~\.ssh\ no servidor/host.

Este exemplo usa a função Repair-AuthorizedKeyPermissions no módulo OpenSSHUtils que foi instalado anteriormente no host nas instruções acima.

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to copy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

Essas etapas concluem a configuração necessária para usar a autenticação baseada em chave com o SSH no Windows.
Depois disso, o usuário pode se conectar ao host sshd de qualquer cliente que tenha a chave privada.

