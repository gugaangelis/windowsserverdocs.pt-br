---
title: Consulta de Sc.exe
description: Saiba como obter informações sobre serviços, Drivers, tipos de serviços ou tipos de drivers usando o utilitário de sc.exe
ms.topic: reference
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b3d7967597724dfae4ab5a12ecee9698a43236f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037524"
---
# <a name="scexe-query"></a>Consulta de Sc.exe

Obtém e exibe informações sobre o serviço, o driver, o tipo de serviço especificado ou o tipo de driver.

## <a name="syntax"></a>Sintaxe

```
sc.exe [<ServerName>] query [<ServiceName>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <BufferSize>] [ri= <ResumeIndex>] [group= <GroupName>]
```

### <a name="parameters"></a>Parâmetros

|       Parâmetro        |                                                                                                                          Descrição                                                                                                                          |
|------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     \<ServerName>      |                       Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato UNC (Convenção de nomenclatura universal) (por exemplo, \\ \\ meuservidor). Para executar SC.exe localmente, omita esse parâmetro.                        |
|     \<ServiceName>     |                                      Especifica o nome do serviço retornado pela operação **GetKeyName** . Esse parâmetro de **consulta** não é usado em conjunto com outros parâmetros de **consulta** (diferente de *ServerName*).                                      |
|     Type = {driver      |                                                                                                                            serviço                                                                                                                            |
|       tipo = {próprio       |                                                                                                                             compartilhar                                                                                                                             |
|     estado = {ativo     |                                                                                                                           inativos                                                                                                                            |
| bufsize = \<BufferSize> |                     Especifica o tamanho (em bytes) do buffer de enumeração. O tamanho do buffer padrão é 1.024 bytes. Você deve aumentar o tamanho do buffer de enumeração quando a exibição resultante de uma consulta exceder 1.024 bytes.                      |
|   ri = \<ResumeIndex>   | Especifica o número de índice no qual a enumeração deve ser iniciada ou retomada. O valor padrão é **0** (zero). Use esse parâmetro em conjunto com o parâmetro **bufsize =** quando mais informações forem retornadas por uma consulta do que o buffer padrão pode exibir. |
|  Grupo = \<GroupName>   |                                                                             Especifica o grupo de serviços a ser enumerado. Por padrão, todos os grupos são enumerados (* * Group = * *).                                                                              |
|           /?           |                                                                                                             Exibe a ajuda no prompt de comando.                                                                                                              |

## <a name="remarks"></a>Comentários

- Sem um espaço entre um parâmetro e seu valor (ou seja, **tipo = próprio**, não **Type = próprio**), a operação falhará.
- A operação de **consulta** exibe as seguintes informações sobre um serviço: service_name (nome da subchave do registro do serviço), tipo, estado (bem como Estados que não estão disponíveis), WIN32_EXIT_B, SERVICE_EXIT_B, ponto de verificação e WAIT_HINT.
- O parâmetro **Type =** pode ser usado duas vezes em alguns casos. A primeira aparência do **tipo =** parâmetro especifica se é preciso consultar serviços, drivers ou ambos (**todos**). A segunda aparência do **tipo =** parâmetro especifica um tipo da operação de **criação** para restringir ainda mais o escopo da consulta.
- Quando a exibição resultante de um comando de **consulta** exceder o tamanho do buffer de enumeração, será exibida uma mensagem semelhante à seguinte:
  ```
  Enum: more data, need 1822 bytes start resume at index 79
  ```
  Para exibir as informações de **consulta** restantes, execute a **consulta**novamente, definindo **bufsize =** como o número de bytes e definindo **ri =** como o índice especificado. Por exemplo, a saída restante seria exibida digitando o seguinte no prompt de comando:
  ```
  sc.exe query bufsize= 1822 ri= 79
  ```

## <a name="examples"></a>Exemplos

Para exibir informações somente para os serviços ativos, digite um dos seguintes comandos:
```
sc.exe query
sc.exe query type= service
```
Para exibir informações de serviços ativos e especificar um tamanho de buffer de 2.000 bytes, digite:
```
sc.exe query type= all bufsize= 2000
```
Para exibir informações para o serviço WUAUSERV, digite:
```
sc.exe query wuauserv
```
Para exibir informações de todos os serviços (ativas e inativas), digite:
```
sc.exe query state= all
```
Para exibir informações de todos os serviços (ativas e inativas), começando na linha 56, digite:
```
sc.exe query state= all ri= 56
```
Para exibir informações para serviços interativos, digite:
```
sc.exe query type= service type= interact
```
Para exibir informações somente para drivers, digite:
```
sc.exe query type= driver
```
Para exibir informações de drivers no grupo de especificação de interface de driver de rede (NDIS), digite:
```
sc.exe query type= driver group= ndis
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
