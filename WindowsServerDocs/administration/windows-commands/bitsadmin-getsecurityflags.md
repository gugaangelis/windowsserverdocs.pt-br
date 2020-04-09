---
title: bitsadmin getsecurityflags
description: O tópico de comandos do Windows para **Bitsadmin getsecurityflags**, que relata os sinalizadores de segurança http para redirecionamento de URL e verificações executadas no certificado do servidor durante a transferência.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 360f8d5514e5251dd9e4a6a6b60335dc34fe4415
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850469"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Relata os sinalizadores de segurança HTTP para o redirecionamento de URL e as verificações executadas no certificado do servidor durante a transferência.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getsecurityflags <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera os sinalizadores de segurança de um trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getsecurityflags myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)