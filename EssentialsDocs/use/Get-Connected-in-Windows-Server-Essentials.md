---
title: Manter-se conectado no Windows Server Essentials [A_Web_Client_H2]
description: Descreve como usar o Windows Server Essentials
ms.date: 05/07/2016
ms.topic: article
ms.assetid: 149a5d34-43b7-4b9e-99e7-9f2294ab9ddb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c1dafb9fb89109a16f2f28662464494691a2f2f0
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87179942"
---
# <a name="get-connected-in-windows-server-essentials"></a>Manter-se conectado no Windows Server Essentials [A_Web_Client_H2]

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Você pode conectar os computadores ao servidor do Windows Server Essentials usando o software Connector. O software Connector é instalado quando você conectar um computador ao servidor usando o Conector usando Connecte um computador ao Assistente do servidor. Você pode iniciar esse assistente digitando **http://<ServerName \> /Connect**, em que **<ServerName \> ** é o nome do seu servidor.

 Neste tópico:


-   [Preparação para conectar os computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)

-   [Conectar computadores ao servidor usando o software Connector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)

-   [Usar a Barra Inicial](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)

-   [Preparação para conectar os computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)

-   [Conectar computadores ao servidor usando o software Connector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)

-   [Usar a Barra Inicial](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)


##  <a name="prepare-to-connect-computers-to-the-server"></a><a name="BKMK_A"></a>Preparar para conectar computadores ao servidor
 Esta seção discute o software Connector, os sistemas operacionais com suporte no Windows Server Essentials, as tarefas de pré-requisito devem ser concluídas antes de conectar os computadores ao servidor, e as alterações feitas pelo servidor nos computadores ao executar o software Connector.


-   [Visão geral do software Connector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)

-   [Pré-requisitos para conectar um computador ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)

-   [Pré-requisitos para conectar um computador Mac à rede](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)

-   [Sistemas operacionais compatíveis para computadores cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)

-   [Alterações que servidor realiza no computador cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)

-   [Nome de usuário da rede e senha.](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)

-   [Conta do administrador do servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)

-   [Remover um computador de um domínio do Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)

###  <a name="connector-software-overview"></a><a name="BKMK_1"></a>Visão geral do software do conector
 O software Conector para o sistema operacional Windows Server Essentials conecta os computadores em sua rede para o servidor do Windows Server Essentials. Quando você conecta computadores ao servidor, o software Connector permite que você faça backup automaticamente nos computadores e monitore sua integridade. O software Connector também permite configurar e administrar remotamente o servidor do Windows Server Essentials. O software Connector é instalado quando você conectar um computador cliente para o servidor. Para obter instruções detalhadas sobre como se conectar os computadores cliente ao servidor do Windows Server Essentials, consulte [conectar computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) posteriormente neste tópico.

-   [Visão geral do software Connector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)

-   [Pré-requisitos para conectar um computador ao servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)

-   [Pré-requisitos para conectar um computador Mac à rede](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)

-   [Sistemas operacionais compatíveis para computadores cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)

-   [Alterações que servidor realiza no computador cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)

-   [Nome de usuário da rede e senha.](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)

-   [Conta do administrador do servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)

-   [Remover um computador de um domínio do Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)

###  <a name="connector-software-overview"></a><a name="BKMK_1"></a>Visão geral do software do conector
 O software Conector para o sistema operacional Windows Server Essentials conecta os computadores em sua rede para o servidor do Windows Server Essentials. Quando você conecta computadores ao servidor, o software Connector permite que você faça backup automaticamente nos computadores e monitore sua integridade. O software Connector também permite configurar e administrar remotamente o servidor do Windows Server Essentials. O software Connector é instalado quando você conectar um computador cliente para o servidor. Para obter instruções detalhadas sobre como se conectar os computadores cliente ao servidor do Windows Server Essentials, consulte [conectar computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) posteriormente neste tópico.


###  <a name="prerequisites-for-connecting-a-computer-to-the-server"></a><a name="BKMK_2"></a>Pré-requisitos para conectar um computador ao servidor
 Os seguintes requisitos devem ser atendidos antes de você conectar um computador à rede:

-   Quando a instalação do Windows Server Essentials estiver completa e o servidor estiver em execução. O software Connector cancelará a instalação se não houver comunicação com o servidor.


-   O computador cliente deve executar um sistema operacional compatível. Para obter mais informações, consulte [sistemas operacionais compatíveis para os computadores cliente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4).


-   O computador cliente deve ter conexão estável com a Internet.

-   O computador cliente está na mesma sub-rede IP do servidor que está executando o Windows Server Essentials, quando o computador cliente estiver na mesma rede como o servidor.

-   O computador cliente deve ter .NET Framework 4.5 instalado. O software Connector instalará automaticamente no computador.

-   O computador cliente atende aos requisitos mínimos de sistema:

    -   1.4 GHz ou processador mais rápido.

    -   1 GB de RAM ou mais

    -   1 GB de espaço disponível no disco rígido (parte desse espaço será liberada após a instalação)

