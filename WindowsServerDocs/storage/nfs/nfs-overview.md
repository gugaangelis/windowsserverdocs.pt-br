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
ms.openlocfilehash: fb31cff44cac6bd66f9aa5b7234ff3fd3b215ccf
ms.sourcegitcommit: bad0692ffd0773f61ac62bd140a421cb02c68acf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099135"
---
# Visão geral de sistema de arquivos de rede

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve o serviço de função de sistema de arquivos de rede e os recursos incluídos com a função de servidor Serviços de arquivo e armazenamento no Windows Server. Sistema NFS (Network File) oferece uma solução para empresas com ambientes heterogêneos que incluem o Windows e não Windows computadores de compartilhamento de arquivos.

## Descrição do recurso

Usando o protocolo NFS, você pode transferir arquivos entre computadores que executam o Windows e outros sistemas operacionais não Windows, como Linux ou UNIX.

NFS no Windows Server inclui o servidor para NFS e cliente para NFS. Um computador executando o Windows Server pode usar o servidor para NFS para atuar como um servidor de arquivos NFS para outros computadores cliente não Windows. Cliente NFS permite que um computador baseado no Windows executando o Windows Server para acessar arquivos armazenados em um servidor não - Windows NFS.

## Versões do Windows e Windows Server

Windows dá suporte a várias versões do NFS cliente e servidor, dependendo da versão do sistema operacional e família. 

| Sistemas operacionais | Versões de servidor NFS |Versões de cliente NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/A | NFS v2, NFS V3 |
| Windows Server 2008, Windows Server 2008 R2 | NFS v2, NFS V3 | NFS v2, NFS V3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFS v2, NFS v3, NFSv4.1  | NFS v2, NFS V3 |

## Aplicações práticas

Aqui estão algumas maneiras de usar NFS:

- Use um servidor de arquivos NFS do Windows para fornecer acesso de vários protocolo para o mesmo compartilhamento de arquivos por meio de protocolos SMB e NFS de clientes para várias plataformas.
- Implante um servidor de arquivos NFS do Windows em um ambiente de sistema operacional basicamente não-Windows para fornecer acesso de computadores a compartilhamentos de arquivos NFS de cliente não Windows.
- Migre aplicativos de um sistema operacional para outro, armazenando os dados nos compartilhamentos de arquivos acessíveis por meio de protocolos SMB e NFS.

## Funcionalidade nova e alterada

Funcionalidades novas e alteradas no sistema de arquivos de rede incluem suporte para o NFS versão 4.1 e implantação aprimorado e capacidade de gerenciamento. Para obter informações sobre recursos novos ou alterados no Windows Server 2012, examine a tabela a seguir:

