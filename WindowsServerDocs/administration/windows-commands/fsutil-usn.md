---
title: fsutil usn
description: Artigo de referência do comando fsutil USN, que gerencia o diário de alterações do USN (número de sequência de atualização).
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b3f71588c7221f0ba8e3659d9a1a1dd133971ce9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930471"
---
# <a name="fsutil-usn"></a>fsutil usn

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Gerencia o diário de alterações do USN (número de sequência de atualização). O diário de alterações do USN fornece um log persistente de todas as alterações feitas nos arquivos no volume. Como arquivos, diretórios e outros objetos NTFS são adicionados, excluídos e modificados, o NTFS insere registros no diário de alterações do USN, um para cada volume no computador. Cada registro indica o tipo de alteração e o objeto alterado. Novos registros são acrescentados ao final do fluxo.

## <a name="syntax"></a>Sintaxe

```
fsutil usn [createjournal] m=<maxsize> a=<allocationdelta> <volumepath>
fsutil usn [deletejournal] {/d | /n} <volumepath>
fsutil usn [enablerangetracking] <volumepath> [options]
fsutil usn [enumdata] <fileref> <lowUSN> <highUSN> <volumepath>
fsutil usn [queryjournal] <volumepath>
fsutil usn [readdata] <filename>
fsutil usn [readjournal] [c= <chunk-size> s=<file-size-threshold>] <volumepath>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| createjournal | Cria um diário de alterações de USN. |
| m =`<maxsize>` | Especifica o tamanho máximo, em bytes, que o NTFS aloca para o diário de alterações. |
| a =`<allocationdelta>` | Especifica o tamanho, em bytes, da alocação de memória que é adicionada ao final e removida do início do diário de alterações. |
| `<volumepath>` | Especifica a letra da unidade (seguida por dois-pontos). |
| deletejournal | Exclui ou desabilita um diário de alterações de USN ativo.<p>**Cuidado:** A exclusão do diário de alterações afeta o FRS (serviço de replicação de arquivos) e o serviço de indexação, pois exige que esses serviços executem uma verificação completa (e demorada) do volume. Isso, por sua vez, afeta negativamente a replicação SYSVOL do FRS e a replicação entre alternativas de link DFS enquanto o volume está sendo examinado novamente. |
| /d | Desabilita um diário de alterações de USN ativo e retorna O controle de entrada/saída (e/s) enquanto o diário de alterações está sendo desabilitado. |
| /n | Desabilita um diário de alterações de USN ativo e retorna O controle de e/s somente depois que o diário de alterações é desabilitado. |
| enablerangetracking | Habilita o rastreamento do intervalo de gravação de USN para um volume. |
| c =`<chunk-size>` | Especifica o tamanho da parte a ser rastreada em um volume. |
| s =`<file-size-threshold>` | Especifica o limite de tamanho do arquivo para rastreamento de intervalo. |
| enumdata | Enumera e lista as entradas do diário de alterações entre dois limites especificados. |
| `<fileref>` | Especifica a posição ordinal dentro dos arquivos no volume no qual a enumeração deve começar. |
| `<lowUSN>` | Especifica o limite inferior do intervalo de valores USN usados para filtrar os registros que são retornados. Somente os registros cujo USN do último diário de alterações está entre ou igual aos valores de membro *lowUSN* e *highUSN* são retornados. |
| `<highUSN>` | Especifica o limite superior do intervalo de valores USN usados para filtrar os arquivos retornados. |
| queryjournal | Consulta os dados de USN de um volume para coletar informações sobre o diário de alterações atual, seus registros e sua capacidade. |
| readdata | Lê os dados de USN de um arquivo. |
| `<filename>` | Especifica o caminho completo para o arquivo, incluindo o nome do arquivo e a extensão, por exemplo: *C:\documents\filename.txt*. |
| readjournal | Lê os registros USN no diário de USN. |
| Minver =`<number>` | Versão principal mínima do USN_RECORD para retornar. Padrão = 2. |
| MaxVer =`<number>` | Versão principal máxima do USN_RECORD a ser retornada. Padrão = 4. |
| startusn =`<USN number>` | USN para o qual começar a ler o diário de USN. Padrão = 0. |

#### <a name="remarks"></a>Comentários

- Os programas podem consultar o diário de alterações do USN para determinar todas as modificações feitas em um conjunto de arquivos. O diário de alterações do USN é muito mais eficiente do que verificar carimbos de data/hora ou registrar notificações de arquivo. O diário de alterações do USN é habilitado e usado pelo serviço de indexação, pelo FRS (serviço de replicação de arquivos), pelo RIS (serviços de instalação remota) e pelo armazenamento remoto.

- Se um diário de alterações já existir em um volume, o parâmetro **createjournal** atualizará os parâmetros **MaxSize** e **allocationdelta** do diário de alterações. Isso permite expandir o número de registros que um diário ativo mantém sem precisar desabilitá-lo.

- O diário de alterações pode crescer maior que esse valor de destino, mas o diário de alterações é truncado no próximo ponto de verificação NTFS para menos que esse valor. O NTFS examina o diário de alterações e o corta quando seu tamanho excede o valor de **MaxSize** mais o valor de **allocationdelta**. Em pontos de verificação NTFS, o sistema operacional grava registros no arquivo de log NTFS que habilita o NTFS para determinar qual processamento é necessário para se recuperar de uma falha.

- O diário de alterações pode aumentar para mais do que a soma dos valores de **MaxSize** e **allocationdelta** antes de ser aparado.

- Excluir ou desabilitar um diário de alterações ativo é muito demorado, pois o sistema deve acessar todos os registros na MFT (tabela de arquivos mestre) e definir o último atributo USN como 0 (zero). Esse processo pode levar vários minutos e pode continuar depois que o sistema for reiniciado, se uma reinicialização for necessária. Durante esse processo, o diário de alterações não é considerado ativo, nem está desabilitado. Enquanto o sistema está desabilitando o diário, ele não pode ser acessado e todas as operações de diário retornam erros. Você deve usar extrema atenção ao desabilitar um diário ativo, pois ele afeta negativamente outros aplicativos que estão usando o diário.

### <a name="examples"></a>Exemplos

Para criar um diário de alterações de USN na unidade C, digite:

```
fsutil usn createjournal m=1000 a=100 c:
```

Para excluir um diário de alterações de USN ativo na unidade C, digite:

```
fsutil usn deletejournal /d c:
```

Para habilitar o rastreamento de intervalo com um tamanho de bloco e tamanho de arquivo especificados, digite:

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

Para enumerar e listar as entradas do diário de alterações entre dois limites especificados na unidade C, digite:

```
fsutil usn enumdata 1 0 1 c:
```

Para consultar dados de USN para um volume na unidade C, digite:

```
fsutil usn queryjournal c:
```

Para ler os dados de USN de um arquivo na pasta \temp na unidade C, digite:

```
fsutil usn readdata c:\temp\sample.txt
```

Para ler o diário de USN com um USN de início específico, digite:

```
fsutil usn readjournal startusn=0xF00
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
