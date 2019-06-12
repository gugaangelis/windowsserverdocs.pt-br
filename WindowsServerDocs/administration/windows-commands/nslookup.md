---
title: nslookup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 194cb96846e42b175978a2f6fc7268a93875d315
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811097"
---
# <a name="nslookup"></a>nslookup

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações que você pode usar para diagnosticar a infra-estrutura do sistema de nome de domínio (DNS). Antes de usar essa ferramenta, você deve estar familiarizado com o funcionamento do DNS. A ferramenta de linha de comando nslookup está disponível somente se você tiver instalado o protocolo TCP/IP.
## <a name="syntax"></a>Sintaxe

```
nslookup [<-SubCommand ...>] [{<computerTofind> | -<Server>}]
nslookup /exit
nslookup /finger [<UserName>] [{[>] <FileName>|[>>] <FileName>}]
nslookup /{help | ?}
nslookup /ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
nslookup /lserver <DNSDomain> 
nslookup /root 
nslookup /server <DNSDomain>
nslookup /set <KeyWord>[=<Value>]
nslookup /set all 
nslookup /set class=<Class>
nslookup /set [no]d2
nslookup /set [no]debug
nslookup /set [no]defname
nslookup /set domain=<DomainName>
nslookup /set [no]ignore
nslookup /set port=<Port>
nslookup /set querytype=<ResourceRecordtype>
nslookup /set [no]recurse
nslookup /set retry=<Number>
nslookup /set root=<RootServer>
nslookup /set [no]search
nslookup /set srchlist=<DomainName>[/...]
nslookup /set timeout=<Number>
nslookup /set type=<ResourceRecordtype>
nslookup /set [no]vc
nslookup /view <FileName>
```

## <a name="parameters"></a>Parâmetros

|                       Parâmetro                       |                                                                                                         Descrição                                                                                                         |
|-------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [nslookup exit Command](nslookup-exit-command.md)   |                                                                                                     Sai **nslookup**.                                                                                                     |
| [nslookup finger Command](nslookup-finger-command.md) |                                                                                  Conecta-se com o servidor de dedo no computador atual.                                                                                   |
|           [nslookup help](nslookup-help.md)           |                                                                                    Exibe um resumo breve dos **nslookup** subcomandos.                                                                                    |
|             [nslookup ls](nslookup-ls.md)             |                                                                                             lista informações de um domínio DNS.                                                                                             |
|        [nslookup lserver](nslookup-lserver.md)        |                                                                                   Altera o servidor padrão para o domínio DNS especificado.                                                                                   |
|           [nslookup root](nslookup-root.md)           |                                                                     Altera o servidor padrão para o servidor para a raiz do espaço de nome de domínio DNS.                                                                     |
|         [nslookup server](nslookup-server.md)         |                                                                                   Altera o servidor padrão para o domínio DNS especificado.                                                                                   |
|            [nslookup set](nslookup-set.md)            |                                                                              Altera as configurações que afetam como função de pesquisas.                                                                               |
|        [nslookup set all](nslookup-set-all.md)        |                                                                                  Imprime os valores atuais das definições de configuração.                                                                                   |
|      [nslookup set class](nslookup-set-class.md)      |                                                                     Altera a classe da consulta. A classe especifica o grupo de protocolo das informações.                                                                     |
|         [nslookup set d2](nslookup-set-d2.md)         |                                                                     Ativa ou desativa o modo de depuração exaustiva. Todos os campos de todos os pacotes são impressos.                                                                      |
|      [nslookup set debug](nslookup-set-debug.md)      |                                                                                               Ativa ou desativa o modo de depuração.                                                                                               |
|                 nslookup /set defname                 |                                            acrescenta o nome de domínio DNS padrão a uma solicitação de pesquisa único componente. Um único componente é um componente que não contém pontos.                                            |
|     [nslookup set domain](nslookup-set-domain.md)     |                                                                                 Altera o nome de domínio DNS padrão para o nome especificado.                                                                                  |
|                 Ignorar /set nslookup                  |                                                                                              Ignora erros de truncamento de pacote.                                                                                              |
|       [nslookup set port](nslookup-set-port.md)       |                                                                          Altera a porta do servidor de nome DNS de TCP/UDP para o valor especificado.                                                                           |
|  [nslookup set querytype](nslookup-set-querytype.md)  |                                                                                       Altera o tipo de registro de recurso para a consulta.                                                                                       |
|    [nslookup set recurse](nslookup-set-recurse.md)    |                                                                    Informa ao servidor de nome DNS para consultar outros servidores se não tiver as informações.                                                                    |
|      [nslookup set retry](nslookup-set-retry.md)      |                                                                                                 Define o número de repetições.                                                                                                 |
|       [nslookup set root](nslookup-set-root.md)       |                                                                                    Altera o nome do servidor raiz usado para consultas.                                                                                    |
|     [nslookup set search](nslookup-set-search.md)     | acrescenta os nomes de domínio DNS na lista de pesquisa de domínio DNS para a solicitação até que uma resposta seja recebida. Isso se aplica quando o conjunto e a pesquisa contêm pelo menos um ponto de solicitação, mas não terminam com um ponto à direita. |
|   [nslookup set srchlist](nslookup-set-srchlist.md)   |                                                                                    Altera a lista de pesquisa e de nome de domínio do DNS padrão.                                                                                     |
|    [nslookup set timeout](nslookup-set-timeout.md)    |                                                                           Altera o número inicial de segundos a aguardar uma resposta a uma solicitação.                                                                           |
|       [nslookup set type](nslookup-set-type.md)       |                                                                                       Altera o tipo de registro de recurso para a consulta.                                                                                       |
|         [nslookup set vc](nslookup-set-vc.md)         |                                                                     Especifica se deve ou não usar um circuito virtual ao enviar solicitações ao servidor.                                                                      |
|           [nslookup view](nslookup-view.md)           |                                                                          classifica e lista a saída de versões anteriores **ls** subcomando ou comandos.                                                                          |