-   A partição de inicialização (ou seja, a partição de disco em que o sistema operacional Windows está instalado) está formatada com o sistema de arquivos NTFS.

-   O nome do computador não permite mais de 15 caracteres.

-   O nome do computador não está em destaque (_).

-   As configurações de data e hora do computador são alinhadas com as configurações no servidor.

-   Um computador cliente pode estar conectado apenas a um servidor do Windows Server Essentials de cada vez.

-   Um computador cliente que já tenha ingressado em outro domínio do Active Directory não pode entrar em um domínio do Windows Server Essentials.

> [!NOTE]
>
>  Em uma implantação de cliente local para Windows Server Essentials ou Windows Server Essentials, você pode conectar computadores ao servidor sem adicioná-los ao domínio do Windows Server Essentials. Este método não está disponível para todos os sistemas operacionais cliente com suporte e recursos, como a Política de Grupo e redes virtuais privadas (VPNs), que exige que um computador esteja conectado ao domínio, não estão disponíveis. Para requisitos e instruções, consulte [conectar computadores a um servidor do Windows Server Essentials sem ingressar no domínio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).

 Para obter instruções passo a passo para conectar um computador ao servidor que executa o Windows Server Essentials, consulte [conectar computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).

>  Em uma implantação de cliente local para Windows Server Essentials ou Windows Server Essentials, você pode conectar computadores ao servidor sem adicioná-los ao domínio do Windows Server Essentials. Este método não está disponível para todos os sistemas operacionais cliente com suporte e recursos, como a Política de Grupo e redes virtuais privadas (VPNs), que exige que um computador esteja conectado ao domínio, não estão disponíveis. Para requisitos e instruções, consulte [conectar computadores a um servidor do Windows Server Essentials sem ingressar no domínio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).

 Para obter instruções passo a passo para conectar um computador ao servidor que executa o Windows Server Essentials, consulte [conectar computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).


###  <a name="prerequisites-for-connecting-a-mac-computer-to-the-network"></a><a name="BKMK_3"></a>Pré-requisitos para conectar um computador Mac à rede
 Os seguintes requisitos devem ser atendidos antes de você conectar um computador Mac à rede:

-   A instalação do sistema operacional do servidor for concluída e o servidor estiver em execução. O software Connector não será instalado se não houver comunicação com o servidor.

-   O computador está executando o Mac OS X 10.5 (Leopard) ou posterior.

-   O computador está na mesma subrede IP do servidor.

-   O computador deve ter uma conexão estável com a Internet.

-   Certifique-se de que o computador atende aos seguintes requisitos mínimos de sistema:

    -   1.4 GHz ou processador mais rápido.

    -   1 GB de RAM ou mais

    -   1 GB de espaço disponível no disco rígido (parte desse espaço será liberada após a instalação)

-   Um computador cliente pode estar conectado a apenas um servidor de cada vez.

###  <a name="supported-operating-systems-for-client-computers"></a><a name="BKMK_4"></a>Sistemas operacionais com suporte para computadores cliente
 Windows Server Essentials fornece o mesmo conjunto de recursos para todos os computadores cliente compatíveis. Esses recursos incluem o Ingresso no Domínio, a Barra Inicial e notificações de integridade por parte do cliente.

> [!IMPORTANT]
>  Windows Server Essentials do domínio não é compatível com a entrada de computadores que executem as versões do Windows Home, Inicial ou Media Center. Além disso, não é possível usar o Acesso Remoto via Web para se conectar a esses computadores.

#### <a name="windows-server-essentials"></a>Windows Server Essentials
  Esta seção se aplica a um servidor que executa o Windows Server Essentials ou a um servidor executando o Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter com a função de experiência do Windows Server Essentials instalada. Os seguintes sistemas operacionais de computador têm suporte:

 **Sistemas operacionais Windows 7**

- Windows 7 Home Basic SP1 (x86 e x64)

- Windows 7 Home Premium SP1 (x86 e x64)

- Windows 7 Professional SP1 (x86 e x64)

- Windows 7 Ultimate SP1 (x86 e x64)

- Windows 7 Enterprise SP1 (x86 e x64)

- Windows 7 Starter SP1 (x86)

  **Sistemas operacionais Windows 8**

- Windows 8

- Windows 8 Pro

- O Windows 8 Enterprise

  **Sistemas operacionais Windows 8.1**

- Windows 8.1

- Windows 8.1 Pro

- Windows 8.1 Enterprise

  **Sistemas operacionais Windows 10**

- Windows 10

- Windows 10 Pro

- Windows 10 Enterprise

- Windows 10 Education

  **Computadores cliente Mac**

- Mac OS X v10.5 Leopard

- Mac OS X v10.6 Snow Leopard

- Mac OS X v 10.7 Lion

- Mac OS X v10.8 Mountain Lion

> [!NOTE]
>  Você pode exibir o status de integridade e de backup de um computador Mac no painel do Windows Server Essentials. No entanto, você não pode configurar o backup do computador ou iniciar um backup do painel. Além disso, não é possível usar o Acesso via Web Remoto para se conectar a um computador Mac.

