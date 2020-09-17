---
title: Configurar sistemas operacionais convidados para backups baseados em VSS para habilitar instantâneos consistentes com o aplicativo para a réplica do Hyper-V
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
ms.date: 8/16/2016
ms.openlocfilehash: b6a7eec504282e63e0cb24efbd2cdc5f66849005
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746881"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>Configurar sistemas operacionais convidados para backups baseados em VSS para habilitar instantâneos consistentes com o aplicativo para a réplica do Hyper-V

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
*Os instantâneos consistentes com o aplicativo exigem que o VSS (serviços de cópias de sombra de volume) esteja habilitado e configurado nos sistemas operacionais convidados de máquinas virtuais que participam da replicação.*

## <a name="impact"></a>Impacto
*Mesmo que os instantâneos consistentes com o aplicativo sejam especificados na configuração de replicação, o Hyper-V não os usará, a menos que o VSS esteja configurado. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Use a conexão de máquina virtual para instalar o Integration Services na máquina virtual.*



