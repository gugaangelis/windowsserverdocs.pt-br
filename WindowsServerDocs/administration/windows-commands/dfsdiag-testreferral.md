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

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica Sistema de Arquivos Distribuído referências a \(DFS @ no__t-1 executando os seguintes testes:  
  
-   Quando você usa o parâmetro DFSpath sem argumentos, esse comando valida que a lista de referências inclui todos os domínios confiáveis.  
  
-   Quando você especifica um domínio, o comando executa uma verificação de integridade dos controladores de domínio \(dfsdiag \/testdcs @ no__t-2 e testa as associações de site e o cache de domínio do host local.  
  
-   Quando você especifica um domínio e \\SYSvol ou \\NETLOGON, além de executar as mesmas verificações de integridade quando especifica um domínio, o comando verifica se a vida útil \(TTL @ no__t-3 de referências de SYSvol ou NETLOGON corresponde ao valor padrão de 900 segundos.  
  
-   Quando você especifica uma raiz de namespace, além de executar as mesmas verificações de integridade quando especifica um domínio, o comando executa uma verificação de configuração de DFS \(dfsdiag \/TestDFSConfig @ no__t-2 e uma verificação de integridade de namespace \(dfsdiag @no__ t-4TestDFSIntegrity @ no__t-5.  
  
-   Quando você especifica uma pasta DFS \(link @ no__t-1, além de executar as mesmas verificações de integridade quando especifica uma raiz de namespace, o comando valida a configuração de site para destinos de pasta \(dfsdiag \/testsites @ no__t-4 e valida a associação do site do host local.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|\/DFSpath: <path for getting referrals>|Esse caminho de DFS pode ser um dos seguintes:<br /><br />-    @ no__t-1blank @ no__t-2: Testa domínios confiáveis.<br />-    @ no__t-1 @ no__t-2Domain: Referências do controlador de domínio.<br />-    @ no__t-1 @ no__t-2Domain @ no__t-3SYSvol: Referências de SYSvol.<br />-    @ no__t-1 @ no__t-2Domain @ no__t-3NETLOGON: Referências de NETLOGON.<br />-    @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5: Referências de raiz de namespace.<br />-    @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7: Pasta DFS \(link @ no__t-1 indicações.|  
|\/Full|Aplicado somente a referências de domínio e raiz. verifica a consistência das informações de associação do site entre o registro e os serviços de domínio Active Directory \(AD DS @ no__t-1.|  
  
## <a name="BKMK_Examples"></a>Disso  
Para TBD, digite:  
  
```  
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace  
```  
  
Para TBD, digite:  
  
```  
dfsdiag /TestReferral /DFSpath:  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  