#### <a name="windows-server-essentials"></a>Windows Server Essentials
  Esta seção se aplica a um servidor que executa o Windows Server Essentials. Os seguintes sistemas operacionais de computador têm suporte:

 **Sistemas operacionais Windows 7**

- Windows 7 Home Basic (x86 e x64)

- Windows 7 Home Premium (x86 e x64)

- Windows 7 Professional (x86 e x64)

- Windows 7 Ultimate (x86 e x64)

- Windows 7 Enterprise (x86 e x64)

- Iniciador do Windows 7 (x86)

  **Sistemas operacionais Windows 8**

- Windows 8

- Windows 8 Pro

- O Windows 8 Enterprise

  **Sistemas operacionais Windows 10**

- Windows 10

- Windows 10 Pro

- Windows 10 Enterprise

- Windows 10 Education

  **Computadores cliente Mac**

- Mac OS X v10.5 Leopard

- Mac OS X v10.6 Snow Leopard

- Mac OS X v 10.7 Lion

> [!NOTE]
>  Você pode exibir a integridade e o status do backup de um computador Mac a partir do Painel do Windows Server Essentials. No entanto, você não pode configurar o backup do computador ou iniciar um backup do painel. Além disso, não é possível usar o Acesso via Web Remoto para se conectar a um computador Mac.

###  <a name="changes-the-server-makes-to-a-client-computer"></a><a name="BKMK_5"></a>Altera o servidor que faz para um computador cliente
 Quando você conectar um computador ao servidor, o software Windows Server Essentials algumas alterações são realizadas no computador para que o computador e o servidor possam trabalhar juntos.

 O software faz o seguinte:

-   Instala o software Connector no computador

-   Instala o Microsoft .NET Framework 4.5, se ainda  não estiver instalado no computador

-   Cria atalhos na área de trabalho do computador para o painel e o Launchpad

-   Configura as portas do Firewall do Windows no computador para permitir que funcionem os seguintes recursos:

    -   Rede Principal

    -   Serviços da Área de Trabalho Remota

-   Faz as seguintes alterações no computador para facilitar backups:

    -   Cria tarefas agendadas para executar backups automáticos

    -   Instala os serviços que gerenciam as operações de backup com o servidor

    -   Instala um driver de dispositivo virtual que é usado durante processos de recuperação de arquivos e pastas

-   Instala o agente de integridade para detectar problemas e criar as correspondentes notificações de alerta

-   Cria tarefas agendadas recorrente no computador para avaliação de integridade e para sincronizar as definições de alerta de integridade

-   Adiciona serviços ao computador, o computador usa para se comunicar com o servidor e outros recursos do Windows Server Essentials

-   Abre a porta TCP 3389 em computadores cliente que executam Windows, para permitir que os Serviços Remotos da área de trabalho funcionem.

-   Implanta a VPN no computador cliente e fornece uma experiência de clique único se a funcionalidade de VPN estiver habilitada no Windows Server Essentials ou fornece uma experiência de conexão automática se a funcionalidade de VPN estiver habilitada no Windows Server Essentials


 Para obter instruções passo a passo sobre a conexão de seu computador ao servidor, consulte [Conectar computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).

###  <a name="network-user-name-and-password-information"></a><a name="BKMK_6"></a>Informações de nome de usuário e senha de rede
 Você pode obter suas informações de nome e senha de usuário da rede com pessoa que gerencia o servidor. Você pode usar essas credenciais para conectar o computador ao servidor e acessar as informações do servidor.

###  <a name="network-user-name-and-password-information"></a><a name="BKMK_6"></a>Informações de nome de usuário e senha de rede
 Você pode obter suas informações de nome e senha de usuário da rede com pessoa que gerencia o servidor. Você pode usar essas credenciais para conectar o computador ao servidor e acessar as informações do servidor.


 Se você for o administrador do servidor, você pode criar as credenciais de rede, adicionando uma conta de usuário a partir de **Usuários** no Painel. Para obter mais informações sobre contas de usuário, consulte [gerenciar contas de usuário usando o Painel](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8).

###  <a name="server-administrators-account"></a><a name="BKMK_7"></a>Conta do administrador do servidor
 Você deve ser capaz de fornecer um nome de conta de administrador de rede e uma senha para instalar o software Connector. Uma conta de administrador de rede permite que o usuário gerencie a rede local para sua organização e ajuda a gerenciar e manter dispositivos de rede como roteadores e sensores.

 As tarefas que podem ser executadas usando uma conta de administrador de rede podem incluir:

- Instalando aplicativos na rede e outros softwares

- Gerenciamento de armazenamento no servidor

- Distribui atualizações de software

- Executar backups de rotina

