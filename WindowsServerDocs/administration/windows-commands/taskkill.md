---
title: taskkill
description: Artigo de referência para taskkill, que encerra uma ou mais tarefas ou processos.
ms.topic: reference
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 71a443f4b588139a60b2330e668919a23250ba0f
ms.sourcegitcommit: 00406560a665a24d5a2b01c68063afdba1c74715
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91716887"
---
# <a name="taskkill"></a>taskkill

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Termina uma ou mais tarefas ou processos. Os processos podem ser encerrados pela identificação do processo ou nome da imagem. **Taskkill** substitui a ferramenta **Kill** .

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```

### <a name="parameters"></a>Parâmetros

|         Parâmetro         |                                                                                                                                        Descrição                                                                                                                                        |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /s \<computer>       |                                                                                    Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                                                                     |
| /u \<Domain>\\\<UserName> | Executa o comando com as permissões de conta do usuário que é especificado por *username* ou *Domain* \\ *username*. **/u** pode ser especificado somente se **/s** for especificado. O padrão é as permissões do usuário que está conectado no momento no computador que está emitindo o comando. |
|      /p \<Password>       |                                                                                                   Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                                                                   |
|       /Fi \<Filter>       |          Aplica um filtro para selecionar um conjunto de tarefas. Você pode usar mais de um filtro ou usar o caractere curinga ( `*` ) para especificar todas as tarefas ou nomes de imagem. Consulte a tabela a seguir [para obter nomes de filtro](#filter-names-operators-and-values), operadores e valores válidos.           |
|     /PID \<ProcessID>     |                                                                                                                 Especifica a ID do processo a ser encerrada.                                                                                                                 |
|     /im \<ImageName>      |                                                                                Especifica o nome da imagem do processo a ser encerrado. Use o caractere curinga ( `*` ) para especificar todos os nomes de imagem.                                                                                |
|            /f             |                                                                    Especifica que os processos são encerrados de modo forçado. Esse parâmetro é ignorado para processos remotos; todos os processos remotos são encerrados de modo forçado.                                                                     |
|            /t             |                                                                                                          Encerra o processo especificado e todos os processos filho iniciados por ele.                                                                                                          |

#### <a name="filter-names-operators-and-values"></a>Filtrar nomes, operadores e valores

| Nome do filtro |    Operadores válidos     |                                                                Valor (es) válido (s)                                                                |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   STATUS    |         eq, ne         |                                                 A EXECUÇÃO DO &#124; NÃO ESTÁ RESPONDENDO &#124; DESCONHECIDO                                                 |
|  IMAGENAME  |         eq, ne         |                                                                  Nome da imagem                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  Valor PID                                                                   |
|   SESSION   | eq, ne, gt, lt, ge, le |                                                                Número da sessão                                                                |
|   CPUtime   | eq, ne, gt, lt, ge, le | O tempo de CPU no formato <em>hh</em>**:**<em>mm</em>**:**<em>SS</em>, em que *mm* e *SS* estão entre 0 e 59 e *hh* é qualquer número não assinado |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              Uso de memória em KB                                                              |
|  USERNAME   |         eq, ne         |                                               Qualquer nome de usuário válido *(usuário* ou usuário de *domínio* \\ *User*)                                               |
|  SERVIÇOS   |         eq, ne         |                                                                 Nome do serviço                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 Título da janela                                                                 |
|   MÓDULOS   |         eq, ne         |                                                                   Nome da DLL                                                                   |

## <a name="remarks"></a>Comentários
* Não há suporte para os filtros de STATUS e WINDOWTITLE quando um sistema remoto é especificado.
* O caractere curinga ( `*` ) é aceito para a opção **/im** somente quando um filtro é aplicado.
* O encerramento de processos remotos sempre é executado de modo forçado, independentemente de a opção **/f** ser especificada.
* O fornecimento de um nome de computador para o filtro de nome de host causa um desligamento e todos os processos são interrompidos.
* Você pode usar o **TaskList** para determinar a ID do processo (PID) do processo a ser encerrado.

## <a name="examples"></a>Exemplos

Para finalizar os processos com as IDs de processo 1230, 1241 e 1253, digite:

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

Para forçar o encerramento do processo Notepad.exe se ele foi iniciado pelo sistema, digite:

```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```

Para finalizar todos os processos no computador remoto srvmain com um nome de imagem começando com observação, ao usar as credenciais para a conta de usuário Hiropln, digite:

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```

Para encerrar o processo com a ID de processo 2134 e todos os processos filho que ele iniciou, mas somente se esses processos tiverem sido iniciados pela conta de administrador, digite:

```
taskkill /pid 2134 /t /fi "username eq administrator"
```

Para finalizar todos os processos que têm uma ID de processo maior ou igual a 1000, independentemente de seus nomes de imagem, digite:

```
taskkill /f /fi "PID ge 1000" /im *
```

## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
