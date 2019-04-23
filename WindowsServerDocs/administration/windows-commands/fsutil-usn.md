---
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
title: fsutil usn
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4bef63f4938b5ce786bbbfdbf3bdd2081280ce95
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829977"
---
# <a name="fsutil-usn"></a>fsutil usn
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gerencia o diário de alterações de USN (número) de sequência de atualização.

## <a name="syntax"></a>Sintaxe

```
fsutil usn [createjournal] m=<MaxSize> a=<AllocationDelta> <VolumePath>
fsutil usn [deletejournal] {/D | /N} <volumepath>
fsutil usn [enablerangetracking] <volumepath> [options]
fsutil usn [enumdata] <FileRef> <LowUSN> <HighUSN> <VolumePath>
fsutil usn [queryjournal] <VolumePath>
fsutil usn [readdata] <FileName>
fsutil usn [readjournal] [c= <chunk-size> s=<file-size-threshold>] <volumepath>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|createjournal|Cria um diário de alterações de USN.|
|m=\<MaxSize>|Especifica o tamanho máximo, em bytes, que o NTFS aloca para o diário de alterações.|
|a=\<AllocationDelta>|Especifica o tamanho, em bytes, da alocação de memória que é adicionado ao final e removidos desde o início do diário de alterações.|
|\<VolumePath>|Especifica a letra da unidade (seguida por dois-pontos).|
|deletejournal|Exclui ou desativa um diário de alterações do USN. **Cuidado:** Excluir o diário de alterações afeta a replicação FRS (serviço) e o serviço de indexação, pois ela exigiria que esses serviços para realizar uma verificação completa (e demorada) do volume. Negativamente por sua vez, isso afeta a replicação do SYSVOL do FRS e a replicação entre alternativos de link do DFS enquanto o volume está sendo examinado novamente.|
|/d|Desabilita um diário de alterações do USN e retorna o controle de e/s () de entrada/saída enquanto o diário de alterações está sendo desativado.|
|/n|Desabilita um diário de alterações do USN e retorna o controle de e/s somente depois que o diário de alterações está desabilitado.|
|enablerangetracking|Permite que o intervalo de gravação de USN de acompanhamento para um volume.|
|c=\<chunk-size>|Especifica o tamanho do bloco para rastrear em um volume.|
|s=\<file-size-threshold>|Especifica o limite de tamanho do arquivo para o intervalo de rastreamento.|
|enumdata|Enumera e relaciona as entradas de diário de alteração entre dois limites especificados.|
|\<FileRef>|Especifica a posição ordinal dentro dos arquivos no volume no qual a enumeração deve iniciar.|
|\<LowUSN>|Especifica o limite inferior do intervalo de USN valores usados para filtrar os registros que são retornados. Somente os registros cujo último USN de diário de alteração é entre ou igual do *LowUSN* e *HighUSN* valores membro são retornados.|
|\<HighUSN>|Especifica o limite superior dos valores de intervalo de USN usados para filtrar os arquivos que são retornados.|
|queryjournal|Consulta dados de USN de um volume para reunir informações sobre o diário de alteração atual, seus registros e sua capacidade.|
|readdata|Lê os dados de USN para um arquivo.|
|\<FileName>|Especifica o caminho completo para o arquivo, incluindo o nome de arquivo e extensão, por exemplo: C:\documents\filename.txt|
|readjournal|Lê os registros de USN no diário de USN.|
|minver=\<number>|Versão principal do mínimo de USN_RECORD para retornar. Padrão = 2.|
|maxver=\<number>|Versão principal do máximo de USN_RECORD para retornar. Padrão = 4.|
|startusn=\<USN number>|USN para começar a ler o diário de USN no. Padrão = 0.|


## <a name="remarks"></a>Comentários

-   Sobre o diário de alterações de USN

    O diário de alterações de USN fornece um log persistente de todas as alterações feitas nos arquivos no volume. Como arquivos, diretórios e outros objetos NTFS são adicionados, excluídos e modificados, o NTFS insere registros no diário de alterações de USN, um para cada volume no computador. Cada registro indica o tipo de alteração e o objeto alterado. Novos registros são acrescentados ao final do fluxo.

    Os programas podem consultar o diário de alterações de USN para determinar todas as modificações feitas em um conjunto de arquivos. O diário de alterações de USN é muito mais eficiente do que a verificação de carimbos de data / hora ou se registrar para notificações de arquivo. O diário de alterações de USN é habilitado e usado pelo serviço de indexação, replicação FRS (serviço), serviços de instalação remota (RIS) e o armazenamento remoto.

-   Usando **createjournal**

    Se já existir um diário de alterações em um volume, o **createjournal** parâmetro atualiza o diário de alterações *MaxSize* e *Intervalo_de_alocação* parâmetros. Isso permite que você expandir o número de registros que um diário ativo mantém sem precisar desabilitá-lo.

-   Usando **m**

    O diário de alterações pode ficar maior do que esse valor de destino, mas o diário de alterações é truncado no próximo ponto de verificação do NTFS para menos que esse valor. NTFS examina o diário de alterações e apara quando seu tamanho excede o valor de *MaxSize* mais o valor de *Intervalo_de_alocação*. Em pontos de verificação do NTFS, o sistema operacional grava registros para o arquivo de log do NTFS que permitem que o NTFS determinar qual processamento é necessário para se recuperar de uma falha.

-   Usando **um**

    O diário de alteração pode crescer até mais do que a soma dos valores de *MaxSize* e *Intervalo_de_alocação* antes que está sendo cortado.

-   Usando **deletejournal**

    Excluir ou desabilitar um diário de alterações é muito demorado, pois o sistema precisa acessar todos os registros na tabela mestra de arquivos (MFT) e defina o atributo de USN último como 0 (zero). Esse processo pode levar vários minutos e pode continuar após a reinicialização do sistema, caso seja necessária uma reinicialização. Durante esse processo, o diário de alteração não é considerado ativo, nem é desativado. Enquanto o sistema estiver desativando o diário, ele não pode ser acessado e todas as operações de diário retornam erros. Você deve ter extremo cuidado ao desabilitar um diário, pois isso afeta negativamente outros aplicativos que estão usando o diário.

## <a name="BKMK_examples"></a>Exemplos
Para criar um USN diário de alteração na unidade C, digite:

```
fsutil usn createjournal m=1000 a=100 c:
```

Para excluir um ativo USN diário de alteração na unidade C, digite:

```
fsutil usn deletejournal /d c:
```

Para habilitar o intervalo de rastreamento com um tamanho de bloco especificado e o limite de tamanho do arquivo, digite:

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

Para enumerar e listar as entradas de diário de alteração entre dois limites especificados na unidade C, digite:

```
fsutil usn enumdata 1 0 1 c:
```

Para consultar dados USN de um volume na unidade C, digite:

```
fsutil usn queryjournal c:
```

Para ler os dados de USN para um arquivo na pasta \Temp na unidade C, digite:

```
fsutil usn readdata c:\temp\sample.txt
```

Para ler o diário de USN com um USN de início específica, digite:

```
fsutil usn readjournal startusn=0xF00
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


