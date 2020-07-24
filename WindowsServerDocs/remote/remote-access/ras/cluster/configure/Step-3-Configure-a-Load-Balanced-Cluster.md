---
title: 'Etapa 3: configurar um cluster com balanceamento de carga'
description: Este tópico faz parte do guia implantar o acesso remoto em um cluster no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: f000066e-7cf8-4085-82a3-4f4fe1cb3c5c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4e6378062215186ab877583a45481fc0b1cc6186
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965898"
---
# <a name="step-3-configure-a-load-balanced-cluster"></a>Etapa 3: configurar um cluster com balanceamento de carga

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Depois de preparar os servidores para o cluster, configure o balanceamento de carga no servidor único, configure os certificados necessários e implante o cluster.  
  
|Tarefa|Descrição|  
|----|--------|  
|[3,1 configurar o prefixo IPv6](#BKMK_Prefix)|Se o ambiente corporativo for IPv4 + IPv6 ou somente IPv6, em seguida, no servidor de acesso remoto único, verifique se o prefixo IPv6 atribuído aos computadores cliente do DirectAccess é grande o suficiente para abranger todos os servidores no cluster.|  
|[3,2 habilitar balanceamento de carga](#BKMK_NLB)|Habilite o balanceamento de carga no servidor de acesso remoto único.|  
|[3,3 instalar o certificado IP-HTTPS](#BKMK_InstallIPHTTP)|Cada servidor no cluster requer um certificado de servidor para autenticar a conexão IP-HTTPS.  Exporte o certificado IP-HTTPS do servidor de acesso remoto único e implante-o em cada servidor que será adicionado ao cluster. Isso é necessário apenas se você estiver usando certificados não autoassinados.|  
|[3,4 instalar o certificado do servidor do local de rede](#BKMK_NLS)|Se o servidor único tiver o servidor de local de rede implantado localmente, você precisará implantar o certificado do servidor de local de rede em cada servidor no cluster. Se o servidor de local de rede estiver hospedado em um servidor externo, um certificado em cada servidor não será necessário. Isso é necessário apenas se você estiver usando certificados não autoassinados.|  
|[3,5 adicionar servidores ao cluster](#BKMK_Add)|Adicione todos os servidores ao cluster. O acesso remoto não deve ser configurado nos servidores a serem adicionados.|  
|[3,6 remover um servidor do cluster](#BKMK_remove)|Instruções para remover um servidor do cluster.|  
|[3,7 desabilitar balanceamento de carga](#BKBK_disable)|Instruções para desabilitar o balanceamento de carga.|  
  
> [!NOTE]  
> O endereço IP selecionado para o DIP não deve estar em uso nos adaptadores de rede do primeiro servidor de acesso remoto no cluster. Iniciar a implantação do DirectAccess com VIP e DIP adicionados ao adaptador de rede resultará em falha.  
  
> [!NOTE]  
> Certifique-se de não usar um DIP que já esteja presente em outro computador na rede.  
  
## <a name="31-configure-the-ipv6-prefix"></a><a name="BKMK_Prefix"></a>3,1 configurar o prefixo IPv6  
  
### <a name="to-configure-the-prefix"></a><a name="configDA"></a>Para configurar o prefixo  
  
1.  No servidor de acesso remoto, clique em **Iniciar**e em **Gerenciamento de acesso remoto**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **Configuração**.  
  
3.  No painel central do console do, na área **etapa 2 do servidor DirectAccess** , clique em **Editar**.  
  
4.  Clique em **configuração de prefixo**. Na página **configuração de prefixo** , no **prefixo IPv6 atribuído aos computadores cliente do DirectAccess**, insira o prefixo IPv6 usado para computadores cliente do DirectAccess com um comprimento de sub-rede de 59, por exemplo, **2001: DB8:1: 1000::/59**. Se a VPN também tiver sido habilitada com IPv6, um prefixo IPv6 seria exibido e o comprimento da sub-rede precisaria ser alterado para 59. Clique em **Próximo**.  
  
5.  No painel central do console do, clique em **concluir**.  
  
6.  Na caixa de diálogo **revisão de acesso remoto** , examine as definições de configuração e clique em **aplicar**. Na caixa de diálogo **Aplicando Configurações do Assistente de Configuração de Acesso Remoto**, clique em **Fechar**.  
  
## <a name="32-enable-load-balancing"></a><a name="BKMK_NLB"></a>3,2 habilitar balanceamento de carga  
  
#### <a name="to-enable-load-balancing"></a>Para habilitar o balanceamento de carga  
  
1.  No servidor DirectAccess configurado, clique em **Iniciar**e em **Gerenciamento de acesso remoto**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No console de gerenciamento de acesso remoto, no painel esquerdo, clique em **configuração**e, no painel **tarefas** , clique em **habilitar balanceamento de carga**.  
  
3.  No assistente habilitar balanceamento de carga, clique em **Avançar**.  
  
4.  Dependendo do que você escolheu nas etapas de planejamento:  
  
    1.  NLB do Windows: na página **método de balanceamento de carga** , clique em **usar NLB (balanceamento de carga de rede) do Windows**e clique em **Avançar**.  
  
    2.  Balanceador de carga externo: na página **método de balanceamento de carga** , clique em **usar um balanceador de carga externo**e, em seguida, clique em **Avançar**.  
  
5.  Em uma única implantação de adaptador de rede, na página **endereços IP dedicados** , faça o seguinte e clique em **Avançar**:  
  
    1.  Na caixa **endereço IPv4** , insira o novo endereço IPv4 para este servidor de acesso remoto; o endereço IPv4 atual será o endereço IP virtual (VIP) do cluster de balanceamento de carga. Na caixa **máscara de sub-rede** , insira a máscara de sub-rede.  
  
    2.  Se o ambiente corporativo for IPv6 nativo, na caixa **endereço IPv6** , insira o novo endereço IPv6 para este servidor de acesso remoto; o endereço IPv6 atual será o VIP do cluster com balanceamento de carga. Na caixa **comprimento do prefixo da sub-rede** , insira o comprimento do prefixo da sub-rede.  
  
6.  Em uma implantação de dois adaptadores de rede, na página **endereços IP dedicados externos** , faça o seguinte e clique em **Avançar**:  
  
    1.  Na caixa **endereço IPv4** , insira o novo endereço IPv4 externo para este servidor de acesso remoto; o endereço IPv4 atual será o endereço IP virtual (VIP) do cluster de balanceamento de carga. Na caixa **máscara de sub-rede** , insira a máscara de sub-rede.  
  
    2.  Se houver endereços IPv6 nativos atualmente configurados no adaptador de rede voltado para a Internet do servidor de acesso remoto, na caixa **endereço IPv6** , insira o novo endereço IPv6 externo para este servidor de acesso remoto; o endereço IPv6 atual será o VIP do cluster de balanceamento de carga. Na caixa **comprimento do prefixo da sub-rede** , insira o comprimento do prefixo da sub-rede.  
  
7.  Em uma implantação de dois adaptadores de rede, na página **endereços IP dedicados internos** , faça o seguinte e clique em **Avançar**:  
  
    1.  Na caixa **endereço IPv4** , insira o novo endereço IPv4 interno para este servidor de acesso remoto; o endereço IPv4 atual será o VIP do cluster de balanceamento de carga. Na caixa **máscara de sub-rede** , insira a máscara de sub-rede.  
  
    2.  Se o ambiente corporativo for IPv6 nativo, na caixa **endereço IPv6** , insira o novo endereço IPv6 interno para este servidor de acesso remoto; o endereço IPv6 atual será o VIP do cluster de balanceamento de carga. Na caixa **comprimento do prefixo da sub-rede** , insira o comprimento do prefixo da sub-rede.  
  
8.  Na página **Resumo** , clique em **confirmar**.  
  
9. Na caixa de diálogo **habilitar balanceamento de carga** , clique em **fechar**.  
  
10. No assistente habilitar balanceamento de carga, clique em **fechar**.  
  
    > [!NOTE]  
    > Se o balanceamento de carga externo estiver sendo usado, anote os IPs virtuais e forneça-os como nos balanceadores de carga externos.  
  
![](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> do Windows PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Se você optar por usar o NLB do Windows nas etapas de planejamento, execute o seguinte:  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -InternetVirtualIPAddress @("2.1.1.1/255.255.255.0","2.1.1.2/255.255.255.0") -InternalVirtualIPAddress @("10.1.1.2/255.255.255.0","3ffe::2/64")  
```  
  
Se você optar por usar um balanceador de carga externo nas etapas de planejamento: execute o seguinte:  
  
```  
Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress "2.1.1.20/255.255.255.0" -InternalDedicatedIPAddress @("10.1.1.30/255.255.255.0","3ffe::20/64") -UseThirdPrtyLoadBalancer  
```  
  
> [!NOTE]  
> É recomendável não incluir alterações nas configurações do balanceador de carga com alterações em quaisquer outras configurações, se você estiver usando GPOs de preparo. As alterações nas configurações do balanceador de carga devem ser aplicadas primeiro e, em seguida, outras alterações de configuração devem ser feitas. Além disso, depois de configurar o balanceador de carga em um novo servidor DirectAccess, aguarde um pouco para que as alterações de IP sejam aplicadas e replicadas nos servidores DNS da empresa, antes de alterar outras configurações do DirectAccess relacionadas ao novo cluster.  
  
## <a name="33-install-the-ip-https-certificate"></a><a name="BKMK_InstallIPHTTP"></a>3,3 instalar o certificado IP-HTTPS  
A associação no grupo local **Administradores**, ou equivalente, é o mínimo necessário para concluir esse procedimento.  
  
### <a name="to-install-the-ip-https-certificate"></a><a name="IPHTTPSCert"></a>Para instalar o certificado IP-HTTPS  
  
1.  No servidor de acesso remoto configurado, clique em **Iniciar**, digite **MMC** e pressione Enter. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No console do MMC, no menu **Arquivo** , clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou remover snap-ins** , clique em **certificados**, em **Adicionar**, em **conta de computador**, em **Avançar**, em **concluir**e em **OK**.  
  
4.  No painel esquerdo do console, navegue até **certificados (computador local) \Personal\Certificates**. Clique com o botão direito do mouse no certificado IP-HTTPS, aponte para **todas as tarefas** e clique em **Exportar**.  
  
5.  Na página **Bem-vindo ao Assistente para Exportação de Certificados**, clique em **Próximo**.  
  
6.  Na página **Exportar Chave Privada**, clique em **Sim, exportar a chave privada** e em **Avançar**.  
  
7.  Na página **formato do arquivo de exportação** , clique em **troca de informações pessoais-#12 PKCS (. PFX)** e clique em **Avançar**.  
  
8.  Na página **segurança** , marque a caixa de seleção **senha** , digite uma senha na caixa **senha** e confirme a senha e clique em **Avançar**.  
  
9. Na página **arquivo a ser exportado** , insira um nome para o arquivo de certificado e salve-o na área de trabalho e clique em **Avançar**.  
  
10. Na página **Concluindo o Assistente para Exportação de Certificados**, clique em **Concluir**.  
  
11. Na caixa de diálogo **Assistente para exportação de certificados** , clique em **OK**.  
  
12. Copie o certificado para todos os servidores que você deseja que sejam membros do cluster.  
  
13. No novo servidor DirectAccess, clique em **Iniciar**, digite **MMC** e pressione Enter. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
14. No console do MMC, no menu **Arquivo** , clique em **Adicionar/Remover Snap-in**.  
  
15. Na caixa de diálogo **Adicionar ou remover snap-ins** , clique em **certificados**, em **Adicionar**, em **conta de computador**, em **Avançar**, em **concluir**e em **OK**.  
  
16. No painel esquerdo do console, navegue até **certificados (computador local) \Personal\Certificates**. Clique com o botão direito do mouse no nó **certificados** , aponte para **todas as tarefas**e clique em **importar**.  
  
17. Na página **Bem-vindo ao Assistente para Importação de Certificados**, clique em **Avançar**.  
  
18. Na página **arquivo a ser importado** , clique em **procurar** para localizar o certificado. Selecione o certificado e clique em **Avançar**.  
  
19. Na página **proteção de chave privada** , na caixa **senha** , digite a senha e clique em **Avançar**.  
  
20. Na página **Armazenamento de Certificados**, clique em **Avançar**.  
  
21. Na página **Concluindo o Assistente para Importação de Certificado**, clique em **Concluir**.  
  
22. Na caixa de diálogo **Assistente para importação de certificados** , clique em **OK**.  
  
23. Repita as etapas de 13-22 em todos os servidores que você deseja que sejam membros do cluster.  
  
## <a name="34-install-the-network-location-server-certificate"></a><a name="BKMK_NLS"></a>3,4 instalar o certificado do servidor do local de rede  
A associação no grupo local **Administradores**, ou equivalente, é o mínimo necessário para concluir esse procedimento.  
  
#### <a name="to-install-a-certificate-for-network-location"></a>Para instalar um certificado para local de rede  
  
1.  No servidor de acesso remoto, clique em **Iniciar**, digite **MMC**e pressione Enter. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  Clique em **Arquivo** e em **Adicionar/Remover Snap-ins**.  
  
3.  Clique em **certificados**, clique em **Adicionar**, em **conta de computador**, em **Avançar**, em **computador local**, em **concluir**e em **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Na página **solicitar certificados** , clique no modelo de certificado do servidor Web e, em seguida, clique em **mais informações são necessárias para se registrar nesse certificado**.  
  
    Se o modelo de certificado do servidor Web não for exibido, verifique se a conta de computador do servidor de acesso remoto tem permissões de registro para o modelo de certificado do servidor Web. Para obter mais informações, consulte [configurar permissões no modelo de certificado do servidor Web](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee649249(v=ws.10)).  
  
8.  Na guia **assunto** da caixa de **diálogo Propriedades do certificado** , em **nome da entidade**, para **tipo**, selecione **nome comum**.  
  
9. Em **valor**, digite o nome de domínio totalmente qualificado (FQDN) para o nome da intranet do site do servidor do local de rede (por exemplo, NLS.Corp.contoso.com) e clique em **Adicionar**.  
  
10. Clique em **OK**, **Registrar** e em **Concluir**.  
  
11. No painel de detalhes do snap-in Certificados, confirme se um novo certificado com o FQDN foi registrado com **Finalidades** de **Autenticação do Servidor**.  
  
12. Clique com o botão direito do mouse no certificado e, em seguida, clique em **Propriedades**.  
  
13. Em **Nome Amigável**, digite **Certificado de Local de Rede** e clique em **OK**.  
  
    > [!TIP]  
    > As etapas 12 e 13 são opcionais, mas facilitam a seleção do certificado para o local de rede ao configurar o acesso remoto.  
  
14. Repita esse procedimento em todos os servidores que você deseja que sejam membros do cluster.  
  
## <a name="35-add-servers-to-the-cluster"></a><a name="BKMK_Add"></a>3,5 adicionar servidores ao cluster  
 
  
#### <a name="to-add-servers-to-the-cluster"></a>Para adicionar servidores ao cluster  
  
1.  No servidor DirectAccess configurado, clique em **Iniciar**e em **Gerenciamento de acesso remoto**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **Configuração**. No painel **tarefas** , em **cluster com balanceamento de carga**, clique em **Adicionar ou remover servidores**.  
  
3.  Na caixa de diálogo **Adicionar ou remover servidores** , clique em **Adicionar servidor**.  
  
4.  Na caixa de diálogo **Adicionar um servidor** , na página **selecionar servidor** , digite o nome do servidor de acesso remoto adicional e clique em **Avançar**.  
  
5.  Na página **adaptadores de rede** , siga um destes procedimentos:  
  
    -   Se você estiver implantando uma topologia com dois adaptadores de rede, em **adaptador externo**, selecione o adaptador que está conectado à rede externa. Em **adaptador interno**, selecione o adaptador que está conectado à rede interna.  
  
    -   Se você estiver implantando uma topologia com um adaptador de rede, em **adaptador de rede**, selecione o adaptador que está conectado à rede interna.  
  
6.  Na página **adaptadores de rede** , em **selecionar o certificado usado para autenticar conexões IP-HTTPS**, clique em **procurar** para localizar e selecionar o certificado IP-HTTPS e, em seguida, clique em **Avançar**.  
  
7.  Na página **servidor de local de rede** , clique em **procurar** para selecionar o certificado para o site do servidor do local de rede em execução no servidor de acesso remoto e clique em **Avançar**.  
  
    > [!NOTE]  
    > A página **servidor de local de rede** aparece somente quando o site do servidor do local de rede está em execução no servidor de acesso remoto.  
  
    > [!NOTE]  
    > Se a VPN também tiver sido configurada no servidor de acesso remoto, você será solicitado a adicionar as informações do pool de endereços IP de VPN neste momento.  
  
8.  Na página **Resumo** , clique em **Adicionar**.  
  
9. Na página **Conclusão**, clique em **Fechar**.  
  
10. Repita esse procedimento para que todos os servidores de acesso remoto sejam adicionados ao cluster.  
  
11. Na caixa de diálogo **Adicionar ou remover servidores** , clique em **confirmar**.  
  
12. Na caixa de diálogo **adicionando e removendo servidores** , clique em **fechar**.  
  
![](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> do Windows PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Add-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
> [!NOTE]  
> Se a VPN não tiver sido habilitada em um cluster de balanceamento de carga, você não deverá fornecer nenhum intervalo de endereços VPN ao adicionar um novo servidor ao cluster usando os cmdlets do Windows PowerShell. Se você tiver feito isso por engano, remova o servidor do cluster e adicione-o novamente ao cluster sem especificar os intervalos de endereços VPN.  
  
## <a name="36-remove-a-server-from-the-cluster"></a><a name="BKMK_remove"></a>3,6 remover um servidor do cluster  
 
  
#### <a name="to-remove-a-server-from-the-cluster"></a>Para remover um servidor do cluster  
  
1.  No servidor de acesso remoto configurado, clique em **Iniciar**e em **Gerenciamento de acesso remoto**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **Configuração**. No painel **tarefas** , em **cluster com balanceamento de carga**, clique em **Adicionar ou remover servidores**.  
  
3.  Na caixa de diálogo **Adicionar ou remover servidores** , selecione o servidor de acesso remoto que você deseja remover e clique em **Remover servidor**.  
  
4.  Na caixa de diálogo **remover aviso do servidor** , escolha o servidor correto e clique em **OK**.  
  
5.  Repita esse procedimento para que todos os servidores de acesso remoto sejam removidos do cluster.  
  
6.  Na caixa de diálogo **Adicionar ou remover servidores** , clique em **confirmar**.  
  
7.  Na caixa de diálogo **adicionando e removendo servidores** , clique em **fechar**.  
  
![](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> do Windows PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Remove-RemoteAccessLoadBalancerNode -RemoteAccessServer <server name>  
```  
  
## <a name="37-disable-load-balancing"></a><a name="BKBK_disable"></a>3,7 desabilitar balanceamento de carga  
[Execute essa etapa usando o Windows PowerShell](assetId:///7a817ca0-2b4a-4476-9d28-9a63ff2453f9)  
  
#### <a name="to-disable-load-balancing"></a>Para desabilitar o balanceamento de carga  
  
1.  No servidor DirectAccess configurado, clique em **Iniciar**e em **Gerenciamento de acesso remoto**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No Console de gerenciamento de acesso remoto, clique em **Configuração**. No painel **tarefas** , em **cluster com balanceamento de carga**, clique em **desabilitar balanceamento de carga**.  
  
3.  Na caixa de diálogo **desabilitar balanceamento de carga** , clique em **OK**.  
  
4.  Na caixa de diálogo **desabilitar balanceamento de carga** , clique em **fechar**.  
  
![](../../../../media/Step-3-Configure-a-Load-Balanced-Cluster/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> do Windows PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
set-RemoteAccessLoadBalancer -disable  
```  
  
Desabilitar o balanceamento de carga removerá configurações de acesso remoto e configurações de NLB (se configurado) de todos os servidores, exceto o servidor do qual está sendo executado. Nesse servidor de acesso remoto, as configurações de NLB serão removidas (se tiver sido configuradas), mas as configurações de acesso remoto permanecerão.  
  
Clicar em **remover definições de configuração** removerá o acesso remoto e o NLB (se configurado) de todos os servidores na implantação.  
  
> [!NOTE]  
> -   Se o acesso remoto for desinstalado quando o balanceamento de carga for implantado, todos os servidores ficarão com DIPs. Os VIPs são removidos. Isso faz com que todas as rotas na rede corporativa que são destinadas aos endereços de VIPs falhem. Isso também afeta as entradas DNS que foram resolvidas para os VIPs, como o nome do assunto do certificado do servidor do local de rede. Para evitar esse problema, desabilite o balanceamento de carga, que deixa os VIPs no último servidor de acesso remoto e, em seguida, desinstale o acesso remoto.  
> -   Depois de usar o cmdlet **set-RemoteAccessLoadBalancer** para desabilitar o balanceamento de carga, aguarde 2 minutos antes de executar qualquer outro cmdlet. Isso também deve ser feito em todos os scripts que executam outro cmdlet após o cmdlet **set-RemoteAccessLoadBalancer-Disable** .  
> -   Desabilitar o balanceamento de carga altera o endereço IP virtual do cluster para um endereço IP dedicado. Como resultado, qualquer operação que consulte o nome do servidor falhará até que a entrada DNS armazenada em cache no servidor expire. Certifique-se de não executar nenhum cmdlet do PowerShell de acesso remoto depois de desabilitar o balanceamento de carga até que o cache no servidor tenha expirado. Esse problema é mais comum se você tentar desabilitar o balanceamento de carga em um computador de outro computador que esteja em outro domínio. Isso também ocorrerá se você desabilitar o balanceamento de carga do console de gerenciamento de acesso remoto e pode impedir que a configuração seja carregada. A configuração será carregada após o cache expirar ou ter sido liberada.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Confira também  
  
-   [Etapa 4: verificando o cluster](Step-4-Verify-the-Cluster.md)  
  
