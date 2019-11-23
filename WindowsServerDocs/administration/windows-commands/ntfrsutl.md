---
title: ntfrsutl
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f1301b6876698e9eb552ae0ef9e70ed278319a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372616"
---
# <a name="ntfrsutl"></a>ntfrsutl

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Despeja as tabelas internas, thread e informações de memória para o serviço de replicação de arquivo NT \(NTFRS\). Ele é executado em servidores locais e remotos. A configuração de recuperação para o NTFRS no Gerenciador de controle de serviço \(SCM\) pode ser essencial para localizar e manter eventos de log importantes no computador. Essa ferramenta fornece um método conveniente para revisar essas configurações.   
  
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
|   idtable   |                                                                                                                                                                                                                                                                                                                                          Tabela de ID                                                                                                                                                                                                                                                                                                                                          |
| ConfigTable |                                                                                                                                                                                                                                                                                                                                  Tabela de configuração do FRS                                                                                                                                                                                                                                                                                                                                   |
|    inlog    |                                                                                                                                                                                                                                                                                                                                        Log de entrada                                                                                                                                                                                                                                                                                                                                         |
|   outlog    |                                                                                                                                                                                                                                                                                                                                        Log de saída                                                                                                                                                                                                                                                                                                                                        |
| <computer>  |                                                                                                                                                                                                                                                                                                                                  Especifica o computador.                                                                                                                                                                                                                                                                                                                                   |
|   memória    |                                                                                                                                                                                                                                                                                                                                        Uso de memória                                                                                                                                                                                                                                                                                                                                        |
|   threads   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|    preparar    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|     AD      |                                                                                                                                                                                                                                                                                                                         lista o modo de exibição do serviço NTFRS do DS.                                                                                                                                                                                                                                                                                                                          |
|    Defina     |                                                                                                                                                                                                                                                                                                                             Especifica os conjuntos de réplica ativas                                                                                                                                                                                                                                                                                                                              |
|   versão   |                                                                                                                                                                                                                                                                                                                       Especifica a API e as versões do serviço NTFRS.                                                                                                                                                                                                                                                                                                                        |
|    Sondagem     | Especifica os intervalos de sondagem atuais.<br /><br />Parâmetros:<br /><br /><ul><li>**\/rapidamente**\[ **\=** \[ <N>\]\]\(pesquisas rapidamente\)<br /><br /><ul><li>**rapidamente** \- sondagens rapidamente até que a configuração estável seja rectrieved</li><li>**\=rapidamente** \- pesquisas rapidamente a cada minutos padrão.</li><li>**\=rapidamente**<N> \- pesquisas rapidamente a cada *N* minutos</li></ul></li><li>**\/lentamente**\[ **\=** \[ <N>\]\] \(as pesquisas lentamente\)<br /><br /><ul><li>**lentamente** \- sondagens lentamente até que a configuração estável seja recuperada</li><li>**\=lentamente** \- sondagens lentamente a cada minutos padrão</li><li>**\=lentamente**<N> \- pesquisas rapidamente a cada *N* minutos</li></ul></li><li>**agora\/** \(sondagens agora\)</li></ul> |
|     \/?     |                                                                                                                                                                                                                                                                                                                            Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="BKMK_Examples"></a>Disso  
Para determinar o intervalo de sondagem para a replicação de arquivo:  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Para determinar a interface do programa de aplicativo do NTFRS \(versão\) da API:  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>referências adicionais  
  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
  
  

