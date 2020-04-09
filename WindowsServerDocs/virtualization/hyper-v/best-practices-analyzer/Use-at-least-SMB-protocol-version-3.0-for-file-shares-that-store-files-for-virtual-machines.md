---
title: Use pelo menos o protocolo SMB versão 3,0 para compartilhamentos de arquivos que armazenam arquivos para máquinas virtuais.
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4bb832b8-f1aa-4c1f-a0f2-324dd53553ea
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 64c78c4725c84cbd02276cb2c0eadffae42d7060
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854189"
---
# <a name="use-at-least-smb-protocol-version-30-for-file-shares-that-store-files-for-virtual-machines"></a>Use pelo menos o protocolo SMB versão 3,0 para compartilhamentos de arquivos que armazenam arquivos para máquinas virtuais.

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Error|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Os arquivos de máquina virtual ou os arquivos de disco rígido virtual são armazenados em um compartilhamento de arquivos que não dá suporte ao protocolo SMB versão 3,0.*  
  
## <a name="impact"></a>**Causa**  
*A Microsoft não oferece suporte a essa configuração. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Mova os arquivos para um compartilhamento de arquivos que usa pelo menos o protocolo SMB versão 3,0.*  
  


