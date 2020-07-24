---
title: resourceSACL de Auditpol
description: Artigo de referência para o comando Auditpol resourceSACL, que configura as SAcls (listas de controle de acesso) do sistema de recursos globais.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4558d18b065cd668294952131b494342d600aee0
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955488"
---
# <a name="auditpol-resourcesacl"></a>resourceSACL de Auditpol

> Aplica-se a: Windows 7 e Windows Server 2008 R2

Configura as SACLs (listas de controle de acesso) do sistema de recursos globais.

Para executar operações *resourceSACL* , você deve ter permissões de **controle total** ou de **gravação** para esse objeto definido no descritor de segurança. Você também pode executar operações de *resourceSACL* se tiver o direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege).

## <a name="syntax"></a>Sintaxe

```
auditpol /resourceSACL
[/set /type:<resource> [/success] [/failure] /user:<user> [/access:<access flags>]]
[/remove /type:<resource> /user:<user> [/type:<resource>]]
[/clear [/type:<resource>]]
[/view [/user:<user>] [/type:<resource>]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /Set | Adiciona uma nova entrada ou atualiza uma entrada existente na SACL de recurso para o tipo de recurso especificado. |
| /remove | Remove todas as entradas do usuário especificado na lista de auditoria de acesso a objetos globais. |
| /Clear | Remove todas as entradas da lista de auditoria de acesso a objetos globais.|
| /view | Lista as entradas de auditoria de acesso a objeto global em uma SACL de recurso. Os tipos de usuário e recurso são opcionais. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="arguments"></a>Argumentos

| Argumento | Descrição |
| -------- | ----------- |
| /type | O recurso para o qual a auditoria de acesso ao objeto está sendo configurada. Os valores de argumento com suporte, diferenciam maiúsculas de minúsculas são *arquivos* (para diretórios e arquivos) e *chave* (para chaves do registro). |
| /success | Especifica a auditoria com êxito. |
| /failure | Especifica a auditoria de falha. |
| / | Especifica um usuário em uma das seguintes formas:<ul><li> DomainName\Account (como DOM\Administrators)</li><li>Conta do StandaloneServer\Group (consulte a [função LookupAccountName](/windows/win32/api/winbase/nf-winbase-lookupaccountnamea))</li><li>{S-1-x-x-x-x} (x é expresso em decimal e o SID inteiro deve estar entre chaves). Por exemplo: {S-1-5-21-5624481-130208933-164394174-1001}<p>**Observação:** Se o formulário SID for usado, nenhuma verificação será feita para verificar a existência dessa conta.</li></ul> |
| /access | Especifica uma máscara de permissão que pode ser especificada por meio de:<p>Direitos de acesso genéricos, incluindo:<ul><li>GA-TUDO GENÉRICO</li><li>GR-LEITURA GENÉRICA</li><li>GW-GRAVAÇÃO GENÉRICA</li><li>GX-EXECUÇÃO GENÉRICA</li></ul><p>Direitos de acesso para arquivos, incluindo:<ul><li>FA-TODO O ACESSO AO ARQUIVO</li><li>FR-ARQUIVO DE LEITURA GENÉRICA</li><li>FW-ARQUIVO DE GRAVAÇÃO GENÉRICA</li><li>FX-ARQUIVO GENÉRICO EXECUTAR</li></ul><p>Direitos de acesso para chaves do registro, incluindo:<ul><li>KA-CHAVE ALL ACCESS</li><li>KR-CHAVE LIDA</li><li>KW-GRAVAÇÃO DE CHAVE</li><li>KX-EXECUTAR CHAVE</li></ul><p>Por exemplo: `/access:FRFW` habilita eventos de auditoria para operações de leitura e gravação.<p>Um valor hexadecimal que representa a máscara de acesso (como 0x1200a9)<p>Isso é útil ao usar máscaras de bits específicas de recursos que não fazem parte do padrão SDDL (Security Descriptor Definition Language). Se omitido, o acesso completo será usado. |

## <a name="examples"></a>Exemplos

Para definir uma SACL de recurso global para auditar tentativas de acesso bem-sucedidas por um usuário em uma chave do registro:

```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```

Para definir uma SACL de recurso global para auditar tentativas bem-sucedidas e com falha por um usuário executar funções genéricas de leitura e gravação em arquivos ou pastas:

```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```

Para remover todas as entradas de SACL de recursos globais para arquivos ou pastas:

```
auditpol /resourceSACL /type:File /clear
```

Para remover todas as entradas de SACL de recursos globais de um usuário específico de arquivos ou pastas:

```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```

Para listar as entradas de auditoria de acesso a objeto global definidas em arquivos ou pastas:

```
auditpol /resourceSACL /type:File /view
```

Para listar as entradas de auditoria de acesso a objeto global para um usuário específico que são definidos em arquivos ou pastas:

```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comandos Auditpol](auditpol.md)