- Monitorar as atividades diárias na rede

  No Windows Server Essentials, no Windows Server Essentials e no Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, você pode atribuir o nível de acesso de administrador de rede a qualquer conta de usuário. Isso concede as permissões necessárias para executar tarefas de administrador de rede. Quando o nível de acesso de administrador de rede é atribuído a um usuário, o prompt **Controle de Acesso de Usuários** é aberto para qualquer tarefa que requeira permissões de administrador.

###  <a name="remove-a-computer-from-a-windows-domain"></a><a name="BKMK_8"></a>Remover um computador de um domínio do Windows
 Para remover um computador do seu domínio, você será solicitado para o nome de usuário e a senha da conta de domínio.

##### <a name="to-remove-a-computer-from-a-windows-domain"></a>Para remover um computador de um domínio do Windows

1.  Clique em **Iniciar**, clique com botão direito do **computador**e clique em **Propriedades**.

2.  Em **configurações de grupo de trabalho, o domínio e o nome do computador**, clique em **alterar configurações**.

    > [!NOTE]
    >  Se você for solicitado para uma senha de administrador ou uma confirmação, digite a senha do domínio ou forneça a confirmação.

3.  No **propriedades do sistema** caixa de diálogo, clique em **nome do computador** guia e, em seguida, clique em **alteração**.

4.  No **alterações de nome/domínio do computador** caixa de diálogo, em **membro do**, clique em **grupo de trabalho**, e siga um destes procedimentos:

    1.  Para associar um grupo de trabalho, digite o nome do grupo de trabalho que você deseja adicionar e clique em **OK**.

    2.  Para criar um grupo de trabalho, digite o nome do grupo de trabalho que você deseja criar e, em seguida, clique em **OK**.

        > [!NOTE]
        >  O computador será removido do domínio e sua conta de computador nesse domínio será desabilitada.

##  <a name="connect-computers-to-the-server-by-using-the-connector-software"></a><a name="BKMK_B"></a>Conectar computadores ao servidor usando o software do conector
 Esta seção fornece acesso às informações que ajudarão a instalar o software Connector, conectar o computador ao servidor e solucionar problemas de conexão com o servidor.


-   [Conectar computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)

-   [Conectar computadores a um servidor do Windows Server Essentials sem entrar no domínio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)

-   [Instalar o software Connector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)

-   [Mover dados do computador e as configurações manualmente](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)

-   [Transferir vários perfis de usuário durante a implantação no computador](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)

-   [Desinstalar o software Connector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)

-   [Desconecta o computador do servidor ou reconecta seu computador.](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)

-   [Como funciona a suspensão de backup e hibernação](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)


###  <a name="connect-computers-to-the-server"></a><a name="BKMK_9"></a>Conectar computadores ao servidor
 Ao conectar um computador a um servidor que esteja executando o Windows Server Essentials ou o Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada, verifique se o computador cliente tem uma conexão válida com a Internet.

 Realize o seguinte procedimento em todos os computadores cliente para conectá-los ao seu servidor.

 Para concluir o procedimento, você precisa ter as seguintes informações:

-   O nome de usuário e a senha da pessoa que usará o computador na nova rede.

-   O nome de usuário e a senha da conta de administrador local do computador

> [!NOTE]
>
>  Em uma implantação de cliente local para Windows Server Essentials ou Windows Server Essentials, você pode conectar computadores ao servidor sem adicioná-los ao domínio do Windows Server Essentials. Este método não está disponível para todos os sistemas operacionais cliente com suporte e recursos, como a Política de Grupo e redes virtuais privadas (VPNs), que exige que um computador esteja conectado ao domínio, não estão disponíveis. Para requisitos e instruções, consulte [conectar computadores a um servidor do Windows Server Essentials sem ingressar no domínio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).
>
>  Em uma implantação de cliente local para Windows Server Essentials ou Windows Server Essentials, você pode conectar computadores ao servidor sem adicioná-los ao domínio do Windows Server Essentials. Este método não está disponível para todos os sistemas operacionais cliente com suporte e recursos, como a Política de Grupo e redes virtuais privadas (VPNs), que exige que um computador esteja conectado ao domínio, não estão disponíveis. Para requisitos e instruções, consulte [conectar computadores a um servidor do Windows Server Essentials sem ingressar no domínio](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).


##### <a name="to-connect-a-client-computer-to-the-server"></a>Para conectar um computador cliente ao servidor

1.  Faça logon no computador que você deseja conectar ao servidor.

    > [!NOTE]
    >  Caso esse computador tenha várias contas de usuário, faça logon com a conta de usuário na qual estejam os documentos, imagens e preferências pessoais você deseja manter depois de conectar o computador ao servidor.

2.  Abra um navegador de Internet, como o Internet Explorer.

3.  Na barra de endereços, digite **http://<ServerName \> /Connect**e pressione Enter.

    > [!NOTE]
    >  Se o computador estiver em um local remoto fora da rede do Windows Server Essentials, para executar o assistente para conectar um computador ao servidor, digite **http://<\> /Connect** na barra de endereços do seu navegador da Web (em que <domínio \> é o nome de domínio da sua organização). Você pode obter as informações de nome de domínio do administrador da rede.

