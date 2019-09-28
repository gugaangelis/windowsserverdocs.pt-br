---
title: tcmsetup
description: Saiba como configurar e desabilitar o cliente TAPI.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0c646acef51f06c57f16ec7e5310e3319a11383f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370682"
---
# <a name="tcmsetup"></a>tcmsetup



Configura ou desabilita o cliente TAPI.

## <a name="syntax"></a>Sintaxe

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/q|Impede a exibição de caixas de mensagem.|
|/x|Especifica que os retornos de chamada orientados a conexão serão usados para redes de tráfego pesado em que a perda de pacotes está alta. Quando esse parâmetro for omitido, os retornos de chamada sem conexão serão usados.|
|/c|Obrigatório. Especifica a configuração do cliente.|
|\<Server1 >|Obrigatório. Especifica o nome do servidor remoto que tem os provedores de serviços TAPI que o cliente usará. O cliente usará as linhas e os telefones dos provedores de serviços. O cliente deve estar no mesmo domínio que o servidor ou em um domínio que tenha uma relação de confiança bidirecional com o domínio que contém o servidor.|
|\<Server2 >...|Especifica qualquer servidor adicional ou servidores que estarão disponíveis para esse cliente. Se você especificar uma lista de servidores, use um espaço para separar os nomes de servidor.|
|/d|Limpa a lista de servidores remotos. Desabilita o cliente TAPI impedindo-o de usar os provedores de serviços TAPI que estão nos servidores remotos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local ou deve ter recebido a autoridade apropriada. Se o computador fizer parte de um domínio, é possível que os membros do grupo Administradores de domínio possam executar esse procedimento. Como prática recomendada de segurança, considere o uso de **Executar como** para executar esse procedimento.
-   Para que a TAPI funcione corretamente, você deve executar **tcmsetup** para especificar os servidores remotos que serão usados por clientes TAPI.
-   Antes que um usuário cliente possa usar um telefone ou uma linha em um servidor TAPI, o administrador do servidor de telefonia deve atribuir o usuário ao telefone ou à linha.
-   A lista de servidores de telefonia que é criada por esse comando substitui qualquer lista existente de servidores de telefonia disponíveis para o cliente. Você não pode usar este comando para adicionar à lista existente.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Visão geral do Shell de comando](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[Especificar servidores de telefonia em um computador cliente](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[Atribuir um usuário de telefonia a uma linha ou telefone](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

