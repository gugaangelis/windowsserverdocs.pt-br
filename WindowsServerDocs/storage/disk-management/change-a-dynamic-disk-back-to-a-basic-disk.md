---
title: Reverter um disco dinâmico em um disco básico
description: Descreve como converter um disco dinâmico de volta em um disco básico.
ms.date: 12/18/2019
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 3ffaf5db818377a3c75fda98255e7881fe6731aa
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936005"
---
# <a name="change-a-dynamic-disk-back-to-a-basic-disk"></a>Reverter um disco dinâmico em um disco básico

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve como excluir tudo em um disco dinâmico e, em seguida, convertê-lo de volta em um disco básico. Discos dinâmicos foram preteridos no Windows e não recomendamos usá-los mais. É recomendável usar a tecnologia de discos básicos ou usar o [Espaços de Armazenamento](https://support.microsoft.com/help/12438/windows-10-storage-spaces) mais recente quando quiser fazer um pool de discos em volumes maiores. Se você quiser espelhar o volume de inicialização do Windows, convém usar um controlador de hardware RAID, como o incluso em muitas placas-mãe.

> [!WARNING]
> Para converter um disco dinâmico de volta em um disco básico, é necessário excluir todos os volumes do disco, além de apagar permanentemente todos os dados contidos nele. Faça o backup de todos os dados que você deseja manter antes de prosseguir.

## <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-by-using-disk-management"></a>Para converter um disco dinâmico em disco básico usando o Gerenciamento de Disco

1.  Faça o backup de todos os volumes do disco que você quer converter de dinâmico para básico.

2. Abra o Gerenciamento de Disco com permissões de administrador.

   Para fazer isso facilmente, digite **Gerenciamento do Computador** na caixa de pesquisa na barra de tarefas, selecione e mantenha o cursor (ou clique com o botão direito do mouse) em **Gerenciamento do Computador** e, em seguida, selecione **Executar como administrador** > **Sim**. Depois que o Gerenciamento do Computador for aberto, acesse **Armazenamento** > **Gerenciamento de Disco**.

2.  No Gerenciamento de Disco, selecione e mantenha o cursor (ou clique com o botão direito do mouse) em cada volume no disco dinâmico que você quer converter em disco básico e, em seguida, clique em **Excluir Volume**.

3.  Quando todos os volumes no disco forem excluídos, clique com botão direito do mouse no disco e, em seguida, clique em **Converter em disco básico**.

## <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-by-using-a-command-line"></a>Para reverter um disco dinâmico em um disco básico usando uma linha de comando

1.  Faça o backup de todos os volumes do disco que você quer converter de dinâmico para básico.

2.  Abra um prompt de comando e digite `diskpart`.

3.  No prompt **DISKPART**, digite `list disk`. Observe o número do disco que você quer converter para básico.

4.  No prompt **DISKPART**, digite `select disk <disknumber>`.

5.  No prompt **DISKPART**, digite `detail disk`.

6.  Para cada volume no disco, no prompt **DISKPART**, digite `select volume= <volumenumber>` e, em seguida, digite `delete volume`.

7.  No prompt **DISKPART**, digite `select disk <disknumber>`, especificando o número do disco que você quer converter em disco básico.

8.  No prompt **DISKPART**, digite `convert basic`.

| Valor  | Descrição |
| --- | --- |
| **list disk**                         | Exibe uma lista de discos e informações sobre eles, como o tamanho, a quantidade de espaço livre disponível, se o disco é básico ou dinâmico e se o disco usa o estilo de partição MBR (Registro Mestre de Inicialização) ou GPT (Tabela de Partição GUID). O disco marcado com um asterisco (*) tem foco. |
| **select disk** <em>disknumber</em>   | Seleciona o disco especificado, onde <em>disknumber</em> é o número do disco e concede foco a ele.  |
| **detail disk** <em>disknumber</em>   | Exibe as propriedades do disco selecionado e os volumes existentes nele.  |
| **select volume** <em>disknumber</em> | Seleciona o volume especificado, onde <em>disknumber</em> é o número do volume e concede foco a ele. Se nenhum volume for especificado, o comando **select** lista o volume atual com foco. É possível especificar o volume por número, letra da unidade ou caminho do ponto de montagem. Em um disco básico, a seleção de um volume também oferece o foco de partição correspondente. |
| **delete volume**                     | Exclui o volume selecionado. Não é possível excluir o volume do sistema, o volume de inicialização ou qualquer volume contendo o arquivo de paginação ativo ou o despejo (despejo de memória). |
| **convert basic** | Converte um disco dinâmico vazio em um disco básico.  |

## <a name="additional-considerations"></a>Considerações adicionais

-   O disco não deve conter quaisquer volumes ou dados antes de você poder revertê-lo para um disco básico. Se quiser manter seus dados, faça o backup ou transfira-os para outro volume antes de converter o disco em um disco básico.
-   Ao alterar um disco dinâmico para um disco básico, será possível criar somente partições e unidades lógicas existentes nele.

## <a name="additional-references"></a>Referências adicionais

-   [Notação da sintaxe de linha de comando](/previous-versions/orphan-topics/ws.11/cc742449(v=ws.11))
