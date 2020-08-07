---
title: O Windows 8 deve ser configurado com pelo menos a quantidade mínima de memória
description: Fornece instruções para resolver o problema relatado por essa regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 519d1091-fa4d-44d7-83ca-83f6aa71fb7d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: bbdb53b23e611ddafe84ec9ce48e6b1f42c8e57e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938935"
---
# <a name="windows-8-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>O Windows 8 deve ser configurado com pelo menos a quantidade mínima de memória

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Erro|
|**Categoria**|Configuração|

As seções a seguir fornecem detalhes sobre o problema específico. Os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para o problema específico.

## <a name="issue"></a>**Problema**
*Uma máquina virtual que executa o Windows 8 é configurada com menos que a quantidade mínima de RAM, que é de 512 MB.*

## <a name="impact"></a>**Impacto**
*O sistema operacional convidado nas seguintes máquinas virtuais pode não ser executado ou pode ser executado de forma não confiável:*
```
<list of virtual machines>
```
## <a name="resolution"></a>**Resolução**
*Use o Gerenciador do Hyper-V para aumentar a memória alocada para esta máquina virtual para pelo menos 512 MB.*

#### <a name="increase-the-memory-using-hyper-v-manager"></a>Aumentar a memória usando o Gerenciador do Hyper-V

1.  Abra o Gerenciador do Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.

2.  No painel de resultados, em **máquinas virtuais**, selecione a máquina virtual que você deseja configurar. O estado da máquina virtual deve ser listado como **desativado**. Se não estiver, clique com o botão direito do mouse na máquina virtual e clique em **desligar**.

3.  No painel **Ação**, abaixo do nome da máquina virtual, clique em **Configurações**.

4.  No painel de navegação, clique em **memória**.

5.  Na página **memória** , defina a **RAM de inicialização** para pelo menos 512 MB e clique em **OK**.

### <a name="increase-the-memory-using-windows-powershell"></a>Aumentar a memória usando o Windows PowerShell

1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **Iniciar** e comece a digitar **Windows PowerShell**.)

2.  Clique com o botão direito do mouse em **Windows PowerShell** e clique em **Executar como administrador**.

3.  Execute este comando depois de substituir \<MyVM> pelo nome da sua máquina virtual:

```
Set-VMMemory <MyVM> -StartupBytes 512MB
```

## <a name="see-also"></a>Consulte Também
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)



