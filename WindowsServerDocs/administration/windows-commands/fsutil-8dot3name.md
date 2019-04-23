---
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
title: Fsutil 8dot3name
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4e8d67de42342f5a0828df9f57ca6d7e2870586e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873187"
---
# <a name="fsutil-8dot3name"></a>Fsutil 8dot3name

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

As consultas ou altera as configurações de comportamento de nome curto (nome de 8dot3), que incluem:

-   Consultar a configuração atual para o comportamento de nome curto

-   Verificar o caminho do diretório especificado para chaves do registro que podem ser afetados se os nomes curtos foram extraídos do caminho de diretório especificado

-   Altere a configuração que controla o comportamento de nome curto. Essa configuração pode ser aplicada a um volume especificado ou para a configuração de volume padrão.

-   Remover os nomes curtos para todos os arquivos dentro de um diretório

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fsutil 8dot3name [query] [<VolumePath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <DirectoryPath>
fsutil 8dot3name [set] { <DefaultValue> | <VolumePath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <DirectoryPath>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|consulta [<VolumePath>]|Consulta o sistema de arquivos para o estado do comportamento de criação de nome curto de 8dot3.<br /><br />Se um *VolumePath* não é especificado como um parâmetro, a configuração de comportamento de criação de 8dot3name padrão para todos os volumes é exibida.|
|verificação <DirectoryPath>|Verifica os arquivos que estão localizados em especificado *DirectoryPath* para chaves do registro que podem ser afetadas se os nomes curtos de 8dot3 foram retirados os nomes de arquivo.|
|set { <DefaultValue> &#124; <VolumePath>}|Altera o comportamento do sistema de arquivos para a criação de nome de 8dot3 nas seguintes instâncias:<br /><br /><ul><li>Quando *DefaultValue* for especificado, a chave do registro, **HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**, é definido como o *DefaultValue*.<br /><br />    O *DefaultValue* pode ter os seguintes valores:<br /><br /><ul><li>**0**: Permite a criação de nome de 8dot3 para todos os volumes no sistema.</li><li>**1**: Desabilita a criação de nome de 8dot3 para todos os volumes no sistema.</li><li>**2**: Define a criação de nome 8dot3 em uma base por volume.</li><li>**3**: Desabilita a criação de nome de 8dot3 para todos os volumes, exceto o volume do sistema.</li></ul></li><li>Quando um *VolumePath* for especificado, os volumes especificados nas propriedades do disco sinalizador 8dot3name são definidos para permitir a criação de nome de 8dot3 de um volume especificado (**0**) ou um conjunto para desabilitar a criação de nome de 8dot3 no o volume especificado (**1**).<br /><br />    Você deve definir o comportamento do sistema de arquivo padrão para a criação de nome de 8dot3 como o valor **2** antes que você pode habilitar ou desabilitar 8dot3 a criação de nome de um volume especificado.</li></ul>|
|Faixa <DirectoryPath>|Remove os nomes de arquivo para todos os arquivos que estão localizados 8dot3 especificado na *DirectoryPath*. O nome do arquivo 8dot3 não é removido de todos os arquivos em que o *DirectoryPath* combinado com o arquivo de nome contém mais de 260 caracteres.<br /><br />Esse comando lista, mas não modifica as chaves do registro que apontam para os arquivos que tiveram os nomes de arquivo 8dot3 removidos permanentemente.<br /><br />Para obter mais informações sobre os efeitos de remover permanentemente os nomes de arquivo 8dot3 de arquivos, consulte [comentários](Fsutil-8dot3name.md#BKMK_remarks).|
|<VolumePath>|Especifica o nome da unidade seguido por dois-pontos ou o GUID no formato **Volume {***GUID***}**.|
|/f|Especifica que todos os arquivos que estão localizados em especificado *DirectoryPath* têm os nomes de arquivo 8dot3 removida mesmo se houver chaves do registro que apontam para arquivos usando o nome do arquivo 8dot3. Nesse caso, a operação remove os nomes de arquivo 8dot3, mas não modifica qualquer chaves do registro que apontam para os arquivos que estão usando os nomes de arquivo 8dot3. **Aviso:** É recomendável que você faça backup de seu diretório ou volume antes de usar o **/f** parâmetro porque ele pode levar a falhas de aplicativo inesperado, inclusive a incapacidade de desinstalar programas.|
|/l [<log file>]|Especifica um arquivo de log em que as informações são gravadas.<br /><br />Se o **/l** parâmetro não for especificado, todas as informações são gravadas no arquivo de log padrão:<br /><br />**%temp%\8dot3_removal_log@(***GMT YYYY-MM-DD HH-MM-SS***).log**|
|/s|Especifica que a operação deve ser aplicada a subdiretórios de especificado *DirectoryPath*.|
|/t|Especifica que a remoção de 8dot3 nomes de arquivo deve ser executada no modo de teste. Todas as operações, exceto a remoção real dos nomes de arquivo 8dot3 são executadas. Você pode usar o modo de teste para descobrir qual registro chaves apontam para arquivos que usam os nomes de arquivo 8dot3.|
|/v|Especifica que todas as informações que são gravadas no arquivo de log também são exibidas na linha de comando.|

## <a name="BKMK_remarks"></a>Comentários

-   Remover permanentemente os nomes de arquivo 8dot3 e não modificando chaves do registro que apontam para os nomes de arquivo 8dot3 pode levar a falhas de aplicativo inesperado, inclusive a incapacidade para desinstalar um aplicativo. É recomendável que primeiro fazer backup de seu diretório ou volume antes de tentar remover nomes de arquivo 8dot3.

## <a name="BKMK_examples"></a>Exemplos
Para consultar o comportamento de nome de 8dot3 disable para um volume de disco que é especificado com o GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, digite:

```
fsutil 8dot3name query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Você também pode consultar o comportamento de nome de 8dot3 usando o **comportamento** subcomando.

Para remover nomes de arquivo 8dot3 na *D:\MyData* pasta e todas as subpastas, ao gravar as informações para o arquivo de log que é especificado como *MyLogFile*, tipo:

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fsutil 8dot3name](Fsutil-8dot3name.md)

[comportamento do fsutil](Fsutil-behavior.md)


