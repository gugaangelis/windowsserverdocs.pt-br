---
title: O Windows Vista deve ser configurado com pelo menos a quantidade mínima de memória
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 49f6f9b6-c290-4b1b-b6f3-cc9a0acd8fb2
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 5897b0e363e725b913c8d6492a84df0a957cabb6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860969"
---
# <a name="windows-vista-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>O Windows Vista deve ser configurado com pelo menos a quantidade mínima de memória

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Error|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Uma máquina virtual que executa o Windows Vista é configurada com menos que a quantidade mínima de RAM, que é de 512 MB.*  
  
## <a name="impact"></a>Impacto  
  
*O sistema operacional convidado nas seguintes máquinas virtuais pode não ser executado ou pode ser executado de forma não confiável:*  
  
\<lista de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Use o Gerenciador do Hyper-V para aumentar a memória alocada para esta máquina virtual para pelo menos 512 MB.*  
  
### <a name="to-increase-the-memory-using-hyper-v-manager"></a>Para aumentar a memória usando o Gerenciador do Hyper-V  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de resultados, em **máquinas virtuais**, selecione a máquina virtual que você deseja configurar. O estado da máquina virtual deve ser listado como **desativado**. Se não estiver, clique com o botão direito do mouse na máquina virtual e clique em **desligar**.  
  
3.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.  
  
4.  No painel de navegação, clique em **memória**.  
  
5.  Na página **memória** , defina a **RAM de inicialização** para pelo menos 512 MB e clique em **OK**.  
  
### <a name="increase-the-memory-using-windows-powershell"></a>Aumentar a memória usando o Windows PowerShell  
  
1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **Iniciar** e comece a digitar **Windows PowerShell**.)  
  
2.  Clique com o botão direito do mouse em **Windows PowerShell** e clique em **Executar como administrador**.  
  
3.  Execute este comando depois de substituir \<MyVM > pelo nome da sua máquina virtual:  
  
```  
Set-VMMemory <MyVM> -StartupBytes 512MB  
```  
  
## <a name="see-also"></a>Consulte também  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


