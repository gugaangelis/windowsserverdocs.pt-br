---
title: fsutil
description: Artigo de referência para o comando fsutil, que executa tarefas relacionadas à FAT (tabela de alocação de arquivos) e sistemas de arquivos NTFS.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
ms.topic: reference
ms.date: 08/21/2018
ms.openlocfilehash: d8ad17d65d2f16a16393e71b707bcd6478de0052
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037764"
---
# <a name="fsutil"></a>fsutil

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Executa tarefas relacionadas à FAT (tabela de alocação de arquivos) e aos sistemas de arquivos NTFS, como o gerenciamento de pontos de nova análise, o gerenciamento de arquivos esparsos ou a desmontagem de um volume. Se for usado sem parâmetros, **fsutil** exibirá uma lista de subcomandos com suporte.

> [!NOTE]
> Você deve estar conectado como administrador ou membro do grupo Administradores para usar o **fsutil**. Esse comando é bastante poderoso e deve ser usado apenas por usuários avançados que têm um conhecimento completo dos sistemas operacionais Windows.
>
>Você deve habilitar o subsistema do Windows para Linux antes de executar o **fsutil**. Execute o seguinte comando como administrador no PowerShell para habilitar esse recurso opcional:
>
> `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux`
>
> Você será solicitado a reiniciar o computador quando ele estiver instalado. Depois que o computador for reiniciado, você poderá executar o **fsutil** como administrador.

### <a name="parameters"></a>Parâmetros

| Subcomando | Descrição |
| ---------- | ----------- |
| [fsutil 8dot3name](fsutil-8dot3name.md) | Consulta ou altera as configurações de comportamento de nome curto no sistema, por exemplo, gera 8,3 nomes de arquivo de comprimento de caractere. Remove nomes curtos de todos os arquivos em um diretório. Examina um diretório e identifica as chaves do registro que podem ser afetadas se nomes curtos forem removidos dos arquivos no diretório. |
| [fsutil dirty](fsutil-dirty.md) | Consulta se o bit sujo do volume está definido ou define um bit sujo do volume. Quando o bit sujo de um volume é definido, o **autochk** verifica automaticamente o volume em busca de erros na próxima vez em que o computador for reiniciado. |
| [fsutil file](fsutil-file.md) | Localiza um arquivo por nome de usuário (se as cotas de disco estiverem habilitadas), consulta intervalos alocados para um arquivo, define o nome curto de um arquivo, define o comprimento de dados válido de um arquivo, define zero data para um arquivo, cria um novo arquivo de um tamanho especificado, encontra uma ID de arquivo, se o nome ou encontra um nome de link de arquivo para uma |
| [fsutil fsinfo](fsutil-fsinfo.md) | Lista todas as unidades e consulta o tipo de unidade, informações de volume, informações de volume específicas do NTFS ou estatísticas do sistema de arquivos. |
| [fsutil hardlink](fsutil-hardlink.md) | Lista links físicos de um arquivo ou cria um link físico (uma entrada de diretório para um arquivo). Cada arquivo pode ser considerado com pelo menos um link físico. Em volumes NTFS, cada arquivo pode ter vários links físicos, de modo que um único arquivo pode aparecer em muitos diretórios (ou mesmo no mesmo diretório, com nomes diferentes). Como todos os links fazem referência ao mesmo arquivo, os programas podem abrir qualquer um dos links e modificar o arquivo. Um arquivo é excluído do sistema de arquivos somente depois que todos os links para ele são excluídos. Depois de criar um link físico, os programas podem usá-lo como qualquer outro nome de arquivo. |
| [fsutil objectid](fsutil-objectid.md) | Gerencia identificadores de objeto, que são usados pelo sistema operacional Windows para controlar objetos como arquivos e diretórios. |
| [fsutil quota](fsutil-quota.md) | Gerencia cotas de disco em volumes NTFS para fornecer um controle mais preciso do armazenamento baseado em rede. As cotas de disco são implementadas por volume e permitem que os limites de armazenamento físico e flexível sejam implementados em uma base por usuário. |
| [fsutil repair](fsutil-repair.md) | Consulta ou define o estado de auto-recuperação do volume. O NTFS de auto-recuperação tenta corrigir as corrupções do sistema de arquivos NTFS online sem exigir que **Chkdsk.exe** sejam executadas. Inclui iniciar a verificação no disco e aguardar a conclusão do reparo. |
| [fsutil reparsepoint](fsutil-reparsepoint.md) | Consulta ou exclui pontos de nova análise (objetos do sistema de arquivos NTFS que têm um atributo definíveis que contém dados controlados pelo usuário). Pontos de nova análise são usados para estender a funcionalidade no subsistema de entrada/saída (e/s). Eles são usados para pontos de junção de diretório e pontos de montagem de volume. Eles também são usados por drivers de filtro do sistema de arquivos para marcar determinados arquivos como especiais para esse driver. |
| [fsutil resource](fsutil-resource.md) | Cria um Gerenciador de recursos transacionais secundário, inicia ou interrompe um Gerenciador de recursos transacionais, exibe informações sobre um Gerenciador de recursos transacionais ou modifica seu comportamento. |
| [fsutil sparse](fsutil-sparse.md) | Gerencia arquivos esparsos. Um arquivo esparso é um arquivo com uma ou mais regiões de dados não alocados nele. Um programa verá essas regiões não alocadas como contendo bytes com o valor zero, mas nenhum espaço em disco será usado para representar esses zeros. Todos os dados significativos ou diferentes de zero são alocados, enquanto todos os dados não significativos (cadeias de caracteres grandes de dados compostos por zeros) não são alocados. Quando um arquivo esparso é lido, os dados alocados são retornados como dados armazenados e não alocados são retornados como zeros (por padrão, de acordo com a especificação de requisito de segurança C2). O suporte a arquivos esparsos permite que os dados sejam desalocados de qualquer lugar no arquivo. |
| [fsutil tiering](fsutil-tiering.md) | Habilita o gerenciamento de funções de camada de armazenamento, como a configuração e a desabilitação de sinalizadores e a listagem de camadas. |
| [fsutil transaction](fsutil-transaction.md)   | Confirma uma transação especificada, reverte uma transação especificada ou exibe informações sobre a transação. |
| [fsutil usn](fsutil-usn.md) | Gerencia o diário de alterações do USN (número de sequência de atualização), que fornece um log persistente de todas as alterações feitas em arquivos no volume. |
| [fsutil volume](fsutil-volume.md) | Gerencia um volume. Desmonta um volume, consulta para ver a quantidade de espaço livre disponível em um disco ou localiza um arquivo que está usando um cluster especificado. |
| [fsutil wim](fsutil-wim.md) | Fornece funções para descobrir e gerenciar arquivos com suporte do WIM. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)