4.  A página **Conectar o computador ao servidor** será exibida. Realize um dos seguintes procedimentos:

    -   Para um computador que executa o sistema operacional Windows, clique em **Baixar software para Windows **.

    -   Em um computador executando o Mac OS X ou posterior, clique em **Baixar software para Mac**.

5.  Na mensagem de aviso de segurança para download de arquivos, clique em **Executar**.

6.  Se a mensagem do Controle de Conta de Usuário for exibida, clique em **Sim** ou digite o nome de usuário local e a senha, se solicitados.

7.  O Assistente de Conexão do Computador ao Servidor será exibido. Para concluir o assistente, faça o seguinte:

    1.  Aceite os Termos de Licença.

    2.  Sobre como página, **Encontrar meu servidor** página, detecção automática do servidor em redes locais e selecione o servidor que você deseja se conectar. Ou, se você tiver as informações, poderá inserir manualmente o nome do servidor ou o endereço de domínio.

    3.  Na página **Digite seu novo nome de usuário de rede e senha**, execute as ações a seguir:

        -   Se este for o primeiro computador que você estiver conectando ao servidor e se for o computador que você usará para administrar o servidor, use a conta de administrador criada durante a instalação.

        -   Para todos os outros computadores, primeiro crie uma conta de usuário de rede no servidor usando o Painel. Crie a conta de usuário com privilégios de administrador ou usuário padrão, com base nas tarefas executadas pela pessoa que utiliza o computador.

    4.  Se o computador estiver executando o Windows 8, Windows 8.1 ou Windows 10, ignore esta etapa. Se seu computador tem o Windows 7 e você deseja manter documentos, fotos ou preferências pessoais (como planos de fundo da área de trabalho, proteções de tela ou favoritos do Internet Explorer) depois de adicionar o computador à nova rede, selecione **Mover meus dados e configurações para minha nova conta de usuário de rede** na página **Selecione se você deseja mover seus dados e configurações existentes**do assistente.

    5.  Selecione se deseja ou não ativar automaticamente este computador para criar um backup na página **Selecione se você deseja ativar este computador para criar seu backup**.

    6.  Siga as instruções restantes no assistente para adicionar o computador à rede.

8.  Depois de associar o computador à rede, use o novo nome de usuário e senha para fazer logon no computador.

    > [!NOTE]
    >  Quando você fizer logon pela primeira vez em um computador que está executando o Windows 8 usando sua conta de rede depois que ele se conectar ao servidor, serão exibidas instruções sobre como migrar arquivos e aplicativos da conta de usuário antiga. Siga as instruções na página **Como migro arquivos e aplicativos de minha conta de usuário antiga?** para migrar todos os arquivos e aplicativos para a conta de usuário de rede.

9. Depois que o computador estiver conectado com êxito ao servidor, os atalhos para o conector TrayApp e o painel do servidor serão exibidos no menu Iniciar, que pode ser usado da seguinte maneira (se o computador estiver executando o Windows 8, Windows 8.1 ou Windows 10, o painel e o conector TrayApp estarão disponíveis na tela inicial do computador):

    -   Se o computador estiver executando o Windows 8, Windows 8.1 ou Windows 10, o painel e o conector TrayApp serão pesquisáveis como um aplicativo.

    -   De TrayApp conector, você pode habilitar ou desabilitar o **permanecer conectado remotamente** recurso. Você também pode clicar duas vezes TrayApp para iniciar a barra inicial. No link Barra Inicial, é possível acessar o atalho Pastas compartilhadas, configurar backups de computador, lidar com alertas e abrir o site do Acesso via Web Remoto.

    -   Por meio do link **Painel**, você pode administrar o servidor.

###  <a name="connect-computers-to-a-windows-server-essentials-server-without-joining-the-domain"></a><a name="BKMK_10"></a>Conectar computadores a um servidor do Windows Server Essentials sem ingressar no domínio
 Este tópico descreve como adicionar um computador com Windows 7, Windows 8, Windows 8.1 ou Windows 10 a uma rede do Windows Server Essentials sem ingressar o computador no domínio do Windows Server Essentials em uma implantação de cliente local. Esse método de conexão tem suporte no Windows Server Essentials e no Windows Server Essentials.

 Essa é uma alternativa ao método normal, que exige a ingressar o computador no domínio do Windows Server Essentials. Com esse método, se o computador estiver em outro domínio, ele deve ser removido do domínio antes que ele possa ser adicionado ao domínio do Windows Server Essentials.

#### <a name="feature-limits"></a>Limites de recursos
 Alguns recursos estão limitados quando um computador cliente não for adicionado ao domínio do Windows Server Essentials:

-   Todos os recursos que exigem que o computador ingresse no domínio? incluindo credenciais de domínio, Política de Grupo e VPNs (redes virtuais privadas)? não estão disponíveis.

-   Quaisquer complementos de terceiros que exijam que o computador seja associado ao domínio, não funcionará corretamente.

-   Este método não pode ser usado para conectar um computador externa para o servidor.

