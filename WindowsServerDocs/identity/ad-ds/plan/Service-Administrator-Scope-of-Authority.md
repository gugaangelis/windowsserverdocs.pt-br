---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: "Escopo de serviço de administrador da autoridade"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ba682067b9c7a7b4bf583482abe6470b60b25450
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="service-administrator-scope-of-authority"></a>Escopo de serviço de administrador da autoridade

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você optar por participar em uma floresta do Active Directory, você deve confiar o proprietário de floresta e os administradores de serviços. Os proprietários de floresta serão responsáveis por selecionar e gerenciar os administradores de serviço; Portanto, quando você confia em um proprietário da floresta, você também confiar os administradores de serviços que gerencia o proprietário da floresta. Esses administradores de serviço têm acesso a todos os recursos na floresta. Antes de tomar a decisão de participar em uma floresta, é importante entender que o proprietário de floresta e os administradores de serviços terão acesso completo aos dados. Você não pode impedir que esse acesso.  
  
Todos os administradores de serviços em uma floresta tem controle total sobre todos os dados e serviços em todos os computadores na floresta. Os administradores de serviço têm a capacidade de fazer o seguinte:  
  
-   Corrigir erros em listas de controle de acesso (ACLs) de objetos. Isso permite que o administrador de serviço ler, modificar ou excluir objetos independente as ACLs que são definidas nesses objetos.  
  
-   Modifique o software do sistema em um controlador de domínio para ignorar as verificações de segurança normais. Isso permite que o administrador de serviço exibir ou manipular qualquer objeto no domínio, independentemente da ACL no objeto.  
  
-   Use a política de segurança grupos restritos para conceder a qualquer acesso administrativo de usuário ou grupo para qualquer computador que tenha ingressado no domínio. Dessa forma, os administradores de serviços podem obter o controle de qualquer computador que tenha ingressado no domínio, independentemente das intenções do proprietário do computador.  
  
-   Redefinir senhas ou alterar associações de grupo de usuários.  
  
-   Obter acesso a outros domínios na floresta modificando o software do sistema em um controlador de domínio. Administradores de serviços podem afetar a operação de qualquer domínio na floresta, modo de exibição ou manipular dados de configuração da floresta, exibir ou manipular os dados armazenados em qualquer domínio e exibir ou manipular os dados armazenados em qualquer computador que tenha ingressado em floresta.  
  
Por esse motivo, os grupos que armazenam dados em unidades organizacionais (UOs) na floresta e que computadores de ingresso em uma floresta devem confiar os administradores de serviços. Para um grupo ingressar em uma floresta, ele deve optar por confiar em todos os administradores de serviços na floresta. Isso envolve a garantir que:  
  
-   O proprietário da floresta pode ser confiável para atuar em interesses do grupo e não tiver o motivo para agir de forma mal-intencionada contra o grupo.  
  
-   O proprietário da floresta adequadamente restringe o acesso físico a controladores de domínio. Controladores de domínio em uma floresta não podem ser isolados uns dos outros. É possível que um invasor que tenha acesso físico a um controlador de domínio único para fazer alterações off-line para o banco de dados do diretório e, ao fazer isso, interferir com a operação de qualquer domínio na floresta, modo de exibição ou manipular os dados armazenados em qualquer lugar na floresta e exibir ou manipular os dados armazenados em qualquer computador que tenha ingressado em floresta. Por esse motivo, o acesso físico a controladores de domínio deve ser limitado à equipe confiável.  
  
-   Você entende e aceita o risco que trusted administradores de serviços podem ser forçados para comprometer a segurança do sistema.  
  
Alguns grupos podem determinar os benefícios colaborativos e economia de custo de participação em uma infraestrutura compartilhada superem os riscos que os administradores de serviços serão fará um uso indevido ou ser forçados em uso indevido de sua autoridade. Esses grupos podem compartilhar uma floresta e usar UOs para delegar autoridade. No entanto, outros grupos não podem aceitar esse risco porque as consequências de comprometimento de segurança são muito graves. Esses grupos exigem florestas separadas.  
  


