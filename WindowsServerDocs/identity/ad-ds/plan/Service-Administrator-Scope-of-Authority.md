---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: Escopo de autoridade do administrador do serviço
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ff6a41c83115538e7b0f55ab1dbec0329bd5046d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967723"
---
# <a name="service-administrator-scope-of-authority"></a>Escopo de autoridade do administrador do serviço

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se você optar por participar de uma floresta Active Directory, deverá confiar no proprietário da floresta e nos administradores do serviço. Os proprietários da floresta são responsáveis por selecionar e gerenciar os administradores de serviço; Portanto, ao confiar em um proprietário de floresta, você também confia nos administradores de serviço gerenciados pelo proprietário da floresta. Esses administradores de serviço têm acesso a todos os recursos na floresta. Antes de tomar a decisão de participar de uma floresta, é importante entender que o proprietário da floresta e os administradores de serviço terão acesso completo aos seus dados. Você não pode impedir esse acesso.

Todos os administradores de serviço em uma floresta têm controle total sobre todos os dados e serviços em todos os computadores da floresta. Os administradores de serviço têm a capacidade de fazer o seguinte:

-   Corrija os erros nas listas de controle de acesso (ACLs) dos objetos. Isso permite que o administrador de serviços Leia, modifique ou exclua objetos, independentemente das ACLs definidas nesses objetos.

-   Modifique o software do sistema em um controlador de domínio para ignorar as verificações de segurança normais. Isso permite que o administrador de serviços exiba ou manipule qualquer objeto no domínio, independentemente da ACL no objeto.

-   Use a política de segurança grupos restritos para conceder a qualquer usuário ou grupo acesso administrativo a qualquer computador ingressado no domínio. Dessa forma, os administradores de serviço podem obter o controle de qualquer computador ingressado no domínio, independentemente das intenções do proprietário do computador.

-   Redefinir senhas ou alterar associações de grupo para usuários.

-   Obter acesso a outros domínios na floresta modificando o software do sistema em um controlador de domínio. Os administradores de serviço podem afetar a operação de qualquer domínio na floresta, exibir ou manipular dados de configuração da floresta, exibir ou manipular dados armazenados em qualquer domínio e exibir ou manipular dados armazenados em qualquer computador ingressado na floresta.

Por esse motivo, os grupos que armazenam dados em UOs (unidades organizacionais) na floresta e que unem computadores a uma floresta devem confiar nos administradores de serviço. Para que um grupo ingresse em uma floresta, ele deve optar por confiar em todos os administradores de serviço na floresta. Isso envolve garantir que:

-   O proprietário da floresta pode ser confiável para agir nos interesses do grupo e não tem motivo para agir de forma mal-intencionada contra o grupo.

-   O proprietário da floresta restringe adequadamente o acesso físico aos controladores de domínio. Controladores de domínio em uma floresta não podem ser isolados uns dos outros. É possível que um invasor que tenha acesso físico a um único controlador de domínio faça alterações offline no banco de dados do diretório e, ao fazer isso, interfira na operação de qualquer domínio na floresta, exiba ou manipule dados armazenados em qualquer lugar da floresta e exiba ou manipule dados armazenados em qualquer computador ingressado na floresta. Por esse motivo, o acesso físico aos controladores de domínio deve ser restrito à equipe confiável.

-   Você entende e aceita o risco potencial que os administradores de serviços confiáveis podem impor para comprometer a segurança do sistema.

Alguns grupos podem determinar que os benefícios colaborativos e econômicos de participar de uma infra-estrutura compartilhada superam os riscos que os administradores de serviço usarão indevidos ou serão forçados a usar indevidamente sua autoridade. Esses grupos podem compartilhar uma floresta e usar UOs para delegar autoridade. No entanto, outros grupos podem não aceitar esse risco porque as consequências de um comprometimento na segurança são muito graves. Esses grupos exigem florestas separadas.



