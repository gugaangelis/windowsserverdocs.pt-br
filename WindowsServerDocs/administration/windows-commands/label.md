---
title: label
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63603eda8d23b6f7b89b8d1ba858575a60e3c65c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724512"
---
# <a name="label"></a>label



Cria, altera ou exclui o rótulo de volume (ou seja, o nome) de um disco. Se usado sem parâmetros, o comando **Label** altera o rótulo do volume atual ou exclui o rótulo existente.



## <a name="syntax"></a>Sintaxe

```
label [/mp] [<Volume>] [<Label>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/mp|Especifica que o volume deve ser tratado como um ponto de montagem ou nome de volume.|
|\<> de volume|Especifica uma letra da unidade (seguida por dois-pontos), ponto de montagem ou nome do volume. Se um nome de volume for especificado, o parâmetro **/MP** será desnecessário.|
|\<Rótulo>|Especifica o rótulo para o volume.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- O Windows exibe o rótulo de volume e o número de série (se tiver um) como parte da listagem de diretório.
- Um rótulo de volume NTFS pode ter até 32 caracteres de comprimento, incluindo espaços. Os rótulos de volume NTFS retêm e exibem o caso que foi usado quando o rótulo foi criado.
- Se você não especificar um valor para o parâmetro **Label** , o comando **Label** exibirá a saída no seguinte formato:  
  ```
  Volume in drive C: xxxxxxxxxxx 
  Volume Serial Number is xxxx-xxxx 
  Volume label (32 characters, ENTER for none)?
  ```  
  Você pode digitar um novo rótulo de volume ou pressionar ENTER para manter o rótulo atual. Se você pressionar ENTER e o volume atualmente tiver um rótulo, o comando **Label** solicitará a você a seguinte mensagem:  
  ```
  Delete current volume label (Y/N)?
  ```  
  Pressione Y para excluir o rótulo ou pressione N para manter o rótulo.

## <a name="examples"></a>Exemplos

Para rotular um disco na unidade A que contém informações de vendas de julho, digite:
```
label a:sales-july
```
Para excluir o rótulo atual da unidade C, siga estas etapas:
1. No prompt de comando, digite:  
   ```
   Label
   ```  
   Uma saída semelhante à seguinte deve ser exibida:  
   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```  
2. Pressione ENTER. O prompt a seguir deve ser exibido:  
   ```
   Delete current volume label (Y/N)?
   ```  
3. Pressione Y para excluir o rótulo atual.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)