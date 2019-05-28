---
title: Visão geral de sistema de arquivos de rede
description: Explica o que é o sistema de arquivos de rede.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9b0d339df588c784f8fe46f7dd0e6ce2975d0c48
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034657"
---
# <a name="network-file-system-overview"></a>Visão geral de sistema de arquivos de rede

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve o serviço de função do sistema de arquivos de rede e recursos incluídos com a função de servidor Serviços de arquivo e armazenamento no Windows Server. Sistema de arquivos de rede (NFS) fornece uma solução para empresas com ambientes heterogêneos que incluem o Windows e computadores não Windows de compartilhamento de arquivos.

## <a name="feature-description"></a>Descrição do recurso

Usando o protocolo NFS, você pode transferir arquivos entre computadores que executam o Windows e outros sistemas operacionais não Windows, como Linux ou UNIX.

NFS no Windows Server inclui o servidor para NFS e Client for NFS. Um computador executando o Windows Server pode usar o Server for NFS para atuar como um servidor de arquivos NFS para outros computadores cliente não-Windows. Cliente NFS permite que um computador baseado em Windows executando o Windows Server para acessar arquivos armazenados em um servidor NFS não - Windows.

## <a name="windows-and-windows-server-versions"></a>Versões do Windows e Windows Server

Windows dá suporte a várias versões do cliente NFS e servidor, dependendo da versão do sistema operacional e a família. 

| Sistemas operacionais | Versões do servidor NFS |Versões do cliente NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/D | NFSv2, NFSv3 |
| Windows Server 2008, Windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFSv2, NFSv3, NFSv4.1  | NFSv2, NFSv3 |

## <a name="practical-applications"></a>Aplicações práticas

Aqui estão algumas maneiras que você pode usar o NFS:

- Use um servidor de arquivos NFS do Windows para fornecer acesso a vários protocolos ao mesmo compartilhamento de arquivos por meio de protocolos SMB e NFS de várias plataformas clientes.
- Implante um servidor de arquivos NFS do Windows em um ambiente de sistema de operacional predominantemente não-Windows para fornecer acesso de computadores aos compartilhamentos de arquivos NFS de cliente não-Windows.
- Migre aplicativos de um sistema operacional para outro, armazenando os dados em compartilhamentos de arquivos acessíveis por meio de protocolos SMB e NFS.

## <a name="new-and-changed-functionality"></a>Funcionalidade nova e alterada

Funcionalidades novas e alteradas no sistema de arquivos de rede incluem suporte para o NFS versão 4.1 e aprimoradas de implantação e capacidade de gerenciamento. Para obter informações sobre a funcionalidade nova ou alterada no Windows Server 2012, examine a tabela a seguir:

