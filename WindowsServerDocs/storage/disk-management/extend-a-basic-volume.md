---
title: Estender um volume básico
description: Este artigo descreve como adicionar espaço a unidades primárias e lógicas para estender um volume básico
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4cad773746ae64a2244178be83e4d59c7c44b6a7
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812442"
---
# <a name="extend-a-basic-volume"></a>Estender um volume básico

> **Aplicável a:** Windows 10, Windows 8.1, Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

É possível adicionar mais espaço às partições primárias e unidades lógicas existentes estendendo-as para um espaço não alocado adjacente no mesmo disco. Para estender um volume básico, ele deve ser bruto (não formatado com um sistema de arquivos) ou formatado com o sistema de arquivos NTFS. É possível estender uma unidade lógica no espaço livre contíguo na partição estendida que contém essa unidade. Se você estender uma unidade lógica além do espaço livre disponível na partição estendida, a partição aumenta para conter a unidade lógica.

Para unidades lógicas e volumes de sistema ou de inicialização, é possível estender o volume apenas no espaço contíguo e somente se for possível fazer upgrade no disco para um disco dinâmico. Para outros volumes, é possível estender o volume no espaço não contíguo, mas será solicitado a você converter o disco para dinâmico.

## <a name="extending-a-basic-volume"></a>Estender um volume básico

#### <a name="to-extend-a-basic-volume-using-the-windows-interface"></a>Para estender um volume básico com a interface do Windows

1. No Gerenciador do Disco, clique com o botão direito do mouse no volume básico que você quer estender.

2. Clique em **Estender volume**.

3. Siga as instruções na tela.

#### <a name="to-extend-a-basic-volume-using-a-command-line"></a>Para estender um volume básico usando uma linha de comando

1. Abra um prompt de comando e digite `diskpart`.

2. No prompt **DISKPART**, digite `list volume`. Anote o volume básico que você quer estender.

3. No prompt **DISKPART**, digite `select volume <volumenumber>`. Isso seleciona o volume básico *volumenumber* que você quer estender no espaço contíguo vazio no mesmo disco.

4. No prompt **DISKPART**, digite `extend [size=<size>]`. Isso estende o volume selecionado por *tamanho* em MB (megabytes).

| Valor | Descrição |
| --- | --- |
| **list volume** | Exibe uma lista dos volumes básicos e dinâmicos em todos os discos. |
| **select volume** | Seleciona o volume especificado, onde <em>volumenumber</em> é o número do volume e concede foco a ele. Se nenhum volume for especificado, o comando **select** lista o volume atual com foco. Você pode especificar o volume por número, letra da unidade ou caminho do ponto de montagem. Em um disco básico, a seleção de um volume também oferece o foco de partição correspondente. |
| **extend** | <ul><li>Estende o volume com foco para o próximo espaço não alocado contíguo. Para volumes básicos, o espaço não alocado deve estar no mesmo disco e deve seguir (ter um deslocamento de setor maior do que) a partição com foco. Um volume dinâmico simples ou estendido pode ser estendido para qualquer espaço vazio em qualquer disco dinâmico. Ao usar esse comando, é possível estender um volume existente no espaço recém-criado.</li ><li>Se a partição foi previamente formatada com o sistema de arquivos NTFS, o sistema de arquivos é estendido automaticamente para ocupar a partição maior. Não há perda de dados. Se a partição foi previamente formatada com qualquer formato de sistema de arquivos diferente de NTFS, ocorre falha de comando sem alteração na partição.</li></ul> |
| **size=** <em>size</em> | A quantidade de espaço, em MB (megabytes), para adicionar à partição atual. Se você não especificar um tamanho, o disco é estendido para ocupar o espaço não alocado contíguo. |

## <a name="additional-considerations"></a>Considerações adicionais

-   Se o disco não tiver partições de inicialização ou de sistema, é possível estender o volume para outros discos sem inicialização ou sem sistema, mas ele será convertido em um disco dinâmico (se puder ser atualizado).

## <a name="see-also"></a>Consulte também

-   [Notação da sintaxe de linha de comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)
