---
title: showmount
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a6dd562e-e3bd-4ee6-be3b-6d29e29fd20e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26acf24922e6ac53a5c902d65eb0f23bff0af93b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852347"
---
# <a name="showmount"></a>showmount

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar **showmount** exibir diretórios montados.  
  
## <a name="syntax"></a>Sintaxe  
```
showmount {-e|-a|-d} <Server>  
```

## <a name="description"></a>Descrição  
O **showmount** comando\-utilitário de linha exibe informações sobre os sistemas de arquivos montados exportado pelo servidor para NFS no computador especificado por *servidor*. Se *Server* não for fornecido, **showmount** exibe informações sobre o computador no qual o **showmount** comando é executado.  
  
Você deve fornecer uma das seguintes opções:  
  
- **\-e** -exibe todos os sistemas exportados no servidor de arquivos.  
- **\-uma** -exibe todos os Network File System \(NFS\) clientes e os diretórios no servidor de cada um tiver montado.  
- **\-1!d** -exibe todos os diretórios no servidor que são montadas no momento por clientes NFS.  
  
## <a name="see-also"></a>Consulte também  
[Serviços para referência de comandos do sistema de arquivos de rede](services-for-network-file-system-command-reference.md)  