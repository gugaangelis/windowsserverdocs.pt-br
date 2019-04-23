---
title: Configurar uma máquina virtual com um controlador SCSI para poder hot plug e desconecte frequente de armazenamento
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 755e7485e54ee58e0acd7ebd75a7ee591aa655f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843277"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>Configurar uma máquina virtual com um controlador SCSI para poder hot plug e desconecte frequente de armazenamento

>Aplica-se a: Windows Server 2016


  
*Para obter mais informações sobre as práticas recomendadas e varreduras, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Encontrar uma máquina virtual que não está configurado com um controlador SCSI.*  
  
## <a name="impact"></a>Impacto  
  
*Você não poderá hot plug ou desconecte frequente de armazenamento para as seguintes máquinas virtuais:*  
  
\<lista de nomes de máquina virtual >  
  
A capacidade de hot plug ou desconecte frequente armazenamento torna mais fácil de gerenciar necessidades de armazenamento de uma máquina virtual sem a necessidade de tempo de inatividade. As máquinas virtuais sem controladores SCSI deve ser desligadas antes que você pode adicionar ou remover o armazenamento.  
  
## <a name="resolution"></a>Resolução  
  
*Se você não precisar hot plug ou desconecte frequente de armazenamento para esta máquina virtual, nenhuma ação é necessária. Caso contrário, desligue a máquina virtual e adicione um controlador SCSI para a configuração.*  
  
Para usar um controlador SCSI para plug hot e desconecte frequente de armazenamento, o sistema operacional convidado deve estar executando a versão atual do integration services.  
  


