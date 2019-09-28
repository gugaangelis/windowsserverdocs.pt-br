---
title: Pré-requisitos para implantação do DirectAccess
description: Este tópico fornece pré-requisitos para a implantação do DirectAccess no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6e04dbf1576277493ec849c8de82aeab51e97649
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388806"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Pré-requisitos para implantação do DirectAccess

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

A tabela a seguir lista os pré-requisitos necessários para usar os assistentes de configuração do para implantar o DirectAccess.  
  
|||  
|-|-|  
|Cenário|Pré-requisitos|  
|[Implantar um único servidor DirectAccess usando o assistente de Introdução](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-O Firewall do Windows deve ser habilitado em todos os perfis<br /><br />-Suporte apenas para clientes que executam o Windows 10 @ no__t-0, <br />              Windows @ no__t-0 8 e Windows @ no__t-1 8,1 Enterprise.<br /><br />-Uma infraestrutura de chave pública não é necessária.<br /><br />-Não há suporte para a implantação da autenticação de dois fatores. Credenciais de domínio são necessárias para autenticação.<br /><br />– Implanta automaticamente o DirectAccess em todos os computadores móveis no domínio atual.<br /><br />-O tráfego para a Internet não passa pelo DirectAccess. Não há suporte para forçar configuração de túnel.<br /><br />-O servidor DirectAccess é o servidor de local de rede.<br /><br />-Não há suporte para NAP (proteção de acesso à rede).<br /><br />-Não há suporte para a alteração de políticas usando um recurso diferente do console de gerenciamento do DirectAccess ou cmdlets do Windows PowerShell.<br /><br />-Para uma configuração multissite, agora ou no futuro, primeiro siga as diretrizes em [implantar um único servidor DirectAccess com configurações avançadas](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|  
|[Implantar um único servidor do DirectAccess com configurações avançadas](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-Uma infraestrutura de chave pública deve ser implantada.<br />    Para obter mais informações, consulte o guia de laboratório de [Test: PKI básica para Windows Server 2012 @ no__t-0.<br /><br />-O Firewall do Windows deve ser habilitado em todos os perfis.<br /><br />Os sistemas operacionais de servidor a seguir dão suporte ao DirectAccess.<br /><br />-Você pode implantar todas as versões do Windows Server 2016 como um cliente DirectAccess ou um servidor DirectAccess.<br />-Você pode implantar todas as versões do Windows Server 2012 R2 como um cliente DirectAccess ou um servidor DirectAccess.<br />-Você pode implantar todas as versões do Windows Server 2012 como um cliente DirectAccess ou um servidor DirectAccess.<br />-Você pode implantar todas as versões do Windows Server 2008 R2 como um cliente DirectAccess ou um servidor DirectAccess.<br /><br />Os sistemas operacionais cliente a seguir dão suporte ao DirectAccess.<br /><br />-Windows 10 @ no__t-0 Enterprise<br />-Windows 10 @ no__t-0 Enterprise 2015 Branch de Manutenção em Longo Prazo (LTSB)<br />-Windows @ no__t-0 8 e 8,1 Enterprise<br />-Windows @ no__t-0 7 Ultimate<br />-Windows @ no__t-0 7 Enterprise<br /><br />-Não há suporte para a configuração de túnel forçado com a autenticação KerbProxy.<br /><br />-Não há suporte para a alteração de políticas usando um recurso diferente do console de gerenciamento do DirectAccess ou cmdlets do Windows PowerShell.<br /><br />-Não há suporte para a separação de funções de servidor NAT64/DNS64 e IPHTTPS em outro servidor.|  
  


