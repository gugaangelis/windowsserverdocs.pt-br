---
title: taskkill
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db264181ef8e5e3632f3312ade61183cac3fc8f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853077"
---
# <a name="taskkill"></a>taskkill

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Termina uma ou mais tarefas ou processos. Os processos podem ser encerrados pela identificação do processo ou nome da imagem. **Taskkill** substitui o **kill** ferramenta.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe
```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/s \<computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.|
|/u \<domínio >\\\<nome de usuário >|Executa o comando com as permissões de conta do usuário que é especificado pelo *nome de usuário* ou *domínio*\\*UserName*. **/u** pode ser especificado somente se **/s** for especificado. O padrão é que as permissões do usuário que está conectado no momento no computador que está emitindo o comando.|
|/p \<Password>|Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.|
|/fi \<Filter>|Aplica um filtro para selecionar um conjunto de tarefas. Você pode usar mais de um filtro ou use o caractere curinga (**\***) para especificar todas as tarefas ou nomes de imagem. Consulte o seguinte [tabela de nomes de filtro válido](#BKMK_table), operadores e valores.|
|/PID \<ProcessID >|Especifica a ID de processo do processo a ser encerrado.|
|/im \<ImageName>|Especifica o nome da imagem do processo a ser encerrado. Use o caractere curinga (**\***) para especificar todos os nomes de imagem.|
|/f|Especifica que os processos de modo forçado ser terminado. Esse parâmetro é ignorado para processos remotos; todos os processos remotos são forçados.|
|/t|Encerra o processo especificado e todos os processos filho iniciados por ele.|

#### <a name="BKMK_table"></a>Os nomes de filtro, operadores e valores
|Nome do filtro|Operadores válidos|Valor (es) válido|
|--------|----------|----------|
|STatUS|eq, ne|RUNNING &#124; NOT RESPONDING &#124; UNKNOWN|
|IMAGENAME|eq, ne|Nome da imagem|
|PID|eq, ne, gt, lt, ge, le|Valor PID|
|SESSÃO|eq, ne, gt, lt, ge, le|Número da sessão|
|CPUtime|eq, ne, gt, lt, ge, le|Tempo de CPU no formato *HH ***:*** MM ***:*** SS*, onde *MM* e *SS* estão entre 0 e 59 e *HH* é qualquer número de unsigned|
|MEMUSAGE|eq, ne, gt, lt, ge, le|Uso de memória em KB|
|NOME DE USUÁRIO|eq, ne|Qualquer nome de usuário válido (*usuário* ou *domínio*\\*usuário*)|
|SERVIÇOS|eq, ne|Nome do serviço|
|WINDOWTITLE|eq, ne|Título da janela|
|MÓDULOS|eq, ne|Nome da DLL|

## <a name="remarks"></a>Comentários
* Não há suporte para os filtros de STatUS e WINDOWTITLE quando um sistema remoto é especificado.
* O caractere curinga (**\***) é aceito para o **/im** opção somente quando um filtro é aplicado.
* Encerramento de processos remotos sempre é realizado de modo forçado, independentemente se o **/f** opção for especificada.
* Fornecer um nome de computador para o filtro de nome de host faz com que um desligamento e todos os processos são interrompidos.
* Você pode usar **tasklist** para determinar o PID (ID) para o processo de encerramento do processo.

## <a name="examples"></a>Exemplos
Para encerrar os processos com 1230 IDs de processo, 1241 e 1253, digite:
```
taskkill /pid 1230 /pid 1241 /pid 1253
```
Ao final de modo forçado o processo de "Notepad.exe" se ele foi iniciado pelo sistema, digite:
```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```
Para encerrar todos os processos no computador remoto "Srvmain" com uma imagem nome que começa com "note", ao mesmo tempo, usando as credenciais da conta de usuário Hiropln, digite:
```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```
Para finalizar o processo com o processo de identificação 2134 e qualquer filho processos que ele iniciou, mas somente se esses processos foram iniciados pela conta de administrador, digite:
```
taskkill /pid 2134 /t /fi "username eq administrator"
```
Para encerrar todos os processos que têm uma ID de processo maior que ou igual a 1000, independentemente de seus nomes de imagem, digite:
```
taskkill /f /fi "PID ge 1000" /im *
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
