---
title: ntfrsutl
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 698237380d02fb1ceb4e738c6fb4f083dd31aef3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723462"
---
# <a name="ntfrsutl"></a>ntfrsutl

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Despeja as tabelas internas, thread e informações de memória para o serviço \(de replicação de arquivo NT\)NTFRS. Ele é executado em servidores locais e remotos. A configuração de recuperação para NTFRS no Gerenciador \(de controle\) de serviço SCM pode ser essencial para localizar e manter eventos de log importantes no computador. Essa ferramenta fornece um método conveniente para revisar essas configurações.   
  
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
|   Versão   |                                                                                                                                                                                                                                                                                                                       Especifica a API e as versões do serviço NTFRS.                                                                                                                                                                                                                                                                                                                        |
|    sondagem     | Especifica os intervalos de sondagem atuais.<p>Parâmetros:<p><ul><li>**\/sonda rapidamente rapidamente** \[ **\=** \[ <N> \] \] \(  \)<p><ul><li>**sonda rapidamente rapidamente até** que a configuração estável seja rectrieved \-</li><li>sonda rapidamente rapidamente a cada minutos padrão. **\= ** \-</li><li>**pesquisa\= rapidamente a** cada *N minutos* <N> \-</li></ul></li><li>**\/pesquisas lentas** \[ **\=** \[ lentamente <N> \] \] \(\)<p><ul><li>sondagens **lentas** \- lentamente até que a configuração estável seja recuperada</li><li>sondagens **lentas\= ** \- lentamente a cada minutos padrão</li><li>**pesquisas\= lentas** <N> \- rapidamente a cada *N* minutos</li></ul></li><li>** \/agora** \(, sondagens agora\)</li></ul> |
|     \/?     |                                                                                                                                                                                                                                                                                                                            Exibe a ajuda no prompt de comando.                                                                                                                                                                                                                                                                                                                            |
  
## <a name="examples"></a>Exemplos  
Para determinar o intervalo de sondagem para a replicação de arquivo:  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
Para determinar a versão atual da API \(\) da interface do programa de aplicativo do NTFRS:  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
  
  

