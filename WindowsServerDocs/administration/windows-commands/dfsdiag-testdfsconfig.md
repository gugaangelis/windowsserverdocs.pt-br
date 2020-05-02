---
title: dfsdiag TestDFSConfig
description: Tópico de referência para Dfsdiag TestDFSConfig, que verifica a configuração de um namespace de Sistema de Arquivos Distribuído (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 057b0662fddb7148837be16380d190cdb37382c5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719585"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag TestDFSConfig

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração de um namespace de Sistema de Arquivos Distribuído (DFS) executando as seguintes ações:  
  
-   Verifica se o serviço de namespace do DFS está em execução e se seu tipo de inicialização está definido como automático em todos os servidores de namespace.  
  
-   Verifica se a configuração do registro DFS é consistente entre os servidores de namespace.  
  
-   Valida as seguintes dependências em servidores de namespace clusterizados que executam o Windows Server 2008 ou posterior:  
  
    -   Dependência de recurso raiz de namespace no recurso de nome de rede.  
  
    -   Dependência de recurso de nome de rede no recurso de endereço IP.  
  
    -   Dependência de recurso raiz de namespace no recurso de disco físico.

## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
#### <a name="parameters"></a>Parâmetros  
  
|       Parâmetro       |               Descrição               |
|-----------------------|-----------------------------------------|
| /DFSRoot:`<namespace>` | O namespace (raiz DFS) a ser diagnosticado. |
  
## <a name="examples"></a>Exemplos  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

