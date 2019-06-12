---
title: Instalar | Atualizar | Migrar para o Windows Server de 2019
description: Como a instalação limpa, atualização in-loco ou migrar para Windows Server 2019.
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
ms.openlocfilehash: 1fd955a640832eb161666f74b93d91bb2c3eff11
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810820"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>Instalar | Atualizar | Migrar para o Windows Server de 2019

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> Suporte estendido para Windows Server 2008 R2 e Windows Server 2008 termina em janeiro de 2020. [Saiba mais sobre as opções de atualização](http://aka.ms/upgradecenter).

É hora de mudar para uma versão mais recente do Windows Server? Dependendo do que está executando agora, você tem muitas opções para chegar lá.

## <a name="clean-install"></a>Clean Install
Se você quiser mover de uma versão mais antiga do Windows Server para Windows Server 2019 no mesmo hardware, você deve fazer uma **instalação limpa**, em que você instala apenas o sistema operacional mais recente diretamente sobre a antiga no mesmo hardware, excluindo, portanto, o sistema operacional anterior. Essa é a maneira mais simples, mas você precisará fazer backup de seus dados primeiro e se preparar para reinstalar seus aplicativos. Há algumas coisas a serem consideradas, como os requisitos de sistema, portanto, certifique-se de verificar os detalhes para [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) , e [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## <a name="in-place-upgrade"></a>Atualização in-loco

Se você quiser manter o mesmo hardware e todas as funções de servidor configurar sem mesclar o servidor, você vai querer fazer uma **atualização In loco**, pela qual você vai fazer mais antiga operar o sistema para uma mais recente, mantendo suas configurações, servidor funções e dados intactos. Por exemplo, se seu servidor estiver executando o Windows Server 2012 R2, você pode atualizá-lo para o Windows Server 2016 ou Windows Server 2019. No entanto, nem todos os sistemas operacionais mais antigos têm um caminho para versões mais recentes. Consulte o diagrama a seguir para os caminhos de atualização disponíveis:

![Diagrama de caminhos de atualização in-loco do Windows Server](media/upgrade-paths.png)

Para obter orientação passo a passo sobre como atualizar, visite o [Centro de atualização do Windows Server](http://aka.ms/upgradecenter):

[![Captura de tela do Centro de atualização do Windows Server](media/upgrade-center.png)](http://aka.ms/upgradecenter)

## <a name="cluster-os-rolling-upgrade"></a>Atualização sem interrupção do sistema operacional do cluster

Atualização sem interrupção do cluster do sistema operacional permite que um administrador atualizar o sistema operacional de nós do cluster do Windows Server 2012 R2 e Windows Server 2016 sem interromper o Hyper-V ou as cargas de trabalho do servidor de arquivos de escalabilidade horizontal. Esse recurso permite que você evite o tempo de inatividade que poderia afetar os Contratos de nível de serviço. Esse recurso novo é abordado com mais detalhes em [Atualização sem interrupção do sistema de operacional do cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="migration"></a>Migração

Migração do Windows Server é quando você move uma função ou recurso em vez de um computador de origem que executa o Windows Server para outro computador de destino que executa o Windows Server, a mesma ou uma versão mais recente. Para essas finalidades, a migração é definida como mover uma função ou um recurso e seus dados para um computador diferente, não atualizar o recurso no mesmo computador. 

## <a name="license-conversion"></a>Conversão de licença
Em algumas versões do sistema operacional, é possível converter uma edição específica da versão para outra edição da mesma versão em uma única etapa, com um simples comando e com a chave de licença adequada. Isso é chamado de **conversão de licença**. Por exemplo, se seu servidor estiver executando o Windows Server 2016 Standard, será possível convertê-lo para o Windows Server 2016 Datacenter. Tenha em mente que, embora você pode mover para cima do Server 2016 Standard para o Server 2016 Datacenter, você é não é possível reverter o processo e ir do Datacenter para o padrão. Em algumas versões do Windows Server, você também pode converter livremente entre as versões OEM, com licença de volume e de varejo com o mesmo comando e a chave apropriada.


 
 
