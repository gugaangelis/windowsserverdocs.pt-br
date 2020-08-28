---
title: mapadmin
description: Artigo de referência para o comando mapadmin, que gerencia Mapeamento de Nomes de Usuário para serviços da Microsoft para o sistema de arquivos de rede.
ms.topic: reference
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b62e31dbe53c5e2b16093bb222b8129d3cca087
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033944"
---
# <a name="mapadmin"></a>mapadmin

O utilitário de linha de comando **mapadmin** administra mapeamento de nomes de usuário no computador local ou remoto que executa os serviços da Microsoft para o sistema de arquivos de rede. Se você estiver conectado com uma conta que não tem credenciais administrativas, poderá especificar um nome de usuário e senha de uma conta que o faça.

## <a name="syntax"></a>Sintaxe

```
mapadmin [<computer>] [-u <user> [-p <password>]]
mapadmin [<computer>] [-u <user> [-p <password>]] {start | stop}
mapadmin [<computer>] [-u <user> [-p <password>]] config <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] add -wu <windowsuser> -uu <UNIXuser> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] add -wg <windowsgroup> -ug <UNIXgroup> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wu <Windowsuser> [-uu <UNIXuser>]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wg <Windowsgroup> [-ug <UNIXgroup>]
mapadmin [<computer>] [-u <user> [-p <password>]] delete <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] list <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] backup <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] restore <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] adddomainmap -d <Windowsdomain> {-y <<NISdomain>> | -f <path>}
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -d <Windowsdomain> -y <<NISdomain>>
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -all
mapadmin [<computer>] [-u <user> [-p <password>]] listdomainmaps
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<computer>` | Especifica o computador remoto que executa o serviço de Mapeamento de Nomes de Usuário que você deseja administrar. Você pode especificar o computador usando um nome WINS (serviço de cadastramento na Internet do Windows) ou um nome DNS (sistema de nomes de domínio) ou por endereço IP (Internet Protocol). |
| -u `<user>` | Especifica o nome de usuário do usuário cujas credenciais devem ser usadas. Pode ser necessário adicionar o nome de domínio ao nome de usuário no formato *domínio \ nomedeusuário*. |
| -p `<password>` | Especifica a senha do usuário. Se você especificar a opção **-u** , mas omitir a opção **-p** , será solicitada a senha do usuário. |
| `start | stop` | Inicia ou interrompe o serviço de Mapeamento de Nomes de Usuário. |
| config | Especifica as configurações gerais para Mapeamento de Nomes de Usuário. As seguintes opções estão disponíveis com este parâmetro:<ul><li>**-r `<dddd>:<hh>:<mm>` :** especifica o intervalo de atualização para atualização dos bancos de dados do Windows e NIS em dias, horas e minutos. O intervalo mínimo é de 5 minutos.</li><li>**-i `{yes | no}` :** ativa o mapeamento simples (**Sim**) ou desativado (**não**). Por padrão, o mapeamento está ativado.</li></ul> |
| add | Cria um novo mapeamento para um usuário ou grupo. As seguintes opções estão disponíveis com este parâmetro:<ul><li>**-Wu `<name>` :** especifica o nome do usuário do Windows para o qual um novo mapeamento está sendo criado.</li><li>**-uu `<name>` :** especifica o nome do usuário UNIX para o qual um novo mapeamento está sendo criado.</li><li>**-WG `<group>` :** especifica o nome do grupo do Windows para o qual um novo mapeamento está sendo criado.</li><li>**-UG `<group>` :** especifica o nome do grupo do UNIX para o qual um novo mapeamento está sendo criado.</li><li>**-setprimary:** Especifica que o novo mapeamento é o mapeamento principal.</li></ul> |
| setprimary | Especifica qual mapeamento é o mapeamento principal para um usuário ou grupo UNIX com vários mapeamentos. As seguintes opções estão disponíveis com este parâmetro:<ul><li>**-Wu `<name>` :** especifica o usuário do Windows do mapeamento primário. Se houver mais de um mapeamento para o usuário, use a opção **-uu** para especificar o mapeamento primário.</li><li>**-uu `<name>` :** especifica o usuário do UNIX do mapeamento principal.</li><li>**-WG `<group>` :** especifica o grupo do Windows do mapeamento primário. Se houver mais de um mapeamento para o grupo, use a opção **-UG** para especificar o mapeamento primário.</li><li>**-UG `<group>` :** especifica o grupo UNIX do mapeamento primário.</li></ul> |
| excluir | Remove o mapeamento de um usuário ou grupo. As seguintes opções estão disponíveis para este parâmetro:<ul><li>**-Wu `<user>` :** especifica o usuário do Windows para o qual o mapeamento será excluído, especificado como `<windowsdomain>\<username>` .<p>Você deve especificar o **-Wu** ou a opção **-uu** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar apenas a opção **-Wu** , todos os mapeamentos para o usuário especificado serão excluídos.</li><li>**-uu `<user>` :** especifica o usuário UNIX para o qual o mapeamento será excluído, especificado como `<username>` .<p>Você deve especificar o **-Wu** ou a opção **-uu** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar apenas a opção **-uu** , todos os mapeamentos para o usuário especificado serão excluídos.</li><li>**-WG `<group>` :** especifica o grupo do Windows para o qual o mapeamento será excluído, especificado como `<windowsdomain>\<username>` .<p>Você deve especificar a opção **-WG** ou **-UG** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar somente a opção **-WG** , todos os mapeamentos do grupo especificado serão excluídos.</li><li>**-UG `<group>` :** especifica o grupo do UNIX para o qual o mapeamento será excluído, especificado como `<groupname>` .<p>Você deve especificar a opção **-WG** ou **-UG** , ou ambos. Se você especificar as duas opções, o mapeamento específico identificado pelas duas opções será excluído. Se você especificar apenas a opção **-UG** , todos os mapeamentos do grupo especificado serão excluídos.</li></ul> |
| list | Exibe informações sobre mapeamentos de usuário e grupo. As seguintes opções estão disponíveis com este parâmetro:<ul><li>**-todos:** Lista os mapeamentos simples e avançados para usuários e grupos.</li><li>**– simples:** Lista todos os usuários e grupos mapeados simples.</li><li>**-avançado:** Lista todos os usuários e grupos mapeados avançados. Os mapas são listados na ordem em que são avaliados. Mapas primários, marcados com um asterisco (*), são listados primeiro, seguidos por mapas secundários, que são marcados com um quilate `(^)` . </li> <li> * *-Wu `<name>` :** lista o mapeamento para um usuário do Windows especificado.</li><li>**-WG `<group>` :** lista o mapeamento de um grupo do Windows.</li><li>**-uu `<name>` :** lista o mapeamento para um usuário UNIX.</li><li>**-UG `<group>` :** lista o mapeamento para um grupo do UNIX.</li></ul> |
| backup | Salva Mapeamento de Nomes de Usuário configuração e mapeamento de dados para o arquivo especificado por `<filename>` . |
| restaurar | Substitui a configuração e o mapeamento de dados com dados do arquivo (especificado por `<filename>` ) que foi criado usando o parâmetro **backup** . |
| adddomainmap | Adiciona um mapa simples entre um domínio do Windows e um domínio ou senha do NIS e arquivos de grupo. As seguintes opções estão disponíveis para este parâmetro:<ul><li>**-d `<windowsdomain>` :** especifica o domínio do Windows a ser mapeado.</li><li>**-y `<NISdomain>` :** especifica o domínio NIS a ser mapeado. Você deve usar o parâmetro **- `<NISserver>` n** para especificar o servidor NIS para o domínio NIS especificado pela opção **-y** .</li><li>**-f `<path>` :** especifica o caminho totalmente qualificado do diretório que contém a senha e os arquivos de grupo a serem mapeados. Os arquivos devem estar localizados no computador que está sendo gerenciado e você não pode usar o **mapadmin** para gerenciar um computador remoto para configurar mapas com base em arquivos de senha e de grupo.</li></ul> |
| removedomainmap | Remove um mapa simples entre um domínio do Windows e um domínio NIS. As opções e o argumento a seguir estão disponíveis para este parâmetro:<ul><li>**-d `<windowsdomain>` :** especifica o domínio do Windows do mapa a ser removido.</li><li>**-y `<NISdomain>` :** especifica o domínio NIS do mapa a ser removido.</li><li>**-todos:** Especifica que todos os mapas simples entre os domínios do Windows e NIS devem ser removidos. Isso também removerá qualquer mapa simples entre um domínio do Windows e a senha e os arquivos de grupo.</li></ul> |
| listdomainmaps | Lista os domínios do Windows que são mapeados para domínios NIS ou senhas e arquivos de grupo. |

#### <a name="remarks"></a>Comentários

- Se você não especificar nenhum parâmetros, o comando **mapadmin** exibirá as configurações atuais para mapeamento de nomes de usuário.

- Para todas as opções que especificam um nome de usuário ou grupo, os seguintes formatos podem ser usados:

    - Para usuários do Windows, use os formatos: `<domain>\<username>` , `\\<computer>\<username>` , `\<computer>\<username>` ou `<computer>\<username>`

    - Para grupos do Windows, use os formatos: `<domain>\<groupname>` , `\\<computer>\<groupname>` , `\<computer>\<groupname>` ou `<computer>\<groupname>`

    - Para usuários do UNIX, use os formatos: `<NISdomain>\<username>` , `<username>@<NISdomain>` , `<username>@PCNFS` ou `PCNFS\<username>`

    - Para grupos do UNIX, use os formatos: `<NISdomain>\<groupname>` , `<groupname>@<NISdomain>` , `<groupname>@PCNFS` ou `PCNFS\<groupname>`

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
