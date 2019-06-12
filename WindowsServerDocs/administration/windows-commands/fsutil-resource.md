---
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
title: Recursos do fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 38b0947171b7fd8afc44a95b2244a3fd2a0c9e73
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439040"
---
# <a name="fsutil-resource"></a>Recursos do fsutil
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Cria um Gerenciador de recursos transacionais secundário, inicia ou interrompe um Gerenciador de recursos transacional, ou exibe informações sobre um Gerenciador de recursos transacionais e modifica o comportamento a seguir:

-   Se um padrão de Gerenciador de recursos transacionais limpará seus metadados transacionais na montagem próximo

-   O Gerenciador de recursos transacionais especificado para dar preferência a consistência em relação à disponibilidade

-   O Gerenciador de recursos de transação especificado prefira disponibilidade a consistência

-   As características de uma execução Gerenciador de recursos transacional

Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples) .

## <a name="syntax"></a>Sintaxe

```
fsutil resource [create] <RmRootPathname>
fsutil resource [info] <RmRootPathname>
fsutil resource [setautoreset] {true|false} <DefaultRmRootPathname>
fsutil resource [setavailable] <RmRootPathname>
fsutil resource [setconsistent] <RmRootPathname>
fsutil resource [setlog] [growth {<Containers> containers|<Percent> percent} <RmRootPathname>] [maxextents <Containers> <RmRootPathname>] [minextents <Containers> <RmRootPathname>] [mode {full|undo} <RmRootPathname>] [rename <RmRootPathname>] [shrink <percent> <RmRootPathname>] [size <Containers> <RmRootPathname>]
fsutil resource [start] <RmRootPathname> [<RmLogPathname> <TmLogPathname>
fsutil resource [stop] <RmRootPathname>
```

### <a name="parameters"></a>Parâmetros

|        Parâmetro        |                                                                                                                                                                                                                                        Descrição                                                                                                                                                                                                                                         |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         criar          |                                                                                                                                                                                                                    Cria um Gerenciador de recursos transacionais secundário.                                                                                                                                                                                                                     |
|    <RmRootPathname>     |                                                                                                                                                                                                        Especifica o caminho completo para um diretório de raiz do Gerenciador de recursos transacional.                                                                                                                                                                                                         |
|          informações           |                                                                                                                                                                                                            Exibe informações de especificado transacional do Resource Manager.                                                                                                                                                                                                            |
|      setautoreset       | Especifica se um padrão de Gerenciador de recursos transacionais limpará os metadados transacionais na montagem a próxima.<br /><br />-Defina as **setautoreset** parâmetro **verdadeiro** para especificar que o Gerenciador de recursos de transação limpará os metadados transacionais na montagem de Avançar, por padrão.<br />-Defina as **setautoreset** parâmetro **falso** para especificar que o Gerenciador de recursos de transação não limpará os metadados transacionais na próxima montagem, por padrão. |
| <DefaultRmRootPathname> |                                                                                                                                                                                                                       Especifica o nome da unidade seguido por dois-pontos.                                                                                                                                                                                                                        |
|      setavailable       |                                                                                                                                                                                                 Especifica que um Gerenciador de recursos transacional será prefira disponibilidade a consistência.                                                                                                                                                                                                 |
|      setconsistent      |                                                                                                                                                                                                 Especifica que um Gerenciador de recursos transacionais preferirão consistência em relação à disponibilidade.                                                                                                                                                                                                 |
|         setlog          |                                                                                                                                                                                                  Altera as características de um Gerenciador de recursos transacional que já está em execução.                                                                                                                                                                                                  |
|         crescimento          |                                                                                                  Especifica a quantidade pela qual o log do Gerenciador de recursos transacional pode atingir.<br /><br />O parâmetro de aumento pode ser especificado da seguinte maneira:<br /><br />– O número de contêineres usando o formato: *Contêineres***recipientes**<br />–   usando o formato de porcentagem: *Percent***percent**                                                                                                   |
|      <containers>       |                                                                                                                                                                                                      Especifica os objetos de dados que são usados pelo Gerenciador de recursos transacionais.                                                                                                                                                                                                       |
|        maxextent        |                                                                                                                                                                                                Especifica o número máximo de contêineres para o Gerenciador transacional de recursos especificado.                                                                                                                                                                                                |
|        minextent        |                                                                                                                                                                                                Especifica o número mínimo de contêineres para o Gerenciador transacional de recursos especificado.                                                                                                                                                                                                |
|  modo de {completo&#124;desfazer}  |                                                                                                                                                                                        Especifica se todas as transações são registradas em log ( **completo**) ou apenas revertida eventos são registrados em log (**desfazer**).                                                                                                                                                                                         |
|         rename          |                                                                                                                                                                                                                  Altera o GUID para o Gerenciador de recursos transacionais.                                                                                                                                                                                                                  |
|         shrink          |                                                                                                                                                                                              Especifica a porcentagem pela qual o log do Gerenciador de recursos transacional pode diminuir o automaticamente.                                                                                                                                                                                              |
|          size           |                                                                                                                                                                                              Especifica o tamanho do Gerenciador de recursos transacional como um número especificado de *contêineres*.                                                                                                                                                                                               |
|          start          |                                                                                                                                                                                                                    Inicia o Gerenciador de recursos transacional especificada.                                                                                                                                                                                                                    |
|          stop           |                                                                                                                                                                                                                    Interrompe o Gerenciador de recursos transacional especificada.                                                                                                                                                                                                                     |

### <a name="BKMK_examples"></a>Exemplos
Para definir o log para o Gerenciador de recursos transacional que é especificado pelo c:\test, para ter um crescimento automático de cinco contêineres, digite:

```
fsutil resource setlog growth 5 containers c:test
```

Para definir o log para o Gerenciador de recursos transacional que é especificado pelo c:\test, para ter um crescimento automático de dois por cento, digite:

```
fsutil resource setlog growth 2 percent c:test
```

Para especificar que o padrão do Gerenciador de recursos transacionais limpará os metadados transacionais na montagem Avançar na unidade C, digite:

```
fsutil resource setautoreset true c:\  
```

### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS Transacional](https://go.microsoft.com/fwlink/?LinkID=165402)


