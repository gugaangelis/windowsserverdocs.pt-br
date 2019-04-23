---
title: bcdedit
description: Tópico de comandos do Windows para **bcdedit** - criar novos repositórios, modificar os existentes e adicionar parâmetros de menu de inicialização.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab2da47d-3aac-44a0-b7fd-bd9561d61553
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/27/2018
ms.openlocfilehash: c1ac016b299cbd72a406121c54762f4457b24286
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872437"
---
# <a name="bcdedit"></a>bcdedit



Arquivos de inicialização de configuração BCD (dados) fornecem um repositório que é usado para descrever aplicativos de inicialização e as configurações do aplicativo de inicialização. Os objetos e os elementos no repositório de substituem a Boot. ini com eficiência.

BCDEdit é uma ferramenta de linha de comando para gerenciar repositórios BCD. Ele pode ser usado para uma variedade de finalidades, incluindo a criação de novas lojas, modificando repositórios existentes, adicionando parâmetros de menu de inicialização e assim por diante. BCDEdit essencialmente a mesma finalidade como Bootcfg.exe em versões anteriores do Windows, mas com duas principais melhorias:
-   Expõe uma gama mais ampla de parâmetros de inicialização que Bootcfg.exe.
-   Melhorou o suporte a scripts.

> [!NOTE]
> São necessários privilégios administrativos para usar o BCDEdit para modificar o BCD.

BCDEdit é a principal ferramenta para editar a configuração da inicialização do Windows Vista e versões posteriores do Windows. Ele é fornecido com a distribuição do Windows Vista na pasta %WINDIR%\System32.

BCDEdit é limitado aos tipos de dados padrão e é projetado principalmente para executar alterações únicas comuns no BCD. Para operações mais complexas ou tipos de dados não padrão, considere usar a interface de programação de aplicativo (API) do BCD Windows Management Instrumentation (WMI) para criar ferramentas personalizadas mais poderosas e flexíveis.

## <a name="syntax"></a>Sintaxe

```
BCDEdit /Command [<Argument1>] [<Argument2>] ...
```

## <a name="parameters"></a>Parâmetros

### <a name="general-bcdedit-command-line-option"></a>Opção de linha de comando BCDEdit geral

|Opção|Descrição|
|------|-----------|
|/?|Exibe uma lista de comandos do BCDEdit. A execução desse comando sem um argumento exibe um resumo dos comandos disponíveis. Para exibir a ajuda detalhada para um determinado comando, execute **bcdedit /?** \<comando >, onde <command> é o nome do comando que você está pesquisando para obter mais informações sobre. Por exemplo, **bcdedit /? createstore** exibe a ajuda detalhada para o comando Createstore.|

### <a name="parameters-that-operate-on-a-store"></a>Parâmetros que operam em um Store

|Opção|Descrição|
|------|-----------|
|/createstore|Cria um novo repositório de dados de configuração de inicialização vazio. O repositório criado não é um repositório do sistema.|
|/export|Exporta o conteúdo do sistema armazena em um arquivo. Esse arquivo pode ser usado posteriormente para restaurar o estado do armazenamento do sistema. Esse comando é válido somente para o armazenamento do sistema.|
|/import|Restaura o estado do armazenamento do sistema usando um arquivo de dados de backup gerado anteriormente usando o **/exportação** opção. Esse comando exclui as entradas existentes no armazenamento do sistema antes que a importação ocorra. Esse comando é válido somente para o armazenamento do sistema.|
|/store|Essa opção pode ser usada com a maioria dos comandos BCDedit para especificar o repositório a ser usado. Se essa opção não for especificada, o BCDEdit opera no armazenamento do sistema. Executando o **bcdedit /store** comando por si só é equivalente a executar o **bcdedit /enum active** comando.|

### <a name="parameters-that-operate-on-entries-in-a-store"></a>Parâmetros que operam em entradas em um Store

