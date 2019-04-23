---
title: prncnfg
description: Saiba como configurar uma impressora usando o comando prncfg.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6426b053a5c56918768f82cbd7631631abcf0a6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859147"
---
# <a name="prncnfg"></a>prncnfg

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura ou exibe informações de configuração sobre uma impressora.

## <a name="syntax"></a>Sintaxe
```
cscript Prncnfg {-g | -t | -x | -?} [-S <ServerName>] [-P <printerName>] [-z <NewprinterName>] [-u <UserName>] [-w <Password>] [-r <PortName>] [-l <Location>] [-h <Sharename>] [-m <Comment>] [-f <SeparatorFileName>] [-y <Datatype>] [-st <starttime>] [-ut <Untiltime>] [-i <DefaultPriority>] [-o <Priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|-g|Exibe informações de configuração sobre uma impressora.|
|-t|Configura uma impressora.|
|-x|renomeia uma impressora.|
|-S \<ServerName\>|Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.|
|-P \<printerName\>|Especifica o nome da impressora que você deseja gerenciar. Obrigatório.|
|-z \<NewprinterName\>|Especifica o novo nome de impressora. Requer o **- x** e **-P** parâmetros.|
|-u \<nome de usuário\> -w \<senha\>|Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores local do computador de destino têm essas permissões, mas também podem ser concedidas as permissões a outros usuários. Se você não especificar uma conta, você deve fazer logon em uma conta com essas permissões para o comando funcione.|
|-r \<PortName\>|Especifica a porta em que a impressora está conectada. Se esse for um paralelo ou uma porta serial, em seguida, use a ID da porta (por exemplo, "LPT1" ou "COM1"). Quando se trata de uma porta TCP/IP, use o nome da porta que foi especificado quando a porta foi adicionada.|
|-l \<local\>|Especifica o local da impressora, como "Copiar sala".|
|-h \<Sharename\>|Especifica o nome do compartilhamento da impressora.|
|-m \<comentário\>|Especifica a cadeia de caracteres de comentário da impressora.|
|-f \<SeparatorFileName\>|Especifica um arquivo que contém o texto que aparece na página do separador.|
|-y \<Datatype\>|Especifica os tipos de dados que a impressora pode aceitar.|
|-st \<starttime\>|Configura a impressora para disponibilidade limitada. Especifica a hora do dia em que a impressora está disponível. Se você enviar um documento para uma impressora quando ele não estiver disponível, o documento será retido (colocado no spool) até que a impressora fique disponível. Você deve especificar a hora como um relógio de 24 horas. Por exemplo, para especificar 11:00 P.M., digite **2300**.|
|-ut \<Endtime\>|Configura a impressora para disponibilidade limitada. Especifica a hora do dia que a impressora não está mais disponível. Se você enviar um documento para uma impressora quando ele não estiver disponível, o documento será retido (colocado no spool) até que a impressora fique disponível. Você deve especificar a hora como um relógio de 24 horas. Por exemplo, para especificar 11:00 P.M., digite **2300**.|
|-o \<prioridade\>|Especifica a prioridade que o spooler usa para rotear trabalhos de impressão na fila de impressão. Uma fila de impressão com uma prioridade mais alta recebe todos os seus trabalhos antes de qualquer fila com uma prioridade mais baixa.|
|-i \<DefaultPriority\>|Especifica a prioridade padrão atribuída a cada trabalho de impressão.|
|{+&#124;-}shared|Especifica se a impressora é compartilhada na rede.|
|{+&#124;-}direct|Especifica se o documento deve ser enviado diretamente para a impressora no spool.|
|{+&#124;-}published|Especifica se esta impressora deve ser publicada no active directory. Se você publicar a impressora, outros usuários podem pesquisar com base em sua localização e recursos (como a impressão colorida e grampeamento).|
|{+&#124;-}hidden|Função reservada.|
|{+&#124;-}rawonly|Especifica se somente trabalhos de impressão dados brutos podem ser colocados no spool nessa fila.|
|{+ &#124; -}queued|Especifica que a impressora não deve começar a imprimir depois que a última página do documento seja colocado no spool. O programa de impressão não está disponível até que o documento terminou a impressão. No entanto, o uso desse parâmetro garante que todo o documento está disponível para a impressora.|
|{+ &#124; -}keepprintedjobs|Especifica se o spooler deve reter os documentos após a impressão. A habilitação dessa opção permite que um usuário envie novamente um documento para a impressora da fila de impressão, em vez do programa de impressão.|
|{+ &#124; -}workoffline|Especifica se um usuário é capaz de enviar trabalhos de impressão à fila de impressão, se o computador não estiver conectado à rede.|
|{+ &#124; -}enabledevq|Especifica se os trabalhos de impressão que não correspondem a configuração da impressora (por exemplo, arquivos PostScript no spool impressoras não-PostScript) devem ser mantidos na fila em vez de que está sendo impresso.|
|{+ &#124; -}docompletefirst|Especifica se o spooler deve enviar trabalhos de impressão com uma prioridade mais baixa que concluíram o spool antes de enviar trabalhos de impressão com uma prioridade mais alta que não concluíram o spool. Se essa opção estiver habilitada e documentos não tiverem concluído o spool, o spooler enviará os documentos maiores antes dos menores. Você deve habilitar essa opção se você quiser maximizar a eficiência da impressora às custas de prioridade do trabalho. Se essa opção estiver desabilitada, o spooler sempre envia os trabalhos com prioridade mais alta para suas respectivas filas pela primeira vez.|
|{+ &#124; -}enablebidi|Especifica se a impressora envia informações de status para o spooler.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   O **prncnfg** comando é um script do Visual Basic localizado no %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando no prompt de comando, digite **cscript** seguido pelo caminho completo do arquivo prncnfg ou altere os diretórios para a pasta apropriada. Por exemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg
    ```
-   Se as informações que você fornece contiverem espaços, use aspas ao redor do texto (por exemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Exemplos
Para exibir informações de configuração para a impressora nomeada Impressora_colorida_2 com uma fila de impressão hospedada pelo computador remoto Servidor_HR, digite:
```
cscript prncnfg -g -S HRServer -P colorprinter_2 
```

Para configurar uma impressora chamada Impressora_colorida_2, de modo que o spooler no computador remoto Servidor_HR mantém os trabalhos de impressão depois que eles foram impressos, digite:
```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs 
```

Para alterar o nome de uma impressora no computador remoto Servidor_HR de ImpCor_2 para Impressora_colorida 3, tipo:
```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3" 
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência do comando Imprimir](print-command-reference.md)
