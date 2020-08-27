---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: Apêndice A-revisando os principais termos de AD DS
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2e899b3f61a23e29a3ecd6a312e50b596ce4e172
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941236"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Apêndice A: Examinar os principais termos do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os termos a seguir são relevantes para o processo de implantação do Windows Server 2008 Active Directory Domain Services (AD DS).

## <a name="active-directory-domain"></a>Domínio do Active Directory
Uma unidade administrativa em uma rede de computador que, para conveniência de gerenciamento, agrupa vários recursos, incluindo o seguinte:

-   **Identidade de usuário em toda a rede**. Em domínios, as identidades de usuário podem ser criadas uma vez e, em seguida, referenciadas em qualquer computador que tenha ingressado na floresta em que o domínio está localizado. Controladores de domínio que compõem uma conta de usuário de armazenamento de domínio e credenciais de usuário, como senhas ou certificados, com segurança.

-   **Autenticação**. Os controladores de domínio fornecem serviços de autenticação para usuários. Eles também fornecem dados de autorização adicionais, como associações de grupo de usuários. Os administradores podem usar esses serviços para controlar o acesso aos recursos na rede.

-   **Relações de confiança**. Os domínios estendem os serviços de autenticação para usuários em outros domínios em sua própria floresta por meio de relações de confiança bidirecionais automáticas. Os domínios também estendem os serviços de autenticação para usuários em domínios em outras florestas por meio de relações de confiança de floresta ou relações de confiança externas criadas manualmente.

-   **Administração de política**. Um domínio é um escopo de políticas administrativas, como a complexidade de senha e as regras de reutilização de senha.

-   **Replicação**. Um domínio define uma partição da árvore de diretórios que fornece dados adequados para fornecer os serviços necessários e que são replicados entre controladores de domínio. Dessa forma, todos os controladores de domínio são pares em um domínio e são gerenciados como uma unidade.

## <a name="active-directory-forest"></a>Floresta do Active Directory
Uma coleção de um ou mais domínios Active Directory que compartilham uma estrutura lógica comum, esquema de diretório e configuração de rede, bem como relações de confiança transitivas automáticas e bidirecionais. Cada floresta é uma única instância do diretório e define um limite de segurança.

## <a name="active-directory-functional-level"></a>Active Directory nível funcional
Uma configuração de AD DS que habilita recursos avançados de AD DS em todo o domínio ou toda a floresta.

## <a name="migration"></a>Migração
O processo de mover um objeto de um domínio de origem para um domínio de destino, preservando ou modificando as características do objeto para torná-lo acessível no novo domínio.

## <a name="domain-restructure"></a>Reestruturação de domínio
Um processo de migração que envolve a alteração da estrutura de domínio de uma floresta. Uma reestruturação de domínio pode envolver a consolidação ou a adição de domínios e pode ocorrer entre florestas ou dentro de uma floresta.

## <a name="domain-consolidation"></a>Consolidação de domínio
Um processo de reestruturação que envolve a eliminação de domínios AD DS mesclando seu conteúdo com o conteúdo de outros domínios.

## <a name="domain-upgrade"></a>Atualização de domínio
O processo de atualização do serviço de diretório de um domínio para uma versão posterior do serviço de diretório. Isso inclui a atualização do sistema operacional em todos os controladores de domínio e o aumento do nível funcional AD DS, quando aplicável.

## <a name="in-place-domain-upgrade"></a>Atualização de domínio in-loco
O processo de atualizar os sistemas operacionais de todos os controladores de domínio em um determinado domínio, por exemplo, atualizando o Windows Server 2003 para o Windows Server 2008 e gerando o nível funcional do domínio, se aplicável, deixando os objetos de domínio, como usuários e grupos, em vigor.

## <a name="forest-root-domain"></a>Domínio raiz da floresta
O primeiro domínio criado na floresta Active Directory. Esse domínio é designado automaticamente como o domínio raiz da floresta. Ele fornece a base para a infraestrutura de floresta Active Directory.

## <a name="regional-domain"></a>Domínio regional
Um domínio filho que é criado em uma região geográfica para otimizar o tráfego de replicação.



