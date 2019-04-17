---
title: Nomes de territórios
description: Este tópico fornece uma visão geral do uso de nomes de territórios na solicitação de conexão de servidor de política de rede no Windows Server 2016 de processamento.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 29a835c9965c2115d0aac1ef9b704e05f430db8b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="realm-names"></a>Nomes de territórios

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016


Você pode usar este tópico para obter uma visão geral do uso de nomes de território de processamento de solicitação de conexão do servidor de política de rede.

O atributo de nome de usuário RADIUS é uma cadeia de caracteres que normalmente contém um local de conta de usuário e um nome de conta de usuário. O local de conta de usuário também é chamado de território ou nome de território e sinônimo o conceito de domínio, incluindo domínios DNS, Active Directory® domínios e domínios Windows NT 4.0. Por exemplo, se uma conta de usuário está localizada no banco de contas de usuário para um domínio denominado example.com, example.com é o nome de território.

Em outro exemplo, se o atributo de nome de usuário RADIUS contém o nome de usuário user1@example.com, user1 é o nome da conta de usuário e example.com é o nome do território. Nomes de territórios podem ser apresentados no nome do usuário como um prefixo ou um sufixo:

- **Example\user1**. Neste exemplo, o nome do território **exemplo** é um prefixo; e também é o nome do Active Directory&reg; domínio de \(AD DS\) de serviços de domínio.

- **user1@example.com**. Neste exemplo, o nome do território **example.com** é um sufixo; e é um nome de domínio DNS ou o nome de um domínio do AD DS.

Você pode usar nomes de territórios configurados nas políticas de solicitação de conexão enquanto Projetando e implantando sua infraestrutura de RAIO para garantir que as solicitações de conexão são roteadas dos clientes RADIUS, também chamados de servidores de acesso à rede, para servidores RADIUS que possam se autenticar e autorizar a solicitação de conexão.

Quando NPS está configurado como um servidor RADIUS com a política de solicitação de conexão padrão, o NPS processa as solicitações de conexão que o servidor NPS é um membro do domínio e para os domínios confiáveis.

Para configurar o NPS para atuar como um proxy RADIUS e solicitações de conexão direta para domínios não confiáveis, você deve criar uma nova política de solicitação de conexão. A nova política de solicitação de conexão, você deve configurar o atributo de nome de usuário com o nome de território será contido o atributo de nome de usuário de solicitações de conexão que você deseja encaminhar. Você também deve configurar a política de solicitação de conexão com um grupo de servidores remotos RADIUS. A política de solicitação de conexão permite NPS calcular qual conexão solicita para encaminhar mensagens para o grupo de servidores remotos RADIUS com base na parte do território do atributo de nome de usuário.

## <a name="acquiring-the-realm-name"></a>Adquirir o nome de território

A parte do nome do território do nome do usuário é fornecido quando as credenciais de usuário do tipos baseada em senha durante uma conexão tentam ou quando um perfil de Conexão Manager (CM) no computador do usuário está configurado para fornecer o nome do território automaticamente.

Você pode designar que os usuários da rede fornecer seu nome de território ao digitar suas credenciais durante as tentativas de conexão de rede.

Por exemplo, você pode exigir que os usuários a digitar seu nome de usuário, incluindo o nome da conta de usuário e o nome de território em **nome de usuário** no **conectar** caixa de diálogo ao fazer uma conexão dial-up ou VPN (rede privada).

Além disso, se você criar um pacote de discagem personalizada com Conexão Manager Administration Kit (CMAK), você pode ajudar os usuários, adicionando o nome do território automaticamente ao nome da conta de usuário em perfis CM que estão instalados nos computadores dos usuários. Por exemplo, você pode especificar uma sintaxe de nome de usuário e o nome do território no perfil CM para que o usuário só precisa especificar o nome da conta de usuário ao digitar as credenciais. Neste caso, o usuário não precisa saber ou se lembrar do domínio onde se encontra sua conta de usuário.

Durante o processo de autenticação, depois que os usuários digitar suas credenciais baseada em senha, o nome do usuário é passado do cliente acesso ao servidor de acesso de rede. O servidor de acesso de rede constrói uma solicitação de conexão e inclui o nome de território dentro do atributo de nome de usuário RADIUS na mensagem de solicitação de acesso que é enviada para o proxy RADIUS ou o servidor.

Se o servidor RADIUS é um servidor NPS, a mensagem de solicitação de acesso é avaliada em relação ao conjunto de políticas de solicitação de conexão configurado. Condições sobre a política de solicitação de conexão podem incluir a especificação de conteúdo do atributo de nome de usuário.

Você pode configurar um conjunto de políticas de solicitação de conexão que são específicos para o nome do território no atributo de nome de usuário de mensagens recebidas. Isso permite que você crie regras de roteamento que encaminham mensagens de RAIO com um nome de território específico para um conjunto específico de servidores RADIUS quando NPS é usado como um proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Regras de manipulação de atributo

Antes da mensagem RADIUS é processada localmente (quando NPS está sendo usado como um servidor RADIUS) ou encaminhada para outro servidor RADIUS (quando NPS está sendo usado como um proxy RADIUS), o atributo de nome de usuário na mensagem pode ser modificado por regras de manipulação de atributo. Você pode configurar as regras de manipulação de atributo para o atributo de nome de usuário, selecionando **nome de usuário** sobre o **condições** guia nas propriedades de uma política de solicitação de conexão. Regras de manipulação de atributo NPS usam sintaxe de expressão regular.

Você pode configurar as regras de manipulação de atributo para o atributo de nome de usuário alterar o seguinte:

- Remova o nome do território do nome de usuário \ (também conhecido como território stripping\). Por exemplo, o nome de usuário user1@example.comé alterado para Usuário1.

- Altere o nome do território, mas não sua sintaxe. Por exemplo, o nome de usuário user1@example.comé alterado para user1@wcoast.example.com.

- Altere a sintaxe do nome do território. Por exemplo, o example\user1 de nome de usuário é alterado para user1@example.com.

Depois que o atributo de nome de usuário é modificado de acordo com as regras de manipulação de atributo que você configurar, configurações adicionais da diretiva de primeira solicitação de conexão correspondentes são usadas para determinar se:

- O servidor NPS processa a mensagem de solicitação de acesso localmente (quando NPS está sendo usado como um servidor RADIUS).

- O servidor NPS encaminha a mensagem para outro servidor RADIUS (quando NPS está sendo usado como um proxy RADIUS).

## <a name="configuring-the-the-nps-supplied-domain-name"></a>Configurando o o nome de domínio fornecido pelo NPS

Quando o nome do usuário não contiver um nome de domínio, o NPS fornece um. Por padrão, o nome de domínio fornecido pelo NPS é o domínio do qual o servidor NPS é um membro. Você pode especificar o nome de domínio fornecido pelo NPS através da seguinte configuração do registro:

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>Edição incorreta do registro pode danificar gravemente o sistema. Antes de fazer alterações no registro, você deve fazer backup de todos os dados importantes no computador.

Alguns servidores de acesso de rede não são da Microsoft excluir ou modificam o nome de domínio conforme especificado pelo usuário. Como resultado, a solicitação de acesso de rede é autenticada em relação ao domínio padrão, que pode não ser o domínio da conta do usuário. Para resolver esse problema, configure os servidores RADIUS para alterar o nome de usuário no formato correto com o nome de domínio correto.
