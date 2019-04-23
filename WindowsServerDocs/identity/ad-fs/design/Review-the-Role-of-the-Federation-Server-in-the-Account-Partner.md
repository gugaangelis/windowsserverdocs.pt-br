---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Analise a função do proxy do servidor de federação no parceiro de conta
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0914d32e8f24d5e7db0a25c733342c1bde3e0329
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835127"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Analise a função do proxy do servidor de federação no parceiro de conta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um servidor de federação nos serviços de Federação do Active Directory \(do AD FS\) funciona como um emissor de token de segurança. Um servidor de Federação gera declarações com base na conta armazenam valores que residem em um atributo de local e as empacota como tokens de segurança para que os usuários podem acessar diretamente da Web\-browser\-aplicativos baseados em \(usando logon único\-na \(SSO\) \) que são hospedados em uma organização do parceiro de recurso.  
  
> [!NOTE]  
> Quando os usuários acessam aplicativos federados por meio de um navegador da Web, um servidor de Federação emite automaticamente cookies para os usuários para manter seu status de logon para que da Web\-navegador\-com base em aplicativo. Esses cookies incluem declarações para os usuários. Os cookies permitem recursos de SSO para que os usuários não precise inserir credenciais cada vez que visitam da Web diferente\-navegador\-com base em aplicativos no parceiro de recurso.  
  
No design SSO da Web, as organizações com uma rede de perímetro que deseja que os usuários da Internet tenham acesso a aplicativos devem instalar um proxy de servidor de federação na rede de perímetro. No design SSO da Web federado, deve haver pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de conta e pelo menos um servidor de Federação instalado na rede corporativa da organização do parceiro de recurso.  
  
> [!NOTE]  
> Antes de configurar um computador do servidor de federação na organização do parceiro de conta, você primeiro deve ingressar o computador a qualquer domínio na floresta do Active Directory em que o servidor de Federação será usado para autenticar os usuários dessa floresta. Para obter mais informações, consulte [lista de verificação: Configurando um servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
