---
title: Visão geral da implantação acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "Implantar baseada em senha 802.1 X autenticado sem fio acesso"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 11d87254d8819606a06acedd407bb2e09a45cb60
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-overview"></a>Visão geral da implantação acesso sem fio

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

A ilustração a seguir mostra os componentes que são necessárias para implantar o 802.1 X autenticado acesso sem fio com PEAP\-MS\-CHAP v2.  

![802.1 x visão geral de infraestrutura de implantação](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Componentes de implantação de acesso sem fio
A infraestrutura a seguir é necessária para essa implantação de acesso sem fio:

### <a name="8021x-capable-wireless-access-points"></a>802.1X\-compatível com pontos de acesso sem fio
Depois que os serviços de infraestrutura de rede necessários dando suporte a sua rede local sem fio estão no lugar, você pode começar o processo de design para o local dos pontos de acesso sem fio. O processo de design de implantação de ponto de acesso sem fio envolve estas etapas:

- Identifique as áreas de cobertura para usuários sem fio. Enquanto identificando as áreas de cobertura, não se esqueça de identificar se você deseja fornecer serviços sem fio fora a criação e em caso afirmativo, determine especificamente onde estão as áreas externas.

- Determine quantos pontos de acesso sem fio para implantar para garantir uma cobertura adequada.

- Determine onde colocar os pontos de acesso sem fio.

- Selecione as frequências de canal para pontos de acesso sem fio.

### <a name="active-directory-domain-services"></a>Serviços de domínio do Active Directory
Os seguintes elementos do AD DS são necessários para a implantação de acesso sem fio.

#### <a name="users-and-computers"></a>Usuários e computadores

Use o Active Directory Users e computadores snap\-in para criar e gerenciar contas de usuário e para criar um grupo de segurança sem fio que inclui cada membro do domínio ao qual você deseja conceder acesso sem fio.

#### <a name="wireless-network-ieee-80211-policies"></a>A rede sem fio \ (IEEE 802.11\) políticas

Você pode usar a rede sem fio \ (IEEE 802.11\) extensão de políticas de gerenciamento de política de grupo para configurar políticas são aplicadas a computadores sem fio quando eles tentam acessar a rede.

No Editor de gerenciamento de política de grupo, quando você right\-click **rede sem fio \ (IEEE 802.11\) políticas**, você tem duas opções para o tipo de política sem fio que você criar.

- **Crie uma nova política de rede sem fio para Windows Vista e versões posteriores**

- **Crie uma nova política XP Windows**

>[!TIP]
>Ao configurar uma nova política de rede sem fio, você tem a opção para alterar o nome e a descrição da política. Se você alterar o nome da política, a alteração será refletida no **detalhes** painel do Editor de gerenciamento de política de grupo e na barra de título da caixa de diálogo de política de rede sem fio. Independentemente de como você renomeia as políticas, a nova política de acesso sem fio XP serão sempre listada no Editor de gerenciamento de grupo política com a **tipo** exibindo **XP**. Outras políticas estão listadas com a **tipo** mostrando **Vista e versões posteriores**.  

Sem fio rede política para o Windows Vista e versões posteriores permite configurar, priorizar e gerenciar vários perfis sem fio. Um perfil sem fio é uma coleção de conectividade e configurações de segurança que são usadas para se conectar a uma rede sem fio específica. Quando a política de grupo é atualizada em computadores cliente sem fio, os perfis que você criar na política de rede sem fio são adicionados automaticamente à configuração nos computadores cliente sem fio ao qual se aplica a política de rede sem fio.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Permitir conexões em várias redes sem fio

Se você tiver clientes sem fio são movidos em todos os locais físicos em sua organização, como entre uma matriz e uma filial, convém computadores para se conectar a mais de uma rede sem fio. Nessa situação, você pode configurar um perfil sem fio que contém as configurações de conectividade e segurança específicas para cada rede.

Por exemplo, suponha que sua empresa tem uma rede sem fio para a sede principal, com um identificador do conjunto \(SSID\) WlanCorp.

Sua filial também tem uma rede sem fio ao qual você também deseja se conectar. Filial tem o SSID configurado como WlanBranch.

Nesse cenário, você pode configurar um perfil para cada rede e computadores ou outros dispositivos que são usados no escritório corporativo e filial pode se conectar a uma das redes sem fio quando estiverem fisicamente no intervalo da área de cobertura da rede.

##### <a name="mixed-mode-wireless-networks"></a>Redes sem fio Mixed\-mode

Como alternativa, suponha que sua rede tem uma mistura de computadores sem fio e dispositivos que dão suporte a padrões de segurança diferentes. Talvez alguns computadores mais antigos tenham adaptadores sem fio que só podem usar WPA\-Enterprise, enquanto dispositivos mais novos podem usar o padrão de WPA2\ Enterprise mais forte.

Você pode criar dois perfis diferentes que usam o mesmo SSID e configurações de conectividade e segurança praticamente idênticas.

Em um perfil, você pode definir a autenticação sem fio para WPA2\-Enterprise com AES e no perfil do você pode especificar WPA\ Enterprise com TKIP.

Isso é conhecido como uma implantação de modo mixed\ e permite que computadores de diferentes tipos e recursos sem fio para compartilhar a mesma rede sem fio.

### <a name="network-policy-server-nps"></a>O servidor de política de rede \(NPS\)
NPS permite que você criar e aplicar políticas de acesso de rede para autenticação de solicitação de conexão e autorização.

Quando você usa o NPS como um servidor RADIUS, você configura os servidores de acesso à rede, como pontos de acesso sem fio, como clientes RADIUS no NPS. Você também pode configurar as políticas de rede que usa o NPS para clientes de acesso de autenticar e autorizar suas solicitações de conexão.  

### <a name="wireless-client-computers"></a>Computadores cliente sem fio
Para fins deste guia, os computadores cliente sem fio são computadores e outros dispositivos que são equipados com adaptadores de rede sem fio IEEE 802.11 e que estejam executando o Windows client ou sistemas operacionais Windows Server.

#### <a name="server-computers-as-wireless-clients"></a>Computadores de servidor como clientes sem fio

Por padrão, a funcionalidade sem fio 802.11 está desabilitada em computadores que executam o Windows Server.

Para habilitar a conectividade sem fio em computadores que executam sistemas operacionais de servidor, você deve instalar e habilitar o \(WLAN\) LAN sem fio recurso de serviço por meio do Windows PowerShell ou a adição de funções e recursos de assistente no Gerenciador do servidor.

Quando você instala o **serviço de LAN sem fio** de recursos, o novo serviço **configuração automática de WLAN** está instalado no **serviços**. Quando a instalação for concluída, você deve reiniciar o servidor.

Depois que o servidor for reiniciado, você pode acessar a configuração automática de WLAN quando você clica em **iniciar**, **ferramentas administrativas do Windows**, e **serviços**.

Depois de instalar e server reiniciar, a configuração automática de WLAN serviço está em um estado parado com um tipo de inicialização **automática**. Para iniciar o serviço, clique duas vezes em **configuração automática de WLAN**. Sobre o **geral** , clique em **iniciar**e, em seguida, clique em **Okey**.

O serviço de configuração automática de WLAN enumera adaptadores sem fio e gerencia conexões sem fio e os perfis sem fio que contêm configurações que são necessárias para configurar o servidor para se conectar a uma rede sem fio.

Para obter uma visão geral da implantação de acesso sem fio, consulte [processo de implantação de acesso sem fio](c-wireless-access-deploy-process.md).
