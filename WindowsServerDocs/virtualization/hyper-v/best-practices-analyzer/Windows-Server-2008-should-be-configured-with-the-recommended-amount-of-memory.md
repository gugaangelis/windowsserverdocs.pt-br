---
title: Windows Server 2008 deve ser configurado com a quantidade de memória recomendada
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a98a8594-603b-487a-8739-78887c568e57
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 77410eb545c8c8ce6b02d083c7c875b569c63937
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818787"
---
# <a name="windows-server-2008-should-be-configured-with-the-recommended-amount-of-memory"></a>Windows Server 2008 deve ser configurado com a quantidade de memória recomendada

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Uma máquina virtual executando o Windows Server 2008 é configurada com o menor do que a quantidade recomendada de RAM, que é de 2 GB.*  
  
## <a name="impact"></a>Impacto  
  
*O sistema operacional convidado e aplicativos não podem executar bem. Talvez não haja memória suficiente para executar vários aplicativos ao mesmo tempo. Isso afeta as seguintes máquinas virtuais:*  
   
\<lista de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Use o Gerenciador do Hyper-V para aumentar a memória alocada para essa máquina virtual pelo menos 2 GB.*  
  
### <a name="increase-the-memory-using-hyper-v-manager"></a>Aumente a memória usando o Gerenciador do Hyper-V  
  
1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.  
  
2.  No painel de resultados, sob **máquinas virtuais**, selecione a máquina virtual que você deseja configurar. O estado da máquina virtual deve estar listado como **desativar**. Se não for, a máquina virtual com o botão direito e, em seguida, clique em **desligar**.  
  
3.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.  
  
4.  No painel de navegação, clique em **memória**.  
  
5.  Sobre o **memória** , defina o **RAM de inicialização** pelo menos 2 GB e depois clique em **Okey**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Aumente a memória usando o Windows PowerShell  
  
1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **inicie** e comece a digitar **Windows PowerShell**.)  
  
2.  Clique com botão direito **Windows PowerShell** e clique em **executar como administrador**.  
  
3.  Execute um comando semelhante ao seguinte, substituindo \<MyVM > com o nome da sua máquina virtual e a memória valores pelo menos os valores mostrados abaixo.  
  
```  
Set-VMMemory <MyVM> -StartupBytes 2GB  
```  
  
## <a name="see-also"></a>Consulte também  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


