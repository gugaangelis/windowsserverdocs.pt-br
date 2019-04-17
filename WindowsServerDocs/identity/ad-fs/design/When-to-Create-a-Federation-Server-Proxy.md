---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: "Quando criar um Proxy de servidor de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1f0253dfb5a690371dae1a2bfcb6b7520077d473
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server-proxy"></a>Quando criar um Proxy de servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Criar um proxy de servidor de Federação em sua organização adiciona camadas de segurança adicional para a implantação de \(AD FS\) de serviços de Federação do Active Directory. Pense em implantar um proxy de servidor de federação na rede do perímetro da sua organização quando quiser:  
  
-   Impedir que os computadores cliente externo acessar diretamente os servidores de Federação. Implantando um proxy de servidor de Federação em sua rede perímetro, efetivamente isolar os servidores de federação para que eles podem ser acessados somente por computadores cliente que estão conectados à rede corporativa por meio de proxies de servidor de federação, que funcionam em nome dos computadores cliente externo. Proxies de servidor de Federação não têm acesso às chaves particulares que são usados para produzir tokens. Para obter mais informações, consulte [onde colocar um Proxy de servidor de Federação](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   Fornecem uma maneira conveniente para diferenciar a experiência de sign\ para os usuários que são provenientes da Internet em vez dos usuários que são provenientes de sua rede corporativa usando a autenticação integrada do Windows. Um proxy de servidor de Federação coleta detalhes de território doméstica ou credenciais de computadores cliente de Internet usando o logon, logout e páginas \(homerealmdiscovery.aspx\) de descoberta de provedor de identidade que são armazenadas no proxy do servidor de Federação.  
  
    Por outro lado, os computadores cliente que vêm de encontro a rede corporativa uma experiência de diferente, com base na configuração do servidor de Federação. O servidor de Federação rede corporativa geralmente está configurado para autenticação integrada do Windows, que fornece uma experiência de sign\ perfeita para os usuários na rede corporativa.  
  
A função que desempenhe um proxy de servidor de federação na sua organização depende se você colocar o proxy do servidor de federação na organização do parceiro de conta ou na organização de parceiros do recurso. Por exemplo, quando um proxy de servidor de Federação é colocado na rede de perímetro do parceiro de conta, sua função é coletar as informações de credenciais do usuário de clientes do navegador. Quando um proxy de servidor de Federação é colocado na rede de perímetro do parceiro de recurso, ele transmite solicitações de token de segurança para um servidor de Federação do recurso e produz tokens de segurança organizacional em resposta aos tokens de segurança que são fornecidos por seus parceiros de conta.  
  
Para obter mais informações, consulte [revisar a função de Proxy do servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) e [revisar a função de Proxy do servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>Como criar um proxy de servidor de Federação  
Você pode criar um proxy de servidor de Federação usando o Assistente de configuração do AD FS federação servidor Proxy ou a ferramenta de linha de command\ Fsconfig.exe. Para obter instruções sobre como fazer isso, consulte [configurar um computador para a função de Proxy do servidor de Federação](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
Para obter informações gerais sobre como configurar todos os pré-requisitos necessários para implantar um proxy de servidor de federação, consulte [lista de verificação: configuração de backup de Proxy de servidor de Federação um](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
