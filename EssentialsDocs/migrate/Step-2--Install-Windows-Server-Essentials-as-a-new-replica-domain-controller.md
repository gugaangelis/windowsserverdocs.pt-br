---
title: "Etapa 2: Instalar Windows Server Essentials como um controlador de domínio réplica novo"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7ccfc34-63fd-436b-a1cd-e05810f60bfe
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 757012b7d1a57a001e3b55cdc0604b63852a3d3c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>Etapa 2: Instalar Windows Server Essentials como um controlador de domínio réplica novo

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Esta seção descreve como instalar o Windows Server Essentials e Windows Server 2012 R2 Standard (com a função de experiência do Windows Server Essentials habilitada) como um controlador de domínio.  
  
 Para ambientes com até 25 usuários e 50 dispositivos, você pode seguir as etapas neste guia para migrar de versões anteriores do Windows SBS para o Windows Server Essentials. Para ambientes com até 100 usuários e 200 dispositivos, você pode seguir a mesma diretriz para migrar às edições do Windows Server 2012 R2 Standard e Datacenter com a função de experiência do Windows Server Essentials instalada. Ambos os cenários são abordados nesta documentação.  
  
> [!IMPORTANT]
>  Se você migrar para o Windows Server Essentials, a seguinte mensagem de erro é adicionada ao log de eventos cada dia durante o período de carência de 21 dias até que você remova o servidor de origem da sua rede. Após o período de carência de 21 dias, o servidor de origem será desligado. <br> **A verificação da função FSMO detectou uma condição em seu ambiente que está fora de conformidade com a política de licenciamento. O servidor de gerenciamento deve manter o controlador de domínio primário e o domínio nomenclatura funções mestre do Active Directory. Mova as funções do Active Directory para o servidor de gerenciamento agora. Esse servidor será ser desligado automaticamente se o problema não for corrigido em 21 dias desde o momento em que essa condição foi detectada primeiro**.   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>Instalar o Windows Server Essentials ou o Windows Server 2012 R2 Standard no servidor de destino  
  
1.  Instalar o Windows Server Essentials ou o Windows Server 2012 R2 Standard com a função de experiência do Windows Server Essentials habilitada, seguindo as instruções em [instalar e configurar o Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
  
    > [!NOTE]
    >  Se configurar Assistente do Windows Server Essentials inicia, cancelá-la.  
  
2.  Transferir as funções FSMO do seu servidor de origem.  
  
    > [!NOTE]
    >  Se o Windows Server Essentials é o único controlador de domínio no domínio, a função FSMO automaticamente é movida para o servidor que executa o Windows Server Essentials quando você rebaixar o servidor de origem.  
  
3.  Abra o Gerenciador do servidor e execute o Assistente de adição de funções e recursos.  
  
4.  Se não estiver instalado, adicione a função de experiência do Windows Server Essentials.  
  
5.  Depois de instalar a função de experiência do Windows Server Essentials, a tarefa de configurar o Windows Server Essentials aparece na área de notificação. Clique na tarefa para iniciar o Assistente para configurar o Windows Server Essentials.  
  
6.  Siga as instruções para concluir a configuração do Windows Server Essentials. Antes de executar o assistente, faça o seguinte:  
  
    -   Altere o nome do servidor, se necessário, porque você não pode alterar o nome depois de concluir o Assistente Configurar do Windows Server Essentials.  
  
    -   Certifique-se de que o servidor tempo e as configurações estão corretas.  
  
7.  Verifique se a instalação da seguinte maneira:  
  
    1.  Abra o painel.  
  
    2.  Clique em para **usuários** guia e verifique se que as contas de usuário no Active Directory são listadas.  
  
### <a name="transfer-the-operations-master-roles"></a>Transferir as funções mestre de operações  
 As funções de operações mestre (também chamado de operações de mestres únicas flexíveis ou FSMO) devem ser transferidas do servidor de origem para o servidor de destino em 21 dias de instalação do Windows Server Essentials no servidor de destino.  
  
##### <a name="to-transfer-the-operations-master-roles"></a>Para transferir as funções mestre de operações  
  
1.  No servidor de destino, abra uma janela de Prompt de comando como administrador. Consulte [para abrir uma janela de Prompt de comando como administrador](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx).  
  
2.  No prompt de comando, digite **NETDOM QUERY FSMO**, e pressione ENTER.  
  
3.  No prompt de comando, digite **ntdsutil**, e pressione ENTER.  
  
4.  No **ntdsutil** prompt de comando, digite os seguintes comandos:  
  
    1.  Tipo **ativar instância NTDS**, e pressione ENTER.  
  
    2.  Tipo **funções**, e pressione ENTER.  
  
    3.  Tipo **conexões**, e pressione ENTER.  
  
    4.  Tipo **se conectar ao servidor** *< ServerName\ >* (onde *< ServerName\ >* é o nome do servidor de destino), e pressione ENTER.  
  
    5.  No prompt de comando, digite **q**, e pressione ENTER.  
  
        1.  Tipo **transferir PDC**, pressione ENTER e, em seguida, clique em **Sim** no **confirmação de transferência de função** caixa de diálogo.  
  
        2.  Tipo **mestre de infraestrutura de transferência**, pressione ENTER e, em seguida, clique em **Sim** no **confirmação de transferência de função** caixa de diálogo.  
  
        3.  Tipo **transfira mestre de nomeação**, pressione ENTER e, em seguida, clique em **Sim** no **confirmação de transferência de função** caixa de diálogo.  
  
        4.  Tipo **mestre de RID transferência**, pressione ENTER e, em seguida, clique em **Sim** no **confirmação de transferência de função** caixa de diálogo.  
  
        5.  Tipo **mestre de esquema de transferência**, pressione ENTER e, em seguida, clique em **Sim** no **confirmação de transferência de função** caixa de diálogo.  
  
    6.  Tipo **q**, e pressione ENTER para voltar ao prompt de comando.  
  
> [!NOTE]
>  De qualquer servidor na rede, você pode verificar que as funções mestre de operações foram transferidas para o servidor de destino. Abra uma janela de Prompt de comando como administrador (para obter mais informações, consulte [para abrir uma janela de Prompt de comando como administrador](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)). Tipo **netdom consulta fsmo**, e pressione ENTER.  
  
## <a name="next-steps"></a>Próximas etapas  
 Você instalou o Windows Server Essentials como um controlador de domínio réplica novo. Acesse agora [etapa 3: ingressar computadores para o novo servidor Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  
  
Para ver todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

