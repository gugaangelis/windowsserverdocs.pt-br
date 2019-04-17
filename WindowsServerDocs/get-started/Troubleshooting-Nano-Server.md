---
title: Solução de problemas do Nano Server
description: Console de Recuperação, Serviços de Gerenciamento de Emergência, depuração de kernel
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e427c66f-9571-4b8c-b65d-e7370d91544d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 0f5d3e352cd022853a1602c67c3aaf2530cfc696
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081802"
---
# <a name="troubleshooting-nano-server"></a>Solução de problemas do Nano Server

>Aplica-se a: Windows Server 2016

> [!IMPORTANT]
> A partir do Windows Server, versão 1709, o Nano Server estará disponível somente como uma [imagem de sistema operacional base do contêiner](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Confira [Mudanças no Nano Server](nano-in-semi-annual-channel.md) para saber o que isso significa. 

Este tópico inclui informações sobre as ferramentas que você pode usar para se conectar às instalações do Nano Server, diagnosticá-las e repará-las.  
  
## <a name="using-the-nano-server-recovery-console"></a>Usar o Console de Recuperação do Nano Server 
 
O Nano Server inclui um Console de Recuperação que garante o acesso ao seu Nano Server mesmo se uma configuração incorreta de rede interferir na conexão com o Nano Server. Você pode usar o Console de Recuperação para corrigir a rede e, em seguida, use as ferramentas de gerenciamento remoto comuns.  
  
Quando você inicializa o Nano Server em uma máquina virtual ou em um computador físico com um monitor e teclado conectados, você verá um prompt de logon de modo de texto em tela inteira. Faça logon nesse prompt com uma conta de administrador para ver o nome do computador e o endereço IP do Nano Server. Você pode usar esses comandos para navegar no console:  
  
-   Use as teclas de seta para rolar  
  
-   Use TAB para mover para qualquer texto que começa com **>**; e pressione ENTER para selecionar.  
  
-   Para retornar uma tela ou página, pressione ESC. Se você estiver na home page, pressionar ESC o desconectará.  
  
-   Algumas telas têm recursos adicionais exibidos na última linha da tela. Por exemplo, se você explorar um adaptador de rede, F4 desabilitará o adaptador de rede.  
  
O Console de Recuperação permite que você exiba e configure adaptadores de rede e configurações de TCP/IP, bem como regras de firewall.
> [!NOTE]  
    > O Console de Recuperação só oferece suporte a funções básicas de teclado. Não há suporte para as luzes do teclado, seções de 10 teclas e alternância do layout do teclado, como caps lock e bloqueio de número. Só há suporte para teclados e conjunto de caracteres em inglês.

## <a name="accessing-nano-server-over-a-serial-port-with-emergency-management-services"></a>Acessar o Nano Server em uma porta serial com Serviços de Gerenciamento de Emergência  
O EMS (Serviços de Gerenciamento de Emergência) permite que você execute a solução de problemas básica, obtenha o status da rede e abra sessões do console (incluindo PowerShell/CMD) usando um emulador de terminal por meio de uma porta serial. Isso substitui a necessidade de um teclado e um monitor para solucionar problemas de um servidor. Para saber mais sobre o EMS, confira [Emergency Management Services Technical Reference (Referência técnica dos Serviços de Gerenciamento de Emergência)](https://technet.microsoft.com/library/cc784411(v=ws.10).aspx).

Para habilitar o EMS em uma imagem do Nano Server para que ele esteja pronto caso você precise dele posteriormente, execute este cmdlet:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\EnablingEMS.vhdx   -EnableEMS   -EMSPort 3   -EMSBaudRate 9600`  
  
Este exemplo de cmdlet habilita o EMS na porta serial 3 com uma taxa de transmissão de 9600 bps. Se você não incluir esses parâmetros, os padrões serão a porta 1 e 115200 bps. Para usar este cmdlet para mídia VHDX, lembre-se de incluir o recurso Hyper-V e os módulos do Windows PowerShell correspondentes.

## <a name="kernel-debugging"></a>Depuração de kernel  
Você pode configurar a imagem do Nano Server para oferecer suporte a vários métodos de depuração de kernel. Para usar a depuração de kernel com uma imagem VHDX, lembre-se de incluir o recurso Hyper-V e os módulos do Windows PowerShell correspondentes. Para saber mais sobre a depuração remota de kernel em geral, confira [Setting Up Kernel-Mode Debugging over a Network Cable Manually (Configurar a depuração de modo Kernel em um cabo de rede manualmente)](https://msdn.microsoft.com/library/windows/hardware/hh439346%28v=vs.85%29.aspx) e [Remote Debugging Using WinDbg (Depuração remota usando o WinDbg)](https://msdn.microsoft.com/library/windows/hardware/hh451173%28v=vs.85%29.aspx).  
  
### <a name="debugging-using-a-serial-port"></a>Depuração usando uma porta serial  
Use este cmdlet de exemplo para permitir que a imagem seja depurada usando uma porta serial:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingSerial   -DebugMethod Serial   -DebugCOMPort 1   -DebugBaudRate 9600`  
  
Este exemplo habilita a depuração serial pela porta 2 com uma taxa de transmissão de 9600 bps. Se você não especificar esses parâmetros, os padrões serão a porta 2 e 115200 bps. Se você pretende usar o EMS e a depuração de kernel, precisará configurá-los para usar duas portas seriais separadas.  
  
### <a name="debugging-over-a-tcpip-network"></a>Depuração em uma rede TCP/IP  
Use este cmdlet de exemplo para permitir que a imagem seja depurada em uma rede TCP/IP:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000`  
  
Esse cmdlet permite a depuração de kernel, de modo que apenas o computador com o endereço IP 192.168.1.100 tenha permissão para se conectar, com toda a comunicação pela porta 64000. Os parâmetros -DebugRemoteIP e -DebugPort são obrigatórios, com um número de porta maior do que 49152. Este cmdlet gera uma chave de criptografia em um arquivo junto com o VHD resultante que é necessário para a comunicação pela porta. Como alternativa, você pode especificar sua própria chave com o parâmetro -DebugKey, como neste exemplo:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000   -DebugKey 1.2.3.4`  
  
### <a name="debugging-using-the-ieee1394-protocol-firewire"></a>Depuração usando o protocolo IEEE1394 (Firewire)  
Para habilitar a depuração por IEEE1394, use este cmdlet de exemplo:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingFireWire   -DebugMethod 1394   -DebugChannel 3`  
  
O parâmetro -DebugChannel é obrigatório.  
  
### <a name="debugging-using-usb"></a>Depuração usando USB  
Você pode habilitar a depuração em USB com este cmdlet:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingUSB   -DebugMethod USB   -DebugTargetName KernelDebuggingUSBNano`  
  
Quando você conecta o depurador remoto ao Nano Server resultante, especifique o nome de destino conforme definido pelo parâmetro -DebugTargetName.    