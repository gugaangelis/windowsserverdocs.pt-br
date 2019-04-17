---
title: Implantar o sistema de arquivos de rede
description: Descreve como implantar o sistema de arquivos de rede.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2f3671283720d515cd3e3e609d98e02343c15892
ms.sourcegitcommit: fcc26ec5a2cc73b59c5752377b39c070d288655e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/19/2018
ms.locfileid: "8976682"
---
# Implantar o sistema de arquivos de rede

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sistema NFS (Network File) oferece uma solução que permite que você transfira arquivos entre computadores que executam o Windows Server e sistemas operacionais UNIX usando o protocolo NFS de compartilhamento de arquivos. Este tópico descrevem as etapas que você deve seguir para implantar o NFS.

## Novidades no sistema de arquivos de rede

Aqui está o que mudou para NFS no Windows Server 2012:

- **Suporte para NFS versão 4.1**. Esta versão do protocolo inclui os seguintes aprimoramentos.
  - Navegar firewalls é mais fácil, melhorando a acessibilidade.
  - Suporta o protocolo RPCSEC\_GSS, fornecendo segurança mais forte e permitindo que os clientes e servidores negociar a segurança.
  - Dá suporte à semântica de arquivo UNIX e Windows.
  - Tira proveito das implantações de servidor de arquivos em cluster.
  - Dá suporte a procedimentos compostos WAN amigável.

- **Módulo NFS para o Windows PowerShell**. A disponibilidade de cmdlets internos do NFS torna mais fácil automatizar várias operações. Os nomes de cmdlet são consistentes com outros cmdlets do Windows PowerShell (usando verbos como "Obter" e "Definir"), tornando mais fácil para os usuários familiarizado com o Windows PowerShell para aprender a usar novos cmdlets.
- **Melhorias de gerenciamento de NFS**. Um novo console de gerenciamento centralizado baseado na interface do usuário simplifica a configuração e gerenciamento de SMB e NFS compartilhamentos, cotas, triagens de arquivo e classificação, além de gerenciar servidores de arquivos em cluster.
- **Melhorias de mapeamento de identidade**. Novo suporte de interface do usuário e baseada em tarefa cmdlets do Windows PowerShell para configurar o mapeamento de identidade, que permite que os administradores configurem rapidamente uma fonte de mapeamento de identidade e, em seguida, criar identidades mapeadas individuais para os usuários. Melhorias tornam mais fácil para os administradores configurem um compartilhamento para acesso a vários protocolos em NFS e SMB.
- **Modelo de recurso de cluster reestruturar**. Essa melhoria traz a consistência entre o modelo de recurso de cluster para o Windows NFS e servidores do protocolo SMB e simplifica a administração. Para servidores NFS que têm muitos compartilhamentos, a rede de recurso e o número de chamadas WMI necessária falharem ao longo de um volume que contém que um grande número de compartilhamentos NFS é reduzido.
- **Integração com o Gerenciador de chave de retomada**. O Gerenciador de chave de retomada é um componente que rastreia o servidor de arquivos e o estado do sistema de arquivo e permite que os servidores de protocolo SMB do Windows e NFS failover sem interromper clientes ou aplicativos de servidor que armazenam seus dados no servidor de arquivos. Essa melhoria é um componente fundamental da funcionalidade do servidor de arquivos que executam o Windows Server 2012 disponibilidade contínua.

## Cenários para usar o sistema de arquivos de rede

NFS dá suporte a um ambiente misto de sistemas operacionais baseados em UNIX e Windows. Os seguintes cenários de implantação são exemplos de como você pode implantar um servidor de arquivos do Windows Server 2012 continuamente disponível usando NFS.

### Compartilhamentos de arquivos de provisionamento em ambientes heterogêneos

