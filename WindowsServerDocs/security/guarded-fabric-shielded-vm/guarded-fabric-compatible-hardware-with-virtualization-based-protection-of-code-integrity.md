---
title: Hardware compatível com a proteção baseada em virtualização do Windows Server de integridade de código
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: a52ff808af94159fe50c72bf0466768047ceea90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820367"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>Hardware compatível com a proteção baseada em virtualização do Windows Server de integridade de código

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Windows Server 2016 introduziu uma nova proteção do código com base em virtualização para ajudar a proteger físicos e máquinas virtuais contra ataques que modificar o código do sistema. Para atingir esse nível de proteção alto, a Microsoft trabalha em conjunto com os fabricantes de hardware do computador (fabricantes originais do equipamento ou OEMs) para impedir gravações mal-intencionado no código de execução do sistema. Essa proteção pode ser aplicada a qualquer sistema e está sendo usada como um dos blocos de construção para implementar a integridade do host do Hyper-V para máquinas virtuais blindadas (VMs). 

Assim como acontece com qualquer proteção baseada em hardware, alguns sistemas não podem ser compatível com devido a questões como marca incorreta de páginas de memória como executáveis ou, na verdade, tentando modificar o código em tempo de execução, o que pode resultar em falhas inesperadas, incluindo perda de dados ou uma azul Erro de tela (também chamado de um erro de parada). 

Para ser compatível e dar suporte total ao novo recurso de segurança, os OEMs precisam implementar a tabela de endereço de memória definida no 2.6 UEFI, que foi publicada em janeiro de 2016. A adoção do novo padrão de UEFI leva tempo; Enquanto isso, para impedir que os clientes a encontrar problemas, queremos que fornecem informações sobre sistemas e configurações que nós testamos este conjunto de recursos com, bem como os sistemas que sabemos que não seja compatível. 

## <a name="non-compatible-systems"></a>Sistemas não compatíveis

As configurações a seguir são conhecidas como não compatível com a proteção baseada em virtualização de integridade de código e não pode ser usado como um host para VMs Blindadas:

- Servidores Dell PowerEdge executando PERC H330 RAID controladores para obter mais informações, consulte o seguinte artigo do suporte Dell [H330 – habilitando "Suporte de Hyper-V de guardião de Host" ou "Device Guard" em um sistema operacional do Win 2016 faz com que a falha de inicialização do sistema operacional](http://www.dell.com/Support/Article/us/en/19/QNA44045).  


## <a name="compatible-systems"></a>Sistemas compatíveis

Esses são os sistemas de que nós e nossos parceiros têm sido teste em nosso ambiente. Certifique-se de que você verifique se que o sistema funciona conforme o esperado em seu ambiente: 

- Máquinas virtuais – você pode habilitar a proteção baseada em virtualização de integridade de código em máquinas virtuais que são executados em um host Hyper-V a partir do Windows Server 2016.



