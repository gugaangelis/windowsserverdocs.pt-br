---
title: chkdsk
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62912a3c-d2cc-4ef6-9679-43709a286035
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: ed9693749cb07c76c3c845844f3556e07a39f009
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811308"
---
# <a name="chkdsk"></a>chkdsk

Verifica o sistema de arquivos e metadados de sistema de arquivos de um volume para erros lógicos e físicos. Se usado sem parâmetros, **chkdsk** exibe apenas o status do volume e não corrige os erros. Se usado com o **/f**, **/r**, **/x**, ou **/b** parâmetros, ele corrigirá erros no volume.

> [!IMPORTANT]
> Associação local **administradores** grupo, ou equivalente, é o mínimo necessário para executar **chkdsk**. Para abrir uma janela de prompt de comando como administrador, clique com botão direito **prompt de comando** no menu Iniciar e clique **executar como administrador**.

> [!IMPORTANT]
> Interromper **chkdsk** não é recomendado. No entanto, cancelando ou interromper **chkdsk** não deve deixar o volume mais corrompido que era antes **chkdsk** foi executado. Executar novamente **chkdsk** verifica e corrige qualquer corrompimento restante no volume.

> [!IMPORTANT]
> **Observação:** CHKDSK pode ser usado somente para discos locais. O comando não pode ser usado com uma letra de unidade local que foi redirecionada pela rede.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
chkdsk [<Volume>[[<Path>]<FileName>]] [/f] [/v] [/r] [/x] [/i] [/c] [/l[:<Size>]] [/b]  
```

## <a name="parameters"></a>Parâmetros

|      Parâmetro      |                                                                                                                      Descrição                                                                                                                       |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      \<Volume>      |                                                                                     Especifica a letra da unidade (seguida por dois-pontos), ponto de montagem ou nome do volume.                                                                                     |
| [\<Path>]<FileName> | Use com a tabela de alocação de arquivos (FAT) e apenas FAT32. Especifica o local e o nome de um arquivo ou conjunto de arquivos que você deseja **chkdsk** para verificar se há fragmentação. Você pode usar o **?** e **&#42;** caracteres curinga para especificar vários arquivos. |
|         /f          |                             Correções de erros no disco. O disco deve ser bloqueado. Se **chkdsk** não é possível bloquear a unidade, uma mensagem é exibida perguntando se você deseja verificar a unidade na próxima vez que você reiniciar o computador.                             |
|         /v          |                                                                                       Exibe o nome de cada arquivo em cada diretório como o disco é verificado.                                                                                        |
|         /r          |                                   Localiza os setores defeituosos e recupera informações legíveis. O disco deve ser bloqueado. **/r** inclui a funcionalidade do **/f**, com a análise adicional de erros de disco físico.                                   |
|         /x          |                                                  Força o volume ser desmontado primeiro, se necessário. Todos os identificadores abertos para a unidade serão invalidados. **/x** também inclui a funcionalidade do **/f**.                                                   |
|         /i          |                                                           Somente para uso com o NTFS. Executa uma verificação menos rígida de entradas de índice, o que reduz a quantidade de tempo necessário para executar **chkdsk**.                                                            |
|         /c          |                                                          Somente para uso com o NTFS. Não verifica os ciclos de dentro da estrutura de pasta, o que reduz a quantidade de tempo necessário para executar **chkdsk**.                                                           |
|    /l[:\<Size>]     |                                                         Somente para uso com o NTFS. Altera o tamanho do arquivo de log para o tamanho que você digita. Se você omitir o parâmetro de tamanho **/l** exibe o tamanho atual.                                                          |
|         /b          |           Apenas NTFS: Limpa a lista de clusters inválidos no volume e examina todos os clusters alocados e livres de erros. **/ b** inclui a funcionalidade do **/r**. Use este parâmetro depois de um volume para uma nova unidade de disco rígido de geração de imagens.            |
|         /?          |                                                                                                          Exibe a ajuda no prompt de comando.                                                                                                          |

## <a name="remarks"></a>Comentários

- Ignorar verificações de volume

  O **/i** ou **/c** switch reduz a quantidade de tempo necessário para executar **chkdsk** ignorando determinados volume verifica.
- Verificando uma unidade bloqueada na reinicialização

  Se você quiser **chkdsk** para corrigir erros de disco, você não pode ter arquivos abertos na unidade. Se os arquivos estiverem abertos, a seguinte mensagem de erro será exibida:

  ```
  Chkdsk cannot run because the volume is in use by another process. Would you like to schedule this volume to be checked the next time the system restarts? (Y/N)  
  ``` 

  Se você optar por verificar a unidade na próxima vez em que você reiniciar o computador **chkdsk** verifica a unidade e corrige os erros automaticamente quando você reinicia o computador. Se a partição da unidade for uma partição de inicialização **chkdsk** reinicia automaticamente o computador depois de verificar a unidade.

  Você também pode usar o **chkntfs /c** comando para agendar o volume a ser verificado na próxima vez que o computador for reiniciado. Use o **conjunto de sujas fsutil** comando para definir o volume do bits sujos (indica corrupção), para que o Windows seja executado **chkdsk** quando o computador for reiniciado.
- Relatório de erros de disco

  Você deve usar **chkdsk** ocasionalmente em sistemas de arquivos FAT e NTFS para verificar se há erros de disco. **CHKDSK** examina o espaço em disco e uso do disco e fornece um relatório de status específico para cada sistema de arquivos. O relatório de status mostra os erros encontrados no sistema de arquivos. Se você executar **chkdsk** sem o **/f** parâmetro em uma partição ativa, ele pode relatar erros falsos porque ele não é possível bloquear a unidade.
- Correção de erros de disco lógico

  **CHKDSK** corrige erros de disco lógico somente se você especificar o **/f** parâmetro. **CHKDSK** deve ser capaz de bloquear a unidade para corrigir erros.

  Porque os reparos em sistemas de arquivos FAT geralmente altera a tabela de alocação de arquivo do disco e, às vezes, causar uma perda de dados, **chkdsk** pode exibir uma mensagem de confirmação semelhante à seguinte:

  ```
  10 lost allocation units found in 3 chains.  
  Convert lost chains to files?  
  ``` 

  Se você pressionar **Y**, Windows salva cada cadeia perdida no diretório raiz, como um arquivo com um nome no formato de arquivo\<nnnn >. chk. Quando **chkdsk** for concluída, você pode verificar esses arquivos para ver se eles contêm dados que você precisa. Se você pressionar **N**, Windows corrigirá o disco, mas não salva o conteúdo das unidades de alocação perdida.

  Se você não usar o **/f** parâmetro, **chkdsk** exibe uma mensagem que o arquivo precisa ser corrigido, mas não corrige os erros.

  Se você usar **chkdsk /f** em um disco muito grande ou um disco com um número muito grande de arquivos (por exemplo, milhões de arquivos), **chkdsk /f** pode levar muito tempo para ser concluída.

- Localizando erros de disco físico

  Use o **/r** afetados de parâmetro a localizar erros de disco físico no sistema de arquivos e tentar recuperar dados de qualquer setores de disco.

- Usando o **chkdsk** com arquivos abertos

  Se você especificar o **/f** parâmetro, **chkdsk** exibe uma mensagem de erro se não houver arquivos abertos no disco. Se você não especificar o **/f** parâmetro e existe, de arquivos abertos **chkdsk** pode relatar as unidades de alocação perdida no disco. Isso pode ocorrer se abra arquivos ainda não foram registrados na tabela de alocação de arquivos. Se **chkdsk** relata a perda de um grande número de unidades de alocação, considere reparar o disco.

- Usando o **chkdsk** com cópias de sombra para pastas compartilhadas

  Porque as cópias de sombra de volume de origem de pastas compartilhadas não podem ser bloqueadas durante a cópias de sombra para pastas compartilhadas estiver habilitada, executando **chkdsk** na fonte de volume pode relatar erros falsos ou causar **chkdsk** inesperadamente. No entanto, você pode, verificar as cópias de sombra para erros executando **chkdsk** no modo somente leitura (sem parâmetros) para verificar as cópias de sombra de volume de armazenamento de pastas compartilhadas.

- Códigos de saída de Noções básicas sobre

  A seguinte tabela lista os códigos de saída que **chkdsk** relata após sua conclusão.  

  | Código de Saída |                                                   Descrição                                                    |
  |-----------|------------------------------------------------------------------------------------------------------------------|
  |     0     |                                              Nenhum erro foi encontrado.                                               |
  |     1     |                                           Erros foram encontrados e corrigidos.                                           |
  |     2     | Executada a limpeza de disco (por exemplo, a coleta de lixo) ou não executou limpeza porque **/f** não foi especificado. |
  |     3     | Não foi possível verificar o disco, não foi possível corrigir os erros ou erros não foram corrigidos porque **/f** não foi especificado.  |


- O **chkdsk** comando com parâmetros diferentes, está disponível no Console de recuperação.
- Nos servidores que são reiniciados com pouca frequência, talvez você queira usar o **chkntfs** ou o **consulta sujas do fsutil** comandos para determinar se o volume's sujo bits já estão definem antes de executar o chkdsk.

## <a name="examples"></a>Exemplos

Se você quiser verificar o disco na unidade D e fazer com que corrigir erros do Windows, digite:

```
chkdsk d: /f  
```

Se encontrar erros, **chkdsk** pausa e exibe as mensagens. **CHKDSK** for concluída, exibindo um relatório que lista o status do disco. Você não pode abrir todos os arquivos na unidade especificada até **chkdsk** for concluída.

Para verificar se todos os arquivos em um disco FAT no diretório atual de blocos não contíguos, digite:

```
chkdsk *.*  
```

**CHKDSK** exibe um relatório de status e, em seguida, lista os arquivos que correspondem as especificações de arquivo que tem blocos não contíguos.
#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
