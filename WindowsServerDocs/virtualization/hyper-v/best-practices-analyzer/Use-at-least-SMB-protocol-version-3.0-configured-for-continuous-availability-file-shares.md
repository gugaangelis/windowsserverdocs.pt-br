---
title: Use pelo menos a versão 3,0 do protocolo SMB configurada para disponibilidade contínua em compartilhamentos de arquivos que armazenam arquivos para máquinas virtuais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3a6cbb6052e2e50b7fd78792c5e01885d7672932
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393344"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>Use pelo menos a versão 3,0 do protocolo SMB configurada para disponibilidade contínua em compartilhamentos de arquivos que armazenam arquivos para máquinas virtuais

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
*Os arquivos de máquina virtual ou os arquivos de disco rígido virtual são armazenados em um compartilhamento de arquivos de rede que não está configurado com o recurso de disponibilidade contínua do protocolo SMB versão 3,0.*  
  
## <a name="impact"></a>**Causa**  
*Microsoft não recomenda essa configuração porque ela pode afetar a disponibilidade das máquinas virtuais usando o servidor. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
Siga um destes procedimentos:  
  
-   Mova os arquivos para um compartilhamento de arquivos SMB 3,0 configurado para disponibilidade contínua.  
  
-   Reconfigure o compartilhamento de arquivos atual para fornecer disponibilidade contínua.  
  