Esse cenário se aplica às empresas com ambientes heterogêneos do Windows e outros sistemas operacionais, como cliente baseado em Linux ou UNIX computadores. Nesse cenário, você pode fornecer acesso de vários protocolo para o mesmo compartilhamento de arquivos por meio de protocolos de SMB e NFS. Normalmente, quando você implanta um servidor de arquivos do Windows neste cenário, você deseja facilitar a colaboração entre usuários no Windows e computadores baseados em UNIX. Quando um compartilhamento de arquivos é configurado, ele é compartilhado com o SMB e NFS protocolos, com os usuários do Windows acessem seus arquivos por meio do protocolo SMB, e os usuários em computadores baseados em UNIX normalmente acessam seus arquivos por meio do protocolo NFS.

Para esse cenário, você deve ter uma configuração de origem de mapeamento de identidade válida. Windows Server 2012 dá suporte aos seguintes repositórios de mapeamento de identidade:

- Arquivo de mapeamento
- Serviços de Domínio do Active Directory (AD DS)
- RFC 2307 compatível LDAP armazena como o Active Directory Lightweight Directory Services (AD LDS)
- Servidor de mapeamento de nome (UNM) de usuário

### Compartilhamentos de arquivos de provisionamento em ambientes baseados em UNIX

Nesse cenário, os servidores de arquivos do Windows são implantados em um ambiente basicamente com base em UNIX para fornecer acesso a compartilhamentos de arquivos NFS para computadores cliente baseados em UNIX. Uma opção de acesso de usuário de UNIX não mapeado (UUUA) foi implementada inicialmente para compartilhamentos NFS no Windows Server 2008 R2 para que o mapeamento de contas Windows servidores podem ser usados para armazenar dados NFS sem criar UNIX para Windows. UUUA permite que os administradores provisionar rapidamente e implantar NFS sem precisar configurar mapeamento de conta. Quando habilitada para NFS, UUUA cria identificadores de segurança personalizado (SIDs) para representar os usuários não mapeados. Contas de usuário mapeadas usam identificadores de segurança do Windows (SIDs) e os usuários não mapeados usam personalizado SIDs NFS.

## Requisitos de sistema

Servidor para NFS pode ser instalado em qualquer versão do Windows Server 2012. Você pode usar o NFS com UNIX com base em computadores que executam um servidor NFS ou um cliente NFS se essas implementações de servidor e cliente NFS em conformidade com um dos seguintes especificações de protocolo:

