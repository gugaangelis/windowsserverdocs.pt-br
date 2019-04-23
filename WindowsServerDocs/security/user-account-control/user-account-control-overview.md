---
title: Visão geral sobre o controle de conta de usuário
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7a39cd-fc10-4408-befd-4b2c45806732
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 90ce72cb3d1850563d16a12d09a6872d107c0690
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887667"
---
# <a name="user-account-control-overview"></a>Visão geral sobre o controle de conta de usuário
Controle de conta de usuário \(UAC\) é um componente fundamental da visão geral de segurança da Microsoft.  O UAC ajuda a minimizar o impacto de um programa mal-intencionado.

## <a name="BKMK_OVER"></a>Descrição do recurso
O UAC permite que todos os usuários façam logon em seus computadores usando uma conta de usuário padrão. Processos iniciados com um token de usuário padrão podem realizar tarefas com direitos de acesso concedidos a um usuário padrão. Por exemplo, o Windows Explorer herda automaticamente as permissões de nível de usuário padrão. Além disso, todos os programas que são executados usando o Windows Explorer \(por exemplo, ao dupla\-clicando em um atalho do aplicativo\) também executados com o conjunto de permissões de usuário padrão. Muitos aplicativos, incluindo aqueles que estão incluídos com o sistema operacional em si, são projetados para funcionar corretamente, dessa forma.

Outros aplicativos, especialmente aquelas que não foram projetados especificamente com as configurações de segurança em mente, geralmente requerem permissões adicionais para executar com êxito. Esses tipos de programas são chamados de aplicativos herdados. Além disso, ações como instalar um novo software e alterações de configuração para programas, como o Firewall do Windows, exigir mais permissões do que está disponível para uma conta de usuário padrão.

Quando um necessidades de aplicativos para executar com direitos de usuário padrão mais de, o UAC pode restaurar grupos de usuários adicionais para o token. Isso permite que o usuário tenha controle explícito de programas que estiver fazendo alterações no nível do sistema em seu computador ou dispositivo.

## <a name="BKMK_APP"></a>Aplicativos práticos
Modo de aprovação de administrador no UAC ajuda a impedir que programas mal-intencionados sejam instalados silenciosamente sem o conhecimento do administrador. Ele também ajuda a proteger contra o sistema inadvertido\-alterações amplas. Por fim, ele pode ser usado para impor um nível mais alto de conformidade, no qual os administradores devem consentir ativamente ou fornecer as credenciais de cada processo administrativo.



