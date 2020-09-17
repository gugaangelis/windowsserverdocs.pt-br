---
title: Configurar uma máquina virtual com um controlador SCSI para poder conectar e desconectar o armazenamento quente
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
ms.date: 8/16/2016
ms.openlocfilehash: 9f0cdb47206c89c4da66786e8dfd118e6a05be81
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746971"
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



