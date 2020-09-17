---
title: O número de processadores lógicos em uso não deve exceder o máximo com suporte
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
ms.date: 8/16/2016
ms.openlocfilehash: 580d04af45416e08e536d815390be0e45b760312
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746141"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>O número de processadores lógicos em uso não deve exceder o máximo com suporte

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Erro|
|**Categoria**|Política|

Nas seções a seguir, os itálicos indicam o texto que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*O servidor está configurado com muitos processadores lógicos.*

## <a name="impact"></a>Impacto

*A Microsoft não dá suporte à execução do Hyper-V neste computador.*

## <a name="resolution"></a>Resolução

*Remova alguns processadores desta máquina ou use o msconfig para limitar o número de processadores disponíveis.*

Consulte as instruções a seguir para usar o msconfig. Para obter detalhes sobre como remover processadores, consulte as instruções fornecidas com o computador ou contate o fabricante do hardware. Para obter detalhes sobre as configurações máximas com suporte para o Hyper-V, consulte [planejar a escalabilidade do Hyper-v no Windows Server 2016](../plan/plan-hyper-v-scalability-in-windows-server.md).

### <a name="to-limit-the-number-of-available-processors"></a>Para limitar o número de processadores disponíveis

1.  Abra o aplicativo de configuração do sistema (Msconfig.exe). Para fazer isso, clique em **Iniciar**, digite **msconfig**, clique com o botão direito do mouse no aplicativo da área de trabalho configuração do **sistema** e clique em **Executar como administrador**.

2.  Na guia **inicialização** , clique em **Opções avançadas**.

3.  Selecione **número de processadores** e, em seguida, selecione um número na lista. Clique em **OK**.

4.  Reinicie o computador para executá-lo usando o novo número de processadores.