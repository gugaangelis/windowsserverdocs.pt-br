---
title: Configurações
description: Saiba mais sobre as configurações no centro de administração do Windows (projeto Honolulu). As configurações de usuário permitem que os usuários alterem seu idioma/região e outras preferências. As configurações de gateway permitem que os administradores configurem o gateway.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e184064aa913bf4fb18cadd8ddbb08b5b97c59c6
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865315"
---
# <a name="windows-admin-center-settings"></a>Configurações do centro de administração do Windows

> Aplica-se a: Windows Admin Center

As configurações do centro de administração do Windows consistem em configurações de nível de usuário e de gateway. Uma alteração em uma configuração de nível de usuário afeta apenas o perfil do usuário atual, enquanto uma alteração em uma configuração de nível de gateway afeta todos os usuários nesse gateway do centro de administração do Windows.

## <a name="user-settings"></a>Configurações do usuário

As configurações de nível de usuário consistem nas seguintes seções:

- Conta
- Personalização
- Idioma/região
- Sugestões
- Avançado

Na guia **conta** , os usuários podem examinar as credenciais que usaram para autenticar no centro de administração do Windows. Se o Azure AD estiver configurado para ser o provedor de identidade, o usuário poderá fazer logoff de sua conta do Azure AD dessa guia.

Na guia **personalização** , os usuários podem alternar para um tema de interface do usuário escuro.

Na guia **idioma/região** , os usuários podem alterar os formatos de idioma e região exibidos pelo centro de administração do Windows.

Na guia **sugestões** , os usuários podem alternar sugestões sobre os serviços do Azure e os novos recursos.

A guia **avançado** fornece aos desenvolvedores de extensão do centro de administração do Windows recursos adicionais.

## <a name="gateway-settings"></a>Configurações do gateway

As configurações de nível de gateway consistem nas seguintes seções:

- Extensões
- Access
- Azure
- Conexões compartilhadas

Somente administradores de gateway podem ver e alterar essas configurações. As alterações nessas configurações alteram a configuração do gateway e afetam todos os usuários do gateway do centro de administração do Windows.

Na guia **extensões** , os administradores podem instalar, desinstalar ou atualizar extensões de gateway. [Saiba mais sobre extensões.](using-extensions.md)

A guia **acesso** permite que os administradores configurem quem pode acessar o gateway do centro de administração do Windows, bem como o provedor de identidade usado para autenticar usuários. [Saiba mais sobre como controlar o acesso ao gateway.](user-access-control.md)

Na guia **Azure** , os administradores podem registrar o gateway com o Azure para habilitar os [recursos de integração do Azure](azure-integration.md) no centro de administração do Windows.

Usando a guia **conexões compartilhadas** , os administradores podem configurar uma única lista de conexões a serem compartilhadas entre todos os usuários do gateway do centro de administração do Windows. [Saiba mais sobre como configurar conexões uma vez para todos os usuários de um gateway.](shared-connections.md)