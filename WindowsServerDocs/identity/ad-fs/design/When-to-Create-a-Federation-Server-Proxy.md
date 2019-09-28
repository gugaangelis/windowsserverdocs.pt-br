---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: Quando criar um proxy do Servidor de Federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f3325efe7acf8b0b0469489e8d9a42614a5af54a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358853"
---
# <a name="when-to-create-a-federation-server-proxy"></a>Quando criar um proxy do Servidor de Federação

A criação de um proxy de servidor de Federação em sua organização adiciona camadas de segurança adicionais à sua implantação do Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1. Considere implantar um proxy de servidor de Federação na rede de perímetro da sua organização quando desejar:  
  
-   Impedir que computadores cliente externos acessem diretamente seus servidores de Federação. Ao implantar um proxy de servidor de Federação em sua rede de perímetro, você isola efetivamente os servidores de Federação para que eles possam ser acessados somente por computadores cliente que estão conectados à rede corporativa por meio de proxies de servidor de Federação, que atuam em nome dos computadores cliente externos. Os proxies de servidor de federação não têm acesso às chaves privadas usadas para gerar tokens. Para obter mais informações, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   Forneça uma maneira conveniente de diferenciar a experiência de assinatura @ no__t-0in para usuários que estão vindo da Internet, em oposição aos usuários que vêm da sua rede corporativa usando a autenticação integrada do Windows. Um proxy de servidor de Federação coleta credenciais ou detalhes de realm inicial de computadores cliente da Internet usando a descoberta de provedor de logon, logoff e identidade @no__t -0homerealmdiscovery. aspx @ no__t-1 que são armazenadas no proxy do servidor de Federação.  
  
    Por outro lado, os computadores cliente que vêm da rede corporativa encontram uma experiência diferente, com base na configuração do servidor de Federação. O servidor de Federação de rede corporativa geralmente é configurado para autenticação integrada do Windows, que fornece uma experiência de assinatura @ no__t-0in para usuários na rede corporativa.  
  
A função que um proxy de servidor de federação desempenha na sua organização depende se você coloca o proxy do servidor de Federação na organização do parceiro de conta ou na organização do parceiro de recurso. Por exemplo, quando um proxy de servidor de Federação é colocado na rede de perímetro do parceiro de conta, sua função é coletar as informações de credenciais de usuário de clientes de navegador. Quando um proxy de servidor de Federação é colocado na rede de perímetro do parceiro de recurso, ele retransmite as solicitações de token de segurança para um servidor de Federação de recurso e produz tokens de segurança organizacionais em resposta aos tokens de segurança que são fornecidos por seu parceiros de conta.  
  
Para obter mais informações, consulte [revisar a função de Proxy do servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) e [revisar a função de Proxy do servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>Como criar um proxy do servidor de federação  
Você pode criar um proxy de servidor de Federação usando o assistente de configuração de proxy do servidor de Federação AD FS ou a ferramenta fsconfig. exe do comando @ no__t-0line. Para obter instruções sobre como fazer isso, consulte [configurar um computador para a função de Proxy do servidor de Federação](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
Para obter informações gerais sobre como configurar todos os pré-requisitos necessários para implantar um proxy de servidor de Federação, consulte [Checklist: Configurando um proxy de servidor de Federação @ no__t-0.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
