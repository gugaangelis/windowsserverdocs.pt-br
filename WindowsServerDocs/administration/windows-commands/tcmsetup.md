---
title: tcmsetup
description: Artigo de referência para o comando TcmSetup, que configura e desabilita o cliente TAPI.
ms.topic: reference
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 315c13310d8059d5553dff9c780fbdd656867a57
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718063"
---
# <a name="tcmsetup"></a>tcmsetup

Configura ou desabilita o cliente TAPI. Para que a TAPI funcione corretamente, você deve executar esse comando para especificar os servidores remotos que serão usados por clientes TAPI.

> [!IMPORTANT]
> Para usar esse comando, você deve ser um membro do grupo **Administradores** no computador local ou deve ter recebido a autoridade apropriada. Se o computador tiver ingressado em um domínio, os membros do grupo **Admins** . do domínio poderão executar esse procedimento. Como prática recomendada de segurança, considere o uso de **Executar como** para executar esse procedimento.

## <a name="syntax"></a>Sintaxe

```
tcmsetup [/q] [/x] /c <server1> [<server2> …]
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /q | Impede a exibição de caixas de mensagem. |
| /x | Especifica que os retornos de chamada orientados a conexão serão usados para redes de tráfego pesado em que a perda de pacotes está alta. Quando esse parâmetro for omitido, os retornos de chamada sem conexão serão usados. |
| /c | Obrigatórios. Especifica a configuração do cliente. |
| `<server1>` | Obrigatórios. Especifica o nome do servidor remoto que tem os provedores de serviços TAPI que o cliente usará. O cliente usará as linhas e os telefones dos provedores de serviços. O cliente deve estar no mesmo domínio que o servidor ou em um domínio que tenha uma relação de confiança bidirecional com o domínio que contém o servidor. |
| `<server2>…` | Especifica qualquer servidor adicional ou servidores que estarão disponíveis para esse cliente. Se você especificar uma lista de servidores, use um espaço para separar os nomes de servidor. |
| /d | Limpa a lista de servidores remotos. Desabilita o cliente TAPI impedindo-o de usar os provedores de serviços TAPI que estão nos servidores remotos. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Antes que um usuário cliente possa usar um telefone ou uma linha em um servidor TAPI, o administrador do servidor de telefonia deve atribuir o usuário ao telefone ou à linha.

- A lista de servidores de telefonia que é criada por esse comando substitui qualquer lista existente de servidores de telefonia disponíveis para o cliente. Você não pode usar este comando para adicionar à lista existente.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Visão geral do Shell de comando](/previous-versions/windows/it-pro/windows-server-2003/cc737438(v=ws.10))

- [Especificar servidores de telefonia em um computador cliente](/previous-versions/windows/it-pro/windows-server-2003/cc759226(v=ws.10))

- [Atribuir um usuário de telefonia a uma linha ou telefone](/previous-versions/windows/it-pro/windows-server-2003/cc736875(v=ws.10))
