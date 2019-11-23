---
title: dfsdiag TestDFSConfig
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8008e02d588edaa6fe7700a331c43f9680d89431
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378424"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag TestDFSConfig

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração de um Sistema de Arquivos Distribuído \(namespace\) do DFS executando as seguintes ações:  
  
-   verifica se o serviço de namespace do DFS está em execução e se seu tipo de inicialização está definido como automático em todos os servidores de namespace.  
  
-   verifica se a configuração do registro DFS é consistente entre os servidores de namespace.  
  
-   Valida as seguintes dependências em servidores de namespace clusterizados que executam o Windows Server 2008 ou posterior:  
  
    -   Dependência de recurso raiz de namespace no recurso de nome de rede.  
  
    -   Dependência de recurso de nome de rede no recurso de endereço IP.  
  
    -   Dependência de recurso raiz de namespace no recurso de disco físico.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|       Parâmetro       |               Descrição               |
|-----------------------|-----------------------------------------|
| \/DFSRoot:<namespace> | O namespace \(\) da raiz DFS para diagnosticar. |
  
## <a name="BKMK_Examples"></a>Disso  
Para TBD, digite:  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>referências adicionais  
  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

