---
title: Instalar | Atualizar | Migrar para o Windows Server 2019
description: Como instalação limpa, atualização in-loco ou migrar para Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4e99cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 58c363fc0a1e336519bc6ec4276651345cc2b5eb
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121385"
---
# Instalar | Atualizar | Migrar para o Windows Server 2019

>Aplicável a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> Suporte estendido para Windows Server 2008 R2 e Windows Server 2008 termina em janeiro de 2020. [Saiba mais sobre suas opções de atualização](http://aka.ms/upgradecenter).

É hora de mudar para uma versão mais recente do Windows Server? Dependendo do que está executando agora, você tem muitas opções para chegar lá.

## Limpar instalação
Se você quiser mover de uma versão mais antiga do Windows Server para Windows Server 2019 no mesmo hardware, você deverá fazer uma **instalação limpa**, onde basta instalar o sistema operacional mais recente diretamente sobre o antigo no mesmo hardware, excluindo, portanto, a sistema operacional anterior. Essa é a maneira mais simples, mas você precisará fazer backup de seus dados primeiro e se preparar para reinstalar seus aplicativos. Há algumas coisas a saber, como requisitos do sistema, portanto, certifique-se de verificar os detalhes para o [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418)e [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124).

## Atualização in-loco
Se você quiser manter o mesmo hardware e todas as funções de servidor que você configurou sem planificar o servidor, você vai querer fazer uma **atualização in-loco**, pelo qual você vai de um sistema operacional antigo para um mais recente, mantendo suas configurações, funções de servidor e dados intactos. Por exemplo, se seu servidor estiver executando o Windows Server 2012 R2, você pode atualizá-lo para o Windows Server 2016 ou Windows Server 2019. No entanto, nem todos os sistemas operacionais mais antigos têm um caminho para versões mais recentes. Consulte o diagrama a seguir para os caminhos de atualização disponíveis:

![Diagrama de caminhos de atualização in-loco do Windows Server](media/upgrade-paths.png)

Para obter orientação passo a passo sobre como atualizar, visite o [Centro de atualização do Windows Server](http://aka.ms/upgradecenter):

<a href="http://aka.ms/upgradecenter"><img src="media/upgrade-center.png" alt="Screenshot of Windows Upgrade Center" title="Centro de atualização do Windows Server"></a>

## Atualização sem interrupção do sistema operacional do cluster
Upgrade sem interrupção do cluster do sistema operacional permite ao administrador fazer upgrade no sistema operacional de nós do cluster do Windows Server 2012 R2 e Windows Server 2016 sem interromper o Hyper-V ou as cargas de trabalho de servidor de arquivos de escalabilidade horizontal. Esse recurso permite que você evite o tempo de inatividade que poderia afetar os Contratos de nível de serviço. Esse recurso novo é abordado com mais detalhes em [Upgrade sem interrupção do sistema de operacional do cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## Migração

Migração do Windows Server é quando você move uma função ou recurso em vez de um computador de origem que está executando o Windows Server para outro computador de destino que está executando o Windows Server, a mesma ou uma versão mais recente. Para essas finalidades, a migração é definida como mover uma função ou um recurso e seus dados para um computador diferente, não atualizar o recurso no mesmo computador. 

## Conversão de licença
Em algumas versões do sistema operacional, é possível converter uma edição específica da versão para outra edição da mesma versão em uma única etapa, com um simples comando e com a chave de licença adequada. Isso é chamado de **conversão de licença**. Por exemplo, se seu servidor estiver executando o Windows Server 2016 Standard, será possível convertê-lo para o Windows Server 2016 Datacenter. Em algumas versões do Windows Server, você também pode converter livremente entre as versões OEM, com licença de volume e de varejo com o mesmo comando e a chave apropriada.


 
 
