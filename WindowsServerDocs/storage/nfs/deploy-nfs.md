---
title: Implantar o sistema de arquivos de rede
description: Descreve como implantar o sistema de arquivos de rede.
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 98213b594ee3ae41196bbbbef4da34dbf280e6ad
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935765"
---
# <a name="deploy-network-file-system"></a>Implantar o sistema de arquivos de rede

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O NFS (sistema de arquivos de rede) fornece uma solução de compartilhamento de arquivos que permite transferir arquivos entre computadores que executam sistemas operacionais Windows Server e UNIX usando o protocolo NFS. Este tópico descreve as etapas que você deve seguir para implantar o NFS.

## <a name="whats-new-in-network-file-system"></a>O que há de novo no sistema de arquivos de rede

Aqui está o que mudou para o NFS no Windows Server 2012:

- **Suporte para NFS versão 4,1**. Essa versão de protocolo inclui os aprimoramentos a seguir.
  - É mais fácil navegar por firewalls, melhorando a acessibilidade.
  - Dá suporte ao \_ protocolo GSS RPCSEC, fornecendo segurança mais forte e permitindo que clientes e servidores negociem a segurança.
  - Dá suporte à semântica de arquivo do UNIX e do Windows.
  - Aproveita as implantações do servidor de arquivos clusterizado.
  - Dá suporte a procedimentos compostos amigáveis para WAN.

- **Módulo NFS para Windows PowerShell**. A disponibilidade de cmdlets NFS internos facilita a automatização de várias operações. Os nomes de cmdlet são consistentes com outros cmdlets do Windows PowerShell (usando verbos como "Get" e "set"), tornando mais fácil para os usuários familiarizados com o Windows PowerShell aprenderem a usar novos cmdlets.
- **Aprimoramentos de gerenciamento de NFS**. Um novo console de gerenciamento baseado em interface do usuário centralizado simplifica a configuração e o gerenciamento de compartilhamentos de SMB e NFS, cotas, triagens de arquivos e classificação, além de gerenciar servidores de arquivos clusterizados.
- **Aprimoramentos de mapeamento de identidade**. Novo suporte de interface do usuário e cmdlets do Windows PowerShell baseados em tarefas para configurar o mapeamento de identidade, que permite aos administradores configurar rapidamente uma origem de mapeamento de identidade e, em seguida, criar identidades mapeadas individuais para os usuários. Os aprimoramentos facilitam para os administradores a configuração de um compartilhamento para o acesso de vários protocolos no NFS e no SMB.
- **Reestruturação do modelo de recurso de cluster**. Essa melhoria traz a consistência entre o modelo de recurso de cluster para os servidores de protocolo NFS e SMB do Windows e simplifica a administração. Para servidores NFS que têm muitos compartilhamentos, a rede de recursos e o número de chamadas WMI exigiam failover de um volume contendo um grande número de compartilhamentos NFS são reduzidos.
- **Integração com o Gerenciador de chaves de retomada**. O Gerenciador de chaves de retomada é um componente que rastreia o servidor de arquivos e o estado do sistema de arquivos e permite que os servidores de protocolo Windows SMB e NFS executem failover sem interromper os clientes ou aplicativos de servidor que armazenam seus dados no servidor de arquivos. Essa melhoria é um componente fundamental da capacidade de disponibilidade contínua do servidor de arquivos que executa o Windows Server 2012.

## <a name="scenarios-for-using-network-file-system"></a>Cenários para usar o sistema de arquivos de rede

O NFS dá suporte a um ambiente misto de sistemas operacionais baseados em UNIX e Windows. Os cenários de implantação a seguir são exemplos de como você pode implantar um servidor de arquivos do Windows Server 2012 continuamente disponível usando o NFS.

### <a name="provision-file-shares-in-heterogeneous-environments"></a>Provisionar compartilhamentos de arquivos em ambientes heterogêneos

