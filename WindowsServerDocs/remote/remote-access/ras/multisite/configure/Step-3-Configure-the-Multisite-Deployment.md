---
title: Etapa 3 configurar uma implantação multissite
description: Este tópico faz parte do guia de implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea7ecd52-4c12-4a49-92fd-b8c08cec42a9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5c74a9277af3853d709a8ecd58c1e53e1bccc719
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812507"
---
# <a name="step-3-configure-the-multisite-deployment"></a>Etapa 3 configurar uma implantação multissite

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Depois de configurar a infraestrutura de multissite, siga estas etapas para configurar a implantação multissite do acesso remoto.  
  
|Tarefa|Descrição|  
|----|--------|  
|3.1. Configurar servidores de acesso remoto|Configure servidores adicionais do acesso remoto como configurar endereços IP, uni-las ao domínio e instalar a função acesso remoto.|  
|3.2. Conceder acesso de administrador|Conceder privilégios nos servidores de acesso remoto adicionais para o administrador do DirectAccess.|  
|3.3. Configurar o IP-HTTPS para uma implantação multissite|Configure o certificado IP-HTTPS usado em uma implantação multissite.|  
|3.4. Configurar o servidor de local de rede para uma implantação multissite|Configure o certificado de servidor de local de rede usado em uma implantação multissite.|  
|3.5. Configurar clientes DirectAccess para uma implantação multissite|Remova os computadores cliente Windows 7 dos grupos de segurança do Windows 8.|  
|3.6. Habilitar implantação multissite|Habilite implantação multissite no primeiro servidor de acesso remoto.|  
|3.7. Adicionar pontos de entrada à implantação multissite|Adicione pontos de entrada adicionais para uma implantação multissite.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigServer"></a>3.1. Configurar servidores de acesso remoto  

  
### <a name="to-install-the-remote-access-role"></a>Para instalar a função Acesso Remoto.  
  
1.  Certifique-se de que cada servidor de acesso remoto é configurado com a topologia de implantação apropriada (borda, atrás de um NAT, uma única interface de rede) e as rotas correspondentes.  
  
2.  Configure os endereços IP em cada servidor acesso remoto de acordo com a topologia de site e o esquema de endereçamento de IP da sua organização.  
  
3.  Junte-se cada servidor de acesso remoto em um domínio do Active Directory.  
  
4.  No console do Gerenciador do servidor, nos **Dashboard**, clique em **adicionar funções e recursos**.  
  
5.   Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.  
  
6.  Sobre o **selecionar funções do servidor** caixa de diálogo, selecione **acesso remoto**e, em seguida, clique em **próxima**.  
  
7.  Clique em **próxima** três vezes.  
  
8.  Sobre o **selecionar serviços de função** caixa de diálogo, selecione **DirectAccess e VPN (RAS)** e, em seguida, clique em **adicionar recursos**.  
  
9.  Selecione **roteamento**, selecione **Proxy de aplicativo Web**, clique em **adicionar recursos**e, em seguida, clique em **Avançar**.  
  
10. Clique em **Avançar**e, em seguida, clique em **Instalar**.  
  
11.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem-sucedida e clique em **Fechar**.  
  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  

  
As etapas 1 a 3 devem ser executadas manualmente e não são realizadas usando este cmdlet do Windows PowerShell.  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Admin"></a>3.2. Conceder acesso de administrador  
  
#### <a name="to-grant-administrator-permissions"></a>Para conceder permissões de administrador  
  
1.  No servidor de acesso remoto no ponto de entrada adicionais: Sobre o **inicie** tela, digite **gerenciamento do computador**, e, em seguida, pressione ENTER.  
  
2.  No painel esquerdo, clique em **usuários e grupos locais**.  
  
3.  Clique duas vezes em **grupos**e, em seguida, clique duas vezes em **administradores**.  
  
4.  Sobre o **propriedades de administradores** caixa de diálogo, clique em **adicionar**e, na **selecionar usuários, computadores, contas de serviço ou grupos** caixa de diálogo, clique em  **Locais**.  
  
