---
title: Configurando a integração do Azure
description: Configurando a integração do Azure centro de administração do Windows (projeto Honolulu). Conectando o gateway do centro de administração do Windows ao Azure.
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: b56960a531c8d7d8cf42cb0462d2fe4d422dfba7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970893"
---
# <a name="configuring-azure-integration"></a>Configurando a integração do Azure

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

O centro de administração do Windows suporta vários recursos opcionais que se integram aos serviços do Azure. [Saiba mais sobre as opções de integração do Azure disponíveis com o centro de administração do Windows.](../plan/azure-integration-options.md)

Para permitir que o gateway do centro de administração do Windows se comunique com o Azure para aproveitar a autenticação Azure Active Directory para acesso ao gateway ou para criar recursos do Azure em seu nome (por exemplo, para proteger as VMs gerenciadas no centro de administração do Windows usando Azure Site Recovery), você precisará primeiro registrar o gateway do centro de administração do Windows com o Azure. Você só precisa fazer isso uma vez para o gateway do centro de administração do Windows-a configuração é preservada quando você atualiza seu gateway para uma versão mais recente.

## <a name="register-your-gateway-with-azure"></a>Registrar seu gateway com o Azure

Na primeira vez que você tentar usar um recurso de integração do Azure no centro de administração do Windows, será solicitado que você registre o gateway no Azure. Você também pode registrar o gateway acessando a guia Azure nas configurações **do** centro de administração do Windows. Observe que somente os administradores do gateway do centro de administração do Windows podem registrar o gateway do centro de administração do Windows com o Azure. [Saiba mais sobre as permissões de usuário e administrador do centro de administração do Windows](../configure/user-access-control.md#gateway-access-role-definitions).

As etapas guiadas no produto criarão um aplicativo do Azure AD em seu diretório, o que permite que o centro de administração do Windows se comunique com o Azure. Para exibir o aplicativo do Azure AD que é criado automaticamente, vá para a guia **Azure** das configurações do centro de administração do Windows. O **modo de exibição no hiperlink do Azure** permite exibir o aplicativo do Azure AD no portal do Azure.

O aplicativo do Azure AD criado é usado para todos os pontos de integração do Azure no centro de administração do Windows, incluindo a [autenticação do Azure ad para o gateway](../configure/user-access-control.md#azure-active-directory). O centro de administração do Windows configura automaticamente as permissões necessárias para criar e gerenciar recursos do Azure em seu nome:

- Graph do Active Directory do Azure
    - Directory.AccessAsUser.All
    - User.Read
- Gerenciamento do Serviço do Azure
    - user_impersonation

### <a name="manual-azure-ad-app-configuration"></a>Configuração manual de aplicativo do Azure AD

Se desejar configurar um aplicativo do Azure AD manualmente, em vez de usar o aplicativo do Azure AD criado automaticamente pelo centro de administração do Windows durante o processo de registro do gateway, você deverá fazer o seguinte.

1. Conceda ao aplicativo Azure AD as permissões de API necessárias listadas acima. Você pode fazer isso navegando até seu aplicativo do Azure AD na portal do Azure. Vá para o portal do Azure > **Azure Active Directory**  >  **registros de aplicativo** > Selecione seu aplicativo do Azure AD que você deseja usar. Em seguida, para a guia **permissões de API** e adicione as permissões de API listadas acima.
2. Adicione a URL do gateway do centro de administração do Windows às URLs de resposta (também conhecidas como URIs de redirecionamento). Navegue até o aplicativo Azure AD e, em seguida, vá para **manifesto**. Localize a chave "replyUrlsWithType" no manifesto. Dentro da chave, adicione um objeto que contém duas chaves: "URL" e "Type". A chave "URL" deve ter um valor da URL do gateway do centro de administração do Windows, acrescentando um caractere curinga no final. A chave "tipo" de chave deve ter um valor de "Web". Por exemplo:

    ```json
    "replyUrlsWithType": [
            {
                    "url": "http://localhost:6516/*",
                    "type": "Web"
            }
    ],
    ```
