---
title: defrag
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aaf1d1ac-996a-4282-9b4d-1e8245ff162c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6997e878b2bb7b77a5920ad7398ef7c2301cc8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813187"
---
# <a name="defrag"></a>defrag

>Aplica-se a: Windows 10, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Localiza e consolida arquivos fragmentados em volumes locais para melhorar o desempenho do sistema.
Associação local **administradores** grupo, ou equivalente, é o mínimo necessário para executar esse comando.

## <a name="syntax"></a>Sintaxe
```
defrag <volumes> | /C | /E <volumes>    [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /A [/H] [/M [n]| [/U] [/V]]
defrag <volumes> | /C | /E <volumes> /X [/H] [/M [n]| [/U] [/V]]
defrag <volume> [/<Parameter>]*
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|`<volume>`|Especifica o unidade montagem ou letra ponto de caminho do volume a ser desfragmentado ou analisados.|
|A|Execute análise em volumes especificados.|
|C|Execute a operação em todos os volumes.|
|D|Execute a desfragmentação tradicional (esse é o padrão). Em um volume em camadas, desfragmentação tradicional é executada somente no nível de capacidade.|
|E|Execute a operação em todos os volumes, exceto aquelas especificadas.|
|G|Otimize as camadas de armazenamento nos volumes especificados.|
|H|Execute a operação em prioridade normal (o padrão é baixo).|
|I n|Otimização de camada seria executado por no máximo n segundos em cada volume.|
|K|Execute consolidação de sessão em volumes especificados.|
|L|Execute retrim em volumes especificados.|
|M [n]|Execute a operação em cada volume em paralelo em segundo plano. No máximo n threads otimizam as camadas de armazenamento em paralelo.|
|O|Execute a otimização adequada para cada tipo de mídia.|
|T|Rastrear uma operação já está em andamento no volume especificado.|
|U|o progresso da operação na tela de impressão.|
|V|Imprima a saída detalhada, que contém as estatísticas de fragmentação.|
|X|Execute consolidação de espaço livre em volumes especificados.|
|?|Exibe essas informações de Ajuda.|

## <a name="remarks"></a>Comentários
-   Você não pode desfragmentar tipos específicos de unidades ou volumes de sistema de arquivos:
    -   Você não pode desfragmentar volumes que bloqueou o sistema de arquivos.
    -   Você não pode desfragmentar volumes que o sistema de arquivos marcado como sujo, que indica um possível corrompimento. Você deve executar **chkdsk** em um volume sujo antes de desfragmentá-lo. Você pode determinar se um volume está sujo usando o **fsutil** sujos comando de consulta. Para obter mais informações sobre **chkdsk** e **fsutil** sujos, consulte [referências adicionais](defrag.md#BKMK_additionalRef).
    -   Você não pode desfragmentar unidades de rede.
    -   Você não pode desfragmentar o CD-ROMs.
    -   Você não pode desfragmentar volumes de sistema de arquivos que não são **NTFS**, **ReFS**, **Fat** ou **Fat32**.
-   Com o Windows Server 2008 R2, Windows Server 2008 e Windows Vista, você pode agendar para desfragmentar um volume. No entanto, você não pode agendar para desfragmentar um volume em um disco rígido Virtual (VHD) que reside em um SSD ou um estado unidade sólido (SSD).
-   Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local ou deve ter recebido a autoridade apropriada. Se o computador fizer parte de um domínio, é possível que os membros do grupo Administradores de domínio possam executar esse procedimento. Como uma segurança prática recomendada, considere usar **executar como** para executar este procedimento.
-   Um volume deve ter pelo menos 15% espaço livre para **defrag** desfragmente total e adequadamente. **executa a desfragmentação** utiliza esse espaço como uma área de classificação para fragmentos de arquivos. Se um volume tiver menos de 15% de espaço livre, **defrag** desfragmentará apenas parcialmente. Para aumentar o espaço livre em um volume, exclua arquivos desnecessários ou mova-os para outro disco.
-   Embora **defrag** é analisar e desfragmentar um volume, ele exibe um cursor piscando. Quando **defrag** é termina de analisar e desfragmentar o volume, ele exibe o relatório de análise, o relatório de desfragmentação ou ambos os relatórios e, em seguida, sai para o prompt de comando.
-   Por padrão, **defrag** exibe um resumo dos relatórios de análise e de desfragmentação, se você não especificar a **/a** ou **/v** parâmetros.
-   Você pode enviar relatórios para um arquivo de texto digitando **>** *FileName.txt*, onde *filename. txt* é um nome de arquivo que você especificar. Por exemplo: `defrag volume /v > FileName.txt`
-   Para interromper o processo de desfragmentação, na linha de comando, pressione **CTRL + C**.
-   Executando o **defrag** Desfragmentador de disco e de comando são mutuamente exclusivos. Se você estiver usando o Desfragmentador de disco para desfragmentar um volume e você executar o **defrag** comando em uma linha de comando, o **defrag** comando falhará. Por outro lado, se você executar o **defrag** comando e abrir o Desfragmentador de disco, as opções de desfragmentação em Desfragmentador de disco não estão disponíveis.

## <a name="BKMK_examples"></a>Exemplos
Para desfragmentar o volume na unidade C, fornecendo o andamento e a saída detalhada, digite:
```
defrag C: /U /V
```
Para desfragmentar volumes em unidades C e D em paralelo em segundo plano, digite:
```
defrag C: D: /M
```
Para executar uma análise de fragmentação de um volume montado na unidade C e fornecer o progresso, digite:
```
defrag C: mountpoint /A /U
```
Para desfragmentar todos os volumes com prioridade normal e fornecer saída detalhada, digite:
```
defrag /C /H /V
```

## <a name="BKMK_scheduledTask"></a>Tarefa agendada
Executa a desfragmentação do agendada será executada como uma tarefa de manutenção e geralmente é agendada para ser executada toda semana. Administrador pode alterar a frequência usando **otimizar unidades** aplicativo.
- Quando executada a partir da tarefa agendada, **defrag** tem política abaixo para SSDs:
   - **Executa a desfragmentação tradicional** (ou seja, movendo arquivos para torná-las razoavelmente contígua) e **retrim** é executado apenas uma vez por mês.
   - Se os dois **tradicional de desfragmentação** e **retrim** são ignorados, o **análise** não está em execução.
      - Se o usuário executou **tradicional de desfragmentação** manualmente em um SSD, diga a execução de tarefas de três semanas após a última agendados, em seguida, executará a próxima tarefa agendada que execute **analysis** e **retrim** mas Ignorar **tradicional de desfragmentação** no SSD.
   - Se **analysis** será ignorada, a **executado pela última vez** tempo no **otimizar unidades** não será atualizado.  Portanto, para o SSDs a **executado pela última vez** tempo no **otimizar unidades** pode ser um mês de idade.
- Esta tarefa de manutenção não pode desfragmentar todos os volumes, às vezes porque essa tarefa faz o seguinte:
   - Não ativar o computador para executar a desfragmentação
   - É iniciado somente se o computador está em corrente alternada e parará se o computador alterna para a energia da bateria
   - Interrompe a se o computador deixar de ficar ocioso

## <a name="BKMK_additionalRef"></a>Referências adicionais
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil suja](fsutil-dirty.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
