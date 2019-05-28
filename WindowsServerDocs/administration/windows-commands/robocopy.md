---
title: robocopy
description: Saiba como usar o comando robocopy, no Windows e Windows Server para copiar arquivos
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4c6e8e9-fcb3-4a4a-9d04-2d8c367b6354
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 07/25/2018
ms.openlocfilehash: a10b3d3877e9511164d298bcc1dab11540e6f596
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188202"
---
# <a name="robocopy"></a>robocopy

Copia dados de arquivo.

## <a name="syntax"></a>Sintaxe

```
robocopy <Source> <Destination> [<File>[ ...]] [<Options>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<origem >|Especifica o caminho para o diretório de origem.|
|\<Destino >|Especifica o caminho para o diretório de destino.|
|\<arquivo >|Especifica o arquivo ou arquivos a serem copiados. Você pode usar caracteres curinga (**&#42;** ou **?** ), se você quiser. Se o **arquivo** parâmetro não for especificado, **\*.\*** é usado como o valor padrão.|
|\<Opções >|Especifica as opções a serem usados com o **robocopy** comando.|

### <a name="copy-options"></a>Opções de cópia

|Opção|Descrição|
|------|-----------|
|/s|Subdiretórios de cópias. Observe que essa opção exclui diretórios vazios.|
|/e|Subdiretórios de cópias. Observe que essa opção inclui pastas vazias. Para obter mais informações, consulte [comentários](#remarks).|
|/lev:\<N>|Copia somente os primeiros *N* níveis da árvore de diretório de origem.|
|/z|Copia os arquivos no modo reiniciável.|
|/b|Copia os arquivos no modo de Backup.|
|/zb|Usa o modo reiniciável. Se o acesso é negado, essa opção usa o modo de Backup.|
|/efsraw|Copia todos os arquivos criptografados em modo RAW EFS.|
|/copy:\<CopyFlags>|Especifica as propriedades de arquivo a ser copiado. A seguir está os valores válidos para essa opção:</br>**1!d** dados</br>**Um** atributos</br>**T** carimbos de horas</br>**S** lista de controle de acesso (ACL) do NTFS</br>**O** informações do proprietário</br>**U** informações de auditoria</br>O valor padrão para **marcas** é **DAT** (dados, atributos e carimbos de data / hora).|
|/dcopy:\<copyflags\>|Define o que será copiado para diretórios. O padrão é DA. As opções são D = dados, um = atributos e T = carimbos de hora.|
|/sec|Copia os arquivos com segurança (equivalente a **/Copy**).|
|/copyall|Copia todas as informações de arquivo (equivalente a **DATSOU**).|
|/nocopy|Não copia nenhuma informação do arquivo (útil com **/limpar**).|
|/secfix|Correções de segurança de arquivos em todos os arquivos, mesmo ignorada aqueles.|
|/timfix|Correções de tempos de arquivos em todos os arquivos, mesmo ignorada aqueles.|
|/Purge|Exclui arquivos de destino e diretórios que não existem mais na fonte. Para obter mais informações, consulte [comentários](#remarks).|
|/mir|Espelha a uma árvore de diretório (equivalente a **/e** plus **/limpar**). Para obter mais informações, consulte [comentários](#remarks).|
|/mov|Move os arquivos e exclui-los da fonte depois que eles são copiados.|
|/move|Move os arquivos e diretórios e exclui-los da fonte depois que eles são copiados.|
|/a+:[RASHCNET]|Adiciona os atributos especificados para arquivos copiados.|
|/a-:[RASHCNET]|Remove os atributos especificados dos arquivos copiados.|
|/create|Cria uma árvore de diretório e somente os arquivos de comprimento zero.|
|/fat|Cria arquivos de destino usando nomes de comprimento de caracteres de arquivo FAT 8.3 somente.|
|/256|Desativa o suporte para caminhos muito longos (mais de 256 caracteres).|
|/mon:\<N>|Monitora a origem e é executada novamente quando mais de *N* forem detectadas alterações.|
|/MOT:\<M >|Monitora o código-fonte e é executado novamente em *M* minutos, se forem detectadas alterações.|
|/MT[:N]|Cria cópias multi-threaded com *N* threads. *N* deve ser um inteiro entre 1 e 128. O valor padrão para *N* é 8.</br>O **/MT** parâmetro não pode ser usado com o **/IPG** e **/EFSRAW** parâmetros.</br>Redirecionamento de saída usando **/LOG** opção para melhorar o desempenho.</br>Observação: O parâmetro /MT se aplica ao Windows Server 2008 R2 e Windows 7.|
|/rh:hhmm-hhmm|Especifica os tempos de execução quando novas cópias podem ser iniciadas.|
|/pf|Tempos de execução de verificações em uma base por arquivo (não por passar).|
|/ipg:n|Especifica a lacuna entre pacotes para liberar a largura de banda em linhas de desaceleração.|
|/sl|Não siga os links simbólicos e em vez disso, crie uma cópia do link.|

> [!IMPORTANT]
> Ao usar o **/SECFIX** copiar opção, especifique o tipo de informações de segurança que você deseja copiar também usando uma destas opções adicionais de cópia:
>- **/COPYALL**
>- **/COPY:O**
>- **/COPY:S**
>- **/COPY:U**
>- **/SEC**

### <a name="file-selection-options"></a>Opções de seleção de arquivo

|Opção|Descrição|
|------|-----------|
|/a|Copia somente arquivos para o qual o **arquivamento** atributo é definido.|
|/m|Copia somente arquivos para o qual o **arquivamento** atributo é definido e redefine o **arquivamento** atributo.|
|/ia:[RASHCNETO]|Inclui somente os arquivos para que qualquer um dos atributos especificados são definidos.|
|/xa:[RASHCNETO]|Exclui arquivos para que qualquer um dos atributos especificados são definidos.|
|/xf \<FileName>[ ...]|Exclui os arquivos que correspondem aos nomes especificados ou caminhos. Observe que *FileName* pode incluir caracteres curinga (**&#42;** e **?** ).|
|/xd \<Directory>[ ...]|Exclui os diretórios que correspondem aos nomes especificados e caminhos.|
|/xc|Exclui os arquivos alterados.|
|/xn|Exclui arquivos mais recentes.|
|/xo|Exclui arquivos antigos.|
|/xx|Exclui os diretórios e arquivos extras.|
|/xl|Exclui os diretórios e arquivos "solitárias".|
|/is|Inclui os mesmos arquivos.|
|/it|Inclui os arquivos "ajustada".|
|/max:\<N>|Especifica o tamanho máximo do arquivo (para excluir arquivos maiores do que *N* bytes).|
|/min:\<N>|Especifica o tamanho mínimo do arquivo (para excluir arquivos menores do que *N* bytes).|
|/maxage:\<N>|Especifica a idade do arquivo máximo (para excluir arquivos mais antigos do que *N* dias ou data).|
|/minage:\<N>|Especifica a idade mínima do arquivo (excluir arquivos mais recente do que *N* dias ou data).|
|/maxlad:\<N>|Especifica o máximo de data do último acesso (exclui arquivos não utilizados desde *N*).|
|/minlad:\<N>|Especifica o mínimo de data do último acesso (exclui arquivos usados desde *N*) se *N* é menor que 1900 *N* Especifica o número de dias. Caso contrário, *N* Especifica uma data no formato AAAAMMDD.|
|/xj|Exclui os pontos de junção, que normalmente são incluídos por padrão.|
|/fft|Pressupõe que o arquivo FAT vezes (precisão de dois segundos).|
|/dst|Compensa diferenças de horário de verão de uma hora.|
|/xjd|Exclui os pontos de junção para diretórios.|
|/xjf|Exclui os pontos de junção para arquivos.|

### <a name="retry-options"></a>As opções de repetição

|Opção|Descrição|
|------|-----------|
|/r:\<N>|Especifica o número de repetições em cópias com falha. O valor padrão de *N* é 1.000.000 (um milhão de repetições).|
|/w:\<N>|Especifica o tempo de espera entre as tentativas, em segundos. O valor padrão de *N* é 30 (30 segundos de tempo de espera).|
|/reg|Salva os valores especificados na **/r** e **/w** opções como configurações padrão no registro.|
|/tbd|Especifica que o sistema aguardará para nomes de compartilhamento seja definido (erro 67 de repetição).|

### <a name="logging-options"></a>Opções de log

|Opção|Descrição|
|------|-----------|
|/l|Especifica que os arquivos devem ser listadas apenas (e não copiado, excluído, ou carimbo de data / hora).|
|/x|Relata todos os arquivos extras, não apenas aqueles que estão selecionados.|
|/v|Produz a saída detalhada e mostra todos os arquivos ignorados.|
|/ts|Inclui carimbos de hora do arquivo de origem na saída.|
|/fp|Inclui os nomes de caminho completo dos arquivos na saída.|
|/bytes|Imprime os tamanhos, como bytes.|
|/ns|Especifica que os tamanhos de arquivo não devem ser registrados.|
|/nc|Especifica que as classes de arquivo não devem ser registrados.|
|/nfl|Especifica que os nomes de arquivo não são devem ser registrados.|
|/ndl|Especifica que os nomes de diretório não são devem ser registrados.|
|/np|Especifica que o progresso da operação de cópia (o número de arquivos ou diretórios copiados até o momento) não será exibido.|
|/eta|Mostra o tempo estimado de chegada (ETA) os arquivos copiados.|
|/log:\<LogFile>|Grava a saída de status para o arquivo de log (substitui o arquivo de log existente).|
|/log+:\<LogFile>|Grava a saída de status para o arquivo de log (anexa a saída ao arquivo de log existente).|
|/unicode|Exibe a saída de status como texto Unicode.|
|/unilog:\<LogFile>|Grava o status de saída para o arquivo de log como texto Unicode (substitui o arquivo de log existente).|
|/unilog+:\<LogFile>|Grava o status de saída para o arquivo de log como texto Unicode (anexa a saída ao arquivo de log existente).|
|/tee|Grava a saída de status para a janela de console, bem como para o arquivo de log.|
|/njh|Especifica que não há nenhum cabeçalho de trabalho.|
|/njs|Especifica que não há nenhum resumo do trabalho.|

### <a name="job-options"></a>Opções de trabalho

|Opção|Descrição|
|------|-----------|
|/job:\<JobName>|Especifica que os parâmetros devem ser derivado do arquivo de trabalho nomeado.|
|/save:\<JobName>|Especifica que os parâmetros devem ser salvas no arquivo de trabalho nomeado.|
|/quit|Sai depois de linha de comando de processamento (para exibir parâmetros).|
|/nosd|Indica que nenhum diretório de origem é especificado.|
|/nodd|Indica que nenhum diretório de destino é especificado.|
|/if|Inclui os arquivos especificados.|

### <a name="exit-return-codes"></a>Códigos de saída (retorno)
Valor | Descrição
-- | --
0 | Não há arquivos foram copiados. Não foi encontrada nenhuma falha.  Não há arquivos eram incompatíveis. Os arquivos já existirem no diretório de destino; Portanto, a operação de cópia foi ignorada.
1 | Todos os arquivos foram copiados com êxito.
2 | Há alguns arquivos adicionais no diretório de destino que não estão presentes no diretório de origem. Não há arquivos foram copiados.
3 | Alguns arquivos foram copiados. Arquivos adicionais estavam presentes. Não foi encontrada nenhuma falha.
5 | Alguns arquivos foram copiados. Alguns arquivos foram incompatíveis. Não foi encontrada nenhuma falha.
6 | Arquivos adicionais e arquivos incompatíveis existem. Não há arquivos foram copiados e não há falhas foram encontradas. Isso significa que os arquivos já existirem no diretório de destino.
7 | Os arquivos foram copiados, uma incompatibilidade de arquivo estava presente e arquivos adicionais estavam presentes.
8 | Vários arquivos não tiver copiado.

> [!NOTE]
> Qualquer valor maior que 8 indica que houve pelo menos uma falha durante a operação de cópia.

### <a name="remarks"></a>Comentários

-   O **/mir** opção é equivalente ao **/e** plus **/limpar** opções com uma pequena diferença no comportamento:  
    -   Com o **/e** plus **/limpar** opções, se o diretório de destino existe, as configurações de segurança do diretório de destino não são substituídas.
    -   Com o **/mir** opção, se existir o diretório de destino, as configurações de segurança do diretório de destino são substituídas.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
