---
title: Etapa 3 configurar um Cluster de balanceamento de carga
description: Este tópico faz parte do guia de implantação de acesso remoto em um Cluster no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f000066e-7cf8-4085-82a3-4f4fe1cb3c5c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f835e27a80e661ff1f066af4779bd7c033cddc99
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446625"
---
# <a name="step-3-configure-a-load-balanced-cluster"></a>Etapa 3 configurar um Cluster de balanceamento de carga

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Depois de preparar servidores para o cluster, configurar o balanceamento de carga em um único servidor, configurar os certificados necessários e implantar o cluster.  
  
|Tarefa|Descrição|  
|----|--------|  
|[3.1 definir o prefixo IPv6](#BKMK_Prefix)|Se o ambiente corporativo for IPv4 + IPv6, ou somente IPv6, em seguida, no servidor de acesso remoto único, certifique-se de que o prefixo de IPv6 atribuído a computadores cliente DirectAccess é grande o suficiente para cobrir todos os servidores no cluster.|  
|[3.2 habilitar balanceamento de carga](#BKMK_NLB)|Habilite o balanceamento de carga em um único servidor de acesso remoto.|  
|[3.3 instalar o certificado IP-HTTPS](#BKMK_InstallIPHTTP)|Cada servidor no cluster requer um certificado de servidor para autenticar a conexão IP-HTTPS.  Exportar o certificado IP-HTTPS do servidor de acesso remoto único e implantá-lo em cada servidor que você irá adicionar ao cluster. Isso é necessário somente se o uso de certificados não autoassinados.|  
|[3.4 instalar o certificado de servidor de local de rede](#BKMK_NLS)|Se o servidor único tem o servidor de local de rede implantado localmente, você precisará implantar o certificado de servidor de local de rede em cada servidor no cluster. Se o servidor de local de rede estiver hospedado em um servidor externo, não é necessário um certificado em cada servidor. Isso é necessário somente se o uso de certificados não autoassinados.|  
|[3.5 adicionar servidores ao cluster](#BKMK_Add)|Adicione todos os servidores no cluster. Acesso remoto não deve ser configurado nos servidores a serem adicionados.|  
|[3.6 remover um servidor de cluster](#BKMK_remove)|Instruções para remover um servidor do cluster.|  
|[3.7 desabilitar balanceamento de carga](#BKBK_disable)|Instruções para desabilitar o balanceamento de carga.|  
  
> [!NOTE]  
> O endereço IP que está selecionado para o DIP não deve estar em uso nos adaptadores de rede do servidor de acesso remoto primeiro no cluster. Começar a implantação do DirectAccess com o VIP e DIP adicionado ao adaptador de rede resultará em falha.  
  
> [!NOTE]  
> Lembre-se de usar um DIP que já está presente em outro computador na rede.  
  
## <a name="BKMK_Prefix"></a>3.1 definir o prefixo IPv6  
  
### <a name="configDA"></a>Para configurar o prefixo  
  
1.  No servidor de acesso remoto, clique em **inicie**e, em seguida, clique em **gerenciamento de acesso remoto**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **Configuração**.  
  
3.  No painel central do console, nos **etapa 2: servidor DirectAccess** área, clique em **editar**.  
  
4.  Clique em **configuração de prefixo**. Sobre o **configuração de prefixo** página, na **prefixo IPv6 atribuído a computadores cliente DirectAccess**, inserir o prefixo IPv6 usado para os computadores cliente DirectAccess com um comprimento de subrede de 59, por exemplo, **2001:db8:1:1000::/ / 59**. Se VPN também foram habilitado com o IPv6, em seguida, um prefixo IPv6 seria exibido e o tamanho de sub-rede precisaria ser alterada a 59. Clique em **Avançar**.  
  
5.  No painel central do console, clique em **concluir**.  
  
6.  Sobre o **análise de acesso remoto** caixa de diálogo, examine as configurações e, em seguida, clique em **aplicar**. Na caixa de diálogo **Aplicando Configurações do Assistente de Configuração de Acesso Remoto** , clique em **Fechar**.  
  
## <a name="BKMK_NLB"></a>3.2 habilitar balanceamento de carga  
  
#### <a name="to-enable-load-balancing"></a>Para habilitar o balanceamento de carga  
  
1.  No servidor do DirectAccess configurado, clique em **inicie**e, em seguida, clique em **gerenciamento de acesso remoto**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No console de gerenciamento de acesso remoto, no painel esquerdo, clique em **Configuration**e, em seguida, no **tarefas** painel, clique em **habilitar balanceamento de carga**.  
  
3.  No habilitar à Assistente de balanceamento de carga, clique em **próxima**.  
  
4.  Dependendo do que você escolheu etapas de planejamento:  
  
    1.  Windows NLB: Sobre o **o método de balanceamento de carga** , clique em **Use Windows Network NLB Balanceamento de carga ()** e, em seguida, clique em **próxima**.  
  
    2.  Balanceador externo de carga: Sobre o **método de balanceamento de carga** , clique em **usar um balanceador de carga externo**e, em seguida, clique em **próxima**.  
  
5.  Em uma implantação de adaptador de rede único, sobre o **endereços de IP dedicado** página, faça o seguinte e, em seguida, clique em **próxima**:  
  
    1.  No **endereço IPv4** caixa, digite o novo endereço de IPv4 para este servidor de acesso remoto; o será de endereço IPv4 atual ser o endereço IP virtual (VIP) do cluster com balanceamento de carga. No **máscara de sub-rede** , digite a máscara de sub-rede.  
  
    2.  Se o ambiente corporativo é IPv6 nativo, em seguida, nos **endereço IPv6** caixa, digite o novo endereço de IPv6 para este servidor de acesso remoto; a atual IPv6 endereço será o VIP do cluster com balanceamento de carga. No **comprimento do prefixo de sub-rede** , digite o comprimento do prefixo de sub-rede.  
  
6.  Em um dois implantação de adaptador de rede a **endereços IP externos de dedicado** página, faça o seguinte e, em seguida, clique em **próxima**:  
  
    1.  No **endereço IPv4** caixa, digite o novo endereço IPv4 externo para este servidor de acesso remoto; o será de endereço IPv4 atual ser o endereço IP virtual (VIP) do balanceamento de carga do cluster. No **máscara de sub-rede** , digite a máscara de sub-rede.  
  
    2.  Se existem atualmente nativos endereços IPv6 configurados no adaptador de rede para a internet do servidor de acesso remoto, nos **endereço IPv6** , digite o novo endereço de IPv6 externo para esse acesso remoto do servidor; o IPv6 atual endereço será o VIP de cluster de balanceamento de carga. No **comprimento do prefixo de sub-rede** , digite o comprimento do prefixo de sub-rede.  
  
7.  Em um dois implantação de adaptador de rede a **endereços IP internos de dedicado** página, faça o seguinte e, em seguida, clique em **próxima**:  
  
    1.  No **endereço IPv4** caixa, digite o novo endereço de IPv4 interno para este servidor de acesso remoto; a atual IPv4 endereço será o VIP do balanceamento de carga do cluster. No **máscara de sub-rede** , digite a máscara de sub-rede.  
  
    2.  Se o ambiente corporativo é IPv6 nativo, em seguida, nos **endereço IPv6** caixa, digite o novo endereço IPv6 interno para este servidor de acesso remoto; a atual IPv6 endereço será o VIP do balanceamento de carga do cluster. No **comprimento do prefixo de sub-rede** , digite o comprimento do prefixo de sub-rede.  
  
8.  Sobre o **resumo** , clique em **confirmar**.  
  
9. Sobre o **habilitar balanceamento de carga** caixa de diálogo, clique em **fechar**.  
  
10. No habilitar à Assistente de balanceamento de carga, clique em **fechar**.  
  
    > [!NOTE]  
    > Se o balanceamento de carga externo está sendo usado, observe a IPs virtuais e fornecê-los como em balanceadores de carga externo.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Se você optar por usar o NLB do Windows nas etapas de planejamento, em seguida, execute o seguinte:  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -InternetVirtualIPAddress @("2.1.1.1/255.255.255.0","2.1.1.2/255.255.255.0") -InternalVirtualIPAddress @("10.1.1.2/255.255.255.0","3ffe::2/64")  
```  
  
Se você optar por usar um balanceador externo de carga em etapas de planejamento:, em seguida, execute o seguinte:  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -UseThirdPrtyLoadBalancer  
```  
  
> [!NOTE]  
> É recomendável não incluir alterações nas configurações do balanceador de carga com as alterações para outras configurações, se você estiver usando GPOs de preparo. Qualquer alteração nas configurações do balanceador de carga deve ser aplicada primeiro e, em seguida, outras alterações de configuração devem ser feitas. Além disso, depois de configurar o balanceador de carga em um novo servidor do DirectAccess, aguarde alguns instantes para que as alterações IP a ser aplicada e replicados entre os servidores DNS da empresa, antes de alterar outras configurações de DirectAccess relacionadas para o novo cluster.  
  
## <a name="BKMK_InstallIPHTTP"></a>3.3 instalar o certificado IP-HTTPS  
A associação no grupo local **Administradores**, ou equivalente, é o mínimo necessário para concluir esse procedimento.  
  
### <a name="IPHTTPSCert"></a>Para instalar o certificado IP-HTTPS  
  
1.  O servidor de acesso remoto configurado, clique em **inicie**, digite **mmc** e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
3.  Sobre o **adicionar ou Remover Snap-ins** caixa de diálogo, clique em **certificados**, clique em **adicionar**, clique em **conta de computador**, clique em  **Próxima**, clique em **término**e, em seguida, clique em **Okey**.  
  
4.  No painel esquerdo do console, navegue até **certificados (computador Local) \Personal\Certificates**. O certificado IP-HTTPS com o botão direito, aponte para **todas as tarefas** e clique em **exportar**.  
  
5.  Na página **Bem-vindo ao Assistente para Exportação de Certificados**, clique em **Avançar**.  
  
6.  Na página **Exportar Chave Privada** , clique em **Sim, exportar a chave privada**e em **Avançar**.  
  
7.  Sobre o **Exportar formato de arquivo** , clique em **troca de informações pessoais - PKCS #12 (. PFX)** e, em seguida, clique em **próxima**.  
  
8.  Sobre o **segurança** página, selecione o **senha** caixa de seleção, insira uma senha no **senha** caixa e confirme a senha e, em seguida, clique em **Avançar**.  
  
9. Sobre o **arquivo a ser exportado** página, insira um nome para o arquivo de certificado e salve-o na área de trabalho e, em seguida, clique em **próxima**.  
  
10. Na página **Concluindo o Assistente para Exportação de Certificados**, clique em **Concluir**.  
  
11. Sobre o **Assistente para exportação de certificados** caixa de diálogo, clique em **Okey**.  
  
12. Copie o certificado para todos os servidores que você deseja ser membros do cluster.  
  
13. No novo servidor do DirectAccess, clique em **inicie**, digite **mmc** e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
14. No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
15. Sobre o **adicionar ou Remover Snap-ins** caixa de diálogo, clique em **certificados**, clique em **adicionar**, clique em **conta de computador**, clique em  **Próxima**, clique em **término**e, em seguida, clique em **Okey**.  
  
16. No painel esquerdo do console, navegue até **certificados (computador Local) \Personal\Certificates**. Clique com botão direito do **certificados** nó, aponte para **todas as tarefas**e, em seguida, clique em **importação**.  
  
17. Na página **Bem-vindo ao Assistente para Importação de Certificados**, clique em **Avançar**.  
  
18. Sobre o **arquivo a ser importado** , clique em **procurar** para localizar o certificado. Selecione o certificado e, em seguida, clique em **próxima**.  
  
19. No **proteção de chave privada** página, o **senha** caixa, digite a senha e, em seguida, clique em **próxima**.  
  
20. Na página **Armazenamento de Certificados**, clique em **Avançar**.  
  
21. Na página **Concluindo o Assistente para Importação de Certificados**, clique em **Concluir**.  
  
22. Sobre o **Assistente para importação de certificados** caixa de diálogo, clique em **Okey**.  
  
23. Repita as etapas 13 a 22 em todos os servidores que você deseja ser membros do cluster.  
  
## <a name="BKMK_NLS"></a>3.4 instalar o certificado de servidor de local de rede  
A associação no grupo local **Administradores**, ou equivalente, é o mínimo necessário para concluir esse procedimento.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Para instalar um certificado para local de rede  
  
1.  No servidor de acesso remoto, clique em **inicie**, digite **mmc**, e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  Clique em **Arquivo** e em **Adicionar/Remover Snap-ins**.  
  
3.  Clique em **certificados**, clique em **Add**, clique em **conta de computador**, clique em **Avançar**, clique em **computador Local**, clique em **terminar**e, em seguida, clique em **Okey**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Sobre o **solicitar certificados** página, clique no modelo de certificado do servidor Web e, em seguida, clique em **obter mais informações são necessárias para se registrar neste certificado**.  
  
    Se o modelo de certificado do servidor Web não aparecer, certifique-se de que a conta de computador do servidor de acesso remoto tem permissões de registro para o modelo de certificado do servidor Web. Para obter mais informações, consulte [configurar permissões no modelo de certificado de servidor Web](https://msdn.microsoft.com/library/ee649249.aspx).  
  
8.  Sobre o **assunto** guia da **propriedades do certificado** na caixa **nome da entidade**, para **tipo**, selecione **comuns nome**.  
  
9. Na **valor**, digite o nome de domínio totalmente qualificado (FQDN) para o nome de intranet do site de servidor de local de rede (por exemplo, nls.corp.contoso.com) e, em seguida, clique em **Add**.  
  
10. Clique em **OK**, **Registrar** e em **Concluir**.  
  
11. No painel de detalhes do snap-in Certificados, confirme se um novo certificado com o FQDN foi registrado com **Finalidades** de **Autenticação do Servidor**.  
  
12. Clique com o botão direito do mouse no certificado e, em seguida, clique em **Propriedades**.  
  
13. Em **Nome Amigável**, digite **Certificado de Local de Rede** e clique em **OK**.  
  
    > [!TIP]  
    > As etapas 12 e 13 são opcionais, mas tornam mais fácil para você selecionar o certificado para o local de rede ao configurar o acesso remoto.  
  
14. Repita esse procedimento em todos os servidores que você deseja ser membros do cluster.  
  
## <a name="BKMK_Add"></a>3.5 adicionar servidores ao cluster  
 
  
#### <a name="to-add-servers-to-the-cluster"></a>Para adicionar servidores ao cluster  
  
1.  No servidor do DirectAccess configurado, clique em **inicie**e, em seguida, clique em **gerenciamento de acesso remoto**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **Configuração**. No **tarefas** painel, em **Cluster de balanceamento de carga**, clique em **adicionar ou remover servidores**.  
  
3.  Sobre o **adicionar ou remover servidores** caixa de diálogo, clique em **Adicionar servidor**.  
  
4.  No **adicionar um servidor** caixa de diálogo a **Selecionar servidor** página, insira o nome do servidor de acesso remoto adicional e, em seguida, clique em **próxima**.  
  
5.  Sobre o **adaptadores de rede** página, faça o seguinte:  
  
    -   Se você estiver implantando uma topologia com dois adaptadores de rede, no **adaptador externo**, selecione o adaptador está conectado à rede externa. Na **adaptador interno**, selecione o adaptador está conectado à rede interna.  
  
    -   Se você estiver implantando uma topologia com um adaptador de rede, no **adaptador de rede**, selecione o adaptador está conectado à rede interna.  
  
6.  Sobre o **adaptadores de rede** página, na **selecione o certificado usado para autenticar conexões IP-HTTPS**, clique em **procurar** para localizar e selecionar o certificado IP-HTTPS, e, em seguida, clique em **próxima**.  
  
7.  Sobre o **servidor de local de rede** , clique em **procurar** para selecionar o certificado para o site de servidor de local de rede em execução no servidor de acesso remoto e, em seguida, clique em **Avançar**.  
  
    > [!NOTE]  
    > O **servidor de local de rede** página será exibida somente quando o site de servidor de local de rede está em execução no servidor de acesso remoto.  
  
    > [!NOTE]  
    > Se a VPN também foram configurada no servidor de acesso remoto, em seguida, você seria solicitado a adicionar as informações de pool de endereço de IP de VPN no momento.  
  
8.  Sobre o **resumo** , clique em **Add**.  
  
9. Na página **Conclusão**, clique em **Fechar**.  
  
10. Repita este procedimento para todos os servidores de acesso remoto a ser adicionado ao cluster.  
  
11. Sobre o **adicionar ou remover servidores** caixa de diálogo, clique em **confirmar**.  
  
12. Sobre o **adicionando e removendo servidores** caixa de diálogo, clique em **fechar**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Add-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
> [!NOTE]  
> Se a VPN não foi habilitado em um cluster com balanceamento de carga, você deve fornecer quaisquer intervalos de endereços VPN ao adicionar um novo servidor ao cluster usando cmdlets do Windows PowerShell. Se você tiver feito isso por engano, remova o servidor de cluster e adicioná-lo novamente ao cluster sem especificar os intervalos de endereços VPN.  
  
## <a name="BKMK_remove"></a>3.6 remover um servidor de cluster  
 
  
#### <a name="to-remove-a-server-from-the-cluster"></a>Para remover um servidor do cluster  
  
1.  O servidor de acesso remoto configurado, clique em **inicie**e, em seguida, clique em **gerenciamento de acesso remoto**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **Configuração**. No **tarefas** painel, em **Cluster de balanceamento de carga**, clique em **adicionar ou remover servidores**.  
  
3.  Sobre o **adicionar ou remover servidores** caixa de diálogo, selecione o servidor de acesso remoto que deseja remover e clique **remover servidor**.  
  
4.  Sobre o **remover servidor aviso** caixa de diálogo, verifique se você escolheu o servidor correto e, em seguida, clique em **Okey**.  
  
5.  Repita este procedimento para todos os servidores de acesso remoto a ser removido do cluster.  
  
6.  Sobre o **adicionar ou remover servidores** caixa de diálogo, clique em **confirmar**.  
  
7.  Sobre o **adicionando e removendo servidores** caixa de diálogo, clique em **fechar**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Remove-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
## <a name="BKBK_disable"></a>3.7 desabilitar balanceamento de carga  
[Siga esta etapa usando o Windows PowerShell](assetId:///7a817ca0-2b4a-4476-9d28-9a63ff2453f9)  
  
#### <a name="to-disable-load-balancing"></a>Para desabilitar o balanceamento de carga  
  
1.  No servidor do DirectAccess configurado, clique em **inicie**e, em seguida, clique em **gerenciamento de acesso remoto**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **Configuração**. No **tarefas** painel, em **Cluster de balanceamento de carga**, clique em **desabilitar balanceamento de carga**.  
  
3.  Sobre o **desabilitar balanceamento de carga** caixa de diálogo, clique em **Okey**.  
  
4.  Sobre o **desabilitar balanceamento de carga** caixa de diálogo, clique em **fechar**.  
  
![Windows PowerShell](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
set-RemoteAccessLoadBalancer -disable  
```  
  
Desabilitando o balanceamento de carga removerá as configurações de acesso remoto e configurações de NLB (se configurado) de todos os servidores, exceto o servidor do qual ele está sendo executado. Neste servidor de acesso remoto, configurações de NLB serão removidas (se configurada), mas permanecerão configurações de acesso remoto.  
  
Clicando em **remover definições de configuração** removerá o acesso remoto e o NLB (se configurado) de todos os servidores na implantação.  
  
> [!NOTE]  
> -   Se o acesso remoto for desinstalado quando o balanceamento de carga é implantado, todos os servidores são deixados com quedas. VIPs são removidos. Isso faz com que todas as rotas na rede corporativa que são direcionados para os endereços VIPs falhe. Isso também afeta as entradas DNS que foram resolvidas para os VIPs, como o nome do assunto de certificado do servidor de local de rede. Para evitar esse problema, desabilite o balanceamento de carga, que deixa os VIPs o último servidor de acesso remoto e, em seguida, desinstale o acesso remoto.  
> -   Depois de usar o **RemoteAccessLoadBalancer conjunto** cmdlet para desabilitar o balanceamento de carga, aguarde 2 minutos antes de executar qualquer outro cmdlet. Isso também deve ser feito em scripts que são executados de outro cmdlet após a **RemoteAccessLoadBalancer Set-desabilitar** cmdlet.  
> -   Desabilitando as alterações de balanceamento de carga com o endereço IP virtual do cluster em um endereço IP dedicado. Como resultado, qualquer operação que consulta para o nome do servidor falharão até que a entrada DNS em cache no servidor expira. Certifique-se de que você execute qualquer cmdlet do PowerShell de acesso remoto depois de desabilitar o balanceamento de carga até que o cache no servidor tiver expirado. Esse problema é mais comum se você tentar desabilitar o balanceamento de carga em um computador de outro computador que está em outro domínio. Isso também ocorre se você desabilitar a partir do console de gerenciamento de acesso remoto de balanceamento de carga e pode impedir que a configuração de carregamento. A configuração será carregado depois que o cache expirou ou foi liberado.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 4: Verificando o cluster](Step-4-Verify-the-Cluster.md)  
  


