---
title: Visão geral da implantação de acesso sem fio
description: Este tópico faz parte do guia de rede do Windows Server 2016 "Implantar baseado em senha 802.1 X acesso autenticado sem fio"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6658c4750ba2f71b24acd4f7da02029da63179bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818917"
---
# <a name="wireless-access-deployment-overview"></a>Visão geral da implantação de acesso sem fio

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

A ilustração a seguir mostra os componentes que são necessárias para implantar o 802.1 X autenticado acesso sem fio com o PEAP\-MS\-CHAP v2.  

![Visão geral da infra-estrutura de implantação de 802.1X](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Componentes de implantação do acesso sem fio
A seguinte infraestrutura é necessária para essa implantação de acesso sem fio:

### <a name="8021x-capable-wireless-access-points"></a>802.1 X\-pontos de acesso sem fio compatíveis com
Depois que os serviços de infraestrutura de rede necessária que dão suporte a sua rede local sem fio estão em vigor, você pode começar o processo de design para o local dos pontos de acesso sem fio. O processo de design de implantação de AP sem fio envolve estas etapas:

- Identificar as áreas de cobertura para usuários sem fio. Enquanto a identificar as áreas de cobertura, certifique-se identificar se você deseja fornecer serviços sem fio fora a construção e em caso afirmativo, determine especificamente onde estão essas áreas externas.

- Determine quantos APs sem fio para implantar para garantir a cobertura adequada.

- Determine onde colocar APs sem fio.

- Selecione as frequências de canal para APs sem fio.

### <a name="active-directory-domain-services"></a>Active Directory Domain Services
Os seguintes elementos do AD DS são necessários para a implantação de acesso sem fio.

#### <a name="users-and-computers"></a>Usuários e computadores

Use os usuários do Active Directory e computadores encaixar\-em para criar e gerenciar contas de usuário e para criar um grupo de segurança sem fio que inclui a cada membro do domínio ao qual você deseja conceder acesso sem fio.

#### <a name="wireless-network-ieee-80211-policies"></a>Rede sem fio \(IEEE 802.11\) políticas

Você pode usar a rede sem fio \(IEEE 802.11\) extensão de políticas de gerenciamento de diretiva de grupo para configurar diretivas que são aplicadas a computadores sem fio quando eles tentam acessar a rede.

No grupo de política de Editor de gerenciamento, quando você com o botão direito\-clique em **rede sem fio \(IEEE 802.11\) políticas**, você tem duas opções a seguir para o tipo de política sem fio que você criar.

- **Criar uma nova política de rede sem fio para Windows Vista e versões posteriores**

- **Criar uma nova diretiva do Windows XP**

>[!TIP]
>Ao configurar uma nova política de rede sem fio, você tem a opção de alterar o nome e descrição da política. Se você alterar o nome da política, a alteração será refletida na **detalhes** painel do Editor de gerenciamento de diretiva de grupo e na barra de título da caixa de diálogo da diretiva de rede sem fio. Independentemente de como você pode renomear suas políticas, a nova política sem fio XP sempre será listada no Editor de gerenciamento do grupo de política com o **tipo** exibindo **XP**. Outras políticas são listadas com o **tipo** mostrando **Vista e versões posteriores**.  

A diretiva de rede sem fio para o Vista de Windows e versões posteriores permitem configurar, priorizar e gerenciar vários perfis sem fio. Um perfil sem fio é uma coleção de conectividade e as configurações de segurança que são usadas para se conectar a uma rede sem fio específica. Quando a diretiva de grupo é atualizada em seus computadores cliente sem fio, os perfis criados na diretiva de rede sem fio são adicionados automaticamente à configuração nos computadores cliente sem fio à qual se aplica a política de rede sem fio.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Permite conexões com várias redes sem fio

Se você tiver clientes sem fio que são movidos entre as localizações físicas na sua organização, como entre um escritório principal e uma filial, convém computadores se conectam a mais de uma rede sem fio. Nessa situação, você pode configurar um perfil sem fio que contém as configurações de conectividade e segurança específicas para cada rede.

Por exemplo, suponha que sua empresa tiver uma rede sem fio para o escritório corporativo principal, com um identificador do conjunto \(SSID\) WlanCorp.

O escritório da filial também tem uma rede sem fio à qual você também deseja se conectar. O escritório da filial tem o SSID configurado como WlanBranch.

Nesse cenário, você pode configurar um perfil para cada rede e computadores ou outros dispositivos que são usados no escritório corporativo e filial pode se conectar a qualquer uma das redes sem fio quando estiverem fisicamente no intervalo da área de cobertura da rede.

##### <a name="mixed-mode-wireless-networks"></a>Misto\-redes sem fio do modo

Como alternativa, suponha que sua rede tem uma mistura de computadores sem fio e dispositivos que dão suporte a padrões de segurança diferentes. Talvez alguns computadores mais antigos têm adaptadores sem fio que só podem usar WPA\-Enterprise, embora dispositivos mais recentes podem usar o WPA2 mais forte\-padrão corporativo.

Você pode criar dois perfis diferentes que usam o mesmo SSID e configurações de conectividade e segurança quase idênticas.

Em um perfil, você pode definir a autenticação sem fio para WPA2\-Enterprise com o AES e no outro perfil, você pode especificar WPA\-Enterprise com TKIP.

Isso é conhecido como um misto\-modo de implantação e ele permite que os computadores de diferentes tipos e recursos sem fio para compartilhar a mesma rede sem fio.

### <a name="network-policy-server-nps"></a>Servidor de diretivas de rede \(NPS\)
O NPS permite que você crie e imponha políticas de acesso à rede para autenticação de solicitação de conexão e autorização.

Quando você usa o NPS como servidor RADIUS, você configura servidores de acesso de rede, como pontos de acesso sem fio como clientes RADIUS no NPS. Você também pode configurar as políticas de rede que o NPS usa para autenticar clientes de acesso e autorizar suas solicitações de conexão.  

### <a name="wireless-client-computers"></a>Computadores cliente sem fio
Com a finalidade deste guia, os computadores cliente sem fio são computadores e outros dispositivos que são equipados com adaptadores de rede sem fio IEEE 802.11 e que estão executando o cliente do Windows ou sistemas operacionais Windows Server.

#### <a name="server-computers-as-wireless-clients"></a>Computadores de servidor como clientes sem fio

Por padrão, a funcionalidade para 802.11 sem fio está desabilitada em computadores que executam o Windows Server.

Para habilitar a conectividade sem fio em computadores que executam sistemas operacionais de servidor, você deve instalar e habilitar a LAN sem fio \(WLAN\) recurso de serviço usando o Windows PowerShell ou o assistente Adicionar funções e recursos no servidor Gerenciador.

Quando você instala o **serviço de LAN sem fio** de recursos, o novo serviço **automática de WLAN** está instalado na **serviços**. Quando a instalação for concluída, você deve reiniciar o servidor.

Depois que o servidor for reiniciado, você pode acessar a configuração automática de WLAN quando você clica **inicie**, **ferramentas administrativas do Windows**, e **serviços**.

Após a instalação e o servidor reiniciar, o serviço está em um estado parado com um tipo de inicialização de configuração automática de WLAN **automática**. Para iniciar o serviço, clique duas vezes em **automática de WLAN**. Sobre o **gerais** , clique em **iniciar**e, em seguida, clique em **Okey**.

O serviço AutoConfig de WLAN enumera adaptadores sem fio e gerencia conexões sem fio e perfis sem fio que contêm configurações que são necessárias para configurar o servidor para se conectar a uma rede sem fio.

Para obter uma visão geral da implantação de acesso sem fio, consulte [processo de implantação de acesso sem fio](c-wireless-access-deploy-process.md).