#### <a name="prerequisites"></a>Pré-requisitos

-   O computador deve ter uma conexão física com a rede local.

-   Um dos seguintes sistemas operacionais deve ser instalado no computador:

    -   Windows 10 pro, Windows 10 Enterprise

    -    Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 8 Pro, Windows 8 Enterprise

    -    Windows 7 Professional (x86 e x64), Windows 7 Enterprise (x86 e x64), Windows 7 Ultimate (x86 e x64)


-   O computador deve atender a todos os outros requisitos para computadores cliente no Windows Server Essentials. Para obter mais informações, consulte [pré-requisitos para conectar um computador para o servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2).


-   Para habilitar uma conexão sem ingressar no domínio, você deve fazer logon no computador com uma conta que seja membro do grupo Administradores local.

-   Para conectar o computador para o servidor do Windows Server Essentials, você precisará as informações de conta a seguir:

    -   Uma conta com direitos de administrador no Windows Server Essentials (sua conta).

    -   O nome de usuário e senha para a conta de domínio da pessoa que usará o computador. A conta de domínio também deve ter direitos de administrador no servidor do Windows Server Essentials.

#### <a name="connect-to-a-windows-server-essentials-network"></a>Conecta a uma rede do Windows Server Essentials
 Depois de verificar se todos os pré-requisitos foram atendidos, conecte o computador à rede Windows Server Essentials.

###### <a name="to-connect-a-computer-in-a-different-domain-to-a-windows-server-essentials-network"></a>Para conectar um computador em um domínio diferente a uma rede do Windows Server Essentials

1.  Logon no computador cliente com uma conta que seja membro do grupo Administradores local.

2.  Abra um prompt de comando com direitos de administrador.

    -   No Windows 10, clique no botão **Iniciar** , selecione **todos os aplicativos**  ->  **Ferramentas do sistema Windows**  ->  **prompt de comando**, clique com o botão direito do mouse em prompt de comando e clique em **Executar como administrador**.

    -   No Windows 8, na página **inicial** , digite **comando** e pressione Enter. Nos resultados, clique com botão direito **Prompt de comando** e clique em **executar como administrador**.

    -   No Windows 7, no menu **Iniciar** , digite **comando** na caixa de pesquisa, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.

3.  No prompt de comando, digite o seguinte comando e pressione Enter:

    ```
    reg add "HKLM\SOFTWARE\Microsoft\Windows Server\ClientDeployment" /v SkipDomainJoin /t REG_DWORD /d 1
    ```


4.  Conclua as etapas em [conectar computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).


####  <a name="join-a-second-server-to-the-network"></a><a name="BKMK_SecondServer"></a>Ingressar um segundo servidor na rede

###### <a name="to-join-a-second-server-to-the-network"></a>Para adicionar um segundo servidor à rede

1.  Faça logon no servidor que você deseja conectar à rede Windows Server Essentials.

2.  Abra um navegador da Internet e, na barra de endereços, digite **http://<ServerName \> /Connect**, em que *<ServerName \> * é o nome do servidor que executa o Windows Server Essentials e pressione Enter.

3.  Se a configuração de segurança aprimorada do Internet Explorer estiver habilitada no servidor que você está tentando se conectar à rede Windows Server Essentials, execute o seguinte; Caso contrário, ignore esta etapa.

    1.  Para aceitar a mensagem de bloqueio, clique em **fechar**.

    2.  Adicione o site do **http://<ServerName \> /Connect** aos sites confiáveis da seguinte maneira:

        1.  No painel de navegação do navegador, clique em **ferramentas**e clique em **opções da Internet**.

        2.  Clique na guia de **segurança**, e em seguida, clique em **Sites confiáveis**.

        3.  Clique em **Sites**.

        4.  O site deve ser mostrado no campo **adicionar este site à zona** . Clique em **Adicionar**.

        5.  Clique em **Fechar**e clique em **OK**.

    3.  Atualize a página da Web.


    4.  Para conectar o segundo servidor para um servidor que executa o Windows Server Essentials, siga as instruções em [conectar computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).


~~~
> [!NOTE]
>  Connecting a second server to a server running Windows Server Essentials differs from connecting to a client computer as follows:
>
>  -   There is no user profile migration; therefore, the current profile will not be migrated.
> -   You cannot back up the second server by using client computer backup, and there is no option to wake up the computer for backup.
~~~

 Depois de associar o segundo servidor para um servidor que está executando o Windows Server Essentials, os recursos a seguir são fornecidos para o servidor conectado:

- Atualização e funcionalidades do status de alertas funcionam da mesma forma outros computadores cliente.

- Funções Online e Offline funcionam da mesma forma outros computadores cliente.

- Você pode se conectar ao segundo servidor usando o Acesso Remoto via Web.

- O segundo servidor será incluído nos relatórios de integridade porque o Windows Server Essentials gerará alertas relacionadas a este servidor.

  Gerenciamento do segundo servidor do servidor que está executando o Windows Server Essentials será diferente de gerenciar outros computadores cliente da seguinte maneira:

