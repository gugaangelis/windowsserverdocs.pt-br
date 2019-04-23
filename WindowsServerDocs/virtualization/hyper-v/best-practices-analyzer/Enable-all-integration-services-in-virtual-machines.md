---
title: Habilitar todos os serviços de integração em máquinas virtuais
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 307e2d407a0defa14a6b57bda95a2f3ab018406d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829427"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>Habilitar todos os serviços de integração em máquinas virtuais

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
  
*Um ou mais serviços de integração são desabilitados ou não funcionando em uma máquina virtual.*  
  
## <a name="impact"></a>Impacto  
  
*O recurso de integração ou serviço pode não funcionar corretamente para as seguintes máquinas virtuais:*  
  
\<lista de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Use a ferramenta Serviços de snap-in ou sc config de linha de comando para verificar se o serviço está configurado para iniciar automaticamente e não será interrompido.*  
  
#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>Para configurar como um serviço é iniciado usando o snap-in de serviços  
  
1.  Use serviços de área de trabalho remota ou Conexão de máquina Virtual para conectar-se para a máquina virtual e o log em para o sistema operacional convidado.  
  
2.  Abrir Serviços. (Clique em **inicie**, clique no **Iniciar pesquisa** , digite **Services. msc**, e pressione ENTER.)  
  
3.  No painel de detalhes, clique com o botão direito no serviço que deseja configurar e clique em **Propriedades**.  
  
4.  Sobre o **gerais** guia de **inicialização** digite, clique em **automático**.  
  
#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>Para configurar como um serviço é iniciado usando SC Config  
  
1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **inicie** e comece a digitar **Windows PowerShell**.)  
  
2.  Clique com botão direito **Windows PowerShell** e clique em **executar como administrador**.  
  
3.  Substitua < service-name > pelo nome do serviço, em seguida, digite:  
  
    ```  
    sc config <service-name> start=auto  
    ```  
  


