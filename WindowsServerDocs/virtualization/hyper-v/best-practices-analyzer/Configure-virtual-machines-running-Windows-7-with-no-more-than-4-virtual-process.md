---
title: Configurar máquinas virtuais que executam o Windows 7 sem mais de 4 processadores virtuais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8fcf0868-b543-4f94-aee7-35324346da55
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: da58833b33f25a8ce724dcbe46161ae99c8d1224
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862029"
---
# <a name="configure-virtual-machines-running-windows-7-with-no-more-than-4-virtual-processors"></a>Configurar máquinas virtuais que executam o Windows 7 sem mais de 4 processadores virtuais

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Error|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Uma máquina virtual que executa o Windows 7 está configurada com mais de 4 processadores virtuais.*  
  
## <a name="impact"></a>**Causa**  
*A Microsoft não oferece suporte à configuração das seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Desligue a máquina virtual e remova um ou mais processadores virtuais.*  
  
#### <a name="to-remove-virtual-processors"></a>Para remover processadores virtuais  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de resultados, em **máquinas virtuais**, selecione a máquina virtual que você deseja configurar. O estado da máquina virtual deve ser listado como **desativado**. Se não estiver, clique com o botão direito do mouse na máquina virtual e clique em **desligar**.  
  
3.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.  
  
4.  No painel de navegação, clique em **processador**.  
  
5.  Na página **processador** , defina o número de processadores para **3** ou menos e clique em **OK**.  
  


