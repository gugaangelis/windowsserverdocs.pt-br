---
title: Conectar-se no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 05/07/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 149a5d34-43b7-4b9e-99e7-9f2294ab9ddb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1d7f3c33f1254c8dbe4af8bdf5baa4144c134248
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="get-connected-in-windows-server-essentials"></a>Conectar-se no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Você pode conectar seus computadores para o servidor Windows Server Essentials usando o conector software. O software Connector é instalado quando você conectar um computador com o servidor usando a conectar um computador para o Assistente do servidor. Você pode iniciar o assistente digitando **http://<servername\>/connect**, onde **< servername\ >** é o nome do seu servidor.  
  
 Neste tópico:  
  

-   [Preparar para conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  
  
-   [Conecte computadores ao servidor usando o software Connector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  
  
-   [Uso da barra inicial](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

-   [Preparar para conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  
  
-   [Conecte computadores ao servidor usando o software Connector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  
  
-   [Uso da barra inicial](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

  
##  <a name="BKMK_A"></a>Preparar para conectar computadores para o servidor  
 Esta seção discute o software do conector, os sistemas operacionais compatíveis com o Windows Server Essentials, as tarefas de pré-requisito que devem ser concluídas antes de se conectar seus computadores para o servidor e as alterações feitas pelo servidor aos computadores quando você executa o software Connector.  
  

-   [Visão geral de software do conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Pré-requisitos para se conectar a um computador para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Pré-requisitos para um computador Mac se conectar à rede](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Sistemas operacionais com suporte para computadores cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Alterações feitas pelo servidor em um computador cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Informações de nome e a senha do usuário de rede](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Conta de administrador s do servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Remover um computador de um domínio do Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  
  
###  <a name="BKMK_1"></a>Visão geral de software do conector  
 O software Connector para o sistema operacional Windows Server Essentials conecta os computadores em sua rede para o servidor Windows Server Essentials. Quando você conectar computadores para o servidor, o software Connector permite que você faça backup dos computadores automaticamente e monitorar sua integridade. O software Connector também permite que você configure e administrar remotamente o servidor Windows Server Essentials. O software Connector é instalado quando você conectar um computador cliente para o servidor. Para obter instruções detalhadas sobre como conectar os computadores cliente para o servidor Windows Server Essentials, consulte [conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) posteriormente neste tópico.  

-   [Visão geral de software do conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Pré-requisitos para se conectar a um computador para o servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Pré-requisitos para um computador Mac se conectar à rede](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Sistemas operacionais com suporte para computadores cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Alterações feitas pelo servidor em um computador cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Informações de nome e a senha do usuário de rede](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Conta de administrador s do servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Remover um computador de um domínio do Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  
  
###  <a name="BKMK_1"></a>Visão geral de software do conector  
 O software Connector para o sistema operacional Windows Server Essentials conecta os computadores em sua rede para o servidor Windows Server Essentials. Quando você conectar computadores para o servidor, o software Connector permite que você faça backup dos computadores automaticamente e monitorar sua integridade. O software Connector também permite que você configure e administrar remotamente o servidor Windows Server Essentials. O software Connector é instalado quando você conectar um computador cliente para o servidor. Para obter instruções detalhadas sobre como conectar os computadores cliente para o servidor Windows Server Essentials, consulte [conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) posteriormente neste tópico.  

  
###  <a name="BKMK_2"></a>Pré-requisitos para se conectar a um computador para o servidor  
 Os seguintes requisitos devem ser atendidos antes de conectar um computador à rede:  
  
-   A instalação do Windows Server Essentials for concluída, e o servidor está sendo executado. O software Connector sairá sua instalação, se não puder se comunicar com o servidor.  
  

-   O computador cliente está executando um sistema operacional compatível. Para obter mais informações, consulte [sistemas operacionais com suporte para computadores cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4).

  
-   O computador cliente deve ter uma conexão válida com a Internet.  
  
-   O computador cliente está na mesma sub-rede IP do servidor que está executando o Windows Server Essentials quando o computador cliente está na mesma rede que o servidor.  
  
-   O computador cliente tiver .NET Framework 4.5 instalado nele. O software Connector automaticamente o instala no computador.  
  
-   O computador cliente atenda aos seguintes requisitos mínimos do sistema:  
  
    -   1,4 GHz ou processador mais rápido  
  
    -   1 GB de RAM ou mais  
  
    -   1 GB de espaço disponível no disco rígido (uma parte desse espaço é liberada após a instalação)  
  
-   A partição de inicialização (ou seja, a partição de disco em que o sistema operacional Windows está instalado) é formatada com o sistema de arquivos NTFS.  
  
-   O nome do computador não inclui mais de 15 caracteres.  
  
-   O nome do computador não incluir um sublinhado (_).  
  
-   As configurações de data e hora do computador s alinham para as configurações no servidor.  
  
-   Um computador cliente pode ser conectado a um único servidor Windows Server Essentials a qualquer momento.  
  
-   Um computador cliente que já ingressou em outro domínio do Active Directory não pode ingressar em um domínio do Windows Server Essentials.  
  
> [!NOTE]

>  Em uma implantação local do cliente para o Windows Server Essentials ou o Windows Server Essentials, você pode conectar computadores para o servidor sem adicioná-los para o domínio do Windows Server Essentials. Esse método não está disponível para todos os sistemas operacionais cliente com suporte e recursos como a política de grupo e redes virtuais privadas (VPNs), que exigem que um computador esteja conectado ao domínio, não estão disponíveis. Para requisitos e instruções, consulte [conectar computadores a um servidor Windows Server Essentials sem ingressar no domínio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  
  
 Para obter instruções passo a passo para se conectar a um computador para o servidor que executa o Windows Server Essentials, consulte [conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

>  Em uma implantação local do cliente para o Windows Server Essentials ou o Windows Server Essentials, você pode conectar computadores para o servidor sem adicioná-los para o domínio do Windows Server Essentials. Esse método não está disponível para todos os sistemas operacionais cliente com suporte e recursos como a política de grupo e redes virtuais privadas (VPNs), que exigem que um computador esteja conectado ao domínio, não estão disponíveis. Para requisitos e instruções, consulte [conectar computadores a um servidor Windows Server Essentials sem ingressar no domínio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  
  
 Para obter instruções passo a passo para se conectar a um computador para o servidor que executa o Windows Server Essentials, consulte [conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
###  <a name="BKMK_3"></a>Pré-requisitos para um computador Mac se conectar à rede  
 Os seguintes requisitos devem ser atendidos antes de conectar um computador Mac à rede:  
  
-   A instalação do sistema operacional do servidor for concluída, e o servidor está sendo executado. O software Connector não será instalado se ele não poderão se comunicar com o servidor.  
  
-   O computador está executando o Mac OS X 10.5 (Leopard) ou posterior.  
  
-   O computador está na mesma sub-rede IP do servidor.  
  
-   O computador deve ter uma conexão válida com a Internet.  
  
-   Certifique-se de que o computador atende aos seguintes requisitos mínimos do sistema:  
  
    -   1,4 GHz ou processador mais rápido  
  
    -   1 GB de RAM ou mais  
  
    -   1 GB de espaço disponível no disco rígido (uma parte desse espaço é liberada após a instalação)  
  
-   Um computador cliente pode ser conectado a um único servidor a qualquer momento.  
  
###  <a name="BKMK_4"></a>Sistemas operacionais com suporte para computadores cliente  
 Windows Server Essentials fornece o mesmo conjunto de recursos para todos os computadores cliente com suporte. Esses recursos incluem o ingresso no domínio, na barra inicial e notificações de integridade do lado do cliente.  
  
> [!IMPORTANT]
>  Windows Server Essentials não oferece suporte a ingressando em computadores que executam as versões do Windows Home, Starter ou Media Center ao domínio. Além disso, você não pode usar o acesso via Web remoto para se conectar a esses computadores.  
  
#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  Esta seção se aplica a um servidor que executa o Windows Server Essentials, ou para um servidor que executa o Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter com a função de experiência do Windows Server Essentials instalada. Há suporte para os seguintes sistemas operacionais de computador:  
  
 **Sistemas operacionais Windows 7**  
  
-    Windows 7 SP1 básica inicial (x x86 e x64)  
  
-    Windows 7 Home Premium SP1 (x86 e x64)  
  
-    Windows 7 SP1 profissional (x x86 e x64)  
  
-    Windows 7 Ultimate SP1 (x86 e x64)  
  
-    Windows 7 Enterprise SP1 (x86 e x64)  
  
-    Windows 7 Starter SP1 (x86)  
  
 **Sistemas operacionais Windows 8**  
  
-   Windows 8  
  
-   Professional do Windows 8  
  
-   Windows 8 Enterprise  
  
 **Sistemas operacionais Windows 8.1**  
  
-   Windows 8.1  
  
-   Windows 8.1 Professional  
  
-   Windows 8.1 Enterprise  
  
 **Sistemas operacionais Windows 10**  
  
-   Windows 10  
  
-   Windows 10 Professional  
  
-   Windows 10 Enterprise  
  
-   Windows 10 Education  
  
 **Computadores cliente Mac**  
  
-   Mac OS X v 10.5 Leopard  
  
-   Mac OS X v10.6 Snow Leopard  
  
-   Mac OS X v 10.7 Leão  
  
-   Mac OS X v10.8 Mountain Lion  
  
> [!NOTE]
>  Você pode exibir a integridade e o status do backup de um computador Mac no painel Windows Server Essentials. No entanto, você não pode configurar o backup do computador ou inicie um backup do painel. Além disso, você não pode usar o acesso via Web remoto para se conectar a um computador Mac.  
  
#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  Esta seção se aplica a um servidor que executa o Windows Server Essentials. Há suporte para os seguintes sistemas operacionais de computador:  
  
 **Sistemas operacionais Windows 7**  
  
-    Windows 7 Home Basic (x86 and x64)  
  
-    Windows 7 Home Premium (x86 and x64)  
  
-    Windows 7 Professional (x86 and x64)  
  
-    Windows 7 Ultimate (x86 and x64)  
  
-    Windows 7 Enterprise (x86 and x64)  
  
-    Windows 7 Starter (x86)  
  
 **Sistemas operacionais Windows 8**  
  
-   Windows 8  
  
-   Professional do Windows 8  
  
-   Windows 8 Enterprise  
  
 **Sistemas operacionais Windows 10**  
  
-   Windows 10  
  
-   Windows 10 Professional  
  
-   Windows 10 Enterprise  
  
-   Windows 10 Education  
  
 **Computadores cliente Mac**  
  
-   Mac OS X v 10.5 Leopard  
  
-   Mac OS X v10.6 Snow Leopard  
  
-   Mac OS X v 10.7 Leão  
  
> [!NOTE]
>  Você pode exibir a integridade e o status do backup de um computador Mac no painel Windows Server Essentials. No entanto, você não pode configurar o backup do computador ou inicie um backup do painel. Além disso, você não pode usar o acesso via Web remoto para se conectar a um computador Mac.  
  
###  <a name="BKMK_5"></a>Alterações feitas pelo servidor em um computador cliente  
 Quando você conectar um computador para o servidor, o software do Windows Server Essentials torna um número de alterações no computador para que o computador e o servidor podem trabalhar juntos.  
  
 O software faz o seguinte:  
  
-   Instala o software Connector no computador  
  
-   Instala o Microsoft .NET Framework 4.5 no computador se ele já não estiver instalado  
  
-   Cria atalhos na área de trabalho computador s para o painel e da barra inicial  
  
-   Configura portas de Firewall do Windows no computador para permitir que os recursos a seguir funcionar:  
  
    -   Rede principal  
  
    -   Serviços de área de trabalho remota  
  
-   Faz as seguintes alterações no computador para facilitar backups:  
  
    -   Cria as tarefas agendadas para executar backups automáticos  
  
    -   Instala os serviços que gerenciam as operações de backup com o servidor  
  
    -   Instala um driver de dispositivo virtual é usado durante a processos de restaurar arquivos e pastas  
  
-   Instala o agente de integridade para detectar problemas e criar as notificações de alerta correspondentes  
  
-   Cria as tarefas agendadas no computador para recorrentes avaliações de integridade e para sincronizar as definições de alerta de integridade  
  
-   Adiciona serviços no computador, o que o computador usa para se comunicar com o servidor e com outros recursos do Windows Server Essentials  
  
-   Abre a porta TCP 3389 em computadores cliente que executam aos clientes do Windows para permitir que os serviços de área de trabalho remota executar  
  
-   Implanta VPN no computador cliente e oferece uma experiência de clique único se a funcionalidade VPN está habilitada no Windows Server Essentials, ou ainda fornece uma experiência de auto-connect se a funcionalidade VPN estiver habilitada no Windows Server Essentials  
  

 Para obter informações sobre como conectar seu computador para o servidor, consulte [conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
###  <a name="BKMK_6"></a>Informações de nome e a senha do usuário de rede  
 Você pode obter suas informações de nome e a senha de usuário rede com a pessoa que gerencia seu servidor. Você pode usar essas credenciais para conectar o computador para o servidor e acessar informações do servidor.  
  
###  <a name="BKMK_6"></a>Informações de nome e a senha do usuário de rede  
 Você pode obter suas informações de nome e a senha de usuário rede com a pessoa que gerencia seu servidor. Você pode usar essas credenciais para conectar o computador para o servidor e acessar informações do servidor. 

  
 Se você for o administrador do servidor, você pode criar as credenciais de rede, adicionando uma conta de usuário do **usuários** guia do painel. Para obter mais informações sobre contas de usuário, consulte [gerenciar contas de usuário usando o painel](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8).  
  
###  <a name="BKMK_7"></a>Conta de administrador s do servidor  
 Você deve ser capaz de fornecer um nome de conta de administrador de rede e uma senha para instalar o software do conector. Uma conta de administrador de rede permite que o usuário gerenciar a rede local para sua organização e ajuda a gerenciar e manter os dispositivos de rede como comutadores e roteadores.  
  
 As tarefas que podem ser executadas usando uma conta de administrador de rede podem incluir:  
  
-   Instalar aplicativos em rede e outros softwares  
  
-   Gerenciar armazenamento no servidor  
  
-   Distribuindo atualizações de software  
  
-   Fazer backups de rotina  
  
-   Monitoramento de atividades diárias na rede  
  
 No Windows Server Essentials, Windows Server Essentials e Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, você pode atribuir o administrador da rede acessem nível para qualquer conta de usuário. Isso concede as permissões necessárias para executar tarefas do administrador de rede. Quando um usuário é atribuído o administrador da rede acessem nível, o **controle de acesso de usuário** prompt é aberto para qualquer tarefa que exige permissões de administrador.  
  
###  <a name="BKMK_8"></a>Remover um computador de um domínio do Windows  
 Para remover um computador do seu domínio, você será solicitado para o nome de usuário e a senha da conta de domínio.  
  
##### <a name="to-remove-a-computer-from-a-windows-domain"></a>Para remover um computador de um domínio do Windows  
  
1.  Clique em **iniciar**, clique com botão direito **computador**e clique em **propriedades**.  
  
2.  Em **configurações de grupo de trabalho, domínio e nome de computador**, clique em **alterar configurações**.  
  
    > [!NOTE]
    >  Se você for solicitado uma senha de administrador ou uma confirmação, digite a senha de domínio ou forneça a confirmação.  
  
3.  No **propriedades do sistema** caixa de diálogo, clique no **nome do computador** guia e, em seguida, clique em **alteração**.  
  
4.  No **alterações de nome/domínio do computador** caixa de diálogo, em **membro do**, clique em **Workgroup**, e siga um destes procedimentos:  
  
    1.  Para ingressar em um grupo de trabalho existente, digite o nome do grupo de trabalho que você deseja ingressar e clique em **Okey**.  
  
    2.  Para criar um grupo de trabalho, digite o nome do grupo de trabalho que você deseja criar e, em seguida, clique em **Okey**.  
  
        > [!NOTE]
        >  Seu computador será removido do domínio e sua conta de computador no domínio será desabilitada.  
  
##  <a name="BKMK_B"></a>Conecte computadores ao servidor usando o software Connector  
 Esta seção fornece acesso às informações que ajudarão você a instalar o software do conector, conecte seu computador para o servidor e solucionar problemas de computadores que se conectam ao servidor e procedimentos.  
  

-   [Conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Conectar computadores a um servidor Windows Server Essentials sem ingressar no domínio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Instale o software do conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Mover dados de computador e as configurações manualmente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Transferir vários perfis de usuário durante a implantação do computador](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  
  
-   [Desinstalar o software do conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Desconecte seu computador contra ou reconecte seu computador para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Como fazer backup funciona com a suspensão e hibernação](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

-   [Conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Conectar computadores a um servidor Windows Server Essentials sem ingressar no domínio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Instale o software do conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Mover dados de computador e as configurações manualmente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Transferir vários perfis de usuário durante a implantação do computador](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  
  
-   [Desinstalar o software do conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Desconecte seu computador contra ou reconecte seu computador para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Como fazer backup funciona com a suspensão e hibernação](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

  
###  <a name="BKMK_9"></a>Conectar computadores para o servidor  
 Quando você conectar um computador para um servidor que está executando o Windows Server Essentials ou o Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, certifique-se de que o computador cliente tem uma conexão válida com a Internet.  
  
 Conclua o procedimento a seguir em todos os computadores cliente para conectá-los ao seu servidor.  
  
 Para concluir o procedimento, você precisa ter as seguintes informações:  
  
-   O nome de usuário e senha da pessoa que usará o computador na nova rede  
  
-   O nome de usuário e senha da conta de administrador local do computador s  
  
> [!NOTE]

>  Em uma implantação local do cliente para o Windows Server Essentials ou o Windows Server Essentials, você pode conectar computadores para o servidor sem adicioná-los para o domínio do Windows Server Essentials. Esse método não está disponível para todos os sistemas operacionais cliente com suporte e recursos como a política de grupo e redes virtuais privadas (VPNs), que exigem que um computador esteja conectado ao domínio, não estão disponíveis. Para requisitos e instruções, consulte [conectar computadores a um servidor Windows Server Essentials sem ingressar no domínio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

>  Em uma implantação local do cliente para o Windows Server Essentials ou o Windows Server Essentials, você pode conectar computadores para o servidor sem adicioná-los para o domínio do Windows Server Essentials. Esse método não está disponível para todos os sistemas operacionais cliente com suporte e recursos como a política de grupo e redes virtuais privadas (VPNs), que exigem que um computador esteja conectado ao domínio, não estão disponíveis. Para requisitos e instruções, consulte [conectar computadores a um servidor Windows Server Essentials sem ingressar no domínio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

  
##### <a name="to-connect-a-client-computer-to-the-server"></a>Para se conectar a um computador cliente para o servidor  
  
1.  Fazer logon no computador em que você deseja se conectar ao servidor.  
  
    > [!NOTE]
    >  Se este computador tiver várias contas de usuário, faça logon usando a conta de usuário que possui documentos, fotos e preferências pessoais que você deseja manter, depois que você conecte o computador para o servidor.  
  
2.  Abra um navegador da Internet, como o Internet Explorer.  
  
3.  Na barra de endereços, digite **http://<servername\>/Connect**, e pressione Enter.  
  
    > [!NOTE]
    >  Se seu computador estiver em um local remoto fora da rede do Windows Server Essentials, para executar a conectar um computador para o Assistente de servidor, digite **http://<domainname\>/connect** na barra de endereços do navegador da web (onde < domain\ > é o nome de domínio da sua organização). Você pode obter as informações de nome de domínio ao administrador de rede.  
  
4.  O **conectar seu computador para o servidor** página será exibida. Siga um destes procedimentos:  
  
    -   Para um computador executando o sistema operacional Windows, clique em **Download de software para o Windows**.  
  
    -   Para um computador executando o Mac OS X ou posterior, clique em **baixar software para Mac**.  
  
5.  Na mensagem de aviso de segurança de download de arquivo, clique em **executar**.  
  
6.  Se aparecer a mensagem de controle de conta de usuário, clique em **Sim** ou digite o nome de usuário local e a senha, se solicitado.  
  
7.  Conectar-se um computador para o Assistente do servidor é exibida. Faça o seguinte para concluir o Assistente:  
  
    1.  Aceite o contrato de licença de usuário final.  
  
    2.  Sobre o **encontrar meu servidor** de página, detectar automaticamente o servidor em redes de locais e selecione o servidor que você deseja se conectar. Ou, se você tiver as informações, você pode inserir manualmente seu endereço de servidor s nome ou domínio.  
  
    3.  Sobre o **digite o novo nome de usuário de rede e a senha** página, faça o seguinte:  
  
        -   Se esse for o primeiro computador que você está se conectando ao servidor, e se esse for o computador que você usará para administrar o servidor, use a conta de administrador que você criou durante a instalação.  
  
        -   Para todos os outros computadores, primeiro crie uma conta de usuário de rede no servidor usando o Dashboard. Criar a conta de usuário com o administrador ou padrão de privilégios de usuário, com base nas tarefas que são executadas a pessoa usando o computador.  
  
    4.  Se o computador estiver executando o Windows 8, Windows 8.1 ou Windows 10, ignore esta etapa. Se o computador estiver executando o Windows 7, e se você tiver documentos, imagens ou preferências pessoais (como telas de fundo da área de trabalho, proteções de tela ou Favoritos do Internet Explorer) que você deseja manter depois de ingressar o computador para a nova rede, no **escolha se você deseja mover os dados existentes e configurações** página do assistente, selecione o **mover meus dados e configurações para minha nova conta de usuário de rede**.  
  
    5.  Escolha se deseja ativar automaticamente o computador para criar um backup no **escolher se deseja ativar este computador para criar seu backup** página.  
  
    6.  Siga as demais instruções do Assistente para ingressar o computador à rede.  
  
8.  Depois de ingressar o computador à rede, use o novo nome de usuário e senha para fazer logon no computador.  
  
    > [!NOTE]
    >  Quando você fizer logon um computador que esteja executando o Windows 8 pela primeira vez usando sua conta de rede, depois que ele se conecta ao servidor, instruções para migrar arquivos e a conta antiga do usuário de aplicativos serão exibidas. Siga as instruções na **como migrar arquivos e aplicativos da minha conta de usuário antigo? **página migrar todos os arquivos e aplicativos para a conta de usuário de rede.  
  
9. Depois que o computador esteja conectado com êxito ao servidor, atalhos para o conector TrayApp e o servidor painel aparecem no menu Iniciar, que pode ser usado da seguinte forma (se o computador estiver executando o Windows 8, Windows 8.1 ou Windows 10, o painel e conector TrayApp estará disponível na tela inicial do computador s):  
  
    -   Se seu computador estiver executando o Windows 8, Windows 8.1 ou Windows 10, o painel e conector TrayApp será pesquisável como um aplicativo.  
  
    -   De TrayApp conector, você pode habilitar ou desabilitar o **Mantenha-me conectado remotamente** recurso. Você também pode clicar duas vezes TrayApp para começar a barra inicial. Da barra inicial, você pode acessar o atalho de pastas compartilhadas, configurar os backups do computador, alertas do endereço e abra o site de acesso via Web remoto.  
  
    -   Do **painel** link, você pode administrar seu servidor.  
  
###  <a name="BKMK_10"></a>Conectar computadores a um servidor Windows Server Essentials sem ingressar no domínio  
 Este tópico descreve como adicionar um computador Windows 7, Windows 8, Windows 8.1 ou Windows 10 a uma rede do Windows Server Essentials sem ingressar o computador no domínio do Windows Server Essentials em uma implantação local do cliente. Esse método de conexão é suportado no Windows Server Essentials e Windows Server Essentials.  
  
 Isso é uma alternativa para o método usual, que requer ingressar o computador no domínio do Windows Server Essentials. Com esse método, se o computador estiver em outro domínio, ele deve ser removido do domínio antes que ele pode ser adicionado no domínio do Windows Server Essentials.  
  
#### <a name="feature-limits"></a>Limites de recursos  
 Alguns recursos são limitados quando um computador cliente não é adicionado ao domínio do Windows Server Essentials:  
  
-   Todos os recursos que exigem que o computador ingressar no domínio? incluindo as credenciais de domínio, política de grupo e redes virtuais privadas (VPNs)? não estão disponíveis.  
  
-   Os complementos de terceiros que exigem que o computador ingressar no domínio não funcionará corretamente.  
  
-   Esse método não pode ser usado para se conectar a um computador fora do local para o servidor.  
  
#### <a name="prerequisites"></a>Pré-requisitos  
  
-   O computador deve ter uma conexão à rede local físico.  
  
-   Um dos seguintes sistemas operacionais deve ser instalado no computador:  
  
    -   Windows 10 Pro, Enterprise do Windows 10  
  
    -    Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 8 Pro, Windows 8 Enterprise  
  
    -    Windows 7 Professional (x86 e x64), Windows 7 Enterprise (x86 e x64), Windows 7 Ultimate (x86 e x64)  
  

-   O computador deve atender a todos os outros requisitos para computadores cliente no Windows Server Essentials. Para obter mais informações, consulte [pré-requisitos para se conectar a um computador para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2).  

  
-   Para habilitar uma conexão sem ingressar no domínio, você deve entrar para o computador com uma conta que é um membro do grupo Administradores local.  
  
-   Para conectar o computador para o servidor Windows Server Essentials, você precisará as seguintes informações de conta:  
  
    -   Uma conta com direitos de administrador no Windows Server Essentials (sua conta).  
  
    -   O nome de usuário e senha da conta de domínio da pessoa que usará o computador. A conta de domínio também deve ter direitos de administrador no servidor Windows Server Essentials.  
  
#### <a name="connect-to-a-windows-server-essentials-network"></a>Conectar a uma rede do Windows Server Essentials  
 Depois de verificar se todos os pré-requisitos foram atendidos, conecte o computador à rede do Windows Server Essentials.  
  
###### <a name="to-connect-a-computer-in-a-different-domain-to-a-windows-server-essentials-network"></a>Para se conectar a um computador em um domínio diferente a uma rede do Windows Server Essentials  
  
1.  Entrar computador cliente com uma conta que é um membro do grupo Administradores local.  
  
2.  Abra um prompt de comando com direitos de administrador.  
  
    -   No Windows 10, clique no **iniciar** botão, selecione **todos os aplicativos** -> **ferramentas do sistema Windows** -> **Prompt de comando**, clique com botão direito Prompt de comando e clique em **executar como administrador**.  
  
    -   No Windows 8, no **iniciar** página, digite **comando** e pressione Enter. Nos resultados, clique com botão direito **Prompt de comando**e clique em **executar como administrador**.  
  
    -   No Windows 7, no **iniciar** menu, insira **comando** na caixa de pesquisa, clique com botão direito **Prompt de comando**e clique em **executar como administrador**.  
  
3.  No prompt de comando, digite o seguinte comando e pressione Enter:  
  
    ```  
    reg add "HKLM\SOFTWARE\Microsoft\Windows Server\ClientDeployment" /v SkipDomainJoin /t REG_DWORD /d 1  
    ```  
  

4.  Conclua as etapas na [conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
####  <a name="BKMK_SecondServer"></a>Ingressar em um segundo servidor na rede  
  
###### <a name="to-join-a-second-server-to-the-network"></a>Para participar de um segundo servidor na rede  
  
1.  Fazer logon no servidor que você deseja se conectar à rede Windows Server Essentials.  
  
2.  Abra um navegador da Internet e na barra de endereços, digite **http://<servername\>/Connect**, onde *< servername\ >* é o nome do servidor que executa o Windows Server Essentials e pressione Enter.  
  
3.  Se a configuração de segurança aprimorada do Internet Explorer está habilitada no servidor que você está tentando se conectar à rede do Windows Server Essentials, conclua as seguintes; Caso contrário, pule esta etapa.  
  
    1.  Para aceitar a mensagem de bloqueio, clique em **fechar**.  
  
    2.  Adicione o **http://<servername\>/Connect** site para os sites confiáveis da seguinte maneira:  
  
        1.  No painel de navegação do navegador, clique em **ferramentas**e clique em **opções da Internet**.  
  
        2.  Clique no **segurança** guia e, em seguida, clique em **sites confiáveis**.  
  
        3.  Clique em **Sites**.  
  
        4.  O site deve ser mostrado no **adicionar este site à zona** campo. Clique em **adicionar**.  
  
        5.  Clique em **fechar**e clique em **Okey**.  
  
    3.  Atualize a página da web.  
  

    4.  Para conectar o segundo servidor a um servidor que executa o Windows Server Essentials, siga as instruções em [conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
    > [!NOTE]
    >  Conectando-se um segundo servidor a um servidor que executa o Windows Server Essentials difere de se conectar a um computador cliente da seguinte maneira:  
    >   
    >  -   Não há nenhuma migração de perfis de usuário; Portanto, o perfil atual não será migrado.  
    > -   Você não pode fazer backup do segundo servidor usando o backup do computador cliente, e não há nenhuma opção para ativar o computador para backup.  
  
 Depois de ingressar o segundo servidor em um servidor que está executando o Windows Server Essentials, os seguintes recursos são fornecidos para o servidor conectado:  
  
-   Atualização e funcionalidades de status de alertas funcionam da mesma forma outros computadores cliente.  
  
-   Funcionalidades online e Offline funcionam da mesma forma outros computadores cliente.  
  
-   Você pode se conectar ao seu servidor segundo usando o acesso via Web remoto.  
  
-   O segundo servidor será incluído nos relatórios integridade porque o Windows Server Essentials irá gerar alertas relacionadas a este servidor.  
  
 Gerenciamento do segundo servidor do servidor que está executando o Windows Server Essentials será diferente do gerenciamento de outros computadores cliente da seguinte maneira:  
  
-   Não haverá nenhum ponto de entrada para TrayApp, na barra inicial e cliente de painel.  
  
-   O segundo servidor está listado no **servidores** agrupar o **dispositivos** guia.  
  
-   Porque não há suporte para o backup do computador cliente para o segundo servidor, o status do backup é exibido como **não tem suporte**. Além disso, não se você selecionar o servidor e o botão direito do mouse a segunda, há nenhum backup e restauração tarefas relacionadas exibidas para o segundo servidor.  
  
-   Se você selecionar o segundo servidor e, em seguida, clique no **exibir as propriedades de servidor** de tarefas, há nenhuma **Backup** guia exibida na página de propriedades de servidor s.  
  
-   Como não há nenhum centro de segurança em um sistema operacional Windows Server, o status de segurança do servidor s segundo exibe como **não aplicável**.  
  
-   O segundo servidor s status de política de grupo exibe como **não aplicável**.  
  
###  <a name="BKMK_11"></a>Instale o software do conector  
 O software Connector no Windows Server Essentials é instalado quando você conecta seu computador para o servidor usando a conectar um computador para o Assistente do servidor. Você pode iniciar esse assistente, digitando **http://<ServerName\>/connect** na barra de endereços do navegador da web (onde *< ServerName\ >* é o nome do seu servidor).  
  
> [!NOTE]
>  Se seu computador estiver em um local remoto, para executar a conectar um computador para o Assistente de servidor, digite **http://<domainname\>/connect** na barra de endereços do navegador da web (onde *< domain\ >* é o nome de domínio da sua organização). Você pode obter as informações de nome de domínio ao administrador de rede.  
  
 O software Connector faz o seguinte:  
  
-   Conecta seu computador para o Windows Server Essentials  
  
-   Faz backup automático do computador noturno (se você configurar o servidor para criar backups de cliente)  
  
-   Ajuda a administrador monitorar a integridade do computador  
  
-   Permite que você configure e administrar remotamente o Windows Server Essentials em seu computador doméstico  
  

 Para obter instruções passo a passo sobre como conectar seu computador para o servidor Windows Server Essentials, consulte [conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).   

  
###  <a name="BKMK_12"></a>Mover dados de computador e as configurações manualmente  
  Windows Server Essentials e Windows Server Essentials suportam migração de perfis de usuário apenas para computadores cliente que executam o sistema operacional Windows 7. Quando você conectar um computador baseado no Windows 7 para o servidor, o computador se conectar ao servidor assistente pode migrar automaticamente o perfil de usuário.  
  
 O perfil de usuário não pode ser transferido automaticamente ao conectar o Windows 8, Windows 8.1 ou no computador Windows 10 para o servidor. No entanto, em um computador Windows 8, você pode usar a transferência fácil do Windows para transferir dados e configurações do usuário local original no computador ingressado no domínio. Para fazer isso, você deve ser um administrador no computador de origem do Windows 8 e o computador de destino do Windows 8. Para obter informações sobre como usar a transferência fácil do Windows para transferir arquivos e configurações, consulte [2735227 do artigo](https://support.microsoft.com/kb/2735227) na Base de Conhecimento Microsoft.  
  
###  <a name="BKMK_Transfer"></a>Transferir vários perfis de usuário durante a implantação do computador  
 Antes de você se conectar a um computador que executa o Windows 7 ou o sistema de operacional Windows 7 SP1 para o servidor Windows Server Essentials, para transferir vários perfis de usuário local, você deve primeiro criar as contas de usuário de rede correspondente no servidor. Para obter mais informações sobre a criação de contas de usuário de rede, consulte [adicionar uma conta de usuário](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  
  
 Migração de perfis de usuário só tem suporte em um computador executando o Windows 7 (para Windows Server Essentials) ou Windows 7 SP1 (para Windows Server Essentials). Quando você conectar um computador para o servidor Windows Server Essentials usando a conectar seu computador para o Assistente de servidor, você terá uma opção para mover os dados do usuário e as configurações antigas locais de contas de usuário para as novas contas de usuário de rede. Para fazer isso, no **mover dados existentes do usuário e configurações** página do assistente, as contas de usuário de rede são mapeadas para as contas de usuário local que existem no computador para transferir vários perfis de usuário que estão localizados no computador cliente.  
  
###  <a name="BKMK_13"></a>Desinstalar o software do conector  
 Você pode desinstalar o software do conector de um computador usando o painel de controle. Você normalmente fará isso se houver um problema com o software Connector ou se você precisar instalar uma versão mais recente do software do conector. Você deve estar conectado ao computador como um administrador para concluir este procedimento.  
  
> [!IMPORTANT]
>  Se você atualizar o sistema operacional em um computador cliente, o software do conector é desinstalado automaticamente. Você deverá reinstalar o software do conector depois que a atualização for concluída. É o método preferencial desinstalar o software do conector antes de atualizar o sistema operacional. Desinstalar o software Connector depois que a atualização for concluída ainda é aceitável; No entanto, ele pode resultar em um estado inconsistente para o computador cliente com o servidor até que o software Connector tiver sido desinstalado e reinstalado.  
  
##### <a name="to-uninstall-connector-software-from-a-computer"></a>Para desinstalar o software do conector de um computador  
  
1.  Em um computador que está executando o Windows 7, Windows 8, Windows 8.1 ou Windows 10, abra **painel de controle**e, em seguida, no **programas** seção, clique em **exibir atualizações instaladas**.  
  
2.  Na lista de programas instalados, selecione **conector do Windows Server Essentials**e clique em **desinstalar**.  
  
3.  Na página de aviso, clique em **Sim**.  
  
4.  Se o **User Account Control** janela for exibida, clique em **permitir**.  
  
5.  No Windows Server Essentials, se a página conector do Windows Server Essentials aparecer sugiram para fechar a barra inicial, clique em **Okey**.  
  
6.  Aguarde até que o programa de desinstalação. Depois que o software for removido, **conector do Windows Server Essentials** deixará de aparecer na lista de programas instalados ou atualizações. Além disso, os atalhos da barra inicial e o painel não são exibidos na área de trabalho do computador s.  
  
> [!NOTE]
>  -   Desinstalar o software Connector não remove o computador da lista de computadores que são exibidos na **dispositivos** guia do painel. Para remover o computador do painel, consulte [remover um computador do servidor](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  
> -   Quando você desinstala o software Connector, pastas compartilhadas no computador cliente que tenham sido mapeadas para o servidor não são excluídas. Você deve excluir manualmente as pastas compartilhadas são mapeadas para o servidor.  

> -   Desinstalar o software Connector não faz o computador sair do domínio original. Você deve sair manualmente o computador do domínio. Para obter instruções, consulte [remover um computador de um domínio do Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  

  
###  <a name="BKMK_14"></a>Desconecte seu computador contra ou reconecte seu computador para o servidor  
 Para desconectar um computador do servidor, você deve concluir as etapas a seguir:  
  

1.  Desinstale o software do conector do computador usando o painel de controle. Para obter instruções detalhadas, consulte [desinstalar o software do conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).   

  
2.  Retire o computador do domínio do Windows Server Essentials e associá-lo ao grupo de trabalho. Para obter instruções passo a passo para ingressar o Windows em um grupo de trabalho, [ingressar ou criar um grupo de trabalho](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  
  
3.  Remova o computador do servidor usando o Dashboard. Para obter instruções detalhadas, consulte [remover um computador do servidor](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  
  
 Para se reconectar a um computador para o servidor que anteriormente foi desconectado de sua rede de servidor Windows Server Essentials, você deve concluir as etapas a seguir:  
  

1.  Desinstale o software do conector do computador usando o painel de controle. Para obter instruções detalhadas, consulte [desinstalar o software do conector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  
  
2.  Retire o computador do domínio do Windows Server Essentials e associá-lo ao grupo de trabalho. Para obter instruções passo a passo para ingressar o Windows em um grupo de trabalho, [ingressar ou criar um grupo de trabalho](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  
  
3.  Conecte o computador para o servidor usando o Assistente de computador se conectar. Para obter instruções detalhadas, consulte [conectar computadores para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
###  <a name="BKMK_Sleep"></a>Como fazer backup funciona com a suspensão e hibernação  
 Se você selecionar o **ativar este computador para Backup** opção quando você conectar um computador com o servidor, o computador será automaticamente despertar do modo de suspensão ou no modo de hibernação diariamente conforme especificado no plano de Backup para que ele possa ser feito backup. Depois que o backup é concluído, o computador retornará para suspender ou Hibernar modo, com base em suas configurações de gerenciamento de energia. Se você não selecionar essa opção, o servidor não fará backup um computador se o computador está no modo de suspensão ou hibernação. Para obter mais informações, consulte [gerenciar o Backup do cliente](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_C"></a>Uso da barra inicial  
 Você pode usar a barra inicial para acessar recursos compartilhados do servidor Windows Server Essentials, fazer backups do computador e responder a alertas de integridade do sistema.  
  
-   [Visão geral da barra inicial](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)  
  
-   [Use a barra inicial com um computador Mac](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  
  
## <a name="see-also"></a>Consulte também  
  
-   [Solucionar problemas de computadores que se conectam ao servidor](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar contas de usuário](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  

-   [Use pastas compartilhadas](Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Trabalhar remotamente](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Reproduzir mídia Digital](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [Use pastas compartilhadas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Trabalhar remotamente](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Reproduzir mídia Digital](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

