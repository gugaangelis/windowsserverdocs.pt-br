---
title: Associação de domínio é recomendada para servidores que executam o Hyper-V
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f4578e5-0848-46b4-a50b-7dbd480b80bf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e9db1d28cfe1ae4afd6c5dc1a93253c83fc42113
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860897"
---
# <a name="domain-membership-is-recommended-for-servers-running-hyper-v"></a>Associação de domínio é recomendada para servidores que executam o Hyper-V

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
  
*Esse servidor é um membro de um grupo de trabalho.*  
  
## <a name="impact"></a>Impacto  
  
*Não há nenhum gerenciamento central para esse servidor.*  
  
Adicionar este computador ao domínio permite o gerenciamento centralizado por meio de políticas de identidade, segurança e auditoria.  
  
## <a name="resolution"></a>Resolução  
  
*Se você tiver um ambiente de domínio disponível, associe esse servidor para esse domínio.*  
  
> [!IMPORTANT]  
> É recomendável que você examine as cargas de trabalho em execução nas máquinas virtuais neste computador para determinar se há implicações de segurança de ingressar esse computador a um domínio. Se qualquer uma das máquinas virtuais são controladores de domínio virtualizados, consulte [considerações sobre o planejamento para controladores de domínio virtualizados](https://go.microsoft.com/fwlink/?LinkId=190192) (https://go.microsoft.com/fwlink/?LinkId=190192).  
  
Ingressar um computador em um domínio requer permissões no computador e o domínio:   
- No computador, você precisará de uma conta de usuário que seja membro do grupo Administradores. Ou faça logon com esse tipo de conta ou forneça o nome de usuário e senha para a conta quando solicitado.   
- No domínio, você precisará de uma conta de usuário que tem autorizados a ingressar o computador no domínio. Você será solicitado o nome de usuário e senha.  
  
Para obter instruções, consulte [ingressar o computador ao domínio](https://go.microsoft.com/fwlink/?LinkId=190193) (https://go.microsoft.com/fwlink/?LinkId=190193).  
  


