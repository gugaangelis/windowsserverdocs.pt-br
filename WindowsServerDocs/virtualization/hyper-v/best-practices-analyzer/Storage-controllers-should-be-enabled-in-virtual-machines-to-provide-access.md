---
title: Os controladores de armazenamento devem ser habilitados em máquinas virtuais para fornecer acesso ao armazenamento anexado
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
ms.date: 8/16/2016
ms.openlocfilehash: 5eb1767fd51253ed26c0a169be1043b633dbcbcd
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746191"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>Os controladores de armazenamento devem ser habilitados em máquinas virtuais para fornecer acesso ao armazenamento anexado

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*Um ou mais controladores de armazenamento podem estar desabilitados em uma máquina virtual.*

## <a name="impact"></a>Impacto

*As máquinas virtuais não podem usar o armazenamento conectado a um controlador de armazenamento desabilitado. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machine names>

## <a name="resolution"></a>Resolução

*Use Device Manager no sistema operacional convidado para habilitar todos os controladores de armazenamento. Se o controlador de armazenamento não for necessário, use o Gerenciador do Hyper-V para removê-lo da máquina virtual.*

Para obter instruções sobre como usar Device Manager, consulte a ajuda no sistema operacional convidado. Para obter instruções sobre como remover o controlador de armazenamento, consulte o procedimento a seguir.

#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>Para remover um controlador de armazenamento SCSI da máquina virtual

1.  Abra o Gerenciador do Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.

2.  No painel de resultados, em **máquinas virtuais**, selecione a máquina virtual que você deseja configurar.

3.  Se a máquina virtual estiver em execução, desligue a máquina virtual. Clique com o botão direito do mouse na máquina virtual e clique em **desligar**.

4.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.

5.  No painel esquerdo da caixa de diálogo **configurações** , em **hardware**, clique em **controlador SCSI**.

6.  No painel direito, clique em **remover**.



