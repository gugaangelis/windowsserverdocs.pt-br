---
title: Instalar a função Hyper-V no Windows Server
description: Fornece instruções para instalar o Hyper-V usando o Gerenciador do servidor ou o Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 8e871317-09d2-4314-a6ec-ced12b7aee89
author: KBDAzure
ms.author: kathydav
ms.date: 12/02/2016
ms.openlocfilehash: 80154c569701608ad190fb76eb3737578895d187
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812375"
---
# <a name="install-the-hyper-v-role-on-windows-server"></a>Instalar a função Hyper-V no Windows Server

>Aplica-se a: Windows Server 2016, Windows Server 2019
  
Para criar e executar máquinas virtuais, instalar a função Hyper-V no Windows Server usando o Gerenciador do servidor ou o **Install-WindowsFeature** cmdlet do Windows PowerShell. Para Windows 10, consulte [instalar o Hyper-V no Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

Para saber mais sobre o Hyper-V, consulte o [visão geral da tecnologia do Hyper-V](../Hyper-V-Technology-Overview.md). Para experimentar o Windows Server 2019, você pode baixar e instalar uma cópia de avaliação. Consulte a [Centro de avaliação](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019).

Antes de instalar o Windows Server ou adicionar a função Hyper-V, verifique se:
- O hardware do computador é compatível. Para obter detalhes, consulte [requisitos de sistema para o Windows Server](../../../get-started/System-Requirements.md) e [requisitos de sistema do Hyper-V no Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).
- Você não planeja usar aplicativos de virtualização de terceiros que dependem dos mesmos recursos de processador Hyper-V exige. Exemplos incluem o VMWare Workstation e o VirtualBox. Você pode instalar o Hyper-V sem desinstalar esses outros aplicativos. Mas, se você tentar usá-las para gerenciar máquinas virtuais quando o hipervisor Hyper-V está em execução, as máquinas virtuais não pode iniciar ou pode ser executado de modo não confiável. Para obter detalhes e instruções sobre como desativar o hipervisor Hyper-V, se você precisar usar um desses aplicativos, consulte [aplicativos de virtualização não funcionam em conjunto com o Hyper-V, o Device Guard e Credential Guard](https://support.microsoft.com/help/3204980/virtualization-applications-do-not-work-together-with-hyper-v-device-g).

Se você deseja instalar apenas as ferramentas de gerenciamento, como o Gerenciador do Hyper-V, consulte [gerenciar remotamente os hosts do Hyper-V com o Gerenciador do Hyper-V](../Manage/Remotely-manage-Hyper-V-hosts.md).
  
## <a name="install-hyper-v-by-using-server-manager"></a>Instalar o Hyper-V usando o Gerenciador do servidor  
  
1. Em **Gerenciador do Servidor**, no menu **Gerenciar** clique em **Adicionar Funções e Recursos**.  
  
2. Na página **Antes de começar**, verifique se o servidor de destino e o ambiente de rede estão preparados para a função e o recurso que você vai instalar. Clique em **Avançar**.  
  
3. Na página **Selecionar tipo de instalação**, selecione **Instalação baseada em função ou recurso** e em **Avançar**.  
  
4. Na página **Selecionar servidor de destino**, escolha um servidor no pool de servidores e clique em **Avançar**.  
  
5. Na página **Selecionar funções do servidor**, selecione **Hyper-V**.  
  
6. Para adicionar as ferramentas usadas para criar e gerenciar máquinas virtuais, clique em **Adicionar Recursos**. Na página Recursos, clique em **Avançar**.  
  
7. Nas páginas **Criar Comutadores Virtuais**, **Migração de Máquina Virtual** e **Repositórios Padrão**, selecione as opções apropriadas.  
  
8. Na página **Confirmar seleções de instalação**, selecione **Reiniciar cada servidor de destino automaticamente, se necessário** e clique em **Instalar**.  
  
9. Quando a instalação for concluída, verifique se que o Hyper-V foi instalado corretamente. Abra o **todos os servidores** página no Gerenciador do servidor e selecione um servidor no qual você instalou o Hyper-V. Verifique as **funções e recursos** lado a lado na página para o servidor selecionado.  
  
## <a name="install-hyper-v-by-using-the-install-windowsfeature-cmdlet"></a>Instalar o Hyper-V usando o cmdlet Install-WindowsFeature  
  
1. Na área de trabalho do Windows, clique no botão Iniciar e digite qualquer parte do nome **Windows PowerShell**.  
  
2. Windows PowerShell com o botão direito e selecione **executar como administrador**.  
  
3. Para instalar o Hyper-V em um servidor que você está conectado remotamente, execute o seguinte comando e substitua `<computer_name>` com o nome do servidor.  
  
    ```powershell
    Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart  
    ```  
  
    Se você estiver conectado localmente no servidor, execute o comando sem `-ComputerName <computer_name>`.  
  
4. Depois que o servidor for reiniciado, você pode ver que a função Hyper-V está instalado e ver quais outros recursos e funções estão instaladas executando o seguinte comando:  
  
    ```powershell
    Get-WindowsFeature -ComputerName <computer_name>  
    ```  
  
    Se você estiver conectado localmente no servidor, execute o comando sem `-ComputerName <computer_name>`.  
  
> [!NOTE]  
> Se você instalar essa função em um servidor que executa a opção de instalação Server Core do Windows Server 2016 e use o parâmetro `-IncludeManagementTools`, somente o Hyper-V Module para Windows PowerShell está instalado. Você pode usar a ferramenta de gerenciamento de GUI, o Gerenciador do Hyper-V, em outro computador para gerenciar remotamente um host Hyper-V que é executado em uma instalação Server Core. Para obter instruções sobre como se conectar remotamente, consulte [gerenciar remotamente os hosts do Hyper-V com o Gerenciador do Hyper-V](../Manage/Remotely-manage-Hyper-V-hosts.md).  
  
## <a name="see-also"></a>Consulte também  
  
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/Microsoft.Windows.ServerManager.Migration/Install-WindowsFeature)  
