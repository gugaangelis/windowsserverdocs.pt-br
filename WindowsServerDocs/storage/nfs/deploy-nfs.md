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
ms.openlocfilehash: ab80b6d73a40256d5935635c9afc55b7c53727d3
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63737825"
---
# <a name="deploy-network-file-system"></a>Implantar o sistema de arquivos de rede

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sistema de arquivos de rede (NFS) fornece uma solução que permite transferir arquivos entre computadores que executam o Windows Server e sistemas operacionais UNIX usando o protocolo NFS de compartilhamento de arquivos. Este tópico descreve as etapas que você deve seguir para implantar o NFS.

## <a name="whats-new-in-network-file-system"></a>O que há de novo no sistema de arquivos de rede

Aqui está o que mudou de NFS no Windows Server 2012:

- **Suporte para NFS versão 4.1**. Esta versão do protocolo inclui os seguintes aprimoramentos.
  - Firewalls de navegação é mais fácil, melhorando a acessibilidade.
  - Oferece suporte a RPCSEC\_protocolo GSS, fornecendo segurança mais forte e permitindo que clientes e servidores negociar a segurança.
  - Dá suporte à semântica de arquivo do UNIX e Windows.
  - Tira proveito das implantações de servidor de arquivos em cluster.
  - Dá suporte a procedimentos compostos de compatível com WAN.

- **Módulo NFS para o Windows PowerShell**. A disponibilidade dos cmdlets internos do NFS torna mais fácil automatizar várias operações. Os nomes de cmdlet são consistentes com outros Windows PowerShell cmdlets (usando verbos como "Get" e "Set"), tornando mais fácil para os usuários familiarizados com o Windows PowerShell para aprender a usar novos cmdlets.
- **Melhorias no gerenciamento de NFS**. Um novo console de gerenciamento centralizado baseado na interface do usuário simplifica a configuração e gerenciamento de SMB e NFS compartilhamentos, cotas, triagens de arquivo e classificação, além de gerenciar servidores de arquivos clusterizados.
- **Aprimoramentos de mapeamento de identidade**. Novo suporte de interface do usuário e cmdlets do Windows PowerShell baseado em tarefas para configurar o mapeamento de identidade, que permite aos administradores configurar rapidamente uma fonte de mapeamento de identidade e, em seguida, criar identidades mapeadas individuais para os usuários. Aprimoramentos tornam mais fácil para os administradores configurem um compartilhamento para acesso a vários protocolos em NFS e SMB.
- **Reestruturação do modelo de recurso de cluster**. Essa melhoria traz a consistência entre o modelo de recurso de cluster de NFS do Windows e servidores de protocolo SMB e simplifica a administração. Para servidores NFS que têm muitos compartilhamentos, a rede de recursos e o número de chamadas WMI necessárias falharem ao longo de um volume que contém que um grande número de compartilhamentos NFS é reduzido.
- **Integração com o Gerenciador de chave de retomada**. O Gerenciador de chave de retomada é um componente que controla o estado do sistema de arquivo e o servidor de arquivos e permite que os servidores de protocolo Windows SMB e NFS fazer failover sem interromper os clientes ou aplicativos de servidor que armazenam seus dados no servidor de arquivos. Essa melhoria é um componente-chave do recurso de disponibilidade contínua do servidor de arquivos executando o Windows Server 2012.

## <a name="scenarios-for-using-network-file-system"></a>Cenários para usar o sistema de arquivos de rede

NFS dá suporte a um ambiente misto de sistemas operacionais Windows e UNIX. Os cenários de implantação a seguir são exemplos de como você pode implantar um servidor de arquivos continuamente disponível do Windows Server 2012 usando NFS.

### <a name="provision-file-shares-in-heterogeneous-environments"></a>Compartilhamentos de arquivos de provisão em ambientes heterogêneos

Esse cenário se aplica às organizações com ambientes heterogêneos que consistem em Windows e outros sistemas operacionais, como o cliente de baseado em Linux ou UNIX computadores. Com esse cenário, você pode fornecer acesso a vários protocolos ao mesmo compartilhamento de arquivos por meio de protocolos de SMB e NFS. Geralmente, quando você implanta um servidor de arquivos do Windows nesse cenário, você deseja facilitar a colaboração entre usuários no Windows e computadores baseados em UNIX. Quando um compartilhamento de arquivos é configurado, ela é compartilhada com protocolos de SMB e NFS, com usuários de Windows acessando seus arquivos através do protocolo SMB, e os usuários em computadores baseados em UNIX normalmente acessam seus arquivos através do protocolo NFS.

