---
title: bdehdcfg
description: Artigo de referência para o comando BdeHdCfg, que prepara um disco rígido com as partições necessárias para Criptografia de Unidade de Disco BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4c92cd74-188e-4fec-b7c4-fe4e8903e032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a8b926459f2d1fb96b9a48910c163eb9aaab02c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923353"
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

## <a name="syntax"></a>Sintaxe

```
bdehdcfg [–driveinfo <drive_letter>] [-target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}] [–newdriveletter] [–size <size_in_mb>] [-quiet]
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
| /? | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
