---
title: A interface de equipe associada a um comutador virtual deve estar no modo padrão
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
ms.date: 8/16/2016
ms.openlocfilehash: 1e3d701ccc0ac7a6a3765df08f6b039fbb4ca3eb
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746131"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>A interface de equipe associada a um comutador virtual deve estar no modo padrão

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
*Alguns comutadores virtuais estão associados a uma interface de equipe, mas a interface de equipe não passa o tráfego em todas as VLANs para os comutadores virtuais.*

## <a name="impact"></a>**Impacto**
*Os seguintes comutadores virtuais não podem ter acesso a todas as VLANs: \n{0}*

## <a name="resolution"></a>**Resolução**
*Use Gerenciador do Servidor ou o cmdlet do Windows PowerShell Set-NetLbfoTeamNic para redefinir a interface de equipe para o modo padrão.*



