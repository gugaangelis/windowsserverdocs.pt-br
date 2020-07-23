---
title: fsutil resource
description: Artigo de referência para o comando fsutil Resource, que gerencia um Gerenciador de recursos transacionais e seu comportamento.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c0be84f00df2e0010f6c2a318f605532a3bc4d23
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958178"
---
# <a name="fsutil-resource"></a>fsutil resource

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Cria um Gerenciador de recursos transacionais secundário, inicia ou interrompe um Gerenciador de recursos transacionais, ou exibe informações sobre um Gerenciador de recursos transacionais e modifica o seguinte comportamento:

- Se um Gerenciador de recursos transacionais padrão limpa seus metadados transacionais na próxima montagem.

- O Gerenciador de recursos transacionais especificado para preferir a consistência sobre a disponibilidade.

- O Gerenciador de recursos de transação especificado para preferir a disponibilidade em relação à consistência.

- As características de um Gerenciador de recursos transacionais em execução.

## <a name="syntax"></a>Sintaxe

```
fsutil resource [create] <rmrootpathname>
fsutil resource [info] <rmrootpathname>
fsutil resource [setautoreset] {true|false} <Defaultrmrootpathname>
fsutil resource [setavailable] <rmrootpathname>
fsutil resource [setconsistent] <rmrootpathname>
fsutil resource [setlog] [growth {<containers> containers|<percent> percent} <rmrootpathname>] [maxextents <containers> <rmrootpathname>] [minextents <containers> <rmrootpathname>] [mode {full|undo} <rmrootpathname>] [rename <rmrootpathname>] [shrink <percent> <rmrootpathname>] [size <containers> <rmrootpathname>]
fsutil resource [start] <rmrootpathname> [<rmlogpathname> <tmlogpathname>
fsutil resource [stop] <rmrootpathname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| create | Cria um Gerenciador de recursos transacionais secundário. |
| `<rmrootpathname>` | Especifica o caminho completo para um diretório raiz do Gerenciador de recursos transacional. |
| informações | Exibe as informações do Gerenciador de recursos transacionais especificado. |
| setautoreset | Especifica se um Gerenciador de recursos transacionais padrão limpará os metadados transacionais na próxima montagem.<ul><li>**true** -especifica que o Gerenciador de recursos de transação limpará os metadados transacionais na próxima montagem, por padrão.</li><li>**false** -especifica que o Gerenciador de recursos de transação não limpará os metadados transacionais na próxima montagem, por padrão. |
| `<defaultrmrootpathname>` | Especifica o nome da unidade seguido por dois-pontos. |
| setdisponível | Especifica que um Gerenciador de recursos transacionais irá preferir a disponibilidade em relação à consistência. |
| setconsistente | Especifica que um Gerenciador de recursos transacionais prefere a consistência em relação à disponibilidade. |
| SETLOG | Altera as características de um Gerenciador de recursos transacionais que já está em execução. |
| growth | Especifica a quantidade pela qual o log do Gerenciador de recursos transacionais pode crescer.<p>O parâmetro de crescimento pode ser especificado da seguinte maneira:<ul><li>Número de contêineres, usando o formato:`<containers> containers`</li><li>Porcentagem, usando o formato:`<percent> percent`</li></ul> |
| `<containers>` | Especifica os objetos de dados que são usados pelo Gerenciador de recursos transacionais. |
| maxextent | Especifica o número máximo de contêineres para o Gerenciador de recursos transacionais especificado. |
| minextent | Especifica o número mínimo de contêineres para o Gerenciador de recursos transacionais especificado. |
| moda`{full|undo}` | Especifica se todas as transações são registradas em log ( **completo**) ou se apenas os eventos revertidos são registrados (**desfazer**). |
| renomear | Altera o GUID do Gerenciador de recursos transacionais. |
| shrink | Especifica o percentual pelo qual o log do Gerenciador de recursos transacionais pode diminuir automaticamente. |
| tamanho | Especifica o tamanho do Gerenciador de recursos transacionais como um número especificado de *contêineres*. |
| start | Inicia o Gerenciador de recursos transacionais especificado. |
| parar | Interrompe o Gerenciador de recursos transacionais especificado. |

### <a name="examples"></a>Exemplos

Para definir o log para o Gerenciador de recursos transacionais especificado por *c:\test*, para ter um aumento automático de cinco contêineres, digite:

```
fsutil resource setlog growth 5 containers c:test
```

Para definir o log para o Gerenciador de recursos transacionais especificado por *c:\test*, para ter um aumento automático de dois percentuais, digite:

```
fsutil resource setlog growth 2 percent c:test
```

Para especificar que o Gerenciador de recursos transacionais padrão limpará os metadados transacionais na próxima montagem na unidade C, digite:

```
fsutil resource setautoreset true c:\
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [NTFS transacional](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v=ws.10))
