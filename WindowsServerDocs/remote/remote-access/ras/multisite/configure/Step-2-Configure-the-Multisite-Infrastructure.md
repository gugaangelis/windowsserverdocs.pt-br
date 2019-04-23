---
title: Etapa 2 configurar a infraestrutura de multissite
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
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: pashort
author: shortpatti
ms.openlocfilehash: beecef692b2ac01e6cb6c36892fec16e55b08209
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835167"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>Etapa 2 configurar a infraestrutura de multissite

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Para configurar uma implantação multissite, há uma série de etapas necessárias para modificar as configurações de infraestrutura de rede incluindo: configurar sites adicionais do Active Directory e controladores de domínio, configuração de grupos de segurança adicional e configurar Objetos de diretiva de grupo (GPOs) se você não estiver usando automaticamente configurado GPOs.  
  
|Tarefa|Descrição|  
|----|--------|  
|2.1. Configure sites adicionais do Active Directory|Configure sites adicionais do Active Directory para a implantação.|  
|2.2. Configurar controladores de domínio adicionais|Configure controladores de domínio do Active Directory adicionais conforme necessário.|  
|2.3. Configurar os grupos de segurança|Configure grupos de segurança para todos os computadores cliente Windows 7.|  
|2.4. Configurar GPOs|Configure objetos de diretiva de grupo adicionais conforme necessário.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigAD"></a>2.1. Configure sites adicionais do Active Directory  
Todos os pontos de entrada podem residir em um único site do Active Directory. Portanto, pelo menos um site do Active Directory é necessário para a implementação de servidores de acesso remoto em uma configuração multissite. Use este procedimento se você precisa criar o primeiro site do Active Directory, ou caso você deseje usar sites adicionais do Active Directory para a implantação multissite. Use o snap-in Serviços e Sites do Active Directory para criar novos sites em sua organização "rede s.  

Associação a **Enterprise Admins** grupo na floresta ou o **Admins. do domínio** grupo no domínio raiz da floresta, ou equivalente, no mínimo é necessário para concluir este procedimento. Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).  

