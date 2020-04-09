---
title: bdehdcfg
description: O tópico de comandos do Windows para **BdeHdCfg**, que prepara um disco rígido com as partições necessárias para criptografia de unidade de disco BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9adf8bbfb655e0820fcff6385d3663fc7abbd9a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850999"
---
# <a name="bdehdcfg"></a>bdehdcfg

Prepara um disco rígido com as partições necessárias para Criptografia de Unidade de Disco BitLocker. A maioria das instalações do Windows 7 não precisarão usar essa ferramenta, pois a configuração do BitLocker inclui a capacidade de preparar e reparticionar unidades conforme necessário.

> [!WARNING]
> Existe um conflito conhecido com a configuração **Negar acesso de gravação a unidades fixas não protegidas por BitLocker** da Política de Grupo localizada em **Configuração do Computador\Modelos Administrativos\Componentes do Windows\Criptografia de Unidade de Disco BitLocker\Unidades de Dados Fixas**.
>
>Se o BdeHdCfg for executado em um computador quando essa configuração de política estiver habilitada, você poderá encontrar os seguintes problemas:
>
>- Se você tentar reduzir a unidade e criar a unidade do sistema, o tamanho da unidade será reduzida com êxito e uma partição não processada será criada. Entretanto, a partição não processada não será formatada. A seguinte mensagem de erro é exibida: a nova unidade ativa não pode ser formatada. Talvez seja necessário preparar manualmente a unidade para o BitLocker.
>
>- Se você tentar usar um espaço não alocado para criar a unidade do sistema, uma partição não processada será criada. Entretanto, a partição não processada não será formatada. A seguinte mensagem de erro é exibida: a nova unidade ativa não pode ser formatada. Talvez seja necessário preparar manualmente a unidade para o BitLocker.
>
>- Se você tentar mesclar uma unidade existente à unidade do sistema, ocorrerá uma falha na ferramenta ao copiar o arquivo de inicialização necessário na unidade de destino para a criação da unidade do sistema. A seguinte mensagem de erro é exibida: a configuração do BitLocker falhou ao copiar os arquivos de inicialização. Talvez seja necessário preparar manualmente a unidade para o BitLocker.
>
>- Se a configuração dessa política estiver sendo imposta, um disco rígido não poderá ser reparticionado, pois a unidade estará protegida. Se estiver atualizando os computadores de sua organização a partir de uma versão anterior do Windows e eles estiverem configurados com uma única partição, você deverá criar a partição de sistema BitLocker necessária antes de aplicar a configuração da política aos computadores.

Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg [–driveinfo <DriveLetter>] [-target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}] [–newdriveletter] [–size <SizeinMB>] [-quiet]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |----------- |
| [BdeHdCfg: DriveInfo](bdehdcfg-driveinfo.md) | Exibe a letra da unidade, o tamanho total, o máximo de espaço livre e as características de partição das partições na unidade especificada. Apenas as partições válidas são listadas. O espaço não alocado não será listado se já existirem quatro partições primárias ou estendidas. |
| [BdeHdCfg: destino](bdehdcfg-target.md) | Define qual parte de uma unidade usar como a unidade do sistema e torna a parte ativa. |
| [BdeHdCfg: newdriveletter](bdehdcfg-newdriveletter.md) | Atribui uma nova letra da unidade à parte de uma unidade usada como a unidade do sistema. |
| [BdeHdCfg: tamanho](bdehdcfg-size.md) | Determina o tamanho da partição do sistema quando uma nova unidade do sistema está sendo criada. |
| [BdeHdCfg: Quiet](bdehdcfg-quiet.md) | Impede a exibição de todas as ações e erros na interface de linha de comando e direciona o BdeHdCfg para usar a resposta Sim para quaisquer prompts Sim/Não que possam ocorrer durante a preparação de unidade subsequente. |
| [BdeHdCfg: reiniciar](bdehdcfg-restart.md) | Instrui o computador a reiniciar após a conclusão da preparação da unidade. |
| /? | Exibe a Ajuda no prompt de comando. |

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

O exemplo a seguir descreve BdeHdCfg sendo usado com a unidade padrão para criar uma partição de sistema de 500 MB. Como nenhuma letra da unidade foi especificada, a nova partição do sistema não terá uma letra da unidade.

```
bdehdcfg -target default -size 500
```

O exemplo a seguir ilustra o BdeHdCfg que está sendo usado com a unidade padrão para criar uma partição do sistema (P:) do tamanho padrão de 300 MB de espaço não alocado na unidade. A ferramenta não solicitará a entrada do usuário nem nenhum erro será exibido. Depois que a unidade do sistema tiver sido criada, o computador será reiniciado automaticamente.

```
bdehdcfg -target unallocated –newdriveletter P: -quiet -restart
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)