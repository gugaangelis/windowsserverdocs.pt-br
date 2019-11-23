---
title: mapadmin
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc4b76c1989298ea83c480b9c838ce0fc18fef5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373763"
---
# <a name="mapadmin"></a>mapadmin



Você pode usar o **mapadmin** para gerenciar o mapeamento de nomes de usuário para os serviços da Microsoft para o sistema de arquivos de rede.

## <a name="syntax"></a>Sintaxe
```
mapadmin [<computer>] [-u <user> [-p <password>]]
mapadmin [<computer>] [-u <user> [-p <password>]] {start | stop}
mapadmin [<computer>] [-u <user> [-p <password>]] config <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] add -wu <WindowsUser> -uu <UNIXUser> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] add -wg <WindowsGroup> -ug <UNIXGroup> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wu <WindowsUser> [-uu <UNIXUser>]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wg <WindowsGroup> [-ug <UNIXGroup>]
mapadmin [<computer>] [-u <user> [-p <password>]] delete <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] list <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] backup <filename> 
mapadmin [<computer>] [-u <user> [-p <password>]] restore <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] adddomainmap -d <WindowsDomain> {-y <<NISdomain>> | -f <path>}
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -d <WindowsDomain> -y <<NISdomain>>
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -all
mapadmin [<computer>] [-u <user> [-p <password>]] listdomainmaps
```

## <a name="description"></a>Descrição
O utilitário de linha de comando **mapadmin** administra mapeamento de nomes de usuário no computador local ou remoto que executa os serviços da Microsoft para o sistema de arquivos de rede. Se você estiver conectado com uma conta que não tem credenciais administrativas, poderá especificar um nome de usuário e senha de uma conta que o faça.

Além dos argumentos de comando específicos, o **mapadmin** aceita os seguintes argumentos e opções:

&lt;computador&gt; especifica o computador remoto que executa o serviço de Mapeamento de Nomes de Usuário que você deseja administrar. Você pode especificar o computador usando um nome WINS (serviço de cadastramento na Internet do Windows) ou um nome DNS (sistema de nomes de domínio) ou por endereço IP (Internet Protocol).

-u &lt;usuário&gt; especifica o nome de usuário do usuário cujas credenciais devem ser usadas. Pode ser necessário adicionar o nome de domínio ao nome de usuário no formato <em>domínio</em> **\\** <em>nome de usuário</em>.

-p &lt;senha&gt; especifica a senha do usuário. Se você especificar a opção **-u** , mas omitir a opção **-p** , será solicitada a senha do usuário.
A ação específica que o **mapadmin** executa depende do argumento de comando que você especificar:

## <a name="parameters"></a>Parâmetros
### <a name="start"></a>start
inicia o serviço Mapeamento de Nomes de Usuário.

### <a name="stop"></a>stop
Interrompe o serviço de Mapeamento de Nomes de Usuário.

### <a name="config"></a>configuração
Especifica as configurações gerais para Mapeamento de Nomes de Usuário. As opções a seguir estão disponíveis com este argumento de comando: **-r &lt;dddd&gt;:&lt;hh&gt;:&lt;mm&gt;** -especifica o intervalo de atualização para atualização dos bancos de dados do Windows e NIS em dias, horas e minutos. O intervalo mínimo é de 5 minutos.
**-i {yes | no}** -ativa o mapeamento simples (**Sim**) ou desativado (**não**). Por padrão, o mapeamento simples está ativado.
**Adicionar** – cria um novo mapeamento para um usuário ou grupo. As seguintes opções estão disponíveis com este argumento de comando:

|Opção|Definição|
|-----|-------|
|-o nome de &lt;Wu&gt;|Especifica o nome do usuário do Windows para o qual um novo mapeamento está sendo criado.|
|-UU &lt;nome&gt;|Especifica o nome do usuário UNIX para o qual um novo mapeamento está sendo criado.|
|-WG &lt;grupo&gt;|Especifica o nome do grupo do Windows para o qual um novo mapeamento está sendo criado.|
|-UG &lt;grupo&gt;|Especifica o nome do grupo UNIX para o qual um novo mapeamento está sendo criado.|
|-setprimary|Especifica que o novo mapeamento é o mapeamento principal.|

**setprimary** – especifica qual mapeamento é o mapeamento principal para um usuário ou grupo do UNIX com vários mapeamentos. As seguintes opções estão disponíveis com este argumento de comando:

|Opção|Definição|
|-----|-------|
|-o nome de &lt;Wu&gt;|Especifica o usuário do Windows do mapeamento primário. Se houver mais de um mapeamento para o usuário, use a opção **-uu** para especificar o mapeamento primário.|
|-UU &lt;nome&gt;|Especifica o usuário UNIX do mapeamento principal.|
|-WG &lt;grupo&gt;|Especifica o grupo do Windows do mapeamento primário. Se houver mais de um mapeamento para o grupo, use a opção **-UG** para especificar o mapeamento primário.|
|-UG &lt;grupo&gt;|Especifica o grupo UNIX do mapeamento primário.|

**excluir** – remove o mapeamento de um usuário ou grupo. As seguintes opções estão disponíveis para este argumento de comando:

