---
title: Solucionar problemas de VMs Blindadas
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/3/2018
ms.openlocfilehash: 13ff0dad1519d394ce74a91efbfcc9e2f237e4a5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850027"
---
# <a name="troubleshoot-shielded-vms"></a>Solucionar problemas de VMs Blindadas

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Começando com o Windows Server versão 1803, PS direta e modo de sessão avançado de Conexão de máquina Virtual (VMConnect) são habilitadas novamente para as VMs blindadas totalmente. O administrador de virtualização ainda exige credenciais de convidado da VM para obter acesso à VM, mas isso torna mais fácil para um hoster solucionar problemas de uma VM blindada quando sua configuração de rede é interrompida.

Para habilitar o VMConnect e direto do PS para suas VMs blindadas, simplesmente movê-los para um host do Hyper-V que executa o Windows Server versão 1803 ou posterior. Os dispositivos virtuais, permitindo a esses recursos serão reabilitados automaticamente. Se uma VM blindada for movida para um host que executa e a versão anterior do Windows Server, VMConnect e PS direta será desabilitado novamente.

Para os clientes confidenciais de segurança que se preocupar se hosters tem acesso à VM e desejarem retornar ao comportamento original, os recursos a seguir devem ser desabilitados no SO convidado:

- Desabilite o serviço do PowerShell Direct na VM:

  ```powershell
  Stop-Service vmicvmsession
  Set-Service vmicvmsession -StartupType Disabled
  ```

- Modo de sessão avançada do VMConnect só pode ser desabilitado se o sistema operacional convidado é pelo menos Windows Server 2019 ou Windows 10, versão 1809. Adicione a seguinte chave do registro na sua VM para desabilitar as conexões do console de sessão avançada do VMConnect:

  ```
  reg add "HKLM\Software\Microsoft\Virtual Machine\Guest" /v DisableEnhancedSessionConsoleConnection /t REG_DWORD /d 1
  ```
