---
title: A autenticação baseada em certificado é recomendada para replicação
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ec5415d0bdad10c4b0f4f0560141dd290eca8e36
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954493"
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



