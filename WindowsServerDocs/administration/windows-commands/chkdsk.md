---
title: chkdsk
description: Tópico de referência para o comando chkdsk, que verifica o sistema de arquivos e os metadados do sistema de arquivos de um volume para erros lógicos e físicos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 62912a3c-d2cc-4ef6-9679-43709a286035
author: jasongerend
ms.author: jgerend
manager: lizapo
ms.date: 10/09/2019
ms.openlocfilehash: 4843624337e031f81453def78e4df97bdbfe821e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82714404"
---
# <a name="chkdsk"></a>chkdsk

Verifica o sistema de arquivos e os metadados do sistema de arquivos de um volume para erros lógicos e físicos. Se usado sem parâmetros, **chkdsk** exibirá apenas o status do volume e não corrigirá nenhum erro. Se usado com os parâmetros **/f**, **/r**, **/x**ou **/b** , ele corrige erros no volume.

> [!IMPORTANT]
> A associação no grupo local de **Administradores** , ou equivalente, é o mínimo necessário para executar **chkdsk**. Para abrir uma janela de prompt de comando como administrador, clique com o botão direito do mouse em **prompt de comando** no menu **Iniciar** e clique em **Executar como administrador**.

> [!IMPORTANT]
> Não é recomendável interromper o **chkdsk** . No entanto, cancelar ou interromper o **chkdsk** não deve deixar o volume mais corrompido do que era antes da execução do **chkdsk** . Executar o **chkdsk** novamente verifica e deve reparar qualquer corrupção restante no volume.

> [!NOTE]
> O chkdsk pode ser usado somente para discos locais. O comando não pode ser usado com uma letra de unidade local que foi redirecionada pela rede.

## <a name="syntax"></a>Sintaxe

