---
title: Entradas de autorização devem ter nomes de grupo de confiança distintos para servidores primários com máquinas virtuais que não fazem parte do mesmo grupo de confiança
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5c5b1a6bf8ef0bbceb5dde6b28cd951f399fc5e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882957"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>Entradas de autorização devem ter nomes de grupo de confiança distintos para servidores primários com máquinas virtuais que não fazem parte do mesmo grupo de confiança

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*O servidor aceitará solicitações de replicação para a máquina virtual de réplica de qualquer um dos servidores na lista de autorização associado com a mesma marca de replicação da máquina virtual.*  
  
## <a name="impact"></a>**Impacto**  
*Pode haver privacidade e questões de segurança com uma máquina virtual aceitando a replicação de servidores primários que pertencem a entradas de autorização diferentes. Isso afeta as seguintes entradas de autorização: \<lista de entradas de autorização >*  
  
## <a name="resolution"></a>**Resolução**  
*Use marcas diferentes nas entradas de autorização para os servidores primários com máquinas virtuais que não fazem parte do mesmo grupo de segurança. Modifica as configurações de Hyper-V para configurar as marcas de replicação.*  
  


