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

&lt;computer @ no__t-1 especifica o computador remoto que executa o serviço de Mapeamento de Nomes de Usuário que você deseja administrar. Você pode especificar o computador usando um nome WINS (serviço de cadastramento na Internet do Windows) ou um nome DNS (sistema de nomes de domínio) ou por endereço IP (Internet Protocol).

-u &lt;user @ no__t-1 especifica o nome de usuário do usuário cujas credenciais devem ser usadas. Pode ser necessário adicionar o nome de domínio ao nome de usuário no formato <em>domínio</em> **\\** <em>nome de usuário</em>.

-p &lt;password @ no__t-1 especifica a senha do usuário. Se você especificar a opção **-u** , mas omitir a opção **-p** , será solicitada a senha do usuário.
A ação específica que o **mapadmin** executa depende do argumento de comando que você especificar:

## <a name="parameters"></a>Parâmetros
### <a name="start"></a>start
inicia o serviço Mapeamento de Nomes de Usuário.

### <a name="stop"></a>stop
Interrompe o serviço de Mapeamento de Nomes de Usuário.

### <a name="config"></a>configuração
Especifica as configurações gerais para Mapeamento de Nomes de Usuário. As opções a seguir estão disponíveis com este argumento de comando: **-r &lt;dddd @ no__t-2: &lt;HH @ no__t-4: &lt;mm @ no__t-6** -especifica o intervalo de atualização para atualização dos bancos de dados do Windows e NIS em dias, horas e minutos. O intervalo mínimo é de 5 minutos.
**-i {yes | no}** -ativa o mapeamento simples (**Sim**) ou desativado (**não**). Por padrão, o mapeamento simples está ativado.
**Adicionar** – cria um novo mapeamento para um usuário ou grupo. As seguintes opções estão disponíveis com este argumento de comando:

|Opção|Definição|
|-----|-------|
|-Wu &lt;name @ no__t-1|Especifica o nome do usuário do Windows para o qual um novo mapeamento está sendo criado.|
|-UU &lt;name @ no__t-1|Especifica o nome do usuário UNIX para o qual um novo mapeamento está sendo criado.|
|-WG &lt;group @ no__t-1|Especifica o nome do grupo do Windows para o qual um novo mapeamento está sendo criado.|
|-UG &lt;group @ no__t-1|Especifica o nome do grupo UNIX para o qual um novo mapeamento está sendo criado.|
|-setprimary|Especifica que o novo mapeamento é o mapeamento principal.|

**setprimary** – especifica qual mapeamento é o mapeamento principal para um usuário ou grupo do UNIX com vários mapeamentos. As seguintes opções estão disponíveis com este argumento de comando:

|Opção|Definição|
|-----|-------|
|-Wu &lt;name @ no__t-1|Especifica o usuário do Windows do mapeamento primário. Se houver mais de um mapeamento para o usuário, use a opção **-uu** para especificar o mapeamento primário.|
|-UU &lt;name @ no__t-1|Especifica o usuário UNIX do mapeamento principal.|
|-WG &lt;group @ no__t-1|Especifica o grupo do Windows do mapeamento primário. Se houver mais de um mapeamento para o grupo, use a opção **-UG** para especificar o mapeamento primário.|
|-UG &lt;group @ no__t-1|Especifica o grupo UNIX do mapeamento primário.|

**excluir** – remove o mapeamento de um usuário ou grupo. As seguintes opções estão disponíveis para este argumento de comando:

|Opção|Definição|
|-----|-------|
|-Wu &lt;user @ no__t-1|O usuário do Windows para o qual o mapeamento será excluído, especificado como &lt;*WindowsDomain @ no__t-2 @ no__t-3 @ no__t-4User Name @ no__t-5*. Você deve especificar o **-Wu** ou a opção **-uu** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar apenas a opção **-Wu** , todos os mapeamentos para o usuário especificado serão excluídos.|
|-WG &lt;group @ no__t-1|O grupo do Windows para o qual o mapeamento será excluído, especificado como &lt;WindowsDomain @ no__t-1 @ no__t-2 @ no__t-3groupname @ no__t-4. Você deve especificar a opção **-WG** ou **-UG** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar somente a opção **-WG** , todos os mapeamentos do grupo especificado serão excluídos.|
|-UU &lt;user @ no__t-1|O usuário do UNIX para o qual o mapeamento será excluído, especificado como &lt;User nome @ no__t-1. Você deve especificar o **-Wu** ou a opção **-uu** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar apenas a opção **-uu** , todos os mapeamentos para o usuário especificado serão excluídos.|
|-UG &lt;group @ no__t-1|O grupo do UNIX para o qual o mapeamento será excluído, especificado como &lt;groupname @ no__t-1. Você deve especificar a opção **-WG** ou **-UG** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar apenas a opção **-UG** , todos os mapeamentos do grupo especificado serão excluídos.|

**lista** -exibe informações sobre mapeamentos de usuário e grupo. As seguintes opções estão disponíveis com este argumento de comando:

