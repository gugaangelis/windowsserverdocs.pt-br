---
title: dfsdiag TestDCs
description: Tópico de comandos do Windows para Dfsdiag TestDCs, que verifica a configuração de controladores de domínio no domínio especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 092ce3710eb6d209f596683bd4ad054dadd11aa3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846309"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag TestDCs

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração de controladores de domínio executando os seguintes testes em cada controlador de domínio no domínio especificado:  
  
-   Verifica se o serviço de namespace do Sistema de Arquivos Distribuído (DFS) está em execução e se seu tipo de inicialização está definido como automático.  
  
-   Verifica o suporte de referências com custo de site para NETLOGON e SYSvol.  
  
-   Verifica a consistência da associação do site pelo nome do host e endereço IP.

## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
#### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|/Domain:`<domain_name>`|Domínio que você deseja verificar.|  
  
## <a name="remarks"></a>Comentários  

/Domain é um parâmetro opcional. O valor padrão é o domínio local ao qual o host local está associado.  
  
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Para verificar a configuração dos controladores de domínio no domínio Contoso.com, digite:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

