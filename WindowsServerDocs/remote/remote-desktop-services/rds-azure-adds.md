---
title: Azure AD Domain Services e Serviços de Área de Trabalho Remota
description: Saiba como integrar o Azure AD Domain Services à sua implantação do RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/02/2017
ms.tgt_pltfrm: na
ms.topic: article
author: christianmontoya
ms.localizationpriority: medium
ms.openlocfilehash: d3fe370f18df980acaeaa847bd96642b97613f80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387635"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Integrar o Azure AD Domain Services à sua implantação do RDS

Você pode usar o [Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) em sua implantação dos Serviços de Área de Trabalho Remota no lugar do Windows Server Active Directory. O Azure AD DS permite usar suas identidades existentes do Azure AD com cargas de trabalho clássicas do Windows.

Com o Azure AD DS, você pode: 
- Criar um ambiente do Azure com um domínio local para organizações criadas na nuvem. 
- Criar um ambiente do Azure isolado com as mesmas identidades usadas para seu ambiente local e online, sem precisar criar uma VPN site a site ou ExpressRoute. 

Quando você concluir a integração do Azure AD DS à sua implantação da Área de Trabalho Remota, sua arquitetura será parecida com o seguinte:

![Um diagrama de arquitetura mostrando a Área de Trabalho Remota com o Azure AD DS](media/aadds-rds.png)

Para ver como essa arquitetura se compara com outros cenários de implantação da Área de Trabalho Remota, confira [Arquitetura de Serviços de Área de Trabalho Remota](desktop-hosting-logical-architecture.md).

Para entender melhor o funcionamento do Azure AD DS, confira a [Visão geral do Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-overview) e [Como decidir se o Azure AD DS é adequado para seu caso de uso](/azure/active-directory-domain-services/active-directory-ds-comparison).

Use as informações a seguir para implantar o Azure AD DS com a Área de Trabalho Remota.

## <a name="prerequisites"></a>Pré-requisitos

Antes que possa trazer identidades do Azure AD para usar em uma implantação de Área de Trabalho Remota, [configure o Azure AD para salvar as senhas com hash para as identidades de seus usuários](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Organizações criadas na nuvem não precisam fazer alterações adicionais em seu diretório; no entanto, organizações locais precisam permitir que os hashes de senha sejam sincronizados e armazenados no Azure AD, o que talvez não seja permitido para algumas organizações. Os usuários precisarão redefinir suas senhas depois de fazer essa alteração de configuração.

## <a name="deploy-azure-ad-ds-and-rds"></a>Implantar o Azure AD DS e a Área de Trabalho Remota 
Use as etapas a seguir para implantar o Azure AD DS e a Área de Trabalho Remota.

1. Habilitar o [Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started). Observe que o artigo vinculado faz o seguinte:
   - Orientam quanto à criação de grupos do Azure AD adequados para a administração de domínio.
   - Realçam quando pode ser necessário forçar os usuários a alterarem suas senhas para que as contas possam funcionar com o Azure AD DS.
   
2. Configure a Área de Trabalho Remota. Você pode usar um modelo do Azure ou implantar a Área de Trabalho Remota manualmente.
   - Use o [Modelo existente do AD](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Personalize o seguinte:
   
     - **Configurações**
       - **Grupo de recursos**: use o grupo de recursos onde deseja criar os recursos da Área de Trabalho Remota.
         > [!NOTE] 
         > No momento, ele deve ser o mesmo grupo de recursos em que está a rede virtual do Azure Resource Manager.

       - **Prefixo de rótulo DNS**: insira a URL que deseja que os usuários usem para acessar a Web da Área de Trabalho Remota.
       - **Nome do domínio do AD**: insira o nome completo de sua instância do Azure AD, por exemplo, "contoso.onmicrosoft.com" ou "contoso.com".
       - **Nome da rede virtual do AD** e **Nome da sub-rede do AD**: insira os mesmos valores que você usou quando criou a rede virtual do Azure Resource Manager. Esta é a sub-rede à qual os recursos da Área de Trabalho Remota vão se conectar.
       - **Nome de usuário do administrador** e **Senha do administrador**: insira as credenciais de um usuário administrador que seja membro do grupo **Administradores do AAD DC** no Azure AD.
   
     - **Modelo**
        - Remova todas as propriedades de **dnsServers**: depois de selecionar **Editar modelo** na página do modelo de início rápido do Azure, pesquise "dnsServers" e remova a propriedade. 

           Por exemplo, antes de remover a propriedade **dnsServers**:
      
           ![Modelo de início rápido do Azure com a propriedade dnsSettings](media/rds-remove-dnssettings-before.png)

           E este é o mesmo arquivo após a remoção da propriedade:

           ![Modelo de início rápido do Azure com a propriedade dnsSettings removida](media/rds-remove-dnssettings-after.png)
   
   - [Implantar a Área de Trabalho Remota manualmente](rds-deploy-infrastructure.md). 

