---
title: Evite usar discos rígidos virtuais diferenciais de formato VHD em máquinas virtuais que executam cargas de trabalho de servidor em um ambiente de produção
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ded65ab95c4a32ae55e9270cd5f77d80a6d1f9e1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946039"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Evite usar discos rígidos virtuais diferenciais de formato VHD em máquinas virtuais que executam cargas de trabalho de servidor em um ambiente de produção

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
*Uma ou mais máquinas virtuais usam discos rígidos virtuais diferenciais de formato VHD.*

## <a name="impact"></a>**Impacto**
*Os discos rígidos virtuais diferenciais de formato VHD podem apresentar problemas de consistência se ocorrer uma falha de energia. Problemas de consistência podem ocorrer se o disco físico executar uma atualização incompleta ou incorreta para um setor em um arquivo. vhd que está sendo modificado quando ocorrer uma falha de energia. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolução**
*Desligue a máquina virtual e converta a cadeia de discos rígidos virtuais diferenciais de formato VHD para o formato VHDX ou mescle a cadeia em um disco rígido virtual fixo. (O formato VHDX tem mecanismos de confiabilidade que ajudam a proteger o disco contra corrupções devido a falhas de energia.) No entanto, não converta o disco rígido virtual se for provável que ele esteja anexado a uma versão anterior do Windows em algum momento. As versões do Windows anteriores ao Windows Server 2012 não dão suporte ao formato VHDX.*



