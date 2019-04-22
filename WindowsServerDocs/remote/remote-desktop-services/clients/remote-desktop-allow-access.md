---
title: Área de trabalho remota - permitir o acesso a seu computador
description: Saiba mais sobre as opções para acessar remotamente seu PC
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f1557ed-53f7-4333-b023-c8e0f4b58bf4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: af41304e08f19ca155f6fd13c9258e9a8f20c163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817007"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>Área de trabalho remota - permitir o acesso a seu computador

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Você pode usar a área de trabalho remota para conectar e controlar seu PC de um dispositivo remoto usando um [cliente de área de trabalho remota Microsoft](remote-desktop-clients.md) (disponível para Windows, iOS, macOS e Android). Quando você permitir conexões remotas para o seu PC, você pode usar outro dispositivo para se conectar ao seu PC e ter acesso a todos os seus aplicativos, arquivos e recursos de rede como se estivesse em sua mesa.  

> [!NOTE]
> Você pode usar a área de trabalho remota para se conectar ao Windows 10 Pro e Enteprise, Windows 8.1 e 8 Enterprise e Pro, Windows 7 Professional, Enterprise e Ultimate e Windows Server em versões mais recentes que o Windows Server 2008. Você não pode se conectar a computadores que executam a edição Home (como o Windows 10 Home). 

Para se conectar a um computador remoto, computador deve estar ativado, ele deve ter uma conexão de rede, área de trabalho remota deve estar habilitada, você deve ter acesso à rede para o computador remoto (Isso pode ser por meio da Internet), e você deve ter permissão para se conectar. Permissão para se conectar, você deve ser na lista de usuários. Antes de iniciar uma conexão, é uma boa ideia para pesquisar o nome do computador que você está se conectando e para verificar se são permitidas conexões de área de trabalho remota por meio de seu firewall.

## <a name="how-to-enable-remote-desktop"></a>Como habilitar a área de trabalho remota

A maneira mais simples para permitir o acesso a seu computador de um dispositivo remoto está usando as opções de área de trabalho remota em configurações. Desde que essa funcionalidade foi adicionada no Windows 10 Fall Creators update (1709), um separado que pode ser baixado, o aplicativo também está disponível, que fornece funcionalidade semelhante para versões anteriores do Windows. Você também pode usar o modo herdado de habilitação de área de trabalho remota, no entanto, esse método fornece menos funcionalidade e a validação.

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Atualização do criador de outono do Windows 10 (1709) ou posterior

Você pode configurar seu computador para o acesso remoto com algumas etapas simples.
1. No dispositivo que você deseja se conectar, selecione **inicie** e clique no **configurações** ícone à esquerda.
2. Selecione o **System** seguido de grupo a [ **área de trabalho remota** ](ms-settings:remotedesktop) item.
3. Use o controle deslizante para habilitar a área de trabalho remota.
4. Também é recomendável manter o PC ativos e detectável para facilitar as conexões. Clique em **Mostrar configurações** para habilitar.
5. Conforme necessário, adicione os usuários que podem se conectar remotamente ao clicar em **selecione os usuários que podem acessar remotamente esse PC**.
   1. Automaticamente, membros do grupo Administradores têm acesso.
6. Anote o nome deste PC sob **como se conectar a este computador**. Você precisará dela para configurar os clientes.

### <a name="windows-7-and-early-version-of-windows-10"></a>Windows 7 e a versão inicial do Windows 10

Para configurar seu computador para acesso remoto, baixe e execute o [Microsoft Remote Desktop Assistant](https://www.microsoft.com/download/details.aspx?id=50042). Este assistente atualiza as configurações do sistema para habilitar o acesso remoto, garante que o computador está ativo para conexões e verifica se o firewall permite conexões de área de trabalho remota. 

### <a name="all-versions-of-windows-legacy-method"></a>Todas as versões do Windows (método herdado)

Para habilitar a área de trabalho remota usando as propriedades do sistema herdado, siga as instruções para [conectar a outro computador usando a Conexão de área de trabalho remota](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## <a name="should-i-enable-remote-desktop"></a>Devo habilitar área de trabalho remota?

Se você quiser acessar o seu computador quando você estiver fisicamente na frente dele, você não precisa habilitar a área de trabalho remota. Habilitar área de trabalho remota abre uma porta em seu computador que está visível para sua rede local. Você só deve habilitar a área de trabalho remota em redes confiáveis, como sua casa. Você também não quiser habilitar a área de trabalho remota em qualquer computador onde o acesso é rigidamente controlado.

Lembre-se de que quando você habilita o acesso à área de trabalho remota, você está concedendo a qualquer pessoa no grupo de administradores, bem como os usuários selecionados, a capacidade de acessar remotamente as suas contas no computador.

Você deve garantir que cada conta que tenha acesso a seu computador está configurada com uma senha forte.

## <a name="why-allow-connections-only-with-network-level-authentication"></a>Por que permitir conexões somente com autenticação no nível da rede? 
 
Se você quiser restringir quem pode acessar seu PC, optar por permitir acesso apenas com o nível de autenticação rede (NLA). Quando você habilitar essa opção, os usuários precisam autenticar-se à rede, antes que eles possam se conectar a seu computador. Permitir conexões somente de computadores que executam a área de trabalho remota com NLA é um método de autenticação mais seguro que pode ajudar a proteger seu computador contra usuários mal-intencionados e de software. Para obter mais informações sobre NLA e área de trabalho remota, fazer check-out [NLA configurar para conexões do RDS](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx). 

Se você estiver se conectando remotamente a um computador em sua rede doméstica, de fora da rede, não selecione essa opção.
