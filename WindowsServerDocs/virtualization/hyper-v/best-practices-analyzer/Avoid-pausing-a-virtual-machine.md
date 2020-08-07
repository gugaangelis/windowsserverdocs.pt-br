---
title: Evitar pausar uma máquina virtual
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 6799795ae383fd0522ce0b35eba0443ae536b0dd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969993"
---
# <a name="avoid-pausing-a-virtual-machine"></a>Evitar pausar uma máquina virtual

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*Este servidor tem uma ou mais máquinas virtuais em um estado pausado.*

## <a name="impact"></a>Impacto

*Dependendo da quantidade de memória disponível, talvez você não consiga executar máquinas virtuais adicionais.*

As máquinas virtuais em pausa não liberam sua memória alocada, o que significa que a memória não está disponível para iniciar outras máquinas virtuais.

## <a name="resolution"></a>Resolução

*Se isso for intencional, nenhuma ação adicional será necessária. Caso contrário, considere retomar essas máquinas virtuais ou desligá-las.*

#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>Usar o Gerenciador do Hyper-V para retomar a máquina virtual

1.  Abra o Gerenciador do Hyper-V. (No menu **ferramentas** do Gerenciador do servidor, clique em **Gerenciador do Hyper-V**.)

2.  Na lista **máquinas virtuais** , localize as máquinas virtuais com um estado de em **pausa**.

    > [!IMPORTANT]
    > Um estado de **pausa-crítico** ocorre quando há muito pouco espaço livre restante no armazenamento físico para essa máquina virtual. Antes de tentar retomar uma máquina virtual nesse estado, libere espaço disponível no armazenamento físico.

3.  Clique com o botão direito do mouse em cada nome de máquina virtual e clique em **continuar**. Isso retorna a máquina virtual para um estado de execução. Depois disso, se você quiser desligar a máquina virtual, clique com o botão direito do mouse nela novamente e escolha **desligar**.

#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>Usar o Windows PowerShell para retomar a máquina virtual

Você pode fazer isso em um comando usando a filtragem e o pipeline depois de obter todas as máquinas virtuais no host. Tipo:

```
get-vm | where state -eq 'paused' | resume-vm
```



