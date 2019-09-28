---
title: A associação de domínio é recomendada para servidores que executam o Hyper-V
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f4578e5-0848-46b4-a50b-7dbd480b80bf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 48ea52e962f2f476d1428a69bab6c6e38c4ec005
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364916"
---
# <a name="domain-membership-is-recommended-for-servers-running-hyper-v"></a>A associação de domínio é recomendada para servidores que executam o Hyper-V

>Aplica-se a: Windows Server 2016


  
*Para obter mais informações sobre práticas recomendadas e verificações, consulte* [analisador de práticas recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Este servidor é membro de um grupo de trabalho.*  
  
## <a name="impact"></a>Impacto  
  
*Não há gerenciamento central para este servidor.*  
  
Ingressar esse computador no domínio permite o gerenciamento centralizado por meio de políticas de identidade, segurança e auditoria.  
  
## <a name="resolution"></a>Resolução  
  
*Se você tiver um ambiente de domínio disponível, ingresse esse servidor nesse domínio.*  
  
> [!IMPORTANT]  
> Recomendamos que você revise as cargas de trabalho em execução nas máquinas virtuais neste computador para determinar se há implicações de segurança ao ingressar este computador em um domínio. Se qualquer uma das máquinas virtuais forem controladores de domínio virtualizados, consulte [considerações de planejamento para controladores de domínio virtualizados](https://go.microsoft.com/fwlink/?LinkId=190192) (https://go.microsoft.com/fwlink/?LinkId=190192).  
  
Ingressar um computador em um domínio requer permissões no computador e no domínio:   
- No computador, você precisará de uma conta de usuário que seja membro do grupo Administradores. Faça logon com esse tipo de conta ou forneça o nome de usuário e a senha da conta quando for solicitado.   
- No domínio, você precisará de uma conta de usuário que esteja autorizada a ingressar o computador no domínio. Você será solicitado a fornecer o nome de usuário e a senha.  
  
Para obter instruções, consulte [unir o computador ao domínio](https://go.microsoft.com/fwlink/?LinkId=190193) (https://go.microsoft.com/fwlink/?LinkId=190193).  
  


