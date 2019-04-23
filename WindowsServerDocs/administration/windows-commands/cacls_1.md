---
title: cacls
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 289015ff580d7e2fba5ff911878028f5302cab8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838077"
---
# <a name="cacls"></a>cacls

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe ou modifica listas de controle de acesso discricionário (DACL) em arquivos especificados.  
## <a name="syntax"></a>Sintaxe  
```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|\<filename\>|Obrigatório. Exibe as Acls de arquivos especificados.|  
|/t|Altera as Acls de arquivos especificados no diretório atual e todos os subdiretórios.|  
|/m|alterações de Acls de volumes montados em um diretório.|  
|/l|Trabalhar em um Link simbólico em si em comparação com o destino.|  
|/s:sddl|substitui as Acls com os especificados na cadeia de caracteres SDDL (não é válido com **/e**, **/g**, **/r**, **p**, ou **/d**).|  
|/e|Edite a ACL em vez de substituí-lo.|  
|/c|Continue em erros de acesso negado.|  
|/g user:\<perm\>|Concessão especificada direitos de acesso do usuário.<br /><br />Valores válidos para a permissão:<br /><br />-n - none<br />-r - leitura<br />-w - gravação<br />-c - alteração (gravação)<br />-f - controle total|  
|usuário /r [...]|Revogue os direitos de acesso do usuário especificado (válido somente com **/e**).|  
|[usuário /p:\<perm\> [...]|Substitua os direitos de acesso do usuário especificado.<br /><br />Valores válidos para a permissão:<br /><br />-n - none<br />-r - leitura<br />-w - gravação<br />-c - alteração (gravação)<br />-f - controle total|  
|[/d usuário [...]|Nega acesso de usuário especificado.|  
|/?|Exibe a ajuda no prompt de comando.|  
## <a name="remarks"></a>Comentários  
-   Esse comando foi preterido. Use [icacls](icacls.md) em vez disso.  
-   Use a tabela a seguir para interpretar os resultados:  

    |Saída|Entrada de controle de acesso (ACE) aplica-se a|  
    |-----|----------------------|  
    |OI|Herdar do objeto. Esta pasta e arquivos.|  
    |CI|Herdar do contêiner. Esta pasta e subpastas.|  
    |E/S|Somente a herança. A ACE não se aplica ao arquivo/diretório atual.|  
    |Nenhuma mensagem de saída|Esta pasta somente.|  
    |(OI) (CI)|Essa pasta, subpastas e arquivos.|  
    |(OI) (CI) (E/S)|Apenas subpastas e arquivos.|  
    |(CI) (E/S)|Apenas subpastas.|  
    |(OI) (E/S)|Somente os arquivos.|  

-   Você pode usar curingas (**?** e **\***) para especificar vários arquivos.  
-   Você pode especificar mais de um usuário.  

#### <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)   
-   [icacls](icacls.md)  