Para obter mais informações, consulte [adicionando um Site na floresta](https://technet.microsoft.com/library/cc732761.aspx).  

### <a name="to-configure-additional-active-directory-sites"></a>Para configurar sites adicionais do Active Directory  
  
1.  No controlador de domínio primário, clique em **inicie**e, em seguida, clique em **serviços e Sites do Active Directory**.  
  
2.  No console Serviços e Sites do Active Directory, na árvore de console, clique com botão direito **Sites**e, em seguida, clique em **novo Site**.  
  
3.  Sobre o **novo objeto - Site** na caixa a **nome** , digite um nome para o novo site.  
  
4.  Na **nome do Link**, clique em um objeto de link de site e, em seguida, clique em **Okey** duas vezes.  
  
5.  Na árvore de console, expanda **Sites**, clique com botão direito **sub-redes**e, em seguida, clique em **nova sub-rede**.  
  
6.  No **novo objeto - subrede** caixa de diálogo **prefixo**, digite o prefixo de sub-rede IPv4 ou IPv6, o **selecione um objeto de site para esse prefixo** lista, clique no site para associar com essa sub-rede e clique **Okey**.  
  
7.  Repita as etapas 5 e 6 até que você criou todas as sub-redes necessárias em sua implantação.  
  
8.  Feche os serviços e Sites do Active Directory.  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Para instalar o recurso do Windows "No módulo do Active Directory para o Windows PowerShell":  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
ou adicione o "Active Directory PowerShell Snap-In" por meio de OptionalFeatures.  
  
Se executando os seguintes cmdlets no Windows Server 2008 R2 ou no Windows 7", em seguida, o módulo do PowerShell do Active Directory deve ser importado:  
  
```  
Import-Module ActiveDirectory  
```  
  
Para configurar um site do Active Directory denominado "Segundo Site" usando o DEFAULTIPSITELINK internos:  
  
```  
New-ADReplicationSite -Name "Second-Site"  
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}  
```  
  
Para configurar as sub-redes IPv4 e IPv6 para o segundo Site:  
  
```  
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"  
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"  
```  
  
## <a name="BKMK_AddDC"></a>2.2. Configurar controladores de domínio adicionais  
Para configurar uma implantação multissite em um único domínio, é recomendável que você tenha pelo menos um controlador de domínio gravável para cada site na sua implantação.  
  
Para executar este procedimento, no mínimo, que você deve ser um membro do grupo Admins. do domínio no domínio em que o controlador de domínio está sendo instalado.  
  
Para obter mais informações, consulte [instalando um controlador de domínio adicional](https://technet.microsoft.com/library/cc733027.aspx).
  
### <a name="to-configure-additional-domain-controllers"></a>Configurar controladores de domínio adicionais  
  
1.  No servidor que atua como um controlador de domínio, na **Gerenciador do servidor**diante a **Dashboard**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **próxima** três vezes para chegar à tela de seleção de função de servidor  
  
3.  Sobre o **selecionar funções do servidor** , selecione **Active Directory Domain Services**. Clique em **adicionar recursos** quando solicitado e, em seguida, clique em **próxima** três vezes.  
  
4.  Na página **Confirmação**, clique em **Instalar**.  
  
5.  Quando a instalação for concluída com êxito, clique em **promover este servidor a um controlador de domínio**.  
  
6.  No Active Directory domínio serviços de Assistente de configuração, sobre o **configuração de implantação** , clique em **adicionar um controlador de domínio a um domínio existente**.  
  
7.  Na **domínio**, digite o domínio nome; por exemplo, corp.contoso.com.  
  
8.  Sob **fornecer as credenciais para executar essa operação**, clique em **alteração**. Sobre o **segurança do Windows** caixa de diálogo caixa, forneça o nome de usuário e senha para uma conta que possa instalar o controlador de domínio adicional. Para instalar um controlador de domínio adicional, você precisa ser membro do grupo Administradores Corporativos ou do grupo Admins. do Domínio. Quando tiver terminado de fornecer as credenciais, clique em **Avançar**.  
  
9. Sobre o **opções do controlador de domínio** página, faça o seguinte:  
  
    1.  Faça as seguintes seleções:  
  
        -   **Servidor de nome DNS (sistema) do domínio**"essa opção é selecionada por padrão para que seu controlador de domínio pode funcionar como um servidor de sistema de nomes de domínio (DNS). Se não quiser que o controlador de domínio seja um servidor DNS, desmarque essa opção.  
  
            Se a função de servidor DNS não estiver instalada no emulador do controlador de domínio primário (PDC) no domínio raiz da floresta, a opção para instalar o servidor DNS em um controlador de domínio não está disponível. Como uma solução alternativa nessa situação, você pode instalar a função de servidor DNS antes ou após a instalação do AD DS.  
  
            > [!NOTE]  
            > Se você selecionar a opção de instalar o servidor DNS, você poderá receber uma mensagem que indica que uma delegação de DNS para o servidor DNS não pôde ser criada e que você deve criar manualmente uma delegação de DNS para o servidor DNS para garantir que a resolução de nome confiável. Se você estiver instalando um controlador de domínio no domínio raiz da floresta ou um domínio raiz da árvore, você não precisa criar a delegação de DNS. Nesse caso, clique em **Sim** e desconsidere a mensagem.  
  
        -   **Catálogo global (GC)**"essa opção é selecionada por padrão. Ela adiciona o catálogo global, as partições de diretório somente leitura, ao controlador de domínio e habilita a funcionalidade de pesquisa do catálogo global.  
  
        -   **O controlador de domínio somente leitura (RODC)**"essa opção não é selecionada por padrão. Ele faz com que o controlador de domínio adicional somente leitura; ou seja, ele torna o controlador de domínio um RODC.  
  
    2.  Na **nome do Site**, selecione um site na lista.  
  
    3.  Sob **digite a senha do modo de restauração dos serviços de diretório (DSRM)**, na **senha** e **Confirmar senha**, digite uma senha forte duas vezes e, em seguida, clique em  **Próxima**. Essa senha deve ser usada para iniciar o AD DS em DSRM para tarefas que devem ser executadas offline.  
  
10. No **opções de DNS** página, selecione o **delegação de DNS atualizar** caixa de seleção se você quiser atualizar delegação de DNS durante a instalação da função e, em seguida, clique em **próxima**.  
  
11. Sobre o **opções adicionais** página, digite ou navegue até os locais de volume e pasta para o arquivo de banco de dados, os arquivos de log do serviço de diretório e os arquivos do sistema (SYSVOL) de volume. Especifique as opções de replicação, conforme necessário e, em seguida, clique em **próxima**.  
  
12. Sobre o **examinar opções** página, examine as opções de instalação e, em seguida, clique em **próxima**.  
  
13. Sobre o **verificação de pré-requisitos** página, depois que os pré-requisitos forem validados, clique em **instalar**.  
  
14. Aguarde até que o assistente conclui a configuração e, em seguida, clique em **fechar**.  
  
15. Reinicie o computador se ele não foi reiniciado automaticamente.  
  
## <a name="BKMK_ConfigSG"></a>2.3. Configurar os grupos de segurança  
Uma implantação multissite requer um grupo de segurança adicionais para computadores de cliente do Windows 7 para cada ponto de entrada na implantação que permite o acesso a computadores cliente do Windows 7. Se houver vários domínios que contêm computadores cliente do Windows 7, em seguida, é recomendável criar um grupo de segurança em cada domínio para o mesmo ponto de entrada. Como alternativa, um grupo de segurança universal que contém os computadores cliente de ambos os domínios pode ser usado. Por exemplo, em um ambiente com dois domínios, se você quiser permitir o acesso aos computadores de cliente do Windows 7 em pontos de entrada 1 e 3, mas não na entrada do ponto 2, em seguida, criar dois novos grupos de segurança para conter os computadores de cliente do Windows 7 para cada ponto de entrada em cada uma da  domínios.  
  
### <a name="to-configure-additional-security-groups"></a>Para configurar grupos de segurança adicional  
  
1.  No controlador de domínio primário, clique em **inicie**e, em seguida, clique em **Active Directory Users and Computers**.  
  
2.  Na árvore de console, clique com botão direito na pasta na qual você deseja adicionar um novo grupo, por exemplo, corp.contoso.com/Users. Aponte para **Novo** e clique em **Grupo**.  
  
3.  Sobre o **novo objeto - grupo** caixa de diálogo **nome do grupo**, digite o nome do novo grupo, por exemplo, Win7_Clients_Entrypoint1.  
  
4.  Sob **escopo do grupo**, clique em **Universal**, em **tipo de grupo**, clique em **segurança**e, em seguida, clique em **Okey**.  
  
5.  Para adicionar computadores ao novo grupo de segurança, clique duas vezes no grupo de segurança e, na **< nome_do_grupo > propriedades** caixa de diálogo, clique o **membros** guia.  
  
6.  Na guia **Membros** , clique em **Adicionar**.  
  
7.  Selecione os computadores Windows 7 para adicionar a esse grupo de segurança e, em seguida, clique em **Okey**.  
  
8.  Repita este procedimento para criar um grupo de segurança para cada ponto de entrada conforme necessário.  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Para instalar o recurso do Windows "No módulo do Active Directory para o Windows PowerShell":  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
ou adicione o "Active Directory PowerShell Snap-In" por meio de OptionalFeatures.  
  
Se executando os seguintes cmdlets no Windows Server 2008 R2 ou no Windows 7", em seguida, o módulo do PowerShell do Active Directory deve ser importado:  
  
```  
Import-Module ActiveDirectory  
```  
  
Para configurar um grupo de segurança chamado Win7_Clients_Entrypoint1 e para adicionar um computador cliente chamado CLIENT2:  
  
```  
New-ADGroup -GroupScope universal -Name Win7_Clients_Entrypoint1  
Add-ADGroupMember -Identity Win7_Clients_Entrypoint1 -Members CLIENT2$  
```  
  
## <a name="ConfigGPOs"></a>2.4. Configurar GPOs  
Uma implantação multissite do acesso remoto requer os seguintes objetos de diretiva de grupo:  
  
-   Um GPO para cada ponto de entrada para o servidor de acesso remoto.  
  
-   Um GPO para nenhum dos computadores cliente Windows 8 para cada domínio.  
  
-   Um GPO em cada domínio que contém os computadores cliente do Windows 7 para cada ponto de entrada configurado para suporte ao Windows 7, os clientes.  
  
    > [!NOTE]  
    > Se você não tiver nenhum dos computadores cliente Windows 7, você precisa criar GPOs para o Windows 7 computadores.  
  
Quando você configurar o acesso remoto, o assistente cria automaticamente os objetos de diretiva de grupo necessários se eles não "t já existe. Se você não tiver as permissões necessárias para criar objetos de diretiva de grupo, eles devem ser criados antes de configurar o acesso remoto. O administrador do DirectAccess deve ter permissões completas sobre os GPOs (editar + modificar a segurança + delete).  
  
> [!IMPORTANT]  
> Depois de criar manualmente os GPOs para acesso remoto, você deve permitir tempo suficiente para o Active Directory e a replicação do DFS para o controlador de domínio no site do Active Directory que está associado com o servidor de acesso remoto. Não se o acesso remoto criado automaticamente os objetos de diretiva de grupo, é necessário nenhum tempo de espera.  
  
Para criar objetos de diretiva de grupo, consulte [criar e editar um objeto de diretiva de grupo](https://technet.microsoft.com/library/cc754740.aspx).  
  
### <a name="DCMaintandDowntime"></a>Tempo de inatividade e a manutenção do controlador de domínio  
Quando um controlador de domínio em execução como o emulador do PDC ou controladores de domínio, gerenciamento de GPOs de servidor enfrentam tempo de inatividade, não é possível carregar ou modificar a configuração de acesso remoto. Isso não afeta a conectividade de cliente se outros controladores de domínio estão disponíveis.  
  
Para carregar ou modificar a configuração de acesso remoto, você pode transferir a função de emulador PDC para outro controlador de domínio para o aplicativo cliente ou servidor GPOs; para GPOs do servidor, altere os controladores de domínio que gerencia os GPOs do servidor.  
  
> [!IMPORTANT]  
> Esta operação pode ser executada somente por um administrador de domínio. O impacto de alterar o controlador de domínio primário não está limitado a acesso remoto; Portanto, tenha cuidado ao transferir a função de emulador PDC.  
  
> [!NOTE]  
> Antes de modificar a associação de controlador de domínio, certifique-se de que todos os GPOs na implantação do acesso remoto foram replicados para todos os controladores de domínio no domínio. Se o GPO não estiver sincronizado, alterações de configuração recentes poderão ser perdidas depois de modificar a associação de controlador de domínio, o que pode levar a uma configuração corrompida. Para verificar a sincronização de GPO, consulte [verificar o Status de infraestrutura da política de grupo](https://technet.microsoft.com/library/jj134176.aspx).  
  
#### <a name="TransferPDC"></a>Para transferir a função de emulador PDC  
  
1.  Sobre o **inicie** tela, digite**DSA. msc**, e pressione ENTER.  
  
2.  No painel esquerdo do console do Active Directory Users and Computers, clique com botão direito **Active Directory Users and Computers**e, em seguida, clique em **Alterar controlador de domínio**. Na caixa de diálogo Alterar servidor de diretório, clique em **instância este controlador de domínio ou do AD LDS**, na lista de controlador de domínio que será o novo detentor da função e, em seguida, clique em clique em **Okey**.  
  
    > [!NOTE]  
    > Você deve executar essa etapa se você não estiver no controlador de domínio ao qual você deseja transferir a função. Não execute esta etapa se você já estiver conectado ao controlador de domínio ao qual você deseja transferir a função.  
  
3.  Na árvore de console, clique com botão direito **Active Directory Users and Computers**, aponte para **todas as tarefas**e, em seguida, clique em **mestres de operações**.  
  
4.  Na caixa de diálogo de mestres de operações, clique no **PDC** guia e, em seguida, clique em **alteração**.  
  
5.  Clique em **Yes** para confirmar que você deseja transferir a função e, em seguida, clique em **fechar**.  
  
#### <a name="ChangeDC"></a>Para alterar o controlador de domínio que gerencia GPOs de servidor  
  
-   Execute o cmdlet do Windows PowerShell `HYPERLINK "https://technet.microsoft.com/library/hh918412.aspx" Set-DAEntryPointDC` no servidor de acesso remoto e especifique o nome do controlador de domínio inacessível para o *ExistingDC* parâmetro. Este comando modifica a associação de controlador de domínio para os GPOs do servidor dos pontos de entrada que estão sendo gerenciados pelo controlador de domínio.  
  
    -   Para substituir o controlador de domínio inacessível "dc1.corp.contoso.com" com o controlador de domínio "dc2.corp.contoso.com", faça o seguinte:  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
    -   Para substituir o controlador de domínio inacessível "dc1.corp.contoso.com" com um controlador de domínio no site do Active Directory mais próximo ao servidor de acesso remoto "DA1.corp.contoso.com", faça o seguinte:  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
### <a name="ChangeTwoDCs"></a>Alterar duas ou mais controladores de domínio que gerencia GPOs de servidor  
Um número mínimo de casos, dois ou mais controladores de domínio que gerencia GPOs de servidor não estão disponíveis. Se isso ocorrer, mais etapas são necessárias para alterar a associação de controlador de domínio para os GPOs do servidor.  
  
Informações de associação de controlador de domínio são armazenadas no registro de servidores de acesso remoto e em todos os GPOs de servidor. No exemplo a seguir, há dois pontos de entrada com dois servidores de acesso remoto, "DA1" no "ponto de entrada 1" e "DA2" no "Ponto de entrada 2". O GPO do servidor de "Ponto de entrada 1" é gerenciado no controlador de domínio "DC1", enquanto o GPO do servidor de "ponto de entrada 2" é gerenciado no controlador de domínio "DC2". "DC1" e "DC2" não estão disponíveis. Um terceiro controlador de domínio ainda está disponível no domínio, "DC3, e os dados do"DC1"e"DC2"já foi replicados para"DC3".  
  
![Configurar a infraestrutura de multissite](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)  
  
##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>Para alterar a dois ou mais controladores de domínio que gerencia GPOs de servidor  
  
1.  Para substituir o controlador de domínio não está disponível "DC2" com o controlador de domínio "DC3", execute o seguinte comando:  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Esse comando atualiza a domínio controlador associação para o GPO no registro do servidor de "Ponto de entrada 2" do DA2 e no servidor "2 do ponto de entrada" GPO em si; No entanto, ele não atualiza o GPO do servidor "1 do ponto de entrada" porque o controlador de domínio que gerencia a ele não está disponível.  
  
    > [!TIP]  
    > Esse comando usa o valor de continuar para o *ErrorAction* parâmetros, que atualiza o GPO do servidor de "Ponto de entrada 2", apesar da falha ao atualizar o GPO do servidor "1 do ponto de entrada".  
  
    A configuração resultante é mostrada no diagrama a seguir.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)  
  
2.  Para substituir o controlador de domínio não está disponível "DC1" com o controlador de domínio "DC3", execute o seguinte comando:  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Esse comando atualiza a associação de controlador de domínio para os GPOs do servidor do GPO do servidor no registro do DA1 e em "1 do ponto de entrada" e "2 do ponto de entrada" de "Ponto de entrada 1". A configuração resultante é mostrada no diagrama a seguir.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)  
  
3.  Para sincronizar a associação de controlador de domínio para o servidor de "Ponto de entrada 2" GPO em "1 do ponto de entrada" GPO de servidor, execute o comando para substituir "DC2" com "DC3" e especifique o servidor de acesso remoto cujo GPO do servidor não está sincronizado, nesse caso, "DA1," para o  *ComputerName* parâmetro.  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue  
    ```  
  
    A configuração final é mostrada no diagrama a seguir.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)  
  
### <a name="ConfigDistOptimization"></a>Otimização de distribuição de configuração  
Ao fazer alterações de configuração, as alterações são aplicadas somente depois que os GPOs do servidor se propague para servidores de acesso remoto. Para reduzir o tempo de distribuição de configuração, o acesso remoto seleciona automaticamente um controlador de domínio gravável que é o HYPERLINK "https://technet.microsoft.com/library/cc978016.aspx" mais próximo ao servidor de acesso remoto ao criar o GPO do servidor.  
  
Em alguns cenários, pode ser necessário modificar manualmente o controlador de domínio que gerencia um GPO de servidor para otimizar o tempo de distribuição de configuração:  
  
-   Não havia nenhum controlador de domínio gravável no site do Active Directory de um servidor de acesso remoto no momento da adicioná-lo como um ponto de entrada. Agora está sendo adicionado a um controlador de domínio gravável para o site do Active Directory do servidor de acesso remoto.  
  
-   Uma alteração de endereço IP ou uma alteração de sub-redes e Sites do Active Directory pode ter movido o servidor de acesso remoto para outro site do Active Directory.  
  
-   A associação de controlador de domínio para um ponto de entrada foi modificada manualmente devido ao trabalho de manutenção em um controlador de domínio, e agora o controlador de domínio estiver online novamente.  
  
Nesses cenários, execute o cmdlet do PowerShell `Set-DAEntryPointDC` no servidor de acesso remoto e especifique o nome do ponto de entrada que você deseja otimizar usando o parâmetro *EntryPointName*. Você deve fazer isso apenas após os dados do GPO do controlador de domínio que estão armazenando o servidor que GPO totalmente já foram replicado para o novo controlador de domínio desejado.  
  
> [!NOTE]  
> Antes de modificar a associação de controlador de domínio, certifique-se de que todos os GPOs na implantação do acesso remoto foram replicados para todos os controladores de domínio no domínio. Se o GPO não estiver sincronizado, alterações de configuração recentes poderão ser perdidas depois de modificar a associação de controlador de domínio, o que pode levar a uma configuração corrompida. Para verificar a sincronização de GPO, consulte [verificar o Status de infraestrutura da política de grupo](https://technet.microsoft.com/library/jj134176.aspx).  
  
Para otimizar o tempo de distribuição de configuração, siga um destes procedimentos:  
  
-   Para gerenciar o servidor GPO da entrada de ponto de "1 do ponto de entrada" em um controlador de domínio no site do Active Directory mais próximo ao servidor de acesso remoto "DA1.corp.contoso.com", execute o seguinte comando:  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
-   Para gerenciar o servidor do GPO da entrada de ponto de "1 do ponto de entrada" sobre o domínio controlador "dc2.corp.contoso.com", execute o seguinte comando:  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
    > [!NOTE]  
    > Ao modificar o controlador de domínio associado com um ponto de entrada específico, você deve especificar um servidor de acesso remoto que é um membro do ponto de entrada para o *ComputerName* parâmetro.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 3: Configurar a implantação multissite](Step-3-Configure-the-Multisite-Deployment.md)  
-   [Etapa 1: Implementar um único servidor de implantação do acesso remoto](Step-1-Implement-a-Single-Server-Remote-Access-Deployment.md)  

