---
title: Ativar o servidor de licenças dos Serviços de Área de Trabalho Remota
description: Instale e ative o servidor de licenças de Área de Trabalho Remota
ms.topic: article
ms.assetid: eb24ddd2-0361-41fe-bd6b-c7c63427cb71
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 0caf683c95bcaaa8838028bb78c1209ccd5c916c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948898"
---
# <a name="activate-the-remote-desktop-services-license-server"></a>Ativar o servidor de licenças dos Serviços de Área de Trabalho Remota

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

O servidor de licenças dos Serviços de Área de Trabalho Remota emite CALs (licenças de acesso para cliente) para usuários e dispositivos quando eles acessam o Host da Sessão RD. Você pode ativar o servidor de licença usando o Gerenciador de Licenciamento de Área de Trabalho Remota.

## <a name="install-the-rd-licensing-role"></a>Instalar a função de Licenciamento de Área de Trabalho Remota

1. Entrar no servidor que deseja usar como servidor de licença usando uma conta de administrador.
2. No Gerenciador do Servidor, clique em **Resumo de Funções** e, em seguida, clique em **Adicionar Funções**.
   Clique em **Avançar** na primeira página do assistente de funções.
3. Selecione **Serviços de Área de Trabalho Remota**, clique em **Avançar** e, em seguida, clique em **Avançar** na página dos Serviços de Área de Trabalho Remota.
4. Selecione **Licenciamento de Área de Trabalho Remota** e, em seguida, clique em **Avançar**.
5. Configure o domínio – selecione **Configurar um escopo de descoberta para este servidor de licenças**, clique em **Este domínio** e, em seguida, clique em **Avançar**.
6. Clique em **Instalar**.

## <a name="activate-the-license-server"></a>Ativar o servidor de licença

1. Abra o Gerenciador de Licenciamento de Área de Trabalho Remota: clique em **Iniciar > Ferramentas Administrativas > Serviços de Área de Trabalho Remota > Gerenciador de Licenciamento de Área de Trabalho Remota**.
2. Clique com o botão direito do mouse no servidor de licença e clique em **Ativar Servidor**.
3. Clique em **Avançar** na página inicial.
4. Para o método de conexão, selecione **Conexão automática (recomendado)** e, em seguida, clique em **Avançar**.
5. Insira as informações da empresa (seu nome, o nome da empresa, sua região geográfica) e clique em **Avançar**.
6. Também é possível inserir outras informações da empresa (por exemplo, endereços de email e da empresa) e, em seguida, clicar em **Avançar**.
7. Certifique-se de que **Iniciar Assistente para Instalação de Licenças agora** não esteja selecionado (instalaremos as licenças em uma etapa posterior) e, em seguida, clique em **Avançar**.

O servidor de licença está pronto para começar a emitir e gerenciar licenças.