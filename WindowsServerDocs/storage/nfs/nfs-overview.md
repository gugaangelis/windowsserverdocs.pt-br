---
title: Visão geral de sistema de arquivos de rede
description: Explica o que é o sistema de arquivos de rede.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: f18c880dd673b17f53815a57fa2fcc66558dad71
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961318"
---
# <a name="network-file-system-overview"></a>Visão geral de sistema de arquivos de rede

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve o serviço de função do sistema de arquivos de rede e os recursos incluídos com a função de servidor de serviços de arquivo e armazenamento no Windows Server. O NFS (sistema de arquivos de rede) fornece uma solução de compartilhamento de arquivos para empresas que têm ambientes heterogêneos que incluem computadores Windows e não Windows.

## <a name="feature-description"></a>Descrição do recurso

Usando o protocolo NFS, você pode transferir arquivos entre computadores que executam o Windows e outros sistemas operacionais que não são do Windows, como Linux ou UNIX.

O NFS no Windows Server inclui o Server for NFS e o Client for NFS. Um computador que executa o Windows Server pode usar o Server for NFS para atuar como um servidor de arquivos NFS para outros computadores cliente não Windows. O Client for NFS permite que um computador baseado em Windows que executa o Windows Server Acesse arquivos armazenados em um servidor NFS não Windows.

## <a name="windows-and-windows-server-versions"></a>Versões do Windows e do Windows Server

O Windows dá suporte a várias versões do cliente e servidor NFS, dependendo da versão e da família do sistema operacional. 

| Sistemas operacionais | Versões do servidor NFS |Versões de cliente NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/D | NFSv2, NFSv3 |
| Windows Server 2008, Windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFSv2, NFSv3, NFSv 4.1  | NFSv2, NFSv3 |

## <a name="practical-applications"></a>Aplicações práticas

Aqui estão algumas maneiras de usar o NFS:

- Use um servidor de arquivos NFS do Windows para fornecer acesso multiprotocolo ao mesmo compartilhamento de arquivos em ambos os protocolos SMB e NFS de clientes de várias plataformas.
- Implante um servidor de arquivos NFS do Windows em um ambiente de sistema operacional não Windows predominantemente para fornecer aos computadores cliente não Windows acesso a compartilhamentos de arquivos NFS.
- Migre aplicativos de um sistema operacional para outro armazenando os dados em compartilhamentos de arquivos acessíveis por meio de protocolos SMB e NFS.

## <a name="new-and-changed-functionality"></a>Funcionalidade nova e alterada

A funcionalidade nova e alterada no sistema de arquivos de rede inclui suporte para a versão 4,1 do NFS e implantação e capacidade de gerenciamento aprimoradas. Para obter informações sobre a funcionalidade que é nova ou alterada no Windows Server 2012, examine a tabela a seguir:

