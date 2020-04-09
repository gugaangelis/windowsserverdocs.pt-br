---
title: cacls
description: O tópico de comandos do Windows para cacls, que exibe ou modifica listas de controle de acesso discricional (DACL) em arquivos especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8cc6300563880a587b6544ec8cb61e9010ad6c2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848239"
---
# <a name="cacls"></a>cacls

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe ou modifica as listas de controle de acesso discricional (DACL) nos arquivos especificados.  

## <a name="syntax"></a>Sintaxe  
```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```  
#### <a name="parameters"></a>Parâmetros  

|        Parâmetro        |                                                                                            Descrição                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      \<nome de arquivo\>       |                                                                            Obrigatório. Exibe ACLs de arquivos especificados.                                                                             |
|           /t            |                                                          altera as ACLs de arquivos especificados no diretório atual e em todos os subdiretórios.                                                          |
|           /m            |                                                                          altera as ACLs de volumes montados em um diretório.                                                                           |
|           /l            |                                                                        Trabalhe no próprio link simbólico em relação ao destino.                                                                         |
|         /s: SDDL         |                                       Substitui as ACLs pelas especificadas na cadeia de caracteres SDDL (não é válida com **/e**, **/g**, **/r**, **/p**ou **/d**).                                        |
|           /e            |                                                                                 Edite a ACL em vez de substituí-la.                                                                                  |
|           /c            |                                                                                 Continuar nos erros de acesso negado.                                                                                  |
|    /g usuário:\<Perm\>     |   Conceda direitos de acesso de usuário especificados.<p>Valores válidos para a permissão:<p>-n-nenhum<br />-r-leitura<br />-w-gravação<br />-c-alterar (gravação)<br />-f-controle total   |
|      /r usuário [...]      |                                                                  Revogar os direitos de acesso do usuário especificado (válido somente com **/e**).                                                                   |
| [/p usuário:\<Perm\> [...] | substituir os direitos de acesso do usuário especificado.<p>Valores válidos para a permissão:<p>-n-nenhum<br />-r-leitura<br />-w-gravação<br />-c-alterar (gravação)<br />-f-controle total |
|     [/d usuário [...]      |                                                                                    Negar acesso de usuário especificado.                                                                                     |
|           /?            |                                                                                Exibe a ajuda no prompt de comando.                                                                                |

## <a name="remarks"></a>Comentários  
- Este comando foi preterido. Em vez disso, use o [icacls](icacls.md) .  
- Use a tabela a seguir para interpretar os resultados:  


  |      Saída       |                A ACE (entrada de controle de acesso) aplica-se a                |
  |-------------------|---------------------------------------------------------------------|
  |        OI         |               Herança de objeto. Esta pasta e arquivos.                |
  |        CI         |           Herança de contêiner. Esta pasta e subpastas.            |
  |        de E/S         | Herdar somente. A ACE não se aplica ao arquivo/diretório atual. |
  | Nenhuma mensagem de saída |                          Somente esta pasta.                          |
  |     Oi CIS      |                 Esta pasta, subpastas e arquivos.                 |
  |   Oi CIS I    |                     Somente subpastas e arquivos.                      |
  |     CIS I      |                          Somente subpastas.                           |
  |     Oi I      |                             Somente arquivos.                             |


- Você pode usar curingas ( **?** e **\\\*** ) para especificar vários arquivos.  
- Você pode especificar mais de um usuário.  

## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)   
-   [icacls](icacls.md)  
