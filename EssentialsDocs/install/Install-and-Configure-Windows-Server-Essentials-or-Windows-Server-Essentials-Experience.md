---
title: "Instalar e configurar o Windows Server Essentials ou da experiência do Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48ea6cd4-3955-4aaf-9236-2515a6c3e730
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8a2310b178663c6ca32a4e07d11656f1aaf2a11b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-and-configure-windows-server-essentials-or-windows-server-essentials-experience"></a>Instalar e configurar o Windows Server Essentials ou da experiência do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server Essentials é um servidor de primeira ideal para pequenas empresas com até 25 usuários e 50 dispositivos. Para organizações com até 100 usuários e 200 dispositivos, agora você pode usar o Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada. Este tópico aborda os dois cenários.  
  
A experiência do Windows Server Essentials é uma função no Windows Server 2016 que permite que você tire proveito de todos os recursos (por exemplo, o backup do computador e de acesso via Web remoto) que estão disponíveis para você no Windows Server Essentials sem bloqueios e limites são impostos no Windows Server Essentials. Essa função de servidor também está disponível no Windows Server Essentials e é habilitada por padrão.
  
Antes de instalar o Windows Server Essentials ou a função de experiência do Essentials, observe as seguintes limitações.  
  
|Experiência do Windows Server Essentials no Windows Server Essentials|Experiência do Windows Server Essentials no Windows Server 2016
|----|----|
|-Deve ser o controlador de domínio na raiz da floresta e no domínio e deve manter todas as funções FSMO.<br /><br /> -Não pode ser instalado em um ambiente com um domínio do Active Directory preexistente (no entanto, há um período de carência de 21 dias para realizar migrações).|-Não precisa ser um controlador de domínio se estiver instalado em um ambiente com um domínio do Active Directory preexistente.<br /><br /> -Se um domínio do Active Directory não existir, instalando a função irá criar um domínio do Active Directory e o servidor se tornará o controlador de domínio na raiz da floresta e domínio, segurando todas as funções FSMO.  
|Só pode ser implantado em um único domínio.|Só pode ser implantado em um único domínio.  
|Um controlador de domínio somente leitura não pode existir em seu domínio.|Um controlador de domínio somente leitura não pode existir em seu domínio.

> [!NOTE]
>  Para baixar versões de avaliação dos sistemas operacionais, visite o Centro de avaliação do TechNet:  
>   
>  [Baixe o Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)  
>   
>  [Baixe o Windows Server Essentials](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016-essentials)
  
## <a name="installation-options"></a>Opções de instalação  
 Este documento fornece instruções passo a passo para instalar e configurar o Windows Server Essentials. Dependendo do seu ambiente de rede, você tem as seguintes opções de instalação disponíveis para você:  
  
-    Windows Server Essentials (com a função de experiência do Windows Server Essentials habilitada por padrão)  
  
-    Windows Server 2016 com a função de experiência do Windows Server Essentials instalada  
 
|Ambiente de implantação|Descrição|Seção relacionados|  
|----------------------------|-----------------|---------------------|  
|Novo ambiente do Active Directory|Você pode instalar o Windows Server Essentials para criar um novo ambiente do Active Directory.|[Implantar o Windows Server Essentials para configurar um novo ambiente do Active Directory](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_NewAD)|  
|Ambiente existente do Active Directory|Você pode instalar o Windows Server Essentials em um ambiente do Active Directory existente.|[Implantar o Windows Server Essentials em um ambiente existente do Active Directory](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_ExistingAD)|  
|Ambiente virtual|Você pode implantar o Windows Server Essentials como uma máquina virtual.|[Virtualizar seu ambiente](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_VirtualWSE)|  
|Implantação automatizada|Você pode automatizar a implantação do Windows Server Essentials por meio do Windows PowerShell.|[Instalar e configurar o Windows Server Essentials por meio do Windows PowerShell](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_PowerShell)|  
  
## <a name="before-you-begin"></a>Antes de começar  
 Antes de começar a instalação, examine a documentação a seguir:  
  
