---
title: Dfsdiag TestReferral
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cd1b87befa8a9cfda5ea27a4ce5a5105ea1a1009
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848017"
---
# <a name="dfsdiag-testreferral"></a>Dfsdiag TestReferral

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica o sistema de arquivos distribuído \(DFS\) referências, executando os seguintes testes:  
  
-   Quando você usa o parâmetro DFSpath sem argumentos, esse comando valida que a lista de referência inclui todos os domínios confiáveis.  
  
-   Quando você especifica um domínio, o comando executa uma verificação de integridade dos controladores de domínio \(dfsdiag \/testdcs\) e testa o associações de site e o cache de domínio do host local.  
  
-   Quando você especifica um domínio e \\SYSvol ou \\NETLOGON, além de realizar a mesma integridade verifica conforme quando você especifica um domínio, o comando verifica se o tempo de vida \(TTL\) de indicações de SYSvol ou NETLOGON corresponde ao valor padrão de 900 segundos.  
  
-   Quando você especifica uma raiz do namespace, além de realizar a mesma integridade verifica conforme quando você especifica um domínio, o comando executa uma verificação de configuração de DFS \(dfsdiag \/TestDFSConfig\) e uma verificação de integridade do namespace \(dfsdiag \/TestDFSIntegrity\).  
  
-   Quando você especifica uma pasta DFS \(link\), além de realizar as mesma verificações de integridade como quando você especifica uma raiz do namespace, o comando valida a configuração do site para destinos de pasta \(dfsdiag \/ testsites\) e valida a associação de site do host local.  
  
  
  
## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|\/DFSpath:<path for getting referrals>|Esse caminho DFS pode ser um dos seguintes:<br /><br />-   \(em branco\): Domínios confiáveis de testes.<br />-   \\\\Domínio: Indicações de controlador de domínio.<br />-   \\\\Domain\\SYSvol: Referências de SYSvol.<br />-   \\\\Domain\\NETLOGON: Referências de NETLOGON.<br />-   \\\\<Domain or server>\\<Namespace Root>: Indicações de raiz do Namespace.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: Pasta DFS \(link\) referências.|  
|\/completo|Aplicada apenas para as referências da raiz e de domínio. verifica a consistência das informações de associação de site entre o registro e os serviços de domínio do Active Directory \(AD DS\).|  
  
## <a name="BKMK_Examples"></a>Exemplos  
Para TBD, digite:  
  
```  
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace  
```  
  
Para TBD, digite:  
  
```  
dfsdiag /TestReferral /DFSpath:  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
  

