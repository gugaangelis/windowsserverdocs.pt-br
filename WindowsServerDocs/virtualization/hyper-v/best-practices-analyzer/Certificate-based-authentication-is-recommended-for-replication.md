---
title: A autenticação baseada em certificado é recomendada para replicação
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
ms.date: 8/16/2016
ms.openlocfilehash: 3ef11472c042e19de9f7ee52ea5958ca0bf6e44b
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745851"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>A autenticação baseada em certificado é recomendada para replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>**Problema**
*Uma ou mais máquinas virtuais selecionadas para replicação estão configuradas para autenticação Kerberos.*

## <a name="impact"></a>**Impacto**
*O tráfego de rede de replicação do servidor primário para o servidor de replicação não é criptografado. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolução**
*Se outro método estiver sendo usado para executar a criptografia, você poderá ignorá-lo. Caso contrário, modifique as configurações da máquina virtual para escolher autenticação baseada em certificado.*