Esse cenário se aplica a organizações com ambientes heterogêneos que consistem em Windows e em outros sistemas operacionais, como computadores cliente baseados em UNIX ou Linux. Com esse cenário, você pode fornecer acesso multiprotocolo ao mesmo compartilhamento de arquivos nos protocolos SMB e NFS. Normalmente, quando você implanta um servidor de arquivos do Windows nesse cenário, você deseja facilitar a colaboração entre os usuários em computadores baseados no Windows e no UNIX. Quando um compartilhamento de arquivos é configurado, ele é compartilhado com os protocolos SMB e NFS, com usuários do Windows acessando seus arquivos por meio do protocolo SMB, e os usuários em computadores baseados em UNIX normalmente acessam seus arquivos por meio do protocolo NFS.

Para este cenário, você deve ter uma configuração de origem de mapeamento de identidade válida. O Windows Server 2012 dá suporte aos seguintes armazenamentos de mapeamento de identidade:

- Arquivo de mapeamento
- Active Directory Domain Services (AD DS)
- Armazenamentos LDAP compatíveis com RFC 2307, como Active Directory Lightweight Directory Services (AD LDS)
- Servidor Mapeamento de Nomes de Usuário (UNM)

### <a name="provision-file-shares-in-unix-based-environments"></a>Provisionar compartilhamentos de arquivos em ambientes baseados em UNIX

Nesse cenário, os servidores de arquivos do Windows são implantados em um ambiente baseado em UNIX predominantemente para fornecer acesso a compartilhamentos de arquivos NFS para computadores cliente baseados em UNIX. Uma opção de acesso de usuário UNIX (UUUA) não mapeada foi inicialmente implementada para compartilhamentos NFS no Windows Server 2008 R2 para que os servidores Windows possam ser usados para armazenar dados NFS sem criar o mapeamento de conta UNIX para Windows. O UUUA permite que os administradores provisionem e implantem o NFS rapidamente sem precisar configurar o mapeamento de conta. Quando habilitado para NFS, o UUUA cria SIDs (identificadores de segurança) personalizados para representar usuários não mapeados. As contas de usuário mapeadas usam SIDs (identificadores de segurança) padrão do Windows e os usuários não mapeados usam SIDs NFS personalizados.

## <a name="system-requirements"></a>Requisitos de sistema

O Server for NFS pode ser instalado em qualquer versão do Windows Server 2012. Você pode usar o NFS com computadores baseados em UNIX que estão executando um servidor NFS ou um cliente NFS se essas implementações de servidor NFS e cliente estiverem em conformidade com uma das seguintes especificações de protocolo:

