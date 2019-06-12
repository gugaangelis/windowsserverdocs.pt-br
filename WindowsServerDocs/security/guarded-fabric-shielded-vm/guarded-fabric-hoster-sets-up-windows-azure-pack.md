---
title: VMs blindadas – provedor de serviços de hospedagem configura o Pacote do Microsoft Azure
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d528c689-58b0-425c-9740-25e2553ed689
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 156832087bc7af0c95a92cab9a0c1501264d47a5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447506"
---
# <a name="shielded-vms---hosting-service-provider-sets-up-windows-azure-pack"></a>VMs blindadas – provedor de serviços de hospedagem configura o Pacote do Microsoft Azure

Este tópico descreve como um provedor de serviços de hospedagem pode configurar o Windows Azure Pack para que os locatários podem usá-lo para implantar VMs blindadas. Pacote do Windows Azure é um portal da web que estende a funcionalidade do System Center Virtual Machine Manager para permitir que os locatários implantar e gerenciar suas próprias VMs por meio de uma interface da web simples. Windows Azure Pack totalmente dá suporte a VMs blindadas e facilita ainda mais para seus locatários criar e gerenciar seus arquivos de dados de blindagem.

Para entender como este tópico se encaixa no processo geral de implantação de VMs blindadas, consulte [hospedar as etapas de configuração de provedor de serviço para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="setting-up-windows-azure-pack"></a>Configuração do Windows Azure Pack

Você concluirá as seguintes tarefas para configurar o Windows Azure Pack em seu ambiente:

1. Conclua a configuração do System Center 2016 - Virtual Machine Manager (VMM) para sua malha de hospedagem. Isso inclui a configuração de modelos de VM e uma nuvem VM, que será exposta por meio do Windows Azure Pack:

    [Cenário - implantar hosts protegidos e máquinas virtuais blindadas no VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview)

2. Instalar e configurar o System Center 2016 – Service Provider Foundation (SPF). Esse software permite que o Windows Azure Pack para se comunicar com servidores do VMM:

    [Implantando o Service Provider Foundation - SPF](https://technet.microsoft.com/system-center-docs/spf/deploy/deploy-spf)

3. Instalar o Windows Azure Pack e configurá-lo para se comunicar com SPF:

    - [Instalar o Windows Azure Pack](#install-windows-azure-pack) (neste tópico)
    - [Configurar o Windows Azure Pack](#configure-windows-azure-pack) (neste tópico)

4. Crie um ou mais planos de hospedagem no Windows Azure Pack para permitir acesso a suas nuvens VM de locatários:

    [Criar um plano no Windows Azure Pack](#create-a-plan-in-windows-azure-pack) (neste tópico)

## <a name="install-windows-azure-pack"></a>Instalar o Windows Azure Pack

Instalar e configurar o Windows Azure Pack (WAP) no computador em que você deseja hospedar o portal da web para seus locatários. Esta máquina precisará ser capaz de acessar o servidor SPF e ser acessível por seus locatários.

1.  Revisando [requisitos de sistema do WAP](https://technet.microsoft.com/library/dn296442.aspx) e instale o [software de pré-requisito](https://technet.microsoft.com/library/dn469335.aspx).

2.  Baixe e instale o [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Se o computador não estiver conectado à Internet, execute as [instruções de instalação offline](http://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release).

3.  Abra o Web Platform Installer e encontrar **Windows Azure Pack: Portal e API Express** sob o **produtos** guia. Clique em **Add**, em seguida, **instalar** na parte inferior da janela.

4.  Continue com a instalação. Depois que a instalação for concluída, o site de configuração (*https://&lt;wapserver&gt;: 30101 /* ) é aberto no navegador da web. Neste site, fornecem informações sobre o SQL server e concluir a configuração do WAP.

Para obter ajuda sobre como configurar Windows Azure Pack, consulte [instalar uma implantação expressa do Windows Azure Pack](https://technet.microsoft.com/dn296439.aspx).

> [!NOTE]
> Se você executar o pacote do Windows Azure já em seu ambiente, você pode usar sua instalação existente. Para trabalhar com a versão mais recente blindado recursos de VM, no entanto, será necessário atualizar sua instalação pelo menos 10 de Rollup de atualização.

### <a name="configure-windows-azure-pack"></a>Configurar o Windows Azure Pack

Antes de usar o Windows Azure Pack, você já deve ter instalado e configurado para sua infraestrutura.

1.  Navegue até o portal de administração do Windows Azure Pack em *https://&lt;wapserver&gt;: 30091*e, em seguida, faça logon usando suas credenciais de administrador.

2.  No painel esquerdo, clique em **nuvens de VM**.

3.  Conecte-se o pacote do Windows Azure para a instância do Service Provider Foundation clicando **registrar System Center Service Provider Foundation**. Você precisará especificar a URL do Service Provider Foundation, bem como um nome de usuário e senha.

    ![Registrar System Center Service Provider Foundation](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-01-register-spf.png)

4.  Depois de concluído, você deve ser capaz de ver as nuvens de VM configurados no seu ambiente do VMM. Verifique se que você tem pelo menos uma nuvem VM que dá suporte a de VMs blindadas disponíveis para WAP antes de continuar.

### <a name="create-a-plan-in-windows-azure-pack"></a>Criar um plano no Windows Azure Pack

Para permitir que os locatários criar VMs no WAP, crie primeiro um plano de hospedagem que locatários podem assinar. Planos de definem as nuvens de VM permitidos, modelos, redes e entidades de cobrança para seus locatários.

1. No painel inferior do portal, clique em **+ novo** &gt; **planejar** &gt; **criar plano**.

2. Na primeira etapa do assistente, escolha um nome para o seu plano. Esse é o nome que seus locatários verá durante a assinatura.

3. Na segunda etapa, selecione **NUVENS da máquina VIRTUAL** como um dos serviços para oferecer no plano.

4. Ignore a etapa sobre como selecionar quaisquer complementos para o plano.

5. Clique em **Okey** (marca de seleção) para criar o plano. Embora isso cria o plano, ele ainda não está em um estado configurado.

   ![Planos no Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-02-create-plan.png)

6. Para começar a configurar o plano, clique em seu nome.

7. Na próxima página, sob **serviços do plano**, clique em **nuvens da máquina Virtual**. Isso abre a página onde você pode configurar as cotas para este plano.

8. Sob **básica**, selecione o servidor de gerenciamento do VMM e a nuvem de máquina Virtual que você deseja oferecer aos locatários. As nuvens que podem oferecer as VMs blindadas serão exibidas com o **(suporte de blindagem)** ao lado do nome.

9. Selecione as cotas que você deseja aplicar neste plano. (Por exemplo, limites de núcleo da CPU e uso de RAM). Certifique-se de deixar o **permitir que máquinas virtuais para blindar** caixa de seleção marcada.

   ![Configurações para nuvens de máquina virtual no Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-03-virtual-machine-clouds.png)
    
10. Role para baixo até a seção intitulada **modelos**e, em seguida, selecione um ou mais modelos para oferecer a seus locatários. Você pode oferecer ambos blindado e modelos não blindados para locatários, mas um modelo blindado deve ser oferecidos para dar garantias de ponta a ponta sobre a integridade da VM e seus segredos de locatários.

11. No **redes** seção, adicione uma ou mais redes para seus locatários.

12. Depois de definir outras configurações ou cotas para o plano, clique em **salvar** na parte inferior.

13. Na parte superior esquerda da tela, clique na seta para levá-lo de volta para o **planejar** página.

14. Na parte inferior da tela, altere o plano de que está sendo **privados** ao **público** , de modo que os locatários podem assinar o plano.

    ![Alterar o acesso para um plano no Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-host-azure-pack-04-change-access.png)

    Neste ponto, o Windows Azure Pack está configurado e locatários poderão assinar o plano que você acabou de criar e implantar VMs blindadas. Para obter as etapas adicionais que os locatários precisam para concluir, consulte [VMs Blindadas para locatários – Implantando uma VM blindada usando o Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md).

## <a name="see-also"></a>Consulte também

- [Etapas de configuração de provedor de serviço de hospedagem de hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