- Não haverá nenhum ponto de entrada para TrayApp, Barra Inicial e cliente do Painel.

- O segundo servidor está listado nos **servidores** grupo de **dispositivos**.

- Porque o backup do computador cliente não é compatível com o segundo servidor, o status do backup fica exibido como **não tem suporte**. Além disso, não se você selecionar o segundo servidor e com o botão direito, há nenhum backup e restauração tarefas relacionadas exibidas para o segundo servidor.

- Se você selecionar o segundo servidor e clicar na tarefa **exibir as propriedades do servidor** , não haverá nenhuma guia **backup** exibida na página de propriedades do servidor.

- Como não há uma central de segurança em um sistema operacional Windows Server, o status de segurança do segundo servidor é exibido como **não aplicável**.

- O status de Política de Grupo do segundo servidor é exibido como **não aplicável**.

###  <a name="install-the-connector-software"></a><a name="BKMK_11"></a>Instalar o software do conector
 O software Connector no Windows Server Essentials é instalado quando você conecta o computador ao servidor usando a conectar um computador para o Assistente do servidor. Você pode iniciar esse assistente digitando **http://<ServerName \> /Connect** na barra de endereços do seu navegador da Web (em que *<\> * ServerName é o nome do seu servidor).

> [!NOTE]
>  Se o seu computador estiver em um local remoto, para executar o assistente para conectar um computador ao servidor, digite **http://<nome_do_domínio \> /Connect** na barra de endereços do navegador da Web (em que *<domínio \> * é o nome de domínio da sua organização). Você pode obter as informações de nome de domínio do administrador da rede.

 O software Connector faz o seguinte:

-   Conectar o computador para o Windows Server Essentials

-   Automaticamente realiza backups no seu computador à noite (se você configurar o servidor para criar backups de cliente)

-   Ajuda o administrador a monitorar a integridade do computador

-   Permite configurar e administrar remotamente o Windows Server Essentials em seu computador doméstico


 Para obter instruções passo a passo sobre a conexão de seu computador ao servidor, consulte [Conectar computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).


