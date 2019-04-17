---
title: Configurações
description: Saiba mais sobre as configurações no Windows Admin Center (Project Honolulu). Configurações do usuário permitem que os usuários altere sua linguagem/região e outras preferências. Configurações de gateway permitem que os administradores de configurar o gateway.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d67a2c743900792353141186112cd09dbf780309
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296618"
---
# Configurações do Windows Admin Center

> Aplicável a: Windows Admin Center

Configurações do Windows Admin Center consistem em configurações de nível de usuário e do gateway. Uma alteração em uma configuração de nível de usuário afeta somente o perfil do usuário atual, enquanto uma alteração em uma configuração de nível de gateway afeta todos os usuários nesse gateway do Windows Admin Center.

## Configurações do usuário

Configurações de nível de usuário consistem em seções a seguir:

- Account
- Personalization
- Idioma/região
- Sugestões
- Avançado

Na guia **conta** , os usuários podem analisar as credenciais que eles têm usados para autenticar para o Windows Admin Center. Se o Azure AD está configurado para ser o provedor de identidade, o usuário poderá fazer logon fora de sua conta do Azure AD nessa guia.

Na guia **personalização** , os usuários podem alternar para um tema escuro da interface do usuário.

Na guia do **Idioma/região** , os usuários podem alterar os formatos de idioma e região exibidos pelo Windows Admin Center.

Na guia **sugestões** , os usuários podem ativar sugestões sobre novos recursos e serviços do Azure.

Na guia **Avançado** oferece aos desenvolvedores de extensão do Windows Admin Center funcionalidades adicionais.

## Configurações de gateway

Configurações de nível de gateway consistem em seções a seguir:

- Extensões
- Access
- Azure
- Conexões compartilhadas

Apenas os administradores do gateway são capazes de ver e alterar essas configurações. As alterações dessas configurações alterar a configuração do gateway e afetam todos os usuários do gateway do Windows Admin Center.

Na guia **extensões** , os administradores podem instalar, desinstalar ou atualizar extensões de gateway. [Saiba mais sobre extensões.](using-extensions.md)

Guia de **acesso** permite que os administradores configure quem pode acessar o gateway do Windows Admin Center, bem como o provedor de identidade usado para autenticar os usuários. [Saiba mais sobre como controlar o acesso ao gateway.](user-access-control.md)

Guia do **Azure** , os administradores podem registrar o gateway com o Azure para habilitar [recursos de integração do Azure](azure-integration.md) no Centro de administração do Windows.

Usando a guia **Conexões compartilhadas** , os administradores podem configurar uma única lista de conexões para ser compartilhado entre todos os usuários do gateway do Windows Admin Center. [Saiba mais sobre como configurar conexões uma vez para todos os usuários de um gateway.](shared-connections.md)