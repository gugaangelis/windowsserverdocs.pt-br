---
title: dfsdiag TestDCs
description: Tópico de referência para Dfsdiag TestDCs, que verifica a configuração de controladores de domínio no domínio especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ac7fe1a7bae6a7b3dab9004b6212b7d93774ade
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719601"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag TestDCs

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração de controladores de domínio executando os seguintes testes em cada controlador de domínio no domínio especificado:  
  
-   Verifica se o serviço de namespace do Sistema de Arquivos Distribuído (DFS) está em execução e se seu tipo de inicialização está definido como automático.  
  
-   Verifica o suporte de referências com custo de site para NETLOGON e SYSvol.  
  
-   Verifica a consistência da associação do site pelo nome do host e endereço IP.

## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
#### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|Controlador`<domain_name>`|Domínio que você deseja verificar.|  
  
## <a name="remarks"></a>Comentários  

/Domain é um parâmetro opcional. O valor padrão é o domínio local ao qual o host local está associado.  
  
## <a name="examples"></a>Exemplos  
Para verificar a configuração dos controladores de domínio no domínio Contoso.com, digite:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

