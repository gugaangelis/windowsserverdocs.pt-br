---
title: Instalar a função Hyper-V no Windows Server
description: Fornece instruções para instalar o Hyper-V usando o Gerenciador do Servidor ou o Windows PowerShell
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: get-started-article
ms.assetid: 8e871317-09d2-4314-a6ec-ced12b7aee89
author: kbdazure
ms.author: kathydav
ms.date: 12/02/2016
ms.openlocfilehash: 5bd77284fd73d75075cec307e989274c86552209
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475643"
---
# <a name="install-the-hyper-v-role-on-windows-server"></a>Instalar a função Hyper-V no Windows Server

>Aplica-se a: Windows Server 2016, Windows Server 2019

Para criar e executar máquinas virtuais, instale a função Hyper-V no Windows Server usando Gerenciador do Servidor ou o cmdlet **install-WindowsFeature** no Windows PowerShell.
Para o Windows 10, consulte [instalar o Hyper-V no Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

Para saber mais sobre o Hyper-V, consulte [visão geral da tecnologia Hyper-v](../Hyper-V-Technology-Overview.md). Para experimentar o Windows Server 2019, você pode baixar e instalar uma cópia de avaliação. Consulte o [centro de avaliação](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019).

Antes de instalar o Windows Server ou adicionar a função Hyper-V, verifique se:
- O hardware do seu computador é compatível. Para obter detalhes, consulte [requisitos do sistema para o Windows Server](../../../get-started/System-Requirements.md) e [requisitos do sistema para o Hyper-V no Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).
- Você não planeja usar aplicativos de virtualização de terceiros que dependem dos mesmos recursos de processador que o Hyper-V exige. Os exemplos incluem VMWare workstation e VirtualBox. Você pode instalar o Hyper-V sem desinstalar esses outros aplicativos. Mas, se você tentar usá-las para gerenciar máquinas virtuais quando o hipervisor do Hyper-V estiver em execução, as máquinas virtuais podem não ser iniciadas ou podem ser executadas de forma não confiável. Para obter detalhes e instruções para desativar o hipervisor do Hyper-V se você precisar usar um desses aplicativos, consulte [aplicativos de virtualização não funcionam junto com o Hyper-v, o Device Guard e o Credential Guard](https://support.microsoft.com/help/3204980/virtualization-applications-do-not-work-together-with-hyper-v-device-g).

Se você quiser instalar apenas as ferramentas de gerenciamento, como o Gerenciador do Hyper-V, consulte [gerenciar remotamente hosts Hyper-v com o Gerenciador do Hyper-v](../Manage/Remotely-manage-Hyper-V-hosts.md).

## <a name="install-hyper-v-by-using-server-manager"></a>Instalar o Hyper-V usando o Gerenciador do Servidor

1. Em **Gerenciador do Servidor**, no menu **Gerenciar** clique em **Adicionar Funções e Recursos**.

2. Na página **Antes de começar**, verifique se o servidor de destino e o ambiente de rede estão preparados para a função e o recurso que você vai instalar. Clique em **Avançar**.

3. Na página **Selecionar tipo de instalação**, selecione **Instalação baseada em função ou recurso** e em **Avançar**.

4. Na página **Selecionar servidor de destino**, escolha um servidor no pool de servidores e clique em **Avançar**.

5. Na página **Selecionar funções do servidor**, selecione **Hyper-V**.

6. Para adicionar as ferramentas usadas para criar e gerenciar máquinas virtuais, clique em **Adicionar Recursos**. Na página Recursos, clique em **Avançar**.

7. Nas páginas **Criar Comutadores Virtuais**, **Migração de Máquina Virtual** e **Repositórios Padrão**, selecione as opções apropriadas.

8. Na página **Confirmar seleções de instalação**, selecione **Reiniciar cada servidor de destino automaticamente, se necessário** e clique em **Instalar**.

9. Quando a instalação for concluída, verifique se o Hyper-V foi instalado corretamente. Abra a página **todos os servidores** em Gerenciador do servidor e selecione um servidor no qual você instalou o Hyper-V. Verifique o bloco **funções e recursos** na página do servidor selecionado.

## <a name="install-hyper-v-by-using-the-install-windowsfeature-cmdlet"></a>Instalar o Hyper-V usando o cmdlet Install-WindowsFeature

1. Na área de trabalho do Windows, clique no botão Iniciar e digite qualquer parte do nome **Windows PowerShell**.

2. Clique com o botão direito do mouse em Windows PowerShell e selecione **Executar como administrador**.

3. Para instalar o Hyper-V em um servidor ao qual você está conectado remotamente, execute o comando a seguir e substitua `<computer_name>` pelo nome do servidor.

    ```powershell
    Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart
    ```

    Se você estiver conectado localmente ao servidor, execute o comando sem `-ComputerName <computer_name>` .

4. Depois que o servidor for reiniciado, você poderá ver que a função Hyper-V está instalada e ver quais outras funções e recursos estão instalados executando o seguinte comando:

    ```powershell
    Get-WindowsFeature -ComputerName <computer_name>
    ```

    Se você estiver conectado localmente ao servidor, execute o comando sem `-ComputerName <computer_name>` .

> [!NOTE]
> Se você instalar essa função em um servidor que executa a opção de instalação Server Core do Windows Server 2016 e usar o parâmetro `-IncludeManagementTools` , somente o módulo Hyper-V para Windows PowerShell será instalado. Você pode usar a ferramenta de gerenciamento de GUI, o Gerenciador do Hyper-V, em outro computador para gerenciar remotamente um host Hyper-V executado em uma instalação do Server Core. Para obter instruções sobre como se conectar remotamente, consulte [gerenciar remotamente hosts Hyper-v com o Gerenciador do Hyper-v](../Manage/Remotely-manage-Hyper-V-hosts.md).

## <a name="additional-references"></a>Referências adicionais

- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/Microsoft.Windows.ServerManager.Migration/Install-WindowsFeature)
