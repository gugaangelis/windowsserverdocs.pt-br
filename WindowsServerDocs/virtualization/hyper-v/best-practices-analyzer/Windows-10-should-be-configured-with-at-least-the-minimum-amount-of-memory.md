---
title: Windows 10 deve ser configurado com pelo menos a quantidade mínima de memória
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e4f5bd2f-b5be-4d43-80e0-0cf198182791
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e621750049666a916d99cf843d3a7a16069773be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824117"
---
# <a name="windows-10-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>Windows 10 deve ser configurado com pelo menos a quantidade mínima de memória

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  
  
As seções a seguir fornecem detalhes sobre o problema específico. Itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para o problema específico.  
  
## <a name="issue"></a>**Problema**  
*Uma máquina virtual executando o Windows 10 é configurada com o menor do que a quantidade mínima de RAM, que é de 512 MB.*  
  
## <a name="impact"></a>**Impacto**  
*O sistema operacional nas seguintes máquinas virtuais pode não ser executado ou pode ser executado de modo não confiável:*  
```  
<list of virtual machines>  
```  
## <a name="resolution"></a>**Resolução**  
*Use o Gerenciador do Hyper-V para aumentar a memória alocada para essa máquina virtual para pelo menos 512 MB.*  
  
#### <a name="increase-the-memory-using-hyper-v-manager"></a>Aumente a memória usando o Gerenciador do Hyper-V  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de resultados, sob **máquinas virtuais**, selecione a máquina virtual que você deseja configurar. O estado da máquina virtual deve estar listado como **desativar**. Se não for, a máquina virtual com o botão direito e, em seguida, clique em **desligar**.  
  
3.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.  
  
4.  No painel de navegação, clique em **memória**.  
  
5.  Sobre o **memória** , defina o **RAM de inicialização** pelo menos 512 MB e depois clique em **Okey**.  
  
### <a name="increase-the-memory-using-windows-powershell"></a>Aumente a memória usando o Windows PowerShell  
  
1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **inicie** e comece a digitar **Windows PowerShell**.)  
  
2.  Clique com botão direito **Windows PowerShell** e clique em **executar como administrador**.  
  
3.  Execute este comando depois de substituir \<MyVM > pelo nome da sua máquina virtual:  
  
```  
Set-VMMemory <MyVM> -StartupBytes 512MB  
```  
  
## <a name="see-also"></a>Consulte também  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


