---
title: Configurar máquinas virtuais que executam o Windows Vista com 1 ou 2 processadores virtuais
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e562bce3-fd68-42c9-821c-12022ae4746c
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fae7d5e437fd83b9c00afcceaaf0eb7e8a7b909b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812067"
---
# <a name="configure-virtual-machines-running-windows-vista-with-1-or-2-virtual-processors"></a>Configurar máquinas virtuais que executam o Windows Vista com 1 ou 2 processadores virtuais

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Configuração|  
|**categoria**|Erro|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Uma máquina virtual executando o Windows Vista é configurada com mais de 2 processadores virtuais.*  
  
## <a name="impact"></a>Impacto  
  
*A Microsoft não suporta a configuração das seguintes máquinas virtuais:*  
  
\<lista de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Desligue a máquina virtual e remova um ou mais processadores virtuais.*  
  
### <a name="to-remove-virtual-processors"></a>Para remover os processadores virtuais  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de resultados, sob **máquinas virtuais**, selecione a máquina virtual que você deseja configurar. O estado da máquina virtual deve estar listado como **desativar**. Se não for, a máquina virtual com o botão direito e, em seguida, clique em **desligar**.  
  
3.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.  
  
4.  No painel de navegação, clique em **processador**.  
  
5.  Sobre o **processador** , defina o número de processadores a serem **1** ou **2** e, em seguida, clique em **Okey**.  
  


