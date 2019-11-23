---
title: Visão geral sobre o controle de conta de usuário
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bdc9f4dc4b8e19d62288f12a4f2b4e8c86b93b68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403313"
---
# <a name="user-account-control-overview"></a>Visão geral sobre o controle de conta de usuário
O controle de conta de usuário \(\) do UAC é um componente fundamental da visão geral da segurança da Microsoft.  O UAC ajuda a minimizar o impacto de um programa mal-intencionado.

## <a name="BKMK_OVER"></a>Descrição do recurso
O UAC permite que todos os usuários façam logon em seus computadores usando uma conta de usuário padrão. Processos iniciados com um token de usuário padrão podem realizar tarefas com direitos de acesso concedidos a um usuário padrão. Por exemplo, o Windows Explorer herda automaticamente as permissões de nível de usuário padrão. Além disso, todos os programas executados usando o Windows Explorer \(por exemplo,\-clicando em um atalho de aplicativo\) também são executados com o conjunto padrão de permissões de usuário. Muitos aplicativos, incluindo aqueles incluídos no próprio sistema operacional, são projetados para funcionar corretamente dessa maneira.

Outros aplicativos, especialmente aqueles que não foram projetados especificamente com as configurações de segurança em mente, geralmente exigem permissões adicionais para serem executados com êxito. Esses tipos de programas são chamados de aplicativos herdados. Além disso, ações como instalar novo software e fazer alterações de configuração em programas como o Firewall do Windows exigem mais permissões do que as disponíveis para uma conta de usuário padrão.

Quando um aplicativo precisa ser executado com mais de direitos de usuário padrão, o UAC pode restaurar grupos de usuários adicionais para o token. Isso permite que o usuário tenha controle explícito de programas que estão fazendo alterações no nível do sistema em seu computador ou dispositivo.

## <a name="BKMK_APP"></a>Aplicativos práticos
O modo de aprovação de administrador no UAC ajuda a impedir que programas mal-intencionados sejam instalados silenciosamente sem o conhecimento de um administrador. Ele também ajuda a proteger contra alterações involuntárias de\-do sistema. Por fim, ele pode ser usado para impor um nível mais alto de conformidade, no qual os administradores devem consentir ativamente ou fornecer as credenciais de cada processo administrativo.



