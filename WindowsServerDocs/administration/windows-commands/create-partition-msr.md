---
title: criar partição MSR
description: Tópico de referência para criar partição MSR, que cria uma partição reservada da Microsoft (MSR) em um disco de tabela de partição GUID (GPT).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 964f57b08a23867c01d3e90e9b8283c7b4fff6e5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719213"
---
# <a name="create-partition-msr"></a>criar partição MSR

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria uma partição MSR (reservada da Microsoft) em um disco de tabela de partição GUID (GPT).
  
> [!CAUTION]  
> Tenha cuidado ao usar esse comando. Como os discos GPT exigem um layout de partição específico, a criação de partições reservadas da Microsoft pode fazer com que o disco se torne ilegível.
  
## <a name="syntax"></a>Sintaxe  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|  Parâmetro  |                                                                                                                         Descrição                                                                                                                         |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  tamanho\=<n>  |               O tamanho da partição em megabytes \(MB.\) A partição é pelo menos tão longa em bytes quanto o número especificado por <n>. Se nenhum tamanho for fornecido, a partição continuará até que não haja mais espaço livre na região atual.               |
| desvio\=<n> | Especifica o deslocamento em kilobytes \(KB\), no qual a partição é criada. O deslocamento é arredondado para preencher completamente qualquer tamanho de setor usado. Se nenhum deslocamento for fornecido, a partição será colocada na primeira extensão de disco grande o suficiente para contê-la. |
|    NOERR    |                            somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.                             |
  
## <a name="remarks"></a>Comentários  
  
-   Em discos GPT que são usados para inicializar o sistema operacional Windows, a partição de sistema \(EFI\) da interface de firmware extensível é a primeira partição no disco, seguida pela partição reservada da Microsoft. os discos GPT que são usados somente para armazenamento de dados não têm uma partição de sistema EFI; nesse caso, a partição reservada da Microsoft é a primeira partição.  
  
-   O Windows não monta partições reservadas da Microsoft. Você não pode armazenar dados neles e não pode excluí-los.  
  
-   Uma partição reservada da Microsoft é necessária em todos os discos GPT. O tamanho dessa partição depende do tamanho total do disco GPT. O tamanho do disco GPT deve ter pelo menos 32 MB para criar uma partição reservada da Microsoft.  
  
-   Um disco GPT básico deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco GPT básico e deslocar o foco para ele.  
  
## <a name="examples"></a>Exemplos  
Para criar uma partição reservada da Microsoft de 1000 megabytes de tamanho, digite:  
  
```  
create partition msr size=1000  
```  
  
## <a name="additional-references"></a>Referências adicionais  
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

  

