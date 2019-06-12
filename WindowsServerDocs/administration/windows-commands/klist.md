---
title: klist
description: 'Tópico de comandos do Windows para * * *- '
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
ms.openlocfilehash: d0b77aed5970c74181ba03da5e57e9b230313a15
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438117"
---
# <a name="klist"></a>klist



Exibe uma lista de tíquetes de Kerberos armazenados em cache no momento. Essas informações se aplicam ao Windows Server 2012. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
klist [-lh <LogonId.HighPart>] [-li <LogonId.LowPart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-lh|Indica a parte alta de identificador de local exclusivo do usuário (LUID), expressada em hexadecimal. Se nem lh – ou – li estiverem presentes, o comando assume como padrão o LUID do usuário que está atualmente conectado.|
|-li|Indica que a parte inferior do identificador de local exclusivo do usuário (LUID), expressada em hexadecimal. Se nem lh – ou – li estiverem presentes, o comando assume como padrão o LUID do usuário que está atualmente conectado.|
|tíquetes|Lista os atualmente armazenada em cache ticket-granting-tickets (TGTs) e tíquetes de serviço da sessão de logon especificada. Essa é a opção padrão.|
|tgt|Exibe o TGT. Kerberos inicial|
|limpar|Permite que você exclua todos os tíquetes de sessão especificado.|
|Sessões|Exibe uma lista de sessões de logon neste computador.|
|kcd_cache|Exibe o Kerberos restrita informações de cache de delegação.|
|obter|Permite que você solicite um tíquete para o computador de destino especificado pelo nome da entidade de serviço (SPN).|
|add_bind|Permite que você especifique um controlador de domínio preferencial para a autenticação Kerberos.|
|query_bind|Exibe uma lista de controladores de domínio preferencial em cache para cada domínio que entrou em contato Kerberos.|
|purge_bind|Remove o armazenados em cache preferencial controladores de domínio para os domínios especificados.|
|kdcoptions|Exibe as opções do Centro de distribuição de chaves (KDC) especificadas no RFC 4120.|
|/?|Exibe a Ajuda para este comando.|

## <a name="remarks"></a>Comentários

Associação na **Admins. do domínio**, ou equivalente, é o mínimo necessário para executar todos os parâmetros desse comando.

Se nenhum parâmetro for fornecido, Klist irá recuperar todos os tíquetes para o usuário conectado no momento.

Os parâmetros de exibem as informações a seguir:
-   **tickets**

    Lista os tíquetes em cache no momento de serviços que você foi autenticado para desde o logon. Exibe os seguintes atributos de todos os tíquetes em cache:  
    -   Identificação de logon: A LUID
    -   Cliente: A concatenação do nome do cliente e o nome de domínio do cliente
    -   Servidor: A concatenação do nome do serviço e o nome de domínio do serviço
    -   Tipo de criptografia KerbTicket: O tipo de criptografia que é usado para criptografar o tíquete Kerberos
    -   Sinalizadores de permissão: Os sinalizadores de tíquete do Kerberos
    -   Hora de início: A hora do qual o tíquete seja válido
    -   Hora de término: A hora em que o tíquete torna-se não for mais válido. Quando um tíquete for após esse tempo, ele não pode ser usado para autenticar um serviço ou a ser usado para a renovação
    -   Tempo de renovação: A hora em que uma nova autenticação inicial é necessária
    -   Tipo de chave de sessão: O algoritmo de criptografia que é usado para a chave de sessão
-   **tgt**

    Lista o TGT de Kerberos inicial e os seguintes atributos do tíquete atualmente armazenada em cache:  
    -   Identificação de logon: Identificado em hexadecimal
    -   ServiceName: krbtgt
    -   TargetName \<SPN >: krbtgt
    -   DomainName: Nome do domínio que emite o TGT
    -   TargetDomainName: Domínio que o TGT é emitido para
    -   AltTargetDomainName: Domínio que o TGT é emitido para
    -   Sinalizadores de permissão: Tipo e ações de endereço e de destino
    -   Chave de sessão: Algoritmo de criptografia e o comprimento de chave
    -   Hora de início: Hora do computador local que o tíquete foi solicitado
    -   EndTime: Hora em que o tíquete torna-se não for mais válido. Quando um tíquete for após esse tempo, ele não pode ser usado para autenticar um serviço.
    -   RenewUntil: Prazo final para a renovação do tíquete
    -   TimeSkew: Diferença de tempo com o Centro de distribuição de chaves (KDC)
    -   EncodedTicket: Tíquete codificado
-   **purge**

    Permite que você exclua uma permissão específica. A eliminação dos tíquetes destrói todos os tíquetes que você tenha armazenado em cache, portanto, use esse atributo com cuidado. Ele provavelmente o impedirão de ser capaz de se autenticar nos recursos. Se isso acontecer, você terá que fazer logoff e logon novamente.  
    -   Identificação de logon: Identificado em hexadecimal
-   **Sessões**

    Permite que você listar e exibir as informações para todas as sessões de logon neste computador.  
    -   Identificação de logon: Se for especificado, exibe a sessão de logon somente pelo valor determinado. Se não for especificado, exibe todas as sessões de logon neste computador.
-   **kcd_cache**

    Permite que você exiba as informações de cache de delegação restringido de Kerberos.  
    -   Identificação de logon: Se for especificado, exibe as informações de cache para a sessão de logon por determinado valor. Se não for especificado, exibe as informações de cache de sessão de logon do usuário atual.
-   **get**

    Permite que você solicite um tíquete para o destino especificado pelo SPN.  
    -   Identificação de logon: Se especificado, solicita um tíquete usando-se a sessão de logon pelo valor determinado. Se não for especificado, solicita um tíquete usando a sessão de logon do usuário atual.
    -   kdcoptions: Solicita um tíquete com as opções especificadas do KDC
-   **add_bind**

    Permite que você especifique um controlador de domínio preferencial para a autenticação Kerberos.
-   **query_bind**

    Permite que você exiba os controladores de domínio preferido, armazenado em cache para os domínios.
-   **purge_bind**

    Permite que você remova os controladores de domínio preferido, armazenado em cache para os domínios.
-   **kdcoptions**

    Para obter a lista atual das opções e suas explicações, consulte [4120 RFC](http://www.ietf.org/rfc/rfc4120.txt).

**Outras considerações**
-   Klist.exe está disponível no Windows Server 2012 e Windows 8, e não requer nenhuma instalação especial.

## <a name="BKMK_Examples"></a>Exemplos

1. Quando estiver diagnosticando um 27 de ID de evento durante o processamento de um serviço de concessão de tíquete (TGS) solicitação para o servidor de destino, a conta não tem uma chave adequada para gerar um tíquete Kerberos. Você pode usar Klist para consultar o cache de tíquete Kerberos para determinar se qualquer tíquetes estão ausentes, se o servidor de destino ou a conta é um erro ou se não há suporte para o tipo de criptografia.  
   ```
   klist 
   ```  
   ```
   klist –li 0x3e7
   ```  
2. Quando você diagnosticar erros de e para conhecer as especificidades de cada tíquete--tíquete de concessão que é armazenado em cache no computador para uma sessão de logon, você pode usar Klist para exibir as informações de TGT.  
   ```
   klist tgt
   ```  
3. Se não for possível estabelecer uma conexão e o diagnóstico poderá levar muito tempo, limpar o cache de tíquete Kerberos, faça logoff e logon novamente.  
   ```
   klist purge
   ```  
   ```
   klist purge –li 0x3e7
   ```  
4. Quando você deseja diagnosticar uma sessão de logon para um usuário ou um serviço, você pode usar o comando a seguir para localizar a identificação de logon que é usada em outros comandos Klist.  
   ```
   klist sessions
   ```  
5. Quando você deseja diagnosticar uma falha de delegação restrita de Kerberos, você pode usar o comando a seguir para localizar o último erro foi encontrado.  
   ```
   klist kcd_cache
   ```  
6. Quando você deseja diagnosticar se um usuário ou um serviço pode obter um tíquete em um servidor, você pode usar esse comando para solicitar um tíquete para um SPN específico.  
   ```
   klist get host/%computername%
   ```  
7. Ao diagnosticar problemas de replicação entre controladores de domínio, você normalmente precisa o computador cliente para ter como destino um controlador de domínio específico. Nesses casos, você pode usar o comando a seguir para ter como destino o computador cliente para esse controlador de domínio específico.  
   ```
   klist add_bind CONTOSO KDC.CONTOSO.COM
   ```  
   ```
   klist add_bind CONTOSO.COM KDC.CONTOSO.COM
   ```  
8. Para consultar quais controladores de domínio deste computador contatado recentemente, você pode usar o comando a seguir.  
   ```
   klist query_bind
   ```  
9. Quando desejar que o Kerberos para redescobrir os controladores de domínio, você pode usar o comando a seguir. Este comando também pode ser usado para liberar o cache antes de criar novas associações de controlador de domínio com add_bind klist.  
   ```
   klist purge_bind
   ```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)