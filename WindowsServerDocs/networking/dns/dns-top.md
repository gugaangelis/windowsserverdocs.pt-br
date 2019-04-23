---
title: Sistema de nome de domínio (DNS)
description: Este tópico fornece uma visão geral do DNS no Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d4ec63e904dd899a3ddc53a59274ad607136edd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870817"
---
# <a name="domain-name-system-dns"></a>Sistema de nome de domínio (DNS)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Sistema de nome de domínio (DNS) é uma do conjunto de protocolos que compõem o TCP/IP do padrão do setor, e juntos, o cliente DNS e servidor DNS fornecem serviços de resolução de nome do computador nome-para endereço IP mapeamento para computadores e usuários.  
  
> [!NOTE]  
> Além deste tópico, o seguinte conteúdo DNS está disponível.  
>   
> -   [O que há de novo no cliente DNS](What-s-New-in-DNS-Client.md)  
> -   [O que há de novo no servidor DNS](What-s-New-in-DNS-Server.md)  
> -   [Guia de cenário de política DNS](deploy/DNS-Policy-Scenario-Guide.md)  
> -   Vídeo: [Windows Server 2016: Gerenciamento de DNS no IPAM](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
No Windows Server 2016, o DNS é uma função de servidor que você pode instalar usando comandos do Gerenciador do servidor ou o Windows PowerShell. Se você estiver instalando uma nova floresta do Active Directory e o domínio, DNS é instalado automaticamente com o Active Directory como o servidor de Catálogo Global para a floresta e domínio.  
  
Os serviços de domínio do Active Directory (AD DS) usa o DNS como seu mecanismo de localização do controlador de domínio. Quando qualquer uma das principais operações do Active Directory é executada, como autenticação, atualização ou pesquisa, computadores usam o DNS para localizar controladores de domínio do Active Directory. Além disso, controladores de domínio usam o DNS para localizar uns aos outros.  
  
O serviço cliente DNS está incluído em todas as versões de cliente e servidor do sistema operacional Windows e é executado por padrão após a instalação do sistema operacional. Quando você configura uma conexão de rede TCP/IP com o endereço IP de um servidor DNS, o cliente DNS consulta o servidor DNS para descobrir os controladores de domínio e para resolver nomes de computador para endereços IP. Por exemplo, quando um usuário de rede com uma conta de usuário do Active Directory faz logon em um domínio do Active Directory, o serviço cliente DNS consulta o servidor DNS para localizar um controlador de domínio para o domínio do Active Directory. Quando o servidor DNS responde à consulta e fornece o endereço IP do controlador de domínio para o cliente, o cliente contata o controlador de domínio e o processo de autenticação pode começar.  
  
Os serviços de servidor DNS do Windows Server 2016 e o cliente DNS usam o protocolo DNS que está incluído no conjunto de protocolos TCP/IP. O DNS é parte da camada de aplicativo do modelo de referência de TCP/IP, conforme mostrado na ilustração a seguir.  
  
![DNS no TCP/IP](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

