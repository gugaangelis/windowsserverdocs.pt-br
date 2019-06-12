---
title: dfsdiag TestDFSConfig
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 922b78b87f3bb66765b87348a3bf136e14c9e837
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436135"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag TestDFSConfig

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração de um sistema de arquivos distribuído \(DFS\) namespace executando as seguintes ações:  
  
-   verifica se o serviço Namespace DFS está em execução e que o tipo de inicialização está definido como automático em todos os servidores de namespace.  
  
-   verifica se a configuração de registro do DFS é consistente entre servidores de namespace.  
  
-   Valida as seguintes dependências nos servidores de namespace em cluster que executam o Windows Server 2008 ou posterior:  
  
    -   Dependência de recurso de raiz de Namespace no recurso de nome de rede.  
  
    -   Dependência de recurso de nome de rede no recurso de endereço IP.  
  
    -   Dependência de recurso de raiz de Namespace no recurso de disco físico.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|       Parâmetro       |               Descrição               |
|-----------------------|-----------------------------------------|
| \/DFSRoot:<namespace> | O namespace \(raiz DFS\) para diagnosticar. |
  
## <a name="BKMK_Examples"></a>Exemplos  
Para TBD, digite:  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

