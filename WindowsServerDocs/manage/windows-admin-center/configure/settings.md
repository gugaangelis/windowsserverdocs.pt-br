---
title: Settings
description: Saiba mais sobre as configurações do Windows Admin Center (Project Honolulu). As configurações de usuário permitem que os usuários alterem o idioma/região e outras preferências. As configurações de gateway permitem que os administradores configurem o gateway.
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.openlocfilehash: ff06a19d85858b8332412a51c029c9aeeba2af50
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997440"
---
# <a name="windows-admin-center-settings"></a>Configurações do Windows Admin Center

> Aplica-se a: Windows Admin Center

As configurações do Windows Admin Center são compostas por configurações de nível de usuário e de gateway. Uma alteração de uma configuração de nível de usuário afeta apenas o perfil do usuário atual, enquanto uma alteração de uma configuração de nível de gateway afeta todos os usuários desse gateway do Windows Admin Center.

## <a name="user-settings"></a>Configurações do usuário

As configurações de nível de usuário são compostas pelas seguintes seções:

- Conta
- Personalização
- Idioma/Região
- Sugestões
- Avançado

Na guia **Conta**, os usuários podem revisar as credenciais que usaram para se autenticar no Windows Admin Center. Se o Azure AD estiver configurado para ser o provedor de identidade, o usuário poderá fazer logoff da conta do Azure AD dele nessa guia.

Na guia **Personalização**, os usuários podem alternar para um tema escuro da interface do usuário.

Na guia **Idioma/Região**, os usuários podem alterar os formatos de idioma e região exibidos pelo Windows Admin Center.

Na guia **Sugestões**, os usuários podem fazer sugestões a respeito dos serviços e novos recursos do Azure.

A guia **Avançado** fornece funcionalidades adicionais aos desenvolvedores de extensões do Windows Admin Center.

## <a name="gateway-settings"></a>Configurações do gateway

As configurações de nível de gateway são compostas pelas seguintes seções:

- Extensões
- Acesso
- Azure
- Conexões Compartilhadas

Somente administradores de gateway podem ver e alterar essas configurações. As alterações dessas configurações afetam a configuração do gateway e, consequentemente, todos os usuários do gateway do Windows Admin Center.

Na guia **Extensões**, os administradores podem instalar, desinstalar ou atualizar extensões de gateway. [Saiba mais sobre extensões.](using-extensions.md)

A guia **Acesso** permite que os administradores configurem quem pode acessar o gateway do Windows Admin Center, bem como o provedor de identidade usado para autenticar usuários. [Saiba mais sobre como controlar o acesso ao gateway.](user-access-control.md)

Na guia **Azure**, os administradores podem registrar o gateway com o Azure para habilitar os [recursos de integração do Azure](../azure/azure-integration.md) no Windows Admin Center.

Ao usar a guia **Conexões Compartilhadas**, os administradores podem configurar uma lista única de conexões a serem compartilhadas entre todos os usuários do gateway do Windows Admin Center. [Saiba mais sobre como configurar conexões uma única vez para todos os usuários de um gateway.](shared-connections.md)