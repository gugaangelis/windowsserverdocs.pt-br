---
title: ntfrsutl
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14e718550247b8854073407146456d366d562d2b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837989"
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
  
#### <a name="parameters"></a>Parâmetros  
  
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
|    conjuntos     |                                                                                                                                                                                                                                                                                                                             Especifica os conjuntos de réplica ativas                                                                                                                                                                                                                                                                                                                              |
|   version   |                                                                                                                                                                                                                                                                                                                       Especifica a API e as versões do serviço NTFRS.                                                                                                                                                                                                                                                                                                                        |
|    sondagem     | Especifica os intervalos de sondagem atuais.<p>Parâmetros:<p><ul><li>**\/rapidamente**\[ **\=** \[ <N>\]\]\(pesquisas rapidamente\)<p><ul><li>**rapidamente** \- sondagens rapidamente até que a configuração estável seja rectrieved</li><li>**\=rapidamente** \- pesquisas rapidamente a cada minutos padrão.</li><li>**\=rapidamente**<N> \- pesquisas rapidamente a cada *N* minutos</li></ul></li><li>**\/lentamente**\[ **\=** \[ <N>\]\] \(as pesquisas lentamente\)<p><ul><li>**lentamente** \- sondagens lentamente até que a configuração estável seja recuperada</li><li>**\=lentamente** \- sondagens lentamente a cada minutos padrão</li><li>**\=lentamente**<N> \- pesquisas rapidamente a cada *N* minutos</li></ul></li><li>**agora\/** \(sondagens agora\)</li></ul> |
|     \/?     |                                                                                                                                                                                                                                                                                                                            Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Para determinar o intervalo de sondagem para a replicação de arquivo:  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Para determinar a interface do programa de aplicativo do NTFRS \(versão\) da API:  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
  
  

