---
title: bitsadmin setaclflag
description: Tópico de comandos do Windows para **setaclflag bitsadmin** -define o controle de acesso de sinalizadores de propagação de lista.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89d825a4bc4512022fed98a3188537d3977fa3c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867397"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Define o controle de acesso de sinalizadores de propagação de ACL (lista) para o trabalho. Os sinalizadores indicam que você deseja manter o proprietário e as informações de ACL com o arquivo que está sendo baixado. Por exemplo, para manter o proprietário e o grupo com o arquivo, defina **sinalizadores** para `OG`.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetAclFlags <Job> <Flags>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Sinalizadores|Especifique um ou mais dos seguintes valores de sinalizador:</br>-/O: Copie informações do proprietário com o arquivo.</br>-   G: Copie informações de grupo com o arquivo.</br>-UNIDADE D: Copie informações da DACL com o arquivo.</br>-S: cópia SACL informações com o arquivo.|

## <a name="remarks"></a>Comentários

A opção SetAclFlags é usada para manter informações de lista de controle de acesso e de proprietário quando um trabalho é Baixando dados de um compartilhamento do Windows (SMB).

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir define o controle de acesso de sinalizadores de propagação de lista do trabalho nomeado *myDownloadJob* para manter as informações do proprietário e o grupo com os arquivos baixados.
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)