---
title: mqbkup
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bdd41c4-75ef-455f-b241-1d64a4c7acf5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25bc5a0b5da0c78ccc68cf7b8fc70a5b4ec11a1f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723901"
---
# <a name="mqbkup"></a>mqbkup

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz backup de arquivos de mensagens do MSMQ e configurações do registro em um dispositivo de armazenamento e restaura as mensagens e configurações armazenadas anteriormente.   
O backup e a operação de restauração irão parar o serviço MSMQ local. Se o serviço MSMQ foi iniciado com antecedência, o utilitário tentará reiniciar o serviço MSMQ no final do backup ou na operação de restauração. Se o serviço já tiver sido interrompido antes da execução do utilitário, nenhuma tentativa de reiniciar o serviço será feita.  
Antes de usar o utilitário de backup/restauração de mensagens MSMQ, você deve fechar todos os aplicativos locais que estão usando o MSMQ.  
## <a name="syntax"></a>Sintaxe  
```  
mqbkup {/b | /r} <folder path_to_storage_device>  
```  
#### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|/b|Especifica a operação de backup|  
|/r|Especifica a operação de restauração|  
|> de <\_de pasta path_to_storage dispositivo|Especifica o caminho onde os arquivos de mensagem MSMQ e as configurações do registro são armazenados|  
|/?|Exibe a ajuda no prompt de comando.|  
## <a name="examples"></a>Exemplos  
Para fazer backup de todos os arquivos de mensagens MSMQ e configurações do registro e armazená-los na pasta *Msmqbkup* na unidade C:.  
```  
mqbkup /b c:\msmqbkup  
```  
se a pasta especificada não existir, o utilitário criará automaticamente uma. Se você optar por especificar uma pasta existente, essa pasta deverá estar vazia. Se você especificar uma pasta não vazia, o utilitário excluirá todos os arquivos e subpastas contidos nela. Nesse caso, você será solicitado a conceder permissão para excluir arquivos e subpastas existentes. Você pode usar o parâmetro **/y** para indicar que concorda antecipadamente com a exclusão de todos os arquivos e subpastas existentes na pasta especificada.  
Para excluir todos os arquivos e subpastas na pasta *Oldbkup* na unidade C: e armazenar os arquivos de mensagens MSMQ e as configurações do registro nesta pasta.  
```  
mqbkup /b /y c:\oldbkup  
```  
Para restaurar mensagens MSMQ e configurações do registro:  
```  
mqbkup /r c:\msmqbkup  
```  
Os locais de pastas usados para armazenar arquivos de mensagens MSMQ são armazenados no registro. Portanto, o utilitário irá restaurar os arquivos de mensagem do MSMQ para as pastas especificadas no registro e não para as pastas de armazenamento usadas antes da operação de restauração. Se as pastas especificadas no registro não existirem, a operação de restauração as criará automaticamente. Se os diretórios de pastas existirem e não estiverem vazios, o utilitário solicitará permissão para excluir o conteúdo atual dessas pastas.  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
