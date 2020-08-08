---
title: Visão geral da implantação de acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "implantar o acesso sem fio autenticado 802.1 X com base em senha"
manager: brianlic
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: eross-msft
ms.author: lizross
ms.openlocfilehash: ab135c7a30a1930fef58fae357a38510eab711eb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969613"
---
# <a name="wireless-access-deployment-overview"></a>Visão geral da implantação de acesso sem fio

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

A ilustração a seguir mostra os componentes necessários para implantar o acesso sem fio autenticado 802.1 X com o PEAP \- MS \- CHAP v2.

![Visão geral da infraestrutura de implantação 802.1 x](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Componentes de implantação de acesso sem fio
A infraestrutura a seguir é necessária para esta implantação de acesso sem fio:

### <a name="8021x-capable-wireless-access-points"></a>pontos de \- acesso sem fio compatíveis com 802.1 x
Depois que os serviços de infraestrutura de rede necessários para dar suporte à sua rede local sem fio estiverem em vigor, você poderá iniciar o processo de design para o local dos APs sem fio. O processo de design de implantação de AP sem fio envolve estas etapas:

- Identifique as áreas de cobertura para usuários sem fio. Ao identificar as áreas de cobertura, não se esqueça de identificar se deseja fornecer o serviço sem fio fora do prédio e, nesse caso, determinar especificamente onde estão essas áreas externas.

- Determine quantos APs sem fio implantar para garantir a cobertura adequada.

- Determine onde posicionar APs sem fio.

- Selecione as frequências de canal para APs sem fio.

### <a name="active-directory-domain-services"></a>Active Directory Domain Services
Os seguintes elementos de AD DS são necessários para a implantação de acesso sem fio.

#### <a name="users-and-computers"></a>Usuários e computadores

Use o snap Active Directory usuários e computadores \- para criar e gerenciar contas de usuário e para criar um grupo de segurança sem fio que inclua cada membro do domínio ao qual você deseja conceder acesso sem fio.

#### <a name="wireless-network-ieee-80211-policies"></a>\(Políticas IEEE 802,11 de rede sem fio \)

Você pode usar a \( extensão de políticas de rede sem fio IEEE 802,11 \) do gerenciamento de política de grupo para configurar políticas que são aplicadas a computadores sem fio quando tentam acessar a rede.

No Editor de Gerenciamento de Política de Grupo, ao clicar com o botão direito do mouse \- em ** \( \) políticas IEEE 802,11 de rede sem fio**, você terá as duas opções a seguir para o tipo de política sem fio que você criar.

- **Criar uma nova política de rede sem fio para o Windows Vista e versões posteriores**

- **Criar uma nova política do Windows XP**

>[!TIP]
>Ao configurar uma nova política de rede sem fio, você tem a opção de alterar o nome e a descrição da política. Se você alterar o nome da política, a alteração será refletida no painel de **detalhes** de editor de gerenciamento de política de grupo e na barra de título da caixa de diálogo política de rede sem fio. Independentemente de como você renomear suas políticas, a nova política sem fio do XP será sempre listada em Editor de Gerenciamento de Política de Grupo com o **tipo** exibindo o **XP**. Outras políticas são listadas com o **tipo** mostrando o **Vista e versões posteriores**.

A diretiva de rede sem fio para Windows Vista e versões posteriores permite que você configure, priorize e gerencie vários perfis sem fio. Um perfil sem fio é uma coleção de configurações de conectividade e segurança que são usadas para se conectar a uma rede sem fio específica. Quando Política de Grupo é atualizado em seus computadores cliente sem fio, os perfis que você cria na diretiva de rede sem fio são adicionados automaticamente à configuração em seus computadores cliente sem fio aos quais a diretiva de rede sem fio se aplica.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Permitindo conexões com várias redes sem fio

Se você tiver clientes sem fio que são movidos entre locais físicos em sua organização, como entre um escritório principal e uma filial, talvez você queira que os computadores se conectem a mais de uma rede sem fio. Nessa situação, você pode configurar um perfil sem fio que contenha as configurações específicas de conectividade e segurança para cada rede.

Por exemplo, suponha que sua empresa tenha uma rede sem fio para o escritório corporativo principal, com um identificador de conjunto de serviços \( SSID \) WlanCorp.

Sua filial também tem uma rede sem fio à qual você também deseja se conectar. A filial tem o SSID configurado como WlanBranch.

Nesse cenário, você pode configurar um perfil para cada rede, e computadores ou outros dispositivos que são usados no escritório corporativo e na filial podem se conectar a qualquer uma das redes sem fio quando estiverem fisicamente no alcance da área de cobertura de uma rede.

##### <a name="mixed-mode-wireless-networks"></a>\-Redes sem fio de modo misto

Como alternativa, suponha que sua rede tenha uma mistura de computadores sem fio e dispositivos que dão suporte a diferentes padrões de segurança. Talvez alguns computadores mais antigos tenham adaptadores sem fio que só possam usar \- o WPA Enterprise, enquanto dispositivos mais novos podem usar o padrão corporativo WPA2 mais forte \- .

Você pode criar dois perfis diferentes que usam o mesmo SSID e configurações de segurança e conectividade quase idênticas.

Em um perfil, você pode definir a autenticação sem fio para WPA2 \- Enterprise com AES e, no outro perfil, você pode especificar o WPA \- Enterprise com TKIP.

Isso é comumente conhecido como uma \- implantação de modo misto e permite que computadores de diferentes tipos e recursos sem fio compartilhem a mesma rede sem fio.

### <a name="network-policy-server-nps"></a>NPS do servidor de políticas de rede \(\)
O NPS permite que você crie e aplique políticas de acesso à rede para autenticação e autorização de solicitação de conexão.

Ao usar o NPS como um servidor RADIUS, você configura os servidores de acesso à rede, como pontos de acesso sem fio, como clientes RADIUS no NPS. Você também configura as políticas de rede que o NPS usa para autenticar clientes de acesso e autorizar suas solicitações de conexão.

### <a name="wireless-client-computers"></a>Computadores cliente sem fio
Para fins deste guia, os computadores cliente sem fio são computadores e outros dispositivos equipados com adaptadores de rede sem fio IEEE 802,11 e que estão executando sistemas operacionais Windows Client ou Windows Server.

#### <a name="server-computers-as-wireless-clients"></a>Computadores de servidor como clientes sem fio

Por padrão, a funcionalidade de 802,11 sem fio é desabilitada em computadores que executam o Windows Server.

Para habilitar a conectividade sem fio em computadores que executam sistemas operacionais de servidor, você deve instalar e habilitar o recurso de serviço WLAN de LAN sem fio \( \) usando o Windows PowerShell ou o assistente para adicionar funções e recursos no Gerenciador do servidor.

Quando você instala o recurso **serviço de LAN sem fio** , o novo serviço **configuração automática de WLAN** é instalado nos **Serviços**. Quando a instalação for concluída, você deverá reiniciar o servidor.

Depois que o servidor for reiniciado, você poderá acessar a configuração automática de WLAN ao clicar em **Iniciar**, **Ferramentas administrativas do Windows**e **Serviços**.

Após a instalação e a reinicialização do servidor, o serviço de configuração automática de WLAN está em um estado parado com um tipo de inicialização **automático**. Para iniciar o serviço, clique duas vezes em **configuração automática de WLAN**. Na guia **geral** , clique em **Iniciar**e em **OK**.

O serviço de configuração automática de WLAN enumera os adaptadores sem fio e gerencia as conexões sem fio e os perfis sem fio que contêm configurações que são necessárias para configurar o servidor para se conectar a uma rede sem fio.

Para obter uma visão geral da implantação de acesso sem fio, consulte [processo de implantação de acesso sem fio](c-wireless-access-deploy-process.md).
