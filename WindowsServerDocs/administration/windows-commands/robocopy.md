---
title: robocopy
description: Artigo de referência para o comando Robocopy, que copia dados de arquivo de um local para outro.
ms.topic: reference
ms.assetid: d4c6e8e9-fcb3-4a4a-9d04-2d8c367b6354
author: jasongerend
ms.author: jgerend
manager: lizapo
ms.date: 06/07/2020
ms.openlocfilehash: 48d9d564c7badf8ea34c77ce7004d0ce642b78cb
ms.sourcegitcommit: fe356f95188b7ce8e719765f44c0789c065832fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89057578"
---
# <a name="robocopy"></a>robocopy

Copia dados de arquivo de um local para outro.

## <a name="syntax"></a>Syntax

```
robocopy <source> <destination> [<file>[ ...]] [<options>]
```

Por exemplo, para copiar um arquivo chamado *yearly-Report. mov* de *c:\Reports* para um compartilhamento de arquivos * \\ marketing\videos* enquanto habilita o multithreading para um melhor desempenho (com o parâmetro **/MT** ) e a capacidade de reiniciar a transferência caso ela seja interrompida (com o parâmetro **/z** ), digite:

```dos
robocopy c:\reports '\\marketing\videos' yearly-report.mov /mt /z
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<source>` | Especifica o caminho para o diretório de origem. |
| `<destination>` | Especifica o caminho para o diretório de destino. |
| `<file>` | Especifica o arquivo ou os arquivos a serem copiados. Há suporte para caracteres curinga (**&#42;** ou **?**). Se você não especificar esse parâmetro, `*.` será usado como o valor padrão. |
| `<options>` | Especifica as opções a serem usadas com o comando **Robocopy** , incluindo opções de **cópia**, **arquivo**, **repetição**, **log**e **trabalho** . |

#### <a name="copy-options"></a>Opções de cópia

| Opção | Descrição |
|--|--|
| /s | Copia subdiretórios. Essa opção exclui automaticamente os diretórios vazios. |
| /e | Copia subdiretórios. Essa opção inclui automaticamente diretórios vazios. |
| nível`<n>` | Copia apenas os *n* níveis superiores da árvore do diretório de origem. |
| /z | Copia arquivos no modo reiniciável. |
| /b | Copia os arquivos no modo de Backup. |
| /zb | Usa o modo reiniciável. Se o acesso for negado, esta opção usa o modo Backup. |
| /efsraw | Copia todos os arquivos criptografados no modo EFS RAW. |
| /Copy`<copyflags>` | Especifica quais propriedades de arquivo copiar. Os valores válidos para essa opção são:<ul><li>**D** -dados</li><li>**A** -atributos</li><li>Carimbos de hora **T**</li><li>**S** -lista de controle de acesso (ACL) NTFS</li><li>**O** -informações do proprietário</li><li>**U** -informações de auditoria</li></ul>O valor padrão dessa opção é **dat** (data, atributos e carimbos de data/hora). |
| /dcopy:`<copyflags>`| Especifica o que copiar em diretórios. Os valores válidos para essa opção são:<ul><li>**D** -dados</li><li>**A** -atributos</li><li>Carimbos de hora **T**</li></ul>O valor padrão dessa opção é **da** (data e atributos). |
| /s | Copia arquivos com segurança (equivalente a **/Copy: DATs**). |
| /copyall | Copia todas as informações do arquivo (equivalente a **/Copy: DATSOU**). |
| /nocopy | Não copia nenhuma informação de arquivo (útil com **/Purge**). |
| /secfix | Corrige a segurança de arquivos em todos os arquivos, até mesmo ignorados. |
| /timfix | Corrige os tempos de arquivo em todos os arquivos, até mesmo ignorados. |
| /purge | Exclui os arquivos de destino e diretórios que não existem mais na origem. Usar essa opção com a opção **/e** e um diretório de destino permite que as configurações de segurança do diretório de destino não sejam substituídas. |
| /mir | Espelha uma árvore de diretório (equivalente a **/e** mais **/Purge**). Usando essa opção com a opção **/e** e um diretório de destino, o substitui as configurações de segurança do diretório de destino. |
| /mov | Move arquivos e os exclui da origem depois que eles são copiados. |
| /move | Move arquivos e diretórios e os exclui da origem depois que eles são copiados. |
| /a +: [RASHCNET] | Adiciona os atributos especificados aos arquivos copiados. |
| /a-:[RASHCNET] | Remove os atributos especificados dos arquivos copiados. |
| /Create | Cria apenas uma árvore de diretórios e arquivos de comprimento zero. |
| /fat | Cria arquivos de destino usando somente nomes de arquivo FAT de comprimento de caractere 8,3. |
| /256 | Desativa o suporte para caminhos com mais de 256 caracteres. |
| Mon`<n>` | Monitora a origem e executa novamente quando mais de *n* alterações são detectadas. |
| /mot:`<m>` | Monitora a origem e executa novamente em *m* minutos, se forem detectadas alterações. |
| /MT`[:n]` | Cria cópias multi-threaded com *n* threads. *n* deve ser um inteiro entre 1 e 128. O valor padrão para *n* é 8. Para obter melhor desempenho, redirecione a saída usando a opção **/log** .<p>O parâmetro **/MT** não pode ser usado com os parâmetros **/IPG** e **/EFSRAW** . |
| /RH: hhmm-hhmm | Especifica os tempos de execução quando novas cópias podem ser iniciadas. |
| /PF | Verifica os tempos de execução em uma base por arquivo (não por passagem). |
| /IPG: n | Especifica a lacuna entre pacotes para a largura de banda livre em linhas lentas. |
| /sl | Não siga links simbólicos e, em vez disso, crie uma cópia do link. |

> [!IMPORTANT]
> Ao usar a opção de cópia **/secfix** , especifique o tipo de informações de segurança que deseja copiar, usando uma destas opções de cópia adicionais:
>
>- **/copyall**
>- **/Copy: o**
>- **/Copy: s**
>- **/Copy: u**
>- **/sec**

#### <a name="file-selection-options"></a>Opções de seleção de arquivo

| Opção | Descrição |
|--|--|
| /a | Copia somente os arquivos para os quais o atributo de **arquivo** está definido. |
| /m | Copia somente os arquivos para os quais o atributo de **arquivo** está definido e redefine o atributo de **arquivo morto** . |
| /ia`[RASHCNETO]` | Inclui somente os arquivos para os quais qualquer um dos atributos especificados são definidos. |
| XA`[RASHCNETO]` | Exclui os arquivos para os quais qualquer um dos atributos especificados são definidos. |
| /xf `<filename>[ ...]` | Exclui os arquivos que correspondem aos nomes ou caminhos especificados. Há suporte para caracteres curinga (**&#42;** e **?**). |
| /xd `<directory>[ ...]` | Exclui os diretórios que correspondem aos nomes e caminhos especificados. |
| /xc | Exclui arquivos alterados. |
| /xn | Exclui arquivos mais recentes. |
| /xo | Exclui arquivos mais antigos. |
| /xx | Exclui arquivos e diretórios adicionais. |
| /xl | Exclui arquivos e diretórios "próprios". |
| /is | Inclui os mesmos arquivos. |
| /It | Inclui arquivos modificados. |
| maximizar`<n>` | Especifica o tamanho máximo do arquivo (para excluir arquivos maiores que *n* bytes). |
| min`<n>` | Especifica o tamanho mínimo do arquivo (para excluir arquivos menores que *n* bytes). |
| período`<n>` | Especifica a idade máxima do arquivo (para excluir arquivos com mais de *n* dias ou data). |
| /minage:`<n>` | Especifica a idade mínima do arquivo (excluir arquivos mais recentes que *n* dias ou data). |
| /maxlad:`<n>` | Especifica a data máxima do último acesso (exclui arquivos não usados desde *n*). |
| /minlad:`<n>` | Especifica a data mínima do último acesso (exclui arquivos usados desde *n*) se *n* for menor que 1900, *n* especifica o número de dias. Caso contrário, *n* especifica uma data no formato AAAAMMDD. |
| /xj | Exclui pontos de junção, que normalmente são incluídos por padrão. |
| /fft | Pressupõe tempos de arquivo FAT (precisão de dois segundos). |
| /dst | Compensa as diferenças de hora de Verão de uma hora. |
| /xjd | Exclui pontos de junção para diretórios. |
| /xjf | Exclui pontos de junção para arquivos. |

#### <a name="retry-options"></a>Opções de repetição

| Opção | Descrição |
|--|--|
| /r:`<n>` | Especifica o número de repetições em cópias com falha. O valor padrão de *n* é 1 milhão (1 milhão tentativas). |
| /w:`<n>` | Especifica o tempo de espera entre as tentativas, em segundos. O valor padrão de *n* é 30 (tempo de espera de 30 segundos). |
| /reg | Salva os valores especificados nas opções **/r** e **/w** como configurações padrão no registro. |
| /tbd | Especifica que o sistema aguardará a definição de nomes de compartilhamento (repita o erro 67). |

#### <a name="logging-options"></a>Opções de log

| Opção | Descrição |
|--|--|
| /l | Especifica que os arquivos devem ser listados apenas (e não copiados, excluídos ou com carimbo de data/hora). |
| /x | Relata todos os arquivos extras, não apenas aqueles que estão selecionados. |
| /v | Produz a saída detalhada e mostra todos os arquivos ignorados. |
| /ts | Inclui carimbos de data/hora do arquivo de origem na saída. |
| /fp | Inclui os nomes de caminho completos dos arquivos na saída. |
| /bytes | Imprime tamanhos, como bytes. |
| /ns | Especifica que os tamanhos de arquivo não devem ser registrados em log. |
| /nc | Especifica que as classes de arquivo não devem ser registradas em log. |
| /nfl | Especifica que os nomes de arquivo não devem ser registrados. |
| /ndl | Especifica que os nomes de diretório não devem ser registrados. |
| /np | Especifica que o progresso da operação de cópia (o número de arquivos ou diretórios copiados até o momento) não será exibido. |
| /eta | Mostra o tempo estimado de chegada (ETA) dos arquivos copiados. |
| /log`<logfile>` | Grava a saída de status no arquivo de log (substitui o arquivo de log existente). |
| /log +:`<logfile>` | Grava a saída de status no arquivo de log (acrescenta a saída ao arquivo de log existente). |
| /unicode | Exibe a saída de status como texto Unicode. |
| /unilog:`<logfile>` | Grava a saída de status no arquivo de log como texto Unicode (Substitui o arquivo de log existente). |
| /UNILOG +:`<logfile>` | Grava a saída de status no arquivo de log como texto Unicode (acrescenta a saída ao arquivo de log existente). |
| /tee | Grava a saída de status na janela do console, bem como no arquivo de log. |
| /njh | Especifica que não há nenhum cabeçalho de trabalho. |
| /njs | Especifica que não há nenhum resumo do trabalho. |

#### <a name="job-options"></a>Opções de trabalho

| Opção | Descrição |
|--|--|
| /minuto`<jobname>` | Especifica que os parâmetros devem ser derivados do arquivo de trabalho nomeado. |
| /Save`<jobname>` | Especifica que os parâmetros devem ser salvos no arquivo de trabalho nomeado. |
| /quit | Encerra após o processamento da linha de comando (para exibir parâmetros). |
| /nosd | Indica que nenhum diretório de origem foi especificado. |
| /nodd | Indica que nenhum diretório de destino foi especificado. |
| /If | Inclui os arquivos especificados. |

### <a name="exit-return-codes"></a>Sair (retornar) códigos

| Valor | Descrição |
|--|--|
| 0 | Nenhum arquivo foi copiado. Nenhuma falha foi encontrada. Nenhum arquivo não correspondeu. Os arquivos já existem no diretório de destino; Portanto, a operação de cópia foi ignorada. |
| 1 | Todos os arquivos foram copiados com êxito. |
| 2 | Há alguns arquivos adicionais no diretório de destino que não estão presentes no diretório de origem. Nenhum arquivo foi copiado. |
| 3 | Alguns arquivos foram copiados. Arquivos adicionais estavam presentes. Nenhuma falha foi encontrada. |
| 5 | Alguns arquivos foram copiados. Alguns arquivos não correspondem. Nenhuma falha foi encontrada. |
| 6 | Arquivos adicionais e arquivos incompatíveis existem. Nenhum arquivo foi copiado e nenhuma falha foi encontrada. Isso significa que os arquivos já existem no diretório de destino. |
| 7 | Os arquivos foram copiados, uma incompatibilidade de arquivo estava presente e arquivos adicionais estavam presentes. |
| 8 | Vários arquivos não foram copiados. |

> [!NOTE]
> Qualquer valor maior que **8** indica que houve pelo menos uma falha durante a operação de cópia.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
