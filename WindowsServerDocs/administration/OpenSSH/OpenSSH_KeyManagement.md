---
ms.date: 09/27/2018
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, instalar, configurar
contributor: maertendMSFT
author: maertendMSFT
title: Configuração do servidor do OpenSSH para Windows
ms.product: w10
ms.openlocfilehash: 3e2981cbbdfe34bf28a77d5121bff0b663e0d3bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837147"
---
# <a name="openssh-key-management"></a>Gerenciamento de chave OpenSSH

A maioria dos autenticação em ambientes do Windows é feita com um par de nome de usuário-senha.
Isso funciona bem para sistemas que compartilham um domínio comum. Ao trabalhar em domínios, tais como entre sistemas locais e hospedados na nuvem, fica mais difícil.

Por comparação, ambientes Linux geralmente usam pares de chave pública/chave privada para autenticação de unidade.
OpenSSH inclui ferramentas para ajudar a dar suporte a isso, especificamente:

* __SSH-keygen__ para gerar as chaves seguras
* __SSH-agent__ e __ssh-add__ para armazenar com segurança as chaves privadas
* __SCP__ e __sftp__ para copiar arquivos de chave pública de forma segura durante o uso inicial de um servidor

Este documento fornece uma visão geral de como usar essas ferramentas no Windows para começar a usar a autenticação de chave SSH. Se você estiver familiarizado com o gerenciamento de chaves SSH, é altamente recomendável revisar [7966 IR de documento do NIST](http://nvlpubs.nist.gov/nistpubs/ir/2015/NIST.IR.7966.pdf) intitulado "Segurança de interativa e automatizada acesso gerenciamento usando Secure Shell (SSH)".

## <a name="about-key-pairs"></a>Sobre pares de chaves

Consultem os arquivos de chave públicos e privados que são usados por determinados protocolos de autenticação de pares de chaves. 

Autenticação de chave pública SSH usa algoritmos de criptografia assimétricos para gerar dois arquivos de chave – um "privado" e outro "public". Os arquivos de chave privados são o equivalente de uma senha e devem ser protegidos em todas as circunstâncias. Se alguém adquirir sua chave privada, ele podem fazer logon como você a qualquer servidor SSH que você tem acesso ao. A chave pública é o que é colocado no servidor SSH e pode ser compartilhado sem comprometer a chave privada.

Ao usar a autenticação de chave com um servidor SSH, o servidor SSH e o cliente comparam a chave pública do nome de usuário fornecido com a chave privada. Se a chave pública não pode ser validada em relação a chave privada do lado do cliente, a autenticação falhará. 

A autenticação multifator pode ser implementada com pares de chaves, exigindo que uma frase secreta fornecido quando o par de chaves é gerado (consulte abaixo de geração de chave). Durante a autenticação que do usuário é solicitado a frase secreta, que é usada junto com a presença da chave privada no cliente SSH para autenticar o usuário. 

## <a name="host-key-generation"></a>Geração de chave de host

As chaves públicas têm requisitos específicos de ACL que, no Windows, serão iguais para somente permitir o acesso a administradores e do sistema. Para tornar isso mais fácil, 

* O módulo do OpenSSHUtils PowerShell foi criado para definir a chave de ACLs adequadamente e deve ser instalado no servidor
* No primeiro uso do sshd, o par de chaves para o host será gerado automaticamente. Se o ssh-agent está em execução, as chaves serão adicionadas automaticamente para o armazenamento local. 

Para facilitar a autenticação de chave com um servidor SSH, execute os seguintes comandos em um prompt elevado do PowerShell:

```powershell

# Install the OpenSSHUtils module to the server. This will be valuable when deploying user keys.
Install-Module -Force OpenSSHUtils -Scope AllUsers

# Start the ssh-agent service to preserve the server keys
Start-Service ssh-agent

# Now start the sshd service
Start-Service sshd
```

Como não há nenhum usuário associado com o serviço sshd, as chaves de host são armazenadas em \ProgramData\ssh.


## <a name="user-key-generation"></a>Geração de chave de usuário

Para usar a autenticação baseada em chave, você precisa primeiro gerar alguns pares de chaves pública/privada para seu cliente. No PowerShell ou cmd, use ssh-keygen para gerar alguns arquivos de chave.

```powershell
cd ~\.ssh\
ssh-keygen
```

Isso deve exibir algo semelhante ao seguinte (em que "username" é substituída por seu nome de usuário)

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\username\.ssh\id_ed25519):
```

Você pode pressionar Enter para aceitar o padrão ou especificar um caminho onde você deseja que suas chaves a ser gerado. Neste ponto, você será solicitado a usar uma senha para criptografar seus arquivos de chave privada.
A frase secreta funciona com o arquivo de chave para fornecer autenticação de fator de 2. Neste exemplo, podemos estão deixando a senha vazia. 

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

Agora você tem um par de chaves do ED25519 pública/privada (os arquivos. pub são as chaves públicas e o restante são as chaves particulares):

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/28/2018  11:09 AM           1679 id_ed25519
-a----        9/28/2018  11:09 AM            414 id_ed25519.pub
```

Lembre-se de que os arquivos de chave privada são que o equivalente de uma senha deve ser protegido da mesma maneira que você protege a sua senha.
Para ajudar com isso, use ssh-agent para armazenar com segurança as chaves privadas dentro de um contexto de segurança do Windows, associado com seu logon do Windows. Para fazer isso, inicie o serviço ssh-agent como administrador e use ssh-add para armazenar a chave privada. 

```powershell
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

```

Depois de concluir essas etapas, sempre que uma chave privada é necessária para a autenticação desse cliente ssh-agent será automaticamente recuperar a chave privada local e passá-lo para seu cliente SSH.

> [!NOTE]
> É altamente recomendável que você faça backup da sua chave privada para um local seguro e excluí-lo do sistema local, *após* adicioná-lo ao ssh-agent.
> A chave privada não pode ser recuperada do agente.
> Se perder o acesso à chave privada, você precisaria criar um novo par de chaves e atualizar a chave pública em todos os sistemas que interagem com.

## <a name="deploying-the-public-key"></a>Implantando a chave pública

Para usar a chave de usuário que foi criada acima, a chave pública precisa ser colocada no servidor em um arquivo de texto chamado *authorized_keys* em users\username\ssh. As ferramentas do OpenSSH incluem scp, que é um utilitário de transferência de arquivos seguro, para ajudar com isso.

Para mover o conteúdo de sua chave pública (~\.ssh\id_ed25519.pub) em texto o arquivo chamado authorized_keys ~\.ssh\ no seu host/servidor.

Este exemplo usa a função de reparo AuthorizedKeyPermissions no módulo OpenSSHUtils que foi instalado anteriormente no host em que as instruções acima.

```powershell
# Make sure that the .ssh directory exists in your server's home folder
ssh user1@domain1@contoso.com mkdir C:\users\user1\.ssh\

# Use scp to opy the public key file generated previously to authorized_keys on your server
scp C:\Users\user1\.ssh\id_ed25519.pub user1@domain1@contoso.com:C:\Users\user1\.ssh\authorized_keys

# Appropriately ACL the authorized_keys file on your server  
ssh --% user1@domain1@contoso.com powershell -c $ConfirmPreference = 'None'; Repair-AuthorizedKeyPermission C:\Users\user1\.ssh\authorized_keys
```

Essas etapas forem concluídas a configuração necessária para usar a autenticação baseada em chave com o SSH no Windows.
Depois disso, o usuário pode se conectar ao host do sshd de qualquer cliente que tenha a chave privada.

