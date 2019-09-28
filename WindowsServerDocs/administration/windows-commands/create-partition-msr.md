---
title: criar partição MSR
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45cc215b097ce048b15f0e907f95f976e4941e28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378901"
---
# <a name="create-partition-msr"></a>criar partição MSR

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria uma partição do Microsoft \(MSR @ no__t-1 reservado em uma tabela de partição GUID \(gpt @ no__t-3 Disk.  
  
> [!CAUTION]  
> Tenha cuidado ao usar esse comando. Como os discos GPT exigem um layout de partição específico, a criação de partições reservadas da Microsoft pode fazer com que o disco se torne ilegível.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|  Parâmetro  |                                                                                                                         Descrição                                                                                                                         |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  tamanho @ no__t-0 @ no__t-1  |               O tamanho da partição em megabytes \(MB @ no__t-1. A partição é pelo menos tão longa em bytes quanto o número especificado por <n>. Se nenhum tamanho for fornecido, a partição continuará até que não haja mais espaço livre na região atual.               |
| deslocamento @ no__t-0 @ no__t-1 | Especifica o deslocamento em kilobytes \(KB @ no__t-1, no qual a partição é criada. O deslocamento é arredondado para preencher completamente qualquer tamanho de setor usado. Se nenhum deslocamento for fornecido, a partição será colocada na primeira extensão de disco grande o suficiente para contê-la. |
|    NOERR    |                            Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                             |
  
## <a name="remarks"></a>Comentários  
  
-   Em discos GPT que são usados para inicializar o sistema operacional Windows, a interface de sistema extensível \(EFI @ no__t-1 é a primeira partição no disco, seguida pela partição reservada da Microsoft. os discos GPT que são usados somente para armazenamento de dados não têm uma partição de sistema EFI; nesse caso, a partição reservada da Microsoft é a primeira partição.  
  
-   O Windows não monta partições reservadas da Microsoft. Você não pode armazenar dados neles e não pode excluí-los.  
  
-   Uma partição reservada da Microsoft é necessária em todos os discos GPT. O tamanho dessa partição depende do tamanho total do disco GPT. O tamanho do disco GPT deve ter pelo menos 32 MB para criar uma partição reservada da Microsoft.  
  
-   Um disco GPT básico deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco GPT básico e deslocar o foco para ele.  
  
## <a name="BKMK_examples"></a>Disso  
Para criar uma partição reservada da Microsoft de 1000 megabytes de tamanho, digite:  
  
```  
create partition msr size=1000  
```  
  
#### <a name="additional-references"></a>Referências adicionais  
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

