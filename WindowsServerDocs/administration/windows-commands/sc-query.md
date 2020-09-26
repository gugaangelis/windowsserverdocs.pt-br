---
title: Consulta de Sc.exe
description: Artigo de referência para o comando sc.exe Query, que obtém e exibe informações sobre o serviço, o driver, o tipo de serviço especificado ou o tipo de driver.
ms.topic: reference
ms.assetid: ac365f89-4b20-4de6-a582-b204c5e7d0eb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7d5994c54914165cb79ba0f505f1a44b2d7f019a
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388209"
---
# <a name="scexe-query"></a>Consulta de Sc.exe

Obtém e exibe informações sobre o serviço, o driver, o tipo de serviço especificado ou o tipo de driver.

## <a name="syntax"></a>Sintaxe

```
sc.exe [<servername>] query [<servicename>] [type= {driver | service | all}] [type= {own | share | interact | kernel | filesys | rec | adapt}] [state= {active | inactive | all}] [bufsize= <Buffersize>] [ri= <Resumeindex>] [group= <groupname>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<servername>` | Especifica o nome do servidor remoto no qual o serviço está localizado. O nome deve usar o formato UNC (Convenção de nomenclatura universal) (por exemplo, \\ meuservidor). Para executar o SC.exe localmente, não use esse parâmetro. |
| `<servicename>` | Especifica o nome do serviço retornado pela operação **GetKeyName** . Esse parâmetro de **consulta** não é usado em conjunto com outros parâmetros de **consulta** (diferente de *ServerName*). |
| `type= {driver | service | all}` | Especifica o que enumerar. As opções incluem:<ul><li>**Driver** -especifica que somente os drivers são enumerados.</li><li>**serviço** -especifica apenas que os serviços são enumerados. Esse é o valor padrão.</li><li>**All** -especifica que os drivers e serviços são enumerados.</li></ul> |
| `type= {own | share | interact | kernel | filesys | rec | adapt}` | Especifica o tipo de serviço ou tipo de drivers a serem enumerados. As opções incluem:<ul><li>**próprio** -especifica um serviço que é executado em seu próprio processo. Ele não compartilha um arquivo executável com outros serviços. Esse é o valor padrão.</li><li>**compartilhamento** – especifica um serviço que é executado como um processo compartilhado. Ele compartilha um arquivo executável com outros serviços.</li><li>**kernel** -especifica um driver.</li><li>**arquivos** -especifica um driver de sistema de arquivos.</li><li>**REC** -especifica um driver reconhecido pelo sistema de arquivos que identifica os sistemas de arquivos usados no computador.</li><li>**interagir** -especifica um serviço que pode interagir com a área de trabalho, recebendo entrada de usuários. Os serviços interativos devem ser executados na conta LocalSystem. Esse tipo deve ser usado em conjunto com **Type = próprio** ou **Type = Shared** (por exemplo, **Type = interaja** **Type =** is). Usar **Type = interagir** por si só gerará um erro.</li></ul> |
| `state= {active | inactive | all}` | Especifica o estado de início do serviço a ser enumerado. As opções incluem:<ul><li>**ativo** -especifica todos os serviços ativos. Esse é o valor padrão.</li><li>**inativo** -especifica todos os serviços pausados ou interrompidos.</li><li>**All** -especifica todos os serviços.</li></ul> |
| `bufsize= <Buffersize>` | Especifica o tamanho (em bytes) do buffer de enumeração. O tamanho do buffer padrão é 1.024 bytes. Você deve aumentar o tamanho do buffer quando a exibição resultante de uma consulta passar mais de 1.024 bytes. |
| `ri= <Resumeindex>` | Especifica o número de índice no qual a enumeração deve ser iniciada ou retomada. O valor padrão é 0 (zero). Se mais informações forem retornadas do que o buffer padrão pode exibir, use esse parâmetro com o `bufsize=` parâmetro. |
| `group= <Groupname>` | Especifica o grupo de serviços a ser enumerado. Por padrão, todos os grupos são enumerados. Por padrão, todos os grupos são enumerados (* * Group = * *). |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Cada opção de linha de comando (parâmetro) deve incluir o sinal de igual como parte do nome da opção.

- Um espaço é necessário entre uma opção e seu valor (por exemplo, **tipo = próprio**. Se o espaço for omitido, a operação falhará.

- A operação de **consulta** exibe as seguintes informações sobre um serviço: service_name (nome da subchave do registro do serviço), tipo, estado (bem como Estados que não estão disponíveis), WIN32_EXIT_B, SERVICE_EXIT_B, ponto de verificação e WAIT_HINT.

- O parâmetro **Type =** pode ser usado duas vezes em alguns casos. A primeira aparência do **tipo =** parâmetro especifica se é preciso consultar serviços, drivers ou ambos (**todos**). A segunda aparência do **tipo =** parâmetro especifica um tipo da operação de **criação** para restringir ainda mais o escopo da consulta.

- Quando os resultados da exibição de um comando de **consulta** excederem o tamanho do buffer de enumeração, será exibida uma mensagem semelhante à seguinte:

  ```
  Enum: more data, need 1822 bytes start resume at index 79

  To display the remaining **query** information, rerun **query**, setting **bufsize=** to be the number of bytes and setting **ri=** to the specified index. For example, the remaining output would be displayed by typing the following at the command prompt:

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

Para exibir informações para o serviço *wuauserv* , digite:

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

Para exibir informações de drivers no *grupo de especificação de interface de driver de rede (NDIS)*, digite:

```
sc.exe query type= driver group= NDIS
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
