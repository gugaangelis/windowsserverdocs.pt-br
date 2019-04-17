---
title: Instalar o servidor de políticas de rede
description: Você pode usar este tópico para instalar o servidor de política de rede (NPS) usando o Windows PowerShell ou a adição de funções e recursos de assistente no Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4842a4ab-70bb-4744-bea7-70f2ac892ad1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b76654fa01b68b5a8c9ea1efc80dfbc47e6a62f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="install-network-policy-server"></a>Instalar o servidor de políticas de rede

Você pode usar este tópico para instalar o servidor de política de rede (NPS) usando o Windows PowerShell ou a adição de funções e recursos do assistente. NPS é um serviço de função da função de servidor Serviços de acesso e política de rede.

> [!NOTE]
> Por padrão, o NPS escuta tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 em todos os adaptadores de rede instalados. Se o Firewall do Windows com segurança avançada é habilitado quando você instala o NPS, exceções de firewall para estas portas são criadas automaticamente durante o processo de instalação para o protocolo IP versão 6 \(IPv6\) e tráfego IPv4. Caso os servidores de acesso de rede estejam configurados para enviar tráfego RADIUS através de portas que não sejam esses padrões, remova as exceções criadas no Firewall do Windows com segurança avançada durante a instalação de NPS e crie exceções para as portas que você usa para o tráfego de RAIO.

**Credenciais administrativas**

Para concluir este procedimento, você deve ser um membro do **Admins. do domínio** grupo.

## <a name="to-install-nps-by-using-windows-powershell"></a>Para instalar o NPS usando o Windows PowerShell

Para executar este procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, digite o seguinte comando e pressione ENTER.

`Install-WindowsFeature NPAS -IncludeManagementTools`

## <a name="to-install-nps-by-using-server-manager"></a>Para instalar o NPS usando o Gerenciador do servidor

1.  No NPS1, no Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**. Abre o assistente Adicionar funções e recursos.

2.  Em **Before You Begin**, clique em **próxima**.

    > [!NOTE]
    > O **Before You Begin** página do Assistente de recursos de adição de funções e não é exibida se você tiver selecionado anteriormente **ignorar esta página por padrão** quando a adição de funções e recursos de assistente foi executado.

3.  Em **selecione tipo de instalação**, certifique-se de que **instalação baseada em função ou recurso baseado** está selecionado e clique em **próxima**.

4.  Em **servidor de destino Select**, certifique-se de que **selecionar um servidor do pool servidor** é selecionado. Em **Pool de servidores**, verifique se o computador local está selecionado. Clique em **próxima**.

5.  Em **selecionar funções de servidor**, na **funções**, selecione **serviços de acesso e política de rede**. É aberta uma caixa de diálogo perguntando se ele deve adicionar recursos que são necessários para serviços de acesso e política de rede. Clique em **adicionar recursos**e clique em **próximo**

6.  Em **Selecione recursos**, clique em **próxima**e em **serviços de acesso e política de rede**, examine as informações que são fornecidas e, em seguida, clique em **próxima**.

7.  Em **selecione Serviços de função**, clique em **NPS**.  Em **adicionar recursos que são necessários para o servidor de políticas de rede**, clique em **adicionar recursos**. Clique em **próxima**.

8.  Em **confirmar seleções de instalação**, clique em **reiniciar o servidor de destino automaticamente, se necessário**. Quando você for solicitado a confirmar a seleção, clique em **Sim**e clique em **instalar**. Página Installation progress exibe o status durante o processo de instalação. Quando o processo for concluído, a mensagem "instalação foi bem-sucedida no *ComputerName*" é exibido, onde *ComputerName* é o nome do computador no qual você instalou o servidor de políticas de rede. Clique em **fechar**.

Para obter mais informações, consulte [gerenciar servidores NPS](nps-manage-servers.md).