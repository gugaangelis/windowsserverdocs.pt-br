---
title: "Alterar um disco de um Registro Mestre de Inicialização (MBR) para Tabela de Partição de GUID (GPT)"
description: "Descreve como alterar um Registro mestre de inicialização (MBR) para uma Tabela de partição de GUID (GPT)"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f28d151d3b1d6b19b9ca330c5ff8576a53acbfa5
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="change-a-master-boot-record-mbr-disk-into-a-guid-partition-table-gpt-disk"></a>Alterar um disco de Registro Mestre de Inicialização (MBR) para Tabela de Partição de GUID (GPT)

> **Aplicável a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os discos de Registro Mestre de Inicialização (MBR) usam a tabela de partição padrão da BIOS. Os discos de Tabela de Partição de GUID (GPT) usam a Unified Extensible Firmware Interface (UEFI). Uma vantagem dos discos GPT é que você pode ter mais de quatro partições em cada disco. A GPT também é necessária para discos com mais de 2 terabytes (TB).

Você pode alterar um estilo de partição de disco de MBR para GPT desde que o disco não contenha nenhuma partição ou volume.
Você não pode usar o estilo de partição GPT em mídia removível ou em discos de cluster que estão conectados a barramentos SCSI ou de Fibre Channel compartilhados que são usados pelo serviço de cluster.

> [!NOTE]
> Antes de converter discos, feche todos os programas em execução nesses discos.

## <a name="changing-an-mbr-disk-into-a-gpt-disk"></a>Converter um disco MBR para GPT

-   [Usando a interface do Windows](#BKMK_WINUI)
-   [Usando uma linha de comando](#BKMK_CMD)

> [!NOTE]
> No mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores** para concluir estas etapas.

<a id="BKMK_WINUI"></a>
#### <a name="to-change-an-mbr-disk-into-a-gpt-disk-using-the-windows-interface"></a>Para converter um disco MBR para GPT usando a interface do Windows

1.  Faça backup ou mova os dados no disco MBR básico que você deseja converter em GPT.

2.  Se o disco tiver partições ou volumes, clique com botão direito do mouse em cada uma e, em seguida, clique em **Excluir partição** ou **Excluir volume**.

3.  Clique com botão direito do mouse no disco MBR que você deseja transformar em um disco GPT e, em seguida, clique em **Converter em Disco GPT**.

<a id="BKMK_CMD"></a>
#### <a name="to-change-an-mbr-disk-into-a-gpt-disk-using-a-command-line"></a>Para transformar um disco MBR em um disco GPT usando uma linha de comando

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

| Value  | Descrição  |
| ----- | ----|
| <p>**list disk**</p> | <p>Exibe uma lista de discos e informações sobre eles, como o tamanho, a quantidade de espaço livre disponível, se o disco é básico ou dinâmico, e se o disco usa o estilo de partição Registro Mestre de Inicialização (MBR) ou a Tabela de Partição de GUID (GPT). O disco marcado com um asterisco (*) tem foco.</p> |
| <p>**select disk** <em>disknumber</em></p> | <p>Seleciona o disco especificado, onde <em>disknumber</em> é o número do disco e concede foco a ele.</p> |
| <p>**clean**</p> | <p>Remove todas as partições ou volumes do disco com foco.</p>  |
| <p>**convert gpt**</p>| <p>Converte um disco básico vazio com o estilo de partição de Registro Mestre de Inicialização (MBR) em um disco básico com o estilo de partição de Tabela de Partição de GUID (GPT).</p> |

## <a name="see-also"></a>Consulte também

-   [Notação da sintaxe de linha de comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


