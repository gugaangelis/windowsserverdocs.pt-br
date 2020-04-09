---
title: VMs blindadas – provedor de serviços de hospedagem configura o Pacote do Microsoft Azure
ms.prod: windows-server
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 1d759af575f98d305a67734d0e23680f701f6b72
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856709"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>VMs blindadas – provedor de serviços de hospedagem configura o Pacote do Microsoft Azure

Este tópico descreve como um provedor de serviços de hospedagem pode configurar Pacote do Microsoft Azure para que os locatários possam usá-lo para implantar VMs blindadas. Pacote do Microsoft Azure é um portal da Web que estende a funcionalidade do System Center Virtual Machine Manager para permitir que os locatários implantem e gerenciem suas próprias VMs por meio de uma interface da Web simples. Pacote do Microsoft Azure dá suporte total a VMs blindadas e torna ainda mais fácil para seus locatários criar e gerenciar seus arquivos de dados de blindagem.

Para entender como este tópico se encaixa no processo geral de implantação de VMs blindadas, consulte [as etapas de configuração do provedor de serviço de hospedagem para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="setting-up-windows-azure-pack"></a>Configurando Pacote do Microsoft Azure

Você concluirá as seguintes tarefas para configurar Pacote do Microsoft Azure em seu ambiente:

1. Conclua a configuração do System Center 2016-Virtual Machine Manager (VMM) para sua malha de hospedagem. Isso inclui a configuração de modelos de VM e uma nuvem de VM, que serão expostos por meio de Pacote do Microsoft Azure:

    [Cenário-implantar hosts protegidos e máquinas virtuais blindadas no VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview)

2. Instalar e configurar o System Center 2016-Service Provider Foundation (SPF). Este software permite que Pacote do Microsoft Azure se comunique com seus servidores do VMM:

    [Implantando Service Provider Foundation-SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. Instale o Pacote do Microsoft Azure e configure-o para se comunicar com o SPF:

    - [Instalar pacote do Microsoft Azure](#install-windows-azure-pack) (neste tópico)
    - [Configurar pacote do Microsoft Azure](#configure-windows-azure-pack) (neste tópico)

4. Crie um ou mais planos de hospedagem no Pacote do Microsoft Azure para permitir que locatários acessem suas nuvens de VM:

    [Criar um plano no pacote do Microsoft Azure](#create-a-plan-in-windows-azure-pack) (neste tópico)

## <a name="install-windows-azure-pack"></a>Instalar Pacote do Microsoft Azure

Instale e configure o Pacote do Microsoft Azure (WAP) no computador em que você deseja hospedar o portal da Web para seus locatários. Este computador precisará ser capaz de acessar o servidor SPF e ser acessível por seus locatários.

1.  Revisão [dos requisitos do sistema WAP](https://technet.microsoft.com/library/dn296442.aspx) e instalação do [software de pré-requisito](https://technet.microsoft.com/library/dn469335.aspx).

2.  Baixe e instale o [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Se o computador não estiver conectado à Internet, siga as [instruções de instalação offline](https://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release).

3.  Abra o Web Platform Installer e localize **pacote do Microsoft Azure: portal e API Express** na guia **produtos** . clique em **Adicionar**e **Instale** na parte inferior da janela.

4.  Continue com a instalação. Após a conclusão da instalação, o site de configuração (*https://&lt;wapserver&gt;: 30101/* ) é aberto no navegador da Web. Neste site, forneça informações sobre o SQL Server e conclua a configuração de WAP.

Para obter ajuda sobre como configurar Pacote do Microsoft Azure, consulte [instalar uma implantação expressa do pacote do Microsoft Azure](https://technet.microsoft.com/dn296439.aspx).

> [!NOTE]
> Se você já tiver executado Pacote do Microsoft Azure em seu ambiente, poderá usar a instalação existente. No entanto, para trabalhar com os recursos mais recentes da VM blindada, você precisará atualizar sua instalação para pelo menos o pacote cumulativo de atualizações 10.

### <a name="configure-windows-azure-pack"></a>Configurar Pacote do Microsoft Azure

Antes de usar Pacote do Microsoft Azure, você já deve tê-lo instalado e configurado para sua infraestrutura.

1.  Navegue até o portal de administração do Pacote do Microsoft Azure em *https://&lt;wapserver&gt;: 30091*e, em seguida, faça logon usando suas credenciais de administrador.

2.  No painel esquerdo, clique em **nuvens de VM**.

3.  Conecte Pacote do Microsoft Azure à instância de Service Provider Foundation clicando em **registrar System Center Service Provider Foundation**. Será necessário especificar a URL para Service Provider Foundation, bem como um nome de usuário e senha.

    ![Registrar System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  Depois de concluído, você poderá ver as nuvens de VM configuradas no seu ambiente do VMM. Verifique se você tem pelo menos uma nuvem de VM que dá suporte a VMs blindadas disponíveis para WAP antes de continuar.

### <a name="create-a-plan-in-windows-azure-pack"></a>Criar um plano no Pacote do Microsoft Azure

Para permitir que os locatários criem VMs em WAP, você deve primeiro criar um plano de hospedagem para o qual os locatários podem assinar. Os planos definem as nuvens de VM permitidas, os modelos, as redes e as entidades de cobrança para seus locatários.

1. No painel inferior do portal, clique em **+ novo** **plano** de &gt; &gt; **criar plano**.

2. Na primeira etapa do assistente, escolha um nome para seu plano. Esse é o nome que seus locatários verão ao assinar.

3. Na segunda etapa, selecione **nuvens de máquina virtual** como um dos serviços a serem oferecidos no plano.

4. Ignore a etapa sobre como selecionar quaisquer Complementos para o plano.

5. Clique em **OK** (marca de seleção) para criar o plano. Embora isso crie o plano, ele ainda não está em um estado configurado.

   ![Planos no Pacote do Microsoft Azure](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6. Para começar a configurar o plano, clique em seu nome.

7. Na página seguinte, em **serviços de plano**, clique em **nuvens de máquina virtual**. Isso abre a página onde você pode configurar cotas para este plano.

8. Em **básico**, selecione o servidor de gerenciamento do VMM e a nuvem da máquina virtual que você deseja oferecer aos seus locatários. As nuvens que podem oferecer VMs blindadas serão exibidas com **(blindagem com suporte)** ao lado do nome.

9. Selecione as cotas que você deseja aplicar neste plano. (Por exemplo, limites de núcleo de CPU e uso de RAM). Lembre-se de deixar a caixa de seleção **permitir que máquinas virtuais sejam blindadas** selecionada.

   ![Configurações para nuvens de máquina virtual no Pacote do Microsoft Azure](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)
    
10. Role para baixo até a seção de **modelos**e selecione um ou mais modelos para oferecer aos seus locatários. Você pode oferecer modelos blindados e não blindados a locatários, mas um modelo blindado deve ser oferecido para dar aos locatários garantias de ponta a ponta sobre a integridade da VM e seus segredos.

11. Na seção **redes** , adicione uma ou mais redes para seus locatários.

12. Depois de definir outras configurações ou cotas para o plano, clique em **salvar** na parte inferior.

13. Na parte superior esquerda da tela, clique na seta para levá-lo de volta à página de **plano** .

14. Na parte inferior da tela, altere o plano de ser **privado** para **público** para que os locatários possam assinar o plano.

    ![Alterar o acesso de um plano no Pacote do Microsoft Azure](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    Neste ponto, Pacote do Microsoft Azure está configurado e os locatários poderão assinar o plano que você acabou de criar e implantar VMs blindadas. Para obter etapas adicionais que os locatários precisam concluir, consulte [VMs blindadas para locatários-implantando uma VM blindada usando pacote do Microsoft Azure](guarded-fabric-shielded-vm-windows-azure-pack.md).

## <a name="see-also"></a>Consulte também

- [Etapas de configuração do provedor de serviços de hospedagem para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
