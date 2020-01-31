---
title: Etapa 3 configurar a implantação multissite
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea7ecd52-4c12-4a49-92fd-b8c08cec42a9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3ae66c125548e31603318a7e600c36c00df9d005
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822549"
---
# <a name="step-3-configure-the-multisite-deployment"></a>Etapa 3 configurar a implantação multissite

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Depois de configurar a infraestrutura multissite, siga estas etapas para configurar a implantação multissite de acesso remoto.  
  
|Tarefa|Descrição|  
|----|--------|  
|3.1. Configurar servidores de acesso remoto|Configure servidores de acesso remoto adicionais Configurando endereços IP, unindo-os ao domínio e instalando a função de acesso remoto.|  
|3.2. Conceder acesso de administrador|Conceda privilégios aos servidores de acesso remoto adicionais para o administrador do DirectAccess.|  
|3.3. Configurar IP-HTTPS para uma implantação multissite|Configure o certificado IP-HTTPS usado em uma implantação multissite.|  
|3.4. Configurar o servidor de local de rede para uma implantação multissite|Configure o certificado do servidor de local de rede usado em uma implantação multissite.|  
|3.5. Configurar clientes do DirectAccess para uma implantação multissite|Remova computadores cliente do Windows 7 de grupos de segurança do Windows 8.|  
|3.6. Habilitar a implantação multissite|Habilite a implantação multissite no primeiro servidor de acesso remoto.|  
|3,7. Adicionar pontos de entrada à implantação multissite|Adicione outros pontos de entrada à implantação multissite.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigServer"></a>3.1. Configurar servidores de acesso remoto  

  
### <a name="to-install-the-remote-access-role"></a>Para instalar a função Acesso Remoto.  
  
1.  Certifique-se de que cada servidor de acesso remoto esteja configurado com a topologia de implantação apropriada (borda, por trás de um NAT, interface de rede única) e rotas correspondentes.  
  
2.  Configure os endereços IP em cada servidor de acesso remoto de acordo com a topologia do site e o esquema de endereçamento IP da sua organização.  
  
3.  Ingresse cada servidor de acesso remoto em um domínio Active Directory.  
  
4.  No console do Gerenciador do Servidor, no **painel**, clique em **adicionar funções e recursos**.  
  
5.   Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.  
  
6.  Na caixa de diálogo **selecionar funções de servidor** , selecione **acesso remoto**e clique em **Avançar**.  
  
7.  Clique em **Avançar** três vezes.  
  
8.  Na caixa de diálogo **selecionar serviços de função** , selecione **DirectAccess e VPN (RAS)** e clique em **Adicionar recursos**.  
  
9.  Selecione **Roteamento**, selecione **proxy de aplicativo Web**, clique em **Adicionar recursos**e, em seguida, clique em **Avançar**.  
  
10. Clique em **Avançar**e, em seguida, clique em **Instalar**.  
  