|Recurso/funcionalidade|Novo ou atualizado|Descrição|
|---|---|---|
|[NFS versão 4.1](#nfs-version-4.1)|Novo|Maior segurança, desempenho e interoperabilidade em comparação comparada o NFS versão 3.|
|[Infraestrutura NFS](#nfs-infrastructure)|Atualizado|Melhora a capacidade de gerenciamento e implantação e aumenta a segurança.|
|[Disponibilidade contínua NFS versão 3](#nfs-version-3-continuous-availability)|Atualizado|Melhora a disponibilidade contínua em clientes do NFS versão 3.|
|[Melhorias de capacidade de gerenciamento e implantação](#deployment-and-manageability-improvements)|Atualizado|Permite que você facilmente implantar e gerenciar o NFS com novos cmdlets do Windows PowerShell e um novo provedor WMI.|

## NFS versão 4.1

NFS versão 4.1 implementa todos os aspectos necessários, além de alguns dos aspectos opcionais, de [RFC 5661](https://tools.ietf.org/html/rfc5661):

- **Sistema de arquivos Pseudo**, um sistema de arquivos que separa namespace física e lógica e é compatível com o NFS versão 3 e NFS versão 2. Um alias é fornecido para o sistema de arquivo exportado, que faz parte do sistema de arquivos pseudo.
- **RPCs compostos** combinar operações relevantes e reduzir a quantidade de conversas cruzado.
- **Sessões e truncamento de sessão** permite apenas uma semântica e que a disponibilidade contínua e melhor desempenho durante a utilização de várias redes entre clientes NFS 4.1 e o servidor NFS.

## Infraestrutura NFS

Melhorias para a infraestrutura NFS geral no Windows Server 2012 são detalhadas abaixo:

- O **procedimento remoto (RPC) / representação de dados externos (XDR)** infraestrutura de transporte, equipada com o protocolo de rede WinSock, está disponível para os dois servidor para NFS e cliente NFS. This replaces Transport Device Interface (TDI), offers better support, and provides better scalability and Receive Side Scaling (RSS).
- O recurso de **porta RPC multiplexador** é (menos portas para gerenciar) com firewalls e simplifica a implantação de NFS.
- **Caches ajustado automaticamente e pools de threads** são recursos de gerenciamento de recursos da nova infraestrutura RPC/XDR que são dinâmicos, ajuste automaticamente caches e thread pools com base na carga de trabalho. Isso remove completamente suposições envolvidos ao ajustar parâmetros, fornecendo desempenho ideal, assim como NFS é implantado.
- **Opções de implementação e autenticação de privacidade do novo Kerberos** com a adição de suporte do Kerberos privacidade (Krb5p) juntamente com as opções de autenticação de krb5i e krb5 existente.
- **Módulo do Windows PowerShell identidade mapeamento** cmdlets facilitam o mapeamento de identidade de gerenciar, configurar o Active Directory Lightweight Directory Services (AD LDS) e configurar senhas UNIX e Linux e arquivos simples.
- **Ponto de montagem de volume** permite que acessar volumes montados em um compartilhamento NFS com NFS versão 4.1.
- O recurso de **Multiplexação de porta** oferece suporte a RPC porta multiplexador (2049), que é compatível com firewall e simplifica a implantação de NFS.

## Disponibilidade contínua NFS versão 3

Clientes da versão 3 NFS podem ter failovers planejados rápida e transparente com mais disponibilidade e o tempo de inatividade reduzido. O processo de failover é mais rápido para clientes da versão 3 NFS porque:

- A infraestrutura de cluster agora permite que um recurso por nome em vez de um recurso por compartilhamento, o que melhora significativamente o tempo de failover dos recursos da rede.
- Caminhos de failover em um servidor NFS são ajustados para melhorar o desempenho.
- Registro curinga em um servidor NFS não for mais necessário, e os failovers são mais refinados.
- Rede Monitor de Status (NSM) notificações são enviadas após o processo de failover e clientes não precisam mais esperar tempos limite TCP reconectá-lo para a falha ao longo do servidor.

Observe que o servidor para NFS oferece suporte a failover transparente somente quando iniciados manualmente, geralmente durante a manutenção planejada. Se ocorrer um failover não planejado, clientes NFS perdem suas conexões. Servidor para NFS também não tem qualquer integração com o filtro de chave de retomada. Isso significa que, se um aplicativo local ou uma sessão do SMB tenta acessar o mesmo arquivo que um cliente NFS está acessando imediatamente após um failover planejado, o cliente NFS pode perder suas conexões (não ter êxito failover transparente).

## Melhorias de capacidade de gerenciamento e implantação

Implantar e gerenciar o NFS melhorou das seguintes maneiras:

- Mais de quarenta novos cmdlets do Windows PowerShell tornar mais fácil configurar e gerenciar os compartilhamentos de arquivos NFS. Para obter mais informações, consulte [NFS Cmdlets no Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).
- Mapeamento de identidade é melhorado com um armazenamento de mapeamento de local de arquivo simples e novos cmdlets do Windows PowerShell para configurar o mapeamento de identidade.
- A interface gráfica do usuário do Gerenciador do servidor é mais fácil de usar.
- O novo provedor de WMI versão 2 está disponível para facilitar o gerenciamento.
- A porta multiplexador (porta RPC 2049) é compatível com firewall e simplifica a implantação de NFS.

## Informações sobre o Gerenciador do Servidor

No Gerenciador do servidor - ou mais recente [Do Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) - use o assistente Adicionar funções e recursos para adicionar o servidor para o serviço de função NFS (sob o arquivo e iSCSI função de serviços). Para obter informações gerais sobre a instalação de recursos, consulte [instalar ou desinstalar funções, serviços de função ou recursos](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). Servidor para ferramentas NFS incluem os serviços do snap-in MMC de sistema de arquivos de rede para gerenciar o servidor para NFS e cliente para componentes NFS. Usando o snap-in, você pode gerenciar o servidor para componentes NFS instalados no computador. Servidor para NFS também contém várias ferramentas de administração de linha de comando do Windows:

- **Montar** monta um compartilhamento NFS remoto (também conhecido como uma exportação) localmente e mapeia para uma letra de unidade local no computador cliente do Windows.
- **Nfsadmin** gerencia as configurações do servidor para NFS e cliente para componentes NFS.
- **Nfsshare** define as configurações de compartilhamento NFS para pastas que são compartilhadas usando o servidor para NFS.
- **Nfsstat** exibe ou redefine as estatísticas de chamadas recebidas pelo servidor para NFS.
- **Showmount** exibe os sistemas de arquivos montados exportados pelo servidor para NFS.
- **Umount** remove unidades NFS montado.

NFS no Windows Server 2012 apresenta o módulo NFS para o Windows PowerShell com vários novos cmdlets especificamente para NFS. Esses cmdlets fornecem uma maneira fácil de automatizar tarefas de gerenciamento de NFS. Para obter mais informações, consulte [NFS cmdlets no Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).

## Informações adicionais

A tabela a seguir fornece recursos adicionais para avaliar NFS.

|Tipo de conteúdo|Referências|
|---|---|
|Implantação|[Implantar o sistema de arquivos de rede](deploy-nfs.md)|
|Operações|[Cmdlets NFS no Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|Tecnologias relacionadas|[Armazenamento no Windows Server](../storage.md)|