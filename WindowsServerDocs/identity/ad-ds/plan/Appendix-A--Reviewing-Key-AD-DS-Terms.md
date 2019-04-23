---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: Apêndice A - revisando o chave AD DS termos
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5434db4124c471c613f159dec28e27dee70e7086
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852017"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Apêndice a: Revisar os termos do chave do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os termos a seguir são relevantes para o processo de implantação para Windows Server 2008 Active Directory Domain Services (AD DS).  
  
## <a name="active-directory-domain"></a>Domínio do Active Directory  
Uma unidade administrativa em uma rede de computadores que, por questões de conveniência de gerenciamento, agrupa diversos recursos, incluindo o seguinte:  
  
-   **Identidade do usuário de toda a rede**. Em domínios, as identidades de usuário podem ser criadas uma vez e, em seguida, referenciadas em qualquer computador que ingressou na floresta em que o domínio está localizado. Controladores de domínio que constituem um domínio armazenam contas de usuário e credenciais de usuário, como senhas ou certificados, com segurança.  
  
-   **Autenticação**. Controladores de domínio fornecem serviços de autenticação para os usuários. Eles também pode fornecer dados de autorização adicional, como associações de grupo do usuário. Os administradores podem usar esses serviços para controlar o acesso aos recursos na rede.  
  
-   **Relações de confiança**. Domínios estendem os serviços de autenticação para usuários em outros domínios na sua própria floresta por meio de relações de confiança bidirecionais automáticas. Domínios também estendem o serviços de autenticação para usuários em domínios em outras florestas por meio de relações de confiança de floresta ou relações de confiança externas criadas manualmente.  
  
-   **Administração da diretiva**. Um domínio é um escopo de diretrizes administrativas, como as regras de reutilização de senha e a complexidade de senha.  
  
-   **Replicação**. Um domínio define uma partição da árvore de diretório que fornece os dados adequados fornecer os serviços necessários e que são replicados entre os controladores de domínio. Dessa forma, todos os controladores de domínio são pares em um domínio, e eles são gerenciados como uma unidade.  
  
## <a name="active-directory-forest"></a>Floresta do Active Directory  
Uma coleção de um ou mais domínios do Active Directory que compartilham uma estrutura comum de lógica, esquema de diretório e configuração de rede, bem como relações de confiança transitivas, bidirecionais automáticas. Cada floresta é uma instância única do diretório e define um limite de segurança.  
  
## <a name="active-directory-functional-level"></a>Nível funcional do Active Directory  
Uma configuração do AD DS habilita recursos avançados de AD DS de todo o domínio ou floresta.  
  
## <a name="migration"></a>Migração  
O processo de mover um objeto de um domínio de origem para um domínio de destino, ao preservar ou modificar características do objeto para torná-lo acessível no novo domínio.  
  
## <a name="domain-restructure"></a>Reestruturação de domínio  
Um processo de migração que envolve a alteração da estrutura de domínio de uma floresta. Uma reestruturação de domínio pode envolver a consolidação ou a adição de domínios e podem ocorrer dentro de uma floresta ou entre florestas.  
  
## <a name="domain-consolidation"></a>Consolidação de domínio  
Um processo de reestruturação que compreende a eliminação de domínios do AD DS, mesclando seus conteúdos com o conteúdo de outros domínios.  
  
## <a name="domain-upgrade"></a>Atualização de domínio  
O processo de atualização de um domínio, o serviço de diretório para uma versão posterior do serviço de diretório. Isso inclui a atualização do sistema operacional em todos os controladores de domínio e elevar o nível funcional do AD DS, onde aplicável.  
  
## <a name="in-place-domain-upgrade"></a>Domínio de atualização in-loco  
O processo de atualizar os sistemas operacionais de todos os controladores de domínio em um determinado domínio, por exemplo, atualização do Windows Server 2003 para o Windows Server 2008 e elevar o nível funcional do domínio, se aplicável, deixando os objetos de domínio, como os usuários e grupos, em vigor.  
  
## <a name="forest-root-domain"></a>Domínio raiz da floresta  
O primeiro domínio criado na floresta do Active Directory. Este domínio é automaticamente designado como o domínio raiz da floresta. Ele fornece a base para a infra-estrutura da floresta do Active Directory.  
  
## <a name="regional-domain"></a>Domínio regional  
Um domínio filho que é criado em uma região geográfica para otimizar o tráfego de replicação.  
  