11.  Na caixa de diálogo **Progresso da instalação** , verifique se a instalação foi bem-sucedida e clique em **Fechar**.  
  
  
![](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  

  
As etapas 1-3 devem ser executadas manualmente e não são realizadas usando este cmdlet do Windows PowerShell.  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Admin"></a>3.2. Conceder acesso de administrador  
  
#### <a name="to-grant-administrator-permissions"></a>Para conceder permissões de administrador  
  
1.  No servidor de acesso remoto no ponto de entrada adicional: na tela **Iniciar** , digite **Gerenciamento do computador**e pressione Enter.  
  
2.  No painel esquerdo, clique em **usuários e grupos locais**.  
  
3.  Clique duas vezes em **grupos**e clique duas vezes em **Administradores**.  
  
4.  Na caixa de diálogo **Propriedades de administradores** , clique em **Adicionar**e, na caixa de diálogo **Selecionar usuários, computadores, contas de serviço ou grupos** , clique em **locais**.  
  
5.  Na caixa de diálogo **locais** , na árvore **local** , clique no local que contém a conta de usuário do administrador do DirectAccess e clique em **OK**.  
  
6.  Em **Inserir os nomes de objeto a serem selecionados**, insira o nome de usuário do administrador do DirectAccess e clique em **OK** duas vezes.  
  
7.  Na caixa de diálogo **Propriedades de administradores** , clique em **OK**.  
  
8.  Feche a janela Gerenciamento do computador.  
  
9. Repita esse procedimento em todos os servidores de acesso remoto que serão parte da implantação multissite.  
  
## <a name="BKMK_IPHTTPS"></a>3.3. Configurar IP-HTTPS para uma implantação multissite  
Em cada servidor de acesso remoto que será adicionado à implantação multissite, um certificado SSL será necessário para verificar a conexão HTTPS com o servidor Web IP-HTTPS. A associação no grupo local **Administradores**, ou equivalente, é o mínimo necessário para concluir esse procedimento.  
  
#### <a name="to-obtain-an-ip-https-certificate"></a>Para obter um certificado IP-HTTPS  
  
1.  Em cada servidor de acesso remoto: na tela **Iniciar** , digite **MMC**e pressione Enter. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  Clique em **Arquivo** e em **Adicionar/Remover Snap-ins**.  
  
3.  Clique em **Certificados**, **Adicionar**, **Conta de computador** e em **Avançar**, selecione **Computador local**, clique em **Concluir** e em **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Na página **solicitar certificados** , clique no modelo de certificado do servidor Web e, em seguida, clique em **mais informações são necessárias para se registrar nesse certificado**.  
  
    Se o modelo de certificado do servidor Web não for exibido, verifique se a conta de computador do servidor de acesso remoto tem permissões de registro para o modelo de certificado do servidor Web. Para obter mais informações, consulte [configurar permissões no modelo de certificado do servidor Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  Na guia **assunto** da caixa de **diálogo Propriedades do certificado** , em **nome da entidade**, para **tipo**, selecione **nome comum**.  
  
9. Em **valor**, digite o nome de domínio totalmente qualificado (FQDN) do nome da Internet do servidor de acesso remoto (por exemplo, Europe.contoso.com) e clique em **Adicionar**.  
  
10. Clique em **OK**, **Registrar** e em **Concluir**.  
  
11. No painel de detalhes do snap-in Certificados, confirme se um novo certificado com o FQDN foi registrado com **Finalidades** de **Autenticação do Servidor**.  
  
12. Clique com o botão direito do mouse no certificado e, em seguida, clique em **Propriedades**.  
  
13. Em **Nome Amigável**, digite **Certificado IP-HTTPS** e clique em **OK**.  
  
    > [!TIP]  
    > As etapas 12 e 13 são opcionais, mas facilitam a seleção do certificado para IP-HTTPS ao configurar o acesso remoto.  
  
14. Repita esse procedimento em todos os servidores de acesso remoto em sua implantação.  
  
## <a name="BKMK_NLS"></a>3,4. Configurar o servidor de local de rede para uma implantação multissite  
Se você tiver selecionado configurar o site do servidor de local de rede no servidor de acesso remoto ao configurar seu primeiro servidor, cada novo servidor de acesso remoto que você adicionar precisará ser configurado com um certificado de servidor Web que tenha o mesmo nome de entidade que foi selecionado para t o servidor de local de rede do primeiro servidor. Cada servidor requer um certificado para autenticar a conexão com o servidor de local de rede, e os computadores cliente localizados na rede interna devem ser capazes de resolver o nome do site no DNS.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Para instalar um certificado para local de rede  
  
1.  No servidor de acesso remoto: na tela **Iniciar** , digite **MMC**e pressione Enter. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  Clique em **Arquivo** e em **Adicionar/Remover Snap-ins**.  
  
3.  Clique em **Certificados**, **Adicionar**, **Conta de computador** e em **Avançar**, selecione **Computador local**, clique em **Concluir** e em **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
    > [!NOTE]  
    > Você também pode importar o mesmo certificado que foi usado para o servidor de local de rede para o primeiro servidor de acesso remoto.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Na página **solicitar certificados** , clique no modelo de certificado do servidor Web e, em seguida, clique em **mais informações são necessárias para se registrar nesse certificado**.  
  
    Se o modelo de certificado do servidor Web não for exibido, verifique se a conta de computador do servidor de acesso remoto tem permissões de registro para o modelo de certificado do servidor Web. Para obter mais informações, consulte [configurar permissões no modelo de certificado do servidor Web](https://technet.microsoft.com/library/ee649249(v=ws.10).aspx).  
  
8.  Na guia **assunto** da caixa de **diálogo Propriedades do certificado** , em **nome da entidade**, para **tipo**, selecione **nome comum**.  
  
9. Em **valor**, digite o nome de domínio totalmente qualificado (FQDN) que foi configurado para o certificado do servidor de local de rede do primeiro servidor de acesso remoto (por exemplo, NLS.Corp.contoso.com) e clique em **Adicionar**.  
  
10. Clique em **OK**, **Registrar** e em **Concluir**.  
  
11. No painel de detalhes do snap-in Certificados, confirme se um novo certificado com o FQDN foi registrado com **Finalidades** de **Autenticação do Servidor**.  
  
12. Clique com o botão direito do mouse no certificado e, em seguida, clique em **Propriedades**.  
  
13. Em **Nome Amigável**, digite **Certificado de Local de Rede** e clique em **OK**.  
  
    > [!TIP]  
    > As etapas 12 e 13 são opcionais, mas facilitam a seleção do certificado para o local de rede ao configurar o acesso remoto.  
  
14. Repita esse procedimento em todos os servidores de acesso remoto em sua implantação.  
  
### <a name="NLS"></a>Para criar os registros DNS do servidor do local de rede  
  
1.  No servidor DNS: na tela **Iniciar** , digite **DNSMGMT. msc**e pressione Enter.  
  
2.  No painel esquerdo do console do **Gerenciador de DNS** , abra a zona de pesquisa direta para a rede interna. Clique com o botão direito do mouse na zona relevante e clique em **novo host (A ou aaaa)** .  
  
3.  Na caixa de diálogo **novo host** , na caixa **nome (usa o nome de domínio pai se estiver em branco)** , insira o nome que foi usado para o servidor de local de rede para o primeiro servidor de acesso remoto. Na caixa **endereço IP** , digite o endereço IPv4 voltado para a intranet do servidor de acesso remoto e clique em **Adicionar host**. Na caixa de diálogo **DNS**, clique em **OK**.  
  
4.  Na caixa de diálogo **novo host** , na caixa **nome (usa o nome de domínio pai se estiver em branco)** , insira o nome que foi usado para o servidor de local de rede para o primeiro servidor de acesso remoto. Na caixa **endereço IP** , digite o endereço IPv6 voltado para a intranet do servidor de acesso remoto e clique em **Adicionar host**. Na caixa de diálogo **DNS**, clique em **OK**.  
  
5.  Repita as etapas 3 e 4 para cada servidor de acesso remoto em sua implantação.  
  
6.  Clique em **Concluído**.  
  
7.  Repita esse procedimento antes de adicionar servidores como pontos de entrada adicionais na implantação.  
  
## <a name="BKMK_Client"></a>3,5. Configurar clientes do DirectAccess para uma implantação multissite  
Os computadores cliente Windows do DirectAccess devem ser membros de grupos de segurança que definem sua associação do DirectAccess. Antes que o multissite esteja habilitado, esses grupos de segurança podem conter clientes do Windows 8 e clientes do Windows 7 (se o modo "anterior" apropriado foi selecionado). Quando o multissite estiver habilitado, os grupos de segurança do cliente existentes, no modo de servidor único, serão convertidos em grupos de segurança somente para o Windows 8. Após a habilitação de multissite, os computadores cliente do Windows 7 do DirectAccess devem ser movidos para os grupos de segurança de cliente do Windows 7 dedicados correspondentes (que estão associados a pontos de entrada específicos) ou não poderão se conectar por meio do DirectAccess. Os clientes do Windows 7 devem primeiro ser removidos dos grupos de segurança existentes, que agora são grupos de segurança do Windows 8. Cuidado: os computadores cliente do Windows 7 que são membros de grupos de segurança de clientes do Windows 7 e do Windows 8 perderão a conectividade remota, e os clientes do Windows 7 sem o SP1 instalado perderão também a conectividade corporativa. Portanto, todos os computadores cliente do Windows 7 devem ser removidos dos grupos de segurança do Windows 8.  
  
#### <a name="remove--windows-7--clients-from-windows-8-security-groups"></a>Remover clientes do Windows 7 de grupos de segurança do Windows 8  
  
1.  No controlador de domínio primário, clique em **Iniciar**e em **Active Directory usuários e computadores**.  
  
2.  Para remover computadores do grupo de segurança, clique duas vezes no grupo de segurança e, na caixa de diálogo **Propriedades do < Group_Name >** , clique na guia **Membros** .  
  
3.  Selecione o computador cliente do Windows 7 e clique em **remover**.  
  
4.  Repita este procedimento para remover os computadores cliente do Windows 7 dos grupos de segurança do Windows 8.  
  
> [!IMPORTANT]  
> Ao habilitar uma configuração de multissite de acesso remoto, todos os computadores cliente (Windows 7 e Windows 8) perderão a conectividade remota até que possam se conectar à rede corporativa diretamente ou por VPN para atualizar suas políticas de grupo. Isso é verdadeiro ao habilitar a funcionalidade multissite pela primeira vez e também ao desabilitar multissite.  
  
## <a name="BKMK_Enable"></a>3,6. Habilitar a implantação multissite  
Para configurar uma implantação multissite, habilite o recurso multissite em seu servidor de acesso remoto existente. Antes de habilitar o multissite em sua implantação, verifique se você tem as seguintes informações:  
  
1.  Configurações de balanceador de carga global e endereços IP se você quiser balancear a carga de conexões de cliente do DirectAccess em todos os pontos de entrada em sua implantação.  
  
2.  Os grupos de segurança que contêm computadores cliente do Windows 7 para o primeiro ponto de entrada em sua implantação, se você quiser habilitar o acesso remoto para computadores cliente com Windows 7.  
  
3.  Política de Grupo nomes de objeto, se for necessário usar objetos de Política de Grupo não padrão, que são aplicados nos computadores cliente do Windows 7 para o primeiro ponto de entrada em sua implantação, se você precisar de suporte para computadores cliente com Windows 7.  
  
### <a name="EnabledMultisite"></a>Para habilitar uma configuração multissite  
  
1.  Em seu servidor de acesso remoto existente: na tela **Iniciar** , digite **RAMgmtUI. exe**e pressione Enter. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No console de gerenciamento de acesso remoto, clique em **configuração**e, no painel **tarefas** , clique em **habilitar multissite**.  
  
3.  No Assistente para **habilitar implantação multissite** , na página **antes de começar** , clique em **Avançar**.  
  
4.  Na página **nome da implantação** , em **nome da implantação multissite**, insira um nome para a implantação. Em **nome do primeiro ponto de entrada**, insira um nome para identificar o primeiro ponto de entrada que é o servidor de acesso remoto atual e clique em **Avançar**.  
  
5.  Na página **seleção de ponto de entrada** , siga um destes procedimentos:  
  
    -   Clique em **atribuir pontos de entrada automaticamente e permita que os clientes selecionem manualmente** para rotear automaticamente os computadores cliente para o ponto de entrada mais adequado, permitindo também que os computadores cliente selecionem um ponto de entrada manualmente. A seleção de ponto de entrada manual está disponível somente para computadores com Windows 8. Clique em **Avançar**.  
  
    -   Clique em **atribuir pontos de entrada automaticamente** para rotear automaticamente os computadores cliente para o ponto de entrada mais adequado e clique em **Avançar**.  
  
6.  Na página **balanceamento de carga global** , siga um destes procedimentos:  
  
    -   Clique em **não, não use balanceamento de carga global** se não quiser usar um balanceamento de carga global e clique em **Avançar**.  
  
        > [!NOTE]  
        > Ao selecionar essa opção, os computadores cliente se conectam ao ponto de entrada mais próximo automaticamente.  
  
    -   Clique em **Sim, use balanceamento de carga global** se desejar balancear a carga do tráfego globalmente entre todos os pontos de entrada. Em **digite o FQDN de balanceamento de carga global a ser usado por todos os pontos de entrada**, insira o FQDN de balanceamento de carga global e, em **digite o endereço IP do balanceamento de carga global para esse ponto de entrada** que contenha o primeiro servidor de acesso remoto, insira o endereço IP do balanceamento de carga global para esse ponto de entrada e clique em **Avançar**.  
  
7.  Na página **suporte ao cliente** , siga um destes procedimentos:  
  
    -   Para limitar o acesso a computadores cliente que executam o Windows 8 ou sistemas operacionais posteriores, clique em **limitar o acesso a computadores cliente que executam o Windows 8 ou um sistema operacional posterior**e clique em **Avançar**.  
  
    -   Para permitir que computadores cliente que executam o Windows 7 acessem esse ponto de entrada, clique em **permitir que computadores cliente que executam o Windows 7 acessem esse ponto de entrada**e clique em **Adicionar**. Na caixa de diálogo **Selecionar grupos** , selecione os grupos de segurança que contêm os computadores cliente do Windows 7, clique em **OK**e em **Avançar**.  
  
8.  Na página **configurações de GPO do cliente** , aceite o GPO padrão para computadores cliente do Windows 7 para esse ponto de entrada, digite o nome do GPO que deseja que o acesso remoto crie automaticamente ou clique em **procurar** para localizar o GPO para computadores cliente com Windows 7 e clique em **Avançar**.  
  
    > [!NOTE]  
    > -   A página **configurações de GPO do cliente** é exibida somente quando você configura o ponto de entrada para permitir que computadores cliente do Windows 7 acessem o ponto de entrada.  
    > -   Opcionalmente, você pode clicar em **validar GPOs** para garantir que você tenha as permissões apropriadas para o GPO ou GPOs selecionados para esse ponto de entrada. Se o GPO não existir e será criado automaticamente, as permissões CREATE e link serão necessárias. No caso em que os GPOs foram criados manualmente, as permissões editar, modificar segurança e excluir são necessárias.  
  
9. Na página **Resumo** , clique em **confirmar**.  
  
10. Na caixa de diálogo **habilitando a implantação** multissite, clique em **fechar** e, em seguida, no assistente habilitar implantação multissite, clique em **Fechar**.  
  
![](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Para habilitar uma implantação multissite chamada "contoso" no primeiro ponto de entrada chamado "Edge1-US". A implantação permite que os clientes selecionem manualmente o ponto de entrada e não use um balanceador de carga global.  
  
```  
Enable-DAMultiSite -Name 'Contoso' -EntryPointName 'Edge1-US' -ManualEntryPointSelectionAllowed 'Enabled'  
```  
  
Para permitir que computadores cliente com Windows 7 acessem por meio do primeiro ponto de entrada por meio do grupo de segurança DA_Clients_US e usando o GPO DA_W7_Clients_GPO_US.  
  
```  
Add-DAClient -EntrypointName 'Edge1-US' -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_US') -DownlevelGpoName @('corp.contoso.com\DA_W7_Clients_GPO_US)  
```  
  
## <a name="BKMK_EntryPoint"></a>3,7. Adicionar pontos de entrada à implantação multissite  
Depois de habilitar o multissite em sua implantação, você pode adicionar outros pontos de entrada usando o assistente Adicionar um ponto de entrada. Antes de adicionar pontos de entrada, verifique se você tem as seguintes informações:  
  
-   Endereços IP de balanceador de carga global para cada novo ponto de entrada se você estiver usando balanceamento de carga global.  
  
-   Os grupos de segurança que contêm computadores cliente do Windows 7 para cada ponto de entrada que será adicionado se você quiser habilitar o acesso remoto para computadores cliente do Windows 7.  
  
-   Política de Grupo nomes de objeto, se for necessário usar objetos de Política de Grupo não padrão, que são aplicados em computadores cliente com Windows 7 para cada ponto de entrada que será adicionado, se você precisar de suporte para computadores cliente com Windows 7.  
  
-   No caso em que o IPv6 é implantado na rede da organização, você precisará preparar o prefixo IP-HTTPS para o novo ponto de entrada.  
  
### <a name="AddEP"></a>Para adicionar pontos de entrada à implantação multissite  
  
1.  Em seu servidor de acesso remoto existente: na tela **Iniciar** , digite **RAMgmtUI. exe**e pressione Enter. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No console de gerenciamento de acesso remoto, clique em **configuração**e, no painel **tarefas** , clique em **Adicionar um ponto de entrada**.  
  
3.  No assistente Adicionar um ponto de entrada, na página **detalhes do ponto de entrada** , em **servidor de acesso remoto**, insira o nome de domínio totalmente qualificado (FQDN) do servidor a ser adicionado. Em **nome do ponto de entrada**, digite o nome do ponto de entrada e clique em **Avançar**.  
  
4.  Na página **configurações de balanceamento de carga global** , insira o endereço IP do balanceamento de carga global desse ponto de entrada e clique em **Avançar**.  
  
    > [!NOTE]  
    > A página **configurações globais de balanceamento de carga** aparece somente quando a configuração multissite usa um balanceador de carga global.  
  
5.  Na página **topologia de rede** , clique na topologia que corresponde à topologia de rede do servidor de acesso remoto que você está adicionando e clique em **Avançar**.  
  
6.  Na página **nome da rede ou endereço IP** , em **digite o nome público ou o endereço IP usado pelos clientes para se conectar ao servidor de acesso remoto**, insira o nome público ou o endereço IP usado pelos clientes para se conectar ao servidor de acesso remoto. O nome público corresponde ao nome da entidade do certificado IP-HTTPS. No caso em que a topologia de rede de borda foi implementada, o endereço IP é o do adaptador externo do servidor de acesso remoto. Clique em **Avançar**.  
  
7.  Na página **adaptadores de rede** , siga um destes procedimentos:  
  
    -   Se você estiver implantando uma topologia com dois adaptadores de rede, em **adaptador externo**, selecione o adaptador que está conectado à rede externa. Em **adaptador interno**, selecione o adaptador que está conectado à rede interna.  
  
    -   Se você estiver implantando uma topologia com um adaptador de rede, em **adaptador de rede**, selecione o adaptador que está conectado à rede interna.  
  
8.  Na página **adaptadores de rede** , em **selecionar o certificado usado para autenticar conexões IP-HTTPS**, clique em **procurar** para localizar e selecionar o certificado IP-HTTPS. Clique em **Avançar**.  
  
9. Se o IPv6 estiver configurado na rede corporativa, na página **configuração de prefixo** , em **prefixo IPv6 atribuído a computadores cliente**, insira um prefixo IP-HTTPS para atribuir endereços IPv6 aos computadores cliente DirectAccess e clique em **Avançar**.  
  
10. Na página **suporte ao cliente** , siga um destes procedimentos:  
  
    -   Para limitar o acesso a computadores cliente que executam o Windows 8 ou sistemas operacionais posteriores, clique em **limitar o acesso a computadores cliente que executam o Windows 8 ou um sistema operacional posterior**e clique em **Avançar**.  
  
    -   Para permitir que computadores cliente que executam o Windows 7 acessem esse ponto de entrada, clique em **permitir que computadores cliente que executam o Windows 7 acessem esse ponto de entrada**e clique em **Adicionar**. Na caixa de diálogo **Selecionar grupos** , selecione os grupos de segurança que contêm os computadores cliente do Windows 7 que se conectarão a esse ponto de entrada, clique em **OK**e clique em **Avançar**.  
  
11. Na página **configurações de GPO do cliente** , aceite o GPO padrão para computadores cliente do Windows 7 para esse ponto de entrada, digite o nome do GPO que você deseja que o acesso remoto crie automaticamente ou clique em **procurar** para localizar o GPO para computadores cliente do Windows 7 e clique em **Avançar**.  
  
    > [!NOTE]  
    > -   A página **configurações de GPO do cliente** é exibida somente quando você configura o ponto de entrada para permitir que computadores cliente do Windows 7 acessem o ponto de entrada.  
    > -   Opcionalmente, você pode clicar em **validar GPOs** para garantir que você tenha as permissões apropriadas para o GPO ou GPOs selecionados para esse ponto de entrada. Se o GPO não existir e será criado automaticamente, as permissões CREATE e link serão necessárias. No caso em que os GPOs foram criados manualmente, as permissões editar, modificar segurança e excluir são necessárias.  
  
12. Na página **configurações do GPO do servidor** , aceite o GPO padrão para esse servidor de acesso remoto, digite o nome do GPO que você deseja que o acesso remoto crie automaticamente ou clique em **procurar** para localizar o GPO para esse servidor e clique em **Avançar**.  
  
13. Na página **servidor de local de rede** , clique em **procurar** para selecionar o certificado para o site do servidor do local de rede em execução no servidor de acesso remoto e clique em **Avançar**.  
  
    > [!NOTE]  
    > A página **servidor de local de rede** aparece somente quando o site do servidor do local de rede está em execução no servidor de acesso remoto.  
  
14. Na página **Resumo** , examine as configurações de ponto de entrada e clique em **confirmar**.  
  
15. Na caixa de diálogo **Adicionar ponto de entrada** , clique em **fechar** e, em seguida, no assistente Adicionar um ponto de entrada, clique em **Fechar**.  
  
    > [!NOTE]  
    > Se o ponto de entrada adicionado estiver em uma floresta diferente dos pontos de entrada ou computadores cliente existentes, será necessário clicar em **Atualizar servidores de gerenciamento** no painel **tarefas** para descobrir os controladores de domínio e Configuration Manager na nova floresta.  
  
16. Repita esse procedimento da etapa 2 para cada ponto de entrada que você deseja adicionar à implantação multissite.  
  
![](../../../../media/Step-3-Configure-the-Multisite-Deployment/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Para adicionar o computador edge2 do domínio corp2 como um segundo ponto de entrada chamado Edge2-Europa. A configuração de ponto de entrada é: um prefixo IPv6 de cliente ' 2001: DB8:2: 2000::/64 ', um endereço de conexão (o certificado IP-HTTPS no computador edge2) ' edge2.contoso.com ', um GPO de servidor denominado "configurações do servidor DirectAccess-Edge2-Europa" e as interfaces internas e externas chamadas Internet e Corpnet2, respectivamente:  
  
```  
Add-DAEntryPoint -RemoteAccessServer 'edge2.corp2.corp.contoso.com' -Name 'Edge2-Europe' -ClientIPv6Prefix '2001:db8:2:2000::/64' -ConnectToAddress 'Europe.contoso.com' -ServerGpoName 'corp2.corp.contoso.com\DirectAccess Server Settings - Edge2-Europe' -InternetInterface 'Internet' -InternalInterface 'Corpnet2'  
```  
  
Para permitir que computadores cliente com Windows 7 acessem por meio do segundo ponto de entrada por meio do grupo de segurança DA_Clients_Europe e usando o DA_W7_Clients_GPO_Europe de GPO.  
  
```  
Add-DAClient -EntrypointName 'Edge2-Europe' -DownlevelGpoName @('corp.contoso.com\ DA_W7_Clients_GPO_Europe') -DownlevelSecurityGroupNameList @('corp.contoso.com\DA_Clients_Europe')  
```  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 2: configurar a infraestrutura multissite](Step-2-Configure-the-Multisite-Infrastructure.md)
