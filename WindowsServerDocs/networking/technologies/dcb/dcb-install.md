---
title: Instalar dados Center ponte (DCB) do Windows cliente ou servidor
description: Este tópico fornece instruções sobre como instalar a ponte do Centro de dados no Windows Server ou o cliente do Windows.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 491bdeb1a7458be1f991be68724e7a7b51f67ecf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Instalar o Centro de dados ligando \(DCB\) no Windows Server 2016 ou Windows 10

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como instalar DCB no Windows Server 2016 ou o Windows 10.

## <a name="prerequisites-for-using-dcb"></a>Pré-requisitos para usar DCB

A seguir estão os pré-requisitos para configurar e gerenciar DCB.

### <a name="install-a-compatible-operating-system"></a>Instalar um sistema operacional compatível

Você pode usar os comandos DCB deste guia nos seguintes sistemas operacionais.

- Windows Server (anual por canal)
- Windows Server 2016
- Windows 10 \(all versions\)

Os seguintes sistemas operacionais incluir versões anteriores dos DCB que não são compatíveis com os comandos que são usados na documentação DCB para Windows Server 2016 e Windows 10.

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>Requisitos de hardware

Esta é uma lista dos requisitos de hardware para DCB.

- Compatível com DCB\ adapter\(s\) de rede Ethernet devem ser instalados em computadores que estão fornecendo o Windows Server 2016 DCB.
- Opções de hardware compatíveis com DCB\ devem ser implantadas em sua rede.


## <a name="install-dcb-in-windows-server-2016"></a>Instalar DCB no Windows Server 2016

Você pode usar as seções a seguir para instalar DCB em um computador executando o Windows Server 2016.

**Credenciais administrativas**

Para executar esses procedimentos, você deve ser um membro do **administradores**.

### <a name="install-dcb-using-windows-powershell"></a>Instalar DCB usando o Windows PowerShell

Você pode usar o procedimento a seguir para instalar DCB usando o Windows PowerShell.

1. Em um computador executando o Windows Server 2016, clique em **iniciar**, em seguida, clique com botão direito no ícone do Windows PowerShell. Um menu é exibido. No menu, clique em **mais**e clique em **executar como administrador**. Se solicitado, digite as credenciais de uma conta que tenha privilégios de administrador no computador. Abre do Windows PowerShell com privilégios de administrador.
2. Digite o seguinte comando e pressione ENTER.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Instalar DCB usando o Gerenciador do servidor

Você pode usar o procedimento a seguir para instalar DCB usando o Gerenciador do servidor.

>[!NOTE]
>Depois que você executar a primeira etapa neste procedimento, o **Before You Begin** página do Assistente de recursos de adição de funções e não é exibida se você tiver selecionado anteriormente **ignorar esta página por padrão** quando a adição de funções e recursos de assistente foi executado. Se o **Before You Begin** página não for exibida, pule na etapa 1 para a etapa 3.

1. Em DC1, no Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**. Abre o assistente Adicionar funções e recursos.
2. Em **Before You Begin**, clique em **próxima**.
3. Em **selecione tipo de instalação**, certifique-se de que **instalação baseada em função ou recurso baseado** está selecionado e clique em **próxima**.
4. Em **servidor de destino Select**, certifique-se de que **selecionar um servidor do pool servidor** é selecionado. Em **Pool de servidores**, verifique se o computador local está selecionado. Clique em **próxima**.
5. Em **selecionar funções de servidor**, clique em **próxima**.
6. Em **Selecione recursos**, na **recursos**, clique em **ponte do Centro de dados**. Abre uma caixa de diálogo para perguntar se você deseja adicionar recursos DCB necessário. Clique em **adicionar recursos**.
7. Em **Selecione recursos**, clique em **próxima**. 
8. 7. Em **confirmar seleções de instalação**, clique em **instalar**. O **progresso da instalação** página exibe o status durante o processo de instalação. Depois que a mensagem for exibida informando essa instalação foi bem-sucedida, clique em **fechar**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurar o depurador de kernel para permitir QoS \(Optional\)

 Por padrão, o kernel depuradores bloqueiam NetQos. Independentemente do método que você usou para instalar DCB, se você tiver um depurador de kernel instalado no computador, você deve configurar o depurador para permitir QoS ser habilitado e configurado, executando o comando a seguir.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Instalar DCB no Windows 10

Você pode executar o procedimento a seguir em um computador Windows 10.

Para executar este procedimento, você deve ser um membro do **administradores **.

### <a name="install-dcb"></a>Instalar DCB

1. Clique em **iniciar**, em seguida, role para baixo e clique em **sistema Windows **.
2. Clique em **painel de controle **. O **painel de controle** caixa de diálogo é aberta.
3. Em **painel de controle**, clique em **exibir por**e, em seguida, clique em **ícones grandes** ou **ícones pequenos **.
4. Clique em **programas e recursos **. Abre a caixa de diálogo programas e recursos.
5. Em **programas e recursos**, no painel esquerdo, clique em **ou desativar recursos do Windows ativar **. O **recursos do Windows** caixa de diálogo é aberta.
6. Em **recursos do Windows**, clique em **ponte do Centro de dados**e clique em **Okey **.

![Ativar ou desativar a caixa de diálogo recursos do Windows](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


