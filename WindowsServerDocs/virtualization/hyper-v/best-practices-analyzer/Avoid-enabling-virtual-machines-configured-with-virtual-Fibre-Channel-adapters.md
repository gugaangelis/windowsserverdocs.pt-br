---
title: Evite habilitar máquinas virtuais configuradas com adaptadores de Fibre Channel virtual para permitir migrações ao vivo quando houver menos caminhos para Fibre Channel LUNs (unidades lógicas) no destino do que na origem
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.date: 8/16/2016
ms.openlocfilehash: 71617bbf6718e77f004b57e38035f5277c45c3bc
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747061"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>Evite habilitar máquinas virtuais configuradas com adaptadores de Fibre Channel virtual para permitir migrações ao vivo quando houver menos caminhos para Fibre Channel LUNs (unidades lógicas) no destino do que na origem

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
*Uma ou mais máquinas virtuais têm a propriedade AllowReducedFcRedunancy definida no provedor WMI de virtualização.*

## <a name="impact"></a>**Impacto**
*A migração dinâmica das seguintes máquinas virtuais pode levar à perda de dados ou interromper a e/s para O armazenamento:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolução**
*Considere limpar a propriedade WMI AllowReducedFcRedundancy nas máquinas virtuais afetadas. Quando essa propriedade for desmarcada, você poderá executar uma migração dinâmica em máquinas virtuais configuradas com adaptadores de Fibre Channel virtual somente quando o número de caminhos para Fibre Channel no destino for o mesmo ou mais do que o número de caminhos na origem. Essas verificações ajudam a evitar a perda de dados ou a interrupção de e/s no armazenamento.*