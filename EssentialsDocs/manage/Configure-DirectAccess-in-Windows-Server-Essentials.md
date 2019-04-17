---
title: Configurar o DirectAccess no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c959b6fc-c67e-46cd-a9cb-cee71a42fa4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cc336dcd2a5418aa79254108c941a02147112e8f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-directaccess-in-windows-server-essentials"></a>Configurar o DirectAccess no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este tópico fornece instruções passo a passo para configurar o DirectAccess no Windows Server Essentials para habilitar sua força de trabalho móvel perfeitamente conectar à rede s da organização de qualquer local remoto equipado Internet sem estabelecer uma conexão de rede virtual privada (VPN). O DirectAccess pode oferecer trabalhadores móveis a mesma experiência de conectividade dentro e fora do escritório de seus computadores Windows 8.1, Windows 8 e Windows 7.  
  
 No Windows Server Essentials, se o domínio contiver mais de um servidor Windows Server Essentials, DirectAccess deve ser configurado no controlador de domínio.  
  
> [!NOTE]
>  Este tópico fornece instruções para configurar o DirectAccess ao seu servidor Windows Server Essentials é o controlador de domínio. Se o servidor Windows Server Essentials é um membro do domínio, siga as instruções para configurar o DirectAccess um membro do domínio em [adicionar DirectAccess para uma implantação de acesso remoto existente (VPN)](https://technet.microsoft.com/library/jj574220.aspx) em vez disso.  
  
## <a name="process-overview"></a>Visão geral do processo  
 Para configurar o DirectAccess no Windows Server Essentials, conclua as etapas a seguir.  
  
> [!IMPORTANT]
>  Antes de usar os procedimentos neste guia para configurar o DirectAccess no Windows Server Essentials, você deve habilitar a VPN no servidor. Para obter instruções, consulte [VPN gerenciar](Manage-VPN-in-Windows-Server-Essentials.md).  
  
-   [Etapa 1: Adicionar ferramentas de gerenciamento de acesso remoto ao seu servidor](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddRAM)  
  
-   [Etapa 2: Alterar o endereço do adaptador de rede do servidor para um endereço IP estático](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddStaticIP)  
  
-   [Etapa 3: Preparar um certificado e registro DNS para o servidor de local de rede](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)  
  
    -   [Etapa 3a: conceder permissões completas para usuários autenticados para o modelo de certificado de servidor s da Web](#BKMK_GrantFullPermissions)  
  
    -   [Etapa 3b: registrar um certificado de servidor de rede local com um nome comum que pode ser resolvido da rede externa](#BKMK_EnrollaCertificate)  
  
    -   [Etapa 3c: adicionar um novo host no servidor DNS e mapeá-lo para o endereço do servidor Windows Server Essentials](#BKMK_MapNewHosttoServerAddress)  
  
-   [Etapa 4: Criar um grupo de segurança para computadores de clientes do DirectAccess](#BKMK_AddSecurityGroup)  
  
-   [Etapa 5: Ativar e configurar o DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableConfigureDA)  
  
    -   [Etapa 5a: habilitar DirectAccess usando o Console de gerenciamento de acesso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
    -   [Etapa 5b: remover o IPv6Prefix inválido no GPO de RRAS (somente Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
    -   [Etapa 5c: Ativar computadores cliente que executam o Windows 7 Enterprise para usar o DirectAccess](#BKMK_Step4cWindows7Setup)  
  
    -   [Etapa 5D: configurar o servidor de local de rede](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
    -   [Etapa 5e: adicionar uma chave do registro para ignorar a certificação da autoridade de certificação quando você estabelece um canal IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
-   [Etapa 6: Definir configurações de tabela de políticas de resolução de nome do servidor do DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NRPT)  
  
-   [Etapa 7: Configurar as regras de firewall TCP e UDP para o servidor do DirectAccess GPOs](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_TCPUDP)  
  
-   [Etapa 8: Alterar a configuração de DNS64 para ouvir a interface de IP HTTPS](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS64)  
  
-   [Etapa 9: Reservar portas para o serviço de WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_ExemptPort)  
  
-   [Etapa 10: Reiniciar o serviço WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_WinNAT)  
  
> [!NOTE]
>  [Apêndice: Configurar o DirectAccess usando o Windows PowerShell](#BKMK_AppendixBPowerShellScript) fornece um script do Windows PowerShell que você pode usar para executar a instalação do DirectAccess.  
  
##  <a name="BKMK_AddRAM"></a>Etapa 1: Adicionar ferramentas de gerenciamento de acesso remoto ao seu servidor  
  
#### <a name="to-add-remote-access-management-tools"></a>Adicionar ferramentas de gerenciamento de acesso remoto  
  
1.  No servidor, no canto inferior esquerdo da página inicial, clique no **Gerenciador do servidor** ícone.  
  
     No Windows Server Essentials, você precisará pesquisar do Gerenciador de servidores para abri-lo. Na página inicial, digite **Gerenciador do servidor**e clique em **Gerenciador do servidor** nos resultados da pesquisa. Para fixar o Gerenciador do servidor para a página inicial, clique com botão direito do Gerenciador de servidores nos resultados da pesquisa e clique em **Fixar na tela inicial**.  
  
2.  Se um **User Account Control** aviso será exibido, clique em **Sim**.  
  
3.  No painel do Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**.  
  
4.  Na adição de funções e recursos do assistente, faça o seguinte:  
  
    1.  Sobre o **tipo de instalação** página, clique em **instalação baseada em função ou recurso com base em**.  
  
    2.  No **página seleção de servidor** (ou o **servidor de destino Select** página no Windows Server Essentials), clique em **selecionar um servidor do pool servidor**.  
  
    3.  No **recursos** página, expanda **ferramentas de administração de servidor remoto (Installed)**, expanda **ferramentas de gerenciamento de acesso remoto (Installed)**, expanda **ferramentas de administração de função (Installed)**, expanda **ferramentas de gerenciamento de acesso remoto**e, em seguida, selecione **GUI de acesso remoto e as ferramentas de linha de comando**.  
  
    4.  Siga as instruções para concluir o assistente.  
  
##  <a name="BKMK_AddStaticIP"></a>Etapa 2: Alterar o endereço do adaptador de rede do servidor para um endereço IP estático  
 O DirectAccess requer um adaptador com um endereço IP estático. Você precisa alterar o endereço IP para o adaptador de rede local em seu servidor.  
  
#### <a name="to-add-a-static-ip-address"></a>Para adicionar um endereço IP estático  
  
1.  Na página inicial, abra **painel de controle**.  
  
2.  Clique em **de rede e Internet**e clique em **Exibir status da rede e tarefas**.  
  
3.  No painel de tarefas do **Central de rede e compartilhamento**, clique em **alterar as configurações do adaptador**.  
  
4.  Clique com botão direito do adaptador de rede local e, em seguida, clique em **propriedades**.  
  
5.  Sobre o **rede**, clique em **Internet Protocol versão 4 (TCP/IPv4)**e, em seguida, clique em **propriedades**.  
  
6.  Sobre o **geral**, clique em **usar o seguinte endereço IP**e, em seguida, digite o endereço IP que você deseja usar.  
  
     Um valor padrão de máscara de sub-rede aparece automaticamente no **máscara de sub-rede** caixa. Aceite o valor padrão, ou digite o valor de máscara de sub-rede que você deseja usar.  
  
7.  No **gateway padrão**, digite o endereço IP do seu gateway padrão.  
  
8.  No **servidor DNS preferencial**, digite o endereço IP do seu servidor DNS.  
  
    > [!NOTE]
    >  Use o endereço IP que é atribuído ao seu adaptador de rede pelo DHCP (por exemplo, 192.168) em vez de uma rede de loopback (por exemplo, 127.0.0.1). Para descobrir o endereço IP, execute **ipconfig** em um prompt de comando.  
  
9. No **servidor DNS alternativo**, digite o endereço IP do seu servidor DNS alternativo, se houver.  
  
10. Clique em **Okey**e clique em **fechar**.  
  
> [!IMPORTANT]
>  Certifique-se de que você configure o roteador para encaminhar portas 80 e 443 para o novo endereço IP estático do servidor.  
  
##  <a name="BKMK_DNS"></a>Etapa 3: Preparar um certificado e registro DNS para o servidor de local de rede  
 Para preparar um certificado e registro DNS para o servidor de local de rede, execute as seguintes tarefas:  
  
-   [Etapa 3a: conceder permissões completas para usuários autenticados para o modelo de certificado de servidor s da Web](#BKMK_GrantFullPermissions)  
  
-   [Etapa 3b: registrar um certificado de servidor de rede local com um nome comum que pode ser resolvido da rede externa](#BKMK_EnrollaCertificate)  
  
-   [Etapa 3c: adicionar um novo host no servidor DNS e mapeá-lo para o endereço do servidor Windows Server Essentials.](#BKMK_MapNewHosttoServerAddress)  
  
###  <a name="BKMK_GrantFullPermissions"></a>Etapa 3a: conceder permissões completas para usuários autenticados para o modelo de certificado de servidor s da Web  
 A primeira tarefa é conceder permissões completas para autenticar os usuários para o modelo de certificado s de servidor da Web na autoridade de certificação.  
  
####  <a name="BKMK_ToGrantFullPermissions"></a>Para conceder permissões completas para usuários autenticados para o servidor Web modelo de certificado s  
  
1.  Sobre o **iniciar** página, abrir **autoridade de certificação**.  
  
2.  Na árvore de console, em **autoridade de certificação (Local)**, expanda **< servername\ >-CA**, clique com botão direito **modelos de certificado**e clique em **gerenciar**.  
  
3.  Em **autoridade de certificação (Local)**, clique com botão direito **servidor Web**e clique em **propriedades**.  
  
4.  Nas propriedades do servidor Web, no **segurança**, clique em **usuários autenticados**, selecione **controle total**e clique em **Okey**.  
  
5.  Reinicie **serviços de certificados do Active Directory**. No painel de controle, abra **exibir serviços locais**. Na lista de serviços, clique com botão direito **serviços de certificados do Active Directory**e clique em **reiniciar**.  
  
###  <a name="BKMK_EnrollaCertificate"></a>Etapa 3b: registrar um certificado de servidor de rede local com um nome comum que pode ser resolvido da rede externa  
 Em seguida, registre um certificado de servidor de rede local com um nome comum que pode ser resolvido da rede externa.  
  
####  <a name="BKMK_ToEnrollaCertificate"></a>Para registrar um certificado de servidor de rede local  
  
1.  Sobre o **iniciar** página, abrir **mmc** (Console de gerenciamento Microsoft).  
  
2.  Se um **User Account Control** aparecer a mensagem de aviso, clique em **Sim**.  
  
     O Console de gerenciamento Microsoft (MMC) é aberta.  
  
3.  Sobre o **arquivo** menu, clique em **Adicionar/Remover Snap-ins**.  
  
4.  No **remotos Snap-ins ou adicionar**, clique em **certificados**e clique em **adicionar**.  
  
5.  Sobre o **snap-in Certificados** página, clique em **conta de computador**e, em seguida, clique em **próxima**.  
  
6.  No **Selecionar computador** página, clique em **computador Local**, clique em **concluir**e clique em **Okey**.  
  
7.  Na árvore de console, expanda **certificados (computador Local)**, expanda **pessoais**, clique com botão direito **certificados**e em seguida, em **todas as tarefas**, clique em **Solicitar novo certificado**.  
  
8.  Quando o Assistente de registro de certificado for exibida, clique em **próxima**.  
  
9. Sobre o **selecione política de registro de certificado** página, clique em **próxima**.  
  
10. Sobre o **solicitar certificados** página, selecione o **servidor Web** caixa de seleção e, em seguida, clique em **mais informações são necessárias para registrar esse certificado**.  
  
11. No **propriedades certificado** caixa, insira as seguintes configurações em **nome do requerente**:  
  
    1.  Para **tipo**, selecione **nome comum**.  
  
    2.  Para **valor**, digite o nome do servidor de local de rede (por exemplo, o DirectAccess-NLS.contoso.local) e, em seguida, clique em **adicionar**.  
  
    3.  Clique em **Okey**e clique em **registrar**.  
  
12. Quando terminar de registro de certificado, clique em **concluir**.  
  
###  <a name="BKMK_MapNewHosttoServerAddress"></a>Etapa 3c: adicionar um novo host no servidor DNS e mapeá-lo para o endereço do servidor Windows Server Essentials  
 Para concluir a configuração de DNS, adicione um novo host no servidor DNS e mapeá-lo para o endereço do servidor Windows Server Essentials.  
  
####  <a name="BKMK_ToMapNewHosttoServerAddress"></a>Para mapear um novo host para o endereço do servidor Windows Server Essentials  
  
1.  Na página inicial, abra o Gerenciador DNS. Para abrir o Gerenciador DNS, procure **dnsmgmt.msc**e clique em **dnsmgmt.msc** nos resultados.  
  
2.  Na árvore de console do Gerenciador DNS, expanda o servidor local, **zonas de pesquisa direta**, clique com botão direito a zona com sufixo de domínio do servidor s e, em seguida, clique em **novo Host (A ou AAAA)**.  
  
3.  Digite o nome e endereço IP do servidor (por exemplo, o DirectAccess-NLS.contoso.local) e ele correspondente endereço do servidor (por exemplo, 192.168).  
  
4.  Clique em **Adicionar Host**, clique em **Okey**e clique em **feito**.  
  
##  <a name="BKMK_AddSecurityGroup"></a>Etapa 4: Criar um grupo de segurança para computadores de clientes do DirectAccess  
 Em seguida, crie um grupo de segurança a ser usado para os computadores cliente DirectAccess e, em seguida, adicione as contas de computador ao grupo.  
  
#### <a name="to-add-a-security-group-for-client-computers-that-use-directaccess"></a>Para adicionar um grupo de segurança para computadores cliente que usar o DirectAccess  
  
1.  No painel do Gerenciador do servidor, clique em **ferramentas**e clique em **usuários e Active Directory computadores**.  
  
    > [!NOTE]
    >  Se você não vir **Active Directory usuários e computadores** sobre o **ferramentas** menu, você precisa instalar o recurso. Para instalar o Active Directory usuários e grupos, execute o seguinte cmdlet do Windows PowerShell como administrador:`Install-WindowsFeature RSAT-ADDS-Tools`. Para obter mais informações, consulte [instalar ou remover o pacote de ferramentas de administração de servidor remoto](https://technet.microsoft.com/library/cc730825.aspx).  
  
2.  Na árvore de console, expanda o servidor, clique com botão direito **usuários**, clique em **nova**e clique em **grupo**.  
  
3.  Insira um nome de grupo, o escopo do grupo e o tipo de grupo (criam um grupo de segurança) e, em seguida, clique em **Okey**.  
  
 O novo grupo de segurança é adicionado ao **usuários** pasta.  
  
#### <a name="to-add-computer-accounts-to-the-security-group"></a>Para adicionar contas de computador ao grupo de segurança  
  
1.  No painel do Gerenciador do servidor, clique em **ferramentas**e clique em **usuários e Active Directory computadores**.  
  
2.  Na árvore de console, expanda o servidor e, em seguida, clique em **usuários**.  
  
3.  Na lista de contas de usuário e grupos de segurança no servidor, clique com botão direito do grupo de segurança que você criou para DirectAccess e, em seguida, clique em **propriedades**.  
  
4.  Sobre o **membros**, clique em **adicionar**.  
  
5.  Na caixa de diálogo, digite os nomes das contas de computador que você deseja adicionar ao grupo, separando os nomes com um ponto e vírgula (;). Clique em **verificar nomes**.  
  
6.  Depois que as contas de computador são validadas, clique em **Okey**. Clique em **Okey** novamente.  
  
> [!NOTE]
>  Você também pode usar o **membro do** guia nas propriedades da conta de computador para adicionar a conta ao grupo de segurança.  
  
##  <a name="BKMK_EnableConfigureDA"></a>Etapa 5: Ativar e configurar o DirectAccess  
 Para ativar e configurar o DirectAccess no Windows Server Essentials, você deve concluir as etapas a seguir:  
  
-   [Etapa 5a: habilitar DirectAccess usando o Console de gerenciamento de acesso remoto](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
-   [Etapa 5b: remover o IPv6Prefix inválido no GPO de RRAS (somente Windows Server Essentials)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
-   [Etapa 5c: Ativar computadores cliente que executam o Windows 7 Enterprise para usar o DirectAccess](#BKMK_Step4cWindows7Setup)  
  
-   [Etapa 5D: configurar o servidor de local de rede](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
-   [Etapa 5e: adicionar uma chave do registro para ignorar a certificação da autoridade de certificação quando você estabelece um canal IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
###  <a name="BKMK_EnableDA"></a>Etapa 5a: habilitar DirectAccess usando o Console de gerenciamento de acesso remoto  
 Esta seção fornece instruções passo a passo para habilitar o DirectAccess no Windows Server Essentials. Se você não tiver configurado ainda VPN no servidor, você deve fazer isso antes de iniciar este procedimento. Para obter instruções, consulte [VPN gerenciar](Manage-VPN-in-Windows-Server-Essentials.md).  
  
##### <a name="to-enable-directaccess-by-using-the-remote-access-management-console"></a>Para habilitar o DirectAccess, usando o Console de gerenciamento de acesso remoto  
  
1.  Na página inicial, abra **gerenciamento de acesso remoto**.  
  
2.  No Assistente para habilitar o DirectAccess, faça o seguinte:  
  
    1.  Análise **DirectAccess pré-requisitos**e clique em **próxima**.  
  
    2.  Sobre o **selecionar grupos** guia, adicione o grupo de segurança que você criou anteriormente para clientes do DirectAccess. (Se você não tiver criado o grupo de segurança, consulte [etapa 4: criar um grupo de segurança para o cliente do DirectAccess computadores](#BKMK_AddSecurityGroup) para obter instruções.)  
  
    3.  No **selecionar grupos**, clique em **habilitar DirectAccess para computadores móveis apenas** se você quiser permitir que os computadores móveis usar o DirectAccess para acessar remotamente o servidor e, em seguida, clique em **próxima**.  
  
    4.  Em **topologia de rede**, selecione a topologia do servidor e, em seguida, clique em **próxima**.  
  
    5.  Em **lista de pesquisa de sufixos DNS**, adicione o sufixo DNS adicional para os computadores cliente, se necessário e, em seguida, clique em **próxima**.  
  
        > [!NOTE]
        >  Por padrão, o Assistente para habilitar o DirectAccess já adiciona o sufixo DNS do domínio atual. No entanto, você pode adicionar mais se necessário.  
  
    6.  Examine os objetos de diretiva de grupo (GPOs) que serão aplicadas e modificá-las, se necessário.  
  
    7.  Clique em **próxima**e clique em **concluir**.  
  
    8.  Reinicie o serviço de gerenciamento de acesso remoto, executando o seguinte comando do Windows PowerShell em modo elevado:  
  
        ```powershell  
        Restart-Service RaMgmtSvc   
        ```  
  
###  <a name="BKMK_RemoveIPv6"></a>Etapa 5b: remover o IPv6Prefix inválido no GPO de RRAS (somente Windows Server Essentials)  
  Esta seção se aplica a um servidor que executa o Windows Server Essentials.  
  
 Abra o Windows PowerShell como administrador e execute os seguintes comandos:  
  
```powershell  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
```  
  
###  <a name="BKMK_Step4cWindows7Setup"></a>Etapa 5c: Ativar computadores cliente que executam o Windows 7 Enterprise para usar o DirectAccess  
 Se você tiver computadores cliente que executam o Windows 7 Enterprise, conclua o procedimento a seguir para habilitar o DirectAccess esses computadores.  
  
##### <a name="to-enable--windows-7-enterprise-computers-to-use-directaccess"></a>Para permitir que os computadores Windows 7 Enterprise usar o DirectAccess  
  
1.  Na página inicial do servidor s, abra **gerenciamento de acesso remoto**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **configuração**. Em seguida, no **detalhes de configuração** painel, em **etapa 2**, clique em **editar**.  
  
     Abre o Assistente de instalação do servidor de acesso remoto.  
  
3.  Sobre o **autenticação** guia, escolha o certificado da CA (autoridade de certificação) que será o certificado raiz confiável (você pode escolher o certificado da CA do servidor Windows Server Essentials). Clique em **computadores cliente de habilitar o Windows 7 para se conectar via DirectAccess**e clique em **próxima**.  
  
4.  Siga as instruções para concluir o assistente.  
  
> [!IMPORTANT]
>  Há um problema conhecido para computadores Windows 7 Enterprise conectar-se pela DirectAccess se o servidor Windows Server Essentials não vieram com UR1 pré-instalado. Para habilitar conexões do DirectAccess nesse ambiente, você deve executar estas etapas adicionais:  
>   
>  1.  Instale o hotfix descrito no [artigo da Base de dados de Conhecimento Microsoft (KB) 2796394](https://support.microsoft.com/kb/2796394) no servidor Windows Server Essentials. Em seguida, reinicie o servidor.  
> 2.  Instale o hotfix descrito no [artigo da Base de dados de Conhecimento Microsoft (KB) 2615847](https://support.microsoft.com/kb/2615847) em cada computador com Windows 7.  
>   
>      Esse problema foi resolvido no Windows Server Essentials.  
  
###  <a name="BKMK_NLS"></a>Etapa 5D: configurar o servidor de local de rede  
 Esta seção fornece instruções passo a passo para definir as configurações de servidor de local de rede.  
  
> [!NOTE]
>  Antes de começar, copie o conteúdo da pasta \inetpub\wwwroot < SystemDrive\ > para o < SystemDrive\ > \Program Files\Windows Server\Bin\WebApps\Site\insideoutside Files\Windows. Também copie o arquivo default.aspx o < SystemDrive\ > \Program Files\Windows Server\Bin\WebApps\Site Files\Windows para o < SystemDrive\ > \Program Files\Windows Server\Bin\WebApps\Site\insideoutside Files\Windows.  
  
##### <a name="to-configure-the-network-location-server"></a>Para configurar o servidor de local de rede  
  
1.  Na página inicial, abra **gerenciamento de acesso remoto**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **configuração**e no **instalação do acesso remoto** fornece detalhes sobre o painel, na **etapa 3**, clique em **editar**.  
  
3.  No Assistente de instalação de servidor de acesso remoto, no **o servidor de rede local**, selecione **o servidor de local de rede é implantado no servidor de acesso remoto**e, em seguida, selecione o certificado foi emitido anteriormente (em [etapa 3: preparar um certificado e registro DNS para o servidor de local de rede](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)).  
  
4.  Siga as instruções para concluir o assistente e clique em **concluir**.  
  
###  <a name="BKMK_CA"></a>Etapa 5e: adicionar uma chave do registro para ignorar a certificação da autoridade de certificação quando você estabelece um canal IPsec  
 A próxima etapa é configurar o servidor para ignorar a certificação da autoridade de certificação quando é estabelecer um canal de IPsec.  
  
##### <a name="to-add-a-registry-key-to-bypass-the-ca-certification"></a>Para adicionar uma chave do registro para ignorar a certificação da autoridade de certificação  
  
1.  Na página inicial, abra **regedit** (o Editor do registro).  
  
2.  No Editor do registro, expanda **HKEY_LOCAL_MACHINE**, expanda **sistema**, expanda **CurrentControlSet**, expanda **serviços**e expanda **IKEEXT**.  
  
3.  Em **IKEEXT**, clique com botão direito **parâmetros**, clique em **nova**e clique em **DWORD (32 bits) valor**.  
  
4.  Renomeie o valor recém-adicionado para **ikeflags**.  
  
5.  Clique duas vezes em **ikeflags**, defina o **tipo** para **Hexadecimal**, defina o valor como **8000**e clique em **Okey**.  
  
> [!NOTE]
>  Você pode usar o seguinte comando do Windows PowerShell em modo elevado para adicionar essa chave do registro:  
>   
>  `Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000`  
  
##  <a name="BKMK_NRPT"></a>Etapa 6: Definir configurações de tabela de políticas de resolução de nome do servidor do DirectAccess  
 Esta seção fornece instruções para editar as entradas de tabela de política de resolução de nome (NPRT) para endereços internos (por exemplo, entradas com um sufixo contoso. local) para GPOs de cliente do DirectAccess e, em seguida, defina o endereço da interface IPHTTPS.  
  
#### <a name="to-configure-name-resolution-policy-table-entries"></a>Para configurar entradas de tabela de políticas de resolução de nome  
  
1.  Na página inicial, abra **Group Policy Management**.  
  
2.  No console de gerenciamento de política de grupo, clique em padrão floresta e no domínio, clique com botão direito **configurações de cliente do DirectAccess**e clique em **editar**.  
  
3.  Clique em **as configurações do computador**, clique em **políticas**, clique em **configurações do Windows**e clique em **política de resolução de nome**. Escolha a entrada que tem o namespace que é idêntico ao sufixo DNS e, em seguida, clique em **Editar regra**.  
  
4.  Clique no **as configurações de DNS para DirectAccess** guia; Selecione **configurações de habilitar o DNS para DirectAccess nessa regra**. Adicione o endereço IPv6 para a interface de IP HTTPS na lista de servidor DNS.  
  
    > [!NOTE]
    >  Você pode usar o seguinte comando do Windows PowerShell para obter o endereço IPv6:  
    >   
    >  `(Get-NetIPInterface -InterfaceAlias IPHTTPSInterface | Get-NetIPAddress -PrefixLength 128)[1].IPAddress`  
  
##  <a name="BKMK_TCPUDP"></a>Etapa 7: Configurar as regras de firewall TCP e UDP para o servidor do DirectAccess GPOs  
 Esta seção inclui instruções passo a passo para configurar as regras de firewall TCP e UDP para o servidor do DirectAccess GPOs.  
  
#### <a name="to-configure-firewall-rules"></a>Para configurar as regras de firewall  
  
1.  Na página inicial, abra **Group Policy Management**.  
  
2.  No console de gerenciamento de política de grupo, clique em padrão floresta e no domínio, clique com botão direito **configurações do servidor DirectAccess**e clique em **editar**.  
  
3.  Clique em **configuração do computador**, clique em **políticas**, clique em **configurações do Windows**, clique em **configurações de segurança**, clique em **Firewall do Windows com segurança avançada**, clique em próximo nível **Firewall do Windows com segurança avançada**e clique em **regras de entrada**. Clique com botão direito **nome de domínio Server (TCP-In)**e clique em **propriedades**.  
  
4.  Clique no **escopo** guia e no **endereço IP Local** listar, adicione o endereço IPv6 da interface IP-HTTPS.  
  
5.  Repita o mesmo procedimento para **servidor de nomes de domínio (UDP-In)**.  
  
##  <a name="BKMK_DNS64"></a>Etapa 8: Alterar a configuração de DNS64 para ouvir a interface de IP HTTPS  
 Você deve alterar a configuração de DNS64 para ouvir a interface de IP HTTPS usando o seguinte comando do Windows PowerShell.  
  
```powershell  
Set-NetDnsTransitionConfiguration  �AcceptInterface IPHTTPSInterface  
```  
  
##  <a name="BKMK_ExemptPort"></a>Etapa 9: Reservar portas para o serviço de WinNat  
 Use o seguinte comando do Windows PowerShell para reservar portas para o serviço WinNat. Substitua "192.168.1.100" com o endereço IPv4 real do seu servidor Windows Server Essentials.  
  
```powershell  
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
```  
  
> [!IMPORTANT]
>  Para evitar conflitos de porta com aplicativos, certifique-se de que o intervalo de portas reservado para o serviço WinNat não inclui porta 6602.  
  
##  <a name="BKMK_WinNAT"></a>Etapa 10: Reiniciar o serviço WinNat  
 Reinicie o serviço de Driver do Windows NAT (WinNat) executando o seguinte comando do Windows PowerShell.  
  
```powershell  
Restart-Service winnat  
```  
  
##  <a name="BKMK_AppendixBPowerShellScript"></a>Apêndice: Configurar o DirectAccess usando o Windows PowerShell  
 Esta seção descreve como instalar e configurar o DirectAccess usando o Windows PowerShell.  
  
### <a name="preparation"></a>Preparação  
 Antes de começar a configurar seu servidor para DirectAccess, você deve concluir o seguinte:  
  
1.  Siga o procedimento [etapa 3: preparar um certificado e registro DNS para o servidor de local de rede](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS) para registrar um certificado denominado **DirectAccess-NLS.contoso.com** (onde **contoso.com** é substituído pelo seu nome de domínio interno real) e adicionar um registro DNS para o servidor da rede local (NLS).  
  
2.  Adicionar um grupo de segurança chamado **DirectAccessClients** no Active Directory, e depois adicionar computadores cliente para o qual você deseja fornecer a funcionalidade do DirectAccess. Para obter mais informações, consulte [etapa 4: criar um grupo de segurança para o cliente do DirectAccess computadores](#BKMK_AddSecurityGroup).  
  
### <a name="commands"></a>Comandos  
  
> [!IMPORTANT]
>  No Windows Server Essentials, você não precisa remover o prefixo IPv6 desnecessário GPO. Excluir a seção de código com o seguinte rótulo:`# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO`.  
  
```powershell  
# Add Remote Access role if not installed yet  
$ra = Get-WindowsFeature RemoteAccess  
If ($ra.Installed -eq $FALSE) { Add-WindowsFeature RemoteAccess }  
  
# Server may need to restart if you installed RemoteAccess role in the above step  
  
# Set the internet domain name to access server, replace contoso.com below with your own domain name  
$InternetDomain = "www.contoso.com"  
#Set the SG name which you create for DA clients  
$DaSecurityGroup = "DirectAccessClients"  
#Set the internal domain name  
$InternalDomain = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().Name  
  
# Set static IP and DNS settings  
$NetConfig = Get-WmiObject Win32_NetworkAdapterConfiguration -Filter "IPEnabled=$true"  
$CurrentIP = $NetConfig.IPAddress[0]  
$SubnetMask = $NetConfig.IPSubnet | Where-Object{$_ -like "*.*.*.*"}  
$NetConfig.EnableStatic($CurrentIP, $SubnetMask)  
$NetConfig.SetGateways($NetConfig.DefaultIPGateway)  
$NetConfig.SetDNSServerSearchOrder($CurrentIP)  
  
# Get physical adapter name and the certificate for NLS server  
$Adapter = (Get-WmiObject -Class Win32_NetworkAdapter -Filter "NetEnabled=$true").NetConnectionId  
$Certs = dir cert:\LocalMachine\My  
$nlscert = $certs | Where-Object{$_.Subject -like "*CN=DirectAccess-NLS*"}  
  
# Add regkey to bypass CA cert for IPsec authentication  
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000  
  
# Install DirectAccess.   
Install-RemoteAccess -NoPrerequisite -DAInstallType FullInstall -InternetInterface $Adapter -InternalInterface $Adapter -ConnectToAddress $InternetDomain -nlscertificate $nlscert -force  
  
#Restart Remote Access Management service  
Restart-Service RaMgmtSvc  
  
# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO   
  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
  
# Enable client computers running Windows 7 to use DirectAccess  
$allcertsinroot = dir cert:\LocalMachine\root  
$rootcert = $allcertsinroot | Where-Object{$_.Subject -like "*-CAA*"}  
Set-DAServer  �IPSecRootCertificate $rootcert[0]  
Set  �DAClient  �OnlyRemoteComputers Disabled -Downlevel Enabled  
  
#Set the appropriate security group used for DA client computers. Replace the group name below with the one you created for DA clients  
Add-DAClient -SecurityGroupNameList $DaSecurityGroup   
Remove-DAClient -SecurityGroupNameList "Domain Computers"  
  
# Gather DNS64 IP address information  
$Remoteaccess = get-remoteaccess  
$IPinterface = get-netipinterface -InterfaceAlias IPHTTPSInterface | get-netipaddress -PrefixLength 128  
$DNS64IP=$IPInterface[1].IPaddress  
$Natconfig = Get-NetNatTransitionConfiguration  
  
# Configure TCP and UDP firewall rules for the DirectAccess server GPO  
$GpoName = 'GPO:'+$InternalDomain+'\DirectAccess Server Settings'  
Get-NetFirewallRule -PolicyStore $GpoName -Displayname "Domain Name Server (TCP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
Get-NetFirewallrule -PolicyStore $GpoName -Displayname "Domain Name Server (UDP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
  
# Configure the name resolution policy settings for the DirectAccess server, replace the DNS suffix below with the one in your domain  
$Suffix = '.' + $InternalDomain  
set-daclientdnsconfiguration -DNSsuffix $Suffix -DNSIPAddress $DNS64IP  
  
# Change the DNS64 configuration to listen to IP-HTTPS interface  
Set-NetDnsTransitionConfiguration -AcceptInterface IPHTTPSInterface  
  
# Copy the necessary files to NLS site folder  
XCOPY 'C:\inetpub\wwwroot' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /E /Y  
XCOPY 'C:\Program Files\Windows Server\Bin\WebApps\Site\Default.aspx' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /Y  
  
# Reserve ports for the WinNat service  
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
  
# Restart the WinNat service  
Restart-Service winnat  
```  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o acesso em qualquer lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
