---
title: ipconfig
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15071c2c-4815-4893-93b2-ab30232e312e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fff4e5088e3e08cf2e9e742d8fb6aa2187bb9e83
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438136"
---
# <a name="ipconfig"></a>ipconfig



Exibe todos os valores de configuração de rede TCP/IP e atualiza as configurações de protocolo de configuração de Host dinâmico (DHCP) e o sistema de nome de domínio (DNS). Usado sem parâmetros, **ipconfig** exibe o protocolo IP versão 4 (IPv4) e IPv6 endereços, máscara de sub-rede e gateway padrão para todos os adaptadores.

## <a name="syntax"></a>Sintaxe

```
ipconfig [/allcompartments] [/all] [/renew [<Adapter>]] [/release [<Adapter>]] [/renew6[<Adapter>]] [/release6 [<Adapter>]] [/flushdns] [/displaydns] [/registerdns] [/showclassid <Adapter>] [/setclassid <Adapter> [<ClassID>]]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/all|Exibe a configuração de TCP/IP completa para todos os adaptadores. Os adaptadores podem representar interfaces físicas, como adaptadores de rede instalados, ou interfaces lógicas, como conexões dial-up.|
|/allcompartments|Exibe a configuração de TCP/IP completa para todos os compartimentos.|
|/displaydns|Exibe o conteúdo do cache do resolvedor DNS client, que inclui entradas pré-carregadas a partir do arquivo Hosts local e qualquer adquiriu recentemente a registros de recursos para consultas de nome resolvidos pelo computador. O serviço cliente DNS usa essas informações para resolver nomes consultados com frequência rapidamente, antes de consultar os servidores DNS configurados.|
|/flushdns|Libera e redefine o conteúdo do cache do resolvedor de cliente DNS. Durante a solução de problemas de DNS, você pode usar este procedimento para descartar entradas de cache negativo do cache, bem como quaisquer outras entradas que foram adicionadas dinamicamente.|
|/registerdns|Inicia o registro dinâmico manual de nomes DNS e endereços IP que são configurados em um computador. Você pode usar esse parâmetro para solucionar problemas de registro de nome DNS com falha ou resolver um problema de atualização dinâmica entre um cliente e o servidor DNS sem reinicializar o computador cliente. As configurações de DNS nas propriedades avançadas do protocolo TCP/IP determinam quais nomes estão registrados no DNS.|
|/ versão [\<adaptador >]|Envia uma mensagem DHCPRELEASE ao servidor DHCP para liberar a configuração DHCP atual e descartar a configuração do endereço IP para todos os adaptadores (se um adaptador não for especificado) ou para um adaptador específico se o *adaptador* parâmetro seja incluído. Esse parâmetro desabilita o TCP/IP para adaptadores configurados para obter um endereço IP automaticamente. Para especificar um nome de adaptador, digite o nome do adaptador que aparece quando você usa **ipconfig** sem parâmetros.|
|/release6[\<Adapter>]|Envia uma mensagem DHCPRELEASE para o servidor DHCPv6 para liberar a configuração DHCP atual e descartar a configuração do endereço IPv6 para todos os adaptadores (se um adaptador não for especificado) ou para um adaptador específico se o *adaptador* parâmetro seja incluído. Esse parâmetro desabilita o TCP/IP para adaptadores configurados para obter um endereço IP automaticamente. Para especificar um nome de adaptador, digite o nome do adaptador que aparece quando você usa **ipconfig** sem parâmetros.|
|/ Renovar [\<adaptador >]|Atualiza a configuração de DHCP para todos os adaptadores (se um adaptador não for especificado) ou para um adaptador específico se o *adaptador* parâmetro seja incluído. Esse parâmetro está disponível somente em computadores com adaptadores configurados para obter um endereço IP automaticamente. Para especificar um nome de adaptador, digite o nome do adaptador que aparece quando você usa **ipconfig** sem parâmetros.|
|/renew6 [\<adaptador >]|Renova a configuração de DHCPv6 para todos os adaptadores (se um adaptador não for especificado) ou para um adaptador específico se o *adaptador* parâmetro seja incluído. Esse parâmetro está disponível somente em computadores com adaptadores configurados para obter um endereço IPv6 automaticamente. Para especificar um nome de adaptador, digite o nome do adaptador que aparece quando você usa **ipconfig** sem parâmetros.|
|/setclassid \<Adapter>[ <ClassID>]|Configura a identificação de classe do DHCP para um adaptador especificado. Para definir a ID de classe do DHCP para todos os adaptadores, use o asterisco ( **&#42;** ) o caractere curinga no lugar de *adaptador*. Esse parâmetro está disponível somente em computadores com adaptadores configurados para obter um endereço IP automaticamente. Se uma ID de classe do DHCP não for especificada, a ID de classe atual é removida.|
|/showclassid \<adaptador >|Exibe a ID de classe do DHCP para um adaptador especificado. Para ver a ID de classe do DHCP para todos os adaptadores, use o asterisco ( **&#42;** ) o caractere curinga no lugar de *adaptador*. Esse parâmetro está disponível somente em computadores com adaptadores configurados para obter um endereço IP automaticamente.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Esse comando é muito útil em computadores que estão configurados para obter um endereço IP automaticamente. Isso permite que os usuários determinar quais valores de configuração de TCP/IP foram configurados pelo DHCP, o endereçamento IP privado automático (APIPA) ou uma configuração alternativa.
- Se o nome que você fornecer para *adaptador* contiver espaços, use aspas ao redor do nome do adaptador (exemplo: **"** <em>Nome do adaptador</em> **"** ).
- Para nomes de adaptador **ipconfig** oferece suporte ao uso do asterisco ( *) caractere curinga para especificar qualquer um dos adaptadores com nomes que começam com uma cadeia de caracteres especificada ou adaptadores com nomes que contêm uma cadeia de caracteres especificada. Por exemplo, **Local\\** *   corresponde a todos os adaptadores que começam com a cadeia de caracteres Local e  **\*Con\\** * corresponde a todos os adaptadores que contêm o cadeia de caracteres de Con.

## <a name="examples"></a>Exemplos

Para exibir a configuração básica do TCP/IP para todos os adaptadores, digite:
```
ipconfig
```
Para exibir a configuração de TCP/IP completa para todos os adaptadores, digite:
```
ipconfig /all
```
Para renovar uma configuração de endereço IP atribuído pelo DHCP para apenas o adaptador de Conexão de área Local, digite:
```
ipconfig /renew "Local Area Connection"
```
Para liberar o cache do resolvedor DNS ao solucionar problemas de resolução de nomes DNS, digite:
```
ipconfig /flushdns
```
Para exibir a ID de classe do DHCP para todos os adaptadores com nomes que começam com o Local, digite:
```
ipconfig /showclassid Local*
```
Para definir a ID de classe do DHCP para o adaptador de Conexão de área Local para teste, digite:
```
ipconfig /setclassid "Local Area Connection" TEST
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)