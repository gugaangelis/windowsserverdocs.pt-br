---
title: Configurar uma máquina virtual com um controlador SCSI para poder conectar e desconectar o armazenamento quente
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0b914b2d250bbcb4c5e795ec778dde8db91613ec
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948457"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>Configurar uma máquina virtual com um controlador SCSI para poder conectar e desconectar o armazenamento quente

>Aplica-se a: Windows Server 2016



*Para obter mais informações sobre práticas recomendadas e verificações, consulte* [analisador de práticas recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*Foi encontrada uma máquina virtual que não está configurada com um controlador SCSI.*

## <a name="impact"></a>Impacto

*Você não poderá conectar-se ao Hot plug ou ao Hot plug do armazenamento para as seguintes máquinas virtuais:*

\<list of virtual machine names>

A capacidade de conectar-se ao Hot plug ou ao Hot Unplug Storage facilita o gerenciamento das necessidades de armazenamento de uma máquina virtual sem a necessidade de tempo de inatividade. As máquinas virtuais sem controladores SCSI devem ser desligadas antes que você possa adicionar ou remover o armazenamento.

## <a name="resolution"></a>Resolução

*Se você não precisar conectar ou desconectar o armazenamento quente para essa máquina virtual, nenhuma ação será necessária. Caso contrário, desligue a máquina virtual e adicione um controlador SCSI à configuração.*

Para usar um controlador SCSI para o Hot plug e o armazenamento em hot-plug, o sistema operacional convidado deve estar executando a versão atual do Integration Services.



