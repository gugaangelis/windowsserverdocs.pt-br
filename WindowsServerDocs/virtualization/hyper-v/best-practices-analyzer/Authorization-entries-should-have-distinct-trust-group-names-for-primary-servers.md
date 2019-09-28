---
title: As entradas de autorização devem ter nomes de grupos de confiança distintos para servidores primários com máquinas virtuais que não fazem parte do mesmo grupo de confiança
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ab71e0e6562b73d81b871914fd0e9e76570c518e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366542"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>As entradas de autorização devem ter nomes de grupos de confiança distintos para servidores primários com máquinas virtuais que não fazem parte do mesmo grupo de confiança

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*O servidor aceitará solicitações de replicação para a máquina virtual de réplica de qualquer um dos servidores na lista de autorização associada à mesma marca de replicação que a máquina virtual.*  
  
## <a name="impact"></a>**Causa**  
*There pode ser uma preocupação de privacidade e segurança com uma máquina virtual que aceita a replicação de servidores primários que pertencem a diferentes entradas de autorização. Isso afeta as seguintes entradas de autorização: \<list de entradas de autorização >*  
  
## <a name="resolution"></a>**Resolução**  
*Use marcas diferentes nas entradas de autorização para servidores primários com máquinas virtuais que não fazem parte do mesmo grupo de segurança. Modifique as configurações do Hyper-V para configurar as marcas de replicação.*  
  


