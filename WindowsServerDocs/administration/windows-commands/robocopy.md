---
title: robocopy
description: Saiba como usar o comando Robocopy no Windows e no Windows Server para copiar arquivos
ms.topic: article
ms.assetid: d4c6e8e9-fcb3-4a4a-9d04-2d8c367b6354
author: jasongerend
ms.author: jgerend
manager: lizapo
ms.date: 06/07/2020
ms.openlocfilehash: fdf7eda5a17dccba0f43cca91cae122872dd5235
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883497"
---
# <a name="robocopy"></a>robocopy

Copia os dados do arquivo.

## <a name="syntax"></a>Sintaxe

```
robocopy <Source> <Destination> [<File>[ ...]] [<Options>]
```

Por exemplo, para copiar um arquivo chamado *yearly-Report. mov* de c:\Reports para um compartilhamento de arquivos (* \\ marketing\videos*) enquanto habilita o multithreading para um melhor desempenho (com o parâmetro/MT) e a capacidade de reiniciar a transferência caso ela seja interrompida (com o parâmetro/z), você usaria a seguinte sintaxe:

```dos
robocopy C:\reports '\\marketing\videos' yearly-report.mov /mt /z
```

### <a name="parameters"></a>Parâmetros

|   Parâmetro    |                                                                                            Descrição                                                                                           |
|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<Source>    |                                                                            Especifica o caminho para o diretório de origem.                                                                           |
| \<Destination> |                                                                          Especifica o caminho para o diretório de destino.                                                                        |
|    \<File>     | Especifica o arquivo ou os arquivos a serem copiados. Você pode usar caracteres curinga (**&#42;** ou **?**), se desejar. Se o parâmetro **File** não for especificado, ** \* . \* ** será usado como o valor padrão. |
|   \<Options>   |                                                                    Especifica as opções a serem usadas com o comando **Robocopy** .                                                                   |

### <a name="copy-options"></a>Opções de cópia

|Opção|Descrição|
|------|-----------|
|/s|Copia subdiretórios. Observe que essa opção exclui diretórios vazios.|
|/e|Copia subdiretórios. Observe que essa opção inclui diretórios vazios. Para obter mais informações, consulte [comentários](#remarks).|
|nível\<N>|Copia apenas os *N* níveis superiores da árvore do diretório de origem.|
|/z|Copia arquivos no modo reiniciável.|
|/b|Copia arquivos no modo de Backup.|
|/zb|Usa o modo reiniciável. Se o acesso for negado, esta opção usa o modo Backup.|
|/efsraw|Copia todos os arquivos criptografados no modo EFS RAW.|
|/Copy\<CopyFlags>|Especifica as propriedades de arquivo a serem copiadas. Estes são os valores válidos para esta opção:</br>Dados **D**</br>**Um** atributo</br>Carimbos de hora **T**</br>Lista de controle de acesso (ACL **) NTFS**</br>Informações sobre **O** proprietário</br>Informações de auditoria **U**</br>O valor padrão para **CopyFlags** é **dat** (data, atributos e carimbos de data/hora).|
|/dcopy:\<copyflags\>|Define o que copiar para diretórios. O padrão é DA. As opções são D = dados, A = atributos e T = carimbos de data/hora.|
|/s|Copia arquivos com segurança (equivalente a **/Copy: DATs**).|
|/copyall|Copia todas as informações do arquivo (equivalente a **/Copy: DATSOU**).|
|/nocopy|Não copia nenhuma informação de arquivo (útil com **/Purge**).|
|/secfix|Corrige a segurança de arquivos em todos os arquivos, até mesmo ignorados.|
|/timfix|Corrige os tempos de arquivo em todos os arquivos, até mesmo ignorados.|
|/purge|Exclui os arquivos de destino e diretórios que não existem mais na origem. Para obter mais informações, consulte [comentários](#remarks).|
|/mir|Espelha uma árvore de diretório (equivalente a **/e** mais **/Purge**). Para obter mais informações, consulte [comentários](#remarks).|
|/mov|Move arquivos e os exclui da origem depois que eles são copiados.|
|/move|Move arquivos e diretórios e os exclui da origem depois que eles são copiados.|
|/a +: [RASHCNET]|Adiciona os atributos especificados aos arquivos copiados.|
|/a-:[RASHCNET]|Remove os atributos especificados dos arquivos copiados.|
|/Create|Cria apenas uma árvore de diretórios e arquivos de comprimento zero.|
|/fat|Cria arquivos de destino usando somente nomes de arquivo FAT de comprimento de caractere 8,3.|
|/256|Desativa o suporte para caminhos muito longos (mais de 256 caracteres).|
|Mon\<N>|Monitora a origem e executa novamente quando mais de *N* alterações são detectadas.|
|/mot:\<M>|Monitora a origem e executa novamente em *M* minutos se forem detectadas alterações.|
|/MT[:N]|Cria cópias multi-threaded com *N* threads. *N* deve ser um inteiro entre 1 e 128. O valor padrão para *N* é 8.</br>O parâmetro **/MT** não pode ser usado com os parâmetros **/IPG** e **/EFSRAW** .</br>Redirecione a saída usando a opção **/log** para melhorar o desempenho.</br>Observação: o parâmetro/MT se aplica ao Windows Server 2008 R2 e ao Windows 7.|
|/RH: hhmm-hhmm|Especifica os tempos de execução quando novas cópias podem ser iniciadas.|
|/PF|Verifica os tempos de execução em uma base por arquivo (não por passagem).|
|/IPG: n|Especifica a lacuna entre pacotes para a largura de banda livre em linhas lentas.|
|/sl|Não siga links simbólicos e, em vez disso, crie uma cópia do link.|

> [!IMPORTANT]
> Ao usar a opção de cópia **/secfix** , especifique o tipo de informações de segurança que você deseja copiar usando também uma destas opções de cópia adicionais:
>- **/COPYALL**
>- **/COPY: O**
>- **/COPY: S**
>- **/COPY: U**
>- **/S**

### <a name="file-selection-options"></a>Opções de seleção de arquivo

|Opção|Descrição|
|------|-----------|
|/a|Copia somente os arquivos para os quais o atributo de **arquivo** está definido.|
|/m|Copia somente os arquivos para os quais o atributo de **arquivo** está definido e redefine o atributo de **arquivo morto** .|
|/ia: [RASHCNETO]|Inclui somente os arquivos para os quais qualquer um dos atributos especificados são definidos.|
|/xa:[RASHCNETO]|Exclui os arquivos para os quais qualquer um dos atributos especificados são definidos.|
|/XF \<FileName> [...]|Exclui os arquivos que correspondem aos nomes ou caminhos especificados. Observe que *filename* pode incluir caracteres curinga (**&#42;** e **?**).|
|/xD \<Directory> [...]|Exclui os diretórios que correspondem aos nomes e caminhos especificados.|
|/xc|Exclui arquivos alterados.|
|/xn|Exclui arquivos mais recentes.|
|/xo|Exclui arquivos mais antigos.|
|/xx|Exclui arquivos e diretórios adicionais.|
|/xl|Exclui arquivos e diretórios "próprios".|
|/is|Inclui os mesmos arquivos.|
|/It|Inclui arquivos "ajustados".|
|maximizar\<N>|Especifica o tamanho máximo do arquivo (para excluir arquivos maiores que *N* bytes).|
|min\<N>|Especifica o tamanho mínimo do arquivo (para excluir arquivos menores que *N* bytes).|
|período\<N>|Especifica a idade máxima do arquivo (para excluir arquivos com mais de *N* dias ou data).|
|/minage:\<N>|Especifica a idade mínima do arquivo (excluir arquivos mais recentes que *N* dias ou data).|
|/maxlad:\<N>|Especifica a data máxima do último acesso (exclui arquivos não usados desde *N*).|
|/minlad:\<N>|Especifica a data mínima do último acesso (exclui arquivos usados desde *n*) se *N* for menor que 1900, *N* especifica o número de dias. Caso contrário, *N* especifica uma data no formato AAAAMMDD.|
|/xj|Exclui pontos de junção, que normalmente são incluídos por padrão.|
|/fft|Pressupõe tempos de arquivo FAT (precisão de dois segundos).|
|/dst|Compensa as diferenças de hora de Verão de uma hora.|
|/xjd|Exclui pontos de junção para diretórios.|
|/xjf|Exclui pontos de junção para arquivos.|

### <a name="retry-options"></a>Opções de repetição

|Opção|Descrição|
|------|-----------|
|/r\<N>|Especifica o número de repetições em cópias com falha. O valor padrão de *N* é 1 milhão (1 milhão tentativas).|
|/w\<N>|Especifica o tempo de espera entre as tentativas, em segundos. O valor padrão de *N* é 30 (tempo de espera de 30 segundos).|
|/reg|Salva os valores especificados nas opções **/r** e **/w** como configurações padrão no registro.|
|/tbd|Especifica que o sistema aguardará a definição de nomes de compartilhamento (repita o erro 67).|

### <a name="logging-options"></a>Opções de log

|Opção|Descrição|
|------|-----------|
|/l|Especifica que os arquivos devem ser listados apenas (e não copiados, excluídos ou com carimbo de data/hora).|
|/x|Relata todos os arquivos extras, não apenas aqueles que estão selecionados.|
|/v|Produz a saída detalhada e mostra todos os arquivos ignorados.|
|/ts|Inclui carimbos de data/hora do arquivo de origem na saída.|
|/fp|Inclui os nomes de caminho completos dos arquivos na saída.|
|/bytes|Imprime tamanhos, como bytes.|
|/ns|Especifica que os tamanhos de arquivo não devem ser registrados em log.|
|/nc|Especifica que as classes de arquivo não devem ser registradas em log.|
|/nfl|Especifica que os nomes de arquivo não devem ser registrados.|
|/ndl|Especifica que os nomes de diretório não devem ser registrados.|
|/np|Especifica que o progresso da operação de cópia (o número de arquivos ou diretórios copiados até o momento) não será exibido.|
|/eta|Mostra o tempo estimado de chegada (ETA) dos arquivos copiados.|
|/log\<LogFile>|Grava a saída de status no arquivo de log (substitui o arquivo de log existente).|
|/log +:\<LogFile>|Grava a saída de status no arquivo de log (acrescenta a saída ao arquivo de log existente).|
|/unicode|Exibe a saída de status como texto Unicode.|
|/unilog:\<LogFile>|Grava a saída de status no arquivo de log como texto Unicode (Substitui o arquivo de log existente).|
|/UNILOG +:\<LogFile>|Grava a saída de status no arquivo de log como texto Unicode (acrescenta a saída ao arquivo de log existente).|
|/tee|Grava a saída de status na janela do console, bem como no arquivo de log.|
|/njh|Especifica que não há nenhum cabeçalho de trabalho.|
|/njs|Especifica que não há nenhum resumo do trabalho.|

### <a name="job-options"></a>Opções de trabalho

|Opção|Descrição|
|------|-----------|
|/minuto\<JobName>|Especifica que os parâmetros devem ser derivados do arquivo de trabalho nomeado.|
|/Save\<JobName>|Especifica que os parâmetros devem ser salvos no arquivo de trabalho nomeado.|
|/quit|Encerra após o processamento da linha de comando (para exibir parâmetros).|
|/nosd|Indica que nenhum diretório de origem foi especificado.|
|/nodd|Indica que nenhum diretório de destino foi especificado.|
|/If|Inclui os arquivos especificados.|

### <a name="exit-return-codes"></a>Sair (retornar) códigos

Valor | Descrição
-- | --
0 | Nenhum arquivo foi copiado. Nenhuma falha foi encontrada.  Nenhum arquivo não correspondeu. Os arquivos já existem no diretório de destino; Portanto, a operação de cópia foi ignorada.
1 | Todos os arquivos foram copiados com êxito.
2 | Há alguns arquivos adicionais no diretório de destino que não estão presentes no diretório de origem. Nenhum arquivo foi copiado.
3 | Alguns arquivos foram copiados. Arquivos adicionais estavam presentes. Nenhuma falha foi encontrada.
5 | Alguns arquivos foram copiados. Alguns arquivos não correspondem. Nenhuma falha foi encontrada.
6 | Arquivos adicionais e arquivos incompatíveis existem. Nenhum arquivo foi copiado e nenhuma falha foi encontrada. Isso significa que os arquivos já existem no diretório de destino.
7 | Os arquivos foram copiados, uma incompatibilidade de arquivo estava presente e arquivos adicionais estavam presentes.
8 | Vários arquivos não foram copiados.

> [!NOTE]
> Qualquer valor maior que 8 indica que houve pelo menos uma falha durante a operação de cópia.

### <a name="remarks"></a>Comentários

-   A opção **/Mir** é equivalente às opções **/e** Plus **/Purge** com uma pequena diferença no comportamento:
    -   Com as opções **/e** Plus **/Purge** , se o diretório de destino existir, as configurações de segurança do diretório de destino não serão substituídas.
    -   Com a opção **/Mir** , se o diretório de destino existir, as configurações de segurança do diretório de destino serão substituídas.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
