---
title: ntfrsutl
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e94d2b0a9ca764a6e8a25a087817598e3f158581
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436407"
---
# <a name="ntfrsutl"></a>ntfrsutl

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Despeja as tabelas internas, thread e informações de memória para o serviço de replicação de arquivos NT \(NTFRS\). Ele é executado em servidores locais e remotos. A configuração de recuperação para no Gerenciador de controle de serviço do NTFRS \(SCM\) pode ser essencial para a localização e manter os eventos de log importantes no computador. Essa ferramenta fornece um método conveniente de revisar essas configurações.   
  
## <a name="syntax"></a>Sintaxe  
  
```  
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]  
ntfrsutl[memory|threads|stage][<computer>]  
ntfrsutl ds[<computer>]  
ntfrsutl [sets][<computer>]  
ntfrsutl [version][<computer>]  
ntfrsutl poll[/quickly[=[<N>]]][/slowly[=[<N>]]][/now][<computer>]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|  Parâmetro  |                                                                                                                                                                                                                                                                                                                                        Descrição                                                                                                                                                                                                                                                                                                                                         |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   idtable   |                                                                                                                                                                                                                                                                                                                                          Tabela de identificação                                                                                                                                                                                                                                                                                                                                          |
| ConfigTable |                                                                                                                                                                                                                                                                                                                                  Tabela de configuração do FRS                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        Entrada de log                                                                                                                                                                                                                                                                                                                                         |
|   log de saída    |                                                                                                                                                                                                                                                                                                                                        Log de saída                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  Especifica o computador.                                                                                                                                                                                                                                                                                                                                   |
|   memória    |                                                                                                                                                                                                                                                                                                                                        Uso de memória                                                                                                                                                                                                                                                                                                                                        |
|   Threads   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    preparar    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     ds      |                                                                                                                                                                                                                                                                                                                         lista modo de exibição do serviço NTFRS do serviço de diretório.                                                                                                                                                                                                                                                                                                                          |
|    Conjuntos de     |                                                                                                                                                                                                                                                                                                                             Especifica os conjuntos de réplica do Active Directory                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Especifica as versões de API e NTFRS do serviço.                                                                                                                                                                                                                                                                                                                        |
|    Sondagem     | Especifica os intervalos de sondagem atual.<br /><br />Parâmetros:<br /><br /><ul><li>**\/rapidamente** \[ **\=** \[ <N> \] \] \(sonda rapidamente\)<br /><br /><ul><li>**rapidamente** \- sonda rapidamente até que a configuração estável é rectrieved</li><li>**rapidamente\=**  \- pesquisa rapidamente a cada minutos padrão.</li><li>**rapidamente\=**  <N> \- sonda rapidamente cada *N* minutos</li></ul></li><li>**\/lentamente** \[ **\=** \[ <N> \] \] \(sonda lentamente\)<br /><br /><ul><li>**lentamente** \- sonda lentamente até que a configuração estável é recuperada</li><li>**lentamente\=**  \- lentamente sonda cada padrão de minutos</li><li>**lentamente\=**  <N> \- sonda rapidamente cada *N* minutos</li></ul></li><li>**\/agora** \(sonda agora\)</li></ul> |
|     \/?     |                                                                                                                                                                                                                                                                                                                            Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="BKMK_Examples"></a>Exemplos  
Para determinar o intervalo de sondagem para replicação de arquivo:  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Para determinar a interface de programa de aplicativo atual do NTFRS \(API\) versão:  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
  
  

