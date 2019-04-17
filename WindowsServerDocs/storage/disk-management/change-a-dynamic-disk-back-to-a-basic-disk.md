---
title: "Converter um disco dinâmico em disco básico"
description: "Descreve como converter um disco dinâmico em disco básico."
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9675141ec02c2362702265a18f5405554dff6038
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="change-a-dynamic-disk-back-to-a-basic-disk"></a>Converter um disco dinâmico em disco básico

> **Aplicável a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve como excluir tudo em um disco dinâmico e, em seguida, convertê-lo em um disco básico. Os discos dinâmicos foram substituídos no Windows. É recomendado usar a tecnologia de [Espaços de armazenamento](https://support.microsoft.com/help/12438/windows-10-storage-spaces) mais recente quando desejar fazer transformar um pool de discos em volumes maiores. Se você quiser espelhar o volume de inicialização do Windows, convém usar um controlador RAID de hardware, como o incluído em muitas placas-mãe.

<br />

> [!WARNING]
> Para converter um disco dinâmico em um disco básico é necessário excluir todos os volumes do disco, além de apagar permanentemente todos os dados dele. Faça o backup de todos os dados que você deseja manter antes de prosseguir.

## <a name="changing-a-dynamic-disk-back-to-a-basic-disk"></a>Converter um disco dinâmico em um disco básico

-   [Usando a interface do Windows](#BKMK_WINUI)
-   [Usando uma linha de comando](#BKMK_CMD)

> [!NOTE]
> No mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores** para concluir estas etapas.

<a href="" id="BKMK_WINUI"></a>
#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-the-windows-interface"></a>Para converter um disco dinâmico em um disco básico usando a interface do Windows
1.  Faça o backup de todos os volumes do disco que você deseja converter de dinâmico para básico.

2.  No Gerenciamento de Disco, clique com botão direito do mouse em cada volume no disco dinâmico que você deseja converter em um disco básico e clique em **Excluir Volume** para cada volume no disco.

3.  Quando todos os volumes no disco forem excluídos, clique com botão direito do mouse no disco e, em seguida, clique em **Converter em disco básico**.


<a href="" id="BKMK_CMD"></a>
#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-a-command-line"></a>Para converter um disco dinâmico em um disco básico usando uma linha de comando

1.  Faça o backup de todos os volumes do disco que você deseja converter de dinâmico para básico.

2.  Abra um prompt de comando e digite `diskpart`.

3.  No prompt de comando **DISKPART**, digite `list disk`. Observe o número do disco que você deseja converter para básico.

4.  No prompt de comando **DISKPART**, digite `select disk <disknumber>`.

5.  No prompt de comando **DISKPART**, digite `detail disk <disknumber>`.

6.  Para cada volume no disco, no prompt de comando **DISKPART**, digite `select volume= <volumenumber>` e, em seguida, digite `delete volume`.

7.  No prompt de comando **DISKPART**, digite `select disk <disknumber>`, especificando o número do disco que você deseja converter para disco básico.

8.  No prompt de comando **DISKPART**, digite `convert basic`.
 
<br /> <br />

| Valor  | Descrição |
| --- |---|
| <p>**list disk**</p>                         | <p>Exibe uma lista de discos e informações sobre eles, como o tamanho, a quantidade de espaço livre disponível, se o disco é básico ou dinâmico, e se o disco usa o estilo de partição Registro Mestre de Inicialização (MBR) ou a Tabela de Partição de GUID (GPT). O disco marcado com um asterisco (*) tem foco.</p> |
| <p>**select disk** <em>disknumber</em></p>   | <p>Seleciona o disco especificado, onde <em>disknumber</em> é o número do disco e concede foco a ele.</p>  |
| <p>**detail disk** <em>disknumber</em></p>   | <p>Exibe as propriedades do disco selecionado e os volumes existentes nele.</p>  |
| <p>**select volume** <em>disknumber</em></p> | <p>Seleciona o volume especificado, onde <em>disknumber</em> é o número do volume e concede foco a ele. Se nenhum volume for especificado, o comando **select** lista o volume atual com foco. Você pode especificar o volume por número, letra da unidade ou caminho do ponto de montagem. Em um disco básico, a seleção de um volume também oferece o foco de partição correspondente.</p> |
| <p>**delete volume**</p>                     | <p>Exclui o volume selecionado. Você não pode excluir o volume do sistema, o volume de inicialização ou qualquer volume contend o arquivo de paginação ativo ou o despejo de pane (despejo de memória).</p> |
| <p>**convert basic**</p> | <p>Converte um disco dinâmico vazio em um disco básico.</p>  |

## <a name="additional-considerations"></a>Considerações adicionais

-   O disco não deve conter todos os volumes ou os dados antes de você poder alterá-lo para um disco básico. Se desejar manter seus dados, faça o backup ou transfira-os para outro volume antes de converter o disco em um disco básico.
-   Ao alterar um disco dinâmico para um disco básico, você pode criar somente partições e unidades lógicas existentes nele.

## <a name="see-also"></a>Consulte também

-   [Notação da sintaxe de linha de comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


