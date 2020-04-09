---
title: label
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ccb86e2167682e1048161f2d5f5386a8b5cf6ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841169"
---
# <a name="label"></a>label



Cria, altera ou exclui o rótulo de volume (ou seja, o nome) de um disco. Se usado sem parâmetros, o comando **Label** altera o rótulo do volume atual ou exclui o rótulo existente.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
label [/mp] [<Volume>] [<Label>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/MP|Especifica que o volume deve ser tratado como um ponto de montagem ou nome de volume.|
|\<volume >|Especifica uma letra da unidade (seguida por dois-pontos), ponto de montagem ou nome do volume. Se um nome de volume for especificado, o parâmetro **/MP** será desnecessário.|
|Rótulo de \<>|Especifica o rótulo para o volume.|
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

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para rotular um disco na unidade A que contém informações de vendas de julho, digite:
```
label a:sales-july
```
Para excluir o rótulo atual da unidade C, siga estas etapas:
1. Em um prompt de comando, digite:  
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