## <a name="remarks"></a>Comentários
- Se *Computador_a_ser_localizado* é um endereço IP e a consulta é para um A ou tipo de registro de recurso do PTR, o nome do computador é retornado. Se *Computador_a_ser_localizado* é um nome e não tem à direita o período, o padrão de nome de domínio DNS é acrescentado ao nome. Esse comportamento depende do estado dos seguintes **definir** subcomandos: **domínio**, **srchlist**, **defname**, e  **pesquisa**.
- Se você digitar um hífen (-) em vez de *Computador_a_ser_localizado*, o prompt de comando muda para **nslookup** modo interativo.
- O comprimento de linha de comando deve ser menor que 256 caracteres.
- **nslookup** tem dois modos: interativos e.
  Se você precisar pesquisar somente uma única parte dos dados, use o modo não interativo. Para o primeiro parâmetro, digite o nome ou endereço IP do computador que você deseja pesquisar. Para o segundo parâmetro, digite o nome ou endereço IP de um servidor de nome DNS. Se você omitir o segundo argumento, **nslookup** usa o servidor de nome DNS padrão.
  Se você precisar pesquisar mais de uma parte dos dados, você pode usar o modo interativo. Digite um hífen (-) para o primeiro parâmetro e o nome ou endereço IP de um servidor de nome DNS para o segundo parâmetro. Ou, omita os dois parâmetros e **nslookup** usa o servidor de nome DNS padrão. A seguir estão algumas dicas sobre como trabalhar no modo interativo:
  -   Para interromper comandos interativos a qualquer momento, pressione CTRL + B.
  -   Para sair, digite **sair**.
  -   Para tratar um comando interno como um nome de computador, preceda-o com o caractere de escape (\\).
  -   Um comando não reconhecido é interpretado como um nome de computador.
- Se a solicitação de pesquisa falhar, **nslookup** imprime uma mensagem de erro. A tabela a seguir lista as possíveis mensagens de erro.
  |**mensagem de erro**|**Descrição**|
  |-----------|----------|
  |`timed out`|O servidor não respondeu a uma solicitação após um determinado período de tempo e um determinado número de repetições. Você pode definir o período de tempo limite com o **definir tempo limite** subcomando. Você pode definir o número de novas tentativas com os **repetição conjunto** subcomando.|
  |`No response from server`|Nenhum servidor de nome DNS está em execução no computador do servidor.|
  |`No records`|O servidor de nomes DNS não tem registros de recurso do tipo de consulta atual para o computador, embora o nome do computador é válido. O tipo de consulta é especificado com o **definir querytype** comando.|
  |`Nonexistent domain`|O computador ou nome de domínio DNS não existe.|
  |`Connection refused`<br /><br />-ou-<br /><br />`Network is unreachable`|Não foi possível estabelecer a conexão com o servidor de nomes DNS ou servidor finger. Esse erro normalmente ocorre com **ls** e **dedo** solicitações.|
  |`Server failure`|O servidor de nomes DNS encontrou uma inconsistência interna em seu banco de dados e não pôde retornar uma resposta válida.|
  |`Refused`|O servidor de nomes DNS se recusou a solicitação de serviço.|
  |`format error`|O servidor de nomes DNS encontrado que o pacote de solicitação não estava no formato correto. Isso pode indicar um erro no **nslookup**.|
- Para obter mais informações sobre o **nslookup** comando e o DNS, consulte os seguintes recursos:
  - Lee, T., Davies, J. 2000. *Referência técnica do Microsoft Windows 2000 TCP/IP Protocols and Services*. Redmond, Washington: Microsoft Press.
  - Albitz, P., Loukides, M. and C. Liu. 2001. *DNS e BIND, quarta edição*. Sebastopol, California: O ' Reilly and associates, Inc.
  - Larson, m e C. Liu. 2001. *DNS no Windows 2000*. Sebastopol, California: O ' Reilly and associates, Inc.
    #### <a name="examples"></a>Exemplos
    Cada opção de linha de comando consiste em um hífen (-) seguido imediatamente por nome do comando e, em alguns casos, um sinal de igual (=) e, em seguida, um valor. Por exemplo, para alterar o tipo de consulta padrão para informações de host (computador) e o tempo limite inicial de 10 segundos, digite: **nslookup - querytype = hinfo - timeout = 10**
    ## <a name="see-also"></a>Consulte também
    [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
