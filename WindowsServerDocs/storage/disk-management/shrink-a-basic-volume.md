---
title: Reduzir um volume básico
description: Este artigo descreve como reduzir um volume básico
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e54632b78fd67a65b51147323565130881d8d81b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885327"
---
# <a name="shrink-a-basic-volume"></a>Reduzir um volume básico

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode reduzir o espaço usado pelas partições primárias e unidades lógicas encolhendo-as para um espaço adjacente e contíguo no mesmo disco. Por exemplo, se você descobrir que precisa de partição adicional, mas não tem discos adicionais, é possível encolher a partição existente do final do volume para criar novo espaço não alocado, que poderá, em seguida, ser usado para uma nova partição. A operação de redução pode ser bloqueada com a presença de determinados tipos de arquivo. Para obter mais informações, consulte [Considerações adicionais](#addcon) 

Quando você diminui uma partição, quaisquer arquivos comuns são automaticamente realocados no disco para criar o novo espaço não alocado. Não é necessário reformatar o disco para reduzir a partição.

> [!CAUTION]
> Se a partição for uma partição bruta (ou seja, sem um sistema de arquivos) que contém dados (como um arquivo de banco de dados), a redução da partição pode destruir os dados.

## <a name="shrinking-a-basic-volume"></a>Reduzir um volume básico

-   [Usando a interface do Windows](#BKMK_WINUI)
-   [Usando uma linha de comando](#BKMK_CMD)

> [!NOTE]
> No mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores** para concluir estas etapas.

<a id="BKMK_WINUI"></a>
#### <a name="to-shrink-a-basic-volume-using-the-windows-interface"></a>Para reduzir um volume básico com a interface do Windows

1.  No Gerenciador de Disco, clique com o botão direito do mouse no volume básico que você deseja reduzir.

2.  Clique em **Reduzir volume**.

3.  Siga as instruções na tela.

<br />

> [!NOTE]
> Você pode reduzir somente volumes básicos que não têm nenhum sistema de arquivos ou que usam o sistema de arquivos NTFS.

<a id="BKMK_CMD"></a>
#### <a name="to-shrink-a-basic-volume-using-a-command-line"></a>Para reduzir um volume básico usando uma linha de comando

1.  Abra um prompt de comando e digite `diskpart`.

2.  No prompt de comando **DISKPART**, digite `list volume`. Observe o número do volume simples que você deseja reduzir.

3.  No prompt de comando **DISKPART**, digite `select volume <volumenumber>`. Seleciona o volume simples *volumenumber* que você deseja reduzir.

4.  No prompt de comando **DISKPART**, digite `shrink [desired=<desiredsize>] [minimum=<minimumsize>]`. Reduz o volume selecionado para *desiredsize* em megabytes (MB), se possível, ou para *minimumsize* se *desiredsize* foi muito grande.

<br />

| Valor | Descrição|
|---|---|
| <p>**list volume**</p> | <p>Exibe uma lista dos volumes básicos e dinâmicos em todos os discos.</p>|
| <p>**Selecione o volume**</p> | <p>Seleciona o volume especificado, onde <em>volumenumber</em> é o número do volume e concede foco a ele. Se nenhum volume for especificado, o comando **select** lista o volume atual com foco. Você pode especificar o volume por número, letra da unidade ou caminho do ponto de montagem. Em um disco básico, a seleção de um volume também oferece o foco de partição correspondente.</p> |
| <p>**shrink**</p> | <p>Reduz o volume com foco para criar um espaço não alocado. Não há perda de dados. Se a partição inclui os arquivos não transferíveis (como o arquivo de página ou a área de armazenamento de cópia de sombra), procedimento reduz o volume até o ponto em que os arquivos estão localizados. |
| <p>**desired=** <em>desiredsize</em></p> | <p>A quantidade de espaço, em megabytes (MB), para recuperar na partição atual.</p> |
| <p>**minimum=** <em>minimumsize</em></p> | <p>A quantidade de espaço mínima, em megabytes (MB), para recuperar na partição atual. Se você não especificar um tamanho mínimo ou desejado, o comando recupera a quantidade máxima de espaço possível.</p> 

<a id="addcon"></a>

## <a name="additional-considerations"></a>Considerações adicionais

-   Quando você diminui uma partição, determinados arquivos (por exemplo, o arquivo de paginação ou a área de armazenamento de cópia de sombra) não podem ser realocados automaticamente, e você não pode reduzir o espaço alocado além do ponto em que os arquivos estão localizados. Se a operação de redução falhar, verifique o Log do aplicativo e procure o Evento 259, que identifica o arquivo não transferível. Se você conhecer os clusters associados ao arquivo que está impedindo a operação de redução, você também pode usar o comando **fsutil** em um prompt de comando (digite **fsutil volume querycluster /?** para uso). Quando você fornece o parâmetro **querycluster**, a saída do comando identificará o arquivo não transferível que está impedindo o êxito da operação de redução.
Em alguns casos, você pode realocar o arquivo temporariamente. Por exemplo, se você precisar reduzir ainda mais a partição, é possível usar o Painel de Controle para mover o arquivo de paginação ou as cópias de sombra armazenadas para outro disco, excluir as cópias de sombra armazenadas, diminuir o volume e mover o arquivo de paginação de volta para o disco. Se a quantidade de clusters inválidos detectados pelo remapeamento de clusters inválidos for muito alto, você não pode reduzir a partição. Se isso ocorrer, você deve considerar mover os dados e substituir o disco.

-  Não use uma cópia de nível de bloco para transferir os dados. Isso também copia a tabela de setores defeituosos e o novo disco tratará os mesmos setores como tendo erros mesmo que estejam normais.

-   Você pode reduzir partições primárias e unidades lógicas em partições brutas (aquelas sem um sistema de arquivos) ou sistema de arquivos de partições usando o NTFS.

## <a name="see-also"></a>Consulte também

-   [Gerenciar volumes básicos](manage-basic-volumes.md)