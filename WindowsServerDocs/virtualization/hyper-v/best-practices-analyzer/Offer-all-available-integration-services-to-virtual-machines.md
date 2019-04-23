---
title: Todos os serviços de integração disponíveis para máquinas virtuais da oferta
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c2b5137594f78980f87f6520ae4b4af8203aef32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883777"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>Todos os serviços de integração disponíveis para máquinas virtuais da oferta

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Um ou mais serviços de integração disponíveis não estão habilitados nas máquinas virtuais.*  
  
## <a name="impact"></a>Impacto  
  
*Alguns recursos não estarão disponíveis para as seguintes máquinas virtuais:*  
  
\<lista de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Se isso é intencional, nenhuma ação adicional é necessária. Caso contrário, considere a oferta de todos os serviços de integração nas configurações dessas máquinas virtuais.*  
  
A disponibilidade de alguns serviços de integração pode ser gerenciada por meio de configurações da máquina virtual.   
  
#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>Para gerenciar a disponibilidade dos serviços de integração para uma máquina virtual  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de resultados, sob **máquinas virtuais**, selecione a máquina virtual que você deseja configurar.  
  
3.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.  
  
4.  Sob **Management**, clique em **Integration Services**.  
  
5.  Na lista de serviços de integração, selecione a caixa de seleção para cada serviço que você quer oferecer à máquina virtual. Se uma caixa de seleção não estiver disponível, o que o serviço de integração específico não é suportado pelo sistema operacional convidado que é executado na máquina virtual.  
  


