---
title: icacls
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: b1aaa329c8925d7fa4245555ed51b08f7366299d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811104"
---
# <a name="icacls"></a>icacls

Exibe ou modifica DACLs (listas de controle de acesso condicional) em arquivos especificados e aplica a DACLs armazenadas a arquivos nos diretórios especificados.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
icacls <FileName> [/grant[:r] <Sid>:<Perm>[...]] [/deny <Sid>:<Perm>[...]] [/remove[:g|:d]] <Sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<Policy>[...]]
icacls <Directory> [/substitute <SidOld> <SidNew> [...]] [/restore <ACLfile> [/c] [/l] [/q]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<FileName>|Especifica o arquivo para o qual exibir DACLs.|
|\<diretório >|Especifica o diretório para o qual exibir DACLs.|
|/t|Executa a operação em todos os arquivos especificados no diretório atual e seus subdiretórios.|
|/c|Continua a operação, apesar dos erros de arquivo. Mensagens de erro ainda serão exibidas.|
|/l|Executa a operação em um link simbólico em comparação com o seu destino.|
|/q|Suprime mensagens de êxito.|
|[/save \<ACLfile> [/t] [/c] [/l] [/q]]|Armazena as DACLs para todos os arquivos correspondentes em *ACLfile* para uso posterior com **/restauração**.|
|[/ setowner \<Username > [/t] [/c] [/l] [/q]]|Altera o proprietário de todos os arquivos correspondentes ao usuário especificado.|
|[/findSID \<Sid> [/t] [/c] [/l] [/q]]|Localiza todos os arquivos correspondentes que contenham uma DACL mencionar explicitamente o identificador de segurança especificados (SID).|
|[/verify [/t] [/c] [/l] [/q]]|Localiza todos os arquivos com as ACLs que não são canônico ou têm comprimentos inconsistentes com contagens ACE (entrada de controle de acesso).|
|[/reset [/t] [/c] [/l] [/q]]|Substitui ACLs padrão herdada ACLs para todos os arquivos correspondentes.|
|[/grant[:r] \<Sid>:<Perm>[...]]|Concede especificado direitos de acesso do usuário. Permissões de substituir permissões explícitas anteriormente concedido.</br>Sem **: r**, as permissões são adicionadas a qualquer explícitas permissões concedidas anteriormente.|
|[/ negar \<Sid >:<Perm>[...]]|Nega explicitamente direitos de acesso do usuário especificado. Uma negação explícita ACE é adicionado para as permissões indicadas e as mesmas permissões em qualquer concessão explícita são removidas.|
|[/remove[:g\|:d]] \<Sid>[...]] [/t] [/c] [/l] [/q]|Remove todas as ocorrências do SID especificado da DACL.</br>**: g** remove todas as ocorrências dos direitos concedidos ao SID especificado.</br>**: 1!d** remove todas as ocorrências de direitos negados para o SID especificado.|
|[/ [(CI)(OI)] de setintegritylevel\<nível >:<Policy>[...]]|Adiciona explicitamente uma ACE de integridade para todos os arquivos correspondentes. *Nível* é especificado como:</br>-   **L**[ow]</br>-   **M**[edium]</br>-   **H**[igh]</br>Opções de herança para a integridade ACE podem preceder o nível e são aplicadas somente aos diretórios.|
|[/substitute \<SidOld> <SidNew> [...]]|Substitui um SID existente (*SidOld*) com um novo SID (*SidNew*). Requer o *Directory* parâmetro.|
|/ restauração \<ACLfile > [/c] [/l] [/q]|Aplica a DACLs armazenadas do *ACLfile* a arquivos no diretório especificado. Requer o *Directory* parâmetro.|
|/inheritancelevel:[e\|d\|r]|Define o nível de herança: <br>  **e** -habilita enheritance <br>**1!d** - desativa a herança e copia as ACEs <br>**r** -remove todas as ACEs de herdados

## <a name="remarks"></a>Comentários

-   SIDs talvez em um formato de nome amigável ou numéricos. Se você usar um formato numérico, fixar o caractere curinga **&#42;** para o início do SID.
-   **icacls** preserva a ordem canônica das entradas ACE como:  
    -   Negações explícitas
    -   Concede explícita
    -   Negações herdadas
    -   Concede herdado
-   *Perm* é uma máscara de permissão que pode ser especificada em uma das seguintes formas:  
    -   Uma sequência de direitos simples:

        **F** (acesso completo)

        **M** (Modificar acesso)

        **RX** (leitura e acesso de execução)

        **R** (acesso somente leitura)

        **W** (acesso somente gravação)
    -   Uma lista separada por vírgulas entre parênteses de direitos específicos:

        **1!d** (excluir)

        **RC** (controle de leitura)

        **WDAC** (Gravar DAC)

        **WO** (proprietário de gravação)

        **S** (Sincronizar)

        **AS** (acesso a segurança do sistema)

        **MA** (máximo permitido)

        **GR** (leitura genérica)

        **GW** (gravação genérica)

        **GE** (execução genérica)

        **GA** (todos os genéricos)

        **Área de trabalho remota** (diretório de dados/lista de leitura)

        **WD** (gravar o arquivo de dados/Adicionar)

        **AD** (acrescentar dados/Adicionar subpasta)

        **REA** (ler atributos estendidos)

        **WEA** (gravar atributos estendidos)

        **X** (executar/completa)

        **Controlador de domínio** (excluir filho)

        **RA** (atributos de leitura)

        **WA** (gravar atributos)
-   Direitos de herança podem preceder qualquer uma *Perm* formulário e elas são aplicadas somente a diretórios:

    **(OI)** : objeto herdar

    **(CI)** : herdar de contêiner

    **(E/S)** : somente herança

    **(NP)** : do not propagate inherit

## <a name="examples"></a>Exemplos

Para salvar as DACLs para todos os arquivos na pasta C:\Windows e seus subdiretórios no arquivo ACLFile, digite:

```
icacls c:\windows\* /save aclfile /t
```

Para restaurar as DACLs para cada arquivo em ACLFile que existe na pasta C:\Windows e seus subdiretórios, digite:

```
icacls c:\windows\ /restore aclfile
```

Para conceder ao usuário permissões de User1 excluir e Gravar DAC em um arquivo chamado "Test1", digite:

```
icacls test1 /grant User1:(d,wdac)
```

Para conceder ao usuário definido pelas permissões de exclusão de SID S-1-1-0 e Gravar DAC em um arquivo, chamado "Test2", digite:

```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