5.  Sobre o **locais** na caixa a **local** de árvore, clique no local que contém a conta de usuário do administrador do DirectAccess e, em seguida, clique em **Okey**.  
  
6.  No **digite os nomes de objeto para selecionar**, insira o nome de usuário do administrador do DirectAccess e, em seguida, clique em **Okey** duas vezes.  
  
7.  Sobre o **propriedades de administradores** caixa de diálogo, clique em **Okey**.  
  
8.  Feche a janela de gerenciamento do computador.  
  
9. Repita esse procedimento em todos os servidores de acesso remoto que farão parte da implantação multissite.  
  
## <a name="BKMK_IPHTTPS"></a>3.3. Configurar o IP-HTTPS para uma implantação multissite  
Em cada servidor de acesso remoto que será adicionado à implantação multissite, um certificado SSL é necessário para verificar a conexão HTTPS ao servidor da web IP-HTTPS. A associação no grupo local **Administradores**, ou equivalente, é o mínimo necessário para concluir esse procedimento.  
  
#### <a name="to-obtain-an-ip-https-certificate"></a>Para obter um certificado IP-HTTPS  
  
1.  Em cada servidor de acesso remoto: Sobre o **inicie** tela, digite **mmc**, e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  Clique em **Arquivo** e em **Adicionar/Remover Snap-ins**.  
  