1. NFS versão 4.1 especificação do protocolo (como definido em RFC [5661](https://tools.ietf.org/html/rfc5661))
2. NFS versão 3 especificação do protocolo (como definido em RFC [1813](https://tools.ietf.org/html/rfc1813))
3. NFS versão 2 especificação do protocolo (como definido em RFC [1094](https://tools.ietf.org/html/rfc1094))

## Implantar a infraestrutura NFS

Você precisa implantar os computadores a seguir e conectá-los em uma rede local (LAN):

- Um ou mais computadores que executam o Windows Server 2012, nos quais você instalará os dois principais componentes dos serviços de NFS: servidor para NFS e cliente para NFS. Você pode instalar esses componentes no mesmo computador ou em computadores diferentes.
- Um ou mais computadores com UNIX que executam o servidor NFS e software do cliente NFS. O computador com base em UNIX que está executando o servidor NFS hospeda um compartilhamento de arquivos NFS ou exportação, que é acessada por um computador que está executando o Windows Server 2012 como um cliente usando o cliente para NFS. Você pode instalar o software de cliente e servidor NFS no mesmo computador com base em UNIX ou em computadores diferentes com base em UNIX, conforme desejado.
- Um controlador de domínio em execução no nível funcional do Windows Server 2008 R2. O controlador de domínio fornece informações de autenticação de usuário e o mapeamento para o ambiente do Windows.
- Quando um controlador de domínio não for implantado, você pode usar um servidor de serviço de informação de rede (NIS) para fornecer informações de autenticação do usuário para o ambiente do UNIX. Ou, se preferir, você pode usar arquivos de senha e de grupo que são armazenados no computador que está executando o serviço de mapeamento de nome de usuário.

### Instalar o sistema de arquivos de rede no servidor com o Gerenciador do servidor

1. No assistente Adicionar funções e recursos, em funções de servidor, selecione o **arquivo e serviços de armazenamento** se ele já não tiver sido instalado.
2. Em **de arquivo e iSCSI Services**, selecione o **Servidor de arquivos** e **servidor para NFS**. Selecione **Adicionar recursos** para incluir recursos NFS selecionados.
3. Selecione **instalação** para instalar os componentes NFS no servidor.

### Instalar o sistema de arquivos de rede no servidor com o Windows PowerShell

1. Inicie o Windows PowerShell. Clique com botão direito no ícone do PowerShell na barra de tarefas e selecionar **Executar como administrador**.
2. Execute os seguintes comandos do Windows PowerShell:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## Configurar a autenticação NFS

Ao usar a versão do NFS 4.1 e os protocolos de versão 3.0 NFS, você tem as seguintes opções de autenticação e segurança.

- RPCSEC\_GSS
  - **Krb5**. Usa o protocolo Kerberos versão 5 para autenticar os usuários antes de conceder acesso ao compartilhamento de arquivos.
  - **Krb5i**. Usa o protocolo Kerberos versão 5 para autenticar com integridade de verificação (somas de verificação), que verifica se os dados não foi alterados.
  - **Krb5p** Usa o protocolo Kerberos versão 5, que autentica tráfego NFS com criptografia de privacidade.
- AUTH\_SYS

Você também pode optar por não usar autorização do servidor (AUTH\_SYS), que oferece a opção para habilitar o acesso de usuário não mapeado. Ao usar o acesso de usuário não mapeado, você pode especificar para permitir o acesso de usuário não mapeado por UID / GID, que é o padrão ou permitir acesso anônimo.

Instruções para configurar a autenticação NFS no abordados na seção a seguir.

## Criar um compartilhamento de arquivos NFS

Você pode criar um compartilhamento de arquivos NFS usando os cmdlets do Gerenciador do servidor ou NFS do Windows PowerShell.

### Criar um compartilhamento de arquivos NFS com o Gerenciador do servidor

1. Faça logon no servidor como um membro do grupo de administradores local.
2. Gerenciador do servidor será iniciado automaticamente. Se ele não iniciar automaticamente, selecione **Iniciar**, digite **servermanager.exe**e, em seguida, selecione o **Gerenciador do servidor**.
3. À esquerda, selecione o **arquivo e serviços de armazenamento**e selecione **compartilhamentos**.
4. Selecione **para criar um compartilhamento de arquivos, inicie o Assistente de novo compartilhamento**.
5. Na página **Selecionar perfil** , **Compartilhamento NFS – rápida** ou **compartilhamento NFS - avançada**, selecione e **Avançar**.
6. Na página **Local de compartilhamento** , selecione um servidor e um volume e selecione **Avançar**.
7. Na página **Nome do compartilhamento** , especifique um nome para o novo compartilhamento e selecione **Avançar**.
8. Na página de **autenticação** , especifique o método de autenticação que você deseja usar para esse compartilhamento.
9. Na página **Permissões de compartilhamento** , selecione **Adicionar**e, em seguida, especifique o host, o grupo de clientes ou netgroup que você deseja conceder permissão para o compartilhamento.
10. Em **permissões**, configure o tipo de controle de acesso que você deseja que os usuários tenham e selecione **Okey**.
11. Na página de **confirmação** , examine a configuração e selecione **criar** para criar o compartilhamento de arquivos NFS.

### Comandos equivalentes do Windows PowerShell

O seguinte cmdlet do Windows PowerShell também pode criar um compartilhamento de arquivos NFS (onde `nfs1` é o nome do compartilhamento e `C:\\shares\\nfsfolder` é o caminho do arquivo):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### Problema conhecido
NFS versão 4.1 permite que os nomes de arquivo a ser criado ou copiado usando caracteres inválidos. Se você tentar abrir os arquivos com editor vi, ele mostra como sendo corrompidos. Você não pode salvar o arquivo de vi, renomear, movê-lo ou alterar as permissões. Evite usar caracteres illigal.
