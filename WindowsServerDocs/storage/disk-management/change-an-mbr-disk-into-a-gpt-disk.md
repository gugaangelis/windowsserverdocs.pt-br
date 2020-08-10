---
title: Transformar um disco MBR (Registro Mestre de Inicialização) em um disco GPT (Tabela de Partição GUID)
description: Descreve como transformar um disco MBR (Registro Mestre de Inicialização) em um disco GPT (Tabela de Partição GUID)
ms.date: 06/07/2019
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 86934f2bf86ea8f91dfbe92f97952d76542a6bc4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935905"
---
# <a name="convert-an-mbr-disk-into-a-gpt-disk"></a>Converter um disco MBR em GPT

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os discos MBR (Registro Mestre de Inicialização) usam a tabela de partição padrão do BIOS. Os discos GPT (Tabela de Partição GUID) usam UEFI (Unified Extensible Firmware Interface). Uma vantagem dos discos GPT é que você pode ter mais de quatro partições em cada disco. GPT também é necessária para discos com mais de 2 TB (terabytes).

É possível alterar um estilo de partição de disco de MBR para GPT desde que o disco não contenha nenhuma partição ou volume.

> [!NOTE]
> Antes de converter um disco, faça backup dos dados contidos nele e feche todos os programas que estão acessando o disco.

> [!NOTE]
> Para concluir estas etapas, no mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores**.

## <a name="converting-using-the-windows-interface"></a>Converter usando a interface do Windows

1.  Faça backup ou mova os dados no disco MBR básico que você deseja converter em disco GPT.

2.  Se o disco contiver partições ou volumes, clique com botão direito do mouse em cada e, em seguida, clique em **Excluir partição** ou **Excluir volume**.

3.  Clique com botão direito do mouse no disco MBR que você deseja transformar em um disco GPT e, em seguida, clique em **Converter em Disco GPT**.

## <a name="converting-using-a-command-line"></a>Converter usando uma linha de comando

Execute as seguintes etapas para converter um disco MBR vazio em um disco GPT. Há também uma ferramenta MBR2GPT.EXE que você pode usar, mas é um pouco complicado. Consulte [Converter partição MBR em GPT](/windows/deployment/mbr-to-gpt) para obter mais detalhes.

1.  Faça backup ou mova os dados no disco MBR básico que você deseja converter em disco GPT.

2.  Abra um prompt de comando com privilégios elevados clicando com o botão direito do mouse em **Prompt de comando** e selecione **Executar como administrador**.

3. Digite `diskpart`. Se o disco não contém quaisquer partições ou volumes, vá para a etapa 6.

4.  No prompt **DISKPART**, digite `list disk`. Observe o número do disco que você deseja converter.

5.  No prompt **DISKPART**, digite `select disk <disknumber>`.

6.  No prompt **DISKPART**, digite `clean`.

    > [!NOTE]
    > A execução do comando **clean** excluirá todas as partições ou volumes no disco.

7.  No prompt **DISKPART**, digite `convert gpt`.

| Valor  | Descrição  |
| ----- | ---- |
| **list disk** | Exibe uma lista de discos e informações sobre eles, como o tamanho, a quantidade de espaço livre disponível, se o disco é básico ou dinâmico e se o disco usa o estilo de partição MBR (Registro Mestre de Inicialização) ou GPT (Tabela de Partição GUID). O disco marcado com um asterisco (*) tem foco. |
| **select disk** *disknumber* | Seleciona o disco especificado, onde *disknumber* é o número do disco e concede foco a ele. |
| **clean** | Remove todas as partições ou volumes do disco com foco.  |
| **convert gpt**| Converte um disco básico vazio com o estilo de partição MBR (Registro Mestre de Inicialização) em um disco básico com o estilo de partição GPT (Tabela de Partição GUID). |

## <a name="see-also"></a>Consulte Também

-   [Notação da sintaxe de linha de comando](/previous-versions/orphan-topics/ws.11/cc742449(v=ws.11))
