---
title: Sistema de nomes de domínio (DNS)
description: Este tópico fornece uma visão geral do DNS no Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 23851d2d8015fc6ae9e0653e8a0843f8c4295162
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="domain-name-system-dns"></a>Sistema de nomes de domínio (DNS)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Sistema de nome de domínio (DNS) faz parte do conjunto de protocolos que compõem o TCP/IP padrão da indústria e juntos do cliente DNS e servidor DNS fornecem computador nome to-IP serviços de resolução de nome de mapeamento de endereço para usuários e computadores.  
  
> [!NOTE]  
> Além de neste tópico, o seguinte conteúdo DNS está disponível.  
>   
> -   [O que há de novo no cliente DNS](What-s-New-in-DNS-Client.md)  
> -   [O que há de novo no servidor DNS](What-s-New-in-DNS-Server.md)  
> -   [Guia do cenário de política de DNS](deploy/DNS-Policy-Scenario-Guide.md)  
> -   Vídeo: [Windows Server 2016: gerenciamento de DNS no IPAM](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
No Windows Server 2016, DNS é uma função de servidor que você pode instalar usando comandos de Gerenciador do servidor ou o Windows PowerShell. Se você estiver instalando uma nova floresta do Active Directory e domínio, DNS é automaticamente instalado com o Active Directory como o servidor de Catálogo Global de floresta e domínio.  
  
Os serviços de domínio do Active Directory (AD DS) usa o DNS como seu mecanismo de localização do controlador de domínio. Quando qualquer uma das operações do Active Directory principais é executada, como autenticação, atualizar ou pesquisando, computadores usam DNS para localizar controladores de domínio do Active Directory. Além disso, os controladores de domínio usam DNS para localizar uns aos outros.  
  
O serviço de cliente DNS está incluído em todas as versões de cliente e servidor do sistema operacional Windows e é executado por padrão durante a instalação do sistema operacional. Quando você configurar uma conexão de rede TCP/IP com o endereço IP de um servidor DNS, o cliente DNS consulta o servidor DNS para descobrir controladores de domínio e resolver nomes de computador para endereços IP. Por exemplo, quando um usuário de rede com uma conta de usuário do Active Directory faz logon um domínio do Active Directory, o serviço DNS cliente consulta o servidor DNS para localizar um controlador de domínio para o domínio do Active Directory. Quando o servidor DNS responde à consulta e fornece o endereço IP do controlador de domínio para o cliente, o cliente entra em contato com o controlador de domínio e o processo de autenticação pode começar.  
  
Os serviços de servidor DNS do Windows Server 2016 e cliente DNS usam o protocolo DNS que está incluído no pacote do protocolo TCP/IP. DNS é parte da camada de aplicativo do modelo de referência de TCP/IP, conforme mostrado na ilustração a seguir.  
  
![DNS em TCP/IP](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

