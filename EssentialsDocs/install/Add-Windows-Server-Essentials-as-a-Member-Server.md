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
ms.openlocfilehash: 413c1d5d3bec7f002b73e9861aa119d495c5cfb2
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63717119"
---
# <a name="add-windows-server-essentials-as-a-member-server"></a>Adicionar o Windows Server Essentials como um servidor membro

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tópico se aplica a um servidor que executa o Windows Server 2012 R2 Standard, Windows Server 2012 R2 Datacenter ou Windows Server 2016 com a função experiência Windows Server Essentials instalada. No restante deste documento, a função Experiência do Windows Server Essentials será referenciada apenas como Windows Server Essentials.  
  
> [!NOTE]
>   Windows Server Essentials só pode ser implantado como controlador de domínio. Neste documento, o Windows Server Essentials não inclui o Windows Server Essentials.  
  
 O Windows Server Essentials não precisa ser um servidor primário em um domínio do Windows. Você pode adicionar o Windows Server Essentials como um servidor membro em um ambiente de domínio do Active Directory existente e aproveitar a proteção de dados simples, acesso remoto seguro e recursos de integração de nuvem que ele oferece. Além disso, o Windows Server Essentials pode ser implantado em um ambiente do Active Directory existente sem precisar ser um controlador de domínio. Isso permite estender armazenamento ou usar uma filial para administração e armazenamento local.  
  
 Você pode adicionar o Windows Server Essentials nos seguintes cenários:  
  
-   Adicionar o Windows Server Essentials em uma filial e associá-lo ao controlador de domínio que está localizado no escritório principal em um local separado, usando as ferramentas nativas. Você pode ativar os recursos do BranchCache para utilização ideal da largura de banda neste servidor membro.  
  
-   Adicione o Windows Server Essentials como servidor membro em uma rede do Windows Server Essentials para ajudar a estender o armazenamento em sua rede com a adição de pastas de servidor no servidor membro.  
  
-   Adicionar o Windows Server Essentials como servidor membro no escritório local se seu servidor primário que executa o Windows Server Essentials estiver hospedado no Microsoft Azure ou hospedado por terceiros. Ter o Windows Server Essentials como servidor membro em seu escritório local ajuda a otimizar a utilização de largura de banda.  
  
## <a name="adding-windows-server-essentials-as-a-member-server"></a>Adicionando o Windows Server Essentials como um servidor membro  
 Para adicionar o Windows Server Essentials como um servidor membro a um servidor primário que executam o Windows Server 2012 R2 ou Windows Server Essentials em um ambiente existente do Active Directory, você deve concluir as etapas a seguir:  
  
1.  Adicione o servidor que executa o Windows Server Essentials a um grupo de trabalho.  
  
2.  Junte-se o servidor que executa o Windows Server Essentials ao domínio de um servidor primário do Windows Server Essentials.  
  
3.  Configure a experiência do Windows Server Essentials do Gerenciador de servidores.  
  
#### <a name="to-join-windows-server-essentials-to-a-workgroup-or-domain"></a>Para adicionar o Windows Server Essentials a um domínio ou grupo de trabalho  
  
1.  Depois de concluir a instalação do Windows Server Essentials em seu segundo servidor, feche o Assistente Configurar o Windows Server Essentials.  
  
2.  Na caixa **Pesquisar** , digite **System Settings**e, nos resultados da pesquisa, clique em **Visualizar configurações avançadas do sistema**.  
  
3.  Em **Propriedades do Sistema**, clique na guia **Nome do Computador**.  
  
4.  Em **Nome do Computador**, na seção **Domínio**, clique em **Alteração**.  
  
5.  Na **alterações de nome/domínio do computador**, no **membro** , escolha se você deseja ingressar o servidor que executa o Windows Server Essentials para um **Workgroup** ou para um **Domínio**.  
  
    -   Para adicionar o servidor a um grupo de trabalho, digite **workgroup** e, em seguida, clique em **OK**.  
  
    -   Para associar este servidor a um domínio do Active Directory existente, digite o nome do domínio e clique em **OK**.  
  
6.  Reinicie o servidor para aplicar as alterações.  
  
 Após você ter adicionado o servidor ao seu domínio de s do servidor primário, você pode continuar a configurar o Windows Server Essentials executando o Assistente Configurar o Windows Server Essentials do Gerenciador de servidores.  
  
#### <a name="to-configure-windows-server-essentials-experience-on-a-member-server"></a>Para configurar a Experiência do Windows Server Essentials em um servidor membro  
  
1.  Se necessário, altere o nome do servidor (opcional).  
  
    > [!IMPORTANT]
    >  Depois de configurar a experiência do Windows Server Essentials, você não pode alterar o nome do servidor.  
  
2.  Autentique-se no servidor usando sua conta de administrador de domínio.  
  
3.  Abra o Gerenciador do Servidor.  
  
4.  Na área de notificação do sinalizador no **Gerenciador do Servidor**, clique no sinalizador e, em seguida, clique em **Configurar o Windows Server Essentials**.  
  
5.  Escolha configurar o servidor como um servidor membro e clique em **Avançar**.  
  
6.  Clique em **Configurar** para começar a configuração. O processo de configuração leva aproximadamente 10 minutos para ser concluído.  
  
7.  Na área de trabalho, clique no ícone do Painel para iniciar o Painel do servidor. Na Página inicial, conclua as tarefas **Introdução** listadas na guia **Configuração** .  
  
## <a name="see-also"></a>Consulte também  
  

-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)

-   [Instalar o Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)

