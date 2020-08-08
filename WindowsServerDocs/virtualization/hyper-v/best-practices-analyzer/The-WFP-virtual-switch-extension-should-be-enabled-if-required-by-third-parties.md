---
title: a extensão do comutador virtual WFP deve ser habilitada se for exigida pelas extensões de terceiros
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 8a8ee1cae5f1eb64e11494d62bc31ebff295d284
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960469"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>a extensão do comutador virtual WFP deve ser habilitada se for exigida pelas extensões de terceiros

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>**Problema**
*A extensão do comutador virtual da Windows Filtering Platform (WFP) está desabilitada.*

## <a name="impact"></a>**Impacto**
*Algumas extensões de comutador virtual de terceiros podem não funcionar corretamente nos seguintes comutadores virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolução**
*Use o cmdlet do Windows PowerShell, Enable-VMSwitchExtension, para habilitar a plataforma de filtragem do Windows se ela for exigida por extensões de terceiros.*

### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>Habilitar a plataforma de filtragem do Windows usando o Windows PowerShell

1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **Iniciar** e comece a digitar **Windows PowerShell**.)

2.  Clique com o botão direito do mouse em **Windows PowerShell** e clique em **Executar como administrador**.

3.  Execute este comando depois de substituir external pelo nome do seu comutador externo:

```
Enable-VMSwitchExtension -VMSwitchName External -Name Microsoft Windows Filtering Platform
```

## <a name="see-also"></a>Consulte Também
[Habilitar-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx)



