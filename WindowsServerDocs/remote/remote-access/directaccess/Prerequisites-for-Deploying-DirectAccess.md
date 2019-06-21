---
title: Pré-requisitos para implantação do DirectAccess
description: Este tópico fornece os pré-requisitos para implantação do DirectAccess no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f77df438ba2b282101a031b4ff4d145286cf3e94
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283620"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Pré-requisitos para implantação do DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

A tabela a seguir lista os pré-requisitos necessários para usar os assistentes de configuração para implantar o DirectAccess.  
  
|||  
|-|-|  
|Cenário|Pré-requisitos|  
|[Implantar um único servidor DirectAccess usando o Assistente de Introdução](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-Windows Firewall deve ser habilitado em todos os perfis<br /><br />-Só tem suporte para clientes que executam o Windows 10&reg;, <br />              Windows&reg; 8 e Windows&reg; 8.1 Enterprise.<br /><br />-Uma infraestrutura de chave pública não é necessária.<br /><br />-Não há suportada para a implantação de autenticação de dois fatores. Credenciais de domínio são necessárias para autenticação.<br /><br />-Implanta o DirectAccess automaticamente para todos os computadores móveis no domínio atual.<br /><br />-O tráfego para a Internet não passa por meio do DirectAccess. Não há suporte para forçar configuração de túnel.<br /><br />-Servidor do DirectAccess é o servidor de local de rede.<br /><br />-Não há suporte para a proteção de acesso rede (NAP).<br /><br />-Não há suporte para alteração de políticas por meio de um recurso diferente do console de gerenciamento do DirectAccess ou os cmdlets do Windows PowerShell.<br /><br />– Para uma configuração multissite, agora ou no futuro, primeiro siga as diretrizes [implantar um único servidor DirectAccess com configurações avançadas](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|  
|[Implantar um único servidor do DirectAccess com configurações avançadas](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-Uma infraestrutura de chave pública deve ser implantada.<br />    Para obter mais informações, consulte [minido módulo guia de laboratório de teste: PKI básico para Windows Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx).<br /><br />-Windows Firewall deve ser habilitado em todos os perfis.<br /><br />Os seguintes sistemas operacionais de servidor oferecem suporte ao DirectAccess.<br /><br />-Você pode implantar todas as versões do Windows Server 2016 como um cliente DirectAccess ou um servidor DirectAccess.<br />-Você pode implantar todas as versões do Windows Server 2012 R2 como um cliente DirectAccess ou um servidor DirectAccess.<br />-Você pode implantar todas as versões do Windows Server 2012 como um cliente DirectAccess ou um servidor DirectAccess.<br />-Você pode implantar todas as versões do Windows Server 2008 R2 como um cliente DirectAccess ou um servidor DirectAccess.<br /><br />Os seguintes sistemas operacionais de cliente oferecem suporte ao DirectAccess.<br /><br />-   Windows 10&reg; Enterprise<br />-Windows 10&reg; Enterprise 2015 o longo prazo (LTSB) do Branch de manutenção<br />-Windows&reg; 8 e 8.1 Enterprise<br />-Windows&reg; 7 Ultimate<br />-   Windows&reg; 7 Enterprise<br /><br />-Não há suporte para a configuração de túnel force com autenticação KerbProxy.<br /><br />-Não há suporte para alteração de políticas por meio de um recurso diferente do console de gerenciamento do DirectAccess ou os cmdlets do Windows PowerShell.<br /><br />-Não há suporte para separação NAT64 e o DNS64 e funções de servidor IPHTTPS em outro servidor.|  
  


