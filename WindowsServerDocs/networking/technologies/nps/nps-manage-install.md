---
title: Instalar o NPS (Servidor de Políticas de Rede)
description: Você pode usar este tópico para instalar o servidor de diretivas de rede (NPS) usando o Windows PowerShell ou o assistente Adicionar funções e recursos no Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9d11f77ff8b9db8bc10970b325afae60ba937cfa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831187"
---
# <a name="install-network-policy-server"></a>Instalar o NPS (Servidor de Políticas de Rede)

Você pode usar este tópico para instalar o servidor de diretivas de rede (NPS) usando o Windows PowerShell ou o assistente Adicionar funções e recursos. O NPS é um serviço de função da função de servidor Serviços de Acesso e Política de Rede.

> [!NOTE]
> Por padrão, o NPS ouve o tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 em todos os adaptadores de rede instalados. Se o Firewall do Windows com segurança avançada é habilitado quando você instala o NPS, as exceções de firewall para essas portas serão criadas automaticamente durante o processo de instalação do Internet Protocol versão 6 \(IPv6\) e tráfego IPv4. Se seus servidores de acesso de rede estiverem configurados para enviar tráfego RADIUS em portas que não sejam essa padrão, remova as exceções criadas no Firewall do Windows com segurança avançada durante a instalação do NPS e crie exceções para as portas que você usa para Tráfego RADIUS.

**Credenciais administrativas**

Para concluir este procedimento, você deve ser um membro do grupo **Administradores de Domínio**.

## <a name="to-install-nps-by-using-windows-powershell"></a>Para instalar o NPS usando o Windows PowerShell

Para executar esse procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, digite o seguinte comando e pressione ENTER.

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>Para instalar o NPS usando o Gerenciador do servidor

1.  No NPS1, no Gerenciador do Servidor, clique em **Gerenciar** e em **Adicionar Funções e Recursos**. O Assistente para Adicionar Funções e Recursos é aberto.

2.  Em **Antes de Começar**, clique em **Avançar**.

    > [!NOTE]
    > A página **Antes de Começar** do Assistente para Adicionar Funções e Recursos não é exibida quando você marca a opção **Ignorar esta página por padrão** quando o Assistente para Funções e Recursos foi executado.

3.  Em **Selecionar Tipo de Instalação**, verifique se **Instalação baseada em função ou recurso** está marcada e clique em **Avançar**.

4.  Em **Selecionar servidor de destino**, verifique se **Selecionar um servidor no pool de servidores** está marcada. Em **Pool de Servidores**, verifique se o computador local está selecionado. Clique em **Avançar**.

5.  Na **selecionar funções do servidor**, na **funções**, selecione **serviços de acesso e diretiva de rede**. Uma caixa de diálogo é aberta, perguntando se ele deve adicionar recursos que são necessários para serviços de acesso e diretiva de rede. Clique em **adicionar recursos**e, em seguida, clique em **Avançar**

6.  Em **Selecionar recursos**, clique em **Avançar** e, em **Serviços de Acesso e Política de Rede**, analise as informações fornecidas e clique em **Avançar**.

7.  Em **Selecionar serviços de função**, clique em **Servidor de Políticas de Rede**.  Em **Adicionar recursos que são necessários para Servidor de Políticas de Rede**, clique em **Adicionar Recursos**. Clique em **Avançar**.

8.  Em **Confirmar seleções de instalação**, clique em **Reiniciar cada servidor de destino automaticamente, se necessário**. Na solicitação de confirmação dessa seleção, clique em **Sim** e em **Instalar**. A página Progresso da instalação exibe o status durante o processo de instalação. Quando o processo for concluído, a mensagem "instalação bem-sucedida no *NomeDoComputador*" é exibida, onde *ComputerName* é o nome do computador no qual você instalou o servidor de políticas de rede. Clique em **Fechar**.

Para obter mais informações, consulte [NPSs gerenciar](nps-manage-servers.md).