---
title: Sistema de nome de domínio (DNS)
description: Este tópico fornece uma visão geral do DNS no Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1117e523e23bfc5415754484385d290eb2375d91
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954092"
---
# <a name="domain-name-system-dns"></a>Sistema de nome de domínio (DNS)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

O DNS (sistema de nomes de domínio) é um dos pacotes padrão do setor de protocolos que compõem TCP/IP e, juntos, o cliente DNS e o servidor DNS fornecem serviços de resolução de nomes de nome de computador para os computadores e usuários.

> [!NOTE]
> Além deste tópico, o conteúdo DNS a seguir está disponível.
>
> -   [O que há de novo no cliente DNS](What-s-New-in-DNS-Client.md)
> -   [O que há de novo no servidor DNS](What-s-New-in-DNS-Server.md)
> -   [Guia de cenários de política de DNS](deploy/DNS-Policy-Scenario-Guide.md)
> -   Vídeo: [Windows Server 2016: gerenciamento de DNS no IPAM](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)

No Windows Server 2016, o DNS é uma função de servidor que você pode instalar usando Gerenciador do Servidor ou comandos do Windows PowerShell. Se você estiver instalando um novo Active Directory floresta e domínio, o DNS será instalado automaticamente com Active Directory como o servidor de catálogo global para a floresta e o domínio.

Active Directory Domain Services (AD DS) usa o DNS como seu mecanismo de localização do controlador de domínio. Quando qualquer uma das principais operações de Active Directory é executada, como autenticação, atualização ou pesquisa, os computadores usam o DNS para localizar Active Directory controladores de domínio. Além disso, os controladores de domínio usam o DNS para localizar um ao outro.

O serviço cliente DNS está incluído em todas as versões de cliente e servidor do sistema operacional Windows e está sendo executado por padrão na instalação do sistema operacional. Quando você configura uma conexão de rede TCP/IP com o endereço IP de um servidor DNS, o cliente DNS consulta o servidor DNS para descobrir controladores de domínio e para resolver nomes de computador para endereços IP. Por exemplo, quando um usuário de rede com uma conta de usuário Active Directory faz logon em um domínio Active Directory, o serviço cliente DNS consulta o servidor DNS para localizar um controlador de domínio para o domínio de Active Directory. Quando o servidor DNS responde à consulta e fornece o endereço IP do controlador de domínio para o cliente, o cliente entra em contato com o controlador de domínio e o processo de autenticação pode começar.

O servidor DNS do Windows Server 2016 e os serviços cliente DNS usam o protocolo DNS que está incluído no pacote de protocolo TCP/IP. O DNS faz parte da camada de aplicativo do modelo de referência TCP/IP, conforme mostrado na ilustração a seguir.

![DNS no TCP/IP](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)


