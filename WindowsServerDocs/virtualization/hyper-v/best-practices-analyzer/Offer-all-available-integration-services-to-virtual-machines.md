---
title: Oferecer todos os serviços de integração disponíveis para máquinas virtuais
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5ec2dc73cea8b8356d832bf9fdb960985df2df6c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393596"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>Oferecer todos os serviços de integração disponíveis para máquinas virtuais

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Um ou mais serviços de integração disponíveis não estão habilitados em máquinas virtuais.*  
  
## <a name="impact"></a>Impacto  
  
*Alguns recursos não estarão disponíveis para as seguintes máquinas virtuais:*  
  
\<list de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*If isso é intencional, nenhuma ação adicional é necessária. Caso contrário, considere a possibilidade de oferecer todos os serviços de integração nas configurações dessas máquinas virtuais.*  
  
A disponibilidade de alguns serviços de integração pode ser gerenciada por meio das configurações de máquina virtual.   
  
#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>Para gerenciar a disponibilidade do Integration Services para uma máquina virtual  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de resultados, em **máquinas virtuais**, selecione a máquina virtual que você deseja configurar.  
  
3.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.  
  
4.  Em **Gerenciamento**, clique em **Integration Services**.  
  
5.  Na lista de serviços de integração, marque a caixa de seleção para cada serviço que você deseja oferecer à máquina virtual. Se uma caixa de seleção não estiver disponível, esse serviço de integração específico não terá suporte do sistema operacional convidado executado na máquina virtual.  
  


