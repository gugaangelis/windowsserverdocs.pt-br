---
title: mqbkup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a809bfc788ba25afab280e80e5c6f0dc9c70dc86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842897"
---
# <a name="mqbkup"></a>mqbkup

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz backup do registro e arquivos de mensagem MSMQ configurações para um dispositivo de armazenamento e restaura as configurações e mensagens armazenadas anteriormente.   
O backup e a operação de restauração irá parar o serviço local do MSMQ. Se o serviço MSMQ foi iniciado com antecedência, o utilitário tentará reiniciar o serviço MSMQ no final do backup ou a operação de restauração. Se o serviço já foi interrompido antes de executar o utilitário, não é feita nenhuma tentativa de reiniciar o serviço.  
Antes de usar o utilitário de Backup/restauração de mensagem do MSMQ, você deve fechar todos os aplicativos locais que estão usando o MSMQ.  
## <a name="syntax"></a>Sintaxe  
```  
mqbkup {/b | /r} <folder path_to_storage_device>  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|/b|Especifica a operação de backup|  
|/r|Especifica a operação de restauração|  
|< pasta path_to_storage\_dispositivo >|Especifica o caminho onde os arquivos de mensagem do MSMQ e configurações do registro são armazenadas|  
|/?|Exibe a ajuda no prompt de comando.|  
## <a name="BKMK_Examples"></a>Exemplos  
Para fazer backup de todos os arquivos de mensagem do MSMQ e as configurações do registro e armazená-los de *Msmqbkup* pasta na unidade c:.  
```  
mqbkup /b c:\msmqbkup  
```  
Se a pasta especificada não existir, o utilitário criará automaticamente um. Se você optar por especificar uma pasta existente, essa pasta deve estar vazia. Se você especificar uma pasta de não vazio, o utilitário excluirá todos os arquivos e subpastas contidos nela. Nesse caso, você será solicitado a dar permissão para excluir as subpastas e arquivos existentes. Você pode usar o **/y** parâmetro para indicar que você concorda com antecedência para a exclusão de todos os arquivos e subpastas na pasta especificada.  
Para excluir todos os arquivos e subpastas na *Oldbkup* pasta em sua unidade c: e arquivos de mensagem do MSMQ de repositório e configurações do registro nessa pasta.  
```  
mqbkup /b /y c:\oldbkup  
```  
Para restaurar as mensagens do MSMQ e as configurações do registro:  
```  
mqbkup /r c:\msmqbkup  
```  
Os locais das pastas usadas para armazenar arquivos de mensagem MSMQ são armazenados no registro. Assim, o utilitário restaurará os arquivos de mensagem do MSMQ para as pastas especificadas no registro e não para as pastas de armazenamento usadas antes da operação de restauração. Se as pastas especificadas no registro não existir, a operação de restauração irá criá-las automaticamente. Se pastas diretórios existe e não estiverem vazios, o utilitário solicitará permissão Excluir o conteúdo atual de uma dessas pastas.  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
