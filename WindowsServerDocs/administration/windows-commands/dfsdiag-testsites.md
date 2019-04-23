---
title: Dfsdiag TestSites
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6486c79cfa58bb262fd3161ad0801e84185b6629
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873967"
---
# <a name="dfsdiag-testsites"></a>Dfsdiag TestSites

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração dos serviços de domínio do Active Directory \(AD DS\) sites, confirmando que os servidores que atuam como servidores de namespace ou pasta \(link\) destinos têm as mesmas associações de site no domínio de todos os controladores.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|\/Computador:<server name>|O nome do servidor no qual verificar a associação do site.|  
|\/DFSpath:<namespace root or DFS folder>|O namespace raiz ou o sistema de arquivos distribuído \(DFS\) pasta \(link\) com destinos para o qual verificar a associação do site.|  
|\/Recurse|Enumera e verifica as associações de site para todos os destinos de pasta na raiz do namespace especificado.|  
|\/completo|verifica que o AD DS e o registro do servidor contêm as mesmas informações de associação do site.|  
  
## <a name="BKMK_Examples"></a>Exemplos  
Para TBD, digite:  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
  
Para TBD, digite:  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
Para TBD, digite:  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

