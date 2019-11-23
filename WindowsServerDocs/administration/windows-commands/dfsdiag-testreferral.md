---
title: dfsdiag TestReferral
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af22520d2c89f9d9f9d91ea6f43a33f3ff9c57f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378362"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag TestReferral

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica Sistema de Arquivos Distribuído \(referências\) do DFS executando os seguintes testes:  
  
-   Quando você usa o parâmetro DFSpath sem argumentos, esse comando valida que a lista de referências inclui todos os domínios confiáveis.  
  
-   Quando você especifica um domínio, o comando executa uma verificação de integridade dos controladores de domínio \(Dfsdiag \/testdcs\) e testa as associações de site e o cache de domínio do host local.  
  
-   Quando você especifica um domínio e \\SYSvol ou \\NETLOGON, além de executar as mesmas verificações de integridade quando especifica um domínio, o comando verifica se a vida útil \(TTL\) das referências de SYSvol ou NETLOGON corresponde ao valor padrão de 900 segundos.  
  
-   Quando você especifica uma raiz de namespace, além de executar as mesmas verificações de integridade quando especifica um domínio, o comando executa uma verificação de configuração de DFS \(Dfsdiag \/TestDFSConfig\) e uma verificação de integridade de namespace \(Dfsdiag \/TestDFSIntegrity\).  
  
-   Quando você especifica uma pasta DFS \(link\), além de executar as mesmas verificações de integridade quando especifica uma raiz de namespace, o comando valida a configuração de site para destinos de pasta \(Dfsdiag \/testsites\) e valida a associação de site do host local.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|\/DFSpath:<path for getting referrals>|Esse caminho de DFS pode ser um dos seguintes:<br /><br />-   \(\)em branco: testa domínios confiáveis.<br />-   \\domínio \\: referências do controlador de domínio.<br />-   \\\\domínio\\SYSvol: referências de SYSvol.<br />-   \\\\domínio\\NETLOGON: referências NETLOGON.<br />-   \\\\<Domain or server>\\<Namespace Root>: referências de raiz de namespace.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: pasta DFS \(link\) referências.|  
|\/completo|Aplicado somente a referências de domínio e raiz. verifica a consistência das informações de associação do site entre o registro e os serviços de domínio Active Directory \(AD DS\).|  
  
## <a name="BKMK_Examples"></a>Disso  
Para TBD, digite:  
  
```  
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace  
```  
  
Para TBD, digite:  
  
```  
dfsdiag /TestReferral /DFSpath:  
```  
  
## <a name="additional-references"></a>referências adicionais  
  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

