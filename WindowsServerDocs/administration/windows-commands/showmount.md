---
title: showmount
description: Tópico de comandos do Windows para o createmount, que exibe diretórios montados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fa61d47bb14cf21d93beec0a6e9257b9f66737b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834219"
---
# <a name="showmount"></a>showmount

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar a **montagem** para exibir diretórios montados.  
  
## <a name="syntax"></a>Sintaxe  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Descrição  
O comando de **desmontagem**\-utilitário de linha exibe informações sobre sistemas de arquivos montados exportados pelo servidor para NFS no computador especificado pelo *servidor*. Se o *servidor* não for fornecido, a **montagem** exibirá informações sobre o computador no qual o comando de **conmontagem** é executado.  
  
Você deve fornecer uma das seguintes opções:  
  
- **\-e** -exibe todos os sistemas de arquivos exportados no servidor.  
- **\-a** -exibe todos os clientes do sistema de arquivos de rede \(NFS\) e os diretórios no servidor que cada um tiver montado.  
- **\-d** – exibe todos os diretórios no servidor que estão montados atualmente por clientes NFS.  
  
## <a name="see-also"></a>Consulte também  
[Referência aos comandos dos Serviços para Network File System](services-for-network-file-system-command-reference.md)  