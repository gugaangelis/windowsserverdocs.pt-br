---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: Escopo de autoridade do administrador do serviço
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b5bf2fb3b06a47d730b9dd124b2b66a0a4c9c691
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864847"
---
# <a name="service-administrator-scope-of-authority"></a>Escopo de autoridade do administrador do serviço

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você optar por participar de uma floresta do Active Directory, você deve confiar que o proprietário da floresta e os administradores de serviço. Os proprietários da floresta serão responsáveis por selecionar e gerenciar os administradores de serviço; Portanto, quando você confia em um proprietário da floresta, você também confia os administradores de serviço que gerencia o proprietário da floresta. Esses administradores de serviço tem acesso a todos os recursos na floresta. Antes de tomar a decisão de participar de uma floresta, é importante entender que o proprietário da floresta e os administradores de serviço terá acesso completo aos seus dados. Você não pode impedir que esse acesso.  
  
Todos os administradores de serviço em uma floresta tem controle total sobre todos os dados e serviços em todos os computadores da floresta. Os administradores de serviço têm a capacidade de fazer o seguinte:  
  
-   Corrija os erros nas listas de controle de acesso (ACLs) de objetos. Isso permite que o administrador de serviço para ler, modificar ou excluir objetos, independentemente das ACLs que são definidas nesses objetos.  
  
-   Modifica o software do sistema em um controlador de domínio para ignorar as verificações de segurança normal. Isso permite que o administrador de serviço para exibir ou manipular qualquer objeto no domínio, independentemente da ACL no objeto.  
  
-   Use a política de segurança grupos restritos para conceder a qualquer usuário ou grupo acesso administrativo a qualquer computador ingressado no domínio. Dessa forma, os administradores de serviço podem obter o controle de qualquer computador ingressado no domínio, independentemente das intenções do proprietário do computador.  
  
-   Redefinir senhas ou alterar as associações de grupo para usuários.  
  
-   Obter acesso a outros domínios na floresta, modificando o software do sistema em um controlador de domínio. Os administradores de serviço podem afetar a operação de qualquer domínio na floresta, modo de exibição ou manipular dados de configuração de floresta, exibir ou manipular os dados armazenados em qualquer domínio e exibir ou manipular os dados armazenados em qualquer computador adicionado à floresta do.  
  
Por esse motivo, os grupos que armazenam dados em unidades organizacionais (OUs) na floresta e que ingressar computadores em uma floresta devem confiar os administradores de serviço. Para um grupo ingressar em uma floresta, ele deve optar por confiar em todos os administradores de serviço na floresta. Isso envolve garantir que:  
  
-   O proprietário da floresta pode ser confiável para assumir os interesses do grupo e não tem motivo para agir de forma mal-intencionada contra o grupo.  
  
-   O proprietário da floresta adequadamente restringe o acesso físico aos controladores de domínio. Controladores de domínio em uma floresta não podem ser isolados uns dos outros. É possível que um invasor com acesso físico a um controlador de domínio único para fazer alterações offline para o banco de dados de diretório e, ao fazer isso, interferir na operação de qualquer domínio na floresta, exibir ou manipular os dados armazenados em qualquer lugar na floresta e exibir ou manipular os dados armazenados em qualquer computador que ingressou na floresta. Por esse motivo, o acesso físico aos controladores de domínio deve ser restrito ao pessoal autorizado.  
  
-   Compreender e aceitar o risco potencial que os administradores de serviço podem ser forçados em comprometer a segurança do sistema de confiança.  
  
Alguns grupos podem determinar que os benefícios de colaboração e economia de custo de participar de uma infraestrutura compartilhada superam os riscos que os administradores de serviço serão usar indevidamente ou serão ser forçados em uso indevido sua autoridade. Esses grupos podem compartilhar uma floresta e use UOs para delegar autoridade. No entanto, outros grupos não podem aceitar esse risco porque as consequências de um comprometimento da segurança são muito graves. Esses grupos requerem florestas separadas.  
  


