---
title: Alterar um disco de um Registro Mestre de Inicialização (MBR) para Tabela de Partição de GUID (GPT)
description: Descreve como alterar um Registro mestre de inicialização (MBR) para uma Tabela de partição de GUID (GPT)
ms.date: 06/19/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8af76edbdb5fc2aa5768811f01c563aab1a89fc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849597"
---
# <a name="convert-an-mbr-disk-into-a-gpt-disk"></a>Converter um disco MBR em um disco GPT

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os discos de Registro Mestre de Inicialização (MBR) usam a tabela de partição padrão da BIOS. Os discos de Tabela de Partição de GUID (GPT) usam a Unified Extensible Firmware Interface (UEFI). Uma vantagem dos discos GPT é que você pode ter mais de quatro partições em cada disco. A GPT também é necessária para discos com mais de 2 terabytes (TB).

Você pode alterar um estilo de partição de disco de MBR para GPT desde que o disco não contenha nenhuma partição ou volume.


> [!NOTE]
> Antes de converter um disco, faça backup dos dados nele e feche os programas que estão acessando o disco.


> [!NOTE]
> No mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores** para concluir estas etapas.

<a id="BKMK_WINUI"></a>

## <a name="converting-using-the-windows-interface"></a>Converter usando a interface do Windows

1.  Faça backup ou mova os dados no disco MBR básico que você deseja converter em GPT.

2.  Se o disco tiver partições ou volumes, clique com botão direito do mouse em cada uma e, em seguida, clique em **Excluir partição** ou **Excluir volume**.

3.  Clique com botão direito do mouse no disco MBR que você deseja transformar em um disco GPT e, em seguida, clique em **Converter em Disco GPT**.

<a id="BKMK_CMD"></a>

## <a name="converting-using-a-command-line"></a>Converter a linha de comando

Use as seguintes etapas para converter um disco MBR vazio em um disco GPT. Há também um MBR2GPT. Ferramenta EXE que você pode usar, mas é um pouco complicado – consulte [partição converter MBR para GPT](https://docs.microsoft.com/windows/deployment/mbr-to-gpt) para obter mais detalhes.

1.  Faça backup ou mova os dados no disco MBR básico que você deseja converter em GPT.

2.  Abra um prompt de comando com privilégios elevados ao clicar com o botão direito do mouse em **Prompt de comando** e selecione **Executar como administrador**.

3. Digite `diskpart`. Se o disco não contém quaisquer partições ou volumes, vá para a etapa 6.

4.  No prompt de comando **DISKPART**, digite `list disk`. Observe o número do disco que você deseja converter.

5.  No prompt de comando **DISKPART**, digite `select disk <disknumber>`.

6.  No prompt de comando **DISKPART**, digite `clean`.

    > [!NOTE]
    > A execução do comando **clean** excluirá todas as partições ou volumes no disco.

7.  No prompt de comando **DISKPART**, digite `convert gpt`.

<br />

| Valor  | Descrição  |
| ----- | ----|
| <p>**disco de lista**</p> | <p>Exibe uma lista de discos e informações sobre eles, como o tamanho, a quantidade de espaço livre disponível, se o disco é básico ou dinâmico, e se o disco usa o estilo de partição Registro Mestre de Inicialização (MBR) ou a Tabela de Partição de GUID (GPT). O disco marcado com um asterisco (*) tem foco.</p> |
| <p>**Selecione o disco** <em>disknumber</em></p> | <p>Seleciona o disco especificado, onde <em>disknumber</em> é o número do disco e concede foco a ele.</p> |
| <p>**clean**</p> | <p>Remove todas as partições ou volumes do disco com foco.</p>  |
| <p>**Converter gpt**</p>| <p>Converte um disco básico vazio com o estilo de partição de Registro Mestre de Inicialização (MBR) em um disco básico com o estilo de partição de Tabela de Partição de GUID (GPT).</p> |

## <a name="see-also"></a>Consulte também

-   [Notação de sintaxe de linha de comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


