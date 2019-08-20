---
title: Instalar, atualizar ou migrar para o Windows Server 2019
description: Como fazer instalação limpa, atualização in-loco ou migrar para o Windows Server
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4e99cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 7e90738a157f620124bfca3d5f1f4c12789d3bf2
ms.sourcegitcommit: b17ccf7f81e58e8f4dd844be8acf784debbb20ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69023914"
---
# <a name="install-upgrade-or-migrate-to-windows-server"></a>Instalar, atualizar ou migrar para o Windows Server

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> O suporte estendido para Windows Server 2008 R2 e Windows Server 2008 termina em janeiro de 2020. [Saiba mais sobre as opções de atualização](http://aka.ms/upgradecenter). Para baixar o Windows Server 2019, confira [Avaliações do Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019).

É hora de mudar para uma versão mais recente do Windows Server? Dependendo do que estiver em execução agora, você tem muitas opções para chegar lá.

## <a name="clean-install"></a>Instalação limpa

A maneira mais simples de instalar o Windows Server é executar uma instalação limpa, em que você instala em um servidor em branco ou substitui um sistema operacional existente. Essa é a maneira mais simples, mas primeiro é necessário fazer backup de seus dados e se preparar para reinstalar seus aplicativos. Há algumas coisas que você deve saber, como os requisitos do sistema, portanto, verifique os detalhes do [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), do [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) e [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## <a name="in-place-upgrade"></a>Atualização in-loco

Se você quiser manter o mesmo hardware e todas as funções de servidor configuradas sem mesclar o servidor, será necessário fazer uma **atualização in-loco**, pela qual você muda de um sistema operacional mais antigo para um mais recente, mantendo intactos seus dados, suas configurações e funções do servidor. Por exemplo, se o servidor estiver executando o Windows Server 2012 R2, você poderá atualizá-lo para o Windows Server 2016 ou Windows Server 2019. Entretanto, nem todos os sistemas operacionais mais antigos têm um caminho para versões mais recentes. 

Para obter orientações passo a passo sobre a atualização, visite o [Windows Server Upgrade Center](http://aka.ms/upgradecenter):

[![Captura de tela do Windows Server Upgrade Center](media/upgrade-center.png)](http://aka.ms/upgradecenter)

## <a name="cluster-os-rolling-upgrade"></a>Atualização sem interrupção do sistema operacional do cluster

A atualização sem interrupção do SO do cluster permite ao administrador atualizar o sistema operacional dos nós de cluster a partir do Windows Server 2012 R2 e do Windows Server 2016 sem interromper o Hyper-V ou as cargas de trabalho do Servidor de Arquivos de Escalabilidade Horizontal. Esse recurso permite que você evite o tempo de inatividade que poderia afetar os Contratos de nível de serviço. Esse recurso novo é abordado com mais detalhes em [Atualização sem interrupção do sistema de operacional do cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="migration"></a>Migração

Migração do Windows Server é quando você migra uma função ou um recurso de cada vez de um computador de origem executando o Windows Server para outro computador de destino executando o Windows Server, usando uma versão igual ou mais recente. Para essas finalidades, a migração é definida como mover uma função ou um recurso e seus dados para um computador diferente, e não como atualizar o recurso no mesmo computador. 

## <a name="license-conversion"></a>Conversão de licença

Em algumas versões do sistema operacional, é possível converter uma edição específica da edição para outra edição da mesma versão em uma única etapa, usando um simples comando e com a chave de licença adequada. Isso é chamado de **conversão de licença**. Por exemplo, se o servidor estiver executando o Windows Server 2016 Standard, será possível convertê-lo para o Windows Server 2016 Datacenter. Tenha em mente que, embora você possa mudar do Server 2016 Standard para o Server 2016 Datacenter, não é possível reverter o processo e mudar do Datacenter para o Standard. Em algumas versões do Windows Server, também é possível fazer conversões livremente entre as versões OEM, de licença por volume e de varejo com o mesmo comando e a chave apropriada.