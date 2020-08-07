---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: Selecionando o domínio raiz da floresta
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: c0a814932bfb5a232e55857e3cc10af4068d14c9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972233"
---
# <a name="selecting-the-forest-root-domain"></a>Selecionando o domínio raiz da floresta

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O primeiro domínio que você implanta em uma floresta Active Directory é chamado de domínio raiz da floresta. Esse domínio permanece o domínio raiz da floresta para o ciclo de vida da implantação do AD DS.

O domínio raiz da floresta contém os grupos Administradores de empresa e administradores de esquema. Esses grupos de administradores de serviço são usados para gerenciar operações em nível de floresta, como a adição e a remoção de domínios e a implementação de alterações no esquema.

Selecionar o domínio raiz da floresta envolve determinar se um dos domínios Active Directory em seu design de domínio pode funcionar como o domínio raiz da floresta ou se você precisa implantar um domínio raiz de floresta dedicada.

Para obter informações sobre como implantar um domínio raiz de floresta, consulte [implantando um domínio raiz de floresta do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Escolhendo um domínio raiz de floresta ou de região dedicada

Se você estiver aplicando um único modelo de domínio, o domínio único funcionará como o domínio raiz da floresta. Se você estiver aplicando um modelo de vários domínios, poderá optar por implantar um domínio raiz de floresta dedicada ou selecionar um domínio regional para funcionar como o domínio raiz da floresta.

### <a name="dedicated-forest-root-domain"></a>Domínio raiz de floresta dedicada

Um domínio raiz de floresta dedicada é um domínio criado especificamente para funcionar como a raiz da floresta. Ele não contém nenhuma conta de usuário além das contas de administrador de serviços para o domínio raiz da floresta. Além disso, ele não representa nenhuma região geográfica em sua estrutura de domínio. Todos os outros domínios na floresta são filhos do domínio raiz da floresta dedicada.

O uso de uma raiz de floresta dedicada oferece as seguintes vantagens:

- Separação operacional de administradores de serviço de floresta dos administradores de serviço de domínio. Em um único ambiente de domínio, os membros dos grupos admins. do domínio e administradores internos podem usar ferramentas e procedimentos padrão para se tornar membros dos grupos Administradores de empresa e administradores de esquema. Em uma floresta que usa um domínio raiz de floresta dedicada, os membros dos grupos admins. do domínio e administradores internos nos domínios regionais não podem se tornar membros dos grupos de administrador de serviço no nível de floresta usando ferramentas e procedimentos padrão.
- Proteção contra alterações operacionais em outros domínios. Um domínio raiz de floresta dedicada não representa uma região geográfica específica em sua estrutura de domínio. Por esse motivo, ele não é afetado por reorganizações ou outras alterações que resultam na renomeação ou reestruturação de domínios.
- Serve como uma raiz neutra para que nenhum país ou região pareça subordinado a outra região. Algumas organizações podem preferir evitar a aparência de um país ou região subordinada a outro país ou região no namespace. Quando você usa um domínio raiz de floresta dedicada, todos os domínios regionais podem ser pares na hierarquia de domínio.

Em um ambiente de vários domínios regionais no qual uma raiz de floresta dedicada é usada, a replicação do domínio raiz da floresta tem impacto mínimo sobre a infraestrutura de rede. Isso ocorre porque a raiz da floresta hospeda apenas as contas de administrador do serviço. A maioria das contas de usuário na floresta e outros dados específicos de domínio são armazenados nos domínios regionais.

Uma desvantagem de usar um domínio raiz de floresta dedicado é que ele cria sobrecarga de gerenciamento adicional para dar suporte ao domínio adicional.

### <a name="regional-domain-as-a-forest-root-domain"></a>Domínio regional como um domínio raiz de floresta

Se você optar por não implantar um domínio raiz de floresta dedicado, deverá selecionar um domínio regional para funcionar como o domínio raiz da floresta. Esse domínio é o domínio pai de todos os outros domínios regionais e será o primeiro domínio que você implantar. O domínio raiz da floresta contém contas de usuário e é gerenciado da mesma forma que os outros domínios regionais são gerenciados. A principal diferença é que ela também inclui os grupos Administradores de empresa e administradores de esquema.

A vantagem de selecionar um domínio regional para funcionar como o domínio raiz da floresta é que ele não cria a sobrecarga de gerenciamento adicional que a manutenção de um domínio adicional cria. Selecione um domínio regional apropriado para ser a raiz da floresta, como o domínio que representa a matriz ou a região que tem as conexões de rede mais rápidas. Se for difícil para sua organização selecionar um domínio regional para ser o domínio raiz da floresta, você poderá optar por usar um modelo de raiz de floresta dedicada.

## <a name="assigning-the-forest-root-domain-name"></a>Atribuindo o nome de domínio raiz da floresta

O nome de domínio raiz da floresta também é o nome da floresta. O nome raiz da floresta é um nome DNS (sistema de nomes de domínio) que consiste em um prefixo e um sufixo na forma de Prefix. sufixo. Por exemplo, uma organização pode ter o nome de raiz da floresta corp.contoso.com. Neste exemplo, Corp é o prefixo e contoso.com é o sufixo.

Selecione o sufixo em uma lista de nomes existentes em sua rede. Para o prefixo, selecione um novo nome que não tenha sido usado anteriormente na sua rede. Ao anexar um novo prefixo a um sufixo existente, você cria um namespace exclusivo. A criação de um novo namespace para Active Directory Domain Services (AD DS) garante que qualquer infraestrutura de DNS existente não precise ser modificada para acomodar AD DS.

### <a name="selecting-a-suffix"></a>Selecionando um sufixo

Para selecionar um sufixo para o domínio raiz da floresta:

1. Contate o proprietário DNS da organização para obter uma lista de sufixos DNS registrados que estão em uso na rede que hospedará AD DS. Observe que os sufixos usados na rede interna podem ser diferentes dos sufixos usados externamente. Por exemplo, uma organização pode usar contosopharma.com na Internet e contoso.com na rede corporativa interna.

2. Consulte o proprietário do DNS para selecionar um sufixo para uso com AD DS. Se não existir nenhum sufixo adequado, registre um novo nome com uma autoridade de nomenclatura da Internet.

Recomendamos que você use nomes DNS que são registrados com uma autoridade de Internet no namespace Active Directory. Somente nomes registrados têm a garantia de serem globalmente exclusivos. Se outra organização posteriormente registrar o mesmo nome de domínio DNS (ou se a sua organização mescla, adquire ou é adquirida por outra empresa que usa o mesmo nome DNS), as duas infraestruturas não podem interagir umas com as outras.

> [!CAUTION]
> Não use nomes DNS de rótulo único. Para obter mais informações, consulte [implantação e operação de Active Directory domínios configurados usando nomes DNS de rótulo único](https://support.microsoft.com/help/300684/). Além disso, não recomendamos o uso de sufixos não registrados, como. local.

### <a name="selecting-a-prefix"></a>Selecionando um prefixo

Se você escolher um sufixo registrado que já esteja em uso na rede, selecione um prefixo para o nome de domínio raiz da floresta usando as regras de prefixo na tabela a seguir. Adicione um prefixo que não esteja em uso no momento para criar um novo nome subordinado. Por exemplo, se o nome raiz do DNS for contoso.com, você poderá criar o nome de domínio raiz da floresta Active Directory concorp.contoso.com se o namespace concorp.contoso.com ainda não estiver em uso na rede. Essa nova ramificação do namespace será dedicada a AD DS e poderá ser integrada facilmente com a implementação de DNS existente.

Se você selecionou um domínio regional para funcionar como um domínio raiz de floresta, talvez seja necessário selecionar um novo prefixo para o domínio. Como o nome de domínio raiz da floresta afeta todos os outros nomes de domínio na floresta, um nome com base em região pode não ser apropriado. Se você estiver usando um novo sufixo que não está em uso no momento na rede, você poderá usá-lo como o nome de domínio raiz da floresta sem escolher um prefixo adicional.

A tabela a seguir lista as regras para selecionar um prefixo para um nome DNS registrado.

| Regra     | Explicação |
| -------- | --------------- |
| Selecione um prefixo que provavelmente não se tornará desatualizado. | Evite nomes como uma linha de produto ou sistema operacional que possam ser alterados no futuro. É recomendável usar nomes genéricos, como Corp ou DS.|
| Selecione um prefixo que inclua somente caracteres padrão da Internet. | A-Z, a-z, 0-9 e (-), mas não totalmente numérica. |
| Inclua 15 caracteres ou menos no prefixo. | Se você escolher um comprimento de prefixo de 15 caracteres ou menos, o nome NetBIOS será o mesmo que o prefixo. |

É importante que o proprietário do DNS Active Directory funcione com o proprietário do DNS da organização para obter a propriedade do nome que será usado para o namespace de Active Directory. Para obter mais informações sobre como criar uma infraestrutura de DNS para dar suporte a AD DS, consulte [criando um design de infraestrutura de DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

## <a name="documenting-the-forest-root-domain-name"></a>Documentando o nome de domínio raiz da floresta

Documente o prefixo e o sufixo DNS que você selecionar para o domínio raiz da floresta. Neste ponto, identifique qual domínio será a raiz da floresta. Você pode adicionar as informações de nome de domínio raiz da floresta à planilha "planejamento de domínio" que você criou para documentar seu plano para domínios novos e atualizados e seus nomes de domínio. Para abri-lo, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de [auxílios de trabalho para o kit de implantação do Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608) e abra o "planejamento de domínio" (DSSLOGI_5.doc).
