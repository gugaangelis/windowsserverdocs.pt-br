---
title: dfsdiag TestSites
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: af72da64dd20d4b37824355a494cb8f97f597b28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378383"
---
# <a name="dfsdiag-testsites"></a>dfsdiag TestSites

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração dos serviços de domínio do Active Directory \(AD DS\) sites verificando se os servidores que atuam como servidores de namespace ou pasta \(link\) destinos têm as mesmas associações de site em todos os controladores de domínio.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|Computador \/:<server name>|O nome do servidor no qual verificar a associação do site.|  
|\/DFSpath:<namespace root or DFS folder>|A raiz do namespace ou Sistema de Arquivos Distribuído \(pasta\) do DFS \(link\) com destinos para os quais verificar a associação do site.|  
|recurse \/|Enumera e verifica as associações de site para todos os destinos de pasta na raiz de namespace especificada.|  
|\/completo|verifica se AD DS e o registro do servidor contêm as mesmas informações de associação do site.|  
  
## <a name="BKMK_Examples"></a>Disso  
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
  
## <a name="additional-references"></a>referências adicionais  
  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