|Recurso/funcionalidade|Novo ou atualizado|Descrição|
|---|---|---|
|[NFS versão 4,1](#nfs-version-41)|Novo|Maior segurança, desempenho e interoperabilidade em comparação com o NFS versão 3.|
|[Infraestrutura de NFS](#nfs-infrastructure)|Atualizado|Melhora a implantação e a capacidade de gerenciamento e aumenta a segurança.|
|[Disponibilidade contínua da versão 3 do NFS](#nfs-version-3-continuous-availability)|Atualizado|Melhora a disponibilidade contínua em clientes NFS versão 3.|
|[Aprimoramentos de implantação e gerenciamento](#deployment-and-manageability-improvements)|Atualizado|Permite que você implante e gerencie facilmente o NFS com novos cmdlets do Windows PowerShell e um novo provedor de WMI.|

## <a name="nfs-version-41"></a>NFS versão 4,1

A versão 4,1 do NFS implementa todos os aspectos necessários, além de alguns dos aspectos opcionais, da [RFC 5661](https://tools.ietf.org/html/rfc5661):

- **Pseudo-sistema de arquivos**, um sistema de arquivos que separa o namespace físico e lógico e é compatível com NFS versão 3 e NFS versão 2. Um alias é fornecido para o sistema de arquivos exportado, que faz parte do sistema de pseudo-arquivos.
- As **RPCs compostas** combinam operações relevantes e reduzem a informação.
- As **sessões e o entroncamento de sessão** habilitam apenas uma semântica e permitem uma disponibilidade contínua e melhor desempenho ao utilizar várias redes entre clientes NFS 4,1 e o servidor NFS.

## <a name="nfs-infrastructure"></a>Infraestrutura de NFS

As melhorias na infraestrutura geral do NFS no Windows Server 2012 são detalhadas abaixo:

- A infraestrutura de transporte do **XDR (chamada de procedimento remoto)/external (representação de dados remota** ), fornecida pelo protocolo de rede Winsock, está disponível para o servidor NFS e o cliente para NFS. Isso substitui a TDI (interface do dispositivo de transporte), oferece um suporte melhor e fornece melhor escalabilidade e RSS (recepção do lado de recebimento).
- O recurso **Multiplexador de portas RPC** é amigável para firewall (menos portas a serem gerenciadas) e simplifica a implantação de NFS.
- **Caches ajustados automaticamente e pools de threads** são recursos de gerenciamento de recursos da nova infraestrutura de RPC/XDR que são dinâmicas, ajustando caches automaticamente e pools de threads com base na carga de trabalho. Isso remove completamente as adivinhações envolvidas ao ajustar parâmetros, fornecendo um desempenho ideal assim que o NFS é implantado.
- **Novas opções de implementação e autenticação de privacidade do Kerberos** com a adição do suporte de privacidade do Kerberos (Krb5p) junto com as opções de autenticação krb5 e krb5i existentes.
- Mapeamento de identidade os cmdlets do **módulo do Windows PowerShell** facilitam o gerenciamento de mapeamento de identidade, a configuração de Active Directory Lightweight Directory Services (AD LDS) e a configuração de arquivos simples e de UNIX e Linux.
- O **ponto de montagem de volume** permite que você acesse volumes montados em um compartilhamento NFS com a versão 4,1 do NFS.
- O recurso de **multiplexação de porta** dá suporte ao Multiplexador de porta RPC (porta 2049), que é amigável para firewall e simplifica a implantação de NFS.

## <a name="nfs-version-3-continuous-availability"></a>Disponibilidade contínua da versão 3 do NFS

Os clientes da versão 3 do NFS podem ter failovers planejados rápidos e transparentes com mais disponibilidade e tempo de inatividade reduzido. O processo de failover é mais rápido para clientes do NFS versão 3 porque:

- A infraestrutura de clustering agora permite um recurso por nome de rede, em vez de um recurso por compartilhamento, o que melhora significativamente o tempo de failover dos recursos.
- Os caminhos de failover em um servidor NFS são ajustados para melhorar o desempenho.
- O registro de curingas em um servidor NFS não é mais necessário e os failovers são mais ajustados.
- As notificações de NSM (Status Monitor de rede) são enviadas após um processo de failover, e os clientes não precisam mais esperar que os tempos limite de TCP se reconectem ao servidor que passou por failback.

Observe que o Server for NFS dá suporte a failover transparente somente quando for iniciado manualmente, geralmente durante a manutenção planejada. Se ocorrer um failover não planejado, clientes NFS perderão suas conexões. O servidor para NFS também não tem nenhuma integração com o filtro de chave de retomada. Isso significa que, se um aplicativo local ou uma sessão SMB tentar acessar o mesmo arquivo que um cliente NFS está acessando imediatamente após um failover planejado, o cliente NFS poderá perder suas conexões (o failover transparente não terá êxito).

## <a name="deployment-and-manageability-improvements"></a>Aprimoramentos de implantação e gerenciamento

A implantação e o gerenciamento do NFS foram aprimorados das seguintes maneiras:

- Mais de 40 novos cmdlets do Windows PowerShell facilitam a configuração e o gerenciamento de compartilhamentos de arquivos NFS. Para obter mais informações, consulte [cmdlets de NFS no Windows PowerShell](/powershell/module/nfs/?view=win10-ps).
- O mapeamento de identidade é melhorado com um repositório de mapeamento de arquivo simples local e novos cmdlets do Windows PowerShell para configurar o mapeamento de identidade.
- A interface gráfica do usuário do Gerenciador do Servidor é mais fácil de usar.
- O novo provedor WMI versão 2 está disponível para facilitar o gerenciamento.
- O multiplexador de porta RPC (porta 2049) é amigável para firewall e simplifica a implantação de NFS.

## <a name="server-manager-information"></a>Informações sobre o Gerenciador do Servidor

No Gerenciador do Servidor-ou no [centro de administração do Windows](../../manage/windows-admin-center/overview.md) mais recente-use o assistente para adicionar funções e recursos para adicionar o servidor para o serviço de função NFS (na função de serviços de arquivo e iSCSI). Para obter informações gerais sobre como instalar recursos, consulte [Instalar ou desinstalar funções, serviços de função ou recursos](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). As ferramentas do servidor para NFS incluem os serviços para o snap-in do MMC do sistema de arquivos de rede para gerenciar o servidor NFS e o cliente para componentes NFS. Usando o snap-in, você pode gerenciar o servidor para componentes NFS instalados no computador. O Server for NFS também contém várias ferramentas de administração de linha de comando do Windows:

- A **montagem** monta um compartilhamento NFS remoto (também conhecido como exportação) localmente e o mapeia para uma letra de unidade local no computador cliente do Windows.
- O **nfsadmin** gerencia as definições de configuração do servidor para os componentes NFS e Client for NFS.
- **Nfsshare** configura as configurações de compartilhamento NFS para pastas que são compartilhadas usando o servidor para NFS.
- **Nfsstat** exibe ou redefine as estatísticas de chamadas recebidas pelo servidor para NFS.
- O **conmount** exibe sistemas de arquivos montados exportados pelo servidor para NFS.
- **Umount** remove as unidades montadas com NFS.

O NFS no Windows Server 2012 apresenta o módulo NFS para o Windows PowerShell com vários cmdlets novos, especificamente para NFS. Esses cmdlets fornecem uma maneira fácil de automatizar tarefas de gerenciamento de NFS. Para obter mais informações, consulte [cmdlets de NFS no Windows PowerShell](/powershell/module/nfs/?view=win10-ps).

## <a name="additional-information"></a>Informações adicionais

A tabela a seguir fornece recursos adicionais para avaliar o NFS.

|Tipo de conteúdo|Referências|
|---|---|
|Implantação|[Implantar o Network File System](deploy-nfs.md)|
|Operações|[Cmdlets do NFS no Windows PowerShell](/powershell/module/nfs/?view=win10-ps)|
|Tecnologias relacionadas|[Armazenamento no Windows Server](../storage.yml)|
