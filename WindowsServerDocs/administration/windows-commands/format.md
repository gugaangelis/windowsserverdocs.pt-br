---
title: Formatar
ms.prod: windows-server
manager: dongill
ms.author: JGerend
ms.technology: storage
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: JasonGerend
ms.date: 10/16/2017
ms.openlocfilehash: 7db57ac8115d99327fea72c12695a2ca6d3bc05f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377042"
---
# <a name="format"></a>Formatar
> Aplica-se a: Windows 10, Windows Server 2016

Formata um disco para aceitar arquivos do Windows.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
format <Volume> [/fs:{FAT|FAT32|NTFS}] [/v:<Label>] [/q] [/a:<UnitSize>] [/c] [/x] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/f:<Size>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/t:<Tracks> /n:<Sectors>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/p:<Passes>]
format <Volume> [/q]
```

## <a name="parameters"></a>Parâmetros

|   Parâmetro    |                                                                                                                                                                                                                    Descrição                                                                                                                                                                                                                     |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<Volume >    |                                                                                         Especifica o ponto de montagem, o nome do volume ou a letra da unidade (seguida por dois-pontos) da unidade que você deseja formatar. Se você não especificar nenhuma das opções de linha de comando a seguir, **Format** usará o tipo de volume para determinar o formato padrão para o disco.                                                                                         |
|    /FS: {FAT    |                                                                                                                                                                                                                       FAT32                                                                                                                                                                                                                        |
|  /v: \<Label >   |                           Especifica o rótulo do volume. Se você omitir a opção de linha de comando **/v** ou usá-la sem especificar um rótulo de volume, o **formato** solicitará o rótulo do volume depois que a formatação for concluída. Use a sintaxe **/v:** para impedir o prompt para um rótulo de volume. Se você usar um único comando **format** para formatar mais de um disco, todos os discos receberão o mesmo rótulo de volume.                            |
| /a: \<UnitSize > | Especifica o tamanho da unidade de alocação a ser usado nos volumes FAT, FAT32 ou NTFS. Se você não especificar *UnitSize*, ele será escolhido com base no tamanho do volume. As configurações padrão são altamente recomendáveis para uso geral. A lista a seguir apresenta os valores válidos para *UnitSize* de NTFS, FAT e FAT32:</br>512</br>1024</br>2048</br>4096</br>8192</br>16K</br>32K</br>64K</br>FAT e FAT32 também suportam 128K e 256K para um tamanho de setor maior que 512 bytes. |
|       /q       |                                                       Executa uma formatação rápida. Exclui a tabela de arquivos e o diretório raiz de um volume formatado anteriormente, mas não executa uma verificação de setor por setor para áreas ruins. Você deve usar a opção de linha de comando **/q** para formatar somente os volumes formatados anteriormente que você sabe que estão em boas condições. Observe que **/q** substitui **/p**.                                                       |
|   /f: \<Size >   |                                                         Especifica o tamanho do disquete a ser formatado. Quando possível, use essa opção de linha de comando em vez das opções de linha de comando **/t** e **/n** . O Windows aceita os seguintes valores de tamanho:</br>-1440 ou 1440k ou 1440kb</br>-   1,44 ou 1,44 m ou 1,44 mb</br>-1,44-MB, dupla face, densidade quádrupla, disco de 3,5 polegadas                                                         |
|  /t: \<Tracks >  |                                                    Especifica o número de trilhas no disco. Quando possível, use a opção de linha de comando **/f** em vez disso. Se você usar a opção **/t**, também deverá usar a opção **/n**. Juntas, essas opções fornecem um método alternativo de especificar o tamanho do disco que está sendo formatado. Essa opção não é válida com a opção **/f**.                                                     |
| /n: \<Sectors >  |                                                         Especifica o número de setores por trilha. Quando possível, use a opção de linha de comando **/f** em vez de **/n**. Se usar **/n**, também deverá usar **/t**. Juntas, essas duas opções fornecem um método alternativo de especificar o tamanho do disco que está sendo formatado. Essa opção não é válida com a opção **/f**.                                                         |
|  /p: \<Passes >  |                                                                                                                                                               Zera todos os setores do volume para o número de etapas especificado. Essa opção não é válida com a opção **/q**.                                                                                                                                                                |
|       /c       |                                                                                                                                                                                     Apenas NTFS. Os arquivos criados no novo volume serão compactados por padrão.                                                                                                                                                                                      |
|       /x       |                                                                                                                                                            Faz o volume ser desmontado, se necessário, antes de ser formatado. Os identificadores abertos para o volume deixarão de ser válidos.                                                                                                                                                            |
|       /?       |                                                                                                                                                                                                        Exibe a ajuda no prompt de comando.                                                                                                                                                                                                        |

## <a name="remarks"></a>Comentários

-   Credenciais administrativas

    Você deve ser um membro do grupo Administradores para formatar um disco rígido.
-   Usando o **formato**

    O comando **Format** cria um novo diretório raiz e sistema de arquivos para o disco. Ele também pode verificar se há áreas ruins no disco e pode excluir todos os dados no disco. Para poder usar um novo disco, você deve primeiro usar esse comando para formatar o disco.
-   Adicionando um rótulo de volume

    Depois de Formatar um disquete, o **formato** exibirá a seguinte mensagem:

    `Volume label (11 characters, ENTER for none)?`

    Para adicionar um rótulo de volume, digite até 11 caracteres (incluindo espaços). Se você não quiser adicionar um rótulo de volume ao disco, pressione ENTER.
-   Formatando um disco rígido

> [!NOTE]
> Você deve ser um membro do grupo Administradores para formatar um disco rígido.

Quando você usa o comando **Format** para formatar um disco rígido, uma mensagem de aviso semelhante à seguinte é exibida:
```
WARNING, ALL DATA ON NON-REMOVABLE DISK 
DRIVE x: WILL BE LOST! 
Proceed with Format (Y/N)? _ 
```
Para formatar o disco rígido, pressione Y; Se você não quiser formatar o disco, pressione N.
-   Tamanho da unidade

    Os sistemas de arquivos FAT restringem o número de clusters para não mais do que 65526. Os sistemas de arquivos FAT32 restringem o número de clusters entre 65527 e 4177917.

    Não há suporte para compactação NTFS para tamanhos de unidade de alocação acima de 4096.

> [!NOTE]
> O **formato** irá parar imediatamente o processamento se determinar que os requisitos anteriores não podem ser atendidos usando o tamanho de cluster especificado.
> -   Mensagens de **formato**

    When formatting is complete, **format** displays messages that show the total disk space, the spaces marked as defective, and the space available for your files.
- Formatação rápida

  Você pode acelerar o processo de formatação usando a opção de linha de comando **/q** . Use esta opção somente se não houver nenhum setor inválido no disco rígido.
- Usar **format** com uma unidade reatribuída ou uma unidade de rede

  Você não deve usar o comando **format** em uma unidade que tenha sido preparada usando o comando **subst**. Você não pode formatar discos através de uma rede.
- Códigos de saída de **format**

  A tabela a seguir lista cada código de saída e uma breve descrição de seu significado.  

  |Código de Saída|Descrição|
  |---------|-----------|
  |0|A operação de formatação foi bem-sucedida.|
  |1|Os parâmetros incorretos foram fornecidos.|
  |4|Ocorreu um erro fatal (que é qualquer erro diferente de 0, 1 ou 5).|
  |5|O usuário pressionou N em resposta ao prompt "continuar com o formato (s/N)?" para interromper o processo.|

  Você pode verificar esses códigos de saída usando a variável de ambiente ERRORLEVEL com o comando em lote **if**.
- Usar **format** no Console de Recuperação

  O comando **format**, com parâmetros diferentes, está disponível no Console de Recuperação.

## <a name="BKMK_examples"></a>Disso

Para formatar um novo disquete na unidade A utilizando o tamanho padrão, digite:
```
format a:
```
Para executar uma operação de formatação rápida em um disquete formatado anteriormente na unidade A, digite:
```
format a: /q
```
Para formatar um disquete na unidade A e atribuir a ele o rótulo de volume "DATA", digite:
```
format a: /v:DATA
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](https://technet.microsoft.com/library/cc771080.aspx)