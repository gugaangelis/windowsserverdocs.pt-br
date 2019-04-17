---
title: "Visão geral do controle de conta de usuário"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="user-account-control-overview"></a>Visão geral do controle de conta de usuário
Usuário controle de conta \(UAC\) é um componente fundamental da visão geral de segurança da Microsoft.  O UAC ajuda a atenuar o impacto de um programa mal-intencionado.

## <a name="BKMK_OVER"></a>Descrição dos recursos
O UAC permite que todos os usuários façam logon em seus computadores usando uma conta de usuário padrão. Processos iniciados com um token de usuário padrão podem realizar tarefas com direitos de acesso concedidos a um usuário padrão. Por exemplo, o Windows Explorer herda automaticamente as permissões de nível de usuário padrão. Além disso, todos os programas que são executados usando o Windows Explorer \ (por exemplo, ao clicar em double\ shortcut\ um aplicativo) também são executados com o conjunto padrão de permissões de usuário. Muitos aplicativos, incluindo aqueles que estão incluídos com o sistema operacional em si, são projetados para funcionar corretamente dessa maneira.

Outros aplicativos, especialmente aqueles que não foram especificamente projetados com configurações de segurança em mente, geralmente exigem permissões adicionais para ser executado com êxito. Esses tipos de programas são chamados de aplicativos herdados. Além disso, ações como instalar um software novo e fazer alterações de configuração para programas como o Firewall do Windows exigem mais permissões do que está disponível para uma conta de usuário padrão.

Quando um necessidades de aplicativos seja executado com direitos de usuário mais de padrão, o UAC pode restaurar grupos de usuários adicionais para o token. Isso permite que o usuário tenha controle explícito de programas que estão fazendo alterações no nível de sistema para seu computador ou dispositivo.

## <a name="BKMK_APP"></a>Aplicativos práticos
Modo de aprovação de administrador no UAC ajuda a impedir que programas mal-intencionados se instalar silenciosamente sem o conhecimento do administrador. Ele também ajuda a proteger contra alterações inadvertidas System \. Por fim, ele pode ser usado para impor um nível mais alto de conformidade onde os administradores ativamente devem consentir ou fornecer as credenciais de cada processo administrativo.



