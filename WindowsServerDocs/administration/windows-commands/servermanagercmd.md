---
title: servermanagercmd
description: Artigo de referência para o comando ServerManagerCmd, que instala e remove funções, serviços de função e recursos.
ms.topic: reference
ms.assetid: 507c4b87-8e13-4872-8b34-0c7508eecbc1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 4a00294b70728a41cc68ae15d35a8c19fd768cf7
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389154"
---
# <a name="servermanagercmd"></a>servermanagercmd

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Instala e remove funções, serviços de função e recursos. Também exibe a lista de todas as funções, serviços de função e recursos disponíveis e mostra quais estão instalados neste computador.

> [!IMPORTANT]
> Esse comando, o ServerManagerCmd, foi preterido e não há garantia de que haja suporte em versões futuras do Windows. É recomendável usar os cmdlets do Windows PowerShell que estão disponíveis para Gerenciador do Servidor. Para mais informações, consulte [Instalar ou desinstalar funções, Serviços de função ou Recursos](/administration/server-manager/install-or-uninstall-roles-role-services-or-features).

## <a name="syntax"></a>Sintaxe

```
servermanagercmd -query [[[<drive>:]<path>]<query.xml>] [-logpath [[<drive>:]<path>]<log.txt>]
servermanagercmd -inputpath  [[[<drive>:]<path>]<answer.xml>] [-resultpath <result.xml> [-restart] | -whatif] [-logpath [[<drive>:]<path>]<log.txt>]
servermanagercmd -install <id> [-allSubFeatures] [-resultpath [[<drive>:]<path>]<result.xml> [-restart] | -whatif] [-logpath [[<Drive>:]<path>]<log.txt>]
servermanagercmd -remove <id> [-resultpath <result.xml> [-restart] | -whatif] [-logpath  [[<drive>:]<path>]<log.txt>]
servermanagercmd [-help | -?]
servermanagercmd -version
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -consulta `[[[<drive>:]<path>]<query.xml>]` | Exibe uma lista de todas as funções, serviços de função e recursos instalados e disponíveis para instalação no servidor. Você também pode usar a forma abreviada desse parâmetro, **-q**. Se você quiser que os resultados da consulta sejam salvos em um arquivo XML, especifique um arquivo XML a ser substituído `<query.xml>` . |
| -inputPath  `[[[<drive>:]<path>]<answer.xml>]` | Instala ou remove as funções, os serviços de função e os recursos especificados em um arquivo de resposta XML representado por `<answer.xml>` . Você também pode usar a forma abreviada desse parâmetro, **-p.** |
| -instalar `<id>` | Instala a função, o serviço de função ou o recurso especificado por `<id>` . Os identificadores não diferenciam maiúsculas de minúsculas. Várias funções, serviços de função e recursos devem ser separados por espaços. Os seguintes parâmetros opcionais são usados com o parâmetro **-install** :<ul><li>**-configuração** `<SettingName>=<SettingValue>` -Especifica as configurações necessárias para a instalação.</li><li>**-allSubFeatures** -especifica a instalação de todos os serviços e recursos subordinados, juntamente com a função pai, o serviço de função ou o recurso nomeado no `<id>` valor.<p>**OBSERVAÇÃO**<br>Alguns contêineres de função não têm um identificador de linha de comando para permitir a instalação de todos os serviços de função. Esse é o caso quando os serviços de função não podem ser instalados na mesma instância do comando Gerenciador do Servidor. Por exemplo, o serviço de função Serviço de Federação dos serviços de Federação do Active Directory e o serviço de função Proxy do Serviço de Federação não podem ser instalados usando a mesma instância de comando Gerenciador do Servidor.</li><li>**-resultPath** `<result.xml>` – Salva os resultados da instalação em um arquivo XML representado pelo `<result.xml>` . Você também pode usar a forma abreviada desse parâmetro, **-r**.<p>**OBSERVAÇÃO**<br>Não é possível executar o ServerManagerCmd com o parâmetro **-resultPath** e o parâmetro **-WhatIf** especificado.</li><li>**-Restart** – reinicia o computador automaticamente quando a instalação é concluída (se a reinicialização for necessária para as funções ou recursos instalados).</li><li>**-WhatIf** -exibe todas as operações especificadas para o parâmetro **-install** . Você também pode usar a forma abreviada do parâmetro **-WhatIf** , **-w**. Não é possível executar o **ServerManagerCmd** com o parâmetro **-resultPath** e o parâmetro **-WhatIf** especificado.</li><li>**-logPath** `<[[<drive>:]<path>]<log.txt>>` -Especifica um nome e um local para o arquivo de log, diferente do padrão, `%windir%\temp\servermanager.log` .</li></ul> |
| -remover `<id>` | Remove a função, o serviço de função ou o recurso especificado por `<id>` . Os identificadores não diferenciam maiúsculas de minúsculas. Várias funções, serviços de função e recursos devem ser separados por espaços. Os seguintes parâmetros opcionais são usados com o parâmetro **-Remove** :<ul><li>**-resultPath** `<[[<drive>:]<path>]result.xml>` – Salva os resultados da remoção em um arquivo XML representado pelo `<result.xml>` . Você também pode usar a forma abreviada desse parâmetro, **-r**.<p>**OBSERVAÇÃO**<br>Não é possível executar o ServerManagerCmd com os parâmetros **-resultPath** e **-WhatIf** especificados.</li><li>**-Restart** – reinicia o computador automaticamente quando a remoção é concluída (se a reinicialização for necessária para funções ou recursos restantes).</li><li>**-WhatIf** -exibe todas as operações especificadas para o parâmetro-remove. Você também pode usar a forma abreviada do parâmetro-WhatIf, **-w**. Não é possível executar o ServerManagerCmd com os parâmetros **-resultPath** e **-WhatIf** especificados.</li><li>**-logPath** `<[[<Drive>:]<path>]<log.txt>>` -Especifica um nome e um local para o arquivo de log, diferente do padrão, `%windir%\temp\servermanager.log` .</li></ul> |
| -version | Exibe o número de versão do Gerenciador do Servidor. Você também pode usar a forma abreviada, **-v**. |
| -help | Exibe a ajuda na janela de prompt de comando. Você também pode usar a forma abreviada, **-?**. |

## <a name="examples"></a>Exemplos

Para exibir uma lista de todas as funções, serviços de função e recursos disponíveis e quais funções, serviços de função e recursos estão instalados no computador, digite:

```
servermanagercmd -query
```

Para instalar a função do servidor Web (IIS) e salvar os resultados da instalação em um arquivo XML representado por *installResult.xml*, digite:

```
servermanagercmd -install Web-Server -resultpath installResult.xml
```

Para exibir informações detalhadas sobre as funções, os serviços de função e os recursos que seriam instalados ou removidos, com base nas instruções especificadas em um arquivo de resposta XML representado por *install.xml*, digite:

```
servermanagercmd -inputpath install.xml -whatif
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Visão geral de Gerenciador do Servidor](/administration/server-manager/server-manager)
