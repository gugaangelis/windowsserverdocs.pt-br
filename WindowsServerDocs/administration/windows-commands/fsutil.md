---
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
title: Fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 08/21/2018
ms.openlocfilehash: df8d25b01b67010734deb8dd7e42f3233e6011fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866807"
---
# <a name="fsutil"></a>Fsutil

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Executa tarefas relacionadas à tabela de alocação (FAT) e sistemas de arquivos NTFS, como gerenciamento de nova análise do arquivo aponta, gerenciar arquivos esparsos ou desmontar um volume. Se for usado sem parâmetros, **Fsutil** exibe uma lista de subcomandos com suporte. 

> [!Note] 
> Você deve fazer logon como um administrador ou um membro do grupo Administradores para usar Fsutil. O comando Fsutil é bastante potente e deve ser usado somente por usuários avançados que tenham um conhecimento profundo dos sistemas de operacionais do Windows.
>
>Você precisa habilitar o subsistema do Windows para Linux antes de executar **Fsutil**. Execute o seguinte comando como administrador no PowerShell para habilitar esse recurso opcional:
>
>```
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
>```
> Você será solicitado a reiniciar o computador quando ele é instalado. Depois que o computador for reiniciado, você poderá executar **Fsutil** como administrador.

## <a name="parameters"></a>Parâmetros

|Subcomando |Descrição|
|---|---|
|[Fsutil 8dot3name](fsutil-8dot3name.md) | Consulta ou altera as configurações de comportamento de nome curto no sistema, por exemplo, gera nomes de arquivo 8.3. Remove a nomes abreviados para todos os arquivos dentro de um diretório. Examina um diretório e identifica as chaves do registro que podem ser afetadas se os nomes curtos foram retirados os arquivos no diretório.|
|[comportamento do fsutil](fsutil-behavior.md) |As consultas ou define o comportamento de volume.|
|[fsutil suja](fsutil-dirty.md)| Consulta se o bit incorreto do volume está definido ou define o bit de um volume incorreto. Quando um volume's sujo bit estiver definido, **autochk** verifica automaticamente o volume de erros na próxima vez que o computador for reiniciado.|
|[Arquivo do fsutil](fsutil-file.md)|Localiza um arquivo por nome de usuário (se as cotas de disco estiverem habilitadas), consulta intervalos alocados para um arquivo, define um nome curto de arquivo, define o comprimento de dados válido do arquivo, define zero dados para um arquivo, cria um novo arquivo de um tamanho especificado, encontra uma ID de arquivo se considerando o nome , ou encontrar um nome de arquivo de link para uma ID de arquivo especificado.|
|[fsutil fsinfo](fsutil-fsinfo.md)|Lista todas as unidades e consulta o tipo de unidade, informações de volume, informações de volume de NTFS específicas ou estatísticas do sistema de arquivos.|
|[fsutil hardlink](fsutil-hardlink.md)|Lista de links físicos para um arquivo ou cria um link físico (uma entrada de diretório para um arquivo). Todos os arquivos podem ser considerados para ter pelo menos um link físico. Em volumes NTFS, cada arquivo pode ter vários links físicos, portanto, um único arquivo pode aparecer em várias pastas (ou até mesmo no mesmo diretório, com nomes diferentes). Porque todos os links de referência no mesmo arquivo, programas podem abrir qualquer um dos links e modificar o arquivo. Um arquivo é excluído do sistema de arquivos somente depois que todos os links relacionados serão excluídos. Depois de criar um link físico, programas podem usá-lo como qualquer outro nome de arquivo.|
|[Objectid do fsutil](fsutil-objectid.md)|Gerencia os identificadores de objeto, que são usados pelo sistema operacional Windows para controlar objetos como arquivos e diretórios.|
|[cota do fsutil](fsutil-quota.md)|Gerencia as cotas de disco em volumes NTFS para fornecer um controle mais preciso do armazenamento baseado em rede. As cotas de disco são implementadas em uma base por volume e permitem que os limites de disco rígido e reversível-armazenamento a ser implementada em uma base por usuário.|
|[reparação fsutil](fsutil-repair.md)|As consultas ou define o estado de auto-recuperação do volume. Autorrecuperação NTFS tenta corrigir corrupções do sistema de arquivos NTFS on-line sem a necessidade **Chkdsk.exe** para ser executado. Inclui iniciando a verificação em disco e aguardar a conclusão de reparo.|
|[fsutil reparsepoint](fsutil-reparsepoint.md)|Consultas ou exclusões de nova análise pontos (NTFS objetos de sistema que têm um atributo definível contendo dados controlados pelo usuário). Nova análise pontos são usados para estender a funcionalidade no subsistema de e/s () de entrada/saída. Eles são usados para pontos de junção de diretório e pontos de montagem de volume. Eles também são usados por drivers de filtro do sistema de arquivos para marcar determinados arquivos como especiais para esse driver.|
|[Recursos do fsutil](fsutil-resource.md)|Cria um Gerenciador de recursos transacionais secundário, inicia ou interrompe um Gerenciador de recursos transacional, exibe informações sobre um Gerenciador de recursos transacional ou modifica seu comportamento.|
|[fsutil esparso](fsutil-sparse.md)|Gerencia arquivos esparsos. Um arquivo esparso é um arquivo com uma ou mais regiões de dados não alocados. Um programa verá essas regiões não alocados como contendo bytes com o valor de zero, mas nenhum espaço em disco é usado para representar esses zeros. Todos os dados significativos ou diferente de zero é alocado, ao passo que todos os dados não significativos (grandes cadeias de caracteres de dados compostas de zeros) não está alocada. Quando um arquivo esparso é lido, dados alocados são retornados como armazenados e não alocado de dados é retornado como zeros (por padrão de acordo com a especificação de requisito de segurança de C2). Suporte a arquivos esparsos permite que os dados a serem desalocados em qualquer lugar no arquivo.|
|[Disposição em camadas do fsutil](fsutil-tiering.md)|Permite o gerenciamento de funções de camada de armazenamento, como configuração e desabilitando sinalizadores e listagem das camadas.|
|[Transação fsutil](fsutil-transaction.md)|Confirma uma transação especificada, reverte uma transação especificada ou exibe informações sobre a transação.|
|[fsutil usn](fsutil-usn.md)|Gerencia o diário de alterações de atualização sequência USN (número), que fornece um log persistente de todas as alterações feitas nos arquivos no volume.|
|[volume do fsutil](fsutil-volume.md)|Gerencia um volume. Desmonta um volume, consultas para ver quanto espaço livre está disponível em um disco, ou localiza um arquivo que está usando um cluster especificado.|
|[Wim fsutil](fsutil-wim.md)|Fornece funções para descobrir e gerenciar arquivos WIM com suporte.|

## <a name="see-also"></a>Consulte também
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)