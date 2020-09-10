---
title: ipxroute
description: Artigo de referência para o comando ipxroute, que exibe e modifica informações sobre as tabelas de roteamento usadas pelo protocolo IPX.
ms.topic: reference
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dbfc2df421429d4265cbcb425d189c4fd69de99d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634425"
---
# <a name="ipxroute"></a>ipxroute

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe e modifica informações sobre as tabelas de roteamento usadas pelo protocolo IPX. Usado sem parâmetros, o **ipxroute** exibe as configurações padrão para pacotes que são enviados a endereços desconhecidos, de difusão e multicast.

## <a name="syntax"></a>Sintaxe

```
ipxroute servers [/type=x]
ipxroute ripout <network>
ipxroute resolve {guid | name} {GUID | <adaptername>}
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]
ipxroute config
```

### <a name="parameters"></a>Parâmetros
| Parâmetro | Descrição |
| ------- | -------- |
| servidores`[/type=x]` | Exibe a tabela de ponto de acesso de serviço (SAP) para o tipo de servidor especificado. **x** deve ser um número inteiro. Por exemplo, `/type=4` exibe todos os servidores de arquivos. Se você não especificar **/Type**, o `ipxroute servers` exibirá todos os tipos de servidores, listando-os por nome do servidor. |
| resolver `{GUID | name}``{GUID | adaptername}` | Resolve o nome do GUID para seu nome amigável ou o nome amigável para seu GUID. |
| quadro = *n* | Especifica o adaptador de rede para o qual consultar ou definir parâmetros. |
| def | Envia pacotes para a difusão todas as rotas. Se um pacote for transmitido para um endereço MAC (cartão de acesso de mídia) exclusivo que não esteja na tabela de roteamento de origem, o **ipxroute** enviará o pacote para a difusão de rotas únicas por padrão. |
| gbr | Envia pacotes para a difusão todas as rotas. Se um pacote for transmitido para o endereço de difusão (FFFFFFFFFFFF), o **ipxroute** enviará o pacote para a difusão de rotas únicas por padrão. |
| MBR | Envia pacotes para a difusão todas as rotas. Se um pacote for transmitido para um endereço de multicast (C000xxxxxxxx), o **ipxroute** enviará o pacote para a difusão de rotas únicas por padrão. |
| Remover =*xxxxxxxxxxxx* | Remove o endereço de nó fornecido da tabela de roteamento de origem. |
| config | Exibe informações sobre todas as associações para as quais o IPX está configurado. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para exibir os segmentos de rede aos quais a estação de trabalho está anexada, o endereço do nó da estação de trabalho e o tipo de quadro que está sendo usado, digite:

```
ipxroute config
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
