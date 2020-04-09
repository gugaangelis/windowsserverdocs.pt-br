---
title: O Windows 7 deve ser configurado com a quantidade recomendada de memória
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d77d648c-6e26-43fa-be0a-6eb4b28f9cb4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 742132228f2dae5b1b5b0d604b62142890cf9c3e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854149"
---
# <a name="windows-7-should-be-configured-with-the-recommended-amount-of-memory"></a>O Windows 7 deve ser configurado com a quantidade recomendada de memória

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema  
  
*Uma máquina virtual que executa o Windows 7 é configurada com menos do que a quantidade recomendada de RAM, que é de 1 GB.*  
  
## <a name="impact"></a>Impacto  
  
*O sistema operacional convidado e os aplicativos podem não ter um bom desempenho. Pode não haver memória suficiente para executar vários aplicativos ao mesmo tempo. Isso afeta as seguintes máquinas virtuais:*  
```  
<list of virtual machine names>  
```  
## <a name="resolution"></a>Resolução  
  
*Use o Gerenciador do Hyper-V para aumentar a memória alocada para esta máquina virtual para pelo menos 1 GB.*  
  
### <a name="to-increase-the-memory-using-hyper-v-manager"></a>Para aumentar a memória usando o Gerenciador do Hyper-V  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de resultados, em **máquinas virtuais**, selecione a máquina virtual que você deseja configurar. O estado da máquina virtual deve ser listado como **desativado**. Se não estiver, clique com o botão direito do mouse na máquina virtual e clique em **desligar**.  
  
3.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.  
  
4.  No painel de navegação, clique em **memória**.  
  
5.  Na página **memória** , defina a **RAM de inicialização** para pelo menos 1 GB e clique em **OK**.  
  
### <a name="increase-the-memory-using-windows-powershell"></a>Aumentar a memória usando o Windows PowerShell  
  
1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **Iniciar** e comece a digitar **Windows PowerShell**.)  
  
2.  Clique com o botão direito do mouse em **Windows PowerShell** e clique em **Executar como administrador**.  
  
3.  Execute este comando depois de substituir \<MyVM > pelo nome da sua máquina virtual:  
  
```  
Set-VMMemory <MyVM> -StartupBytes 1GB  
```  
  
## <a name="see-also"></a>Consulte também  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


