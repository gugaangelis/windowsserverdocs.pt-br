---
title: klist
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a8f574b65ec8c123379e1b02ee1571cc9f21fa1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867060"
---
# <a name="klist"></a>klist



Exibe uma lista de tíquetes Kerberos atualmente armazenados em cache. Essas informações se aplicam ao Windows Server 2012. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
klist [-lh <LogonId.HighPart>] [-li <LogonId.LowPart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-LH|Denota a parte superior do identificador local exclusivo (LUID) do usuário, expresso em hexadecimal. Se nenhum – LH ou – li estiver presente, o comando usa como padrão o LUID do usuário que está conectado no momento.|
|-Li|Denota a parte inferior do identificador local exclusivo (LUID) do usuário, expresso em hexadecimal. Se nenhum – LH ou – li estiver presente, o comando usa como padrão o LUID do usuário que está conectado no momento.|
|ticket|Lista os tíquetes de concessão de tíquetes (TGTs) e Tíquetes de serviço armazenados em cache no momento da sessão de logon especificada. Essa é a opção padrão.|
|TGT|Exibe o TGT Kerberos inicial.|
|limpar|Permite que você exclua todos os tíquetes da sessão de logon especificada.|
|Das|Exibe uma lista de sessões de logon neste computador.|
|kcd_cache|Exibe as informações de cache de delegação restrita de Kerberos.|
|obter|Permite solicitar um tíquete para o computador de destino especificado pelo SPN (nome da entidade de serviço).|
|add_bind|Permite especificar um controlador de domínio preferencial para a autenticação Kerberos.|
|query_bind|Exibe uma lista de controladores de domínio preferenciais em cache para cada domínio que o Kerberos tenha contatado.|
|purge_bind|Remove os controladores de domínio preferenciais em cache para os domínios especificados.|
|kdcoptions|Exibe as opções de centro de distribuição de chaves (KDC) especificadas no RFC 4120.|
|/?|Exibe a ajuda para este comando.|

## <a name="remarks"></a>Comentários

A associação em **Admins**. do domínio, ou equivalente, é o mínimo necessário para executar todos os parâmetros desse comando.

Se nenhum parâmetro for fornecido, o klist recuperará todos os tíquetes para o usuário conectado no momento.

Os parâmetros exibem as seguintes informações:
-   **ticket**

    Lista os tíquetes atualmente armazenados em cache dos serviços que você autenticou desde o logon. Exibe os seguintes atributos de todos os tíquetes em cache:  
    -   LogonId: O LUID
    -   Cliente: A concatenação do nome do cliente e o nome de domínio do cliente
    -   Servidor: A concatenação do nome do serviço e o nome de domínio do serviço
    -   Tipo de criptografia KerbTicket: O tipo de criptografia usado para criptografar o tíquete Kerberos
    -   Sinalizadores de tíquete: Os sinalizadores de tíquete Kerberos
    -   Hora de início: A hora da qual o tíquete será válido
    -   Hora de término: A hora em que o tíquete se torna não mais válido. Quando um tíquete é passado desta vez, ele não pode mais ser usado para autenticar em um serviço ou ser usado para renovação
    -   Hora da renovação: A hora em que uma nova autenticação inicial é necessária
    -   Tipo de chave de sessão: O algoritmo de criptografia que é usado para a chave de sessão
-   **TGT**

    Lista o TGT Kerberos inicial e os seguintes atributos do tíquete atualmente armazenado em cache:  
    -   LogonId: Identificado em hexadecimal
    -   ServiceName: krbtgt
    -   > \<De SPN de TargetName: krbtgt
    -   DomainName Nome do domínio que emite o TGT
    -   TargetDomainName: Domínio para o qual o TGT é emitido
    -   AltTargetDomainName: Domínio para o qual o TGT é emitido
    -   Sinalizadores de tíquete: Ações de endereço e destino e tipo
    -   Chave de sessão: Comprimento da chave e algoritmo de criptografia
    -   StartTime Hora do computador local em que o tíquete foi solicitado
    -   Final Hora em que o tíquete se torna não mais válido. Quando um tíquete é passado desta vez, ele não pode mais ser usado para autenticar em um serviço.
    -   RenewUntil: Prazo para renovação de tíquete
    -   Distorção de horários: Diferença de tempo com o centro de distribuição de chaves (KDC)
    -   EncodedTicket: Tíquete codificado
-   **depuração**

    Permite que você exclua um tíquete específico. A limpeza de tíquetes destrói todos os tíquetes armazenados em cache, portanto, use esse atributo com cuidado. Ele pode impedir que você possa se autenticar nos recursos. Se isso acontecer, você precisará fazer logoff e logon novamente.  
    -   LogonId: Identificado em hexadecimal
-   **das**

    Permite listar e exibir as informações de todas as sessões de logon neste computador.  
    -   LogonId: Se especificado, exibe a sessão de logon somente pelo valor especificado. Se não for especificado, exibirá todas as sessões de logon neste computador.
-   **kcd_cache**

    Permite que você exiba as informações de cache de delegação restrita de Kerberos.  
    -   LogonId: Se especificado, exibe as informações de cache para a sessão de logon pelo valor especificado. Se não for especificado, exibe as informações de cache para a sessão de logon do usuário atual.
-   **get**

    Permite solicitar um tíquete para o destino especificado pelo SPN.  
    -   LogonId: Se especificado, solicita um tíquete usando a sessão de logon pelo valor especificado. Se não for especificado, o solicitará um tíquete usando a sessão de logon do usuário atual.
    -   kdcoptions: Solicita um tíquete com as opções de KDC fornecidas
-   **add_bind**

    Permite especificar um controlador de domínio preferencial para a autenticação Kerberos.
-   **query_bind**

    Permite exibir controladores de domínio preferenciais em cache para os domínios.
-   **purge_bind**

    Permite remover controladores de domínio preferenciais em cache para os domínios.
-   **kdcoptions**

    Para obter a lista atual de opções e suas explicações, consulte [RFC 4120](http://www.ietf.org/rfc/rfc4120.txt).

**Outras considerações**
-   O klist. exe está disponível no Windows Server 2012 e no Windows 8, e não requer instalação especial.

## <a name="BKMK_Examples"></a>Disso

1. Quando você estiver diagnosticando uma ID de evento 27 durante o processamento de uma solicitação de TGS (serviço de concessão de tíquetes) para o servidor de destino, a conta não terá uma chave adequada para gerar um tíquete Kerberos. Você pode usar klist para consultar o cache de tíquete Kerberos para determinar se algum tíquete está ausente, se o servidor ou a conta de destino estiver em erro ou se não houver suporte para o tipo de criptografia.  
   ```
   klist 
   ```  
   ```
   klist –li 0x3e7
   ```  
2. Ao diagnosticar erros e você deseja saber as especificidades de cada tíquete de concessão de tíquete que é armazenado em cache no computador para uma sessão de logon, você pode usar klist para exibir as informações de TGT.  
   ```
   klist tgt
   ```  
3. Se não for possível estabelecer uma conexão e o diagnóstico puder levar muito tempo, você poderá limpar o cache do tíquete Kerberos, fazer logoff e, em seguida, fazer logon novamente.  
   ```
   klist purge
   ```  
   ```
   klist purge –li 0x3e7
   ```  
4. Quando você deseja diagnosticar uma sessão de logon para um usuário ou um serviço, pode usar o comando a seguir para localizar o LogonId que é usado em outros comandos do klist.  
   ```
   klist sessions
   ```  
5. Quando desejar diagnosticar uma falha de delegação restrita de Kerberos, você poderá usar o comando a seguir para localizar o último erro encontrado.  
   ```
   klist kcd_cache
   ```  
6. Quando desejar diagnosticar se um usuário ou um serviço puder obter um tíquete para um servidor, você poderá usar esse comando para solicitar um tíquete para um SPN específico.  
   ```
   klist get host/%computername%
   ```  
7. Ao diagnosticar problemas de replicação em controladores de domínio, normalmente você precisa do computador cliente para direcionar um controlador de domínio específico. Nesses casos, você pode usar o comando a seguir para direcionar o computador cliente para esse controlador de domínio específico.  
   ```
   klist add_bind CONTOSO KDC.CONTOSO.COM
   ```  
   ```
   klist add_bind CONTOSO.COM KDC.CONTOSO.COM
   ```  
8. Para consultar quais controladores de domínio este computador contatou recentemente, você pode usar o comando a seguir.  
   ```
   klist query_bind
   ```  
9. Quando desejar que o Kerberos redescubra controladores de domínio, você poderá usar o comando a seguir. Esse comando também pode ser usado para liberar o cache antes de criar novas associações de controlador de domínio com klist add_bind.  
   ```
   klist purge_bind
   ```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)