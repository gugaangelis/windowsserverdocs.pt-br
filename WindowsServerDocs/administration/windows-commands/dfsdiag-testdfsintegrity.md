---
title: dfsdiag TestDFSIntegrity
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f344e2d1fecc542efc39688f20165fd3e39a04a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378433"
---
# <a name="dfsdiag-testdfsintegrity"></a>dfsdiag TestDFSIntegrity

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a integridade da Sistema de Arquivos Distribuído \(namespace\) do DFS executando os seguintes testes:  
  
-   Verifica se há danos ou inconsistências nos metadados do DFS entre os controladores de domínio.  
  
-   Valida a configuração de enumeração baseada em\-de acesso para garantir que seja consistente entre os metadados do DFS e o compartilhamento do servidor de namespace.  
  
-   Detecta pastas DFS sobrepostas \(links\), pastas duplicadas e pastas com destinos de pasta sobrepostos.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|\/DFSRoot:<DFS root path>|O namespace do DFS para diagnosticar.|  
|recurse \/|Executa os testes, incluindo os Interlinks de namespace.|  
|\/completo|verifica a consistência do compartilhamento e das ACLs de NTFS e da configuração do lado do cliente em todos os destinos de pasta. Ele também verifica se a propriedade online está definida.|  
  
## <a name="BKMK_Examples"></a>Disso  
Para TBD, digite:  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full  
```  
  
## <a name="additional-references"></a>referências adicionais  
  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

