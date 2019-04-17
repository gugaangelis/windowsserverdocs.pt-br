---
title: Serviços de domínio do Windows Azure AD e os serviços de área de trabalho remota
description: Saiba como integrar serviços de domínio do Windows Azure AD na sua implantação do RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/02/2017
ms.tgt_pltfrm: na
ms.topic: article
author: christianmontoya
ms.localizationpriority: medium
ms.openlocfilehash: e60cf70f1f91ad87046bedf024fe9afc459075b6
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1614510"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Integre os serviços de domínio do Windows Azure AD sua implantação do RDS

Você pode usar o [Windows Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) em sua implantação de serviços de área de trabalho remota no lugar do Windows Server Active Directory. Azure AD DS permite usar suas identidades do Azure AD existentes com cargas de trabalho do Windows clássicas.

Com o Windows Azure AD DS, você pode: 
- Crie um ambiente do Azure com um domínio local para organizações de nascer em-nuvem. 
- Crie um ambiente do Azure isolado com as identidades mesmas usadas para seu local e o ambiente on-line, sem precisar criar uma conexão VPN ou ExpressRoute em-to-site. 

Quando terminar de integrar o Azure AD DS em sua implantação de área de trabalho remota, sua arquitetura terá uma aparência semelhante a esta:

![Um diagrama de arquitetura mostrando RDS com o Azure AD DS](media/aadds-rds.png)

Para ver como essa arquitetura compara com outros cenários de implantação do RDS, confira [as arquiteturas de serviços de área de trabalho remota](desktop-hosting-logical-architecture.md).

Para obter uma compreensão melhor do Azure AD DS, confira a [Visão geral do Windows Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-overview) e [como decidir se o Azure AD DS é ideal para o caso de uso](/azure/active-directory-domain-services/active-directory-ds-comparison).

Use as informações a seguir para implantar o Windows Azure AD DS com RDS.

## <a name="prerequisites"></a>Pré-requisitos

Antes, você pode trazer suas identidades do Azure AD para usar em uma implantação do RDS, [configure o Azure AD para salvar as senhas com hash de identidades dos usuários](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Nascer em-nuvem organizações não precisam fazer alterações adicionais em seu diretório; No entanto, as organizações do local necessário permitir hashes de senha a ser sincronizado e armazenadas no Azure AD, que pode não ser permitido para algumas organizações. Os usuários terão redefinam suas senhas depois de fazer essa alteração de configuração.

## <a name="deploy-azure-ad-ds-and-rds"></a>Implantar o RDS e o Azure AD DS 
Use as etapas a seguir para implantar o Windows Azure AD DS e RDS.

1. Habilite o [Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started). Observe que o artigo vinculado faz o seguinte:
   - Percorrer criando apropriada grupos do Azure AD para administração de domínio.
   - Realce quando você pode precisar forçar os usuários alterem sua senha para que suas contas possam trabalhar com o Windows Azure AD DS.
   
2. Configurar o RDS. Você pode usar um modelo Azure ou implantar manualmente o RDS.
   - Use o [modelo existente AD](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Certifique-se personalizar o seguinte:
   
      - **Configurações**
         - **Grupo de recursos**: usar o grupo de recursos onde você deseja criar os recursos do RDS.
         > [!NOTE] 
         > Agora, isso deve ser o mesmo grupo de recursos em que a rede virtual do Azure resource manager existe.

         - **Prefixo do rótulo de DNS**: insira a URL que você deseja que os usuários usam para acessar a Web da área de trabalho remota.
         - **Nome de domínio do AD**: digite o nome completo da sua instância do Azure AD, por exemplo, "contoso.onmicrosoft.com" ou "contoso.com".
         - **Nome do anúncio Vnet** e o **Nome de sub-rede da Ad**: insira os mesmos valores que você usou quando você criou a rede virtual do Gerenciador de recursos do Windows Azure. Esta é a sub-rede à qual se conectará os recursos do RDS.
         - **Nome de usuário Admin** e a **Senha de Admin**: insira as credenciais para um usuário de administrador que seja membro do grupo **Administradores de DC AAD** no Azure AD.
   
      - **Modelo**
         - Remover todas as propriedades do **dnsServers**: depois de selecionar **Editar modelo** da página de modelo de início rápido do Azure, procure "dnsServers" e remover a propriedade. 

            Por exemplo, antes de remover a propriedade **dnsServers** :
      
            ![Modelo de início rápido Azure com a propriedade dnsSettings](media/rds-remove-dnssettings-before.png)

            E aqui está o mesmo arquivo após remover a propriedade:

            ![Modelo de início rápido Azure com a propriedade dnsSettings removido](media/rds-remove-dnssettings-after.png)
   
   - [RDS implantar manualmente](rds-deploy-infrastructure.md). 