Para este cenário, você deve ter uma configuração de fonte de mapeamento de identidade válida. Windows Server 2012 dá suporte aos seguintes armazenamentos de mapeamento de identidade:

- Arquivo de mapeamento
- Serviços de Domínio do Active Directory (AD DS)
- LDAP de 2307 compatível com RFC armazena como o Active Directory Lightweight Directory Services (AD LDS)
- Servidor de nome de mapeamento (UNM) do usuário

### <a name="provision-file-shares-in-unix-based-environments"></a>Compartilhamentos de arquivos de provisão em ambientes baseados no UNIX

Nesse cenário, os servidores de arquivos do Windows são implantados em um ambiente predominantemente baseados em UNIX para fornecer acesso aos compartilhamentos de arquivos NFS para computadores cliente baseados em UNIX. Uma opção de UNIX usuário UUUA (acesso) inicialmente foi implementada para compartilhamentos NFS no Windows Server 2008 R2 para que o mapeamento de contas Windows servidores podem ser usados para armazenar dados NFS sem criar UNIX para Windows. UUUA permite aos administradores rapidamente provisionar e implantar o NFS sem a necessidade de configurar o mapeamento de conta. Quando habilitado para NFS, UUUA cria identificadores de segurança personalizada (SIDs) para representar os usuários não mapeados. Contas de usuário mapeadas usam identificadores de segurança padrão Windows (SIDs) e os usuários não mapeados usam SIDs de NFS personalizado.

## <a name="system-requirements"></a>Requisitos de sistema

O Server for NFS pode ser instalado em qualquer versão do Windows Server 2012. Você pode usar NFS com computadores baseados em UNIX que estão executando um servidor NFS ou cliente NFS se essas implementações de cliente e servidor NFS está em conformidade com um dos seguintes especificações de protocolo:

