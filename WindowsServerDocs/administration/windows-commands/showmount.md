---
title: showmount
description: Tópico de referência para o inmount, que exibe diretórios montados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60057c154e7a646d14a0e57d5cf4cd72d0f90876
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721809"
---
# <a name="showmount"></a>showmount

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar a **montagem** para exibir diretórios montados.  
  
## <a name="syntax"></a>Sintaxe  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Descrição  
O utilitário de linha\-de comando de **addmount** exibe informações sobre sistemas de arquivos montados exportados pelo servidor para NFS no computador especificado pelo *servidor*. Se o *servidor* não for fornecido, a **montagem** exibirá informações sobre o computador no qual o comando de **conmontagem** é executado.  
  
Você deve fornecer uma das seguintes opções:  
  
- e-exibe todos os sistemas de arquivos exportados no servidor. ** \-**  
- a-exibe todos os clientes NFS \(\) do sistema de arquivos de rede e os diretórios no servidor que cada um montou. ** \-**  
- d-exibe todos os diretórios no servidor que estão atualmente montados por clientes NFS. ** \-**  
  
## <a name="see-also"></a>Consulte Também  
[Referência aos comandos dos Serviços para Network File System](services-for-network-file-system-command-reference.md)  