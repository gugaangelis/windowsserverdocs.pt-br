---
title: Instalar e configurar o Windows Server Essentials ou a Experiência do Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844407"
---
# <a name="install-and-configure-windows-server-essentials-or-windows-server-essentials-experience"></a>Instalar e configurar o Windows Server Essentials ou a Experiência do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server Essentials é um primeiro servidor ideal para pequenas empresas com até 25 usuários e 50 dispositivos. Para organizações com até 100 usuários e 200 dispositivos, agora você pode usar o Windows Server 2012 R2 com a função experiência Windows Server Essentials instalada. Este tópico aborda os dois cenários.  
  
Experiência do Windows Server Essentials é uma função no Windows Server 2016 que permite que você tire proveito de todos os recursos (por exemplo, o acesso via Web remoto e backup de computador) que estão disponíveis no Windows Server Essentials sem os bloqueios e limites que são impostos em  Windows Server Essentials. Essa função de servidor também está disponível no Windows Server Essentials e é habilitada por padrão.
  
Antes de instalar o Windows Server Essentials ou a função experiência do Essentials, observe as seguintes limitações.  
  
|Experiência do Windows Server Essentials no Windows Server Essentials|Experiência do Windows Server Essentials no Windows Server 2016
|----|----|
|-Deve ser o controlador de domínio na raiz da floresta e domínio e deve conter todas as funções FSMO.<br /><br /> -Não pode ser instalado em um ambiente com um domínio do Active Directory pré-existente (no entanto, há um período de cortesia de 21 dias para executar migrações).|-Não precisa ser um controlador de domínio se ele estiver instalado em um ambiente com um domínio do Active Directory já existente.<br /><br /> – Se não existir um domínio do Active Directory, instalar a função criará um domínio do Active Directory e o servidor se tornará o controlador de domínio na raiz da floresta e domínio, mantendo todas as funções FSMO.  
|Só pode ser implantado em um único domínio.|Só pode ser implantado em um único domínio.  
|Um controlador de domínio somente leitura não pode existir no seu domínio.|Um controlador de domínio somente leitura não pode existir no seu domínio.

> [!NOTE]
>  Para baixar as versões de avaliação dos sistemas operacionais, visite o Centro de Avaliação TechNet:  
>   
>  [Baixe o Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)  
>   
>  [Baixe o Windows Server Essentials](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016-essentials)
  
## <a name="installation-options"></a>Opções de instalação  
 Este documento fornece instruções passo a passo para instalar e configurar o Windows Server Essentials. Dependendo do ambiente de rede, você tem as seguintes opções de instalação disponíveis para você:  
  
-    Windows Server Essentials (com a função experiência Windows Server Essentials habilitada por padrão)  
  
-    Windows Server 2016 com a função experiência Windows Server Essentials instalada  
 
|Ambiente de implantação|Descrição|Seção relacionada|  
|----------------------------|-----------------|---------------------|  
|Novo ambiente do Active Directory|Você pode instalar o Windows Server Essentials para criar um novo ambiente do Active Directory.|[Implantar o Windows Server Essentials para configurar um novo ambiente do Active Directory](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_NewAD)|  
|Ambiente existente do Active Directory|Você pode instalar o Windows Server Essentials em um ambiente existente do Active Directory.|[Implantar o Windows Server Essentials em um ambiente existente do Active Directory](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_ExistingAD)|  
|Ambiente virtual|Você pode implantar o Windows Server Essentials como uma máquina virtual.|[Virtualizar seu ambiente](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_VirtualWSE)|  
|Implantação automatizada|Você pode automatizar a implantação do Windows Server Essentials usando o Windows PowerShell.|[Instalar e configurar o Windows Server Essentials usando o Windows PowerShell](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_PowerShell)|  
  
## <a name="before-you-begin"></a>Antes de começar  
 Antes de começar a instalação, examine a documentação a seguir:  
  