|Opção|Definição|
|-----|-------|
|-todos|lista os mapeamentos simples e avançados para usuários e grupos.|
|– simples|lista todos os usuários e grupos mapeados simples.|
|-avançado|lista todos os usuários e grupos mapeados avançados. Os mapas são listados na ordem em que são avaliados. Mapas primários, marcados com um asterisco (*), são listados primeiro, seguidos por mapas secundários, que são marcados com um quilate (^).|
|-Wu &lt;name @ no__t-1|lista o mapeamento para um usuário do Windows especificado.|
|-WG &lt;group @ no__t-1|lista o mapeamento de um grupo do Windows.|
|-UU &lt;name @ no__t-1|lista o mapeamento para um usuário UNIX.|
|-UG &lt;group @ no__t-1|lista o mapeamento de um grupo do UNIX.|

**backup** -salva mapeamento de nomes de usuário configuração e mapeamento de dados para o arquivo especificado por &lt;filename @ no__t-2.
**restaurar** – substitui a configuração e o mapeamento de dados com dados do arquivo (especificado por &lt;filename @ no__t-2) que foi criado usando o argumento de comando **backup** .
**adddomainmap** -adiciona um mapa simples entre um domínio do Windows e um domínio ou senha do NIS e arquivos de grupo. As seguintes opções estão disponíveis para este argumento de comando:

|Opção|Definição|
|-----|-------|
|-d &lt;WindowsDomain @ no__t-1|Especifica o domínio do Windows a ser mapeado.|
|-y &lt;NISdomain @ no__t-1|Especifica o domínio NIS a ser mapeado. &lt;br/&gt; @ no__t-2BR/&gt; **-n** &lt;nisServer @ no__t-6 especifica o servidor NIS para o domínio NIS especificado com a opção **-y** .|
|-f &lt;path @ no__t-1|Especifica o caminho totalmente qualificado do diretório que contém a senha e os arquivos de grupo a serem mapeados. Os arquivos devem estar localizados no computador que está sendo gerenciado e você não pode usar o **mapadmin** para gerenciar um computador remoto para configurar mapas com base em arquivos de senha e de grupo.|

**removedomainmap** -remove um mapa simples entre um domínio do Windows e um domínio NIS. As opções e o argumento a seguir estão disponíveis para este argumento de comando:

|Opção|Definição|
|-----|-------|
|-d &lt;WindowsDomain @ no__t-1|Especifica o domínio do Windows do mapa a ser removido.|
|-y &lt;NISdomain @ no__t-1|Especifica o domínio NIS do mapa a ser removido.|
|-todos|Especifica que todos os mapas simples entre os domínios do Windows e NIS devem ser removidos. Isso também removerá qualquer mapa simples entre um domínio do Windows e a senha e os arquivos de grupo.|

**listdomainmaps** -lista os domínios do Windows que são mapeados para domínios NIS ou senhas e arquivos de grupo.

## <a name="notes"></a>Observações
-   Se você não especificar um argumento de comando, **mapadmin** exibirá as configurações atuais para mapeamento de nomes de usuário.
-   para todas as opções que especificam um nome de usuário ou grupo, os seguintes formatos podem ser usados:
-   para usuários do Windows, use o formulário &lt;domain @ no__t-1 @ no__t-2 @ no__t-3user Name @ no__t-4, \\ @ no__t-6 @ no__t-7computer @ no__t-8 @ no__t-9 @ no__t-10user Name @ no__t-11, &gt;2 @ no__t-13computer @ no__t-14 @ no__t-15 @ no__t-16user Name @ no__ t-17 ou 8computer @ no__t-19 @ no__t-20 @ no__t-21user Name @ no__t-22
-   para grupos do Windows, use o formulário &lt;domain @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4groupname @ no__t-5 @ no__t-6, \\ @ no__t-8 @ no__t-9computer @ no__t-10 @ no__t-11 @ no__t-12 @ no__t-13groupname @ no__t-14 @ no__t-15, &gt;6 @ no__t-17computer @ No_ _T-18 @ no__t-19 @ no__t-20 @ no__t-21groupname @ no__t-22 @ no__t-23 ou \\4computer @ no__t-25 @ no__t-26 @ no__t-27 @ no__t-28groupname @ no__t-29 @ no__t-30
-   para usuários do UNIX, use o formulário &lt;NISdomain @ no__t-1 @ no__t-2 @ no__t-3user Name @ no__t-4, &lt;user Name @ no__t-6 @ no__t-7 @ no__t-8NISdomain @ no__t-9, User &gt;0name @ no__t-11 @ no__t-12 ou PCNFS @ no__t-13 @ no__t-14user Name @ no__t-15
-   para grupos do UNIX, use o formulário &lt;NISdomain @ no__t-1 @ no__t-2 @ no__t-3groupname @ no__t-4, &lt;groupname @ no__t-6 @ no__t-7 @ no__t-8NISdomain @ no__t-9, &gt;0groupname @ no__t-11 @ no__t-12 ou PCNFS @ no__t-13 @ no__t-14groupname @ no__t-15

## <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
