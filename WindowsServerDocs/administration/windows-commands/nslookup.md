---
title: nslookup
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15062d81992ee1b6e55d47cb9e49822350e4f2bc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838089"
---
# <a name="nslookup"></a>nslookup

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações que você pode usar para diagnosticar a infraestrutura do DNS (sistema de nomes de domínio). Antes de usar essa ferramenta, você deve estar familiarizado com o funcionamento do DNS. A ferramenta de linha de comando nslookup só estará disponível se você tiver instalado o protocolo TCP/IP.
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

### <a name="parameters"></a>Parâmetros

|                       Parâmetro                       |                                                                                                         Descrição                                                                                                         |
|-------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [nslookup exit Command](nslookup-exit-command.md)   |                                                                                                     Sai do **nslookup**.                                                                                                     |
| [nslookup finger Command](nslookup-finger-command.md) |                                                                                  Conecta-se ao servidor Finger no computador atual.                                                                                   |
|           [nslookup help](nslookup-help.md)           |                                                                                    Exibe um breve resumo dos subcomandos **nslookup** .                                                                                    |
|             [nslookup ls](nslookup-ls.md)             |                                                                                             Lista informações para um domínio DNS.                                                                                             |
|        [nslookup lserver](nslookup-lserver.md)        |                                                                                   Altera o servidor padrão para o domínio DNS especificado.                                                                                   |
|           [nslookup root](nslookup-root.md)           |                                                                     Altera o servidor padrão para o servidor para a raiz do espaço do nome de domínio DNS.                                                                     |
|         [nslookup server](nslookup-server.md)         |                                                                                   Altera o servidor padrão para o domínio DNS especificado.                                                                                   |
|            [nslookup set](nslookup-set.md)            |                                                                              Altera as definições de configuração que afetam o funcionamento das pesquisas.                                                                               |
|        [nslookup set all](nslookup-set-all.md)        |                                                                                  Imprime os valores atuais dos parâmetros de configuração.                                                                                   |
|      [nslookup set class](nslookup-set-class.md)      |                                                                     Altera a classe de consulta. A classe especifica o grupo de protocolo das informações.                                                                     |
|         [nslookup set d2](nslookup-set-d2.md)         |                                                                     Ativa ou desativa o modo de depuração exaustiva. Todos os campos de cada pacote são impressos.                                                                      |
|      [nslookup set debug](nslookup-set-debug.md)      |                                                                                               Ativa ou desativa o modo de depuração.                                                                                               |
|                 nslookup/Set defname                 |                                            Anexa o nome de domínio DNS padrão a uma única solicitação de pesquisa de componente. Um único componente é um componente que não contém períodos.                                            |
|     [nslookup set domain](nslookup-set-domain.md)     |                                                                                 Altera o nome de domínio DNS padrão para o nome especificado.                                                                                  |
|                 nslookup/Set ignorar                  |                                                                                              Ignora erros de truncamento de pacote.                                                                                              |
|       [nslookup set port](nslookup-set-port.md)       |                                                                          Altera a porta do servidor de nome DNS TCP/UDP padrão para o valor especificado.                                                                           |
|  [nslookup set querytype](nslookup-set-querytype.md)  |                                                                                       Altera o tipo de registro de recurso para a consulta.                                                                                       |
|    [nslookup set recurse](nslookup-set-recurse.md)    |                                                                    Informa ao servidor de nomes DNS para consultar outros servidores se ele não tiver as informações.                                                                    |
|      [nslookup set retry](nslookup-set-retry.md)      |                                                                                                 Define o número de repetições.                                                                                                 |
|       [nslookup set root](nslookup-set-root.md)       |                                                                                    Altera o nome do servidor raiz usado para consultas.                                                                                    |
|     [nslookup set search](nslookup-set-search.md)     | Acrescenta os nomes de domínio DNS na lista de pesquisa de domínio DNS à solicitação até que uma resposta seja recebida. Isso se aplica quando o conjunto e a solicitação de pesquisa contêm pelo menos um ponto, mas não terminam com um ponto à direita. |
|   [nslookup set srchlist](nslookup-set-srchlist.md)   |                                                                                    Altera o nome de domínio DNS padrão e a lista de pesquisa.                                                                                     |
|    [nslookup set timeout](nslookup-set-timeout.md)    |                                                                           Altera o número inicial de segundos para aguardar uma resposta a uma solicitação.                                                                           |
|       [nslookup set type](nslookup-set-type.md)       |                                                                                       Altera o tipo de registro de recurso para a consulta.                                                                                       |
|         [nslookup set vc](nslookup-set-vc.md)         |                                                                     Especifica o uso ou não de um circuito virtual ao enviar solicitações para o servidor.                                                                      |
|           [nslookup view](nslookup-view.md)           |                                                                          Classifica e lista a saída do subcomando ou dos comandos **ls** anteriores.                                                                          |

