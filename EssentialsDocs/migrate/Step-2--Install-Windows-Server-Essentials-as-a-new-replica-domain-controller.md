---
title: 'Etapa 2: Instalar o Windows Server Essentials como um novo controlador de domínio de réplica'
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7ccfc34-63fd-436b-a1cd-e05810f60bfe
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5968db77c091dbca1eb7d38f5e924e5f449052ce
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318772"
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>Etapa 2: Instalar o Windows Server Essentials como um novo controlador de domínio de réplica

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta seção descreve como instalar o Windows Server Essentials e o Windows Server 2012 R2 Standard (com a função de experiência do Windows Server Essentials habilitada) como um controlador de domínio.  
  
 Para ambientes com até 25 usuários e 50 dispositivos, você pode seguir as etapas neste guia para migrar de versões anteriores do Windows SBS para o Windows Server Essentials. Para ambientes com até 100 usuários e 200 dispositivos, você pode seguir as mesmas diretrizes para migrar para as edições Standard e Datacenter do Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada. Os dois cenários são abordados nesta documentação.  
  
> [!IMPORTANT]
>  Se você migrar para o Windows Server Essentials, a seguinte mensagem de erro será adicionada ao log de eventos a cada dia durante o período de carência de 21 dias até que você remova o servidor de origem da rede. Após o período de cortesia de 21 dias, o servidor de origem será desativado. <br> **A verificação da função FSMO detectou uma condição em seu ambiente que está fora de conformidade com a política de licenciamento. O servidor de gerenciamento deve conter o controlador de domínio primário e o mestre de nomeação de domínio Active Directory funções. Mova as funções de Active Directory para o servidor de gerenciamento agora. Esse servidor será desligado automaticamente se o problema não for corrigido em 21 dias a partir do momento em que essa condição foi detectada pela primeira**vez.   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>Instalar o Windows Server Essentials ou o Windows Server 2012 R2 Standard no servidor de destino  
  
1.  Instale o Windows Server Essentials ou o Windows Server 2012 R2 Standard com a função experiência do Windows Server Essentials habilitada seguindo as instruções em [instalar e configurar o Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
  
    > [!NOTE]
    >  Se o Assistente Configurar o Windows Server Essentials for iniciado, cancele-o.  
  
2.  Transferir as funções FSMO do servidor de origem.  
  
    > [!NOTE]
    >  Se o Windows Server Essentials for o único controlador de domínio no domínio, a função FSMO será automaticamente movida para o servidor que executa o Windows Server Essentials quando você rebaixar o servidor de origem.  
  
3.  Abra o Gerenciador do servidor e execute o Assistente para adicionar funções e recursos.  
  
4.  Se não estiver instalado, adicione a função Experiência do Windows Server Essentials.  
  
5.  Depois de instalar a função Experiência do Windows Server Essentials, a tarefa de configurar o Windows Server Essentials aparece na área de notificação. Clique na tarefa para iniciar o assistente Configurar o Windows Server Essentials.  
  
6.  Siga as instruções para concluir a configuração do Windows Server Essentials. Antes de executar o assistente, faça o seguinte:  
  
    -   Altere o nome do servidor, se necessário, porque não é possível alterar o nome depois de concluir o assistente Configurar o Windows Server Essentials.  
  
    -   Verifique se a hora e as configurações do servidor estão corretas.  
  
7.  Verifique a instalação da seguinte maneira:  
  
    1.  Abra o Dashboard.  
  
    2.  Clique na guia **Usuários** e verifique se as contas de usuário no seu Active Directory estão listadas.  
  
### <a name="transfer-the-operations-master-roles"></a>Transferir as funções mestre de operações  
 As funções mestre de operações (também chamadas de operações de mestre único flexíveis ou FSMO) devem ser transferidas do servidor de origem para o servidor de destino dentro de 21 dias após a instalação do Windows Server Essentials no servidor de destino.  
  
##### <a name="to-transfer-the-operations-master-roles"></a>Para transferir as funções mestre de operações  
  
1.  No servidor de destino, abra uma janela de Prompt de Comando como administrador. Veja [Para abrir uma janela de prompt de comando como um administrador](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx).  
  
2.  No prompt de comando, digite **FSMO DE CONSULTA NETDOM** e pressione ENTER.  
  
3.  No prompt de comando, digite **ntdsutil** e pressione ENTER.  
  
4.  No prompt de comando **ntdsutil**, digite os seguintes comandos:  
  
    1.  Digite **activate instance NTDS** e pressione ENTER.  
  
    2.  Digite **funções** e pressione ENTER.  
  
    3.  Digite **conexões** e pressione ENTER.  
  
    4.  Digite **conectar-se ao servidor** *<* ServerName\>(em que *< ServerName\>* é o nome do servidor de destino) e pressione Enter.  
  
    5.  No prompt de comando, digite **q** e pressione ENTER.  
  
        1.  Digite **transfer PDC**, pressione ENTER e clique em **Sim** na caixa de diálogo **Confirmação de Transferência de Função**.  
  
        2.  Digite **mestre de infraestrutura de transferência**, pressione ENTER e clique em **Sim** na caixa de diálogo **Confirmação de Transferência de Função**.  
  
        3.  Digite **mestre de nomeação de transferência**, pressione ENTER e clique em **Sim** na caixa de diálogo **Confirmação de Transferência de Função**.  
  
        4.  Digite **mestre de RID de transferência**, pressione ENTER e clique em **Sim** na caixa de diálogo **Confirmação de Transferência de Função**.  
  
        5.  Digite **mestre de esquema de transferência**, pressione ENTER e clique em **Sim** na caixa de diálogo **Confirmação de Transferência de Função**.  
  
    6.  Digite **q** e pressione ENTER para retornar ao prompt de comando.  
  
> [!NOTE]
>  A partir de qualquer servidor na rede, você pode verificar se as funções mestre de operações foram transferidas para o servidor de destino. Abra uma janela de prompt de comando como administrador (para obter mais informações, consulte [Para abrir uma janela de prompt de comando como um administrador](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)). Digite **fsmo de consulta netdom** e pressione ENTER.  
  
## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}  
 Você instalou o Windows Server Essentials como um novo controlador de domínio de réplica. Agora vá para a [etapa 3: unir computadores ao novo servidor do Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  
  
Para exibir todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

