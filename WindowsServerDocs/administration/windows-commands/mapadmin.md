---
title: mapadmin
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3ad9f55ba130014227326f4abe8540c78755f6c5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437369"
---
# <a name="mapadmin"></a>mapadmin



Você pode usar **Mapadmin** para gerenciar o mapeamento de nome de usuário para o Microsoft Services para o sistema de arquivos de rede.

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
O **mapadmin** utilitário de linha de comando administra o mapeamento de nome de usuário no computador local ou remoto executando o Microsoft Services para o sistema de arquivos de rede. Se você estiver conectado com uma conta que não tem credenciais administrativas, você pode especificar um nome de usuário e senha de uma conta que possua.

Além dos argumentos de comando específico **mapadmin** aceita os seguintes argumentos e opções:

&lt;computador&gt; Especifica o computador remoto que executa o serviço de mapeamento de nome de usuário que você deseja administrar. Você pode especificar o computador usando um nome de serviço de nome na Internet do Windows (WINS) ou um nome de sistema de nome de domínio (DNS) ou pelo IP (Internet Protocol) de endereço.

-u &lt;usuário&gt; Especifica o nome de usuário do usuário cujas credenciais devem ser usados. Talvez seja necessário adicionar o nome de domínio para o nome de usuário na forma <em>domínio</em> **\\** <em>nome de usuário</em>.

-p &lt;senha&gt; Especifica a senha do usuário. Se você especificar o **-u** opção, mas omita a **-p** opção, será solicitada a senha do usuário.
A ação específica que **mapadmin** executa depende do argumento de comando que você especificar:

## <a name="parameters"></a>Parâmetros
### <a name="start"></a>start
Inicia o serviço de mapeamento de nome de usuário.

### <a name="stop"></a>stop
Interrompe o serviço de mapeamento de nome de usuário.

### <a name="config"></a>config
Especifica as configurações gerais para o mapeamento de nome de usuário. As seguintes opções estão disponíveis com este argumento de comando: **- r &lt;dddd&gt;:&lt;hh&gt;:&lt;mm&gt;**  -Especifica o intervalo de atualização Atualizando a partir de bancos de dados do Windows e o NIS em dias, horas e minutos. O intervalo mínimo é de 5 minutos.
**-i {Sim | não}** -liga mapeamento simples (**Sim**) ou desativado (**nenhum**). Por padrão, o mapeamento simples está ativada.
**Adicionar** -cria um novo mapeamento para um usuário ou grupo. As seguintes opções estão disponíveis com este argumento de comando:

|Opção|Definição|
|-----|-------|
|-wu &lt;name&gt;|Especifica o nome do usuário Windows para o qual está sendo criado um novo mapeamento.|
|-uu &lt;name&gt;|Especifica o nome do usuário de UNIX para o qual está sendo criado um novo mapeamento.|
|-wg &lt;group&gt;|Especifica o nome do grupo Windows para o qual está sendo criado um novo mapeamento.|
|-ug &lt;grupo&gt;|Especifica o nome do grupo de UNIX para o qual está sendo criado um novo mapeamento.|
|-setprimary|Especifica que o novo mapeamento é o mapeamento do primário.|

**setprimary** -Especifica que o mapeamento é o mapeamento primário para um usuário de UNIX ou grupo com vários mapeamentos. As seguintes opções estão disponíveis com este argumento de comando:

|Opção|Definição|
|-----|-------|
|-wu &lt;name&gt;|Especifica o usuário do Windows do mapeamento primário. Se existir mais de um mapeamento para o usuário, use o **- uu** opção para especificar o mapeamento do primário.|
|-uu &lt;name&gt;|Especifica o usuário do UNIX do mapeamento primário.|
|-wg &lt;group&gt;|Especifica o grupo do Windows do mapeamento primário. Se existir mais de um mapeamento para o grupo, use o **- ug** opção para especificar o mapeamento do primário.|
|-ug &lt;grupo&gt;|Especifica o grupo do UNIX do mapeamento primário.|

**Excluir** -remove o mapeamento para um usuário ou grupo. As seguintes opções estão disponíveis para este argumento de comando:

|Opção|Definição|
|-----|-------|
|-wu &lt;user&gt;|O usuário do Windows para o qual o mapeamento será excluído, especificado como &lt; *WindowsDomain&gt;\\&lt;nome de usuário&gt;* . Você deve especificar o **- wu** ou o **- uu** opção, ou ambos. Se você especificar ambas as opções, o mapeamento específico identificado por duas opções será excluído. Se você especificar apenas o **- wu** opção, todos os mapeamentos para o usuário especificado será excluído.|
|-wg &lt;group&gt;|O grupo do Windows para o qual o mapeamento será excluído, especificado como &lt;WindowsDomain&gt;\\&lt;groupname&gt;. Você deve especificar o **- wg** ou o **- ug** opção, ou ambos. Se você especificar ambas as opções, o mapeamento específico identificado por duas opções será excluído. Se você especificar apenas o **- wg** opção, todos os mapeamentos para o grupo especificado será excluído.|
|-uu &lt;user&gt;|O usuário do UNIX para o qual o mapeamento será excluído, especificado como &lt;nome de usuário&gt;. Você deve especificar o **- wu** ou o **- uu** opção, ou ambos. Se você especificar ambas as opções, o mapeamento específico identificado por duas opções será excluído. Se você especificar apenas o **- uu** opção, todos os mapeamentos para o usuário especificado será excluído.|
|-ug &lt;grupo&gt;|O grupo do UNIX para o qual o mapeamento será excluído, especificado como &lt;groupname&gt;. Você deve especificar o **- wg** ou o **- ug** opção, ou ambos. Se você especificar ambas as opções, o mapeamento específico identificado por duas opções será excluído. Se você especificar apenas o **- ug** opção, todos os mapeamentos para o grupo especificado será excluído.|

