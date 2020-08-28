---
title: fsutil 8dot3name
description: Artigo de referência para o comando fsutil 8dot3name, que consulta ou altera as configurações de comportamento de nome curto (8dot3 Name).
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 7b5be73c47b419733e29e949327fd57c77967561
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030604"
---
# <a name="fsutil-8dot3name"></a>fsutil 8dot3name

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Consulta ou altera as configurações de comportamento de nome curto (8dot3 Name), que inclui:

- Consultando a configuração atual para o comportamento de nome curto.

- Verificando o caminho de diretório especificado para chaves do registro que podem ser impactadas se nomes curtos forem removidos do caminho de diretório especificado.

- Alterar a configuração que controla o comportamento de nome curto. Essa configuração pode ser aplicada a um volume especificado ou à configuração de volume padrão.

- Removendo os nomes curtos de todos os arquivos em um diretório.

> [!IMPORTANT]
> Remover permanentemente nomes de arquivo 8dot3 e não modificar as chaves do registro que apontam para os nomes de arquivo 8dot3 pode levar a falhas de aplicativo inesperadas, incluindo a incapacidade de desinstalar um aplicativo. É recomendável primeiro fazer backup de seu diretório ou volume antes de tentar remover nomes de arquivo 8dot3.

## <a name="syntax"></a>Sintaxe

```
fsutil 8dot3name [query] [<volumepath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <directorypath>
fsutil 8dot3name [set] { <defaultvalue> | <volumepath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <directorypath>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| consultá `[<volumepath>]` | Consulta o sistema de arquivos para o estado do comportamento de criação de nome curto 8dot3.<p>Se um *VolumePath* não for especificado como um parâmetro, a configuração de comportamento de criação de 8dot3name padrão para todos os volumes será exibida. |
| scanner `<directorypath>` | Examina os arquivos que estão localizados no *DirectoryPath* especificado para chaves do registro que podem ser impactadas se nomes 8dot3 curtos forem removidos dos nomes dos arquivos. |
| Definição `<defaultvalue> | <volumepath>}` | Altera o comportamento do sistema de arquivos para a criação de nome 8dot3 nas seguintes instâncias:<ul><li>Quando *DefaultValue* é especificado, a chave do registro, **HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**, é definida como *DefaultValue*.<p>O *DefaultValue* pode ter os seguintes valores:<ul><li>**0**: habilita a criação de nome 8dot3 para todos os volumes no sistema.</li><li>**1**: desabilita a criação de nome 8dot3 para todos os volumes no sistema.</li><li>**2**: define a criação de nome 8dot3 em uma base por volume.</li><li>**3**: desabilita a criação de nome 8dot3 para todos os volumes, exceto o volume do sistema.</li></ul><li>Quando um *VolumePath* é especificado, os volumes especificados em disco sinalizam Propriedades de 8dot3name são definidos para habilitar a criação de nome 8dot3 para um volume especificado (**0**) ou definido para desabilitar a criação de nome 8dot3 no volume especificado (**1**).<p>Você deve definir o comportamento padrão do sistema de arquivos para a criação de nome 8dot3 para o valor **2** antes de habilitar ou desabilitar a criação de nome 8dot3 para um volume especificado.</li></ul> |
| faixas `<directorypath>` | Remove os nomes de arquivo 8dot3 de todos os arquivos que estão localizados no *DirectoryPath*especificado. O nome de arquivo 8dot3 não é removido para nenhum arquivo em que o *DirectoryPath* combinado com o nome de arquivo contenha mais de 260 caracteres.<p>Esse comando lista, mas não modifica as chaves do registro que apontam para os arquivos que tinham nomes de arquivo 8dot3 removidos permanentemente. |
| `<volumepath>` | Especifica o nome da unidade seguido por dois-pontos ou pelo GUID no formato `volume{GUID}` . |
| /f | Especifica que todos os arquivos localizados no *DirectoryPath* especificado tenham os nomes de arquivo 8dot3 removidos mesmo se houver chaves do registro que apontem para arquivos usando o nome de arquivo 8dot3. Nesse caso, a operação Remove os nomes de arquivo 8dot3, mas não modifica nenhuma chave do registro que aponte para os arquivos que estão usando os nomes de arquivo 8dot3. **AVISO:** É recomendável que você faça backup de seu diretório ou volume antes de usar o parâmetro **/f** porque ele pode levar a falhas de aplicativo inesperadas, incluindo a incapacidade de desinstalar programas. |
| /l `[<log file>]` | Especifica um arquivo de log onde as informações são gravadas.<p>Se o parâmetro **/l** não for especificado, todas as informações serão gravadas no arquivo de log padrão: `%temp%\8dot3_removal_log@(GMT YYYY-MM-DD HH-MM-SS)` . log * * |
| /s | Especifica que a operação deve ser aplicada aos subdiretórios do *DirectoryPath*especificado. |
| /t | Especifica que a remoção de nomes de arquivo 8dot3 deve ser executada no modo de teste. Todas as operações, exceto a remoção real dos nomes de arquivo 8dot3, são executadas. Você pode usar o modo de teste para descobrir quais chaves do Registro apontam para arquivos que usam os nomes de arquivo 8dot3. |
| /v | Especifica que todas as informações gravadas no arquivo de log também são exibidas na linha de comando. |

### <a name="examples"></a>Exemplos

Para consultar o comportamento de desabilitar 8dot3 Name para um volume de disco especificado com o GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, digite:

```
fsutil 8dot3name query volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Você também pode consultar o comportamento de nome 8dot3 usando o subcomando **Behavior** .

Para remover nomes de arquivo 8dot3 no diretório *D:\MyData* e em todos os subdiretórios, ao gravar as informações no arquivo de log especificado como *MyLogFile. log*, digite:

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [fsutil behavior](fsutil-behavior.md)
