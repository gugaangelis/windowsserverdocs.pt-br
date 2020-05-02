---
title: dfsdiag TestSites
description: Tópico de referência para Dfsdiag TestSites, que verifica a configuração dos sites dos serviços de domínio Active Directory (AD DS), verificando se os servidores que atuam como servidores de namespace ou destinos de pasta (link) têm as mesmas associações de site em todos os controladores de domínio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68048699a812beac94fa121d6801da5f42e5393b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719560"
---
# <a name="dfsdiag-testsites"></a>dfsdiag TestSites

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração dos sites dos serviços de domínio do Active Directory (AD DS) verificando se os servidores que atuam como servidores de namespace ou destinos de pasta (link) têm as mesmas associações de site em todos os controladores de domínio.

## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
#### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|\/Tradução<server name>|O nome do servidor no qual verificar a associação do site.|  
|\/DFSpath<namespace root or DFS folder>|A raiz do namespace ou a pasta do Sistema de Arquivos Distribuído (DFS) (link) com destinos para os quais verificar a associação do site.|  
|\/Recurse|Enumera e verifica as associações de site para todos os destinos de pasta na raiz de namespace especificada.|  
|\/Completo|verifica se AD DS e o registro do servidor contêm as mesmas informações de associação do site.|  
  
## <a name="examples"></a>Exemplos  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
 
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

