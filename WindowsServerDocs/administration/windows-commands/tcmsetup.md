---
title: tcmsetup
description: Saiba como configurar e desativar o cliente TAPI.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac92c7b793274227bd20e6fa90a4106a32ea0446
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861997"
---
# <a name="tcmsetup"></a>tcmsetup



Define ou desabilita o cliente TAPI.

## <a name="syntax"></a>Sintaxe

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/q|Impede a exibição de caixas de mensagem.|
|/x|Especifica que os retornos de chamada orientados a conexão serão usados para redes de tráfego pesado onde a perda de pacotes é alta. Quando esse parâmetro for omitido, os retornos de chamada sem conexão serão usados.|
|/c|Obrigatório. Especifica a configuração do cliente.|
|\<Server1 >|Obrigatório. Especifica o nome do servidor remoto que tem os provedores de serviços TAPI que o cliente usará. O cliente usará os provedores de serviço linhas e telefones. O cliente deve ser no mesmo domínio que o servidor ou em um domínio que tenha uma relação de confiança bidirecional com o domínio que contém o servidor.|
|\<Server2 >...|Especifica qualquer servidor adicional ou servidores que estarão disponíveis para esse cliente. Se você especificar uma lista de servidores, use um espaço para separar os nomes de servidor.|
|/d|Limpa a lista de servidores remotos. Desabilita o cliente TAPI, impedindo que ele usa os provedores de serviços TAPI que estão nos servidores remotos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local ou deve ter recebido a autoridade apropriada. Se o computador fizer parte de um domínio, é possível que os membros do grupo Administradores de domínio possam executar esse procedimento. Como prática recomendada de segurança, considere o uso de **Executar como** para executar esse procedimento.
-   Em ordem para TAPI funcione corretamente, você deve executar **tcmsetup** para especificar os servidores remotos que serão usados por clientes TAPI.
-   Antes de um usuário do cliente pode usar um telefone ou uma linha em um servidor TAPI, o administrador do servidor de telefonia deve atribuir o usuário para o telefone ou a linha.
-   A lista de servidores de telefonia que é criada por este comando substitui qualquer lista existente de servidores de telefonia disponíveis para o cliente. Você não pode usar esse comando para adicionar à lista existente.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

[Visão geral do shell de comando](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[Especificar servidores de telefonia em um computador cliente](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[Atribuir um usuário de telefonia a uma linha ou telefone](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