|Parâmetro|Descrição|
|---------|-----------|
|/copy|Faz uma cópia de uma entrada de inicialização especificado no mesmo armazenamento de sistema.|
|/create|Cria uma nova entrada no armazenamento de dados de configuração de inicialização. Se for especificado um identificador conhecido, em seguida, a **aplicativo/**, **/ herdam**, e **/device** parâmetros não podem ser especificados. Se um identificador não for especificado ou não bem conhecido, um **aplicativo/**, **/ herdam**, ou **/device** opção deve ser especificada.|
|/delete|Exclui um elemento de uma entrada especificada.|

### <a name="parameters-that-operate-on-entry-options"></a>Parâmetros que operam em Opções de entrada

|Parâmetro|Descrição|
|---------|-----------|
|/deletevalue|Exclui um elemento especificado de uma entrada de inicialização.|
|/set|Define um valor de opção de entrada.|

### <a name="parameters-that-control-output"></a>Parâmetros que controlam a saída

|Parâmetro|Descrição|
|---------|-----------|
|/enum|Entradas de listas em um repositório. O **/enum** opção é o valor padrão para BCEdit, portanto, executando o **bcdedit** comando sem parâmetros é equivalente a executar o **bcdedit /enum active** comando.|
|/v|modo detalhado. Normalmente, todos os identificadores bem conhecidos de entrada são representados por sua forma abreviada amigável. Especificando **/v** como uma opção de linha de comando exibe todos os identificadores por completo. Executando o **bcdedit /v** comando por si só é equivalente a executar o **bcdedit /enum active /v** comando.|

### <a name="parameters-that-control-the-boot-manager"></a>Parâmetros que controlam o Gerenciador de inicialização

|Parâmetro|Descrição|
|---------|-----------|
|/bootsequence|Especifica uma ordem de exibição única a ser usado para a próxima inicialização. Esse comando é semelhante para o **/displayorder** opção, exceto que ele é usado apenas na próxima vez que o computador é iniciado. Depois disso, o computador será revertido para a ordem de exibição original.|
|/default|Especifica a entrada padrão que o Gerenciador de inicialização seleciona quando o tempo limite expirar.|
|/displayorder|Especifica a ordem de exibição que usa o Gerenciador de inicialização ao exibir os parâmetros de inicialização para um usuário.|
|/timeout|Especifica o tempo de espera, em segundos, antes que o Gerenciador de inicialização seleciona a entrada padrão.|
|/toolsdisplayorder|Especifica a ordem de exibição do Gerenciador de inicialização usar ao exibir o **ferramentas** menu.|

### <a name="parameters-that-control-emergency-management-services"></a>Parâmetros que controlam os serviços de gerenciamento de emergência

|Parâmetro|Descrição|
|---------|-----------|
|/bootems|Habilita ou desabilita os serviços de gerenciamento de emergência (EMS) para a entrada especificada.|
|/ems|Habilita ou desabilita o EMS para a entrada de inicialização do sistema operacional especificado.|
|/emssettings|Define as configurações globais do EMS para o computador. **/emssettings** não habilitar ou desabilitar o EMS para qualquer entrada de inicialização específica.|

### <a name="parameters-that-control-debugging"></a>Parâmetros que controlam a depuração

|Parâmetro|Descrição|
|---------|-----------|
|/bootdebug|Habilita ou desabilita o depurador de inicialização para uma entrada de inicialização especificada. Embora esse comando funciona para qualquer entrada de inicialização, ele é eficaz somente para aplicativos de inicialização.|
|/dbgsettings|Especifica ou exibe as configurações globais do depurador para o sistema. Esse comando não habilitar ou desabilitar o depurador de kernel; Use o **/Debug** opção para essa finalidade. Para definir um configuração do depurador global individual, use o **bcdedit /set** \<dbgsettings > <type> <value> .|
|/debug|Habilita ou desabilita o depurador de kernel para uma entrada de inicialização especificada.|

## <a name="examples"></a>Exemplos

Para obter exemplos de BCDEdit, consulte o [referência de opções de BCDEdit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcd-boot-options-reference).