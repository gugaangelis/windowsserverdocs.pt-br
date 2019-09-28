---
title: Reservar uma ou mais redes virtuais externas para uso exclusivo por máquinas virtuais
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: a72f3d616bb0c520e49c27f90686196463f25953
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364782"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>Reservar uma ou mais redes virtuais externas para uso exclusivo por máquinas virtuais

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Erro|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Todas as redes virtuais externas são configuradas para uso tanto pelo sistema operacional de gerenciamento quanto por máquinas virtuais.*  
  
## <a name="impact"></a>Impacto  
  
*O desempenho da rede pode ser degradado no sistema operacional de gerenciamento.*  
  
## <a name="resolution"></a>Resolução  
  
*Use o Gerenciador de comutador virtual para interromper o compartilhamento de uma rede virtual externa com o sistema operacional de gerenciamento.*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>Para interromper o compartilhamento da rede virtual externa com o sistema operacional de gerenciamento  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No menu **Ações**, clique em **Gerenciador do Comutador Virtual**.  
  
3.  Em **comutadores virtuais**, clique no nome do comutador virtual externo.  
  
4.  Na área **tipo de conexão** , sob o nome do adaptador de rede física, desmarque a caixa de seleção **permitir que o sistema operacional de gerenciamento Compartilhe este adaptador de rede** .  
  
5.  Clique em **OK**.  
  