|Recurso/funcionalidade|Novo ou atualizado|Descrição|
|---|---|---|
|[NFS versão 4.1](#nfs-version-41)|Novo|Aumentar a segurança, desempenho e interoperabilidade em comparação com o NFS versão 3.|
|[Infraestrutura NFS](#nfs-infrastructure)|Atualizado|Melhora a implantação e a capacidade de gerenciamento e aumenta a segurança.|
|[Disponibilidade contínua do NFS versão 3](#nfs-version-3-continuous-availability)|Atualizado|Melhora a disponibilidade contínua em clientes do NFS versão 3.|
|[Melhorias de implantação e a capacidade de gerenciamento](#deployment-and-manageability-improvements)|Atualizado|Permite que você facilmente implante e gerencie NFS com novos cmdlets do Windows PowerShell e um novo provedor WMI.|

## <a name="nfs-version-41"></a>NFS versão 4.1

NFS versão 4.1 implementa todos os aspectos necessários, além de alguns dos aspectos opcionais, de [RFC 5661](https://tools.ietf.org/html/rfc5661):

- **Sistema de arquivos de pseudo**, um sistema de arquivos que separa a físico e lógico de namespace e é compatível com o NFS versão 3 e NFS versão 2. Um alias é fornecido para o sistema de arquivo exportado, o que faz parte do sistema de arquivos pseudo.
- **Composta RPCs** combinam operações relevantes e reduzir a conversa.
- **Sessões e sessão entroncamento** permite apenas uma semântica e permite disponibilidade contínua e melhor desempenho ao utilizar várias redes entre clientes NFS 4.1 e o servidor NFS.

## <a name="nfs-infrastructure"></a>Infraestrutura NFS

Melhorias para infraestrutura geral de NFS no Windows Server 2012 estão detalhadas abaixo:

- O **chamada de procedimento remoto (RPC) / representação externa de dados (XDR)** infra-estrutura de transporte, fornecida pelo protocolo de rede WinSock, está disponível para o Server for NFS e Client for NFS. Isso substitui a Interface de dispositivo de transporte (TDI), oferece um suporte melhor e oferece melhor escalabilidade e RSS Receive Side Scaling ().
- O **porta RPC multiplexador** recurso é compreendida pelo firewall (menos portas para gerenciar) e simplifica a implantação do NFS.
- **Caches ajustados automaticamente e pools de threads** são recursos de gerenciamento da nova infra-estrutura do RPC/XDR são dinâmicos, ajuste automaticamente os caches e com base na carga de trabalho de pools de threads de recurso. Isso remove completamente a incerteza envolvidas ao ajustar os parâmetros, fornecendo o desempenho ideal, assim que o NFS é implantado.
- **Novo Kerberos privacidade implementação opções de autenticação e** com a adição de suporte a Kerberos privacidade (Krb5p) junto com o krb5 existente e as opções de autenticação krb5i.
- **Módulo do PowerShell do Windows de mapeamento de identidade** cmdlets torná-lo mais fácil de gerenciar o mapeamento de identidade, configurar o Active Directory Lightweight Directory Services (AD LDS) e configurar passwd UNIX e Linux e arquivos simples.
- **Ponto de montagem de volume** permite que você acesse os volumes montados em um compartilhamento NFS com NFS versão 4.1.
- O **multiplexação de porta** recurso é compatível com a porta RPC multiplexador (porta 2049), que é compatível com firewall e simplifica a implantação de NFS.

## <a name="nfs-version-3-continuous-availability"></a>Disponibilidade contínua do NFS versão 3

Clientes do NFS versão 3 podem ter failovers planejados rápidos e transparentes com mais disponibilidade e tempo de inatividade reduzido. O processo de failover é mais rápida para clientes do NFS versão 3 porque:

- Infraestrutura de cluster agora permite que um recurso por nome de rede em vez de um recurso por compartilhamento, o que melhora significativamente o tempo de failover dos recursos.
- Caminhos de failover em um servidor NFS são ajustados para melhorar o desempenho.
- Registro de caractere curinga em um servidor NFS não for mais necessário, e os failovers são mais ajustados.
- Notificações de Status Monitor (NSM) de rede são enviadas após um processo de failover e os clientes não precisam mais esperar para tempos limite de TCP para reconectar-se para a falha no servidor.

Observe que o Server for NFS dá suporte a failover transparente somente quando for iniciado manualmente, geralmente durante a manutenção planejada. Se ocorrer um failover não planejado, clientes NFS perderão suas conexões. O Server for NFS também não tem nenhuma integração com o filtro de chave de retomada. Isso significa que, se um aplicativo local ou uma sessão SMB tentar acessar o mesmo arquivo que um cliente NFS está acessando imediatamente após um failover planejado, o cliente NFS poderá perder suas conexões (o failover transparente não terá êxito).

## <a name="deployment-and-manageability-improvements"></a>Melhorias de implantação e a capacidade de gerenciamento

Implantando e gerenciando o NFS foi aprimorado das seguintes maneiras:

- Quarenta mais novos cmdlets do Windows PowerShell torna mais fácil de configurar e gerenciar compartilhamentos de arquivos NFS. Para obter mais informações, consulte [Cmdlets NFS no Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).
- Mapeamento de identidade é aprimorado com um repositório de mapeamento de local de arquivo simples e novos cmdlets do Windows PowerShell para configurar o mapeamento de identidade.
- A interface gráfica do usuário é mais fácil de usar o Gerenciador do servidor.
- O novo provedor de WMI versão 2 está disponível para facilitar o gerenciamento.
- A porta multiplexador (porta RPC 2049) é compatível com firewall e simplifica a implantação do NFS.

## <a name="server-manager-information"></a>Informações sobre o Gerenciador do Servidor

No Gerenciador de servidores - ou o mais recente [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) -use o assistente Adicionar funções e recursos para adicionar o servidor para o serviço de função NFS (sob o arquivo e iSCSI função de serviços). Para obter informações gerais sobre como instalar recursos, consulte [Instalar ou desinstalar funções, serviços de função ou recursos](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). Servidor para as ferramentas do NFS incluem os serviços para o snap-in MMC de sistema de arquivos de rede para gerenciar o Server for NFS e NFS componentes do cliente para. Usando o snap-in, você pode gerenciar o servidor para os componentes NFS instalada no computador. O Server for NFS também contém várias ferramentas de administração de linha de comando do Windows:

- **Montar** monta um compartilhamento NFS (também conhecido como uma exportação) remoto localmente e mapeia-os para uma letra de unidade local no computador cliente Windows.
- **Nfsadmin** gerencia as configurações do servidor para NFS e Client para componentes NFS.
- **Nfsshare** define as configurações de compartilhamento NFS para pastas compartilhadas usando o Server for NFS.
- **Nfsstat** exibe ou redefine as estatísticas de chamadas recebidas pelo servidor para NFS.
- **Showmount** exibe os sistemas de arquivos montados exportados com o Server for NFS.
- **Desmonte** remove unidades montados em NFS.

NFS no Windows Server 2012 apresenta o módulo NFS para o Windows PowerShell com vários novos cmdlets especificamente para NFS. Esses cmdlets oferecem uma maneira fácil de automatizar tarefas de gerenciamento de NFS. Para obter mais informações, consulte [cmdlets NFS no Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).

## <a name="additional-information"></a>Informações adicionais

A tabela a seguir fornece recursos adicionais para avaliar o NFS.

|Tipo de conteúdo|Referências|
|---|---|
|Implantação|[Implantar o Network File System](deploy-nfs.md)|
|Operações|[Cmdlets NFS no Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|Tecnologias relacionadas|[Armazenamento no Windows Server](../storage.md)|