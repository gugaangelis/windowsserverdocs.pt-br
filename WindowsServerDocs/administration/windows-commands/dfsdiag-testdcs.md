---
title: Dfsdiag TestDCs
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62956ae65d2311939ac0db6a4b86950f21dba407
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836597"
---
# <a name="dfsdiag-testdcs"></a>Dfsdiag TestDCs

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração de controladores de domínio executando os testes a seguir em cada controlador de domínio no domínio especificado:  
  
-   verifica se o sistema de arquivos distribuído \(DFS\) Namespace serviço está em execução e seu tipo de inicialização está definido como automático.  
  
-   Verifica se há o suporte do site\-custo estimado de referências para o SYSvol e NETLOGON.  
  
-   verifica a consistência da associação do site pelo nome do host e endereço IP.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|\/Domínio:<Domain name>|Domínio que você deseja verificar.|  
  
## <a name="remarks"></a>Comentários  
\/Domínio é um parâmetro opcional. O valor padrão é que o host local tenha ingressado no domínio local.  
  
## <a name="BKMK_Examples"></a>Exemplos  
Para verificar a configuração de controladores de domínio no domínio Contoso.com, digite:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