-   [Visão geral do produto Windows Server Essentials](https://www.microsoft.com/server-cloud/windows-server-essentials/windows-server-2012-r2-essentials.aspx)  
  

-   [Requisitos de sistema do Windows Server Essentials](../get-started/system-requirements.md)   

  
##  <a name="BKMK_NewAD"></a> Implantar o Windows Server Essentials para configurar um novo ambiente do Active Directory  
 O Windows Server Essentials fornece uma maneira de configurar um ambiente do Active Directory e recursos do servidor relacionados rapidamente.  
  
###  <a name="BKMK_WSEDeploy"></a> Implantando o Windows Server Essentials  
 Se você estiver usando o Windows Server Essentials, a experiência do Windows Server Essentials já está habilitada. No entanto, você deve concluir algumas etapas para configurar o servidor.  
  
##### <a name="to-configure-windows-server-essentials-on-a-physical-server"></a>Para configurar o Windows Server Essentials em um servidor físico  
  
1.  Após a página **Boas-vindas** do Windows, o **Assistente Configurar o Windows Server Essentials** está visível na área de trabalho.  
  
2.  Siga as instruções para concluir o assistente a seguir:  
  
    1.  Na página **Configurar o Windows Server Essentials** , clique em **Avançar**.  
  
    2.  Em **Configurações de data/hora**, verifique se a data, a hora e o fuso horário estão corretas e clique em **Avançar**.  
  
    3.  Em **Informações da Empresa**, digite o nome da empresa, como **Contoso,Ltd.** e clique em **Avançar**. Opcionalmente, você pode alterar o nome do domínio interno e o nome de servidor.  
  
    4.  Em **Criar um administrador de rede**, digite um novo nome de conta de administrador e uma senha.  
  
        > [!NOTE]
        >  Não use nome de conta de **Administrador** e senha padrões.  
  
    5.  Clique em **Configurar**.  
  
3.  O servidor será reiniciado várias vezes durante o processo de configuração e seus logons serão automáticos até que a configuração seja concluída. Esse processo leva cerca de 20 minutos.  
  
4.  Na área de trabalho, clique no ícone do Painel para iniciar o Painel do servidor. Na página **Início**, execute as tarefas **Introdução** listadas na guia **Instalação**.  
  
 Depois de concluir a configuração do servidor, o servidor que está executando o Windows Server Essentials será configurado como um controlador de domínio.  
  
###  <a name="BKMK_DeployWSERole"></a> Implantando a função experiência Windows Server Essentials no Windows Server 2012 R2 Standard e Datacenter  
 Você pode usar o Gerenciador do servidor para habilitar e configurar a função experiência do Windows Server Essentials no Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter usando o procedimento a seguir.  
  
##### <a name="to-deploy-the-windows-server-essentials-experience-role-in-windows-server-2012-r2"></a>Para implantar a função da Experiência do Windows Server Essentials no Windows Server 2012 R2  
  
1.  Faça logon no seu servidor como um administrador local.  
  
2.  Abra **Gerenciador do Servidor**e clique em **Adicionar Funções e Recursos**.  
  
3.  Em **Selecionar funções de servidor**, selecione a função **Experiência do Windows Server Essentials** . Na caixa de diálogo, clique em **Adicionar recursos**e clique em **Avançar**.  
  
4.  Em **Recursos**, clique em **Avançar**.  
  
5.  Examine a descrição da função **Experiência do Windows Server Essentials** e clique em **Avançar**.  
  
6.  Nas páginas a seguir, clique em **Avançar** e, na página de confirmação, clique em **Instalar**.  
  
7.  Depois que a instalação for concluída, a experiência do Windows Server Essentials deve estar listada como uma função de servidor no Gerenciador do servidor.  
  
8.  Na área de notificação do sinalizador no Gerenciador do Servidor, clique no sinalizador e, em seguida, clique em **Configurar o Windows Server Essentials**.  
  
9. (Opcional) Se necessário, altere o nome do servidor.  
  
    > [!IMPORTANT]
    >  Não é possível alterar o nome do servidor depois que você tiver configurado o Windows Server Essentials.  
  

10. Siga o Assistente para configurar o Windows Server Essentials conforme descrito anteriormente na [Deploying Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) seção.  

10. Siga o Assistente para configurar o Windows Server Essentials conforme descrito anteriormente na [Deploying Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) seção.  

  
##  <a name="BKMK_ExistingAD"></a> Implantar o Windows Server Essentials em um ambiente existente do Active Directory  
 Você também pode implantar o Windows Server Essentials se sua organização já tiver um ambiente existente do Active Directory. Além disso, você pode escolher se deseja implantar o Windows Server Essentials como um controlador de domínio.  
  
> [!IMPORTANT]
>  Essa opção só estará disponível se você implantar a função experiência Windows Server Essentials no Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter.  
  
#### <a name="to-deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a>Implante o Windows Server Essentials em um ambiente existente do Active Directory.  
  
1.  (Opcional) Se necessário, altere o nome do servidor.  
  
    > [!IMPORTANT]
    >  Não é possível alterar o nome do servidor depois que você tiver configurado o Windows Server Essentials.  
  
2.  Ingresse o servidor que executa o Windows Server Essentials em um domínio existente da seguinte maneira:  
  
    1.  Se desejar que este servidor seja um controlador de domínio, configure o servidor como um controlador de domínio de réplica.  
  
    2.  Se você não quiser que este servidor seja um controlador de domínio, adicione este servidor ao domínio usando as ferramentas nativas do Windows.  
  
3.  Reinicie o servidor e faça logon no servidor como um administrador de domínio.  
  
4.  Abra o Gerenciador do Servidor e clique em **Adicionar Funções e Recursos**.  
  
5.  Nas páginas a seguir, clique em **Avançar**.  
  
6.  Em **Selecionar funções do servidor**, selecione **Experiência do Windows Server Essentials**. Na caixa de diálogo, clique em **Adicionar recursos**e clique em **Avançar**.  
  
7.  Em **Recursos**, clique em **Avançar**.  
  
8.  Examine a descrição da função **Experiência do Windows Server Essentials** e clique em **Avançar**.  
  
9. Nas páginas a seguir, clique em **Avançar** e, na página de confirmação, clique em **Instalar**.  
  
10. Depois que a instalação for concluída, a experiência do Windows Server Essentials será listada como uma função de servidor no Gerenciador do servidor.  
  
11. Na área de notificação do sinalizador no **Gerenciador do Servidor**, clique no sinalizador e, em seguida, clique em **Configurar o Windows Server Essentials**.  
  
12. Execute o Assistente para configurar o Windows Server Essentials. Dependendo da configuração do Active Directory, você será informado se você estiver configurando o Windows Server Essentials em um controlador de domínio ou como um membro do domínio. Clique em **Configurar** para começar a configuração. O processo de configuração leva aproximadamente 10 minutos para ser concluído.  
  
##  <a name="BKMK_VirtualWSE"></a> Virtualizar seu ambiente  
  Windows Server Essentials, Windows Server 2012 R2 Standard e Datacenter do Windows Server 2012 R2 podem ser executados como máquinas virtuais. Você executa máquinas virtuais usando as ferramentas de gerenciamento do Hyper-V em um servidor executando o Hyper-V. Da perspectiva de licenciamento, o Windows Server Essentials permite que você configure a função Hyper-V e virtualizar seu ambiente. A licença permite que você configure outro sistema operacional convidado que está executando o Windows Server Essentials. Dependendo do provedor do sistema "configuração do relatório s, o Windows Server Essentials permite que você configurar um ambiente virtualizado perfeitamente.  
  
#### <a name="to-deploy-windows-server-essentials-as-a-virtual-machine"></a>Você pode implantar o Windows Server Essentials como uma máquina virtual.  
  
1.  Após a página de boas-vindas do Windows (dependendo do seu provedor "relatório s configuração do sistema), o **antes de começar** página fornece uma opção para configurar o Windows Server Essentials como uma instância virtual ou no hardware físico. A disponibilidade dessas opções é predefinida pelo provedor do seu sistema e as nem sempre as duas opções podem estar disponíveis. Para instalar o Windows Server Essentials como uma máquina virtual, na **instalar o Windows Server Essentials**, selecione **instalar como instância virtual**e, em seguida, clique em **configurar**.  
  
2.  O assistente provisionará uma máquina virtual que leva cerca de cinco minutos.  
  

3.  Em seguida, configure o Windows Server Essentials conforme descrito anteriormente na [Deploying Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) seção.  

3.  Em seguida, configure o Windows Server Essentials conforme descrito anteriormente na [Deploying Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) seção.  

  
##  <a name="BKMK_PowerShell"></a> Instalar e configurar o Windows Server Essentials usando o Windows PowerShell  
 Você pode automatizar a instalação do Windows Server Essentials usando cmdlets do Windows PowerShell.  
  
#### <a name="to-install-windows-server-essentials-by-using-windows-powershell"></a>Para instalar o Windows Server Essentials usando o Windows PowerShell  
  
1.  Abra o console do Windows PowerShell em um prompt de comandos com privilégios elevados.  
  
2.  Instale a função experiência do Windows Server Essentials usando o comando a seguir:  
  
    ```  
    Add-WindowsFeature ServerEssentialsRole  
    ```  
  
3.  Execute `Get-Help Start-WssConfigurationService`.  
  
    1.  Execute o seguinte comando para iniciar a configuração do Windows Server Essentials como um controlador de domínio:  
  
        ```  
        Start-WssConfigurationService -CompanyName "ContosoTest" -DNSName "ContosoTest.com" -NetBiosName "ContosoTest" -ComputerName "YourServerName  œNewAdminCredential $cred  
        ```  
  
    2.  Execute o seguinte comando para iniciar a configuração do Windows Server Essentials como um membro de domínio existente. Você deve ser membro do grupo Admin Corporativo e do grupo Admin do Domínio no Active Directory para executar essa tarefa:  
  
        ```  
        Start-WssConfigurationService  œCredential <Your Credential>  
  
        ```  
  
4.  Monitore o progresso da instalação usando os seguintes comandos:  
  
    -   Para que o status da instalação seja exibido na barra de progresso, execute `Get-WssConfigurationStatus  œShowProgress`.  
  
    -   Para obter o progresso imediato sem a barra de progresso, execute `Get-WssConfigurationStatus`.  
  
## <a name="see-also"></a>Consulte também  
  
-   [O que há de novo no Windows Server Essentials](../get-started/what-s-new.md)  
  
-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)  
  
-   [Introdução ao Windows Server Essentials](../get-started/get-started.md)
