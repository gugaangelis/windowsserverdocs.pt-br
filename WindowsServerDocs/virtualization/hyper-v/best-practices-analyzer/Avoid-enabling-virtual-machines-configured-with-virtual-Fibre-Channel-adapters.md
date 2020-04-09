---
title: Evite habilitar máquinas virtuais configuradas com adaptadores de Fibre Channel virtual para permitir migrações ao vivo quando houver menos caminhos para Fibre Channel LUNs (unidades lógicas) no destino do que na origem
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 7989d2e1908f6be32f4661900fc507b5843b55c9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857769"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>Evite habilitar máquinas virtuais configuradas com adaptadores de Fibre Channel virtual para permitir migrações ao vivo quando houver menos caminhos para Fibre Channel LUNs (unidades lógicas) no destino do que na origem

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*Uma ou mais máquinas virtuais têm a propriedade AllowReducedFcRedunancy definida no provedor WMI de virtualização.*  
  
## <a name="impact"></a>**Causa**  
*A migração dinâmica das seguintes máquinas virtuais pode levar à perda de dados ou interromper a e/s para O armazenamento:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Considere limpar a propriedade WMI AllowReducedFcRedundancy nas máquinas virtuais afetadas. Quando essa propriedade for desmarcada, você poderá executar uma migração dinâmica em máquinas virtuais configuradas com adaptadores de Fibre Channel virtual somente quando o número de caminhos para Fibre Channel no destino for o mesmo ou mais do que o número de caminhos na origem. Essas verificações ajudam a evitar a perda de dados ou a interrupção de e/s no armazenamento.* 