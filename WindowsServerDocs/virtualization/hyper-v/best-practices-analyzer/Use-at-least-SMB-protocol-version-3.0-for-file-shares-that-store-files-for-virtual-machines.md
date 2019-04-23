---
title: Use pelo menos versão do protocolo SMB 3.0 para compartilhamentos de arquivos que armazenam arquivos de máquinas virtuais.
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4bb832b8-f1aa-4c1f-a0f2-324dd53553ea
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 28e0f3769fd4fc993710d0a0b800dfad7c9ab157
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834337"
---
# <a name="use-at-least-smb-protocol-version-30-for-file-shares-that-store-files-for-virtual-machines"></a>Use pelo menos versão do protocolo SMB 3.0 para compartilhamentos de arquivos que armazenam arquivos de máquinas virtuais.

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Arquivos de máquina virtual ou disco rígido virtual são armazenados em um compartilhamento de arquivos que não oferece suporte a pelo menos versão do protocolo SMB 3.0.*  
  
## <a name="impact"></a>**Impacto**  
*Microsoft não oferece suporte a essa configuração. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Mova os arquivos para um compartilhamento de arquivo usa pelo menos versão do protocolo SMB 3.0.*  
  


