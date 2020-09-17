---
title: O Integration Services deve ser instalado antes que as máquinas virtuais primárias ou de réplica possam usar um endereço IP alternativo após um failover
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas, com links para mais informações.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
ms.date: 8/16/2016
ms.openlocfilehash: 3a612c9e119ac2b74bea070feb458703dd50040f
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745821"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>O Integration Services deve ser instalado antes que as máquinas virtuais primárias ou de réplica possam usar um endereço IP alternativo após um failover

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Erro|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*As máquinas virtuais que participam da replicação podem ser configuradas para usar um endereço IP específico em caso de failover, mas somente se o Integration Services estiver instalado no sistema operacional convidado da máquina virtual.*

## <a name="impact"></a>Impacto
*No caso de um failover (planejado, não planejado ou de teste), a máquina virtual de réplica será colocado online usando o mesmo endereço IP que a máquina virtual primária. Essa configuração pode causar problemas de conectividade. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Use a conexão de máquina virtual para instalar o Integration Services na máquina virtual.*

A partir do Windows Server 2016, o Integration Services para máquinas virtuais do Windows é entregue por meio de Windows Update. Verifique se essas máquinas virtuais estão configuradas para receber atualizações do Windows para obter a versão mais recente do Integration Services. O kernel do Linux agora inclui o LIS (Linux Integration Services) e é atualizado para novas versões, mas as distribuições do Linux baseadas em kernels mais antigos podem não ter os aprimoramentos ou correções mais recentes. Para obter detalhes, consulte [máquinas virtuais Linux e FreeBSD com suporte para o Hyper-V no Windows](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).