1. Especificação do protocolo NFS versão 4.1 (conforme definido no RFC [5661](https://tools.ietf.org/html/rfc5661))
2. Especificação do protocolo NFS versão 3 (conforme definido no RFC [1813](https://tools.ietf.org/html/rfc1813))
3. Especificação do protocolo NFS versão 2 (conforme definido no RFC [1094](https://tools.ietf.org/html/rfc1094))

## <a name="deploy-nfs-infrastructure"></a>Implantar a infraestrutura do NFS

Você precisa implantar os computadores a seguir e conectá-los em uma rede local (LAN):

- Um ou mais computadores com Windows Server 2012, no qual você irá instalar os dois serviços principais para componentes NFS: Server for NFS e Client for NFS. Você pode instalar esses componentes no mesmo computador ou em computadores diferentes.
- Um ou mais computadores UNIX que executam o servidor NFS e o software cliente do NFS. O computador com UNIX que está executando o servidor NFS hospeda um compartilhamento de arquivos NFS ou exportação, que pode é acessada por um computador que está executando o Windows Server 2012 como um cliente usando o Client for NFS. Você pode instalar o software de cliente e servidor NFS no mesmo computador com base em UNIX ou em diferentes computadores baseados em UNIX, conforme desejado.
- Um controlador de domínio em execução no nível funcional do Windows Server 2008 R2. O controlador de domínio fornece informações de autenticação de usuário e o mapeamento para o ambiente do Windows.
- Quando um controlador de domínio não está implantado, você pode usar um servidor do serviço de informação de rede (NIS) para fornecer informações de autenticação de usuário para o ambiente UNIX. Ou, se preferir, você pode usar a senha e o grupo de arquivos que são armazenados no computador que está executando o serviço de mapeamento de nome de usuário.

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>Instalar o sistema de arquivos de rede no servidor com o Gerenciador do servidor

1. No Assistente de Adição de Funções e Recursos, em Funções do Servidor, selecione **Serviços de Arquivo e Armazenamento** se ainda não tiverem sido instalados.
2. Sob **arquivo e iSCSI serviços**, selecione **servidor de arquivos** e **Server for NFS**. Selecione **adicionar recursos** para incluir os recursos selecionados do NFS.
3. Selecione **instalar** para instalar os componentes NFS no servidor.

### <a name="install-network-file-system-on-the-server-with-windows-powershell"></a>Instalar o sistema de arquivos de rede no servidor com o Windows PowerShell

1. Inicie o Windows PowerShell. Clique com o botão direito do mouse no ícone do PowerShell na barra de tarefas e selecione **Executar como Administrador**.
2. Execute os seguintes comandos do Windows PowerShell:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## <a name="configure-nfs-authentication"></a>Configurar a autenticação do NFS

Ao usar a versão 4.1 do NFS e protocolos do NFS versão 3.0, você tem as seguintes opções de autenticação e segurança.

- RPCSEC\_GSS
  - **Krb5**. Usa o protocolo Kerberos versão 5 para autenticar usuários antes de conceder acesso ao compartilhamento de arquivos.
  - **Krb5i**. Usa o protocolo Kerberos versão 5 para autenticar com a integridade de verificação (somas de verificação), que verifica que os dados não foram alterados.
  - **Krb5p** protocolo usa Kerberos versão 5, o que autentica o tráfego NFS com criptografia para privacidade.
- AUTH\_SYS

Você também pode escolher para não usar a autorização de servidor (AUTH\_SYS), que oferece a opção para habilitar o acesso de usuários não mapeados. Ao usar o acesso de usuários não mapeados, você pode especificar para permitir o acesso de usuários não mapeados por UID / GID, que é o padrão ou permitir o acesso anônimo.

Instruções para configurar a autenticação de NFS no é discutido na seção a seguir.

## <a name="create-an-nfs-file-share"></a>Criar um compartilhamento de arquivos NFS

Você pode criar um compartilhamento de arquivos NFS usando cmdlets do Gerenciador do servidor ou NFS do Windows PowerShell.

### <a name="create-an-nfs-file-share-with-server-manager"></a>Criar um compartilhamento de arquivos NFS com o Gerenciador do servidor

1. Faça logon no servidor como membro do grupo Administradores local.
2. O Gerenciador do Servidor será iniciado automaticamente. Se ele não for iniciado automaticamente, selecione **inicie**, digite **servermanager.exe**e, em seguida, selecione **Gerenciador do servidor**.
3. À esquerda, selecione **serviços de arquivo e armazenamento**e, em seguida, selecione **compartilhamentos**.
4. Selecione **para criar um compartilhamento de arquivos, inicie o Assistente de novo compartilhamento**.
5. No **Selecionar perfil** página, selecione **compartilhamento de NFS – rápido** ou **compartilhamento NFS - avançado**, em seguida, selecione **Avançar**.
6. Sobre o **local de compartilhamento** página, selecione um servidor e um volume e selecione **próxima**.
7. Sobre o **nome do compartilhamento** página, especifique um nome para o novo compartilhamento e selecione **próxima**.
8. Sobre o **autenticação** , especifique o método de autenticação que você deseja usar para este compartilhamento.
9. Sobre o **permissões de compartilhamento** página, selecione **Add**e, em seguida, especifique o host, o grupo de clientes ou o netgroup, você deseja conceder permissão para o compartilhamento.
10. Na **permissões**, configure o tipo de controle de acesso que você deseja que os usuários têm e, em seguida, selecione **Okey**.
11. Sobre o **confirmação** página, examine a configuração e selecione **criar** para criar o compartilhamento de arquivos NFS.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes do Windows PowerShell

O seguinte cmdlet do Windows PowerShell também pode criar um compartilhamento de arquivos NFS (onde `nfs1` é o nome do compartilhamento e `C:\\shares\\nfsfolder` é o caminho do arquivo):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>Problema conhecido
Versão 4.1 do NFS permite que os nomes de arquivo a ser criado ou copiados usando caracteres ilegais. Se você tentar abrir os arquivos com o editor vi, ele mostra como sendo corrompido. Você não pode salvar o arquivo do vi, renomear, movê-lo ou alterar as permissões. Evite usar caracteres illigal.
