---
title: Área de Trabalho Remota – permitir o acesso ao seu computador
description: Saiba mais sobre as opções para acessar remotamente seu computador
ms.topic: article
ms.assetid: 0f1557ed-53f7-4333-b023-c8e0f4b58bf4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f581aeee099c906c970fdc320150fd1d6c102e17
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946519"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>Área de Trabalho Remota – permitir o acesso ao seu computador

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Você pode usar a Área de Trabalho Remota para conectar e controlar seu computador de um dispositivo remoto usando um [Cliente de Área de Trabalho Remota da Microsoft](remote-desktop-clients.md) (disponível para Windows, iOS, macOS e Android). Ao permitir conexões remotas ao seu computador, você pode usar outro dispositivo para se conectar ao seu computador e ter acesso a todos os seus aplicativos, arquivos e recursos de rede, como se estivesse sentado em sua mesa.

> [!NOTE]
> Use a Área de Trabalho Remota para se conectar ao Windows 10 Pro e Enterprise, ao Windows 8.1 e 8 Enterprise e Pro, ao Windows 7 Professional, Enterprise e Ultimate e ao Windows Server em versões mais recentes que o Windows Server 2008. Não é possível se conectar a computadores que executam a Home Edition (como o Windows 10 Home).

Para se conectar a um computador remoto, o computador deve estar ligado, deve ter uma conexão de rede, a Área de Trabalho Remota deve estar habilitada, você deve ter acesso à rede do computador remoto (isso pode ser por meio da Internet) e você deve ter permissão para se conectar. Para obter permissão para se conectar, você deve estar na lista de usuários. Antes de iniciar uma conexão, é uma boa ideia pesquisar o nome do computador que você está se conectando para verificar se as conexões de Área de Trabalho Remota são permitidas no seu firewall.

## <a name="how-to-enable-remote-desktop"></a>Para habilitar a Área de Trabalho Remota

A maneira mais simples de permitir o acesso ao seu computador a partir de um dispositivo remoto é utilizando as opções de Área de Trabalho Remota em Configurações. Desde que essa funcionalidade foi adicionada na Windows 10 Fall Creators Update (1709), um aplicativo que pode ser baixado separadamente, também está disponível e fornece funcionalidade semelhante para versões anteriores do Windows. Você também pode usar o modo herdado de habilitação de Área de Trabalho Remota, no entanto, esse método fornece menos funcionalidade e validação.

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Windows 10 Fall Creator Update (1709) ou posterior

Você pode configurar seu computador para acesso remoto com algumas etapas simples.
1. No dispositivo que você deseja se conectar, selecione **Iniciar** e, em seguida, clique no ícone **Configurações** à esquerda.
2. Selecione o grupo **Sistema**, seguido do item [**Área de Trabalho Remota**](ms-settings:remotedesktop).
3. Use o controle deslizante para habilitar a Área de Trabalho Remota.
4. Recomendamos manter o computador ativo e detectável para facilitar as conexões. Clique em **Mostrar configurações** para habilitar.
5. Conforme necessário, adicione os usuários que podem se conectar remotamente clicando em **Selecione os usuários que podem acessar remotamente esse computador**.
   1. Os membros do grupo Administradores têm acesso automático.
6. Anote o nome deste computador em **Como se conectar a este computador**. Você precisará dele para configurar os clientes.

### <a name="windows-7-and-early-version-of-windows-10"></a>Windows 7 e a versão inicial do Windows 10

Para configurar seu computador para acesso remoto, baixe e execute o [Assistente de Área de Trabalho Remota da Microsoft](https://www.microsoft.com/download/details.aspx?id=50042). Este assistente atualiza as configurações do sistema para habilitar o acesso remoto, garante que o computador está ativo para conexões e verifica se o firewall permite conexões da Área de Trabalho Remota.

### <a name="all-versions-of-windows-legacy-method"></a>Todas as versões do Windows (método herdado)

Para habilitar a Área de Trabalho Remota usando as propriedades do sistema herdado, siga as instruções para [Conectar a outro computador usando a Conexão de área de trabalho remota](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## <a name="should-i-enable-remote-desktop"></a>Devo habilitar a Área de Trabalho Remota?

Caso deseje acessar o computador quando estiver fisicamente usando-o, você não precisará habilitar a Área de Trabalho Remota. Habilitar a Área de Trabalho Remota abre em seu computador uma porta visível na sua rede local. Você só deve habilitar a Área de Trabalho Remota em redes confiáveis, como sua casa. Também não é recomendado habilitar a Área de Trabalho Remota em qualquer computador onde o acesso é rigidamente controlado.

Lembre-se de que quando você habilita o acesso à Área de Trabalho Remota, você está concedendo a qualquer pessoa no grupo de administradores, bem como aos usuários selecionados, a capacidade de acessar remotamente suas contas no computador.

Você deve garantir que cada conta que tenha acesso a seu computador está configurada com uma senha forte.

## <a name="why-allow-connections-only-with-network-level-authentication"></a>Por que permitir conexões somente com Autenticação no Nível da Rede?

Se você quiser restringir quem pode acessar seu computador, opte por permitir acesso apenas com a NLA (Autenticação no Nível da Rede). Ao habilitar essa opção, os usuários precisam autenticar-se à rede, antes que possam se conectar a seu computador. Permitir conexões somente de computadores que executam a Área de Trabalho Remota com NLA é um método de autenticação mais seguro que pode ajudar a proteger seu computador contra usuários e softwares mal-intencionados. Para saber mais sobre NLA e a Área de Trabalho Remota, confira [Configurar NLA para conexões do RDS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732713(v=ws.11)).

Se você estiver se conectando remotamente a um computador em sua rede doméstica, de fora da rede, não selecione essa opção.
