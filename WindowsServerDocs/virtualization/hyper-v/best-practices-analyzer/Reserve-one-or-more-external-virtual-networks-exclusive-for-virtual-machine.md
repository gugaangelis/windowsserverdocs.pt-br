---
title: Reservar um ou mais redes virtuais externas para uso exclusivo por máquinas virtuais
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c8c90a74352bae0b348608db0fc05107e4d09010
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884737"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>Reservar um ou mais redes virtuais externas para uso exclusivo por máquinas virtuais

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Todas as redes virtuais externas são configuradas para uso pelo sistema operacional de gerenciamento e as máquinas virtuais.*  
  
## <a name="impact"></a>Impacto  
  
*Desempenho de rede pode estar danificado no sistema operacional de gerenciamento.*  
  
## <a name="resolution"></a>Resolução  
  
*Use o Gerenciador de comutador Virtual para interromper o compartilhamento de uma rede virtual externa com o sistema operacional de gerenciamento.*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>Para interromper o compartilhamento de rede virtual externa com o sistema operacional de gerenciamento  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No menu **Ações**, clique em **Gerenciador do Comutador Virtual**.  
  
3.  Sob **comutadores virtuais**, clique no nome do comutador virtual externo.  
  
4.  No **tipo de Conexão** área abaixo do nome do adaptador de rede física, desmarque as **permitem que o sistema operacional de gerenciamento compartilhe esse adaptador de rede** caixa de seleção.  
  
5.  Clique em **OK**.  
  


