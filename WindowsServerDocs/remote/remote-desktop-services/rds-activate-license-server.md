---
title: Ativar o servidor de licença dos serviços de área de trabalho remota
description: Instalar e ativar o servidor de licenças de área de trabalho remota
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eb24ddd2-0361-41fe-bd6b-c7c63427cb71
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: d28ceac9cde0ee2d4c92867bdd90d5c463a8cd4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888007"
---
# <a name="activate-the-remote-desktop-services-license-server"></a>Ativar o servidor de licença dos serviços de área de trabalho remota

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

O Remote Desktop Services server problemas CALs (CALs) de licença para usuários e dispositivos quando eles acessarem o Host de sessão de área de trabalho remota. Você pode ativar o servidor de licença usando o Gerenciador de licenciamento de área de trabalho remota. 

## <a name="install-the-rd-licensing-role"></a>Instalar a função de licenciamento de área de trabalho remota

1. Entrar no servidor que você deseja usar como o servidor de licença usando uma conta de administrador.
2. No Gerenciador do servidor, clique em **resumo de funções**e, em seguida, clique em **adicionar funções**.
   Clique em **próxima** na primeira página do Assistente de funções.
3. Selecione **dos serviços de área de trabalho remota**e, em seguida, clique em **próxima**e, em seguida, **próxima** na página de serviços de área de trabalho remota.
4. Selecione **licenciamento de área de trabalho remota**e, em seguida, clique em **próxima**.
5. Configurar o domínio - selecione **configurar um escopo de descoberta para este servidor de licença**, clique em **nesse domínio**e, em seguida, clique em **próxima**.
6. Clique em **Instalar**.

## <a name="activate-the-license-server"></a>Ativar o servidor de licença

1. Abra o Gerenciador de licenciamento de área de trabalho remota: clique em **Iniciar > Ferramentas administrativas > Remote Desktop Services > Gerenciador de licenciamento de área de trabalho remota**.
2. Clique com botão direito no servidor de licença e, em seguida, clique em **Ativar servidor**.
3. Clique em **próxima** na página de boas-vinda.
4. Para o método de conexão, selecione **conexão automática (recomendado)** e, em seguida, clique em **próxima**.
5. Insira as informações da sua empresa (seu nome, o nome da empresa, sua região geográfica) e, em seguida, clique em **próxima**.
6. Opcionalmente, insira outras informações da empresa (por exemplo, endereços de email e da empresa) e, em seguida, clique em **próxima**. 
7. Certifique-se de que **iniciar instalar licenças o assistente agora** não estiver selecionada (instalaremos as licenças em uma etapa posterior) e, em seguida, clique em **próxima**.

O servidor de licenças agora está pronto para começar a emissão e gerenciamento de licenças. 