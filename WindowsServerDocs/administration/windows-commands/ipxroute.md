---
title: ipxroute
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bd5f33766ff9b33c9d6020b7284f2fbf9552d44d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375333"
---
# <a name="ipxroute"></a>ipxroute

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe e modifica informações sobre as tabelas de roteamento usadas pelo protocolo IPX. Usado sem parâmetros, o **ipxroute** exibe as configurações padrão para pacotes que são enviados a endereços desconhecidos, de difusão e multicast.   
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
|servidores [/Type = X]|Exibe a tabela de ponto de acesso de serviço (SAP) para o tipo de servidor especificado.  **X** deve ser um número inteiro. Por exemplo, **/Type = 4** exibe todos os servidores de arquivos. Se você não especificar **/Type**, o **ipxroute** servers exibirá todos os tipos de servidores, listando-os por nome do servidor.|  
|Rede ripout|Descobre se a *rede* pode ser acessada consultando a tabela de rotas da pilha IPX e enviando uma solicitação RIP, se necessário.  *Rede* é o número de segmento de rede IPX.|  
|resolver {nome&#124; do GUID} {&#124; GUID AdapterName}|Resolve o nome do GUID para seu nome amigável ou o nome amigável para seu GUID.|  
|quadro = *N*|Especifica o adaptador de rede para o qual consultar ou definir parâmetros.|  
|particular|Envia pacotes para a difusão todas as rotas. Se um pacote for transmitido para um endereço MAC (cartão de acesso de mídia) exclusivo que não esteja na tabela de roteamento de origem, o **ipxroute** enviará o pacote para a difusão de rotas únicas por padrão.|  
|gbr|Envia pacotes para a difusão todas as rotas. Se um pacote for transmitido para o endereço de difusão (FFFFFFFFFFFF), o **ipxroute** enviará o pacote para a difusão de rotas únicas por padrão.|  
|MBR|Envia pacotes para a difusão todas as rotas. Se um pacote for transmitido para um endereço de multicast (C000xxxxxxxx), o **ipxroute** enviará o pacote para a difusão de rotas únicas por padrão.|  
|Remover = *xxxxxxxxxxxx*|Remove o endereço de nó fornecido da tabela de roteamento de origem.|  
|configuração|Exibe informações sobre todas as associações para as quais o IPX está configurado.|  
|/?|Exibe a ajuda no prompt de comando.|  
## <a name="BKMK_Examples"></a>Disso  
Para exibir os segmentos de rede aos quais a estação de trabalho está anexada, o endereço do nó da estação de trabalho e o tipo de quadro que está sendo usado, digite:  
```  
ipxroute config  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
