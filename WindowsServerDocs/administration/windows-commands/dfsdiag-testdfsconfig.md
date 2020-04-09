---
title: dfsdiag TestDFSConfig
description: Tópico de comandos do Windows para Dfsdiag TestDFSConfig, que verifica a configuração de um namespace de Sistema de Arquivos Distribuído (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ffb75ba26b4ed90dbf5c8bfda80f4a81f986e46a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846318"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag TestDFSConfig

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
  
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