-   [Visão geral do produto Windows Server Essentials](https://www.microsoft.com/server-cloud/windows-server-essentials/windows-server-2012-r2-essentials.aspx)  
  

-   [Requisitos de sistema do Windows Server Essentials](../get-started/system-requirements.md)   

  
##  <a name="BKMK_NewAD"></a>Implantar o Windows Server Essentials para configurar um novo ambiente do Active Directory  
 Windows Server Essentials oferece uma maneira para você configurar um ambiente do Active Directory e recursos do servidor relacionados rapidamente.  
  
###  <a name="BKMK_WSEDeploy"></a>Implantação do Windows Server Essentials  
 Se você estiver usando o Windows Server Essentials, Windows Server Essentials Experience já está habilitado. No entanto, você deve concluir algumas etapas para configurar seu servidor.  
  
##### <a name="to-configure-windows-server-essentials-on-a-physical-server"></a>Para configurar o Windows Server Essentials em um servidor físico  
  
1.  Após o Windows **boas-vindas** página, o **configurar o Windows Server Essentials assistente** esteja visível na sua área de trabalho.  
  
2.  Siga as instruções para concluir o Assistente da seguinte maneira:  
  
    1.  Sobre o **configurar o Windows Server Essentials** página, clique em **próxima**.  
  
    2.  Em **configurações de hora**, verifique se sua data, hora e fuso horário estão corretos e, em seguida, clique em **próxima**.  
  
    3.  Em **informações da empresa**, digite o nome da empresa, como **Contoso, Ltd.**e clique em **próxima**. Opcionalmente, você pode alterar o nome de domínio interno e o nome do servidor.  
  
    4.  Em **criar administrador de rede**, digite um novo nome de conta de administrador e a senha.  
  
        > [!NOTE]
        >  Não use o padrão **administrador** nome e a senha da conta.  
  
    5.  Clique em **configurar**.  
  
3.  O servidor será reiniciado várias vezes durante o processo de configuração e seu logons será automáticos até que a configuração seja concluída. Esse processo leva cerca de 20 minutos.  
  
4.  Na área de trabalho, clique no ícone de painel para iniciar o painel do servidor. No **Home** página, conclua a **Introdução** tarefas listadas no **instalação** guia.  
  
 Depois de concluir a configuração do servidor, o servidor que está executando o Windows Server Essentials ser configurará como um controlador de domínio.  
  
###  <a name="BKMK_DeployWSERole"></a>Implantar a função de experiência do Windows Server Essentials no Windows Server 2012 R2 Standard e Datacenter  
 Você pode usar o Gerenciador do servidor para habilitar e definir a função de experiência do Windows Server Essentials no Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter usando o procedimento a seguir.  
  
##### <a name="to-deploy-the-windows-server-essentials-experience-role-in-windows-server-2012-r2"></a>Para implantar a função de experiência do Windows Server Essentials no Windows Server 2012 R2  
  
1.  Faça logon em seu servidor como um administrador local.  
  
2.  Abrir **Gerenciador do servidor**e clique em **adicionar funções e recursos**.  
  
3.  Em **selecionar funções de servidor**, selecione o **a experiência do Windows Server Essentials** função. Na caixa de diálogo, clique em **adicionar recursos**e clique em **próxima**.  
  
4.  Em **recursos**, clique em **próxima**.  
  
5.  Reveja o **a experiência do Windows Server Essentials** descrição da função e clique **próxima**.  
  
6.  Nas páginas que siga, clique em **próxima**e na página de confirmação, clique em **instalar**.  
  
7.  Depois que a instalação for concluída, a experiência do Windows Server Essentials deve estar listada como uma função de servidor no Gerenciador do servidor.  
  
8.  Na área de notificação de sinalizador no Gerenciador do servidor, clique no sinalizador e, em seguida, clique em **configurar o Windows Server Essentials**.  
  
9. (Opcional) Altere o nome do servidor, se necessário.  
  
    > [!IMPORTANT]
    >  Você não pode alterar o nome do servidor depois que você configurou o Windows Server Essentials.  
  

10. Siga o Assistente para configurar o Windows Server Essentials conforme descrito anteriormente no [Implantando o Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) seção.  

10. Siga o Assistente para configurar o Windows Server Essentials conforme descrito anteriormente no [Implantando o Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) seção.  

  
##  <a name="BKMK_ExistingAD"></a>Implantar o Windows Server Essentials em um ambiente existente do Active Directory  
 Você também pode implantar o Windows Server Essentials se sua organização já tem um ambiente existente do Active Directory. Além disso, você pode escolher se deseja implantar o Windows Server Essentials como um controlador de domínio.  
  
> [!IMPORTANT]
>  Essa opção só estará disponível se você implantar a função de experiência do Windows Server Essentials no Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter.  
  
#### <a name="to-deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a>Para implantar o Windows Server Essentials em um ambiente existente do Active Directory  
  
1.  (Opcional) Altere o nome do servidor, se necessário.  
  
    > [!IMPORTANT]
    >  Você não pode alterar o nome do servidor depois que você configurou o Windows Server Essentials.  
  
2.  Participe de servidor que executa o Windows Server Essentials em um domínio existente da seguinte maneira:  
  
    1.  Se você quiser que esse servidor para ser um controlador de domínio, configurar o servidor como um controlador de domínio duplicado.  
  
    2.  Se não quiser que este servidor seja um controlador de domínio, adicione este servidor ao domínio usando as ferramentas nativas do Windows.  
  
3.  Reiniciar o servidor e faça logon no servidor como um administrador de domínio.  
  
4.  Abra o Gerenciador do servidor e, em seguida, clique em **adicionar funções e recursos**.  
  
5.  Nas páginas que siga, clique em **próxima**.  
  
6.  Em **selecionar funções de servidor**, selecione **a experiência do Windows Server Essentials**. Na caixa de diálogo, clique em **adicionar recursos**e clique em **próxima**.  
  
7.  Em **recursos**, clique em **próxima**.  
  
8.  Reveja o **a experiência do Windows Server Essentials** descrição e clique em **próxima**.  
  
9. Nas páginas que siga, clique em **próxima**e na página de confirmação, clique em **instalar**.  
  
10. Depois que a instalação for concluída, a experiência do Windows Server Essentials será listada como uma função de servidor no Gerenciador do servidor.  
  
11. Na área de notificação sinalizador **Gerenciador do servidor**, clique no sinalizador e, em seguida, clique em **configurar o Windows Server Essentials**.  
  
12. Siga o Assistente para configurar o Windows Server Essentials. Dependendo da configuração do Active Directory, você será informado se você estiver configurando o Windows Server Essentials em um controlador de domínio ou como um membro do domínio. Clique em **configurar** para começar a configuração. O processo de configuração leva cerca de 10 minutos para ser concluída.  
  
##  <a name="BKMK_VirtualWSE"></a>Virtualizar seu ambiente  
  Windows Server Essentials, Windows Server 2012 R2 Standard e Windows Server 2012 R2 Datacenter podem ser executado como máquinas virtuais. Você pode executar máquinas virtuais usando as ferramentas de gerenciamento do Hyper-V em um servidor que executa o Hyper-V. De uma perspectiva de licenciamento, o Windows Server Essentials permite que você configurar a função Hyper-V e virtualizar seu ambiente. A licença permite configurar outro sistema operacional convidado que está executando o Windows Server Essentials. Dependendo de seu provedor de sistema "¢ s configuração, Windows Server Essentials permite que você configurar um ambiente virtualizado perfeitamente.  
  
#### <a name="to-deploy-windows-server-essentials-as-a-virtual-machine"></a>Para implantar o Windows Server Essentials como uma máquina virtual  
  
1.  Após a página boas-vindas do Windows (dependendo do seu provedor "¢ s configuração do sistema), o **antes de começar** página fornece uma opção para configurar o Windows Server Essentials como uma instância virtual ou em um hardware físico. A disponibilidade dessas opções é predefinida pelo fornecedor do seu sistema e as duas opções não sempre estejam disponíveis. Para instalar o Windows Server Essentials como uma máquina virtual, em **instalar o Windows Server Essentials**, selecione **instalar como instância virtual**e clique em **configurar**.  
  
2.  O assistente irá provisionar uma máquina virtual que leva cerca de cinco minutos.  
  

3.  Em seguida, configure o Windows Server Essentials conforme descrito anteriormente no [Implantando o Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) seção.  

3.  Em seguida, configure o Windows Server Essentials conforme descrito anteriormente no [Implantando o Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) seção.  

  
##  <a name="BKMK_PowerShell"></a>Instalar e configurar o Windows Server Essentials por meio do Windows PowerShell  
 Você pode automatizar a instalação do Windows Server Essentials usando os cmdlets do Windows PowerShell.  
  
#### <a name="to-install-windows-server-essentials-by-using-windows-powershell"></a>Para instalar o Windows Server Essentials usando o Windows PowerShell  
  
1.  Abra o console do Windows PowerShell em um prompt de comando com privilégios elevados.  
  
2.  Instale a função de experiência do Windows Server Essentials usando o seguinte comando:  
  
    ```  
    Add-WindowsFeature ServerEssentialsRole  
    ```  
  
3.  Executar `Get-Help Start-WssConfigurationService`.  
  
    1.  Execute o seguinte comando para iniciar a configuração para configurar o Windows Server Essentials como um controlador de domínio:  
  
        ```  
        Start-WssConfigurationService -CompanyName "ContosoTest" -DNSName "ContosoTest.com" -NetBiosName "ContosoTest" -ComputerName "YourServerName  œNewAdminCredential $cred  
        ```  
  
    2.  Execute o seguinte comando para iniciar a configuração para configurar o Windows Server Essentials como um membro do domínio existente. Você deve ser um membro do grupo Administradores corporativos e grupo de administradores de domínio no Active Directory para executar essa tarefa:  
  
        ```  
        Start-WssConfigurationService  œCredential <Your Credential>  
  
        ```  
  
4.  Monitore o andamento da instalação usando os seguintes comandos:  
  
    -   Para que o status de instalação exibido na barra de progresso, execute `Get-WssConfigurationStatus  œShowProgress`.  
  
    -   Para obter o progresso de imediato sem a barra de progresso, execute `Get-WssConfigurationStatus`.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Quais são as novidades no Windows Server Essentials](../get-started/what-s-new.md)  
  
-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)  
  
-   [Introdução ao Windows Server Essentials](../get-started/get-started.md)
