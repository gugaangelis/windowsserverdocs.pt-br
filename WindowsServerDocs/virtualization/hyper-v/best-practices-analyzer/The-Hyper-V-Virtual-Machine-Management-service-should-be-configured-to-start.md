---
title: O serviço gerenciamento de máquinas virtuais do Hyper-V deve ser configurado para iniciar automaticamente
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 222bbe76-c514-4a3f-b61b-860a4dc2826a
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 26122d40b3fbdbdc40a94801d5e3ff8fcf4fa646
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859309"
---
# <a name="the-hyper-v-virtual-machine-management-service-should-be-configured-to-start-automatically"></a>O serviço gerenciamento de máquinas virtuais do Hyper-V deve ser configurado para iniciar automaticamente

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema  
  
*O serviço gerenciamento de máquinas virtuais do Hyper-V não está configurado para iniciar automaticamente.*  
  
## <a name="impact"></a>Impacto  
  
*As máquinas virtuais não podem ser gerenciadas até que o serviço seja iniciado.*  
  
As máquinas virtuais que estão em execução continuarão a ser executadas. No entanto, você não poderá gerenciar máquinas virtuais ou criá-las ou excluí-las até que o serviço esteja em execução.  
  
## <a name="resolution"></a>Resolução  
  
*Use o snap-in de serviços ou a ferramenta de linha de comando sc config para reconfigurar o serviço para iniciar automaticamente.*  
  
> [!TIP]  
> Se você não encontrar o serviço no aplicativo da área de trabalho ou a ferramenta de linha de comando informar que o serviço não existe, as ferramentas de gerenciamento do Hyper-V provavelmente não estão instaladas. Para instalá-los:  
>   
> - No Windows Server, abra Gerenciador do Servidor e use o assistente para adicionar funções e recursos. Para obter mais detalhes, consulte [instalar a função Hyper-V no Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  
> - No Windows, na área de trabalho, comece digitando **programas**, clique em **programas e recursos** (painel de controle) > **Ativar ou desativar recursos do Windows** > **Hyper-v** > **ferramentas de gerenciamento do Hyper-v**. Depois, clique em **OK**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Para reconfigurar o serviço para iniciar automaticamente usando o aplicativo de área de trabalho de serviços  
  
1.  Abra o aplicativo de área de trabalho serviços. (Clique em **Iniciar**, clique na caixa Pesquisar, comece a digitar **Serviços**e clique em serviços na lista de resultados.  
  
2.  No painel de detalhes, clique com o botão direito do mouse em **Gerenciamento de máquinas virtuais do Hyper-V**e clique em **Propriedades**.  
  
3.  Na guia **geral** , em tipo de **inicialização** , clique em **automático**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-sc-config-command"></a>Para reconfigurar o serviço para iniciar automaticamente usando o comando SC config  
  
1.  Abra o Windows PowerShell.  
  
2.  Tipo:  
  
    ```  
    set-service  vmms -startuptype automatic  
    ```  
  
3.  Se o serviço ainda não estiver em execução, digite:  
  
    ```  
    start-service -name vmms  
    ```  
  


