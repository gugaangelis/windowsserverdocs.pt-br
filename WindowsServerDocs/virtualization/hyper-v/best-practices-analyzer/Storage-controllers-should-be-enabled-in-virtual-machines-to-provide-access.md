---
title: Os controladores de armazenamento devem ser habilitados em máquinas virtuais para fornecer acesso ao armazenamento anexado
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f0d10ab4c419a6014a9edb4b7f721714dc92798d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393485"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>Os controladores de armazenamento devem ser habilitados em máquinas virtuais para fornecer acesso ao armazenamento anexado

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema  
  
*Um ou mais controladores de armazenamento podem estar desabilitados em uma máquina virtual.*  
  
## <a name="impact"></a>Impacto  
  
os computadores *Virtual não podem usar o armazenamento conectado a um controlador de armazenamento desabilitado. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Use Device Manager no sistema operacional convidado para habilitar todos os controladores de armazenamento. Se o controlador de armazenamento não for necessário, use o Gerenciador do Hyper-V para removê-lo da máquina virtual.*  
  
Para obter instruções sobre como usar Device Manager, consulte a ajuda no sistema operacional convidado. Para obter instruções sobre como remover o controlador de armazenamento, consulte o procedimento a seguir.  
  
#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>Para remover um controlador de armazenamento SCSI da máquina virtual  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de resultados, em **máquinas virtuais**, selecione a máquina virtual que você deseja configurar.  
  
3.  Se a máquina virtual estiver em execução, desligue a máquina virtual. Clique com o botão direito do mouse na máquina virtual e clique em **desligar**.  
  
4.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.  
  
5.  No painel esquerdo da caixa de diálogo **configurações** , em **hardware**, clique em **controlador SCSI**.  
  
6.  No painel direito, clique em **remover**.  
  


