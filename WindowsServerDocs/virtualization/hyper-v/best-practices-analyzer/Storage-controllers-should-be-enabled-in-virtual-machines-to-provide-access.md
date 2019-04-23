---
title: Controladores de armazenamento devem ser habilitadas em máquinas virtuais para fornecer acesso a armazenamento anexado
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 42803a0eef84bf006e9f9e7ed6297ea21b4eb7b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849157"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>Controladores de armazenamento devem ser habilitadas em máquinas virtuais para fornecer acesso a armazenamento anexado

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
  
*Um ou mais controladores de armazenamento podem ser desabilitados em uma máquina virtual.*  
  
## <a name="impact"></a>Impacto  
  
*As máquinas virtuais não é possível usar o armazenamento conectado a um controlador de armazenamento desabilitada. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Use o Gerenciador de dispositivos no sistema operacional convidado para habilitar todos os controladores de armazenamento. Se o controlador de armazenamento não for necessário, use o Gerenciador do Hyper-V para removê-lo da máquina virtual.*  
  
Para obter instruções sobre como usar o Gerenciador de dispositivos, consulte a Ajuda no sistema operacional convidado. Para obter instruções sobre como remover o controlador de armazenamento, consulte o procedimento a seguir.  
  
#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>Para remover um controlador de armazenamento SCSI da máquina virtual  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de resultados, sob **máquinas virtuais**, selecione a máquina virtual que você deseja configurar.  
  
3.  Se a máquina virtual está em execução, desligue a máquina virtual. A máquina virtual com o botão direito e clique em **desligar**.  
  
4.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.  
  
5.  No painel esquerdo do **as configurações** caixa de diálogo **Hardware**, clique em **controlador SCSI**.  
  
6.  No painel direito, clique em **remover**.  
  


