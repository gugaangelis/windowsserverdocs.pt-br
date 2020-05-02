---
title: dfsdiag TestReferral
description: Tópico de referência para Dfsdiag TestReferral, que verifica referências de Sistema de Arquivos Distribuído (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b4c616181d367a8a95efe6484f74af0ff88cc5f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719565"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag TestReferral

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica as referências de Sistema de Arquivos Distribuído (DFS) executando os seguintes testes:

- Quando você usa o parâmetro DFSpath sem argumentos, esse comando valida que a lista de referências inclui todos os domínios confiáveis.

- Quando você especifica um domínio, o comando executa uma verificação de integridade dos controladores de domínio (Dfsdiag/testdcs) e testa as associações de site e o cache de domínio do host local.

- Quando você especifica um domínio e \SYSvol ou \NETLOGON, além de executar as mesmas verificações de integridade quando especifica um domínio, o comando verifica se a vida útil (\TTL) das referências de SYSvol ou NETLOGON corresponde ao valor padrão de 900 segundos.

- Quando você especifica uma raiz de namespace, além de executar as mesmas verificações de integridade quando especifica um domínio, o comando executa uma verificação de configuração de DFS (Dfsdiag/TestDFSConfig) e uma verificação de integridade de namespace (Dfsdiag/TestDFSIntegrity).

- Quando você especifica uma pasta DFS (link), além de executar as mesmas verificações de integridade quando especifica uma raiz de namespace, o comando valida a configuração de site para destinos de pasta (Dfsdiag/testsites) e valida a associação de site do host local.

## <a name="syntax"></a>Sintaxe

```
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
| DFSpath<path for getting referrals>|Esse caminho de DFS pode ser um dos seguintes:<p>-   \(em\)branco: testa domínios confiáveis.<br />-   \\\\Domínio: referências do controlador de domínio.<br />-   \\\\Domínio\\SYSVOL: referências de SYSVOL.<br />-   \\\\Doma em\\Netlogon: referências de Netlogon.<br />-   \\\\<Domain or server>\\<Namespace Root>: Referências de raiz de namespace.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: Referências do \(link\) da pasta DFS.|
|/Full|Aplicado somente a referências de domínio e raiz. verifica a consistência das informações de associação do site entre o registro e os serviços \(de domínio\)Active Directory AD DS.|

## <a name="examples"></a>Exemplos

```
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace
```

```
dfsdiag /TestReferral /DFSpath:
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)


