---
title: O serviço de gerenciamento de máquinas virtuais do Hyper-V deve estar em execução
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: 58886b68ca30ddeb064fc12c6cb4c00183399715
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826107"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>O serviço de gerenciamento de máquinas virtuais do Hyper-V deve estar em execução

>Aplica-se a: Windows Server 2016
  
Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Pré-requisitos|  

Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.

## <a name="issue"></a>Problema  
  
*O serviço necessário para gerenciar máquinas virtuais não está em execução.*  
  
## <a name="impact"></a>Impacto  
  
*Não há operações de gerenciamento de máquina virtual podem ser executadas.*  
  
As máquinas virtuais que estão em execução continuará a ser executado. No entanto, você não será capaz de gerenciar máquinas virtuais, ou criar ou excluí-los até que o serviço está em execução.  
  
## <a name="resolution"></a>Resolução  
  
*Use o snap-in Serviços ou a ferramenta de linha de comando do Sc config para reconfigurar o serviço para iniciar automaticamente.*  
  
> [!TIP]  
> Se você não é possível localizar o serviço no aplicativo da área de trabalho ou a ferramenta de linha de comando relata que o serviço não existir, as ferramentas de gerenciamento do Hyper-V provavelmente não estão instalados. E se você não for capaz de ver o console do MMC do Hyper-V no menu Iniciar, você deve instalar as ferramentas de gerenciamento do Hyper-V.

Para instalar as ferramentas de gerenciamento do Hyper-V:  
>   
> - No Windows Server, abra o Gerenciador do servidor e use o assistente Adicionar funções e recursos. Para obter mais detalhes, consulte [instalar a função Hyper-V no Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  Você também pode usar o PowerShell para instalar as ferramentas (`Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell`) 
> - No Windows, na área de trabalho, comece digitando **programas**, clique em **programas e recursos** (painel de controle) > **ativar ou desativar recursos do Windows ativar**  >   **Hyper-V** > **ferramentas de gerenciamento do Hyper-V**. Clique em **OK**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Para reconfigurar o serviço para iniciar automaticamente usando o aplicativo de serviços de área de trabalho  
  
1.  Abra o aplicativo de serviços da área de trabalho. (Clique em **inicie**, clique no **Iniciar pesquisa** , digite **Services. msc**, e pressione ENTER.)  
  
2.  No painel de detalhes, clique com botão direito **gerenciamento de máquinas virtuais do Hyper-V**e, em seguida, clique em **propriedades**.  
  
3.  Sobre o **gerais** guia de **inicialização** digite, clique em **automático**.  
  
4.  Para iniciar o serviço, clique em **iniciar**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>Para reconfigurar o serviço para iniciar automaticamente usando SC Config  
  
1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **inicie** e comece a digitar **Windows PowerShell**.)  
  
2.  Clique com botão direito **Windows PowerShell** e clique em **executar como administrador**.  
  
3.  Para reconfigurar o serviço, digite:  
  
    ```  
    sc config vmms start=auto  
    ```  
  
4.  Para iniciar o serviço, digite:  
  
    ```  
    sc start vmms  
    ```  
  
Se o serviço já está configurado para iniciar automaticamente e é preciso reiniciar o serviço, você pode fazer isso no Gerenciador Hyper-V ou do comando "sc start vmms" mostrada acima.  
  
#### <a name="to-restart-the-service-from-hyper-v-manager"></a>Para reiniciar o serviço do Gerenciador do Hyper-V  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de navegação, clique no nome do servidor, se ainda não estiver selecionada.  
  
3.  No **ações** painel, clique em **iniciar serviço**.  
  


