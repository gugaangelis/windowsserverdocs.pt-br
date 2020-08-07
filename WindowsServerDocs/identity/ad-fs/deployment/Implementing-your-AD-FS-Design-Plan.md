---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: Implementando seu plano de design do AD FS
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 84e5b1ad8c9491c85627657b7ca651d509f25653
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972203"
---
# <a name="implementing-your-ad-fs-design-plan"></a>Implementando seu plano de design do AD FS

As seguintes condições e requisitos ambientais são fatores importantes na implementação do seu Serviços de Federação do Active Directory (AD FS) \( AD FS \) plano de design:

-   **Parceiros com suporte:** Normalmente, você usa AD FS para trabalhar com organizações parceiras. Para estabelecer a Federação de identidade, determine as organizações com as quais você deseja formar uma parceria. Depois que uma linha de base AD FS implantação está em vigor, a operação com parceiros envolve a adição de parceiros, a exclusão de parceiros e a atualização de informações de parceiros. As alterações nas parcerias podem ocorrer por vários motivos. Por exemplo, sua implantação de AD FS pode exigir atualizações de parceria se seu parceiro alterar sua empresa de forma significativa, sua organização se tornará parte de uma organização maior ou de uma federação de organizações, ou sua organização será adquirida por uma empresa diferente. Em qualquer cenário no qual você federa identidades de vários domínios, você precisará saber os parceiros de \( domínios \) aos quais você tem suporte no momento e todos os domínios adicionais que representam parceiros em potencial.

-   **Tipos de aplicativos e serviços com suporte:** Alguns aplicativos e serviços exigem acesso aos recursos do sistema operacional, enquanto outros são "reconhecimento de declarações". É importante entender os tipos de aplicativos e serviços que AD FS dá suporte para que você possa formular os requisitos de administração.

-   **Diagramas lógicos e de arquitetura física ou topologia de implantação:** Você precisará saber:

    -   Se os servidores de Federação funcionarão em um conjunto de servidores em farm ou em um único servidor.

    -   Onde sua rede implanta firewalls e proxies.

    -   O local dos recursos e se os usuários estão acessando recursos de dentro da sua organização, fora da organização ou ambos.

## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Como implementar seu design de AD FS usando este guia
A próxima etapa na implementação do design é determinar em qual ordem cada tarefa de implantação deve ser executada. Este guia usa listas de verificação para ajudá-lo a passar pelas várias tarefas de implantação para servidores e aplicativos que são necessárias para implementar seu plano de design. As listas de verificação pai e filho são usadas conforme necessário para representar a ordem na qual as tarefas para um design de AD FS específico devem ser processadas.

Use as seguintes listas de verificação pai nesta seção do guia para se familiarizar com as tarefas de implantação para implementar o design de AD FS preferencial de sua organização:

-   [Lista de verificação: Como implementar um design SSO da Web](Checklist--Implementing-a-Web-SSO-Design.md)

-   [Lista de verificação: Como implementar um design de SSO da Web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)
