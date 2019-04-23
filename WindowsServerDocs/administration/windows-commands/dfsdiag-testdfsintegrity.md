---
title: dfsdiag TestDFSIntegrity
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9a79e034f7c60be89266eb29dcd69e8f73b2aafe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837087"
---
# <a name="dfsdiag-testdfsintegrity"></a>dfsdiag TestDFSIntegrity

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a integridade do sistema de arquivos distribuído \(DFS\) namespace executando os seguintes testes:  
  
-   Verifica se há corrupção de metadados do DFS ou inconsistências entre os controladores de domínio.  
  
-   Valida a configuração de acesso\-com base em enumeração para garantir que ele seja consistente entre os metadados DFS e o compartilhamento do servidor de namespace.  
  
-   Detecta as pastas DFS sobrepostas \(links\), pastas duplicadas e pastas com destinos de pasta de sobreposição.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|\/DFSRoot:<DFS root path>|O namespace do DFS para diagnosticar.|  
|\/Recurse|Executa o teste, incluindo que o namespace do interlinks.|  
|\/completo|verifica a consistência de compartilhamento e configuração do lado do cliente e Acls do NTFS em todos os destinos de pasta. Ela também verifica que a propriedade online está definida.|  
  
## <a name="BKMK_Examples"></a>Exemplos  
Para TBD, digite:  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

