---
title: Instalação do Data Center Bridging (DCB) no Windows Server ou Client
description: Este tópico fornece instruções sobre como instalar o Data Center Bridging no Windows Server ou cliente Windows.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7c20ef027279780181ff176afa39a19f2976c4c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845437"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Instalação do Data Center Bridging \(DCB\) no Windows Server 2016 ou Windows 10

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a instalar o DCB no Windows Server 2016 ou Windows 10.

## <a name="prerequisites-for-using-dcb"></a>Pré-requisitos para usar o DCB

A seguir estão os pré-requisitos para configurar e gerenciar o DCB.

### <a name="install-a-compatible-operating-system"></a>Instalar um sistema operacional compatível

Você pode usar os comandos DCB este guia em sistemas operacionais a seguir.

- Windows Server (canal semestral)
- Windows Server 2016
- Windows 10 \(todas as versões\)

Os seguintes sistemas operacionais incluem versões anteriores do DCB que não são compatíveis com os comandos que são usados na documentação do DCB para Windows Server 2016 e Windows 10.

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>Requisitos de hardware

A seguir está uma lista dos requisitos de hardware do DCB.

- O DCB\-adaptador de rede Ethernet compatíveis com\(s\) deve ser instalado em computadores que fornecem o DCB do Windows Server 2016.
- O DCB\-comutadores de hardware com capacidade devem ser implantados em sua rede.


## <a name="install-dcb-in-windows-server-2016"></a>Instalar o DCB no Windows Server 2016

Você pode usar as seções a seguir para instalar o DCB em um computador executando o Windows Server 2016.

**Credenciais administrativas**

Para executar estes procedimentos, você deve ser um membro da **administradores**.

### <a name="install-dcb-using-windows-powershell"></a>Instalar o DCB usando o Windows PowerShell

Você pode usar o procedimento a seguir para instalar o DCB usando o Windows PowerShell.

1. Em um computador executando o Windows Server 2016, clique em **iniciar**, em seguida, clique com botão direito no ícone do Windows PowerShell. Um menu é exibido. No menu, clique em **mais**e, em seguida, clique em **executar como administrador**. Se solicitado, digite as credenciais para uma conta que tenha privilégios de administrador no computador. Windows PowerShell é aberto com privilégios de administrador.
2. Digite o comando a seguir e pressione ENTER.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Instalar o DCB usando o Gerenciador do servidor

Você pode usar o procedimento a seguir para instalar o DCB usando o Gerenciador do servidor.

>[!NOTE]
>Depois de executar a primeira etapa neste procedimento, o **antes de começar** página do assistente Adicionar funções e recursos não será exibida se você tiver selecionado anteriormente **pular esta página por padrão** quando a adicionar Assistente de funções e recursos foi executado. Se o **antes de começar** página não for exibida, pule da etapa 1 para a etapa 3.

1. No DC1, no Gerenciador do servidor, clique em **Manage**e, em seguida, clique em **para adicionar funções e recursos**. O Assistente para Adicionar Funções e Recursos é aberto.
2. Em **Antes de Começar**, clique em **Avançar**.
3. Em **Selecionar Tipo de Instalação**, verifique se **Instalação baseada em função ou recurso** está marcada e clique em **Avançar**.
4. Em **Selecionar servidor de destino**, verifique se **Selecionar um servidor no pool de servidores** está marcada. Em **Pool de Servidores**, verifique se o computador local está selecionado. Clique em **Avançar**.
5. Em **Selecionar funções de servidor**, clique em **Avançar**.
6. Na **selecionar recursos**, na **recursos**, clique em **Data Center Bridging**. Uma caixa de diálogo é aberta para perguntar se você deseja adicionar recursos necessários do DCB. Clique em **adicionar recursos**.
7. Na **selecionar recursos**, clique em **próxima**. 
8. 7. Além **confirmar seleções de instalação**, clique em **instalar**. O **progresso da instalação** página exibe o status durante o processo de instalação. Depois que a mensagem será exibida informando que instalação foi bem-sucedida, clique em **fechar**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurar o depurador de kernel para permitir o QoS \(opcional\)

 Por padrão, os depuradores de kernel bloqueiam NetQos. Independentemente do método que você usou para instalar o DCB, se você tiver um depurador de kernel instalado no computador, você deve configurar o depurador para permitir o QoS ser habilitada e configurada, executando o comando a seguir.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Instalar o DCB no Windows 10

Você pode executar o procedimento a seguir em um computador Windows 10.

Para executar este procedimento, você deve ser um membro da **administradores**.

### <a name="install-dcb"></a>Instalar DCB

1. Clique em **inicie**, em seguida, role para baixo e clique em **sistema Windows**.
2. Clique em **Painel de Controle**. O **painel de controle** caixa de diálogo é aberta.
3. Na **painel de controle**, clique em **exibir por**e, em seguida, clique em **ícones grandes** ou **ícones pequenos**.
4. Clique em **programas e recursos**. Abre a caixa de diálogo programas e recursos.
5. Na **programas e recursos**, no painel esquerdo, clique em **ativar ou desativar recursos do Windows ativar**. O **recursos do Windows** caixa de diálogo é aberta.
6. Na **recursos do Windows**, clique em **Data Center Bridging**e, em seguida, clique em **Okey**.

![Ativar ou desativar a caixa de diálogo recursos do Windows](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


