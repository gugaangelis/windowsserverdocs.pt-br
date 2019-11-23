---
title: bitsadmin setaclflag
description: O tópico de comandos do Windows para **Bitsadmin setaclflag** -define os sinalizadores de propagações da lista de controle de acesso.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fbdb12c29af7b4db8b25846d43ee1c93b2454ff2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380760"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Define os sinalizadores de propagações da ACL (lista de controle de acesso) para o trabalho. Os sinalizadores indicam que você deseja manter as informações de proprietário e ACL com o arquivo que está sendo baixado. Por exemplo, para manter o proprietário e o grupo com o arquivo, defina **sinalizadores** como `OG`.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetAclFlags <Job> <Flags>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Sinalizadores|Especifique um ou mais dos seguintes valores de sinalizador:</br>-O: copiar informações do proprietário com o arquivo.</br>-G: copiar informações do grupo com o arquivo.</br>-D: copiar informações de DACL com o arquivo.</br>-S: copiar informações de SACL com o arquivo.|

## <a name="remarks"></a>Comentários

A opção SetAclFlags é usada para manter informações de proprietário e de lista de controle de acesso quando um trabalho está baixando dados de um compartilhamento do Windows (SMB).

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir define os sinalizadores de propagação da lista de controle de acesso para o trabalho chamado *myDownloadJob* para manter as informações de proprietário e grupo com os arquivos baixados.
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)