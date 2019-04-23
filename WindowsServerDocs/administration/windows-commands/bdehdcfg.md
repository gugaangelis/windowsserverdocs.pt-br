---
title: bdehdcfg
description: Tópico de comandos do Windows para **bdehdcfg** -prepara um disco rígido com as partições necessárias para a criptografia de unidade BitLocker.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4fda562f4aa6b8caa885fcd70155b7150c91d12
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869697"
---
# <a name="bdehdcfg"></a>bdehdcfg



Prepara um disco rígido com as partições necessárias para a criptografia de unidade BitLocker. A maioria das instalações do Windows 7 não precisará usar essa ferramenta porque a instalação do BitLocker inclui a capacidade de preparar e reparticionar unidades conforme necessário.

> [!WARNING]
> Existe um conflito conhecido com a configuração **Negar acesso de gravação a unidades fixas não protegidas por BitLocker** da Política de Grupo localizada em **Configuração do Computador\Modelos Administrativos\Componentes do Windows\Criptografia de Unidade de Disco BitLocker\Unidades de Dados Fixas**.</br>> Se Bdehdcfg for executado em um computador, quando essa configuração de política está habilitada, você pode encontrar os seguintes problemas:</br>> – Se você tentar reduzir a unidade e criar a unidade do sistema, o tamanho da unidade será reduzido com êxito e uma partição não processada será criada. Entretanto, a partição não processada não será formatada. A seguinte mensagem de erro é exibida: "A nova unidade ativa não pode ser formatada. Pode ser necessário preparar manualmente a unidade para o BitLocker."</br>> – Se você tentar usar o espaço não alocado para criar a unidade do sistema, uma partição não processada será criada. Entretanto, a partição não processada não será formatada. A seguinte mensagem de erro é exibida: "A nova unidade ativa não pode ser formatada. Pode ser necessário preparar manualmente a unidade para o BitLocker."</br>> – Se você tentar mesclar uma unidade existente na unidade do sistema, a ferramenta não copiar o arquivo de inicialização necessária na unidade de destino para criar a unidade do sistema. A seguinte mensagem de erro é exibida: "Falha da configuração do BitLocker ao copiar arquivos de inicialização. Pode ser necessário preparar manualmente a unidade para o BitLocker."</br>> Se esta configuração de política está sendo imposta, um disco rígido não pode ser reparticionado, pois a unidade estará protegida. Se estiver atualizando os computadores de sua organização a partir de uma versão anterior do Windows e eles estiverem configurados com uma única partição, você deverá criar a partição de sistema BitLocker necessária antes de aplicar a configuração da política aos computadores.

Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Bdehdcfg: driveinfo](bdehdcfg-driveinfo.md)|Exibe a letra da unidade, o tamanho total, o espaço livre máximo e as características da partição das partições na unidade especificada. Apenas as partições válidas são listadas. O espaço não alocado não será listado se já existirem quatro partições primárias ou estendidas.|
|[Bdehdcfg: destino](bdehdcfg-target.md)|Define qual parte de uma unidade a ser usada como a unidade do sistema e faz parte do Active Directory.|
|[Bdehdcfg: newdriveletter](bdehdcfg-newdriveletter.md)|Atribui uma nova letra de unidade para a parte de uma unidade usada como a unidade do sistema.|
|[Bdehdcfg: size](bdehdcfg-size.md)|Determina o tamanho da partição do sistema quando uma nova unidade do sistema está sendo criada.|
|[Bdehdcfg: quiet](bdehdcfg-quiet.md)|Impede a exibição de todas as ações e erros na interface de linha de comando e direciona Bdehdcfg para usar a resposta "Sim" para qualquer Sim/preparação de unidade sem prompts que podem ocorrer durante subsequentes.|
|[bdehdcfg: reiniciar](bdehdcfg-restart.md)|Direciona o computador seja reiniciado após a preparação da unidade.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir ilustra o que está sendo usado com a unidade padrão para criar uma partição de sistema de 500 MB de Bdehdcfg. Como nenhuma letra da unidade é especificada, a nova partição de sistema não terá uma letra de unidade.
```
bdehdcfg -target default -size 500
```
O exemplo a seguir ilustra Bdehdcfg que está sendo usado com a unidade padrão para criar uma partição de sistema (p) do tamanho padrão de 300 MB espaço não alocado na unidade. A ferramenta não solicitará ao usuário para qualquer entrada adicional nem todos os erros serão ser exibidos. Depois que a unidade do sistema tiver sido criada, o computador será reiniciado automaticamente.
```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)