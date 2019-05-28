---
title: Alterar um disco de Tabela de Partição de GUID (GPT) para um disco de Registro Mestre de Inicialização (MBR)
description: Descreve como alterar um disco de Tabela de Partição de GUID (GPT) para um disco de estilo de partição de Registro Mestre de Inicialização (MBR).
ms.date: 06/19/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c42cffec0ddc1ae480ae67982147e9f186f0e50a
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222858"
---
# <a name="convert-a-gpt-disk-into-an-mbr-disk"></a>Converter um disco GPT em um disco MBR

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os discos de Registro Mestre de Inicialização (MBR) usam a tabela de partição padrão da BIOS. Os discos de Tabela de Partição de GUID (GPT) usam a Unified Extensible Firmware Interface (UEFI). Os discos MBR não oferecem suporte a mais de quatro partições em cada disco. O método de partição MBR não é recomendado para discos com mais de 2 terabytes (TB).

Você pode alterar um estilo de partição de disco de GPT para MBR desde que o disco esteja vazio e não contenha nenhum volume.

> [!NOTE]
> Antes de converter um disco, faça backup dos dados nele e feche os programas que estão acessando o disco.

> [!NOTE]
> No mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores** para concluir estas etapas.

## <a name="converting-using-the-windows-interface"></a>Converter usando a interface do Windows

1.  Faça backup ou mova todos os volumes no disco GPT básico que você deseja converter em MBR.

2.  Se o disco tiver partições ou volumes, clique com botão direito do mouse em cada uma e, em seguida, clique em **Excluir volume**.

3.  Clique com botão direito do mouse no disco GPT que você deseja transformar em um disco MBR e, em seguida, clique em **Converter em disco MBR**.

## <a name="converting-using-a-command-line"></a>Converter a linha de comando

1.  Faça backup ou mova todos os volumes no disco GPT básico que você deseja converter em MBR.

2.  Abra um prompt de comando com privilégios elevados ao clicar com o botão direito do mouse em **Prompt de comando** e selecione **Executar como administrador**.

3. Digite `diskpart`. Se o disco não contém partições ou volumes, vá para a etapa 6.

4.  No prompt de comando **DISKPART**, digite `list disk`. Observe o número do disco que você deseja excluir.

5.  No prompt de comando **DISKPART**, digite `select disk <disknumber>`.

6.  No prompt de comando **DISKPART**, digite `clean`.

    > [!IMPORTANT]
    > A execução do comando **clean** excluirá todas as partições ou volumes no disco.

7.  No prompt de comando **DISKPART**, digite `convert mbr`.

<br />

| Valor | Descrição |
| --- | --- |
| <p>**disco de lista**</p> | <p>Exibe uma lista de discos e informações sobre eles, como o tamanho, a quantidade de espaço livre disponível, se o disco é Básico ou Dinâmico, e se o disco usa o estilo de partição Registro Mestre de Inicialização (MBR) ou a Tabela de Partição de GUID (GPT). O disco marcado com um asterisco (*) tem foco.</p> |
| <p>**Selecione o disco**</p> | <p>Seleciona o disco especificado, onde <em>disknumber</em> é o número do disco e concede foco a ele.</p> | <p>**clean**</p> | <p>Remove todas as partições ou volumes do disco com foco.</p> |
| <p>**convert mbr**</p> | <p>Converte um disco básico vazio com o estilo de partição da Tabela de Partição de GUID (GPT) em um disco básico com o estilo de partição de Registro Mestre de Inicialização (MBR).</p>

## <a name="see-also"></a>Consulte também

-   [Notação de sintaxe de linha de comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


