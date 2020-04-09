---
title: Evite armazenar arquivos de paginação inteligente em um disco do sistema
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 111cf3b3b95f50d6d36a6b30b5a0bb46e255ee28
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857729"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Evite armazenar arquivos de paginação inteligente em um disco do sistema

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Operações|  
  
Nas seções a seguir, os itálicos indicam o texto que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*A configuração de memória para uma ou mais máquinas virtuais pode exigir o uso de paginação inteligente se a máquina virtual for reinicializada e o local especificado para o arquivo de paginação inteligente é o disco do sistema do servidor que executa o Hyper-V.*  
  
## <a name="impact"></a>Impacto  
*O uso do disco do sistema para paginação inteligente pode fazer com que o servidor que executa o Hyper-V tenha problemas. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Reconfigure as máquinas virtuais para armazenar os arquivos de paginação inteligente em um disco que não seja do sistema.*  
  


