---
title: Configurando a integração do Azure
description: Configurando a integração do Azure centro de administração do Windows (projeto Honolulu). Conectando o gateway do centro de administração do Windows ao Azure.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 448a8fb3e4340752b673b06f86d5d49211b6b147
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357362"
---
# <a name="configuring-azure-integration"></a>Configurando a integração do Azure

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

O centro de administração do Windows suporta vários recursos opcionais que se integram aos serviços do Azure. [Saiba mais sobre as opções de integração do Azure disponíveis com o centro de administração do Windows.](../plan/azure-integration-options.md)

Para permitir que o gateway do centro de administração do Windows se comunique com o Azure para aproveitar a autenticação Azure Active Directory para acesso ao gateway ou para criar recursos do Azure em seu nome (por exemplo, para proteger VMs gerenciadas no centro de administração do Windows usando o site do Azure Recuperação), você precisará primeiro registrar o gateway do centro de administração do Windows com o Azure. Você só precisa fazer isso uma vez para o gateway do centro de administração do Windows-a configuração é preservada quando você atualiza seu gateway para uma versão mais recente.

## <a name="register-your-gateway-with-azure"></a>Registrar seu gateway com o Azure

Na primeira vez que você tentar usar um recurso de integração do Azure no centro de administração do Windows, será solicitado que você registre o gateway no Azure. Você também pode registrar o gateway acessando a guia Azure nas configurações **do** centro de administração do Windows.

As etapas guiadas no produto criarão um aplicativo do Azure AD em seu diretório, o que permite que o centro de administração do Windows se comunique com o Azure. Para exibir o aplicativo do Azure AD que é criado automaticamente, vá para a guia **Azure** das configurações do centro de administração do Windows. O **modo de exibição no hiperlink do Azure** permite exibir o aplicativo do Azure AD no portal do Azure. 

O aplicativo do Azure AD criado é usado para todos os pontos de integração do Azure no centro de administração do Windows, incluindo a [autenticação do Azure ad para o gateway](../configure/user-access-control.md#azure-active-directory).