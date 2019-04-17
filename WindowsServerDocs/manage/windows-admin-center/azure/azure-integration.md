---
title: Configurar a integração do Azure
description: Configurando o Azure integração com o Windows Admin Center (Project Honolulu). Conectar seu gateway do Windows Admin Center no Azure.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 38b973680463cdebc1b3168e447abfcf1caba6b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296845"
---
# Configurar a integração do Azure

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Windows Admin Center oferece suporte a vários recursos opcionais que se integram com serviços do Azure. [Saiba mais sobre as opções de integração do Azure disponíveis com o Windows Admin Center.](../plan/azure-integration-options.md)

Para permitir que o gateway do Windows Admin Center para se comunicar com o Azure para aproveitar a autenticação do Azure Active Directory para o acesso do gateway, ou para criar recursos do Azure em seu nome (por exemplo, para proteger as VMs gerenciadas no Windows Admin Center usando o Azure Site Recuperação), você precisará primeiro registre seu gateway do Windows Admin Center com o Azure. Você só precisa fazer isso depois que seu gateway do Windows Admin Center - a configuração é preservada quando você atualizar seu gateway para uma versão mais recente.

## Registrar seu gateway com o Azure

Na primeira vez que você tentar usar um recurso de integração do Azure no Centro de administração do Windows, você será solicitado a registrar o gateway no Azure. Você também pode registrar o gateway indo até a guia **Azure** nas configurações do Windows Admin Center.

As etapas do produto guiadas criará um aplicativo do Azure AD no seu diretório, que permite que o Windows Admin Center para se comunicar com o Azure. Para exibir o aplicativo do Azure AD criado automaticamente, vá para a guia **Azure** das configurações do Windows Admin Center. O **modo de exibição no Azure** hiperlink permite exibir o aplicativo do Azure AD no portal do Azure. 

O aplicativo do Azure AD criado é usado para todos os pontos de integração do Azure no Centro de administração do Windows, incluindo [autenticação do Azure AD para o gateway](../configure/user-access-control.md#azure-active-directory).