**lista** -exibe informações sobre mapeamentos de usuário e grupo. As seguintes opções estão disponíveis com este argumento de comando:

|Opção|Definição|
|-----|-------|
|-all|lista os mapeamentos simples e avançados para usuários e grupos.|
|-simples|lista todos os grupos e usuários mapeados simples.|
|-advanced|lista todos os usuários avançados de mapeada e grupos. Mapas são listados na ordem em que eles são avaliados. Mapas primários, marcados com um asterisco (*) são listados primeiro, seguido de mapas secundários, que são marcados com um acento circunflexo (^).|
|-wu &lt;name&gt;|lista o mapeamento para um usuário do Windows especificado.|
|-wg &lt;group&gt;|lista o mapeamento para um grupo do Windows.|
|-uu &lt;name&gt;|lista o mapeamento para um usuário do UNIX.|
|-ug &lt;grupo&gt;|lista o mapeamento para um grupo do UNIX.|

**backup** - nome de usuário salva o mapeamento de configuração e mapear dados para o arquivo especificado por &lt;filename&gt;.
**Restaure** -substitui dados de configuração e o mapeamento de dados do arquivo (especificado por &lt;filename&gt;) que foi criado usando o **backup** argumento de comando.
**adddomainmap** -adiciona um mapa simple entre um domínio do Windows e um arquivos de grupo e senha ou de domínio NIS. As seguintes opções estão disponíveis para este argumento de comando:

|Opção|Definição|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Especifica o domínio do Windows a serem mapeados.|
|-y &lt;NISdomain&gt;|Especifica o domínio NIS a serem mapeados. &lt;br /&gt;&lt;br /&gt; **- n** &lt;nisServer&gt; Especifica o servidor de NIS para o domínio NIS especificado com o **- y**opção.|
|-f &lt;path&gt;|Especifica o caminho totalmente qualificado do diretório que contém os arquivos de senha e de grupo a ser mapeado. Os arquivos devem estar localizados no computador que está sendo gerenciado e não é possível usar **mapadmin** para gerenciar um computador remoto para configurar mapas com base em senha e grupo de arquivos.|

**removedomainmap** -remove um mapa simple entre um domínio do Windows e um domínio NIS. As seguintes opções e argumentos estão disponíveis para este argumento de comando:

|Opção|Definição|
|-----|-------|
|-d &lt;WindowsDomain&gt;|Especifica o domínio do Windows do mapa a ser removido.|
|-y &lt;NISdomain&gt;|Especifica o domínio NIS do mapa a ser removido.|
|-all|Especifica que todos os mapeamentos simples entre domínios do Windows e o NIS serão removidas. Isso também removerá qualquer mapa simple entre um arquivos de grupo e senha e domínio do Windows.|

**listdomainmaps** -lista os domínios do Windows que são mapeados para domínios NIS ou arquivos de senha e de grupo.

## <a name="notes"></a>Observações
-   Se você não especificar um argumento de comando **mapadmin** exibe as configurações atuais para o mapeamento de nome de usuário.
-   para todas as opções que especificam um nome de grupo ou usuário, podem ser usados os seguintes formatos:
-   para usuários do Windows, use o formulário &lt;domínio&gt;\\&lt;nome de usuário&gt;, \\ \\ &lt;computador&gt;\\&lt;usuário nome da&gt;, \\ &lt;computador&gt;\\&lt;nome de usuário&gt;, ou &lt;computador&gt;\\&lt;usuário nome&gt;
-   para grupos do Windows, use o formulário &lt;domínio&gt;\\&lt;&lt;groupname&gt;&gt;, \\ \\ &lt;computador&gt; \\ &lt; &lt;groupname&gt;&gt;, \\ &lt;computador&gt;\\&lt;&lt;groupname&gt; &gt;, ou &lt;computador&gt;\\&lt;&lt;groupname&gt;&gt;
-   para usuários do UNIX, use o formulário &lt;NISdomain&gt;\\&lt;nome de usuário&gt;, &lt;nome de usuário&gt;@&lt;NISdomain&gt;, usuário &lt;nome&gt;@PCNFS, ou PCNFS\\&lt;nome de usuário&gt;
-   para grupos de UNIX, use o formulário &lt;NISdomain&gt;\\&lt;groupname&gt;, &lt;groupname&gt;@&lt;NISdomain&gt;, &lt;groupname&gt;@PCNFS, ou PCNFS\\&lt;groupname&gt;

## <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
