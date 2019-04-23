---
title: Configurações
description: Saiba mais sobre as configurações no Windows Admin Center (projeto Paulo). As configurações de usuário permitem aos usuários alterar seu idioma/região e outras preferências. As configurações de gateway permitem aos administradores configurar o gateway.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1e1231500733f70ddfcbd4f8a847047b73f24a00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882377"
---
# <a name="settings"></a>Configurações

> Aplica-se a: Windows Admin Center

Configurações do Windows Admin Center consistem em configurações de nível de usuário e o nível do gateway. Uma alteração em uma configuração de nível de usuário só afeta o perfil do usuário atual, enquanto uma alteração em uma configuração de nível do gateway afeta todos os usuários que o gateway do Windows Admin Center.

## <a name="user-settings"></a>Configurações do usuário

Configurações de nível de usuário consistem nas seções a seguir:

- Conta
- Idioma/região
- Sugestões

No **conta** guia, os usuários podem examinar as credenciais que eles usaram para autenticar para o Windows Admin Center. Se o AD do Azure está configurado para ser o provedor de identidade, o usuário pode fazer fora de sua conta do Azure AD nessa guia.

No **idioma/região** guia, os usuários podem alterar os formatos de idioma e região exibidos pelo Windows Admin Center.

No **sugestões** guia, os usuários podem alternar sugestões sobre novos recursos e serviços do Azure.

### <a name="dark-theme"></a>Tema escuro

> Aplica-se a: Windows Admin Center Preview

No Windows Admin Center versão prévia, você pode desbloquear adicional **personalização** seção, que contém a opção para alternar para um tema escuro de interface do usuário. Para habilitar o **personalização** , digite ```msft.sme.shell.personalization``` como uma chave de teste.

>[!IMPORTANT]
> Tema escuro é um trabalho em andamento, faça não relatar bugs no momento.

## <a name="gateway-settings"></a>Configurações de gateway

Configurações de nível de gateway consistem as seções a seguir:

- Extensões
- Acesso
- Azure

Somente os administradores de gateway são capazes de ver e alterar essas configurações. As alterações nessas configurações alterar a configuração do gateway e afetam todos os usuários do gateway do Windows Admin Center.

No **extensões** guia, os administradores podem instalar, desinstalar ou atualizar extensões de gateway. [Saiba mais sobre as extensões.](using-extensions.md)

O **acesso** guia permite que os administradores configurem a quem pode acessar o gateway do Windows Admin Center, bem como o provedor de identidade usada para autenticar usuários. [Saiba mais sobre como controlar o acesso ao gateway.](user-access-control.md)

Dos **Azure** guia, os administradores podem registrar o gateway com o Azure para habilitar [recursos de integração do Azure](azure-integration.md) no Windows Admin Center.