---
title: Evite habilitar máquinas virtuais configuradas com adaptadores de Fibre Channel virtual para permitir que as migrações ao vivo quando há menos caminhos unidades lógicas de Fibre Channel (LUNs) no destino que na origem
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6ff69d5cb09133a806c2a2df3446713264a4e892
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849547"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>Evite habilitar máquinas virtuais configuradas com adaptadores de Fibre Channel virtual para permitir que as migrações ao vivo quando há menos caminhos unidades lógicas de Fibre Channel (LUNs) no destino que na origem

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|

Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*Uma ou mais máquinas virtuais têm a propriedade de AllowReducedFcRedunancy definida no provedor WMI de virtualização.*  
  
## <a name="impact"></a>**Impacto**  
*Migração ao vivo das seguintes máquinas virtuais pode levar à perda de dados ou interrupção de e/s para o armazenamento:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Considere a possibilidade de limpar a propriedade de WMI AllowReducedFcRedundancy nas máquinas virtuais afetadas. Quando essa propriedade é desmarcada, você pode executar uma migração ao vivo em máquinas virtuais configuradas com adaptadores de Fibre Channel virtual somente quando o número de caminhos para Fibre Channel no destino é igual ou maior que o número de caminhos na origem. Essas verificações ajudam a evitar a perda de dados ou interrupção de e/s para o armazenamento.* 