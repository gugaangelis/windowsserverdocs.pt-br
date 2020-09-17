---
title: Oferecer todos os serviços de integração disponíveis para máquinas virtuais
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
ms.date: 8/16/2016
ms.openlocfilehash: 1343761ae9e0982d25133bd429218c8fe31aadbc
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746251"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>Oferecer todos os serviços de integração disponíveis para máquinas virtuais

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*Um ou mais serviços de integração disponíveis não estão habilitados em máquinas virtuais.*

## <a name="impact"></a>Impacto

*Alguns recursos não estarão disponíveis para as seguintes máquinas virtuais:*

\<list of virtual machine names>

## <a name="resolution"></a>Resolução

*Se isso for intencional, nenhuma ação adicional será necessária. Caso contrário, considere a possibilidade de oferecer todos os serviços de integração nas configurações dessas máquinas virtuais.*

A disponibilidade de alguns serviços de integração pode ser gerenciada por meio das configurações de máquina virtual.

#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>Para gerenciar a disponibilidade do Integration Services para uma máquina virtual

1.  Abra o Gerenciador do Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.

2.  No painel de resultados, em **máquinas virtuais**, selecione a máquina virtual que você deseja configurar.

3.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.

4.  Em **Gerenciamento**, clique em **Integration Services**.

5.  Na lista de serviços de integração, marque a caixa de seleção para cada serviço que você deseja oferecer à máquina virtual. Se uma caixa de seleção não estiver disponível, esse serviço de integração específico não terá suporte do sistema operacional convidado executado na máquina virtual.



