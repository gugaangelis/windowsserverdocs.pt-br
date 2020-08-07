---
title: format
description: Artigo de referência para o comando Format, que formata um disco para aceitar arquivos do Windows.
manager: dongill
ms.author: jgerend
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: jasongerend
ms.date: 10/16/2017
ms.openlocfilehash: a923706252e6094cf12dcdf2632366ee7b2401f8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890124"
---
# <a name="format"></a>Formatar

> Aplica-se a: Windows 10, Windows Server 2016

Formata um disco para aceitar arquivos do Windows. Você deve ser um membro do grupo Administradores para formatar um disco rígido.

> [!NOTE]
> Você também pode usar o comando **Format** , com parâmetros diferentes, no console de recuperação. Para obter mais informações sobre o console de recuperação, consulte [ambiente de recuperação do Windows (Windows re)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxe

```
format <volume> [/fs:{FAT|FAT32|NTFS}] [/v:<label>] [/q] [/a:<unitsize>] [/c] [/x] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/f:<size>] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/t:<tracks> /n:<sectors>] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/p:<passes>]
format <volume> [/q]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<volume>` | Especifica o ponto de montagem, o nome do volume ou a letra da unidade (seguida por dois-pontos) da unidade que você deseja formatar. Se você não especificar nenhuma das opções de linha de comando a seguir, **Format** usará o tipo de volume para determinar o formato padrão para o disco. |
| /FS: {FAT | FAT32 | NT | Especifica o tipo de sistema de arquivos (FAT, FAT32, NTFS). |
| /v:`<label>` | Especifica o rótulo do volume. Se você omitir a opção de linha de comando **/v** ou usá-la sem especificar um rótulo de volume, o **formato** solicitará o rótulo do volume depois que a formatação for concluída. Use a sintaxe **/v:** para impedir o prompt para um rótulo de volume. Se você usar um único comando **format** para formatar mais de um disco, todos os discos receberão o mesmo rótulo de volume. |
| SRDF`<unitsize>` | Especifica o tamanho da unidade de alocação a ser usado nos volumes FAT, FAT32 ou NTFS. Se você não especificar *Units*, ele será escolhido com base no tamanho do volume. As configurações padrão são altamente recomendáveis para uso geral. A lista a seguir apresenta valores válidos para NTFS, FAT e *unidades*FAT32:<ul><li>512</li><li>1024</li><li>2.048</li><li>4096</li><li>8192</li><li>16K</li><li>32K</li><li>64 K</li></ul>FAT e FAT32 também suportam 128K e 256K para um tamanho de setor maior que 512 bytes. |
| /q | Executa uma formatação rápida. Exclui a tabela de arquivos e o diretório raiz de um volume formatado anteriormente, mas não executa uma verificação de setor por setor para áreas ruins. Você deve usar a opção de linha de comando **/q** para formatar somente os volumes formatados anteriormente que você sabe que estão em boas condições. Observe que **/q** substitui **/p**. |
| f`<size>` | Especifica o tamanho do disquete a ser formatado. Quando possível, use essa opção de linha de comando em vez das opções de linha de comando **/t** e **/n** . O Windows aceita os seguintes valores de tamanho:<ul><li>1440 ou 1440k ou 1440kb</li><li>1,44 ou 1,44 m ou 1,44 MB</li><li>1,44-MB, dois lados, densidade quádrupla, disco de 3,5 polegadas</li></ul> |
| /t:`<tracks>` | Especifica o número de trilhas no disco. Quando possível, use a opção de linha de comando **/f** em vez disso. Se você usar a opção **/t**, também deverá usar a opção **/n**. Juntas, essas opções fornecem um método alternativo de especificar o tamanho do disco que está sendo formatado. Essa opção não é válida com a opção **/f**. |
| opção`<sectors>` | Especifica o número de setores por trilha. Quando possível, use a opção de linha de comando **/f** em vez de **/n**. Se usar **/n**, também deverá usar **/t**. Juntas, essas duas opções fornecem um método alternativo de especificar o tamanho do disco que está sendo formatado. Essa opção não é válida com a opção **/f**. |
| /p`<passes>` | Zera todos os setores do volume para o número de etapas especificado. Essa opção não é válida com a opção **/q**. |
| /c | Apenas NTFS. Os arquivos criados no novo volume serão compactados por padrão. |
| /x | Faz com que o volume seja desmontado, se necessário, antes de ser formatado. Os identificadores abertos para o volume deixarão de ser válidos. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- O comando **Format** cria um novo diretório raiz e sistema de arquivos para o disco. Ele também pode verificar se há áreas ruins no disco e pode excluir todos os dados no disco. Para poder usar um novo disco, você deve primeiro usar esse comando para formatar o disco.

- Depois de Formatar um disquete, o **formato** exibirá a seguinte mensagem:

    `Volume label (11 characters, ENTER for none)?`

    Para adicionar um rótulo de volume, digite até 11 caracteres (incluindo espaços). Se você não quiser adicionar um rótulo de volume ao disco, pressione ENTER.

- Quando você usa o comando **Format** para formatar um disco rígido, uma mensagem de aviso semelhante à seguinte é exibida:

  ```
  WARNING, ALL DATA ON NON-REMOVABLE DISK
  DRIVE x: WILL BE LOST!
  Proceed with Format (Y/N)? _
  ```

  Para formatar o disco rígido, pressione **Y**; Se você não quiser formatar o disco, pressione **N**.

- Os sistemas de arquivos FAT restringem o número de clusters para não mais do que 65526. Os sistemas de arquivos FAT32 restringem o número de clusters entre 65527 e 4177917.

- Não há suporte para compactação NTFS para tamanhos de unidade de alocação acima de 4096.

  > [!NOTE]
  > O **formato** irá parar imediatamente o processamento se determinar que os requisitos anteriores não podem ser atendidos usando o tamanho de cluster especificado.

- Quando a formatação for concluída, o **formato** exibirá mensagens que mostram o espaço total em disco, os espaços marcados como defeituosos e o espaço disponível para os arquivos.

- Você pode acelerar o processo de formatação usando a opção de linha de comando **/q** . Use esta opção somente se não houver nenhum setor inválido no disco rígido.

- Você não deve usar o comando **Format** em uma unidade que foi preparada usando o comando **subst** . Não é possível formatar discos em uma rede.

- A tabela a seguir lista cada código de saída e uma breve descrição de seu significado.

  | Código de saída | Descrição |
  | --------- | ----------- |
  | 0 | A operação de formatação foi bem-sucedida. |
  | 1 | Os parâmetros incorretos foram fornecidos. |
  | 4 | Ocorreu um erro fatal (que é qualquer erro diferente de 0, 1 ou 5). |
  | 5 | O usuário pressionou N em resposta ao prompt "continuar com o formato (s/N)?" para interromper o processo. |

  Você pode verificar esses códigos de saída usando a variável de ambiente ERRORLEVEL com o comando em lote **if**.

### <a name="examples"></a>Exemplos

Para formatar um novo disquete na unidade A utilizando o tamanho padrão, digite:

```
format a:
```

Para executar uma operação de formatação rápida em um disquete formatado anteriormente na unidade A, digite:

```
format a: /q
```

Para formatar um disquete na unidade A e atribuí-lo os *dados*do rótulo do volume, digite:

```
format a: /v:DATA
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc771080(v=ws.11))
