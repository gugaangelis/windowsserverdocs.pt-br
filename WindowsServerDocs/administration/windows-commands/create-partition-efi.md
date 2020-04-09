---
title: criar partição EFI
description: Tópico de comandos do Windows para criar partição EFI, que cria uma partição de sistema de EFI (Extensible Firmware Interface) em um disco GPT (tabela de partição GUID) em computadores baseados em Itanium.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1c61f103fc719c8144e942f64172e3be554a414
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847039"
---
# <a name="create-partition-efi"></a>criar partição EFI

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Em computadores baseados em Itanium, o cria uma partição de sistema de EFI (Extensible Firmware Interface) em um disco GPT (tabela de partição GUID).

## <a name="syntax"></a>Sintaxe  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|  Parâmetro  |                                                                                             Descrição                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  tamanho\=<n>  |                         O tamanho da partição em megabytes \(MB\). Se nenhum tamanho for fornecido, a partição continuará até que não haja mais espaço livre na região atual.                         |
| \=de deslocamento <n> |             O deslocamento em kilobytes \(KB\), no qual a partição é criada. Se nenhum deslocamento for fornecido, a partição será colocada na primeira extensão de disco grande o suficiente para contê-la.              |
|    NOERR    | somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |
  
## <a name="remarks"></a>Comentários  
  
-   Depois que a partição tiver sido criada, o foco será fornecido para a nova partição.  
  
-   Um disco GPT deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
Para criar uma partição EFI de 1000 megabytes no disco selecionado, digite:  
  
```  
create partition efi size=1000  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

