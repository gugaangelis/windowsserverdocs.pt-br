---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: Implementando seu plano de design do AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6150b52030734c57b345aea731302650bcbddbfd
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192131"
---
# <a name="implementing-your-ad-fs-design-plan"></a>Implementando seu plano de design do AD FS

As seguintes condições ambientais e os requisitos são fatores importantes na implementação de seus serviços de Federação do Active Directory \(do AD FS\) plano de design:  
  
-   **Parceiros com suporte:** Normalmente, você usa o AD FS para trabalhar com organizações parceiras. Para estabelecer a federação de identidades, determine as organizações com o qual você deseja formar uma parceria. Depois que uma implantação de linha de base do AD FS está em vigor, operando com parceiros envolve adicionar parceiros, excluindo os parceiros e atualizar informações do parceiro. Alterações em parcerias podem ocorrer por vários motivos. Por exemplo, sua implantação do AD FS pode exigir atualizações de parceria se seu parceiro altera seus negócios significativamente, sua organização se torna parte de uma organização maior ou de uma federação de organizações ou sua organização é adquirida por outro empresa. Em qualquer cenário em que você federar identidades de vários domínios, você precisará saber os domínios \(parceiros\) que atualmente oferecem suporte a você e todos os domínios adicionais que representam possíveis parceiros.  
  
-   **Aplicativos com suporte e tipos de serviço:** Alguns aplicativos e serviços exigem acesso aos recursos do sistema operacional, enquanto outras são "reconhecimento de declaração." É importante entender os tipos de aplicativos e serviços que o AD FS dá suporte para que você pode formular os requisitos de administração.  
  
-   **Diagramas de arquitetura lógicos e físicos ou topologia de implantação:** Você precisará saber:  
  
    -   Se os servidores de Federação funcionará em um conjunto de servidores de farm ou em um único servidor.  
  
    -   Onde sua rede implanta firewalls e proxies.  
  
    -   O local dos recursos e se os usuários estão acessando recursos de dentro de sua organização, fora da organização, ou ambos.  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Como implementar o design do AD FS usando este guia  
A próxima etapa na implementação do seu design é determinar em qual ordem de cada tarefa de implantação deve ser executada. Este guia usa listas de verificação para ajudá-lo a passar pelas várias tarefas de implantação para servidores e aplicativos que são necessárias para implementar seu plano de design. Listas de verificação pai e filho são usadas conforme o necessário para representar a ordem em que as tarefas para um específico do AD FS design deve ser processado.  
  
Use as seguintes listas de verificação pai nesta seção do guia para se familiarizar com as tarefas de implantação para implementar o design preferencial de AD FS da sua organização:  
  
-   [Lista de verificação: Como implementar um design SSO da Web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Lista de verificação: Como implementar um design de SSO da Web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
