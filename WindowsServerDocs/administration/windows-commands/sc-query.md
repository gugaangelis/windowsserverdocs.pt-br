---
title: Consulta SC
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60b6e945c4b2944f97d40cbc27694acc2915c615
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441623"
---
# <a name="sc-query"></a>Consulta SC



Obtém e exibe informações sobre o serviço especificado, o driver, o tipo de serviço ou o tipo de driver.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
sc [<ServerName>] query [<ServiceName>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <BufferSize>] [ri= <ResumeIndex>] [group= <GroupName>]
```

## <a name="parameters"></a>Parâmetros

|       Parâmetro        |                                                                                                                          Descrição                                                                                                                          |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     \<ServerName>      |                       Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato de convenção de nomenclatura Universal (UNC) (por exemplo, \\ \\myserver). Para executar SC.exe localmente, omita este parâmetro.                        |
|     \<ServiceName>     |                                      Especifica o nome do serviço retornado pelo **getkeyname** operação. Isso **consulta** parâmetro não é usado em conjunto com outras **consulta** parâmetros (diferente de *ServerName*).                                      |
|     type= {driver      |                                                                                                                            serviço                                                                                                                            |
|       type= {own       |                                                                                                                             compartilhar                                                                                                                             |
|     estado = {Active Directory     |                                                                                                                           inativo                                                                                                                            |
| bufsize= \<BufferSize> |                     Especifica o tamanho (em bytes) do buffer de enumeração. O tamanho do buffer padrão é 1.024 bytes. Você deve aumentar o tamanho do buffer de enumeração quando o resultado de uma consulta ultrapassa 1.024 bytes.                      |
|   ri= \<ResumeIndex>   | Especifica o número de índice no qual a enumeração é iniciar ou retomar. O valor padrão é **0** (zero). Use esse parâmetro junto com o **bufsize =** parâmetro quando mais de informações são retornadas por uma consulta do buffer padrão pode exibir. |
|  group= \<GroupName>   |                                                                             Especifica o grupo de serviço a serem enumerados. Por padrão, todos os grupos são enumerados (**grupo = ""** ).                                                                              |
|           /?           |                                                                                                             Exibe a ajuda no prompt de comando.                                                                                                              |

## <a name="remarks"></a>Comentários

- Sem um espaço entre um parâmetro e seu valor (ou seja, **tipo = próprios**, e não **tipo = próprio**), a operação falhará.
- O **consulta** operação exibe as seguintes informações sobre um serviço: SERVICE_NAME (nome da subchave do serviço no registro), TYPE, STATE (bem como os estados que não estão disponíveis), WIN32_EXIT_B, SERVICE_EXIT_B, CHECKPOINT e WAIT_HINT.
- O **tipo =** parâmetro pode ser usado duas vezes em alguns casos. A primeira ocorrência da **tipo =** parâmetro especifica se é necessário consultar os serviços, drivers ou ambos (**todos os**). A segunda ocorrência da **tipo =** parâmetro especifica um tipo dos **criar** operação para refinar ainda mais o escopo de uma consulta.
- Quando a exibição resultante de uma **consulta** comando excede o tamanho do buffer de enumeração, será exibida uma mensagem semelhante à seguinte:  
  ```
  Enum: more data, need 1822 bytes start resume at index 79
  ```  
  Para exibir os restantes **consulta** informações, execute novamente **consulta**, definindo **bufsize =** como o número de bytes e configuração **ri =** para o índice especificado. Por exemplo, o restante da saída seria exibido, digitando o seguinte no prompt de comando:  
  ```
  sc query bufsize= 1822 ri= 79
  ```

## <a name="BKMK_examples"></a>Exemplos

Para exibir informações de apenas a serviços do Active Directory, digite um dos comandos a seguir:
```
sc query
sc query type= service
```
Para exibir informações para os serviços do Active Directory e para especificar um tamanho de buffer de 2.000 bytes, digite:
```
sc query type= all bufsize= 2000
```
Para exibir informações para o serviço WUAUSERV, digite:
```
sc query wuauserv
```
Para exibir informações para todos os serviços (ativas e inativas), digite:
```
sc query state= all
```
Para exibir informações para todos os serviços (ativas e inativas), começando na linha de 56, digite:
```
sc query state= all ri= 56
```
Para exibir informações de serviços interativos, digite:
```
sc query type= service type= interact
```
Para exibir informações para somente os drivers, digite:
```
sc query type= driver
```
Para exibir informações de drivers no grupo de NDIS Network Driver Interface Specification (), digite:
```
sc query type= driver group= ndis
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)