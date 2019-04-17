---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: "Examine a função do servidor de federação no parceiro de conta"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0914d32e8f24d5e7db0a25c733342c1bde3e0329
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Examine a função do servidor de federação no parceiro de conta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um servidor de federação no serviços de Federação do Active Directory \(AD FS\) funciona como um emissor do token de segurança. Um servidor de Federação gera declarações com base na conta armazenam valores que residem em um atributo local e as empacota como tokens de segurança para que os usuários podem acessar perfeitamente Web\ browser\-aplicativos baseados em \ (usando único sign\ em \(SSO\)\) que são hospedados em uma organização de parceiro de recurso.  
  
> [!NOTE]  
> Quando os usuários acessam federada aplicativos usando um navegador da Web, um servidor de Federação automaticamente problemas de cookies para os usuários a manter seu status de logon para o aplicativo com base em browser\ Web\. Esses cookies incluem declarações para os usuários. Os cookies habilitar os recursos SSO para que os usuários não precisam inserir credenciais cada vez que eles visitam diferentes Web\ browser\-aplicativos baseados no parceiro de recurso.  
  
O design de SSO da Web, as organizações com uma rede de perímetro que deseja que os usuários de Internet tenham acesso a aplicativos devem instalar um proxy de servidor de federação na rede de perímetro. O design de SSO da Web federado, deve haver pelo menos um servidor de Federação instalados na rede corporativa da organização do parceiro de conta e pelo menos um servidor de Federação instalados na rede corporativa da organização do parceiro de recurso.  
  
> [!NOTE]  
> Antes de você pode configurar um computador do servidor de federação na organização do parceiro de conta, primeiro você deve ingressar o computador em qualquer domínio na floresta do Active Directory em que o servidor de Federação será usado para autenticar usuários de floresta. Para obter mais informações, consulte [lista de verificação: configuração de um servidor federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
