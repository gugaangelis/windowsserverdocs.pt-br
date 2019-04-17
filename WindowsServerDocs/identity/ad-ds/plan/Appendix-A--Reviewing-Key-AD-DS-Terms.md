---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: "Apêndice A - examinando o chave AD DS termos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ce7c0b639ded498b15200dfd594b3f34d6492dce
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Apêndice a: examinando chave AD DS termos

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os termos a seguir são relevantes para o processo de implantação do Windows Server 2008 Active Directory domínio Services (AD DS).  
  
## <a name="active-directory-domain"></a>Domínio do Active Directory  
Uma unidade administrativa em uma rede de computador que, para fins de praticidade de gerenciamento, agrupa diversos recursos, incluindo o seguinte:  
  
-   **Identidade do usuário de toda a rede**. Nos domínios, identidades do usuário podem ser criadas uma vez e então referenciadas em qualquer computador que tenha ingressado em floresta em que o domínio está localizado. Controladores de domínio que compõem a um domínio armazenam contas de usuário e credenciais do usuário, como senhas ou certificados, com segurança.  
  
-   **Autenticação**. Controladores de domínio fornecem serviços de autenticação para os usuários. Eles também fornecem dados de autorização adicionais, como associações de grupo do usuário. Os administradores podem usar esses serviços para controlar o acesso aos recursos na rede.  
  
-   **Relações de confiança**. Domínios estendem serviços de autenticação para os usuários em outros domínios em sua própria floresta por meio de relações de confiança bidirecionais automáticas. Domínios também estendem serviços de autenticação para os usuários em domínios em outras florestas por meio de relações de confiança de floresta ou relações de confiança externas criadas manualmente.  
  
-   **Administração de política**. Um domínio é um escopo de políticas administrativas, como as regras de reutilização de senha e complexidade de senha.  
  
-   **Replicação**. Um domínio define uma partição da árvore de diretório que fornece dados que seja adequado para fornecer serviços necessários e que são replicados entre controladores de domínio. Dessa forma, todos os controladores de domínio são pares em um domínio, e elas são gerenciadas como uma unidade.  
  
## <a name="active-directory-forest"></a>Floresta do Active Directory  
Uma coleção de um ou mais domínios do Active Directory que compartilham uma estrutura lógica comum, esquema de diretório e a configuração de rede, bem como relações de confiança transitivas automática e bidirecionais. Cada floresta é uma única instância de diretório, e ele define um limite de segurança.  
  
## <a name="active-directory-functional-level"></a>Nível funcional do Active Directory  
Um AD DS definindo que habilita recursos avançados do AD DS de todo o domínio ou floresta.  
  
## <a name="migration"></a>Migração  
O processo de mover um objeto de um domínio de origem para um domínio de destino, enquanto preserva ou modificação de características do objeto para torná-lo acessível no novo domínio.  
  
## <a name="domain-restructure"></a>Reestruturação de domínio  
Um processo de migração que envolve alterar a estrutura de domínio de uma floresta. Uma reestruturação de domínio pode envolver a consolidação ou a adição de domínios e podem ocorrer entre florestas ou em uma floresta.  
  
## <a name="domain-consolidation"></a>Consolidação de domínio  
Um processo reestruturação que compreende a eliminação de domínios do AD DS, mesclando seus conteúdos com o conteúdo de outros domínios.  
  
## <a name="domain-upgrade"></a>Atualização de domínio  
O processo de atualização de um domínio, o serviço de diretório para uma versão mais recente do serviço de diretório. Isso inclui atualizando o sistema operacional em todos os controladores de domínio e aumentando o nível funcional do AD DS quando aplicável.  
  
## <a name="in-place-domain-upgrade"></a>Domínio de atualização in-loco  
O processo de atualizar os sistemas operacionais de todos os controladores de domínio em um domínio, por exemplo, atualização do Windows Server 2003 para o Windows Server 2008 e aumentar o nível funcional do domínio, se aplicável, deixando os objetos de domínio, como os usuários e grupos, no lugar.  
  
## <a name="forest-root-domain"></a>Domínio raiz da floresta  
O primeiro domínio é criado na floresta do Active Directory. Neste domínio automaticamente é designado como domínio raiz da floresta. Ele fornece a base para a infraestrutura de floresta do Active Directory.  
  
## <a name="regional-domain"></a>Domínio regional  
Um domínio filho que é criado em uma região geográfica para otimizar o tráfego de replicação.  
  


