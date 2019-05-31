---
title: create partition primary
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbbb4b89540b7264fe907f79ca003117429da08e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881707"
---
# <a name="create-partition-primary"></a>create partition primary

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cria uma partição primária no disco básico com o foco.  
  
> [!CAUTION]  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|size\=<n>|Especifica o tamanho da partição em megabytes \(MB\). Se nenhum tamanho for especificado, a partição continuará até que haja nenhum espaço não alocado na região atual.|  
|offset\=<n>|O deslocamento, em quilobytes \(KB\), no qual a partição é criada. Se o deslocamento não for especificado, a partição será iniciado no início da extensão de disco maior que seja grande o suficiente para contê-la.|  
|align\=<n>|Alinha-se todas as extensões de partição para o limite de alinhamento mais próximo. Normalmente usado com o número de unidade lógica do RAID de hardware \(LUN\) matrizes para melhorar o desempenho. <n> é o número de kilobytes \(KB\) desde o início do disco para o limite de alinhamento mais próximo.|  
|id\={ <byte> &#124; <guid> }|Especifica o tipo de partição. Este parâmetro destina-se ao fabricante de equipamento original \(OEM\) usar somente. Nenhum byte de tipo de partição ou o GUID pode ser especificado com esse parâmetro. DiskPart não verifica o tipo de partição quanto à validade, exceto para assegurar que ele é um byte em formato hexadecimal ou um GUID. **Cuidado:** Criação de partições com esse parâmetro pode fazer com que o computador falhe ou não conseguir iniciar. A menos que você seja um OEM ou um profissional de TI com experiência em discos gpt, não crie partições em discos gpt usando esse parâmetro. Em vez disso, sempre use a **criar partição efi** comando para criar partições de sistema EFI, a **criar partição msr** comando para criar partições Microsoft Reserved e o **criar partição primária** comando \(sem o **id\={byte&#124;guid}** parâmetro\) para criar partições primárias em discos gpt.<br /><br />**Discos de registro mestre de inicialização**<br /><br />para o registro mestre de inicialização \(MBR\) discos, especifique um byte de tipo de partição, em formato hexadecimal, para a partição. Se esse parâmetro não for especificado para um disco MBR, o comando cria uma partição do tipo 0x06, que especifica que um sistema de arquivos não está instalado. Os exemplos incluem:<br /><br />-Partição de dados LDM: 0x42<br />-partição de recuperação: 0x27<br />-Partição de OEM reconhecida: 0x12, 0x84, 0xDE, 0xFE, 0xA0<br /><br />**Discos de tabela de partição GUID**<br /><br />para a tabela de partição GUID \(gpt\) discos, você pode especificar um tipo de partição GUID para a partição que você deseja criar. GUIDs reconhecidos incluem:<br /><br />-Partição do sistema EFI: c12a7328\-f81f\-11d 2\-ba4b\-00a0c93ec93b<br />-Partição Microsoft Reserved: e3c9e316\-0b5c\-4db8\-1!d 817\-f92df00215ae<br />-Partição de dados básica: ebd0a0a2\-b9e5\-4433\-87c 0\-68b6b72699c7<br />-Partição de metadados LDM em um disco dinâmico: 5808c8aa\-7e8f\-42e0\-85d2\-e1e90434cfb3<br />-Partição de dados LDM em um disco dinâmico: af9b60a0\-1431\-4f62\-bc68\-3311714a69ad<br />-partição de recuperação: de94bba4\-1!d 06 1\-4d 40\-a16a\-bfd50179d6ac<br /><br />Se esse parâmetro não for especificado para um disco gpt, o comando cria uma partição de dados básica.|  
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem o GPT, um erro causar o DiskPart sair com um código de erro.|  
  
## <a name="remarks"></a>Comentários  
  
-   Depois de criar a partição, o foco mudará automaticamente para a nova partição.  
  
-   A partição não recebe uma letra de unidade. Você deve usar o **atribuir** comando DiskPart para atribuir uma letra de unidade para a partição.  
  
-   Um disco básico deve ser selecionado para essa operação seja bem-sucedida. Use o **Selecionar disco** comando para selecionar um disco básico e mudar o foco a ele.  
  
## <a name="BKMK_examples"></a>Exemplos  
Para criar uma partição primária de 1000 megabytes em tamanho, digite:  
  
```  
create partition primary size=1000  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

  
