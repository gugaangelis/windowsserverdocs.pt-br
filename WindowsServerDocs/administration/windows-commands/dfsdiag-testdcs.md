---
title: dfsdiag TestDCs
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a193e68b6f015b1535a98e20b52deb2a4a14034c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378443"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag TestDCs

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração de controladores de domínio executando os seguintes testes em cada controlador de domínio no domínio especificado:  
  
-   verifica se o serviço namespace\) \(Sistema de Arquivos Distribuído do DFS está em execução e se seu tipo de inicialização está definido como automático.  
  
-   Verifica o suporte do site\-referências com custo para NETLOGON e SYSvol.  
  
-   verifica a consistência da associação do site pelo nome do host e endereço IP.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|\/domínio:<Domain name>|Domínio que você deseja verificar.|  
  
## <a name="remarks"></a>Comentários  
\/domínio é um parâmetro opcional. O valor padrão é o domínio local ao qual o host local está associado.  
  
## <a name="BKMK_Examples"></a>Disso  
Para verificar a configuração dos controladores de domínio no domínio Contoso.com, digite:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>referências adicionais  
  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