1. Especificação do protocolo NFS versão 4,1 (conforme definido na RFC [5661](https://tools.ietf.org/html/rfc5661))
2. Especificação do protocolo NFS versão 3 (conforme definido no RFC [1813](https://tools.ietf.org/html/rfc1813))
3. Especificação do protocolo NFS versão 2 (conforme definido no RFC [1094](https://tools.ietf.org/html/rfc1094))

## <a name="deploy-nfs-infrastructure"></a>Implantar a infraestrutura NFS

Você precisa implantar os computadores a seguir e conectá-los em uma rede local (LAN):

- Um ou mais computadores que executam o Windows Server 2012 no qual você instalará os dois principais serviços para componentes NFS: Server for NFS e Client for NFS. Você pode instalar esses componentes no mesmo computador ou em computadores diferentes.
- Um ou mais computadores baseados em UNIX que estão executando o servidor NFS e o software cliente NFS. O computador baseado em UNIX que está executando o servidor NFS hospeda um compartilhamento de arquivos NFS ou uma exportação, que é acessada por um computador que está executando o Windows Server 2012 como um cliente usando o Client for NFS. Você pode instalar o software de servidor e cliente NFS no mesmo computador baseado em UNIX ou em diferentes computadores baseados em UNIX, conforme desejado.
- Um controlador de domínio em execução no nível funcional do Windows Server 2008 R2. O controlador de domínio fornece informações de autenticação de usuário e mapeamento para o ambiente do Windows.
- Quando um controlador de domínio não é implantado, você pode usar um servidor de Serviço de Informação de Rede (NIS) para fornecer informações de autenticação de usuário para o ambiente UNIX. Ou, se preferir, você pode usar os arquivos de senha e de grupo armazenados no computador que está executando o serviço de Mapeamento de Nomes de Usuário.

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>Instalar o sistema de arquivos de rede no servidor com Gerenciador do Servidor

1. No Assistente de Adição de Funções e Recursos, em Funções do Servidor, selecione **Serviços de Arquivo e Armazenamento** se ainda não tiverem sido instalados.
2. Em **arquivos e serviços iSCSI**, selecione **servidor de arquivos** e **servidor para NFS**. Selecione **Adicionar recursos** para incluir os recursos de NFS selecionados.
3. Selecione **instalar** para instalar os componentes do NFS no servidor.

### <a name="install-network-file-system-on-the-server-with-windows-powershell"></a>Instalar o sistema de arquivos de rede no servidor com o Windows PowerShell

1. Inicie o Windows PowerShell. Clique com o botão direito do mouse no ícone do PowerShell na barra de tarefas e selecione **Executar como Administrador**.
2. Execute os seguintes comandos do Windows PowerShell:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## <a name="configure-nfs-authentication"></a>Configurar a autenticação NFS

Ao usar os protocolos NFS versão 4,1 e NFS versão 3,0, você tem as seguintes opções de autenticação e segurança.

- RPCSEC \_ GSS
  - **Krb5**. Usa o protocolo Kerberos versão 5 para autenticar usuários antes de conceder acesso ao compartilhamento de arquivos.
  - **Krb5i**. Usa o protocolo Kerberos versão 5 para se autenticar com a verificação de integridade (checksums), que verifica se os dados não foram alterados.
  - **Krb5p** Usa o protocolo Kerberos versão 5, que autentica o tráfego NFS com criptografia para privacidade.
- SYS de autenticação \_

Você também pode optar por não usar a autorização do servidor (AUTH \_ sys), que oferece a opção de habilitar o acesso de usuário não mapeado. Ao usar o acesso de usuário não mapeado, você pode especificar para permitir o acesso de usuário não mapeado por UID/GID, que é o padrão, ou permitir acesso anônimo.

Instruções para configurar a autenticação NFS no abordado na seção a seguir.

## <a name="create-an-nfs-file-share"></a>Criar um compartilhamento de arquivos NFS

Você pode criar um compartilhamento de arquivos NFS usando cmdlets do Gerenciador do Servidor ou do NFS do Windows PowerShell.

### <a name="create-an-nfs-file-share-with-server-manager"></a>Criar um compartilhamento de arquivos NFS com Gerenciador do Servidor

1. Faça logon no servidor como membro do grupo Administradores local.
2. O Gerenciador do Servidor será iniciado automaticamente. Se não for iniciado automaticamente, selecione **Iniciar**, digite **servermanager.exe**e, em seguida, selecione **Gerenciador do servidor**.
3. À esquerda, selecione **serviços de arquivo e armazenamento**e, em seguida, selecione **compartilhamentos**.
4. Selecione **para criar um compartilhamento de arquivos, inicie o assistente de novo compartilhamento**.
5. Na página **selecionar perfil** , selecione **compartilhamento NFS – compartilhamento rápido** ou **NFS-avançado**e, em seguida, selecione **Avançar**.
6. Na página **local do compartilhamento** , selecione um servidor e um volume e selecione **Avançar**.
7. Na página **nome do compartilhamento** , especifique um nome para o novo compartilhamento e selecione **Avançar**.
8. Na página **autenticação** , especifique o método de autenticação que você deseja usar para esse compartilhamento.
9. Na página **permissões de compartilhamento** , selecione **Adicionar**e, em seguida, especifique o host, o grupo de clientes ou o grupo de netgroup que você deseja conceder à permissão para o compartilhamento.
10. Em **permissões**, configure o tipo de controle de acesso que você deseja que os usuários tenham e selecione **OK**.
11. Na página **confirmação** , examine sua configuração e selecione **criar** para criar o compartilhamento de arquivos NFS.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes do Windows PowerShell

O cmdlet do Windows PowerShell a seguir também pode criar um compartilhamento de arquivos NFS (onde `nfs1` é o nome do compartilhamento e `C:\\shares\\nfsfolder` é o caminho do arquivo):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>Problema conhecido
A versão 4,1 do NFS permite que os nomes de arquivo sejam criados ou copiados usando caracteres ilegais. Se você tentar abrir os arquivos com o editor vi, ele aparecerá como corrompido. Não é possível salvar o arquivo de vi, renomear, movê-lo ou alterar permissões. Evite usar caracteres ilegais.