###  <a name="move-computer-data-and-settings-manually"></a><a name="BKMK_12"></a>Mover dados e configurações do computador manualmente
  O Windows Server Essentials e o Windows Server Essentials dão suporte à migração de perfil de usuário somente para computadores cliente que executam o sistema operacional Windows 7. Quando você conectar um computador baseado no Windows 7 para o servidor, o computador se conectar ao Assistente de servidor pode migrar automaticamente o perfil de usuário.

 O perfil do usuário não pode ser transferido automaticamente ao conectar um computador Windows 8, Windows 8.1 ou Windows 10 ao servidor. No entanto, em um computador com Windows 8, você pode usar a transferência fácil do Windows para transferir dados e configurações do usuário local original para o computador associado ao domínio. Para fazer isso, você deve ser um administrador no computador de origem Windows 8 e o computador de destino do Windows 8. Para obter informações sobre como usar a Transferência Fácil do Windows para transferir arquivos e configurações, consulte o [artigo 2735227](https://support.microsoft.com/kb/2735227) na Base de Dados de Conhecimento da Microsoft.

###  <a name="transfer-multiple-user-profiles-during-computer-deployment"></a><a name="BKMK_Transfer"></a>Transferir vários perfis de usuário durante a implantação do computador
 Antes de você conectar um computador executando o Windows 7 ou o sistema operacional Windows 7 SP1 para o servidor do Windows Server Essentials; para transferir vários perfis de usuário locais, você deve primeiro criar as contas de usuário de rede correspondente no servidor. Para obter mais informações sobre como criar contas de usuário de rede, veja [adicionar uma conta de usuário](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).

 A migração de perfil de usuário só tem suporte em um computador com Windows 7 (para Windows Server Essentials) ou Windows 7 SP1 (para Windows Server Essentials). Quando você conectar um computador para o servidor do Windows Server Essentials usando a conectar seu computador para o Assistente do servidor, você terá uma opção para mover os dados de usuário e configurações antigo local de contas de usuário para as novas contas de usuário de rede. Para fazer isso, no **mover os dados existentes do usuário e configurações** página do assistente, mapear as contas de usuário de rede para as contas de usuário local que existem no computador para transferir vários perfis de usuário estão localizados no computador cliente.

###  <a name="uninstall-the-connector-software"></a><a name="BKMK_13"></a>Desinstalar o software do conector
 Você pode desinstalar o software Connector em um computador usando o painel de controle. Você geralmente fará isso se houver um problema com o software Connector ou se você precisar instalar uma versão mais recente do software Connector. Você deve estar conectado ao computador como um administrador para concluir este procedimento.

> [!IMPORTANT]
>  Se você atualizar o sistema operacional em um computador cliente, o software Connector será desinstalado automaticamente. Depois que a atualização for concluída, você deverá reinstalar o software de conector. É o método preferido desinstalar o software Connector antes de atualizar o sistema operacional. Desinstalar o software Connector depois que a atualização for concluída ainda é aceitável; No entanto, pode resultar em um estado inconsistente para o computador cliente com o servidor até que o software Connector seja desinstalado e reinstalado.

##### <a name="to-uninstall-connector-software-from-a-computer"></a>Para desinstalar o software Connector em um computador

1.  De um computador que está executando o Windows 7, Windows 8, Windows 8.1 ou Windows 10, abra o **Painel de Controle** e, em seguida, na seção de **Programas**, clique em **Exibir atualizações instaladas**.

2.  Na lista de programas instalados, selecione **Windows Server Essentials Connector**e clique em **desinstalar**.

3.  Na página de aviso, clique em **Sim**.

4.  Se a janela **Controle de Conta de Usuário** for exibida, clique em **Permitir**.

5.  No Windows Server Essentials, se a página do conector do Windows Server Essentials aparecer sugerindo para fechar a barra inicial, clique em **OK**.

6.  Aguarde até que o programa de desinstalação. Depois que o software for removido, **Windows Server Essentials Connector** não é mais exibido na lista de programas instalados ou atualizações. Além disso, os atalhos para o Launchpad e o painel não são mais exibidos na área de trabalho do computador.

> [!NOTE]
> - Desinstalar o software Connector não remover o computador da lista de computadores que são exibidos em **DISPOSITIVOS** no Painel. Para remover o computador do Painel, veja [remover um computador do servidor](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).
>   -   Quando você desinstalar o software Connector, pastas compartilhadas no computador cliente que foram mapeadas para o servidor não são excluídas. Você deve excluir manualmente as pastas compartilhadas são mapeadas para o servidor.
>
> -   Desinstalar o software Connector não desassocia o computador do domínio original. Você deve manualmente desassocie o computador do domínio. Para obter instruções, consulte [remover um computador de um domínio do Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).


###  <a name="disconnect-your-computer-from-or-reconnect-your-computer-to-the-server"></a><a name="BKMK_14"></a>Desconecte o computador do ou reconecte o computador ao servidor
 Para desconectar um computador do servidor, você deve concluir as seguintes etapas:


1. Desinstale o software Connector do computador usando o painel de controle. Para obter instruções passo a passo, consulte [desinstalar o software Connector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).


2. Desassocie o computador do domínio do Windows Server Essentials e associá-lo ao grupo de trabalho. Para obter instruções passo a passo para realizar o ingresso do Windows em um grupo de trabalho, consulte [Ingressar em um grupo de trabalho ou criar um](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).

3. Remova o computador do servidor usando o Painel. Para obter instruções passo a passo, consulte [remover um computador do servidor](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).

   Para se reconectar a um computador para o servidor foi desconectado da rede do Windows Server Essentials server anteriormente, você deve concluir as seguintes etapas:


4. Desinstale o software Connector do computador usando o painel de controle. Para obter instruções passo a passo, consulte [desinstalar o software Connector](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).

5. Desassocie o computador do domínio do Windows Server Essentials e associá-lo ao grupo de trabalho. Para obter instruções passo a passo para realizar o ingresso do Windows em um grupo de trabalho, consulte [Ingressar em um grupo de trabalho ou criar um](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).

6. Conecta o computador ao servidor usando o Assistente Connect do computador. Para obter instruções passo a passo, consulte [conectar computadores ao servidor](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).

###  <a name="how-backup-works-with-sleep-and-hibernate-modes"></a><a name="BKMK_Sleep"></a>Como o backup funciona com modos de suspensão e hibernação
 Se você selecionar o **ativar este computador para Backup** opção quando você conectar um computador para o servidor, o computador será automaticamente sai do modo de suspensão ou de hibernação diariamente, conforme especificado na agenda de Backup para que possa ser feito backup. Depois que o backup for concluído, o computador retornará para o modo de suspensão ou de hibernação, com base em suas configurações de gerenciamento de energia. Se você não selecionar essa opção, o servidor não fará backup um computador se o computador está em suspensão ou hibernação. Para obter mais informações, consulte [gerenciar o backup do cliente](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).

##  <a name="use-the-launchpad"></a><a name="BKMK_C"></a>Usar o Launchpad
 Você pode usar a barra inicial para acessar recursos compartilhados do servidor do Windows Server Essentials, realizar backups de computador e responder a alertas de integridade do sistema.

-   [Visão geral da barra inicial](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)

-   [Usar a Barra Inicial com um computador Mac](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)

## <a name="additional-references"></a>Referências adicionais

-   [Solucionar problemas de computadores que se conectam ao servidor](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md)

-   [Gerenciar contas de usuário](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)


-   [Usar pastas compartilhadas](Use-Shared-Folders-in-Windows-Server-Essentials.md)

-   [Trabalhar remotamente](Work-Remotely-in-Windows-Server-Essentials.md)

-   [Reproduzir mídia digital](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [Usar pastas compartilhadas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)

-   [Trabalhar remotamente](../use/Work-Remotely-in-Windows-Server-Essentials.md)

-   [Reproduzir mídia digital](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

