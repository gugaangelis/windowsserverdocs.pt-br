---
title: bitsadmin setaclflag
description: Tópico de comandos do Windows para Bitsadmin setaclflag, que define os sinalizadores de propagações da lista de controle de acesso.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ac47e554dde6a555e891d89668cd12fec3179d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849669"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Define os sinalizadores de propagações da ACL (lista de controle de acesso) para o trabalho. Os sinalizadores indicam que você deseja manter as informações de proprietário e ACL com o arquivo que está sendo baixado. Por exemplo, para manter o proprietário e o grupo com o arquivo, defina **sinalizadores** como `OG`.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetAclFlags <Job> <Flags>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|Sinalizadores|Especifique um ou mais dos seguintes valores de sinalizador:</br>-O: copiar informações do proprietário com o arquivo.</br>-G: copiar informações do grupo com o arquivo.</br>-D: copiar informações de DACL com o arquivo.</br>-S: copiar informações de SACL com o arquivo.|

## <a name="remarks"></a>Comentários

A opção SetAclFlags é usada para manter informações de proprietário e de lista de controle de acesso quando um trabalho está baixando dados de um compartilhamento do Windows (SMB).

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define os sinalizadores de propagação da lista de controle de acesso para o trabalho chamado *myDownloadJob* para manter as informações de proprietário e grupo com os arquivos baixados.
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)