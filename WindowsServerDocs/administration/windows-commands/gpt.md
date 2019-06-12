---
title: gpt
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cba9839f98dfd5a72289838273a057dd0e09a7e5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438185"
---
# <a name="gpt"></a>gpt

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Em discos de gpt (tabela) de partição GUID básicos, atribui o atributo de gpt para a partição com foco.  atributos de partição GPT oferecem informações adicionais sobre o uso da partição. Alguns atributos são específicos para o tipo de partição GUID.

> [!CAUTION]
> Alterar os atributos de gpt pode fazer com que os volumes de dados básicos para falhar a ser atribuído letras de unidade, ou para impedir que o sistema de arquivos de montagem. Você não deve alterar os atributos de gpt, a menos que você for um fabricante de equipamento original (OEM) ou um profissional de TI que é experiência com os discos gpt.
> ## <a name="syntax"></a>Sintaxe
> ```
> gpt attributes=<n>
> ```
> ## <a name="parameters"></a>Parâmetros
> 
> |   Parâmetro    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
> |----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | attributes=<n> | Especifica o valor do atributo que você deseja aplicar para a partição com foco. O campo de atributo gpt é um campo de 64 bits que contém dois subcampos. O campo superior é interpretado somente no contexto da ID da partição, enquanto o campo inferior é comuns a todas as IDs de partição. Os valores aceitos são:<br /><br />-   **0x0000000000000001**. Especifica que a partição seja exigida por computador para funcionar corretamente.<br />-   **0x8000000000000000**. Especifica que a partição não receberá uma letra de unidade por padrão quando o disco for movido para outro computador ou quando o disco é visto pela primeira vez por um computador.<br />-   **0x4000000000000000**. Oculta o volume de uma partição. Ou seja, a partição não será detectada pelo Gerenciador de montagem.<br />-   **0x2000000000000000**. Especifica que a partição é uma cópia de sombra de outra partição.<br />-   **0x1000000000000000**. Especifica que a partição é somente leitura. Esse atributo impede que o volume que estão sendo gravados.<br /><b />Para obter mais informações sobre esses atributos, consulte a seção de atributos em [create_PARTITION_PARAMETERS estrutura](https://go.microsoft.com/fwlink/?LinkId=203812) (<https://go.microsoft.com/fwlink/?LinkId=203812>). |
> 
> ## <a name="remarks"></a>Comentários
> - A partição do sistema EFI só contém os binários necessários para iniciar o sistema operacional. Isso torna mais fácil para os binários de OEM ou binários específicos a um sistema operacional a ser colocado em outras partições.
> - Uma partição gpt básica deve ser selecionada para essa operação seja bem-sucedida. Use o **Selecionar partição** comando para selecionar uma partição gpt básica e mudar o foco a ele.
>   ## <a name="BKMK_examples"></a>Exemplos
>   Se você estiver movendo um disco gpt para um novo computador e gostaria de impedir que esse computador automaticamente atribuir uma letra de unidade para a partição com foco, tipo:
>   ```
>   gpt attributes=0x8000000000000000
>   ```

