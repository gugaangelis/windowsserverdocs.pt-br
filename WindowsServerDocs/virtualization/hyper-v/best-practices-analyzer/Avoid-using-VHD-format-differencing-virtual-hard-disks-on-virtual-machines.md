---
title: Evite usar discos rígidos virtuais diferenciais de formato VHD em máquinas virtuais que executam cargas de trabalho de servidor em um ambiente de produção
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a11959266db4c9f3da73123c41a211198f27b9a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857719"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Evite usar discos rígidos virtuais diferenciais de formato VHD em máquinas virtuais que executam cargas de trabalho de servidor em um ambiente de produção

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
*Uma ou mais máquinas virtuais usam discos rígidos virtuais diferenciais de formato VHD.*  
  
## <a name="impact"></a>**Causa**  
*Os discos rígidos virtuais diferenciais de formato VHD podem apresentar problemas de consistência se ocorrer uma falha de energia. Problemas de consistência podem ocorrer se o disco físico executar uma atualização incompleta ou incorreta para um setor em um arquivo. vhd que está sendo modificado quando ocorrer uma falha de energia. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Desligue a máquina virtual e converta a cadeia de discos rígidos virtuais diferenciais de formato VHD para o formato VHDX ou mescle a cadeia em um disco rígido virtual fixo. (O formato VHDX tem mecanismos de confiabilidade que ajudam a proteger o disco contra corrupções devido a falhas de energia.) No entanto, não converta o disco rígido virtual se for provável que ele esteja anexado a uma versão anterior do Windows em algum momento. As versões do Windows anteriores ao Windows Server 2012 não dão suporte ao formato VHDX.*  
  


