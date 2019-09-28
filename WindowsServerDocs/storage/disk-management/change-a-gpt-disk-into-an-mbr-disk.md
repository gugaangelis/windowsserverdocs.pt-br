---
title: Transformar um disco GPT (Tabela de Partição GUID) em um disco MBR (Registro Mestre de Inicialização)
description: Descreve como converter um disco GUID (Tabela de Partição GUID) em um disco do estilo de partição MBR (Registro Mestre de Inicialização).
ms.date: 06/19/2018
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5c6efb0697af663b32ce6f0e27634c3962eca492
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402103"
---
# <a name="convert-a-gpt-disk-into-an-mbr-disk"></a>Transformar um disco GPT em um disco MBR

> **Aplicável a:** Windows 10, Windows 8.1, Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os discos MBR (Registro Mestre de Inicialização) usam a tabela de partição padrão do BIOS. Os discos GPT (Tabela de Partição GUID) usam UEFI (Unified Extensible Firmware Interface). Os discos MBR não dão suporte a mais de quatro partições em cada disco. O método de partição MBR não é recomendado para discos com mais de 2 TB (terabytes).

É possível mudar um disco de um estilo de partição GPT para MBR desde que o disco esteja vazio e não contenha nenhum volume.

> [!NOTE]
> Antes de converter um disco, faça backup dos dados contidos nele e feche todos os programas que estão acessando o disco.

> [!NOTE]
> Para concluir estas etapas, no mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores**.

## <a name="converting-using-the-windows-interface"></a>Converter usando a interface do Windows

1.  Faça backup ou mova todos os volumes no disco GPT básico que você quer converter em disco MBR.

2.  Se o disco tiver partições ou volumes, clique com botão direito do mouse em cada partição ou volume e, em seguida, clique em **Excluir volume**.

3.  Clique com botão direito do mouse no disco GPT que você quer transformar em um disco MBR e, em seguida, clique em **Converter em disco MBR**.

## <a name="converting-using-a-command-line"></a>Converter usando uma linha de comando

1.  Faça backup ou mova todos os volumes no disco GPT básico que você quer converter em disco MBR.

2.  Abra um prompt de comando com privilégios elevados clicando com o botão direito do mouse em **Prompt de comando** e selecione **Executar como administrador**.

3. Digite `diskpart`. Se o disco não contém partições ou volumes, vá para a etapa 6.

4.  No prompt **DISKPART**, digite `list disk`. Observe o número do disco que você quer excluir.

5.  No prompt **DISKPART**, digite `select disk <disknumber>`.

6.  No prompt **DISKPART**, digite `clean`.

    > [!IMPORTANT]
    > A execução do comando **clean** excluirá todas as partições ou volumes no disco.

7.  No prompt **DISKPART**, digite `convert mbr`.

|                Valor                  |      Descrição   |
| ------------------------------------- | -----------------  |
|  <strong>list disk</strong>  | Exibe uma lista de discos e informações sobre eles, como o tamanho, a quantidade de espaço livre disponível, se o disco é Básico ou Dinâmico e se o disco usa o estilo de partição MBR (Registro Mestre de Inicialização) ou GPT (Tabela de Partição GUID). O disco marcado com um asterisco (\*) tem foco. |
| <strong>select disk</strong> |                                                                                                          Seleciona o disco especificado, onde <em>disknumber</em> é o número do disco e concede foco a ele.                                                                                                           |
| <strong>convert mbr</strong> |                                                                               Converte um disco básico vazio com o estilo de partição GPT (Tabela de Partição GUID) em um disco básico com o estilo de partição MBR (Registro Mestre de Inicialização).                                                                                |

## <a name="see-also"></a>Consulte também

-   [Notação da sintaxe de linha de comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)