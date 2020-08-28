---
title: prncnfg
description: Artigo de referência para o comando prncnfg, que configura ou exibe informações de configuração sobre uma impressora.
ms.topic: reference
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: ba5d465a46a23261942428761d11ef279b78a62e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038717"
---
# <a name="prncnfg"></a>prncnfg

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura ou exibe informações de configuração sobre uma impressora. Esse comando é um script Visual Basic localizado no `%WINdir%\System32\printing_Admin_Scripts\<language>` diretório. Para usar esse comando em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prncnfg ou altere os diretórios para a pasta apropriada. Por exemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg`.

## <a name="syntax"></a>Sintaxe

```
cscript prncnfg {-g | -t | -x | -?} [-S <Servername>] [-P <Printername>] [-z <newprintername>] [-u <Username>] [-w <password>] [-r <portname>] [-l <location>] [-h <sharename>] [-m <comment>] [-f <separatorfilename>] [-y <datatype>] [-st <starttime>] [-ut <untiltime>] [-i <defaultpriority>] [-o <priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -g | Exibe informações de configuração sobre uma impressora. |
| -T | Configura uma impressora. |
| -X | Renomeia uma impressora. |
| -S `<Servername>` | Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado. |
| -P `<Printername>` | Especifica o nome da impressora que você deseja gerenciar. Obrigatórios. |
| -z `<newprintername>` | Especifica o novo nome da impressora. Requer os parâmetros **-x** e **-P** . |
| -u `<Username>` -w `<password>` | Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione. |
| -r `<portname>` | Especifica a porta onde a impressora está conectada. Se essa for uma porta paralela ou serial, use a ID da porta (por exemplo, LPT1 ou COM1). Se esta for uma porta TCP/IP, use o nome da porta que foi especificado quando a porta foi adicionada. |
| -l `<location>` | Especifica o local da impressora, como **Copyroom**. Se o local contiver espaços, use aspas em volta do texto, como **"copiar sala"**.|
| -h `<sharename>` | Especifica o nome de compartilhamento da impressora. |
| -m `<comment>` | Especifica a cadeia de caracteres de comentário da impressora. |
| -f `<separatorfilename>` | Especifica um arquivo que contém o texto que aparece na página separadora. |
| -y `<datatype>` | Especifica os tipos de dados que a impressora pode aceitar. |
| -St `<starttime>` | Configura a impressora para a disponibilidade limitada. Especifica a hora do dia em que a impressora está disponível. Se você enviar um documento para uma impressora quando ela não estiver disponível, o documento será mantido (colocado em spool) até que a impressora fique disponível. Você deve especificar a hora como um relógio de 24 horas. Por exemplo, para especificar 11:00 P.M., digite **2300**. |
| -UT `<endtime>` | Configura a impressora para a disponibilidade limitada. Especifica a hora do dia em que a impressora não está mais disponível. Se você enviar um documento para uma impressora quando ela não estiver disponível, o documento será mantido (colocado em spool) até que a impressora fique disponível. Você deve especificar a hora como um relógio de 24 horas. Por exemplo, para especificar 11:00 P.M., digite **2300**. |
| -o `<priority>` | Especifica uma prioridade que o spooler usa para rotear trabalhos de impressão na fila de impressão. Uma fila de impressão com prioridade mais alta recebe todos os seus trabalhos antes de qualquer fila com prioridade mais baixa. |
| -i `<defaultpriority>` | Especifica a prioridade padrão atribuída a cada trabalho de impressão. |
| `{+|-}`compartilhado | Especifica se esta impressora é compartilhada na rede. |
| `{+|-}`Encaminhe | Especifica se o documento deve ser enviado diretamente para a impressora sem ser colocado no spool. |
| `{+|-}`Checked | Especifica se esta impressora deve ser publicada no Active Directory. Se você publicar a impressora, outros usuários poderão procurá-la com base em sua localização e recursos (como impressão de cores e grampeamento). |
| `{+|-}`oculto | Função reservada. |
| `{+|-}`rawonly | Especifica se somente trabalhos de impressão de dados brutos podem ser colocados em spool nessa fila. |
| `{+|-}`} na fila | Especifica que a impressora não deve começar a imprimir até que a última página do documento seja colocada no spool. O programa de impressão não está disponível até que o documento termine de ser impresso. No entanto, o uso desse parâmetro garante que todo o documento esteja disponível para a impressora. |
| `{+|-}`keepprintedjobs | Especifica se o spooler deve reter os documentos depois de serem impressos. Habilitar essa opção permite que um usuário reenvie um documento para a impressora da fila de impressão em vez do programa de impressão. |
| `{+|-}`workoffline | Especifica se um usuário é capaz de enviar trabalhos de impressão para a fila de impressão se o computador não estiver conectado à rede. |
| `{+|-}`enabledevq | Especifica se os trabalhos de impressão que não correspondem à configuração da impressora (por exemplo, arquivos PostScript em spool em impressoras não-PostScript) devem ser mantidos na fila em vez de serem impressos. |
| `{+|-}`docompletefirst | Especifica se o spooler deve enviar trabalhos de impressão com uma prioridade mais baixa que concluiu o spooling antes de enviar trabalhos de impressão com uma prioridade mais alta que não concluiu o spooling. Se essa opção estiver habilitada e nenhum documento tiver concluído o spool, o spooler enviará documentos maiores antes dos menores. Você deve habilitar essa opção se quiser maximizar a eficiência da impressora no custo da prioridade do trabalho. Se essa opção estiver desabilitada, o spooler sempre enviará trabalhos de prioridade mais alta para suas respectivas filas primeiro. |
| `{+|-}`enablebidi | Especifica se a impressora envia informações de status para o spooler. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para exibir informações de configuração para a impressora chamada *ColorPrinter_2* com uma fila de impressão hospedada pelo computador remoto chamado *HRServer*, digite:

```
cscript prncnfg -g -S HRServer -P colorprinter_2
```

Para configurar uma impressora chamada *ColorPrinter_2* para que o spooler no computador remoto chamado *HRServer* Mantenha os trabalhos de impressão depois que eles forem impressos, digite:

```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs
```

Para alterar o nome de uma impressora no computador remoto chamado *HRServer* de *ColorPrinter_2* para *colorprinter 3*, digite:

```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3"
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos de impressão](print-command-reference.md)
