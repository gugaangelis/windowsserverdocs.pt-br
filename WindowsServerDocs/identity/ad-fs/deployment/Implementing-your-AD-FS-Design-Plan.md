---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: Implementando o AD FS elaborar o plano
description: 
author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 19cfe06a4dcf11e525d31865673e2f5c522cbb7a
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="implementing-your-ad-fs-design-plan"></a>Implementando o AD FS elaborar o plano

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

As seguintes condições ambientais e os requisitos são fatores importantes na implementação do seu plano de design de \(AD FS\) de serviços de Federação do Active Directory:  
  
-   **Suporte para parceiros:** você geralmente usa AD FS para trabalhar com organizações parceiras. Para estabelecer federação de identidade, determine as organizações com o qual você deseja formam uma parceria. Após uma implantação de linha de base AD FS no lugar, operando com parceiros envolve a adição de parceiros, excluindo parceiros e atualizar informações de parceiros. Alterações em parcerias podem ocorrer por vários motivos: Por exemplo, a implantação do AD FS pode exigir atualizações de parceria se seu parceiro altera seus negócios significativamente, sua organização se torna parte de uma organização maior ou um grupo de empresas ou sua organização é adquirida por uma empresa diferente. Em qualquer cenário em que você interaja identidades de vários domínios, você precisará saber o \(partners\) domínios que atualmente com suporte e todos os domínios adicionais que representam os parceiros potenciais.  
  
-   **Suporte para o aplicativo e tipos de serviço:** alguns aplicativos e serviços exigem acesso aos recursos do sistema operacional, enquanto outras são "declarações cientes". É importante compreender os tipos de aplicativos e serviços compatíveis com o AD FS para que você pode formular requisitos de administração.  
  
-   **Diagramas de arquitetura lógicos e físicos ou topologia de implantação:** você precisará saber:  
  
    -   Se os servidores de Federação funcionarão em um conjunto de servidores farm ou em um único servidor.  
  
    -   Onde sua rede implanta firewalls e proxies.  
  
    -   A localização dos recursos e se os usuários acessam recursos de dentro da organização, fora da organização, ou ambos.  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Como implementar seu design de AD FS usando este guia  
A próxima etapa em implementar seu design é determinar em que ordem cada tarefa de implantação deve ser realizada. Este guia usa as listas de verificação para ajudá-lo a percorrer as tarefas de implantação vários para servidores e aplicativos que são necessárias para implementar o seu plano de design. Listas de verificação de pai e filho são usadas conforme necessário para representar a ordem na quais tarefas para um específico do AD FS design deve ser processado.  
  
Use as seguintes listas de verificação de pai nesta seção do guia para se familiarizar com as tarefas de implantação para implementar o design de AD FS preferencial da sua organização:  
  
-   [Lista de verificação: Implementando um Design SSO da Web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Lista de verificação: Implementando um Design SSO da Web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
