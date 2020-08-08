---
title: Use pelo menos a versão 3,0 do protocolo SMB configurada para disponibilidade contínua em compartilhamentos de arquivos que armazenam arquivos para máquinas virtuais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 7fd84ecf7876638d421f9a8f7042e81c131f2ab2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960269"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>Use pelo menos a versão 3,0 do protocolo SMB configurada para disponibilidade contínua em compartilhamentos de arquivos que armazenam arquivos para máquinas virtuais

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
*Os arquivos de máquina virtual ou os arquivos de disco rígido virtual são armazenados em um compartilhamento de arquivos de rede que não está configurado com o recurso de disponibilidade contínua do protocolo SMB versão 3,0.*

## <a name="impact"></a>**Impacto**
*A Microsoft não recomenda essa configuração porque ela pode afetar a disponibilidade das máquinas virtuais usando o servidor. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolução**
Realize um dos seguintes procedimentos:

-   Mova os arquivos para um compartilhamento de arquivos SMB 3,0 configurado para disponibilidade contínua.

-   Reconfigure o compartilhamento de arquivos atual para fornecer disponibilidade contínua.



