---
title: Reduzir um volume básico
description: Este artigo descreve como reduzir um volume básico
ms.date: 06/07/2019
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 652e5c85e0e094dd463302bffa8922ef5548dd6b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971173"
---
# <a name="shrink-a-basic-volume"></a>Reduzir um volume básico

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

É possível reduzir o espaço usado pelas partições primárias e unidades lógicas reduzindo-as para um espaço contíguo e adjacente no mesmo disco. Por exemplo, se você descobrir que precisa de uma partição adicional mas não tem discos adicionais, é possível reduzir a partição existente do final do volume para criar novo espaço não alocado, o qual poderá, em seguida, ser usado para uma nova partição. A operação de redução pode ser bloqueada com a presença de determinados tipos de arquivo. Para obter mais informações, consulte [Considerações adicionais](#additional-considerations)

Quando você reduz uma partição, quaisquer arquivos comuns são automaticamente realocados no disco para criar o novo espaço não alocado. Não é necessário reformatar o disco para reduzir a partição.

> [!CAUTION]
> Se a partição for uma partição bruta (ou seja, sem um sistema de arquivos) que contém dados (como um arquivo de banco de dados), reduzir a partição pode destruir os dados.

## <a name="shrinking-a-basic-volume"></a>Reduzir um volume básico

> [!NOTE]
> Para concluir estas etapas, no mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores**.

#### <a name="to-shrink-a-basic-volume-using-the-windows-interface"></a>Para reduzir um volume básico com a interface do Windows

1.  No Gerenciador de Discos, clique com o botão direito do mouse no volume básico que você quer reduzir.

2.  Clique em **Reduzir volume**.

3.  Siga as instruções na tela.


> [!NOTE]
> É possível reduzir somente volumes básicos que não tenham nenhum sistema de arquivos ou que usem o sistema de arquivos NTFS.

#### <a name="to-shrink-a-basic-volume-using-a-command-line"></a>Para reduzir um volume básico usando uma linha de comando

1.  Abra um prompt de comando e digite `diskpart`.

2.  No prompt **DISKPART**, digite `list volume`. Observe o número do volume simples que você quer reduzir.

3.  No prompt **DISKPART**, digite `select volume <volumenumber>`. Selecione o volume simples *volumenumber* que você quer reduzir.

4.  No prompt **DISKPART**, digite `shrink [desired=<desiredsize>] [minimum=<minimumsize>]`. Reduza o volume selecionado para *desiredsize* em MB (megabytes), se possível, ou para *minimumsize* se *desiredsize* foi muito grande.

| Valor             | Descrição |
| ---               | ----------- |
| **list volume** | Exibe uma lista dos volumes básicos e dinâmicos em todos os discos. |
| **select volume** | Seleciona o volume especificado, onde <em>volumenumber</em> é o número do volume e concede foco a ele. Se nenhum volume for especificado, o comando **select** lista o volume atual com foco. É possível especificar o volume por número, letra da unidade ou caminho do ponto de montagem. Em um disco básico, a seleção de um volume também fornece o foco da partição correspondente. |
| **shrink** | Reduz o volume com foco para criar um espaço não alocado. Não há perda de dados. Se a partição incluir arquivos não-móveis (como arquivo de paginação ou área de armazenamento de cópia de sombra), o volume será reduzido até o ponto em que esses arquivos não-móveis localizados. |
| **desired=** <em>desiredsize</em> | A quantidade de espaço, em MB (megabytes), para recuperar na partição atual. |
| **minimum=** <em>minimumsize</em> | A quantidade de espaço mínima, em MB (megabytes), para recuperar na partição atual. Se você não especificar um tamanho mínimo ou desejado, o comando recupera a quantidade máxima de espaço possível. |

## <a name="additional-considerations"></a>Considerações adicionais

-   Quando você reduz uma partição, determinados arquivos (por exemplo, o arquivo de paginação ou a área de armazenamento de cópia de sombra) não podem ser realocados automaticamente, e não será possível reduzir o espaço alocado além do ponto em que os arquivos não-móveis estão localizados.
Se a operação de redução falhar, verifique o Log do aplicativo e procure o Evento 259, que identifica o arquivo não-móvel. Se você souber o(s) cluster(s) associado(s) ao arquivo que está impedindo a operação de redução, também é possível usar o comando **fsutil** em um prompt de comando (digite **fsutil volume querycluster /?** para uso). Quando você fornece o parâmetro **querycluster**, a saída do comando identificará o arquivo não-móvel que está impedindo o êxito da operação de redução.
Em alguns casos, é possível realocar o arquivo temporariamente. Por exemplo, se você precisar reduzir ainda mais a partição, é possível usar o Painel de Controle para mover o arquivo de paginação ou as cópias de sombra armazenadas para outro disco, excluir as cópias de sombra armazenadas, reduzir o volume e, em seguida, mover o arquivo de paginação de volta para o disco. Se a quantidade de clusters inválidos detectados pelo remapeamento dinâmico de clusters inválidos for muito alta, não será possível reduzir a partição. Se isso ocorrer, você deve considerar mover os dados e substituir o disco.

-  Não use uma cópia em nível de bloco para transferir os dados. Isso também copia a tabela de setores defeituosos e o novo disco tratará os mesmos setores como defeituosos mesmo que estejam normais.

-   É possível reduzir partições primárias e unidades lógicas em partições brutas (aquelas sem um sistema de arquivos) ou partições usando o sistema de arquivos NTFS.

## <a name="see-also"></a>Consulte Também

-   [Gerenciar volumes básicos](manage-basic-volumes.md)