---
title: ipxroute
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d995204eea0af776a2084a82411fa95542d1d77a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889087"
---
# <a name="ipxroute"></a>ipxroute

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe e modifica as informações sobre as tabelas de roteamento usada pelo protocolo IPX. Usado sem parâmetros, **ipxroute** exibe as configurações padrão para pacotes que são enviados para endereços multicast, difusão e desconhecidos.   
## <a name="syntax"></a>Sintaxe  
```  
ipxroute servers [/type=X]  
ipxroute ripout <Network>  
ipxroute resolve {guid | name} {GUID | <AdapterName>}  
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]  
ipxroute config  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|servers[ /type=X]|Exibe a tabela de ponto de acesso de serviço (SAP) para o tipo de servidor especificado.  **X** deve ser um inteiro. Por exemplo, **/tipo = 4** exibe todos os servidores de arquivos. Se você não especificar **/Type**, **ipxroute servidores** exibe todos os tipos de servidores, listando-os pelo nome do servidor.|  
|ripout rede|Descobre se *rede* está acessível consultando tabela de rota da pilha IPX e enviando uma solicitação rip se necessário.  *Rede* é o número de segmento de rede IPX.|  
|resolver {GUID&#124; nome de} {GUID&#124; AdapterName}|Resolve o nome de GUID para seu nome amigável ou o nome amigável para seu GUID.|  
|board= *N*|Especifica o adaptador de rede para o qual consultar ou definir parâmetros.|  
|def|Envia pacotes para a transmissão de todas as ROTAS. Se um pacote é transmitido para um endereço exclusivo do cartão de acesso à mídia (MAC) que não está na tabela de roteamento de origem, **ipxroute** envia o pacote para o único difusão ROUTES por padrão.|  
|gbr|Envia pacotes para a transmissão de todas as ROTAS. Se um pacote é transmitido para o endereço de difusão (FFFFFFFFFFFF), **ipxroute** envia o pacote para o único difusão ROUTES por padrão.|  
|mbr|Envia pacotes para a transmissão de todas as ROTAS. Se um pacote é transmitido para um endereço de multicast (C000xxxxxxxx) **ipxroute** envia o pacote para o único difusão ROUTES por padrão.|  
|remove= *xxxxxxxxxxxx*|Remove o endereço do nó determinado da tabela de roteamento de origem.|  
|config|Exibe informações sobre todas as associações para os quais o IPX estiver configurado.|  
|/?|Exibe a ajuda no prompt de comando.|  
## <a name="BKMK_Examples"></a>Exemplos  
Para exibir os segmentos de rede anexado a estação de trabalho, o endereço do nó da estação de trabalho e o tipo de quadro que está sendo usada, digite:  
```  
ipxroute config  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
