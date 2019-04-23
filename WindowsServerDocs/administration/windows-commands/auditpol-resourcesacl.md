---
title: auditpol resourceSACL
description: Tópico de comandos do Windows para **uditpol resourceSACL** -configura as listas de controle de acesso do recurso global sistema (SACL).
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 375f37250404dd6740027cb18959697626c1ffc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837487"
---
# <a name="auditpol-resourcesacl"></a>auditpol resourceSACL



Configura as listas de controle de acesso do recurso global sistema (SACL).

> [!NOTE]
> Aplica-se somente ao Windows 7 e Windows Server 2008 R2.

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
|/set|Adiciona uma nova entrada à ou atualiza uma entrada existente na SACL de recurso para o recurso de tipo especificado.|
|/remove|Remove todas as entradas para o usuário especificado na lista de auditoria de acesso a objeto global.|
|/clear|Remove todas as entradas da lista de auditoria de acesso a objeto global.|
|/View|Lista as entradas de auditoria de acesso do objeto global em uma SACL de recurso. Os tipos de usuário e recursos são opcionais.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="arguments"></a>Argumentos

|Argumento|Descrição|
|--------|-----------|
|/type|O recurso para o qual objeto de auditoria de acesso está sendo configurada. Os valores de argumento com suporte são o arquivo (para diretórios e arquivos) e a chave (para chaves do registro).</br>Observação: Os valores de arquivo e chave diferenciam maiusculas de minúsculas.|
|/success|Especifica a auditoria de êxito.|
|/failure|Especifica a auditoria de falha.|
|/user|Especifica um usuário em uma das seguintes formas:</br>-DomainName\Account (como DOM\Administrators)</br>-Conta StandaloneServer\Group (consulte [função LookupAccountName](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx))</br>-{S-1-x-x-x-x} (x é expresso na notação decimal, e o SID inteiro deve ser colocado entre chaves); Por exemplo: {S-1-5-21-5624481-130208933-164394174-1001}</br>    Observação:     Se o formulário de SID é usado, nenhuma verificação é feita para verificar a existência dessa conta.|
|/access|Especifica uma máscara de permissão que pode ser especificada em uma destas duas formas:</br>-Uma sequência de direitos simples:</br>    -Direitos de acesso genérico:</br>        -GA - GENÉRICA A TODOS OS</br>        -LEIA GR - GENÉRICO</br>        -GRAVAR GW - GENÉRICO</br>        -EXECUTAR GX - GENÉRICO</br>    -Direitos de acesso para arquivos:</br>        -FA - TODOS OS ACESSOS DE ARQUIVO</br>        LEIA - FR - ARQUIVO GENÉRICO</br>        -FW - GRAVAÇÃO GENÉRICA</br>        EXECUTAR - FX - ARQUIVO GENÉRICO</br>    -Direitos de acesso chaves do registro:</br>        -KA - TODO O ACESSO DE CHAVE</br>        LEIA - KR - CHAVE</br>        GRAVAÇÃO DE - KW - CHAVE</br>        EXECUTAR - KX - CHAVE</br>    Por exemplo: ' / acesso: FRFW' será habilitar eventos de auditoria para leitura e operações de gravação</br>-Um valor hexadecimal que representa a máscara de acesso (como 0x1200a9.)</br>    Isso é útil ao usar máscaras de bits de recurso específico que não fazem parte do que o padrão de linguagem (SDDL) de definição de descritor de segurança. Se omitido, o acesso completo é usado.|

## <a name="remarks"></a>Comentários

Para operações de resourceSACL, você deve ter permissão de gravação ou controle total naquele objeto definido no descritor de segurança. Você também pode executar operações de resourceSACL que possui o **gerenciar o log de auditoria e segurança** direito de usuário (SeSecurityPrivilege). No entanto, esse direito permite acesso adicional que não é necessário para executar a operação de remoção.

## <a name="BKMK_Examples"></a>Exemplos

Para definir um recurso global SACL para auditar o acesso com êxito tentativas por um usuário em uma chave do registro:
```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```
Para definir um recurso global SACL para fazer auditoria das tentativas bem-sucedidas e com falhas por um usuário para realizar a leitura genérica e escrever funções em arquivos ou pastas:
```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```
Para remover todas as entradas da SACL de recurso global para arquivos ou pastas:
```
auditpol /resourceSACL /type:File /clear
```
Para remover todas as entradas da SACL de recursos globais para um usuário específico de arquivos ou pastas:
```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```
Para listar entradas definidas em arquivos ou pastas de auditoria de acesso a objeto global:
```
auditpol /resourceSACL /type:File /view
```
Para listar o objeto global acessar entradas de auditoria para um usuário específico que são definidas em arquivos ou pastas:
```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)