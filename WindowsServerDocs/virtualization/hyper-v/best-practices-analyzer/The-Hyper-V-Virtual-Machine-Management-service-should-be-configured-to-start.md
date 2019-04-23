---
title: O serviço de gerenciamento de máquinas virtuais do Hyper-V deve ser configurado para iniciar automaticamente
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 222bbe76-c514-4a3f-b61b-860a4dc2826a
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c33f81678d7fdc71e81834a002fd3d7917a6f632
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833247"
---
# <a name="the-hyper-v-virtual-machine-management-service-should-be-configured-to-start-automatically"></a>O serviço de gerenciamento de máquinas virtuais do Hyper-V deve ser configurado para iniciar automaticamente

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  

Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.

## <a name="issue"></a>Problema  
  
*O serviço de gerenciamento de máquinas virtuais do Hyper-V não está configurado para iniciar automaticamente.*  
  
## <a name="impact"></a>Impacto  
  
*As máquinas virtuais não podem ser gerenciadas até que o serviço é iniciado.*  
  
As máquinas virtuais que estão em execução continuará a ser executado. No entanto, você não será capaz de gerenciar máquinas virtuais, ou criar ou excluí-los até que o serviço está em execução.  
  
## <a name="resolution"></a>Resolução  
  
*Use a ferramenta Serviços de snap-in ou sc config de linha de comando para reconfigurar o serviço para iniciar automaticamente.*  
  
> [!TIP]  
> Se você não é possível localizar o serviço no aplicativo da área de trabalho ou a ferramenta de linha de comando relata que o serviço não existir, as ferramentas de gerenciamento do Hyper-V provavelmente não estão instalados. Para instalá-los:  
>   
> - No Windows Server, abra o Gerenciador do servidor e use o assistente Adicionar funções e recursos. Para obter mais detalhes, consulte [instalar a função Hyper-V no Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  
> - No Windows, na área de trabalho, comece digitando **programas**, clique em **programas e recursos** (painel de controle) > **ativar ou desativar recursos do Windows ativar**  >   **Hyper-V** > **ferramentas de gerenciamento do Hyper-V**. Clique em **OK**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Para reconfigurar o serviço para iniciar automaticamente usando o aplicativo de serviços de área de trabalho  
  
1.  Abra o aplicativo de serviços da área de trabalho. (Clique em **inicie**, clique na caixa de pesquisa, comece a digitar **Services**e, em seguida, clique em serviços na lista de resultados.  
  
2.  No painel de detalhes, clique com botão direito **gerenciamento de máquinas virtuais do Hyper-V**e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **gerais** guia de **inicialização** digite, clique em **automático**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-sc-config-command"></a>Para reconfigurar o serviço para iniciar automaticamente usando o comando SC Config  
  
1.  Abra o Windows PowerShell.  
  
2.  Digite:  
  
    ```  
    set-service  vmms -startuptype automatic  
    ```  
  
3.  Se o serviço não estiver sendo executado, digite:  
  
    ```  
    start-service -name vmms  
    ```  
  


