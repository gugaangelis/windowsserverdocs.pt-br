---
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
title: Recurso fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 2013173978838764b2ff83a8b7cbc35adc485264
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844089"
---
# <a name="fsutil-resource"></a>Recurso fsutil
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Cria um Gerenciador de recursos transacionais secundário, inicia ou interrompe um Gerenciador de recursos transacionais ou exibe informações sobre um Gerenciador de recursos transacionais e modifica o seguinte comportamento:

-   Se um Gerenciador de recursos transacionais padrão limpará seus metadados transacionais na próxima montagem

-   O Gerenciador de recursos transacionais especificado para preferir a consistência sobre a disponibilidade

-   O Gerenciador de recursos de transação especificado para preferir a disponibilidade em relação à consistência

-   As características de um Gerenciador de recursos transacionais em execução

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

#### <a name="parameters"></a>Parâmetros

|        Parâmetro        |                                                                                                                                                                                                                                        Descrição                                                                                                                                                                                                                                         |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         criar          |                                                                                                                                                                                                                    Cria um Gerenciador de recursos transacionais secundário.                                                                                                                                                                                                                     |
|    <RmRootPathname>     |                                                                                                                                                                                                        Especifica o caminho completo para um diretório raiz do Gerenciador de recursos transacional.                                                                                                                                                                                                         |
|          informações           |                                                                                                                                                                                                            Exibe as informações do Gerenciador de recursos transacionais especificado.                                                                                                                                                                                                            |
|      setautoreset       | Especifica se um Gerenciador de recursos transacionais padrão limpará os metadados transacionais na próxima montagem.<p>-Defina o parâmetro **setautoreset** como **true** para especificar que o Gerenciador de recursos de transação limpará os metadados transacionais na próxima montagem, por padrão.<br />-Defina o parâmetro **setautoreset** como **false** para especificar que o Gerenciador de recursos de transação não limpará os metadados transacionais na próxima montagem, por padrão. |
| <DefaultRmRootPathname> |                                                                                                                                                                                                                       Especifica o nome da unidade seguido por dois-pontos.                                                                                                                                                                                                                        |
|      setdisponível       |                                                                                                                                                                                                 Especifica que um Gerenciador de recursos transacionais irá preferir a disponibilidade em relação à consistência.                                                                                                                                                                                                 |
|      setconsistente      |                                                                                                                                                                                                 Especifica que um Gerenciador de recursos transacionais prefere a consistência em relação à disponibilidade.                                                                                                                                                                                                 |
|         SETLOG          |                                                                                                                                                                                                  Altera as características de um Gerenciador de recursos transacionais que já está em execução.                                                                                                                                                                                                  |
|         growth          |                                                                                                  Especifica a quantidade pela qual o log do Gerenciador de recursos transacionais pode crescer.<p>O parâmetro de crescimento pode ser especificado da seguinte maneira:<p>-Número**de contêineres** usando o formato _: contêiner contêineres_<br />-Porcentagem usando o formato: _percentual_**de porcentagem**                                                                                                   |
|      <containers>       |                                                                                                                                                                                                      Especifica os objetos de dados que são usados pelo Gerenciador de recursos transacionais.                                                                                                                                                                                                       |
|        maxextent        |                                                                                                                                                                                                Especifica o número máximo de contêineres para o Gerenciador de recursos transacionais especificado.                                                                                                                                                                                                |
|        minextent        |                                                                                                                                                                                                Especifica o número mínimo de contêineres para o Gerenciador de recursos transacionais especificado.                                                                                                                                                                                                |
|  modo {desfazer&#124;completo}  |                                                                                                                                                                                        Especifica se todas as transações são registradas em log ( **completo**) ou se apenas os eventos revertidos são registrados (**desfazer**).                                                                                                                                                                                         |
|         rename          |                                                                                                                                                                                                                  Altera o GUID do Gerenciador de recursos transacionais.                                                                                                                                                                                                                  |
|         shrink          |                                                                                                                                                                                              Especifica o percentual pelo qual o log do Gerenciador de recursos transacionais pode diminuir automaticamente.                                                                                                                                                                                              |
|          size           |                                                                                                                                                                                              Especifica o tamanho do Gerenciador de recursos transacionais como um número especificado de *contêineres*.                                                                                                                                                                                               |
|          start          |                                                                                                                                                                                                                    Inicia o Gerenciador de recursos transacionais especificado.                                                                                                                                                                                                                    |
|          stop           |                                                                                                                                                                                                                    Interrompe o Gerenciador de recursos transacionais especificado.                                                                                                                                                                                                                     |

### <a name="examples"></a><a name="BKMK_examples"></a>Disso
Para definir o log para o Gerenciador de recursos transacionais especificado por c:\test, para ter um aumento automático de cinco contêineres, digite:

```
fsutil resource setlog growth 5 containers c:test
```

Para definir o log para o Gerenciador de recursos transacionais especificado por c:\test, para ter um aumento automático de dois percentuais, digite:

```
fsutil resource setlog growth 2 percent c:test
```

Para especificar que o Gerenciador de recursos transacionais padrão limpará os metadados transacionais na próxima montagem na unidade C, digite:

```
fsutil resource setautoreset true c:\  
```

### <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

[NTFS Transacional](https://go.microsoft.com/fwlink/?LinkID=165402)


