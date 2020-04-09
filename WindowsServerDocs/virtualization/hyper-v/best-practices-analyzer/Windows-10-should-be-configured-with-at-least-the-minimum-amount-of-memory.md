---
title: O Windows 10 deve ser configurado com pelo menos a quantidade mínima de memória
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e4f5bd2f-b5be-4d43-80e0-0cf198182791
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1bdde1924c983748238e51984facf732356f4a2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854989"
---
# <a name="windows-10-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>O Windows 10 deve ser configurado com pelo menos a quantidade mínima de memória

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Error|  
|**Categoria**|Configuração|  
  
As seções a seguir fornecem detalhes sobre o problema específico. Os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para o problema específico.  
  
## <a name="issue"></a>**Problema**  
*Uma máquina virtual que executa o Windows 10 é configurada com menos que a quantidade mínima de RAM, que é de 512 MB.*  
  
## <a name="impact"></a>**Causa**  
*O sistema operacional convidado nas seguintes máquinas virtuais pode não ser executado ou pode ser executado de forma não confiável:*  
```  
<list of virtual machines>  
```  
## <a name="resolution"></a>**Resolução**  
*Use o Gerenciador do Hyper-V para aumentar a memória alocada para esta máquina virtual para pelo menos 512 MB.*  
  
#### <a name="increase-the-memory-using-hyper-v-manager"></a>Aumentar a memória usando o Gerenciador do Hyper-V  
  
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
  


