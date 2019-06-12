---
title: nslookup root
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47a26be99a5eee510970d3eee6b486331a98b159
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436902"
---
# <a name="nslookup-root"></a>nslookup root

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o servidor padrão para o servidor para a raiz do espaço de nome de domínio de sistema de nome de domínio (DNS).
## <a name="syntax"></a>Sintaxe
```
root 
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                      Descrição                      |
|-----------------|-------------------------------------------------------|
| {Ajuda &#124; ?} | Exibe um resumo breve dos **nslookup** subcomandos. |

## <a name="remarks"></a>Comentários
- Atualmente, o servidor de nomes ddn.mil é usado. Esse comando é um sinônimo de lserver ddn.mil. Você pode alterar o nome do servidor raiz com o **raiz do conjunto** comando.
  ## <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup definir raiz](nslookup-set-root.md)
