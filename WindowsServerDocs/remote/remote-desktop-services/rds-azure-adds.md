---
title: Serviços de domínio do AD do Azure e serviços de área de trabalho remota
description: Saiba como integrar o Azure AD Domain Services em sua implantação do RDS.
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
ms.openlocfilehash: 8b1baf642ffa3c8e8a0a2cfc70d2f49b58f208b3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446586"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Integrar o Azure AD Domain Services à implantação do RDS

Você pode usar [Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) em sua implantação de serviços de área de trabalho remota no lugar do Windows Server Active Directory. Azure AD DS permite o uso de suas identidades existentes do Azure AD com o clássicas cargas de trabalho do Windows.

Com o Azure AD DS, você pode: 
- Crie um ambiente do Azure com um domínio local para organizações gerados-em-nuvem. 
- Crie um ambiente do Azure isolado com as mesmas identidades usadas para o local e o ambiente online, sem a necessidade de criar uma VPN de site a site ou ExpressRoute. 

Quando você concluir a integração do Azure AD DS para sua implantação de área de trabalho remota, sua arquitetura será algo parecido com isso:

![Um diagrama de arquitetura mostrando RDS com o Azure AD DS](media/aadds-rds.png)

Para ver como essa arquitetura se compara com outros cenários de implantação do RDS, fazer check-out [arquiteturas de serviços de área de trabalho remota](desktop-hosting-logical-architecture.md).

Para obter um melhor entendimento do Azure AD DS, confira a [visão geral do Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-overview) e [como decidir se o Azure AD DS é adequada para seu caso de uso](/azure/active-directory-domain-services/active-directory-ds-comparison).

Use as informações a seguir para implantar o Azure AD DS com o RDS.

## <a name="prerequisites"></a>Pré-requisitos

Antes de você colocar suas identidades do Azure AD para usar em uma implantação do RDS [configurar o Azure AD para salvar as senhas com hash de identidades de seus usuários](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Não é necessário fazer alterações adicionais em seu diretório; organizações gerados-em-nuvem No entanto, as organizações de local necessário permitir que os hashes de senha para ser sincronizadas e armazenadas no AD do Azure, que pode não ser permitido para algumas organizações. Os usuários precisarão redefinir suas senhas depois de fazer essa alteração de configuração.

## <a name="deploy-azure-ad-ds-and-rds"></a>Implantar RDS e Azure AD DS 
Use as etapas a seguir para implantar o Azure AD DS e RDS.

1. Habilitar [do Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started). Observe que o artigo vinculado faz o seguinte:
   - Orientam durante a criação apropriada a grupos do Azure AD para a administração de domínio.
   - Realçar quando talvez seja necessário forçar os usuários alterem suas senhas para que suas contas podem trabalhar com o Azure AD DS.
   
2. Configurar o RDS. Você pode usar um modelo do Azure ou implantar RDS manualmente.
   - Use o [modelo existente do AD](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Certifique-se de personalizar o seguinte:
   
     - **Configurações**
       - **Grupo de recursos**: Use o grupo de recursos onde você deseja criar os recursos RDS.
         > [!NOTE] 
         > No momento, isso deve ser o mesmo grupo de recursos em que existe a rede virtual do Azure resource manager.

       - **Prefixo de rótulo DNS**: Insira a URL que você deseja que os usuários usem para acessar a Web da área de trabalho remota.
       - **Nome de domínio do AD**: Insira o nome completo da sua instância do AD do Azure, por exemplo, "contoso.onmicrosoft.com" ou "contoso.com".
       - **Nome da rede virtual do AD** e **nome da sub-rede Ad**: Insira os mesmos valores que você usou quando criou a rede virtual do Azure resource manager. Isso é a sub-rede à qual se conectar os recursos RDS.
       - **Nome de usuário administrador** e **senha do administrador**: Insira as credenciais para um usuário de administrador é um membro do **AAD DC Administrators** grupo no AD do Azure.
   
     - **modelo**
        - Remover todas as propriedades de **dnsServers**: depois de selecionar **Editar modelo** na página de modelo de início rápido do Azure, pesquise "dnsServers" e remova a propriedade. 

           Por exemplo, antes de remover o **dnsServers** propriedade:
      
           ![Modelo de início rápido do Azure com a propriedade dnsSettings](media/rds-remove-dnssettings-before.png)

           E aqui está o mesmo arquivo depois de remover a propriedade:

           ![Modelo de início rápido do Azure com a propriedade dnsSettings removida](media/rds-remove-dnssettings-after.png)
   
   - [Implantar manualmente o RDS](rds-deploy-infrastructure.md). 

