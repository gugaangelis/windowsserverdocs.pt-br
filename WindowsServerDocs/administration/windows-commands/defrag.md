---
title: defrag
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2d930e224ac1610b5e49cbf5701778bfb6f14b2e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378715"
---
# <a name="defrag"></a>defrag

>Aplica-se a: Windows 10, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Localiza e consolida arquivos fragmentados em volumes locais para melhorar o desempenho do sistema.
A associação no grupo local de **Administradores** , ou equivalente, é o requisito mínimo necessário para executar esse comando.

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
|`<volume>`|Especifica a letra da unidade ou o caminho do ponto de montagem do volume a ser desfragmentado ou analisado.|
|A|Executar análise nos volumes especificados.|
|C|Execute a operação em todos os volumes.|
|D|Execute a desfragmentação tradicional (esse é o padrão). No entanto, em um volume em camadas, a desfragmentação tradicional é executada apenas na camada de capacidade.|
|E|Execute a operação em todos os volumes, exceto aqueles especificados.|
|G|Otimize as camadas de armazenamento nos volumes especificados.|
|H|Execute a operação em prioridade normal (o padrão é baixo).|
|Eu n|A otimização da camada seria executada para no máximo n segundos em cada volume.|
|K|Execute a consolidação de Slab nos volumes especificados.|
|L|Execute recortar nos volumes especificados.|
|M [n]|Execute a operação em cada volume em paralelo em segundo plano. No máximo, os n threads otimizam as camadas de armazenamento em paralelo.|
|O|Execute a otimização correta para cada tipo de mídia.|
|T|Rastreie uma operação que já está em andamento no volume especificado.|
|T|Imprima o andamento da operação na tela.|
|V|Imprima a saída detalhada que contém as estatísticas de fragmentação.|
|X|Execute a consolidação de espaço livre nos volumes especificados.|
|?|Exibe essas informações de ajuda.|

## <a name="remarks"></a>Comentários
- Não é possível desfragmentar tipos específicos de unidades ou volumes do sistema de arquivos:
  -   Não é possível desfragmentar volumes que o sistema de arquivos bloqueou.
  -   Não é possível desfragmentar volumes que o sistema de arquivos marcou como sujo, o que indica possível corrupção. Você deve executar **chkdsk** em um volume sujo antes de poder desfragmentá-lo. Você pode determinar se um volume está sujo usando o comando de consulta **fsutil** Dirty. Para obter mais informações sobre **chkdsk** e **fsutil** Dirty, consulte [referências adicionais](defrag.md#BKMK_additionalRef).
  -   Não é possível desfragmentar unidades de rede.
  -   Não é possível desfragmentar os cdROMs.
  -   Não é possível desfragmentar volumes do sistema de arquivos que não sejam **NTFS**, **ReFS**, **Fat** ou **FAT32**.
- Com o Windows Server 2008 R2, o Windows Server 2008 e o, o Windows Vista, você pode agendar para desfragmentar um volume. No entanto, não é possível agendar para desfragmentar uma unidade de estado sólido (SSD) ou um volume em um VHD (disco rígido virtual) que reside em um SSD.
- Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local ou deve ter recebido a autoridade apropriada. Se o computador fizer parte de um domínio, é possível que os membros do grupo Administradores de domínio possam executar esse procedimento. Como prática recomendada de segurança, considere o uso de **Executar como** para executar esse procedimento.
- Um volume deve ter pelo menos 15% de espaço livre para **desfragmentar de forma completa** e adequada. a **desfragmentação** usa esse espaço como uma área de classificação para fragmentos de arquivos. Se um volume tiver menos de 15% de espaço livre, a **desfragmentação** irá desfragmentá-lo apenas parcialmente. Para aumentar o espaço livre em um volume, exclua arquivos desnecessários ou mova-os para outro disco.
- Embora a **desfragmentação** esteja analisando e desfragmentando um volume, ele exibe um cursor piscando. Quando **a desfragmentação** termina de analisar e desfragmentar o volume, ele exibe o relatório de análise, o relatório de desfragmentação ou ambos os relatórios e, em seguida, sai para o prompt de comando.
- Por padrão, a **desfragmentação** exibirá um resumo dos relatórios de análise e de desfragmentação se você não especificar os parâmetros **/a** ou **/v** .
- Você pode enviar os relatórios para um arquivo de texto digitando **>** <em>filename. txt</em>, em que *filename. txt* é um nome de arquivo que você especificar. Por exemplo: `defrag volume /v > FileName.txt`
- Para interromper o processo de desfragmentação, na linha de comando, pressione **Ctrl + C**.
- A execução do comando de **desfragmentação** e do Desfragmentador de disco são mutuamente exclusivas. Se você estiver usando o Desfragmentador de disco para desfragmentar um volume e executar o comando de **desfragmentação** em uma linha de comando, o comando de **desfragmentação** falhará. Por outro lado, se você executar o **comando de** desfragmentação e abrir o Desfragmentador de disco, as opções de desfragmentação no Desfragmentador de disco não estarão disponíveis.

## <a name="BKMK_examples"></a>Disso
Para desfragmentar o volume na unidade C ao fornecer o progresso e a saída detalhada, digite:
```
defrag C: /U /V
```
Para desfragmentar os volumes nas unidades C e D em paralelo em segundo plano, digite:
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
A tarefa agendada da desfragmentação é executada como uma tarefa de manutenção e geralmente é agendada para ser executada todas as semanas. O administrador pode alterar a frequência usando o aplicativo **otimizar unidades** .
- Quando executado da tarefa agendada, a **desfragmentação** tem a política abaixo para o SSDs:
   - A **desfragmentação tradicional** (ou seja, mover arquivos para torná-los razoavelmente contíguos) e **recortar** é executada apenas uma vez por mês.
   - Se a **desfragmentação tradicional** e o **recorte** forem ignorados, a **análise** não será executada.
      - Se o usuário executou a **desfragmentação tradicional** manualmente em um SSD, digamos que 3 semanas após a última execução da tarefa agendada, a próxima execução agendada executará a **análise** e **recortará** , mas ignorará a **desfragmentação tradicional** nesse SSD.
   - Se a **análise** for ignorada, a hora da **última execução** em **otimizar unidades** não será atualizada.  Portanto, para o SSDs, o tempo da **última execução** em **otimizar unidades** pode ser um mês antigo.
- Essa tarefa de manutenção pode não desfragmentar todos os volumes, às vezes, porque essa tarefa faz o seguinte:
   - Não ativa o computador para executar a desfragmentação
   - Inicia somente se o computador estiver com corrente alternada e parar se o computador mudar para a energia da bateria
   - Para quando o computador deixar de ficar ocioso

## <a name="BKMK_additionalRef"></a>Referências adicionais
-   [chkdsk](chkdsk.md)
-   [fsutil](fsutil.md)
-   [fsutil Dirty](fsutil-dirty.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