```
chkdsk [<volume>[[<path>]<filename>]] [/f] [/v] [/r] [/x] [/i] [/c] [/l[:<size>]] [/b]  
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<volume>` | Especifica a letra da unidade (seguida por dois-pontos), ponto de montagem ou nome do volume. |
| [ `[<path>]<filename>` | Use somente com FAT (tabela de alocação de arquivos) e FAT32. Especifica o local e o nome de um arquivo ou conjunto de arquivos que você deseja que o **chkdsk** Verifique a fragmentação. Você pode usar o **?** e **&#42;** caracteres curinga para especificar vários arquivos. |
| /f | Corrige erros no disco. O disco deve estar bloqueado. Se **chkdsk** não puder bloquear a unidade, será exibida uma mensagem perguntando se você deseja verificar a unidade na próxima vez que reiniciar o computador. |
| /v | Exibe o nome de cada arquivo em cada diretório à medida que o disco é verificado. |
| /r | Localiza setores inválidos e recupera informações legíveis. O disco deve estar bloqueado. **/r** inclui a funcionalidade de **/f**, com a análise adicional de erros de disco físico. |
| /x | Força o volume a ser desmontado primeiro, se necessário. Todos os identificadores abertos para a unidade são invalidados. **/x** também inclui a funcionalidade de **/f**.  |
| /i | Use somente com NTFS. Executa uma verificação menos rigorosa de entradas de índice, o que reduz a quantidade de tempo necessária para executar **chkdsk**. |
| /c | Use somente com NTFS. Não verifica os ciclos dentro da estrutura de pastas, o que reduz a quantidade de tempo necessária para executar **chkdsk**.  |
| /l [:`<size>`] | Use somente com NTFS. Altera o tamanho do arquivo de log para o tamanho que você digitar. Se você omitir o parâmetro de tamanho, **/l** exibirá o tamanho atual. |
| /b | Use somente com NTFS. Limpa a lista de clusters inválidos no volume e examina novamente todos os clusters alocados e livres para erros. **/b** inclui a funcionalidade de **/r**. Use esse parâmetro após a geração de imagens de um volume para uma nova unidade de disco rígido. |
| /Scan | Use somente com NTFS. Executa uma verificação online no volume. |
| /forceofflinefix | Use somente com NTFS (deve ser usado com **/Scan**). Ignorar todo o reparo online; todos os defeitos encontrados são enfileirados para reparo offline (por `chkdsk /spotfix`exemplo,). |
| /perf | Use somente com NTFS (deve ser usado com **/Scan**). O usa mais recursos do sistema para concluir uma verificação o mais rápido possível. Isso pode afetar o desempenho negativo em outras tarefas em execução no sistema. |
| /spotfix | Use somente com NTFS. Executa a correção de pontos no volume. |
| /sdcleanup | Use somente com NTFS. O lixo coleta dados de descritor de segurança desnecessários (implica **/f**). |
| /offlinescanandfix | Executa uma verificação offline e corrige o volume. |
| /freeorphanedchains | Use somente com FAT/FAT32/exFAT. Libera quaisquer cadeias de cluster órfãs em vez de recuperar seu conteúdo. |
| /markclean | Use somente com FAT/FAT32/exFAT. Marca o volume limpo se nenhuma corrupção for detectada, mesmo se **/f** não tiver sido especificado. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários

- A opção **/i** ou **/c** reduz a quantidade de tempo necessária para executar **chkdsk** ignorando determinadas verificações de volume.

- Se você quiser que o **chkdsk** corrija erros de disco, não poderá ter arquivos abertos na unidade. Se os arquivos estiverem abertos, a seguinte mensagem de erro será exibida:

  ```
  Chkdsk cannot run because the volume is in use by another process. Would you like to schedule this volume to be checked the next time the system restarts? (Y/N)  
  ```

- Se você optar por verificar a unidade na próxima vez que reiniciar o computador, o **chkdsk** verificará a unidade e corrigirá os erros automaticamente quando você reiniciar o computador. Se a partição da unidade for uma partição de inicialização, **chkdsk** reinicia automaticamente o computador depois de verificar a unidade.

- Você também pode usar o `chkntfs /c` comando para agendar o volume a ser verificado na próxima vez em que o computador for reiniciado. Use o `fsutil dirty set` comando para definir o bit sujo do volume (indicando corrupção), para que o Windows execute **chkdsk** quando o computador for reiniciado.

- Você deve usar **chkdsk** ocasionalmente em sistemas de arquivos FAT e NTFS para verificar se há erros de disco. O **chkdsk** examina o espaço em disco e o uso do disco e fornece um relatório de status específico para cada sistema de arquivos. O relatório de status mostra os erros encontrados no sistema de arquivos. Se você executar **chkdsk** sem o parâmetro **/f** em uma partição ativa, ele poderá relatar erros artificiais porque ele não pode bloquear a unidade.

- O **chkdsk** corrigirá erros de disco lógico somente se você especificar o parâmetro **/f** . **Chkdsk** deve ser capaz de bloquear a unidade para corrigir erros.

  Como os reparos em sistemas de arquivos FAT geralmente alteram a tabela de alocação de arquivos de um disco e, às vezes, causam perda de dados, o **chkdsk** pode exibir uma mensagem de confirmação semelhante à seguinte:

  ```
  10 lost allocation units found in 3 chains.  
  Convert lost chains to files?  
  ```

    - Se você pressionar **s**, o Windows salvará cada cadeia perdida no diretório raiz como um arquivo com um nome no formato file`<nnnn>`. chk. Quando **chkdsk** for concluído, você poderá verificar esses arquivos para ver se eles contêm os dados necessários.

    - Se você pressionar **N**, o Windows corrigirá o disco, mas não salvará o conteúdo das unidades de alocação perdidas.

- Se você não usar o parâmetro **/f** , **chkdsk** exibirá uma mensagem informando que o arquivo precisa ser corrigido, mas não corrigirá nenhum erro.

- Se você usar `chkdsk /f*` o em um disco muito grande ou um disco com um número muito grande de arquivos (por exemplo, milhões de arquivos) `chkdsk /f` , o pode levar muito tempo para ser concluído.

- Use o parâmetro **/r** para localizar erros de disco físico no sistema de arquivos e tentar recuperar dados de quaisquer setores de disco afetados.

- Se você especificar o parâmetro **/f** , **chkdsk** exibirá uma mensagem de erro se houver arquivos abertos no disco. Se você não especificar o parâmetro **/f** e os arquivos abertos existirem, o **chkdsk** poderá relatar unidades de alocação perdidas no disco. Isso pode acontecer se os arquivos abertos ainda não tiverem sido gravados na tabela de alocação de arquivos. Se **chkdsk** reportar a perda de um grande número de unidades de alocação, considere reparar o disco.

- Como o volume de origem Cópias de Sombra para Pastas Compartilhadas não pode ser bloqueado enquanto **cópias de sombra para pastas compartilhadas** está habilitado, a execução de **chkdsk** no volume de origem pode relatar erros falsos ou fazer com que **chkdsk** saia inesperadamente. No entanto, você pode verificar se há erros nas cópias de sombra executando **chkdsk** no modo somente leitura (sem parâmetros) para verificar o volume de armazenamento de cópias de sombra para pastas compartilhadas.

- O comando **chkdsk** , com parâmetros diferentes, está disponível no console de recuperação.

- Em servidores que são reiniciados com pouca frequência, talvez você queira usar o **chkntfs** ou os `fsutil dirty query` comandos para determinar se o bit sujo do volume já está definido antes de executar chkdsk.

### <a name="understanding-exit-codes"></a>Noções básicas sobre códigos de saída

A tabela a seguir lista os códigos de saída que o **chkdsk** relata após a conclusão.  

  | Código de saída | Descrição |
  | --------- | ----------- |
  | 0 | Nenhum erro encontrado. |
  | 1 | Foram encontrados e corrigidos os erros. |
  | 2 | Realizou a limpeza de disco (como a coleta de lixo) ou não executou a limpeza porque **/f** não foi especificado. |
  | 3 | Não foi possível verificar o disco, não foi possível corrigir os erros ou os erros não foram corrigidos porque **/f** não foi especificado. |

## <a name="examples"></a>Exemplos

Para verificar o disco na unidade D e corrigir os erros do Windows, digite:

```
chkdsk d: /f  
```

Se ele encontrar erros, **chkdsk** pausará e exibirá mensagens. **Chkdsk** é concluído exibindo um relatório que lista o status do disco. Você não pode abrir nenhum arquivo na unidade especificada até que **chkdsk** seja concluído.

Para verificar se todos os arquivos em um disco FAT no diretório atual são blocos não contíguos, digite:

```
chkdsk *.*  
```

**Chkdsk** exibe um relatório de status e lista os arquivos que correspondem às especificações de arquivo que têm blocos não contíguos.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
