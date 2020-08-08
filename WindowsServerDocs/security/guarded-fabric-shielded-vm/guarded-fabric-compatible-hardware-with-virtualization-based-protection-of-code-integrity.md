---
title: Hardware compatível com proteção baseada em virtualização do Windows Server de integridade de código
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 616718b2b2a5ce3c30655d3fef37da5ee2ea25e5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971353"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>Hardware compatível com proteção baseada em virtualização do Windows Server de integridade de código

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

O Windows Server 2016 introduziu uma nova proteção de código baseada em virtualização para ajudar a proteger as máquinas físicas e virtuais contra ataques que modificam o código do sistema.
Para atingir esse alto nível de proteção, a Microsoft trabalha em conjunto com os fabricantes de hardware do computador (originais do equipamento original ou OEMs) para impedir gravações mal-intencionadas no código de execução do sistema.
Essa proteção pode ser aplicada a qualquer sistema e está sendo usada como um dos blocos de construção para implementar a integridade do host Hyper-V para VMs (máquinas virtuais) blindadas.

Como em qualquer proteção baseada em hardware, alguns sistemas podem não estar em conformidade devido a problemas como a marcação incorreta de páginas de memória como executáveis ou, na verdade, tentar modificar o código em tempo de execução, o que pode resultar em falhas inesperadas, incluindo a perda de dados ou um erro de tela azul (também chamado de erro de parada).

Para ser compatível e oferecer suporte total ao novo recurso de segurança, os OEMs precisam implementar a tabela de endereços de memória definida em UEFI 2,6, que foi publicada em Jan. 2016.
A adoção do novo padrão UEFI leva tempo; Enquanto isso, para evitar que os clientes descubram problemas, queremos fornecer informações sobre sistemas e configurações que testamos esse conjunto de recursos, bem como sistemas que sabemos que não são compatíveis.

## <a name="non-compatible-systems"></a>Sistemas não compatíveis

As configurações a seguir são conhecidas como não compatíveis com a proteção baseada em virtualização de integridade de código e não podem ser usadas como um host para VMs blindadas:

- Servidores Dell PowerEdge que executam controladores RAID PERC H330 para obter mais informações, consulte o seguinte artigo da H330 de suporte da Dell [– habilitar "suporte ao Hyper-V do guardião de host" ou "Device Guard" no sistema operacional Win 2016 causa falha na inicialização do sistema operacional](http://www.dell.com/Support/Article/us/en/19/QNA44045).


## <a name="compatible-systems"></a>Sistemas compatíveis

Esses são os sistemas que nós e nossos parceiros têm testado em nosso ambiente.
Certifique-se de verificar se o sistema funciona conforme o esperado em seu ambiente:

- Máquinas virtuais – você pode habilitar a proteção baseada em virtualização de integridade de código em máquinas virtuais executadas em um host Hyper-V a partir do Windows Server 2016.



