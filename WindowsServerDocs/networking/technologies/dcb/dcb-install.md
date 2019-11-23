---
title: Instalar a ponte do Data Center (DCB) no Windows Server ou no cliente
description: Este tópico fornece instruções sobre como instalar a ponte do Data Center no Windows Server ou no Windows Client.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ecb6ef072dd2328a0a45d57d181dca9c2928a30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405782"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Instalar a ponte do Data Center \(DCB\) no Windows Server 2016 ou no Windows 10

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a instalar o DCB no Windows Server 2016 ou no Windows 10.

## <a name="prerequisites-for-using-dcb"></a>Pré-requisitos para usar o DCB

A seguir, os pré-requisitos para configurar e gerenciar o DCB.

### <a name="install-a-compatible-operating-system"></a>Instalar um sistema operacional compatível

Você pode usar os comandos DCB deste guia nos sistemas operacionais a seguir.

- Windows Server (Canal semestral)
- Windows Server 2016
- Windows 10 \(todas as versões\)

Os seguintes sistemas operacionais incluem versões anteriores do DCB que não são compatíveis com os comandos que são usados na documentação do DCB para Windows Server 2016 e Windows 10.

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>Requisitos de hardware

A seguir está uma lista de requisitos de hardware para o DCB.

- O DCB\-adaptador de rede Ethernet compatível\(s\) deve ser instalado em computadores que estão fornecendo o Windows Server 2016 DCB.
- DCB\-opções de hardware compatíveis devem ser implantadas em sua rede.


## <a name="install-dcb-in-windows-server-2016"></a>Instalar o DCB no Windows Server 2016

Você pode usar as seções a seguir para instalar o DCB em um computador que executa o Windows Server 2016.

**Credenciais administrativas**

Para executar esses procedimentos, você deve ser membro de **Administradores**.

### <a name="install-dcb-using-windows-powershell"></a>Instalar o DCB usando o Windows PowerShell

Você pode usar o procedimento a seguir para instalar o DCB usando o Windows PowerShell.

1. Em um computador que executa o Windows Server 2016, clique em **Iniciar**e clique com o botão direito do mouse no ícone do Windows PowerShell. Um menu é exibido. No menu, clique em **mais**e, em seguida, clique em **Executar como administrador**. Se solicitado, digite as credenciais para uma conta que tenha privilégios de administrador no computador. O Windows PowerShell é aberto com privilégios de administrador.
2. Digite o comando a seguir e pressione ENTER.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Instalar o DCB usando o Gerenciador do Servidor

Você pode usar o procedimento a seguir para instalar o DCB usando Gerenciador do Servidor.

>[!NOTE]
>Depois de executar a primeira etapa neste procedimento, a página **antes de começar** do assistente para adicionar funções e recursos não será exibida se você tiver selecionado anteriormente **ignorar esta página por padrão** quando o assistente para adicionar funções e recursos for executado. Se a página **antes de começar** não for exibida, pule da etapa 1 para a etapa 3.

1. No DC1, no Gerenciador do Servidor, clique em **gerenciar**e em **adicionar funções e recursos**. O Assistente para Adicionar Funções e Recursos é aberto.
2. Em **Antes de Começar**, clique em **Avançar**.
3. Em **Selecionar Tipo de Instalação**, verifique se **Instalação baseada em função ou recurso** está marcada e clique em **Avançar**.
4. Em **Selecionar servidor de destino**, verifique se **Selecionar um servidor no pool de servidores** está marcada. Em **Pool de Servidores**, verifique se o computador local está selecionado. Clique em **Avançar**.
5. Em **Selecionar funções de servidor**, clique em **Avançar**.
6. Em **selecionar recursos**, em **recursos**, clique em **ponte de data center**. Uma caixa de diálogo é aberta para perguntar se você deseja adicionar os recursos necessários do DCB. Clique em **Adicionar recursos**.
7. Em **selecionar recursos**, clique em **Avançar**. 
8. 7.In **confirmar seleções de instalação**, clique em **instalar**. A página **progresso da instalação** exibe o status durante o processo de instalação. Depois que a mensagem for exibida informando que a instalação foi bem-sucedida, clique em **fechar**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurar o depurador de kernel para permitir que o QoS \(\) opcional

 Por padrão, os depuradores de kernel bloqueiam NetQos. Independentemente do método usado para instalar o DCB, se você tiver um depurador de kernel instalado no computador, deverá configurar o depurador para permitir que o QoS seja habilitado e configurado executando o comando a seguir.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Instalar o DCB no Windows 10

Você pode executar o procedimento a seguir em um computador com Windows 10.

Para executar esse procedimento, você deve ser membro de **Administradores**.

### <a name="install-dcb"></a>Instalar DCB

1. Clique em **Iniciar**, role para baixo e clique em **sistema Windows**.
2. Clique em **Painel de Controle**. A caixa de diálogo **painel de controle** é aberta.
3. No **painel de controle**, clique em **Exibir por**e, em seguida, clique em **ícones grandes** ou **ícones pequenos**.
4. Clique em **programas e recursos**. A caixa de diálogo programas e recursos é aberta.
5. Em **programas e recursos**, no painel esquerdo, clique em **Ativar ou desativar recursos do Windows**. A caixa de diálogo **recursos do Windows** é aberta.
6. Em **recursos do Windows**, clique em **ponte do Data Center**e, em seguida, clique em **OK**.

![Caixa de diálogo ativar ou desativar recursos do Windows](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


