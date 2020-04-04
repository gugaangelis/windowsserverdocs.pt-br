---
title: Etapa 2 configurar a infraestrutura multissite
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9434f3192da110c8ad61e999d2aecd02bfff3812
ms.sourcegitcommit: 3c3dfee8ada0083f97a58997d22d218a5d73b9c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639844"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>Etapa 2 configurar a infraestrutura multissite

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Para configurar uma implantação multissite, há uma série de etapas necessárias para modificar as configurações de infraestrutura de rede, incluindo: Configurando sites Active Directory adicionais e controladores de domínio, configurando grupos de segurança adicionais e configurando Objetos de Política de Grupo (GPOs) se você não estiver usando GPOs configurados automaticamente.  
  
|{1&gt;Tarefa&lt;1}|Descrição|  
|----|--------|  
|2.1. Configurar sites Active Directory adicionais|Configure sites Active Directory adicionais para a implantação.|  
|2.2. Configurar controladores de domínio adicionais|Configure Active Directory controladores de domínio adicionais conforme necessário.|  
|2.3. Configurar os grupos de segurança|Configure grupos de segurança para qualquer computador cliente com Windows 7.|  
|2.4. Configurar GPOs|Configure objetos Política de Grupo adicionais, conforme necessário.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets de exemplo do Windows PowerShell que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="21-configure-additional-active-directory-sites"></a><a name="BKMK_ConfigAD"></a>2,1. Configurar sites Active Directory adicionais  
Todos os pontos de entrada podem residir em um único site de Active Directory. Portanto, pelo menos um site de Active Directory é necessário para a implementação de servidores de acesso remoto em uma configuração multissite. Use este procedimento se você precisar criar o primeiro site Active Directory ou se desejar usar sites Active Directory adicionais para a implantação multissite. Use o snap-in Active Directory sites e serviços para criar novos sites na rede da sua organização.  

A associação no grupo **Administradores de empresa** na floresta ou no grupo Administradores de **domínio** no domínio raiz da floresta ou equivalente, no mínimo, é necessária para concluir este procedimento. Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).  