## <a name="remarks"></a>Comentários
- Se *computerTofind* for um endereço IP e a consulta for para um tipo de registro de recurso A ou PTR, o nome do computador será retornado. Se *computerTofind* for um nome e não tiver um ponto à direita, o nome de domínio DNS padrão será anexado ao nome. Esse comportamento depende do estado dos seguintes subcomandos **set** : **Domain**, **srchlist**, **defname**e **Search**.
- Se você digitar um hífen (-) em vez de *computerTofind*, o prompt de comando será alterado para o modo interativo do **nslookup** .
- O comprimento da linha de comando deve ter menos de 256 caracteres.
- o **nslookup** tem dois modos: interativo e não interativo.
  Se você precisar pesquisar apenas um único dado, use o modo não interativo. Para o primeiro parâmetro, digite o nome ou o endereço IP do computador que você deseja pesquisar. Para o segundo parâmetro, digite o nome ou o endereço IP de um servidor de nomes DNS. Se você omitir o segundo argumento, **nslookup** usará o servidor de nomes DNS padrão.
  Se você precisar pesquisar mais de um dado, poderá usar o modo interativo. Digite um hífen (-) para o primeiro parâmetro e o nome ou endereço IP de um servidor de nomes DNS para o segundo parâmetro. Ou, omita os parâmetros e **nslookup** usa o servidor de nomes DNS padrão. A seguir estão algumas dicas sobre como trabalhar no modo interativo:
  -   Para interromper comandos interativos a qualquer momento, pressione CTRL + B.
  -   Para sair, digite **Exit**.
  -   Para tratar um comando interno como um nome de computador, preceda-o com o caractere de escape (\\).
  -   Um comando não reconhecido é interpretado como um nome de computador.
- Se a solicitação de pesquisa falhar, o **nslookup** imprime uma mensagem de erro. A tabela a seguir lista as possíveis mensagens de erro.
  |**Mensagem de erro**|**Descrição**|
  |-----------|----------|
  |`timed out`|O servidor não respondeu a uma solicitação após determinado período de tempo e um determinado número de tentativas. Você pode definir o período de tempo limite com o subcomando **set timeout** . Você pode definir o número de repetições com o subcomando **set Retry** .|
  |`No response from server`|Nenhum servidor de nome DNS está sendo executado no computador servidor.|
  |`No records`|O servidor de nomes DNS não tem registros de recursos do tipo de consulta atual para o computador, embora o nome do computador seja válido. O tipo de consulta é especificado com o comando **set QueryType** .|
  |`Nonexistent domain`|O computador ou o nome de domínio DNS não existe.|
  |`Connection refused`<p>-ou-<p>`Network is unreachable`|Não foi possível estabelecer a conexão com o servidor de nomes DNS ou com o servidor Finger. Esse erro geralmente ocorre com solicitações **ls** e **Finger** .|
  |`Server failure`|O servidor de nomes DNS encontrou uma inconsistência interna em seu banco de dados e não pôde retornar uma resposta válida.|
  |`Refused`|O servidor de nomes DNS recusou-se a atender à solicitação.|
  |`format error`|O servidor de nomes DNS descobriu que o pacote de solicitação não estava no formato adequado. Isso pode indicar um erro no **nslookup**.|
- Para obter mais informações sobre o comando **nslookup** e o DNS, consulte os seguintes recursos:
  - Lee, T., Davies, J. 2000. *Referência técnica de serviços e protocolos TCP/IP do Microsoft Windows 2000*. Redmond, Washington: Microsoft Press.
  - Albitz, P., Loukides, M. e C. Liu. 2001. *DNS e BIND, quarta edição*. Sebastopol, Califórnia: o ' Reilly and Associates, Inc.
  - Larson, M. e C. Liu. 2001. *DNS no Windows 2000*. Sebastopol, Califórnia: o ' Reilly and Associates, Inc.
    #### <a name="examples"></a>Exemplos
    Cada opção de linha de comando consiste em um hífen (-) seguido imediatamente pelo nome do comando e, em alguns casos, um sinal de igual (=) e, em seguida, um valor. Por exemplo, para alterar o tipo de consulta padrão para informações de host (computador) e o tempo limite inicial para 10 segundos, digite: **nslookup-QueryType = HINFO-Timeout = 10**
    ## <a name="see-also"></a>Consulte também
    - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
