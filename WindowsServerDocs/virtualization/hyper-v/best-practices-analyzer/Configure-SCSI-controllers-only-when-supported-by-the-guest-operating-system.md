---
title: Configurar controladores SCSI somente quando houver suporte pelo sistema operacional convidado
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
ms.date: 8/16/2016
ms.openlocfilehash: cadf4c0d0ce8cdd50d66a61f428b3e547dd457dc
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746991"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>Configurar controladores SCSI somente quando houver suporte pelo sistema operacional convidado

>Aplica-se a: Windows Server 2016



|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*Uma máquina virtual está configurada com um controlador SCSI que não pode ser usado porque o sistema operacional convidado não oferece suporte a controladores SCSI.*

## <a name="impact"></a>Impacto

*As máquinas virtuais não podem usar o armazenamento anexado ao controlador SCSI. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução

*Desligue a máquina virtual e use o Gerenciador do Hyper-V para remover o controlador SCSI da máquina virtual. Em seguida, reinicie a máquina virtual.*



