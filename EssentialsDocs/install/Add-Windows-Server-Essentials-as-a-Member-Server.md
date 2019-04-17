---
title: Adicionar o Windows Server Essentials como um servidor membro
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d09dd82f-f7d2-47ce-862d-fd9869f2021c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8fb73f8186d3984c9e93f7a6e39cb72a54db1e58
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="add-windows-server-essentials-as-a-member-server"></a>Adicionar o Windows Server Essentials como um servidor membro

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tópico se aplica a um servidor que executa o Windows Server 2012 R2 Standard, Windows Server 2012 R2 Datacenter ou Windows Server 2016 com a função de experiência do Windows Server Essentials instalada. No restante deste documento, a função de experiência do Windows Server Essentials será chamada como Windows Server Essentials.  
  
> [!NOTE]
>   Windows Server Essentials só pode ser implantado como controlador de domínio. Neste documento, o Windows Server Essentials não inclui o Windows Server Essentials.  
  
 Windows Server Essentials não precisa ser um servidor primário dentro de um domínio do Windows. Você pode adicionar o Windows Server Essentials como um servidor membro em um ambiente de domínio do Active Directory existente e aproveitar os recursos de integração de nuvem que fornece, proteção de dados simples e acesso remoto seguro. Além disso, o Windows Server Essentials pode ser implantado em um ambiente do Active Directory existente sem precisar ser um controlador de domínio. Isso permite que você expandir o armazenamento ou usar uma filial de administração e armazenamento local.  
  
 Você pode adicionar o Windows Server Essentials nos seguintes cenários:  
  
-   Adicione o Windows Server Essentials em uma filial e associá-lo ao controlador de domínio que está localizado na sede da empresa em um local separado usando as ferramentas nativas. Você pode ativar os recursos de BranchCache para utilização de largura de banda ótima nesse servidor membro.  
  
-   Adicione o Windows Server Essentials como um servidor membro em uma rede do Windows Server Essentials para ajudar a expandir o armazenamento em sua rede, adicionando pastas de servidor adicionais em seu servidor membro.  
  
-   Adicionar o Windows Server Essentials como um servidor membro no escritório local se o servidor principal que executa o Windows Server Essentials é hospedado no Microsoft Azure ou hospedado por um hoster de terceiros. Com o Windows Server Essentials como um servidor membro no seu site de local do office ajuda a otimizar o uso de largura de banda.  
  
## <a name="adding-windows-server-essentials-as-a-member-server"></a>Adicionando o Windows Server Essentials como um servidor membro  
 Para adicionar o Windows Server Essentials como um servidor membro em um servidor primário executando o Windows Server 2012 R2 ou Windows Server Essentials em um ambiente existente do Active Directory, você deve concluir as etapas a seguir:  
  
1.  Junte-se o servidor que executa o Windows Server Essentials para um grupo de trabalho.  
  
2.  Junte-se o servidor que executa o Windows Server Essentials para o domínio de um servidor Windows Server Essentials primário.  
  
3.  Configure a experiência do Windows Server Essentials do Gerenciador do servidor.  
  
#### <a name="to-join-windows-server-essentials-to-a-workgroup-or-domain"></a>Para participar do Windows Server Essentials em um grupo de trabalho ou domínio  
  
1.  Depois de concluir a instalação do Windows Server Essentials no segundo servidor, feche o Assistente Configurar do Windows Server Essentials.  
  
2.  No **pesquisa** , digite **configurações do sistema**e nos resultados da pesquisa, clique em **exibir configurações avançadas do sistema**.  
  
3.  Em **propriedades do sistema**, clique no **nome do computador** guia.  
  
4.  Em **nome do computador**, além do **domínio** seção, clique em **alteração**.  
  
5.  Em **alterações de nome/domínio do computador**, no **membro** seção, escolha se deseja participar do servidor que executa o Windows Server Essentials para um **Workgroup** ou para um **domínio**.  
  
    -   Para adicionar o servidor a um grupo de trabalho, digite **workgroup**e clique em **Okey**.  
  
    -   Para ingressar este servidor em um domínio do Active Directory existente, digite o nome do domínio e, em seguida, clique em **Okey**.  
  
6.  Reinicie o servidor para aplicar as alterações.  
  
 Depois de você ter adicionado o servidor ao domínio s servidor primário, você pode continuar a configurar o Windows Server Essentials, executando o Assistente Configurar do Windows Server Essentials do Gerenciador do servidor.  
  
#### <a name="to-configure-windows-server-essentials-experience-on-a-member-server"></a>Para configurar a experiência do Windows Server Essentials em um servidor membro  
  
1.  (Opcional) Altere o nome do servidor, se necessário.  
  
    > [!IMPORTANT]
    >  Você não pode alterar o nome do servidor depois de configurar a experiência do Windows Server Essentials.  
  
2.  Entrar no servidor usando sua conta de administrador do domínio.  
  
3.  Abra o Gerenciador do servidor.  
  
4.  Na área de notificação sinalizador **Gerenciador do servidor**, clique no sinalizador e, em seguida, clique em **configurar o Windows Server Essentials**.  
  
5.  Aceitar para configurar o servidor como um servidor membro e clique em **próxima**.  
  
6.  Clique em **configurar** para começar a configuração. O processo de configuração leva cerca de 10 minutos para ser concluída.  
  
7.  Na área de trabalho, clique no ícone de painel para iniciar o painel do servidor. Na Home page, conclua a **Introdução** tarefas que estão listadas no **instalação** guia.  
  
## <a name="see-also"></a>Consulte também  
  

-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)

-   [Instalar o Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)

