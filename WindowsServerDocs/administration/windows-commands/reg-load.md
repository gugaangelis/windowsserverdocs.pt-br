---
title: carga reg
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2523876e2ea2305ede3289226c934c9352825ba9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722543"
---
# <a name="reg-load"></a>carga reg



Grava subchaves e entradas salvas em uma subchave diferente no registro. Destinado a uso com arquivos temporários que são usados para solução de problemas ou edição de entradas do registro.



## <a name="syntax"></a>Sintaxe

```
reg load KeyName FileName
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave a ser carregada. Para especificar computadores remotos, inclua o nome do computador (no \\ \\formato ComputerName\) como parte do *KeyName*. \\ \\Omitir computername \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão: HKLM e HKU.|
|\<Nome de arquivo>|Especifica o nome e o caminho do arquivo a ser carregado. Esse arquivo deve ser criado com antecedência usando a operação **reg Save** e uma extensão. HIV.|
|/?|Exibe a ajuda para a **carga reg** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores de retorno para a operação **reg Load** .

|Valor|Descrição|
|-----|-----------|
|0|Sucesso|
|1|Falha|

## <a name="examples"></a>Exemplos

Para carregar o arquivo chamado TempHive. HIV na chave HKLM\TempHive, digite:
```
REG LOAD HKLM\TempHive TempHive.hiv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)