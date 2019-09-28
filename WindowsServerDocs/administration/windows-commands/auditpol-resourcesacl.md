---
title: resourceSACL de Auditpol
description: Tópico de comandos do Windows para **Uditpol resourceSACL** – configura as SAcls (listas de controle de acesso) do sistema de recursos globais.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2acffd75298f0f36a9c15e0622816feaae57cb64
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382431"
---
# <a name="auditpol-resourcesacl"></a>resourceSACL de Auditpol



Configura as SACLs (listas de controle de acesso) do sistema de recursos globais.

> [!NOTE]
> Aplica-se somente ao Windows 7 e ao Windows Server 2008 R2.

Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
auditpol /resourceSACL
[/set /type:<resource> [/success] [/failure] /user:<user> [/access:<access flags>]]
[/remove /type:<resource> /user:<user> [/type:<resource>]]
[/clear [/type:<resource>]]
[/view [/user:<user>] [/type:<resource>]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/Set|Adiciona uma nova entrada ou atualiza uma entrada existente na SACL de recurso para o tipo de recurso especificado.|
|/remove|Remove todas as entradas do usuário especificado na lista de auditoria de acesso a objetos globais.|
|/Clear|Remove todas as entradas da lista de auditoria de acesso a objetos globais.|
|/view|Lista as entradas de auditoria de acesso a objeto global em uma SACL de recurso. Os tipos de usuário e recurso são opcionais.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="arguments"></a>Argumentos

|Argumento|Descrição|
|--------|-----------|
|/Type|O recurso para o qual a auditoria de acesso ao objeto está sendo configurada. Os valores de argumento com suporte são File (para diretórios e arquivos) e Key (para chaves do registro).</br>Observação: Os valores de arquivo e chave diferenciam maiúsculas de minúsculas.|
|/success|Especifica a auditoria com êxito.|
|/failure|Especifica a auditoria de falha.|
|/|Especifica um usuário em uma das seguintes formas:</br>-DomainName\Account (como DOM\Administrators)</br>-Conta StandaloneServer\Group (consulte a [função LookupAccountName](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx))</br>-{S-1-x-x-x-x} (x é expresso em decimal e o SID inteiro deve estar entre chaves); por exemplo: {S-1-5-21-5624481-130208933-164394174-1001}</br>    Observação:     Se o formulário SID for usado, nenhuma verificação será feita para verificar a existência dessa conta.|
|/access|Especifica uma máscara de permissão que pode ser especificada em uma das duas formas:</br>-Uma sequência de direitos simples:</br>    -Direitos de acesso genéricos:</br>        -GA-TUDO GENÉRICO</br>        -GR-LEITURA GENÉRICA</br>        -GW-GRAVAÇÃO GENÉRICA</br>        -GX-EXECUÇÃO GENÉRICA</br>    -Direitos de acesso para arquivos:</br>        -FA-TODO O ACESSO AO ARQUIVO</br>        -FR-ARQUIVO DE LEITURA GENÉRICA</br>        -FW-ARQUIVO DE GRAVAÇÃO GENÉRICA</br>        -FX-EXECUTAR GENERIC DE ARQUIVO</br>    -Direitos de acesso para chaves do registro:</br>        -KA-CHAVE TODOS OS ACESSOS</br>        -KR-CHAVE LIDA</br>        -KW-GRAVAÇÃO DE CHAVE</br>        -KX-EXECUTAR CHAVE</br>    Por exemplo: '/Access: FRFW ' habilitará eventos de auditoria para operações de leitura e gravação</br>-Um valor hexadecimal que representa a máscara de acesso (como 0x1200a9)</br>    Isso é útil ao usar máscaras de bits específicas de recursos que não fazem parte do padrão SDDL (Security Descriptor Definition Language). Se omitido, o acesso completo será usado.|

## <a name="remarks"></a>Comentários

Para operações resourceSACL, você deve ter a permissão gravar ou controle total nesse objeto definido no descritor de segurança. Você também pode executar operações de resourceSACL por meio do direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar a operação de remoção.

## <a name="BKMK_Examples"></a>Disso

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

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)