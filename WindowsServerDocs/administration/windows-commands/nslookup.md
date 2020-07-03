---
title: nslookup
description: Artigo de referência para o comando Nslookup, que exibe informações que você pode usar para diagnosticar a infraestrutura de DNS (sistema de nomes de domínio).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87f973349426016b6d62bd1f018f268d4e873c51
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925387"
---
# <a name="nslookup"></a>nslookup

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações que você pode usar para diagnosticar a infraestrutura do DNS (sistema de nomes de domínio). Antes de usar essa ferramenta, você deve estar familiarizado com o funcionamento do DNS. A ferramenta de linha de comando nslookup só estará disponível se você tiver instalado o protocolo TCP/IP.

A ferramenta de linha de comando nslookup tem dois modos: interativo e não interativo.

Se você precisar pesquisar apenas um único dado, é recomendável usar o modo não interativo. Para o primeiro parâmetro, digite o nome ou o endereço IP do computador que você deseja pesquisar. Para o segundo parâmetro, digite o nome ou o endereço IP de um servidor de nomes DNS. Se você omitir o segundo argumento, **nslookup** usará o servidor de nomes DNS padrão.

Se você precisar pesquisar mais de um dado, poderá usar o modo interativo. Digite um hífen (-) para o primeiro parâmetro e o nome ou endereço IP de um servidor de nomes DNS para o segundo parâmetro. Se você omitir os dois parâmetros, a ferramenta usará o servidor de nomes DNS padrão. Ao usar o modo interativo, você pode:

- Interrompa comandos interativos a qualquer momento, pressionando CTRL + B.

- Saia, digitando **sair**.

- Trate um comando interno como um nome de computador, precedendo-o com o caractere de escape ( \) . Um comando não reconhecido é interpretado como um nome de computador.

## <a name="syntax"></a>Sintaxe

```
nslookup [exit | finger | help | ls | lserver | root | server | set | view] [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| [saída do nslookup](nslookup-exit-command.md) | Sai da ferramenta de linha de comando nslookup. |
| [dedo nslookup](nslookup-finger-command.md) | Conecta-se ao servidor Finger no computador atual. |
| [nslookup help](nslookup-help.md) | Exibe um breve resumo dos subcomandos. |
| [nslookup ls](nslookup-ls.md) | Lista informações para um domínio DNS. |
| [nslookup lserver](nslookup-lserver.md) | Altera o servidor padrão para o domínio DNS especificado. |
| [nslookup root](nslookup-root.md) | Altera o servidor padrão para o servidor para a raiz do espaço do nome de domínio DNS. |
| [nslookup server](nslookup-server.md) | Altera o servidor padrão para o domínio DNS especificado. |
| [nslookup set](nslookup-set.md) | Altera as definições de configuração que afetam o funcionamento das pesquisas. |
| [nslookup set all](nslookup-set-all.md) | Imprime os valores atuais dos parâmetros de configuração. |
| [nslookup set class](nslookup-set-class.md) | Altera a classe de consulta. A classe especifica o grupo de protocolo das informações. |
| [nslookup set d2](nslookup-set-d2.md) | Ativa ou desativa o modo de depuração exaustiva. Todos os campos de cada pacote são impressos. |
| [nslookup set debug](nslookup-set-debug.md) | Ativa ou desativa o modo de depuração. |
| [nslookup set domain](nslookup-set-domain.md) | Altera o nome de domínio DNS padrão para o nome especificado. |
| [nslookup set port](nslookup-set-port.md) | Altera a porta do servidor de nome DNS TCP/UDP padrão para o valor especificado. |
| [nslookup set querytype](nslookup-set-querytype.md) | Altera o tipo de registro de recurso para a consulta. |
| [nslookup set recurse](nslookup-set-recurse.md) | Informa ao servidor de nomes DNS para consultar outros servidores se ele não tiver as informações. |
| [nslookup set retry](nslookup-set-retry.md) | Define o número de repetições. |
| [nslookup set root](nslookup-set-root.md) | Altera o nome do servidor raiz usado para consultas. |
| [nslookup set search](nslookup-set-search.md) | Acrescenta os nomes de domínio DNS na lista de pesquisa de domínio DNS à solicitação até que uma resposta seja recebida. Isso se aplica quando o conjunto e a solicitação de pesquisa contêm pelo menos um ponto, mas não terminam com um ponto à direita. |
| [nslookup set srchlist](nslookup-set-srchlist.md) | Altera o nome de domínio DNS padrão e a lista de pesquisa. |
| [nslookup set timeout](nslookup-set-timeout.md) | Altera o número inicial de segundos para aguardar uma resposta a uma solicitação. |
| [nslookup set type](nslookup-set-type.md) | Altera o tipo de registro de recurso para a consulta. |
| [nslookup set vc](nslookup-set-vc.md) | Especifica o uso ou não de um circuito virtual ao enviar solicitações para o servidor. |
| [nslookup view](nslookup-view.md) | Classifica e lista a saída do subcomando ou dos comandos **ls** anteriores. |

### <a name="remarks"></a>Comentários

- Se *computerTofind* for um endereço IP e a consulta for para um tipo de registro de recurso **A** ou **PTR** , o nome do computador será retornado.

- Se *computerTofind* for um nome e não tiver um ponto à direita, o nome de domínio DNS padrão será anexado ao nome. Esse comportamento depende do estado dos seguintes subcomandos **set** : **Domain**, **srchlist**, **defname**e **Search**.

- Se você digitar um hífen (-) em vez de *computerTofind*, o prompt de comando será alterado para o modo interativo do **nslookup** .

- Se a solicitação de pesquisa falhar, a ferramenta de linha de comando fornecerá uma mensagem de erro, incluindo:

  | Mensagem de erro | Descrição |
  | ------------- | ----------- |
  | tempo limite atingido |O servidor não respondeu a uma solicitação após um determinado período de tempo e um determinado número de tentativas. Você pode definir o período de tempo limite com o comando [set timeout do nslookup](nslookup-set-timeout.md) . Você pode definir o número de repetições com o comando [set Retry do nslookup](nslookup-set-retry.md) . |
  | Sem resposta do servidor | Nenhum servidor de nome DNS está sendo executado no computador servidor. |
  | Nenhum registro | O servidor de nomes DNS não tem registros de recursos do tipo de consulta atual para o computador, embora o nome do computador seja válido. O tipo de consulta é especificado com o comando [set QueryType do nslookup](nslookup-set-querytype.md) . |
  | Domínio inexistente | O computador ou o nome de domínio DNS não existe. |
  | Conexão recusada ou rede inacessível | Não foi possível estabelecer a conexão com o servidor de nomes DNS ou com o servidor Finger. Esse erro geralmente ocorre com as solicitações **ls** e **Finger** . |
  | Falha do servidor | O servidor de nomes DNS encontrou uma inconsistência interna em seu banco de dados e não pôde retornar uma resposta válida. |
  | Resposta | O servidor de nomes DNS recusou-se a atender à solicitação. |
  | erro de formato | O servidor de nomes DNS descobriu que o pacote de solicitação não estava no formato adequado. Isso pode indicar um erro no **nslookup**. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