3.  Clique em **Certificados**, **Adicionar**, **Conta de computador** e em **Avançar**, selecione **Computador local**, clique em **Concluir** e em **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Sobre o **solicitar certificados** página, clique no modelo de certificado do servidor Web e, em seguida, clique em **obter mais informações são necessárias para se registrar neste certificado**.  
  
    Se o modelo de certificado do servidor Web não aparecer, certifique-se de que a conta de computador do servidor de acesso remoto tem permissões de registro para o modelo de certificado do servidor Web. Para obter mais informações, consulte [configurar permissões no modelo de certificado de servidor Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  Sobre o **assunto** guia da **propriedades do certificado** na caixa **nome da entidade**, para **tipo**, selecione **comuns nome**.  
  
9. Na **valor**, digite o nome de domínio totalmente qualificado (FQDN) do nome da Internet do servidor de acesso remoto (por exemplo, Europe.contoso.com) e, em seguida, clique em **Add**.  
  
10. Clique em **OK**, **Registrar** e em **Concluir**.  
  
11. No painel de detalhes do snap-in Certificados, confirme se um novo certificado com o FQDN foi registrado com **Finalidades** de **Autenticação do Servidor**.  
  
12. Clique com o botão direito do mouse no certificado e, em seguida, clique em **Propriedades**.  
  
13. Em **Nome Amigável**, digite **Certificado IP-HTTPS** e clique em **OK**.  
  
    > [!TIP]  
    > As etapas 12 e 13 são opcionais, mas tornam mais fácil para selecionar o certificado para IP-HTTPS, ao configurar o acesso remoto.  
  
14. Repita esse procedimento em todos os servidores de acesso remoto em sua implantação.  
  
## <a name="BKMK_NLS"></a>3.4. Configurar o servidor de local de rede para uma implantação multissite  
Se você selecionou para configurar o site de servidor de local de rede no servidor de acesso remoto quando você configura seu primeiro servidor, cada novo servidor de acesso remoto que você adicione precisa ser configurado com um certificado de servidor Web que tem o mesmo nome de assunto que foi selecionado para t servidor de local para o primeiro servidor de rede. Cada servidor requer um certificado para autenticar a conexão ao servidor de local de rede e os computadores cliente localizados na rede interna devem ser capazes de resolver o nome do site no DNS.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Para instalar um certificado para local de rede  
  
1.  No servidor de Acesso Remoto: Sobre o **inicie** tela, digite **mmc**, e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  Clique em **Arquivo** e em **Adicionar/Remover Snap-ins**.  
  
3.  Clique em **Certificados**, **Adicionar**, **Conta de computador** e em **Avançar**, selecione **Computador local**, clique em **Concluir** e em **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
    > [!NOTE]  
    > Você também pode importar o mesmo certificado que foi usado para o servidor de local de rede para o primeiro servidor de acesso remoto.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Sobre o **solicitar certificados** página, clique no modelo de certificado do servidor Web e, em seguida, clique em **obter mais informações são necessárias para se registrar neste certificado**.  
  
    Se o modelo de certificado do servidor Web não aparecer, certifique-se de que a conta de computador do servidor de acesso remoto tem permissões de registro para o modelo de certificado do servidor Web. Para obter mais informações, consulte [configurar permissões no modelo de certificado de servidor Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  Sobre o **assunto** guia da **propriedades do certificado** na caixa **nome da entidade**, para **tipo**, selecione **comuns nome**.  
  
9. Na **valor**, digite o nome de domínio totalmente qualificado (FQDN) que foi configurado para o certificado de servidor de local de rede do primeiro servidor de acesso remoto (por exemplo, nls.corp.contoso.com) e, em seguida, clique em **Add**.  
  
10. Clique em **OK**, **Registrar** e em **Concluir**.  
  
11. No painel de detalhes do snap-in Certificados, confirme se um novo certificado com o FQDN foi registrado com **Finalidades** de **Autenticação do Servidor**.  
  
12. Clique com o botão direito do mouse no certificado e, em seguida, clique em **Propriedades**.  
  
13. Em **Nome Amigável**, digite **Certificado de Local de Rede** e clique em **OK**.  
  
    > [!TIP]  
    > As etapas 12 e 13 são opcionais, mas tornam mais fácil para você selecionar o certificado para o local de rede ao configurar o acesso remoto.  
  
14. Repita esse procedimento em todos os servidores de acesso remoto em sua implantação.  
  
### <a name="NLS"></a>Criar registros DNS de servidor de local de rede  
  
1.  No servidor DNS: Sobre o **inicie** tela, digite **Dnsmgmt. msc**, e pressione ENTER.  
  
2.  No painel esquerdo do **Gerenciador de DNS** console, abra a zona de pesquisa direta para a rede interna. Clique com botão direito a zona relevante e clique em **novo Host (A ou AAAA)**.  
  
3.  Sobre o **novo Host** na caixa de **nome (usa nome do domínio pai se deixado em branco)** , digite o nome que foi usado para o servidor de local de rede para o primeiro servidor de acesso remoto. No **endereço IP** caixa, digite o endereço de IPv4 voltado para a intranet do servidor de acesso remoto e, em seguida, clique em **Adicionar Host**. Na caixa de diálogo **DNS**, clique em **OK**.  
  
4.  Sobre o **novo Host** na caixa de **nome (usa nome do domínio pai se deixado em branco)** , digite o nome que foi usado para o servidor de local de rede para o primeiro servidor de acesso remoto. No **endereço IP** caixa, digite o endereço de IPv6 voltado para a intranet do servidor de acesso remoto e, em seguida, clique em **Adicionar Host**. Na caixa de diálogo **DNS**, clique em **OK**.  
  
5.  Repita as etapas 3 e 4 para cada servidor de acesso remoto na sua implantação.  
  
6.  Clique em **Concluído**.  
  
7.  Repita esse procedimento antes de adicionar servidores como pontos de entrada adicionais na implantação.  
  
## <a name="BKMK_Client"></a>3.5. Configurar clientes DirectAccess para uma implantação multissite  
Os computadores cliente Windows do DirectAccess devem ser membros de grupos de segurança que definem sua associação do DirectAccess. Antes de funcionalidade multissite está habilitada, esses grupos de segurança podem conter os clientes do Windows 8 e clientes Windows 7 (se o modo de "nível inferior" apropriado foi selecionado). Depois que a funcionalidade multissite está habilitada, grupos de segurança de cliente existente, no modo de servidor único, são convertidos em grupos de segurança para o Windows 8 apenas. Após a funcionalidade multissite está habilitada, os computadores cliente DirectAccess Windows 7 devem ser movidos para dedicado Windows 7 client grupos de segurança correspondentes (que são associados com pontos de entrada específicos) ou não será capazes de se conectar por meio do DirectAccess. Os clientes do Windows 7 primeiro devem ser removidos dos grupos de segurança existentes que agora estão em grupos de segurança do Windows 8. Cuidado:  Computadores cliente Windows 7 que são membros dos grupos de segurança de cliente Windows 7 e Windows 8 perderá conectividade remota e os clientes do Windows 7 sem o SP1 instalado perderá conectividade corporativa também. Portanto, todos os computadores de cliente do Windows 7 devem ser removidos de grupos de segurança do Windows 8.  
  
#### <a name="remove--windows-7--clients-from-windows-8-security-groups"></a>Remova os clientes do Windows 7 dos grupos de segurança do Windows 8  
  
1.  No controlador de domínio primário, clique em **inicie**e, em seguida, clique em **Active Directory Users and Computers**.  
  
2.  Para remover computadores do grupo de segurança, clique duas vezes no grupo de segurança e, na **< nome_do_grupo > propriedades** caixa de diálogo, clique o **membros** guia.  
  
3.  Selecione o computador de cliente do Windows 7 e, em seguida, clique em **remover**.  
  
4.  Repita este procedimento para remover os computadores de cliente do Windows 7 dos grupos de segurança do Windows 8.  
  
> [!IMPORTANT]  
> Ao habilitar uma configuração de acesso remoto multissite todos os computadores cliente (Windows 7 e Windows 8) perderá conectividade remota até que eles sejam capazes de se conectar à rede corporativa diretamente ou por VPN de atualizar suas diretivas de grupo. Isso é verdadeiro ao habilitar a funcionalidade multissite pela primeira vez e também ao desabilitar o multissite.  
  
## <a name="BKMK_Enable"></a>3.6. Habilitar implantação multissite  
Para configurar uma implantação multissite, habilite o recurso multissite no servidor de acesso remoto existente. Antes de habilitar o multissite em sua implantação, verifique se que você tem as seguintes informações:  
  
1.  Configurações do balanceador de carga global e endereços IP se desejar carregar equilibrar as conexões de cliente do DirectAccess em todos os pontos de entrada em sua implantação.  
  
2.  A segurança grupos contendo computadores de cliente do Windows 7 para o primeiro ponto de entrada em sua implantação, se você quiser permitir que computadores cliente de acesso remoto para Windows 7.  
  
3.  Nomes de objeto de diretiva de grupo se for necessário usar objetos de diretiva de grupo não padrão, são aplicadas em computadores de cliente do Windows 7 para o primeiro ponto de entrada em sua implantação, se você precisar de suporte para computadores cliente do Windows 7.  
  
### <a name="EnabledMultisite"></a>Para habilitar uma configuração multissite  
  
1.  No servidor de acesso remoto existente: Sobre o **inicie** tela, digite **RAMgmtUI.exe**, e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **Configuration**e, em seguida, no **tarefas** painel, clique em **habilitar multissite**.  
  
3.  No **habilitar implantação multissite** assistente no **antes de começar** , clique em **próxima**.  
  
4.  Sobre o **nome de implantação** página, na **nome da implantação multissite**, insira um nome para sua implantação. Na **nome do ponto de entrada primeiro**, insira um nome para identificar o primeiro ponto de entrada que é o servidor de acesso remoto atual e, em seguida, clique em **próxima**.  
  
5.  Sobre o **seleção de ponto de entrada** página, faça o seguinte:  
  
    -   Clique em **atribuir pontos de entrada automaticamente, e permitir que os clientes selecionar manualmente** para encaminhar automaticamente os computadores cliente para o ponto de entrada mais adequado, enquanto também permite aos computadores clientes selecionar um ponto de entrada manualmente. Seleção de ponto de entrada manual está disponível somente para computadores com Windows 8. Clique em **Avançar**.  
  
    -   Clique em **atribuir pontos de entrada automaticamente** automaticamente rotear os computadores cliente para o ponto de entrada mais adequado e, em seguida, clique em **próxima**.  
  
6.  Sobre o **balanceamento de carga Global** página, faça o seguinte:  
  
    -   Clique em **não, não use balanceamento de carga global** se você não quiser usar o balanceamento de carga global e, em seguida, clique em **próxima**.  
  
        > [!NOTE]  
        > Ao selecionar esse cliente opção computadores se conectam automaticamente ao seu ponto de entrada mais próximo.  
  
    -   Clique em **Sim, use o balanceamento de carga global** se você deseja carregar equilibrar o tráfego global entre todos os pontos de entrada. Na **digite a FQDN para ser usado por todos os pontos de entrada de balanceamento de carga global**, insira a FQDN, de balanceamento de carga global e, na **digite a endereço IP para esse ponto de entrada de balanceamento de carga global** que contém o primeiro Servidor de acesso remoto, insira a endereço IP para esse ponto de entrada de balanceamento de carga global e, em seguida, clique em **próxima**.  
  
7.  Sobre o **suporte ao cliente** página, faça o seguinte:  
  
    -   Para limitar o acesso a computadores cliente que executam o Windows 8 ou sistemas operacionais posteriores, clique em **limitar o acesso a computadores cliente que executam o Windows 8 ou um sistema operacional posterior**e, em seguida, clique em **próxima**.  
  
    -   Para permitir que computadores cliente que executam o Windows 7 para acessar esse ponto de entrada, clique em **permitir que os computadores que executam o Windows 7 para acessar esse ponto de entrada cliente**e clique em **Add**. Sobre o **selecionar grupos** caixa de diálogo, selecione os grupos de segurança que contém os computadores cliente Windows 7, clique em **Okey**e, em seguida, clique em **próxima**.  
  
8.  Sobre o **configurações de GPO do cliente** página, aceite os computadores de cliente do GPO para o Windows 7 padrão para esse ponto de entrada, digite o nome do GPO que deseja acesso remoto para criar automaticamente, ou clique em **procurar** para localizar os computadores cliente de GPO para o Windows 7 e, em seguida, clique em **próxima**.  
  
    > [!NOTE]  
    > -   O **configurações de GPO do cliente** página será exibida somente quando você configura o ponto de entrada para permitir que o cliente do Windows 7 computadores acessar o ponto de entrada.  
    > -   Você pode, opcionalmente, clique em **GPOs validar** para garantir que você tenha as permissões apropriadas para o GPO selecionado ou GPOs para esse ponto de entrada. Se o GPO não existe e será criado automaticamente, em seguida, cria e vincular as permissões são necessárias. No caso em que os GPOs criados manualmente, em seguida, editar, modificar a segurança e exclui as permissões são necessárias.  
  
9. Sobre o **resumo** , clique em **confirmar**.  
  
10. Sobre o **habilitando a implantação de multissite** caixa de diálogo, clique em **fechar** e, em seguida, no assistente habilitar implantação multissite, clique em **fechar**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Para habilitar uma implantação multissite chamada 'Contoso' no primeiro ponto de entrada denominado 'Edge1-US'. A implantação permite que os clientes manualmente, selecione o ponto de entrada e não usa um balanceador de carga global.  
  
```  
Enable-DAMultiSite -Name 'Contoso' -EntryPointName 'Edge1-US' -ManualEntryPointSelectionAllowed 'Enabled'  
```  
  
Para permitir o acesso de computadores cliente Windows 7 por meio do primeiro ponto de entrada por meio do grupo de segurança DA_Clients_US e usando o GPO DA_W7_Clients_GPO_US.  
  
```  
Add-DAClient -EntrypointName 'Edge1-US' -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_US') -DownlevelGpoName @('corp.contoso.com\DA_W7_Clients_GPO_US)  
```  
  
## <a name="BKMK_EntryPoint"></a>3.7. Adicionar pontos de entrada à implantação multissite  
Depois de habilitar o multissite em sua implantação, você pode adicionar pontos de entrada adicionais usando o Assistente adicionar uma ponto de entrada. Antes de adicionar pontos de entrada, verifique se que você tem as seguintes informações:  
  
-   Endereços IP do balanceador de carga global para cada nova entrada de ponto se você estiver usando o balanceamento de carga global.  
  
-   Os grupos de segurança que contém computadores com cliente Windows 7 para cada ponto de entrada que serão adicionados para habilitar os computadores cliente de acesso remoto para Windows 7.  
  
-   Nomes de objeto de diretiva de grupo se você precisar usar objetos de diretiva de grupo não padrão, são aplicadas em computadores de cliente do Windows 7 para cada ponto de entrada que serão adicionados, se você precisar de suporte para computadores cliente do Windows 7.  
  
-   No caso em que o IPv6 está implantado na rede da organização, você precisará preparar o prefixo IP-HTTPS para o novo ponto de entrada.  
  
### <a name="AddEP"></a>Para adicionar pontos de entrada para sua implantação multissite  
  
1.  No servidor de acesso remoto existente: Sobre o **inicie** tela, digite **RAMgmtUI.exe**, e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **configuração**e, em seguida, o **tarefas** painel, clique em **adicionar um ponto de entrada**.  
  
3.  Em Adicionar um ponto de entrada no assistente, no **detalhes do ponto de entrada** página, na **servidor de acesso remoto**, insira o nome de domínio totalmente qualificado (FQDN) do servidor a ser adicionado. Na **nome do ponto de entrada**, insira o nome do ponto de entrada e, em seguida, clique em **próxima**.  
  
4.  Sobre o **Global configurações balanceamento de carga** página, insira a endereço IP desse ponto de entrada de balanceamento de carga global e, em seguida, clique em **próxima**.  
  
    > [!NOTE]  
    > O **Global configurações balanceamento de carga** página será exibida somente quando a configuração de multissite usa um balanceador de carga global.  
  
5.  Sobre o **topologia de rede** página, clique na topologia que corresponde à topologia de rede do servidor de acesso remoto que você está adicionando e, em seguida, clique em **próxima**.  
  
6.  Sobre o **nome de rede ou o endereço IP** página, na **digite o nome público ou endereço IP usado por clientes para se conectar ao servidor de acesso remoto**, insira o nome público ou endereço IP usado por clientes para se conectar ao Servidor de acesso remoto. O nome público corresponde com o nome da entidade do certificado IP-HTTPS. No caso em que a topologia de rede de borda foi implementada, o endereço IP é que um adaptador externo do servidor de acesso remoto. Clique em **Avançar**.  
  
7.  Sobre o **adaptadores de rede** página, faça o seguinte:  
  
    -   Se você estiver implantando uma topologia com dois adaptadores de rede, no **adaptador externo**, selecione o adaptador está conectado à rede externa. Na **adaptador interno**, selecione o adaptador está conectado à rede interna.  
  
    -   Se você estiver implantando uma topologia com um adaptador de rede, no **adaptador de rede**, selecione o adaptador está conectado à rede interna.  
  
8.  Sobre o **adaptadores de rede** página, na **selecione o certificado usado para autenticar conexões IP-HTTPS**, clique em **procurar** para localizar e selecionar o certificado IP-HTTPS. Clique em **Avançar**.  
  
9. Se o IPv6 está configurado na rede corporativa, nos **configuração de prefixo** página, na **prefixo IPv6 atribuído aos computadores cliente**, insira um prefixo IP-HTTPS para atribuir endereços IPv6 ao cliente do DirectAccess computadores e clique em **próxima**.  
  
10. Sobre o **suporte ao cliente** página, faça o seguinte:  
  
    -   Para limitar o acesso a computadores cliente que executam o Windows 8 ou sistemas operacionais posteriores, clique em **limitar o acesso a computadores cliente que executam o Windows 8 ou um sistema operacional posterior**e, em seguida, clique em **próxima**.  
  
    -   Para permitir que computadores cliente que executam o Windows 7 para acessar esse ponto de entrada, clique em **permitir que os computadores que executam o Windows 7 para acessar esse ponto de entrada cliente**e clique em **Add**. Sobre o **selecionar grupos** caixa de diálogo, selecione os grupos de segurança que contêm os computadores cliente do Windows 7 que irá se conectar a esse ponto de entrada, clique em **Okey**e clique em **próximo**.  
  
11. Sobre o **configurações de GPO do cliente** página, aceite os computadores de cliente do GPO para o Windows 7 padrão para esse ponto de entrada, digite o nome do GPO que você deseja acesso remoto para criar automaticamente, ou clique em **procurar** para localizar os computadores cliente de GPO para o Windows 7 e, em seguida, clique em **próxima**.  
  
    > [!NOTE]  
    > -   O **configurações de GPO do cliente** página será exibida somente quando você configura o ponto de entrada para permitir que o cliente do Windows 7 computadores acessar o ponto de entrada.  
    > -   Você pode, opcionalmente, clique em **GPOs validar** para garantir que você tenha as permissões apropriadas para o GPO selecionado ou GPOs para esse ponto de entrada. Se o GPO não existe e será criado automaticamente, em seguida, cria e vincular as permissões são necessárias. No caso em que os GPOs criados manualmente, em seguida, editar, modificar a segurança e exclui as permissões são necessárias.  
  
12. Sobre o **configurações de GPO do servidor** página, aceite o padrão GPO para este servidor de acesso remoto, digite o nome do GPO que você deseja acesso remoto para criar automaticamente, ou clique em **procurar** para localizar o GPO para Esse servidor e clique **próxima**.  
  
13. Sobre o **servidor de local de rede** , clique em **procurar** para selecionar o certificado para o site de servidor de local de rede em execução no servidor de acesso remoto e, em seguida, clique em **Avançar**.  
  
    > [!NOTE]  
    > O **servidor de local de rede** página será exibida somente quando o site de servidor de local de rede está em execução no servidor de acesso remoto.  
  
14. Sobre o **resumo** página, examine as configurações de ponto de entrada e, em seguida, clique em **confirmar**.  
  
15. Sobre o **adicionando ponto de entrada** caixa de diálogo, clique em **fechar** e, em seguida, em que o Assistente adicionar uma entrada de ponto, clique em **fechar**.  
  
    > [!NOTE]  
    > Se o ponto de entrada que foi adicionado está em uma floresta diferente que os pontos de entrada existentes ou computadores cliente, é necessário clicar **atualizar servidores de gerenciamento** na **tarefas** painel para descobrir o controladores de domínio e o System Center Configuration Manager na nova floresta.  
  
16. Repita esse procedimento da etapa 2 para cada ponto de entrada que você deseja adicionar à implantação multissite.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Para adicionar o computador edge2 do domínio corp2 como um segundo ponto de entrada denominado Edge2 Europa. É a configuração do ponto de entrada: um prefixo IPv6 do cliente ' 2001:db8:2:2000::/ / 64', conexão com o endereço (o certificado IP-HTTPS no computador edge2) 'edge2.contoso.com', um GPO de servidor chamado "DirectAccess Server configurações – Edge2-Europe" e o interno e interfaces externas chamadas Internet e Corpnet2, respectivamente:  
  
```  
Add-DAEntryPoint -RemoteAccessServer 'edge2.corp2.corp.contoso.com' -Name 'Edge2-Europe' -ClientIPv6Prefix '2001:db8:2:2000::/64' -ConnectToAddress 'Europe.contoso.com' -ServerGpoName 'corp2.corp.contoso.com\DirectAccess Server Settings - Edge2-Europe' -InternetInterface 'Internet' -InternalInterface 'Corpnet2'  
```  
  
Para permitir o acesso de computadores cliente Windows 7 por meio do segundo ponto de entrada por meio do grupo de segurança DA_Clients_Europe e usando o GPO DA_W7_Clients_GPO_Europe.  
  
```  
Add-DAClient -EntrypointName 'Edge2-Europe' -DownlevelGpoName @('corp.contoso.com\ DA_W7_Clients_GPO_Europe') -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_Europe')  
```  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 2: Configurar a infraestrutura de multissite](Step-2-Configure-the-Multisite-Infrastructure.md)
