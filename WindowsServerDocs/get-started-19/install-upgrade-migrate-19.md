---
title: Instalar | Atualizar | Migrar para o Windows Server 2019
description: Como fazer instalação limpa, atualização in-loco ou migrar para Windows Server 2019.
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810820"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>Instalar | Atualizar | Migrar para o Windows Server 2019

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> O suporte estendido para Windows Server 2008 R2 e Windows Server 2008 termina em janeiro de 2020. [Saiba mais sobre as opções de atualização](http://aka.ms/upgradecenter).

É hora de mudar para uma versão mais recente do Windows Server? Dependendo do que estiver executando agora, você tem muitas opções para chegar lá.

## <a name="clean-install"></a>Instalação limpa
Se você quer mudar de uma versão mais antiga do Windows Server para o Windows Server 2019 no mesmo hardware, é necessário fazer uma **Instalação limpa**, onde basta instalar o sistema operacional mais recente diretamente sobre o antigo no mesmo hardware, excluindo, portanto, o sistema operacional anterior. Essa é a maneira mais simples, mas primeiro será necessário fazer backup de seus dados e se preparar para reinstalar seus aplicativos. Há algumas coisas que você deve saber, como os requisitos do sistema, portanto, verifique os detalhes do [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), do [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) e [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## <a name="in-place-upgrade"></a>Atualização in-loco

Se você quiser manter o mesmo hardware e todas as funções de servidor configuradas sem mesclar o servidor, será necessário fazer uma **atualização in-loco**, pela qual você muda de um sistema operacional mais antigo para um mais recente, mantendo intactos seus dados, suas configurações e funções do servidor. Por exemplo, se o servidor estiver executando o Windows Server 2012 R2, você poderá atualizá-lo para o Windows Server 2016 ou Windows Server 2019. Entretanto, nem todos os sistemas operacionais mais antigos têm um caminho para versões mais recentes. Consulte o diagrama a seguir para ver os caminhos de atualização disponíveis:

![Diagrama de caminhos de atualização in-loco do Windows Server](media/upgrade-paths.png)

Para obter orientações passo a passo sobre a atualização, visite o [Windows Server Upgrade Center](http://aka.ms/upgradecenter):

[![Captura de tela do Windows Server Upgrade Center](media/upgrade-center.png)](http://aka.ms/upgradecenter)

## <a name="cluster-os-rolling-upgrade"></a>Atualização sem interrupção do SO do cluster

A atualização sem interrupção do SO do cluster permite ao administrador atualizar o sistema operacional dos nós de cluster a partir do Windows Server 2012 R2 e do Windows Server 2016 sem interromper o Hyper-V ou as cargas de trabalho do Servidor de Arquivos de Escalabilidade Horizontal. Esse recurso permite que você evite o tempo de inatividade que poderia afetar os Contratos de nível de serviço. Esse recurso novo é abordado com mais detalhes em [Atualização sem interrupção do sistema de operacional do cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="migration"></a>Migração

Migração do Windows Server é quando você migra uma função ou um recurso de cada vez de um computador de origem executando o Windows Server para outro computador de destino executando o Windows Server, usando uma versão igual ou mais recente. Para essas finalidades, a migração é definida como mover uma função ou um recurso e seus dados para um computador diferente, e não como atualizar o recurso no mesmo computador. 

## <a name="license-conversion"></a>Conversão de licença
Em algumas versões do sistema operacional, é possível converter uma edição específica da edição para outra edição da mesma versão em uma única etapa, usando um simples comando e com a chave de licença adequada. Isso é chamado de **conversão de licença**. Por exemplo, se o servidor estiver executando o Windows Server 2016 Standard, será possível convertê-lo para o Windows Server 2016 Datacenter. Tenha em mente que, embora você possa mudar do Server 2016 Standard para o Server 2016 Datacenter, não é possível reverter o processo e mudar do Datacenter para o Standard. Em algumas versões do Windows Server, também é possível fazer conversões livremente entre as versões OEM, de licença por volume e de varejo com o mesmo comando e a chave apropriada.


 
 
