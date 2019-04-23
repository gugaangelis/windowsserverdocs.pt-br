---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: Quando criar um proxy do Servidor de Federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1f0253dfb5a690371dae1a2bfcb6b7520077d473
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883197"
---
# <a name="when-to-create-a-federation-server-proxy"></a>Quando criar um proxy do Servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Criar um proxy do servidor de federação na sua organização adiciona camadas extras de segurança para seus serviços de Federação do Active Directory \(do AD FS\) implantação. Considere a implantação de um proxy do servidor de federação na rede de perímetro da sua organização quando você deseja:  
  
-   Impedir que computadores cliente externos acessem diretamente os servidores de Federação. Ao implantar um proxy do servidor de federação na rede de perímetro, você efetivamente isolar seus servidores de federação para que eles podem ser acessados somente por computadores cliente que estão conectados a rede corporativa por meio de proxies de servidor de federação, que atuam em nome os computadores cliente externo. Os proxies de servidor de federação não têm acesso às chaves privadas usadas para gerar tokens. Para obter mais informações, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   Fornecem uma maneira conveniente de diferenciar o sinal\-na experiência para usuários que são provenientes da Internet em oposição aos usuários originados em sua rede corporativa usando a autenticação integrada do Windows. Um proxy do servidor de Federação coleta credenciais ou detalhes de realm inicial de computadores cliente da Internet usando a descoberta de provedor de logon, logoff e identidade \(homerealmdiscovery\) páginas que são armazenadas na federação proxy do servidor.  
  
    Por outro lado, os computadores cliente que são provenientes de encontram da rede corporativa uma experiência diferente, com base na configuração do servidor de Federação. O servidor de Federação da rede corporativa é geralmente configurado para autenticação integrada do Windows, que fornece um sinal contínuo\-na experiência dos usuários na rede corporativa.  
  
A função desempenhada por um proxy do servidor de federação na sua organização depende se você colocar o proxy do servidor de federação na organização do parceiro de conta ou na organização do parceiro de recurso. Por exemplo, quando um proxy do servidor de Federação é colocado na rede de perímetro do parceiro de conta, sua função é coletar as informações de credenciais do usuário de clientes do navegador. Quando um proxy do servidor de Federação é colocado na rede de perímetro do parceiro de recurso, ele retransmite solicitações para um servidor de Federação do recurso de token de segurança e gera tokens de segurança organizacionais em resposta aos tokens de segurança que são fornecidos pelo seu parceiros de conta.  
  
Para obter mais informações, consulte [Review the Role of the Federation Server Proxy in the Account Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) e [Review the Role of the Federation Server Proxy in the Resource Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md).  
  
## <a name="how-to-create-a-federation-server-proxy"></a>Como criar um proxy do servidor de federação  
Você pode criar um proxy de servidor de Federação usando o Assistente de configuração do AD FS Federation Server Proxy ou o comando Fsconfig.exe\-ferramenta de linha. Para obter instruções sobre como fazer isso, consulte [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
Para obter informações gerais sobre como configurar todos os pré-requisitos necessários para implantar um proxy do servidor de federação, consulte [lista de verificação: Como configurar um Proxy do servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
