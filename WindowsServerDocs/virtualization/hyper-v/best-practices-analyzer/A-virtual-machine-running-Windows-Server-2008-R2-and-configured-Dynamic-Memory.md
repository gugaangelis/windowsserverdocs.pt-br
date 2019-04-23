---
title: Uma máquina virtual executando o Windows Server 2008 R2 e configurados com a memória dinâmica deve usar valores recomendados para as configurações de memória
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 81b5034a-31ea-4397-bcd0-7b9ef50beb94
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 026885a15e1df5e556462d33b2dc0762a63c97d0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842387"
---
# <a name="a-virtual-machine-running-windows-server-2008-r2-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>Uma máquina virtual executando o Windows Server 2008 R2 e configurados com a memória dinâmica deve usar valores recomendados para as configurações de memória

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Uma ou mais máquinas virtuais são configuradas para usar a memória dinâmica com menor do que a quantidade de memória recomendada para o Windows Server 2008 R2.*  
  
## <a name="impact"></a>Impacto  
*O sistema operacional nas seguintes máquinas virtuais pode não ser executado ou pode ser executado de modo não confiável:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V ou o Windows PowerShell para aumentar a memória mínima para pelo menos 256 MB, memória de inicialização para pelo menos 512 MB e memória máxima a pelo menos 2 GB.*  
  
#### <a name="increase-memory-using-hyper-v-manager"></a>Aumente a memória usando o Gerenciador do Hyper-V  
  
1.  Abra o Gerenciador Hyper-V. (No Gerenciador do servidor, clique em **ferramentas** > **Gerenciador Hyper-V**.)  
  
2.  Na lista de máquinas virtuais, clique com botão direito aquele que você deseja e clique **configurações**.  
  
3.  No painel de navegação, clique em **memória**.  
  
4.  Alterar o **RAM** para pelo menos 512 MB.  
  
5.  Sob **memória dinâmica**, altere o **mínimo RAM** para pelo menos 256 MB e o **RAM máxima** a 2 GB.  
  
6.  Clique em **OK**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Aumente a memória usando o Windows PowerShell  
  
1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **inicie** e comece a digitar **Windows PowerShell**.)  
  
2.  Clique com botão direito **Windows PowerShell** e clique em **executar como administrador**.  
  
3.  Execute um comando semelhante ao seguinte, substituindo MyVM pelo nome da sua máquina virtual e da memória valores pelo menos os valores mostrados abaixo.  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 2GB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


