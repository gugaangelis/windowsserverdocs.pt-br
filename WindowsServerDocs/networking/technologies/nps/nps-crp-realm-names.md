---
title: Nomes de realm
description: Este tópico fornece uma visão geral do uso de nomes de territórios no processamento de solicitação de conexão do servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e219169ba3132a18c0a76219fa8da96e30902090
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969393"
---
# <a name="realm-names"></a>Nomes de realm

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para obter uma visão geral do uso de nomes de territórios no processamento de solicitação de conexão do servidor de políticas de rede.

O atributo RADIUS de nome de usuário é uma cadeia de caracteres que normalmente contém um local de conta de usuário e um nome de conta de usuário. O local da conta de usuário também é chamado de realm ou nome de realm, e é sinônimo do conceito de domínio, incluindo domínios DNS, domínios de® de Active Directory e domínios do Windows NT 4,0. Por exemplo, se uma conta de usuário estiver localizada no banco de dados de contas de usuário para um domínio chamado example.com, example.com será o nome do realm.

Em outro exemplo, se o atributo RADIUS de nome de usuário contiver o nome de usuário user1@example.com , user1 será o nome da conta de usuário e example.com será o nome do realm. Os nomes de realm podem ser apresentados no nome de usuário como um prefixo ou como um sufixo:

- **Example\user1**. Neste exemplo, o **exemplo** de nome de realm é um prefixo; e também é o nome de um domínio de AD DS do Active Directory Domain &reg; Services \( \) .

- <strong>user1@example.com</strong>. Neste exemplo, o nome do Realm **example.com** é um sufixo; e é um nome de domínio DNS ou o nome de um domínio AD DS.

Você pode usar nomes de território configurados em políticas de solicitação de conexão ao criar e implantar sua infraestrutura RADIUS para garantir que as solicitações de conexão sejam roteadas de clientes RADIUS, também chamadas de servidores de acesso à rede, para servidores RADIUS que podem autenticar e autorizar a solicitação de conexão.

Quando o NPS é configurado como um servidor RADIUS com a política de solicitação de conexão padrão, o NPS processa as solicitações de conexão para o domínio no qual o NPS é membro e para domínios confiáveis.

Para configurar o NPS para atuar como um proxy RADIUS e encaminhar solicitações de conexão para domínios não confiáveis, você deve criar uma nova política de solicitação de conexão. Na nova política de solicitação de conexão, você deve configurar o atributo de nome de usuário com o nome de realm que estará contido no atributo User-Name das solicitações de conexão que você deseja encaminhar. Você também deve configurar a política de solicitação de conexão com um grupo de servidores RADIUS remoto. A política de solicitação de conexão permite que o NPS Calcule quais solicitações de conexão encaminhar para o grupo de servidores remotos RADIUS com base na parte de realm do atributo User-Name.

## <a name="acquiring-the-realm-name"></a>Adquirindo o nome do Realm

A parte do nome de realm do nome de usuário é fornecida quando o usuário digita credenciais baseadas em senha durante uma tentativa de conexão ou quando um perfil CM (Gerenciador de conexões) no computador do usuário é configurado para fornecer o nome do Realm automaticamente.

Você pode designar que os usuários da sua rede forneçam seu nome de realm ao digitar suas credenciais durante as tentativas de conexão de rede.

Por exemplo, você pode exigir que os usuários digitem seu nome de usuário, incluindo o nome da conta de usuário e o nome do Realm, em **nome de usuário** na caixa de diálogo **conectar** ao fazer uma conexão dial-up ou VPN (rede virtual privada).

Além disso, se você criar um pacote de discagem personalizado com o CMAK (Kit de administração do Gerenciador de conexões), poderá ajudar os usuários adicionando o nome do Realm automaticamente ao nome da conta de usuário em perfis CM que estão instalados nos computadores dos usuários. Por exemplo, você pode especificar um nome de realm e uma sintaxe de nome de usuário no perfil CM para que o usuário precise apenas especificar o nome da conta de usuário ao digitar as credenciais. Nessa circunstância, o usuário não precisa saber ou lembrar o domínio em que sua conta de usuário está localizada.

Durante o processo de autenticação, depois que os usuários digitam suas credenciais baseadas em senha, o nome de usuário é passado do cliente de acesso para o servidor de acesso à rede. O servidor de acesso à rede constrói uma solicitação de conexão e inclui o nome do Realm dentro do atributo RADIUS do nome de usuário na mensagem de solicitação de acesso que é enviada para o proxy ou servidor RADIUS.

Se o servidor RADIUS for um NPS, a mensagem de solicitação de acesso será avaliada em relação ao conjunto de políticas de solicitação de conexão configuradas. As condições na política de solicitação de conexão podem incluir a especificação do conteúdo do atributo User-Name.

Você pode configurar um conjunto de políticas de solicitação de conexão que são específicas para o nome do Realm dentro do atributo User-Name de mensagens de entrada. Isso permite que você crie regras de roteamento que encaminham mensagens RADIUS com um nome de realm específico para um conjunto específico de servidores RADIUS quando o NPS é usado como um proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Regras de manipulação de atributos

Antes que a mensagem RADIUS seja processada localmente (quando o NPS está sendo usado como um servidor RADIUS) ou encaminhado para outro servidor RADIUS (quando o NPS está sendo usado como um proxy RADIUS), o atributo User-Name na mensagem pode ser modificado por regras de manipulação de atributos. Você pode configurar regras de manipulação de atributos para o atributo User-Name selecionando **nome de usuário** na guia **condições** nas propriedades de uma política de solicitação de conexão. As regras de manipulação de atributos do NPS usam a sintaxe de expressão regular.

Você pode configurar regras de manipulação de atributo para o atributo User-Name para alterar o seguinte:

- Remova o nome de realm do nome de usuário \( também conhecido como remoção de realm \) . Por exemplo, o nome de usuário user1@example.com é alterado para Usuário1.

- Altere o nome do Realm, mas não sua sintaxe. Por exemplo, o nome de usuário user1@example.com é alterado para user1@wcoast.example.com .

- Altere a sintaxe do nome do realm. Por exemplo, o nome de usuário example\user1 é alterado para user1@example.com .

Depois que o atributo User-Name é modificado de acordo com as regras de manipulação de atributo que você define, as configurações adicionais da primeira política de solicitação de conexão correspondente são usadas para determinar se:

- O NPS processa a mensagem de solicitação de acesso localmente (quando o NPS está sendo usado como um servidor RADIUS).

- O NPS encaminha a mensagem para outro servidor RADIUS (quando o NPS está sendo usado como um proxy RADIUS).

## <a name="configuring-the-nps-supplied-domain-name"></a>Configurando o nome de domínio fornecido pelo NPS

Quando o nome de usuário não contiver um nome de domínio, o NPS fornecerá um. Por padrão, o nome de domínio fornecido pelo NPS é o domínio do qual o NPS é membro. Você pode especificar o nome de domínio fornecido pelo NPS por meio da seguinte configuração do registro:

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
```

> [!CAUTION]
> A edição incorreta do Registro pode danificar seriamente o sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

Alguns servidores de acesso à rede que não são da Microsoft excluem ou modificam o nome de domínio conforme especificado pelo usuário. Como resultado, a solicitação de acesso à rede é autenticada no domínio padrão, que pode não ser o domínio da conta do usuário. Para resolver esse problema, configure os servidores RADIUS para alterar o nome de usuário para o formato correto com o nome de domínio preciso.