|Opção|Definição|
|-----|-------|
|-&gt; de usuário do Wu &lt;|O usuário do Windows para o qual o mapeamento será excluído, especificado como &lt;*WindowsDomain&gt;\\&lt;nome de usuário&gt;* . Você deve especificar o **-Wu** ou a opção **-uu** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar apenas a opção **-Wu** , todos os mapeamentos para o usuário especificado serão excluídos.|
|-WG &lt;grupo&gt;|O grupo do Windows para o qual o mapeamento será excluído, especificado como &lt;WindowsDomain&gt;\\&lt;GroupName&gt;. Você deve especificar a opção **-WG** ou **-UG** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar somente a opção **-WG** , todos os mapeamentos do grupo especificado serão excluídos.|
|-UU &lt;usuário&gt;|O usuário do UNIX para o qual o mapeamento será excluído, especificado como &lt;nome de usuário&gt;. Você deve especificar o **-Wu** ou a opção **-uu** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar apenas a opção **-uu** , todos os mapeamentos para o usuário especificado serão excluídos.|
|-UG &lt;grupo&gt;|O grupo do UNIX para o qual o mapeamento será excluído, especificado como &lt;GroupName&gt;. Você deve especificar a opção **-WG** ou **-UG** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar apenas a opção **-UG** , todos os mapeamentos do grupo especificado serão excluídos.|

**lista** -exibe informações sobre mapeamentos de usuário e grupo. As seguintes opções estão disponíveis com este argumento de comando:

|Opção|Definição|
|-----|-------|
|-todos|lista os mapeamentos simples e avançados para usuários e grupos.|
|– simples|lista todos os usuários e grupos mapeados simples.|
|-avançado|lista todos os usuários e grupos mapeados avançados. Os mapas são listados na ordem em que são avaliados. Mapas primários, marcados com um asterisco (*), são listados primeiro, seguidos por mapas secundários, que são marcados com um quilate (^).|
|-o nome de &lt;Wu&gt;|lista o mapeamento para um usuário do Windows especificado.|
|-WG &lt;grupo&gt;|lista o mapeamento de um grupo do Windows.|
|-UU &lt;nome&gt;|lista o mapeamento para um usuário UNIX.|
|-UG &lt;grupo&gt;|lista o mapeamento de um grupo do UNIX.|

**backup** -salva mapeamento de nomes de usuário configuração e mapeamento de dados para o arquivo especificado por &lt;nome de arquivo&gt;.
**Restore** – substitui a configuração e o mapeamento de dados com dados do arquivo (especificado por &lt;nome de arquivo&gt;) que foi criado usando o argumento de comando **backup** .
**adddomainmap** -adiciona um mapa simples entre um domínio do Windows e um domínio ou senha do NIS e arquivos de grupo. As seguintes opções estão disponíveis para este argumento de comando:

|Opção|Definição|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Especifica o domínio do Windows a ser mapeado.|
|-y &lt;NISdomain&gt;|Especifica o domínio NIS a ser mapeado.&lt;br/&gt;&lt;br/&gt; **-n** &lt;NisServer&gt; especifica o servidor NIS para o domínio NIS especificado com a opção **-y** .|
|-f &lt;caminho&gt;|Especifica o caminho totalmente qualificado do diretório que contém a senha e os arquivos de grupo a serem mapeados. Os arquivos devem estar localizados no computador que está sendo gerenciado e você não pode usar o **mapadmin** para gerenciar um computador remoto para configurar mapas com base em arquivos de senha e de grupo.|

**removedomainmap** -remove um mapa simples entre um domínio do Windows e um domínio NIS. As opções e o argumento a seguir estão disponíveis para este argumento de comando:

|Opção|Definição|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Especifica o domínio do Windows do mapa a ser removido.|
|-y &lt;NISdomain&gt;|Especifica o domínio NIS do mapa a ser removido.|
|-todos|Especifica que todos os mapas simples entre os domínios do Windows e NIS devem ser removidos. Isso também removerá qualquer mapa simples entre um domínio do Windows e a senha e os arquivos de grupo.|

**listdomainmaps** -lista os domínios do Windows que são mapeados para domínios NIS ou senhas e arquivos de grupo.

## <a name="notes"></a>Observações
-   Se você não especificar um argumento de comando, **mapadmin** exibirá as configurações atuais para mapeamento de nomes de usuário.
-   para todas as opções que especificam um nome de usuário ou grupo, os seguintes formatos podem ser usados:
-   para usuários do Windows, use o formato &lt;domínio&gt;\\&lt;nome de usuário&gt;, \\\\&lt;computador&gt;\\&lt;nome de usuário&gt;, \\&lt;computador&gt;\\&lt;nome de usuário&gt;ou &lt;computador&gt;\\&lt;nome de usuário&gt;
-   para grupos do Windows, use o formulário &lt;domínio&gt;\\&lt;&lt;GroupName&gt;&gt;, \\\\&lt;computador&gt;\\&lt;&lt;GroupName&gt;&gt;, \\&lt;computador&gt;\\&lt;&lt;GroupName&gt;&gt;ou &lt;computador&gt;\\&lt;&lt;GroupName&gt;&gt;
-   para usuários do UNIX, use o formulário &lt;NISdomain&gt;\\&lt;nome de usuário&gt;, &lt;nome de usuário&gt;@&lt;NISdomain&gt;, usuário &lt;nome&gt;@PCNFSou PCNFS\\&lt;nome de usuário&gt;
-   para grupos do UNIX, use o formulário &lt;NISdomain&gt;\\&lt;GroupName&gt;, &lt;GroupName&gt;@&lt;NISdomain&gt;, &lt;GroupName&gt;@PCNFSou PCNFS\\&lt;GroupName&gt;

## <a name="additional-references"></a>referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
