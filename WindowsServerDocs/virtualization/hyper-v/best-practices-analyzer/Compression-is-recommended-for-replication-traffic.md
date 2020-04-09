---
title: A compactação é recomendada para o tráfego de replicação
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: aca66991eae57d702f38e2282eeb4253bc1cd244
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857659"
---
# <a name="compression-is-recommended-for-replication-traffic"></a>A compactação é recomendada para o tráfego de replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*O tráfego de replicação enviado pela rede do servidor primário para o servidor de réplica é descompactado.*  
  
## <a name="impact"></a>Impacto  
*O tráfego de replicação usará mais largura de banda do que o necessário. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Configure a réplica do Hyper-V para compactar os dados transmitidos pela rede nas configurações da máquina virtual no Gerenciador do Hyper-V. Você também pode usar ferramentas fora do Hyper-V para executar a compactação.*  
  


