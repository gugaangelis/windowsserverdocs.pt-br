---
title: Área de trabalho remota - permitir o acesso ao seu computador
description: Saiba mais sobre suas opções para acessar remotamente o seu computador
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
ms.openlocfilehash: d3c85c1c765583eb26cef60ecd245708e0e02772
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297215"
---
# Área de trabalho remota - permitir o acesso ao seu computador

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Você pode usar a área de trabalho remota para se conectar a e controlar seu computador de um dispositivo remoto usando um [cliente de área de trabalho remota Microsoft](remote-desktop-clients.md) (disponível para Windows, iOS, macOS e Android). Quando você permite conexões remotas ao seu computador, você pode usar outro dispositivo se conectar ao seu computador e ter acesso a todos os seus aplicativos, arquivos e recursos de rede como se estivesse em sua mesa.  

> [!NOTE]
> Você pode usar a área de trabalho remota para se conectar ao Windows 10 Pro e Enteprise, Windows 8.1 e 8 versões de Enterprise e Pro, Windows 7 Professional, Enterprise e Ultimate e Windows Server mais recentes que o Windows Server 2008. Você não pode se conectar a computadores que executam a edição Home (como Windows 10 Home). 

Para se conectar a um computador remoto, que computador deve ser ativado, ele deve ter uma conexão de rede, a área de trabalho remota deve estar habilitada, você deve ter acesso à rede ao computador remoto (Isso pode ser por meio da Internet), e você deve ter permissão para se conectar. Permissão para se conectar, você deve estar na lista de usuários. Antes de iniciar uma conexão, é uma boa ideia para pesquisar o nome do computador que você está se conectando e certificar-se de conexões de área de trabalho remota são permitidas por meio de seu firewall.

## Como habilitar a área de trabalho remota

A maneira mais simples para permitir o acesso ao seu computador em um dispositivo remoto está usando opções de área de trabalho remota em configurações. Como essa funcionalidade foi adicionada ao Windows 10 Fall Creators update (1709), um download separado aplicativo também está disponível que fornece uma funcionalidade semelhante para versões anteriores do Windows. Você também pode usar a maneira herdada da habilitação de área de trabalho remota, no entanto, esse método fornece menos funcionalidade e validação.

### Windows 10 Fall Creator Update (1709) ou posterior

Você pode configurar seu computador para acesso remoto com algumas etapas simples.
1. No dispositivo deseja se conectar para selecionar o clique no ícone de **configurações** no lado esquerdo e **Iniciar** .
2. Selecione o grupo de **sistema** seguido pelo item de [**Área de trabalho remota**](ms-settings:remotedesktop) .
3. Use o controle deslizante para habilitar a área de trabalho remota.
4. Também é recomendável manter o computador ativo e detectável para facilitar conexões. Clique em **Mostrar configurações** para habilitar.
5. Conforme necessário, adicione os usuários que podem se conectar remotamente, clicando em **Selecionar usuários que podem acessar remotamente este computador**.
   1. Membros do grupo Administradores têm acesso automaticamente.
6. Anote o nome do PC em **como se conectar a este computador**. Isso será necessário configurar os clientes.

### Windows 7 e a versão anterior do Windows 10

Para configurar seu computador para acesso remoto, baixe e execute o [Assistente do remoto da área de trabalho do Microsoft](https://www.microsoft.com/download/details.aspx?id=50042). Este assistente atualiza as configurações do sistema para habilitar o acesso remoto, garante que o computador estiver ativo para conexões e verifica que seu firewall permite conexões de área de trabalho remota. 

### Todas as versões do Windows (método herdado)

Para habilitar a área de trabalho remota usando as propriedades do sistema herdados, siga as instruções para [conectar a outro computador usando a Conexão de área de trabalho remota](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## Posso deve habilitar a área de trabalho remota?

Se você deseja acessar seu computador quando você é fisicamente sentado em frente a ele, você não precisará habilitar a área de trabalho remota. Habilitar a área de trabalho remota abre uma porta no computador que estiver visível para sua rede local. Você só deve habilitar a área de trabalho remota em redes confiáveis, como sua casa. Você também não deseja habilitar a área de trabalho remota em qualquer computador onde o acesso é controlado rigidamente.

Lembre-se de que quando você habilita o acesso à área de trabalho remota, você está concedendo qualquer pessoa do grupo de administradores, bem como quaisquer usuários adicionais você selecionar, a capacidade de acessar remotamente suas contas no computador.

Você deve garantir que cada conta que tenha acesso ao seu computador está configurada com uma senha forte.

## Por que permitir conexões somente com autenticação no nível da rede? 
 
Se você deseja restringir quem pode acessar seu computador, optar por permitir acesso somente com o nível de autenticação NLA (rede). Quando você habilitar essa opção, os usuários precisam autenticar-se à rede antes que eles podem se conectar ao seu computador. Permitir conexões somente de computadores que executam a área de trabalho remota com NLA é um método de autenticação mais seguro que pode ajudar a proteger seu computador contra usuários e softwares mal. Para saber mais sobre NLA e área de trabalho remota, confira [NLA configurar conexões RDS](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx). 

Se você estiver se conectar remotamente a um computador em sua rede doméstica de fora da rede, não selecione essa opção.
