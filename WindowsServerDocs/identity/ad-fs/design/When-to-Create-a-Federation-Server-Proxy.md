---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: Quando criar um proxy do Servidor de Federação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 963070a15b075275cd5680fa3a39044abbb6b3ad
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972033"
---
# <a name="when-to-create-a-federation-server-proxy"></a>Quando criar um proxy do Servidor de Federação

A criação de um proxy de servidor de Federação em sua organização adiciona camadas de segurança adicionais à sua implantação do Serviços de Federação do Active Directory (AD FS) \( AD FS \) . Considere implantar um proxy de servidor de Federação na rede de perímetro da sua organização quando desejar:

-   Impedir que computadores cliente externos acessem diretamente seus servidores de Federação. Ao implantar um proxy de servidor de Federação em sua rede de perímetro, você isola efetivamente os servidores de Federação para que eles possam ser acessados somente por computadores cliente que estão conectados à rede corporativa por meio de proxies de servidor de Federação, que atuam em nome dos computadores cliente externos. Os proxies de servidor de federação não têm acesso às chaves privadas usadas para gerar tokens. Para obter mais informações, consulte [Onde colocar um Proxy do servidor de Federação](Where-to-Place-a-Federation-Server-Proxy.md).

-   Forneça uma maneira conveniente de diferenciar a experiência de entrada \- para usuários que estão vindo da Internet, em oposição aos usuários que vêm da sua rede corporativa usando a autenticação integrada do Windows. Um proxy de servidor de Federação coleta credenciais ou detalhes de realm inicial de computadores cliente da Internet usando as páginas de logon, logoff e descoberta do provedor de identidade \( homerealmdiscovery. aspx \) que são armazenadas no proxy do servidor de Federação.

    Por outro lado, os computadores cliente que vêm da rede corporativa encontram uma experiência diferente, com base na configuração do servidor de Federação. O servidor de Federação de rede corporativa geralmente é configurado para a autenticação integrada do Windows, que fornece uma experiência de conexão direta \- para os usuários na rede corporativa.

A função que um proxy de servidor de federação desempenha na sua organização depende se você coloca o proxy do servidor de Federação na organização do parceiro de conta ou na organização do parceiro de recurso. Por exemplo, quando um proxy de servidor de Federação é colocado na rede de perímetro do parceiro de conta, sua função é coletar as informações de credenciais de usuário de clientes de navegador. Quando um proxy de servidor de Federação é colocado na rede de perímetro do parceiro de recurso, ele retransmite as solicitações de token de segurança para um servidor de Federação de recurso e produz tokens de segurança organizacionais em resposta aos tokens de segurança fornecidos por seus parceiros de conta.

Para obter mais informações, consulte [Review the Role of the Federation Server Proxy in the Account Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) e [Review the Role of the Federation Server Proxy in the Resource Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md).

## <a name="how-to-create-a-federation-server-proxy"></a>Como criar um proxy do servidor de federação
Você pode criar um proxy de servidor de Federação usando o assistente de configuração de proxy de servidor de Federação AD FS ou a ferramenta de linha de comando Fsconfig.exe \- . Para obter instruções sobre como fazer isso, consulte [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).

Para obter mais informações sobre como configurar todos os pré-requisitos necessários para implantar um proxy do servidor de federação, consulte [Checklist: Setting Up a Federation Server Proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
