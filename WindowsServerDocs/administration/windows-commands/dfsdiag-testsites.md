---
title: dfsdiag TestSites
description: O tópico de comandos do Windows para Dfsdiag TestSites, que verifica a configuração dos sites dos serviços de domínio Active Directory (AD DS), verificando se os servidores que atuam como servidores de namespace ou destinos de pasta (link) têm as mesmas associações de site em todos os controladores de domínio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80cc9095748dafb030b204130bfa2ccb61ec69ea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846219"
---
# <a name="dfsdiag-testsites"></a>dfsdiag TestSites

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica a configuração dos sites dos serviços de domínio do Active Directory (AD DS) verificando se os servidores que atuam como servidores de namespace ou destinos de pasta (link) têm as mesmas associações de site em todos os controladores de domínio.

## <a name="syntax"></a>Sintaxe  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
#### <a name="parameters"></a>Parâmetros  
  
|Parâmetro|Descrição|  
|-------|--------|  
|Computador \/:<server name>|O nome do servidor no qual verificar a associação do site.|  
|\/DFSpath:<namespace root or DFS folder>|A raiz do namespace ou a pasta do Sistema de Arquivos Distribuído (DFS) (link) com destinos para os quais verificar a associação do site.|  
|recurse \/|Enumera e verifica as associações de site para todos os destinos de pasta na raiz de namespace especificada.|  
|\/completo|verifica se AD DS e o registro do servidor contêm as mesmas informações de associação do site.|  
  
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
 
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>Referências adicionais  
  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

