---
title: Pré-requisitos para implantação do DirectAccess
description: Este tópico fornece pré-requisitos para a implantação do DirectAccess no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f1fdae2625245675c2df7b5c5480fe5b08732b4a
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87518242"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Pré-requisitos para implantação do DirectAccess

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

A tabela a seguir lista os pré-requisitos necessários para usar os assistentes de configuração do para implantar o DirectAccess.

|Cenário|Pré-requisitos|
|-|-|
|[Implantar um único servidor DirectAccess usando o assistente de Introdução](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-O Firewall do Windows deve ser habilitado em todos os perfis<p>-Suporte apenas para clientes que executam o Windows 10 &reg; , <br />              Windows &reg; 8 e windows &reg; 8,1 Enterprise.<p>-Uma infraestrutura de chave pública não é necessária.<p>-Não há suporte para a implantação da autenticação de dois fatores. Credenciais de domínio são necessárias para autenticação.<p>– Implanta automaticamente o DirectAccess em todos os computadores móveis no domínio atual.<p>-O tráfego para a Internet não passa pelo DirectAccess. Não há suporte para forçar configuração de túnel.<p>-O servidor DirectAccess é o servidor de local de rede.<p>-Não há suporte para NAP (proteção de acesso à rede).<p>-Não há suporte para a alteração de políticas usando um recurso diferente do console de gerenciamento do DirectAccess ou cmdlets do Windows PowerShell.<p>-Para uma configuração multissite, agora ou no futuro, primeiro siga as diretrizes em [implantar um único servidor DirectAccess com configurações avançadas](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|
|[Implantar um único servidor do DirectAccess com configurações avançadas](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-Uma infraestrutura de chave pública deve ser implantada.<br /> Para obter mais informações, consulte [Guia de laboratório de teste mini-Module: PKI básica para Windows Server 2012](https://docs.microsoft.com/answers/topics/windows-server-2012.html).<p>-O Firewall do Windows deve ser habilitado em todos os perfis.<p>Os sistemas operacionais de servidor a seguir dão suporte ao DirectAccess.<p>-Você pode implantar todas as versões do Windows Server 2016 como um cliente DirectAccess ou um servidor DirectAccess.<br />-Você pode implantar todas as versões do Windows Server 2012 R2 como um cliente DirectAccess ou um servidor DirectAccess.<br />-Você pode implantar todas as versões do Windows Server 2012 como um cliente DirectAccess ou um servidor DirectAccess.<br />-Você pode implantar todas as versões do Windows Server 2008 R2 como um cliente DirectAccess ou um servidor DirectAccess.<p>Os sistemas operacionais cliente a seguir dão suporte ao DirectAccess.<p>-Windows 10 &reg; Enterprise<br />-Windows 10 &reg; Enterprise 2015 Branch de manutenção em longo prazo (LTSB)<br />-Windows &reg; 8 e 8,1 Enterprise<br />-Windows &reg; 7 Ultimate<br />-Windows &reg; 7 Enterprise<p>-Não há suporte para a configuração de túnel forçado com a autenticação KerbProxy.<p>-Não há suporte para a alteração de políticas usando um recurso diferente do console de gerenciamento do DirectAccess ou cmdlets do Windows PowerShell.<p>-Não há suporte para a separação de funções de servidor NAT64/DNS64 e IPHTTPS em outro servidor.|