Para obter mais informações, consulte [adicionando um site à floresta](https://technet.microsoft.com/library/cc732761.aspx).  

### <a name="to-configure-additional-active-directory-sites"></a>Para configurar sites Active Directory adicionais  
  
1.  No controlador de domínio primário, clique em **Iniciar**e em **Active Directory sites e serviços**.  
  
2.  No console Active Directory sites e serviços, na árvore de console, clique com o botão direito do mouse em **sites**e clique em **novo site**.  
  
3.  Na caixa de diálogo **novo objeto – site** , na caixa **nome** , insira um nome para o novo site.  
  
4.  Em **nome do link**, clique em um objeto de link de site e, em seguida, clique em **OK** duas vezes.  
  
5.  Na árvore de console, expanda **sites**, clique com o botão direito do mouse em **sub-redes**e clique em **nova sub-rede**.  
  
6.  Na caixa de diálogo **novo objeto-sub-rede** , em **prefixo**, digite o prefixo de sub-rede IPv4 ou IPv6, na lista **selecionar um objeto de site para este prefixo** , clique no site a ser associado a essa sub-rede e, em seguida, clique em **OK**.  
  
7.  Repita as etapas 5 e 6 até que você tenha criado todas as sub-redes necessárias na sua implantação.  
  
8.  Feche Active Directory sites e serviços.  
  
![](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
Para instalar o recurso do Windows "Módulo Active Directory para Windows PowerShell":  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
ou adicione o "snap-in do Active Directory PowerShell" via OptionalFeatures.  
  
Se estiver executando os seguintes cmdlets no Windows 7 "ou no Windows Server 2008 R2, o módulo Active Directory PowerShell deverá ser importado:  
  
```  
Import-Module ActiveDirectory  
```  
  
Para configurar um site de Active Directory chamado "segundo site" usando o DEFAULTIPSITELINK interno:  
  
```  
New-ADReplicationSite -Name "Second-Site"  
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}  
```  
  
Para configurar sub-redes IPv4 e IPv6 para o segundo site:  
  
```  
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"  
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"  
```  
  
## <a name="22-configure-additional-domain-controllers"></a><a name="BKMK_AddDC"></a>2,2. Configurar controladores de domínio adicionais  
Para configurar uma implantação multissite em um único domínio, é recomendável que você tenha pelo menos um controlador de domínio gravável para cada site em sua implantação.  
  
Para executar esse procedimento, no mínimo você deve ser um membro do grupo Admins. do domínio no domínio no qual o controlador de domínio está sendo instalado.  
  
Para obter mais informações, consulte [instalando um controlador de domínio adicional](https://technet.microsoft.com/library/cc733027.aspx).
  
### <a name="to-configure-additional-domain-controllers"></a>Para configurar controladores de domínio adicionais  
  
1.  No servidor que atuará como um controlador de domínio, em **Gerenciador do servidor**, no **painel**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para acessar a tela de seleção de função de servidor  
  
3.  Na página **selecionar funções de servidor** , selecione **Active Directory Domain Services**. Clique em **Adicionar recursos** quando solicitado e, em seguida, clique em **Avançar** três vezes.  
  
4.  Na página **Confirmação**, clique em **Instalar**.  
  
5.  Quando a instalação for concluída com êxito, clique em **promover este servidor a um controlador de domínio**.  
  
6.  No assistente de configuração do Active Directory Domain Services, na página **configuração de implantação** , clique em **Adicionar um controlador de domínio a um domínio existente**.  
  
7.  Em **domínio**, insira o nome de domínio; por exemplo, corp.contoso.com.  
  
8.  Em **fornecer as credenciais para executar esta operação**, clique em **alterar**. Na caixa de diálogo **segurança do Windows** , forneça o nome de usuário e a senha para uma conta que possa instalar o controlador de domínio adicional. Para instalar um controlador de domínio adicional, você precisa ser membro do grupo Administradores Corporativos ou do grupo Admins. do Domínio. Quando tiver terminado de fornecer as credenciais, clique em **Avançar**.  
  
9. Na página **Opções do controlador de domínio** , faça o seguinte:  
  
    1.  Faça as seguintes seleções:  
  
        -   **Servidor DNS (sistema de nomes de domínio)** "esta opção é selecionada por padrão para que o controlador de domínio possa funcionar como um servidor DNS (sistema de nomes de domínio). Se não quiser que o controlador de domínio seja um servidor DNS, desmarque essa opção.  
  
            Se a função do servidor DNS não estiver instalada no emulador do controlador de domínio primário (PDC) no domínio raiz da floresta, a opção para instalar o servidor DNS em um controlador de domínio adicional não estará disponível. Como alternativa nessa situação, você pode instalar a função de servidor DNS antes ou depois da instalação do AD DS.  
  
            > [!NOTE]  
            > Se você selecionar a opção para instalar o servidor DNS, poderá receber uma mensagem que indica que uma delegação de DNS para o servidor DNS não pôde ser criada e que você deve criar manualmente uma delegação de DNS para o servidor DNS para garantir a resolução confiável de nomes. Se você estiver instalando um controlador de domínio adicional no domínio raiz da floresta ou em um domínio raiz da árvore, não será necessário criar a delegação de DNS. Nesse caso, clique em **Sim** e Desconsidere a mensagem.  
  
        -   **Catálogo global (GC)** "esta opção é selecionada por padrão. Ela adiciona o catálogo global, as partições de diretório somente leitura, ao controlador de domínio e habilita a funcionalidade de pesquisa do catálogo global.  
  
        -   **RODC (controlador de domínio somente leitura)** "essa opção não é selecionada por padrão. Ele torna o controlador de domínio adicional somente leitura; ou seja, ele torna o controlador de domínio um RODC.  
  
    2.  Em **nome do site**, selecione um site na lista.  
  
    3.  Em **digite a senha do modo de restauração dos serviços de diretório (DSRM)** , em **senha** e **Confirmar senha**, digite uma senha forte duas vezes e clique em **Avançar**. Essa senha deve ser usada para iniciar AD DS no DSRM para tarefas que devem ser executadas offline.  
  
10. Na página **Opções de DNS** , marque a caixa de seleção **Atualizar delegação de DNS** se desejar atualizar a delegação de DNS durante a instalação da função e clique em **Avançar**.  
  
11. Na página **Opções adicionais** , digite ou navegue até os locais de volume e pasta para o arquivo de banco de dados, os arquivos de log do serviço de diretório e os arquivos de volume do sistema (SYSVOL). Especifique as opções de replicação conforme necessário e clique em **Avançar**.  
  
12. Na página **Opções de revisão** , examine as opções de instalação e clique em **Avançar**.  
  
13. Na página **verificação de pré-requisitos** , depois que os pré-requisitos forem validados, clique em **instalar**.  
  
14. Aguarde até que o assistente conclua a configuração e clique em **fechar**.  
  
15. Reinicie o computador se ele não tiver sido reiniciado automaticamente.  
  
## <a name="23-configure-security-groups"></a><a name="BKMK_ConfigSG"></a>2,3. Configurar os grupos de segurança  
Uma implantação multissite requer um grupo de segurança adicional para computadores cliente do Windows 7 para cada ponto de entrada na implantação que permite o acesso a computadores cliente com o Windows 7. Se houver vários domínios que contenham computadores cliente do Windows 7, é recomendável criar um grupo de segurança em cada domínio para o mesmo ponto de entrada. Como alternativa, um grupo de segurança universal contendo os computadores cliente de ambos os domínios pode ser usado. Por exemplo, em um ambiente com dois domínios, se você quiser permitir o acesso a computadores cliente com Windows 7 nos pontos de entrada 1 e 3, mas não no ponto de entrada 2, crie dois novos grupos de segurança para conter os computadores cliente do Windows 7 para cada ponto de entrada em cada um dos domínios.  
  
### <a name="to-configure-additional-security-groups"></a>Para configurar grupos de segurança adicionais  
  
1.  No controlador de domínio primário, clique em **Iniciar**e em **Active Directory usuários e computadores**.  
  
2.  Na árvore de console, clique com o botão direito do mouse na pasta na qual você deseja adicionar um novo grupo, por exemplo, corp.contoso.com/Users. Aponte para **Novo** e clique em **Grupo**.  
  
3.  Na caixa de diálogo **novo grupo de objetos** , em **nome do grupo**, digite o nome do novo grupo, por exemplo, Win7_Clients_Entrypoint1.  
  
4.  Em **escopo do grupo**, clique em **Universal**, em **tipo de grupo**, clique em **segurança**e em **OK**.  
  
5.  Para adicionar computadores ao novo grupo de segurança, clique duas vezes no grupo de segurança e, na caixa de diálogo **Propriedades do < Group_Name >** , clique na guia **Membros** .  
  
6.  Na guia **Membros**, clique em **Adicionar**.  
  
7.  Selecione os computadores com Windows 7 a serem adicionados a este grupo de segurança e clique em **OK**.  
  
8.  Repita este procedimento para criar um grupo de segurança para cada ponto de entrada, conforme necessário.  
  
![](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
Para instalar o recurso do Windows "Módulo Active Directory para Windows PowerShell":  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
ou adicione o "snap-in do Active Directory PowerShell" via OptionalFeatures.  
  
Se estiver executando os seguintes cmdlets no Windows 7 "ou no Windows Server 2008 R2, o módulo Active Directory PowerShell deverá ser importado:  
  
```  
Import-Module ActiveDirectory  
```  
  
Para configurar um grupo de segurança chamado Win7_Clients_Entrypoint1 e adicionar um computador cliente chamado CLIENT2:  
  
```  
New-ADGroup -GroupScope universal -Name Win7_Clients_Entrypoint1  
Add-ADGroupMember -Identity Win7_Clients_Entrypoint1 -Members CLIENT2$  
```  
  
## <a name="24-configure-gpos"></a><a name="ConfigGPOs"></a>2,4. Configurar GPOs  
Uma implantação de acesso remoto multissite requer os seguintes objetos Política de Grupo:  
  
-   Um GPO para cada ponto de entrada para o servidor de acesso remoto.  
  
-   Um GPO para qualquer computador cliente do Windows 8 para cada domínio.  
  
-   Um GPO em cada domínio que contém computadores cliente do Windows 7 para cada ponto de entrada configurado para dar suporte a clientes do Windows 7.  
  
    > [!NOTE]  
    > Se você não tiver computadores cliente com o Windows 7, não será necessário criar GPOs para computadores com Windows 7.  
  
Quando você configura o acesso remoto, o assistente cria automaticamente os objetos de Política de Grupo necessários, caso eles não existam "t. Se você não tiver as permissões necessárias para criar Política de Grupo objetos, eles deverão ser criados antes de configurar o acesso remoto. O administrador do DirectAccess deve ter permissões totais nos GPOs (editar + modificar segurança + excluir).  
  
> [!IMPORTANT]  
> Depois de criar manualmente os GPOs para acesso remoto, você deve permitir tempo suficiente para replicação de Active Directory e DFS para o controlador de domínio no site do Active Directory que está associado ao servidor de acesso remoto. Se o acesso remoto criar automaticamente os objetos de Política de Grupo, nenhum tempo de espera será necessário.  
  
Para criar Política de Grupo objetos, consulte [criar e editar um objeto política de grupo](https://technet.microsoft.com/library/cc754740.aspx).  
  
### <a name="domain-controller-maintenance-and-downtime"></a><a name="DCMaintandDowntime"></a>Manutenção e tempo de inatividade do controlador de domínio  
Quando um controlador de domínio em execução como emulador de PDC ou controladores de domínio Gerenciando GPOs de servidor experimentam tempo de inatividade, não é possível carregar ou modificar a configuração de acesso remoto. Isso não afetará a conectividade do cliente se outros controladores de domínio estiverem disponíveis.  
  
Para carregar ou modificar a configuração de acesso remoto, você pode transferir a função de emulador de PDC para um controlador de domínio diferente para os GPOs do cliente ou do servidor de aplicativos; para GPOs de servidor, altere os controladores de domínio que gerenciam os GPOs do servidor.  
  
> [!IMPORTANT]  
> Esta operação só pode ser executada por um administrador de domínio. O impacto da alteração do controlador de domínio primário não é restrito ao acesso remoto; Portanto, tome cuidado ao transferir a função de emulador de PDC.  
  
> [!NOTE]  
> Antes de modificar a associação do controlador de domínio, verifique se todos os GPOs na implantação de acesso remoto foram replicados para todos os controladores de domínio no domínio. Se o GPO não estiver sincronizado, as alterações de configuração recentes poderão ser perdidas após a modificação da associação do controlador de domínio, o que pode levar a uma configuração corrompida. Para verificar a sincronização do GPO, consulte [verificar política de grupo status da infraestrutura](https://technet.microsoft.com/library/jj134176.aspx).  
  
#### <a name="to-transfer-the-pdc-emulator-role"></a><a name="TransferPDC"></a>Para transferir a função de emulador de PDC  
  
1.  Na tela **Iniciar** , digite**DSA. msc**e pressione Enter.  
  
2.  No painel esquerdo do console do Active Directory usuários e computadores, clique com o botão direito do mouse em **Active Directory usuários e computadores**e clique em **alterar controlador de domínio**. Na caixa de diálogo Alterar servidor de diretório, clique **nesse controlador de domínio ou AD LDS instância**, na lista, clique no controlador de domínio que será o novo detentor da função e, em seguida, clique em **OK**.  
  
    > [!NOTE]  
    > Você deve executar essa etapa se não estiver no controlador de domínio para o qual deseja transferir a função. Não execute esta etapa se você já estiver conectado ao controlador de domínio para o qual deseja transferir a função.  
  
3.  Na árvore de console, clique com o botão direito do mouse em **Active Directory usuários e computadores**, aponte para **todas as tarefas**e clique em **mestres de operações**.  
  
4.  Na caixa de diálogo mestres de operações, clique na guia **PDC** e, em seguida, clique em **alterar**.  
  
5.  Clique em **Sim** para confirmar que deseja transferir a função e, em seguida, clique em **fechar**.  
  
#### <a name="to-change-the-domain-controller-that-manages-server-gpos"></a><a name="ChangeDC"></a>Para alterar o controlador de domínio que gerencia GPOs de servidor  
  
-   Execute o cmdlet do Windows PowerShell [set-DAEntryPointDC](https://docs.microsoft.com/powershell/module/remoteaccess/set-daentrypointdc) no servidor de acesso remoto e especifique o nome do controlador de domínio inacessível para o parâmetro *ExistingDC* . Esse comando modifica a associação do controlador de domínio para os GPOs do servidor dos pontos de entrada que são atualmente gerenciados por esse controlador de domínio.
  
    -   Para substituir o controlador de domínio inacessível "dc1.corp.contoso.com" pelo controlador de domínio "dc2.corp.contoso.com", faça o seguinte:  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
    -   Para substituir o controlador de domínio inacessível "dc1.corp.contoso.com" por um controlador de domínio no site Active Directory mais próximo ao servidor de acesso remoto "DA1.corp.contoso.com", faça o seguinte:  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
### <a name="change-two-or-more-domain-controllers-that-manage-server-gpos"></a><a name="ChangeTwoDCs"></a>Alterar dois ou mais controladores de domínio que gerenciam GPOs de servidor  
Em um número mínimo de casos, dois ou mais controladores de domínio que gerenciam GPOs de servidor não estão disponíveis. Se isso ocorrer, mais etapas serão necessárias para alterar a associação do controlador de domínio para os GPOs do servidor.  
  
As informações de associação do controlador de domínio são armazenadas no registro dos servidores de acesso remoto e em todos os GPOs do servidor. No exemplo a seguir, há dois pontos de entrada com dois servidores de acesso remoto, "DA1" em "ponto de entrada 1" e "DA2" no "ponto de entrada 2". O GPO do servidor de "ponto de entrada 1" é gerenciado no controlador de domínio "DC1", enquanto o GPO do servidor de "ponto de entrada 2" é gerenciado no controlador de domínio "DC2". "DC1" e "DC2" não estão disponíveis. Um terceiro controlador de domínio ainda está disponível no domínio, "DC3", e os dados de "DC1" e "DC2" já foram replicados para "DC3".  
  
![Configurar a infraestrutura multissite](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)  
  
##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>Para alterar dois ou mais controladores de domínio que gerenciam GPOs de servidor  
  
1.  Para substituir o controlador de domínio não disponível "DC2" pelo controlador de domínio "DC3", execute o seguinte comando:  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Esse comando atualiza a associação do controlador de domínio para o GPO do servidor "ponto de entrada 2" no registro de DA2 e no próprio GPO do servidor "ponto de entrada 2"; no entanto, ele não atualiza o GPO do servidor "ponto de entrada 1" porque o controlador de domínio que o gerencia não está disponível.  
  
    > [!TIP]  
    > Esse comando usa o valor continue para o parâmetro *ErrorAction* , que atualiza o GPO do servidor "ponto de entrada 2", apesar da falha ao atualizar o GPO do servidor "ponto de entrada 1".  
  
    A configuração resultante é mostrada no diagrama a seguir.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)  
  
2.  Para substituir o controlador de domínio "DC1" não disponível pelo controlador de domínio "DC3", execute o seguinte comando:  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Esse comando atualiza a associação do controlador de domínio para o GPO do servidor "ponto de entrada 1" no registro de DA1 e nos GPOs do servidor "ponto de entrada 1" e "ponto de entrada 2". A configuração resultante é mostrada no diagrama a seguir.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)  
  
3.  Para sincronizar a associação do controlador de domínio para o GPO do servidor "ponto de entrada 2" no GPO do servidor "ponto de entrada 1", execute o comando para substituir "DC2" por "DC3" e especifique o servidor de acesso remoto cujo GPO do servidor não está sincronizado, neste caso, "DA1", para o parâmetro *ComputerName* .  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue  
    ```  
  
    A configuração final é mostrada no diagrama a seguir.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)  
  
### <a name="optimization-of-configuration-distribution"></a><a name="ConfigDistOptimization"></a>Otimização da distribuição de configuração  
Ao fazer alterações de configuração, as alterações são aplicadas somente depois que os GPOs do servidor se propagam para os servidores de acesso remoto. Para reduzir o tempo de distribuição da configuração, o acesso remoto seleciona automaticamente um controlador de domínio gravável que é [mais próximo do servidor de acesso remoto](https://technet.microsoft.com/library/cc978016.aspx) ao criar seu GPO de servidor.  
  
Em alguns cenários, pode ser necessário modificar manualmente o controlador de domínio que gerencia um GPO do servidor para otimizar o tempo de distribuição da configuração:  
  
-   Não havia nenhum controlador de domínio gravável no site de Active Directory de um servidor de acesso remoto no momento de adicioná-lo como um ponto de entrada. Um controlador de domínio gravável agora está sendo adicionado ao site de Active Directory do servidor de acesso remoto.  
  
-   Uma alteração de endereço IP, ou uma alteração Active Directory sites e sub-redes, pode ter movido o servidor de acesso remoto para um site diferente do Active Directory.  
  
-   A associação de controlador de domínio para um ponto de entrada foi modificada manualmente devido ao trabalho de manutenção em um controlador de domínio e agora o controlador de domínio está novamente online.  
  
Nesses cenários, execute o cmdlet do PowerShell `Set-DAEntryPointDC` no servidor de acesso remoto e especifique o nome do ponto de entrada que você deseja otimizar usando o parâmetro *entrypointname*. Você deve fazer isso somente depois que os dados do GPO do controlador de domínio atualmente armazenando o GPO do servidor já foram totalmente replicados para o novo controlador de domínio desejado.  
  
> [!NOTE]  
> Antes de modificar a associação do controlador de domínio, verifique se todos os GPOs na implantação de acesso remoto foram replicados para todos os controladores de domínio no domínio. Se o GPO não estiver sincronizado, as alterações de configuração recentes poderão ser perdidas após a modificação da associação do controlador de domínio, o que pode levar a uma configuração corrompida. Para verificar a sincronização do GPO, consulte [verificar política de grupo status da infraestrutura](https://technet.microsoft.com/library/jj134176.aspx).  
  
Para otimizar o tempo de distribuição da configuração, siga um destes procedimentos:  
  
-   Para gerenciar o GPO do servidor do ponto de entrada "ponto de entrada 1" em um controlador de domínio no site Active Directory mais próximo ao servidor de acesso remoto "DA1.corp.contoso.com", execute o seguinte comando:  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
-   Para gerenciar o GPO do servidor do ponto de entrada "ponto de entrada 1" no controlador de domínio "dc2.corp.contoso.com", execute o seguinte comando:  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
    > [!NOTE]  
    > Ao modificar o controlador de domínio associado a um ponto de entrada específico, você deve especificar um servidor de acesso remoto que seja membro desse ponto de entrada para o parâmetro *ComputerName* .  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 3: configurar a implantação multissite](Step-3-Configure-the-Multisite-Deployment.md)  
-   [Etapa 1: implementar uma implantação de acesso remoto de servidor único](Step-1-Implement-a-Single-Server-Remote-Access-Deployment.md)  
