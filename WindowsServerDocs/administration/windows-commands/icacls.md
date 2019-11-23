---
title: icacls
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 494c87073cfd78c7f5e17c72d4c65bec33a49b98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375492"
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
|\<nome de arquivo >|Especifica o arquivo para o qual exibir as DACLs.|
|\<do diretório >|Especifica o diretório para o qual exibir as DACLs.|
|/t|Executa a operação em todos os arquivos especificados no diretório atual e em seus subdiretórios.|
|/c|Continua a operação, apesar de quaisquer erros de arquivo. As mensagens de erro ainda serão exibidas.|
|/l|Executa a operação em um link simbólico versus seu destino.|
|/q|Suprime as mensagens de êxito.|
|[/Save \<ACLfile > [/t] [/c] [/l] [/q]]|Armazena as DACLs de todos os arquivos correspondentes no *ACLfile* para uso posterior com **/Restore**.|
|[/SetOwner \<nome de usuário > [/t] [/c] [/l] [/q]]|Altera o proprietário de todos os arquivos correspondentes para o usuário especificado.|
|[/findSID \<Sid > [/t] [/c] [/l] [/q]]|Localiza todos os arquivos correspondentes que contêm uma DACL mencionando explicitamente o SID (identificador de segurança) especificado.|
|[/Verify [/t] [/c] [/l] [/q]]|Localiza todos os arquivos com ACLs que não são canônicas ou têm tamanhos inconsistentes com as contagens de ACE (entrada de controle de acesso).|
|[/Reset reiniciar [/t] [/c] [/l] [/q]]|Substitui ACLs por ACLs herdadas padrão para todos os arquivos correspondentes.|
|[/Grant [: r] \<Sid >:<Perm>[...]]|Concede direitos de acesso de usuário especificado. As permissões substituem as permissões explícitas concedidas anteriormente.</br>Sem **: r**, as permissões são adicionadas a quaisquer permissões explícitas concedidas anteriormente.|
|[/Deny \<Sid >:<Perm>[...]]|Nega explicitamente os direitos de acesso de usuário especificados. Uma ACE de negação explícita é adicionada para as permissões declaradas e as mesmas permissões em qualquer concessão explícita são removidas.|
|[/remove [: g\|:d]] \<Sid > [...]] /t /c /l /q|Remove todas as ocorrências do SID especificado da DACL.</br>**: g** remove todas as ocorrências de direitos concedidos para o SID especificado.</br>**:d** remove todas as ocorrências de direitos negados para o SID especificado.|
|[/setintegritylevel [(CI) (OI)]\<nível >:<Policy>[...]]|Adiciona explicitamente uma ACE de integridade a todos os arquivos correspondentes. O *nível* é especificado como:</br>-   **L**[OMO]</br>-   **M**[edium]</br>-   **H**[perior]</br>As opções de herança para a ACE de integridade podem preceder o nível e são aplicadas somente a diretórios.|
|[/substitute \<SidOld > <SidNew> [...]]|Substitui um SID existente (*SidOld*) por um novo SID (*SidNew*). Requer o parâmetro *Directory* .|
|/Restore \<ACLfile > [/c] [/l] [/q]|Aplica as DACLs armazenadas de *ACLfile* a arquivos no diretório especificado. Requer o parâmetro *Directory* .|
|/InheritanceLevel: [e\|d\|r]|Define o nível de herança: <br>  **e** -habilita enheritance <br>**d** -desabilita a herança e copia as ACEs <br>**r** -remove todas as ACEs herdadas

## <a name="remarks"></a>Comentários

-   Os SIDs podem estar na forma de nome numérico ou amigável. Se você usar um formato numérico, afixará o caractere **&#42;** curinga ao início do Sid.
-   o **icacls** preserva a ordem canônica das entradas Ace como:  
    -   Negações explícitas
    -   Concessões explícitas
    -   Negações herdadas
    -   Concessões herdadas
-   *Perm* é uma máscara de permissão que pode ser especificada em uma das seguintes formas:  
    -   Uma sequência de direitos simples:

        **F** (acesso completo)

        **M** (modificar acesso)

        **RX** (acesso de leitura e execução)

        **R** (acesso somente leitura)

        **W** (acesso somente gravação)
    -   Uma lista separada por vírgulas em parênteses de direitos específicos:

        **D** (excluir)

        **RC** (controle de leitura)

        **WDAC** (DAC de gravação)

        **Wo** (proprietário da gravação)

        **S** (sincronizar)

        **Como** (segurança do sistema de acesso)

        **Ma** (máximo permitido)

        **Gr** (leitura genérica)

        **GW** (gravação genérica)

        **GE** (execução genérica)

        **GA** (todos genéricos)

        **RD** (ler dados/diretório de lista)

        **WD** (gravar dados/Adicionar arquivo)

        **Ad** (acrescentar dados/adicionar subdiretório)

        **Rea** (atributos estendidos de leitura)

        **Tem** (atributos estendidos de gravação)

        **X** (executar/percorrer)

        **DC** (excluir filho)

        **Ra** (atributos de leitura)

        **Wa** (atributos de gravação)
-   Os direitos de herança podem preceder o formulário *Perm* e são aplicados somente aos diretórios:

    **(Oi)** : herança de objeto

    **(CI)** : herança de contêiner

    **(E/s)** : herdar somente

    **(NP)** : não propagar herança

## <a name="examples"></a>Exemplos

Para salvar as DACLs de todos os arquivos no diretório C:\Windows e seus subdiretórios no arquivo ACLFile, digite:

```
icacls c:\windows\* /save aclfile /t
```

Para restaurar as DACLs para cada arquivo em ACLFile que existe no diretório C:\Windows e em seus subdiretórios, digite:

```
icacls c:\windows\ /restore aclfile
```

Para conceder ao usuário usuário1 excluir e gravar permissões de DAC em um arquivo chamado "test1", digite:

```
icacls test1 /grant User1:(d,wdac)
```

Para conceder ao usuário definido por SID S-1-1-0 excluir e gravar permissões de DAC em um arquivo, chamado "test2", digite:

```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
