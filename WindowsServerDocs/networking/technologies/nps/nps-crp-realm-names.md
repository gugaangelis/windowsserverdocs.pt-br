---
title: Nomes de realm
description: Este tópico fornece uma visão geral do uso de nomes de território em processamento no Windows Server 2016 de solicitação de conexão de servidor de políticas de rede.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0257c4d15db4fc54e55ef430f6f2eea9cea2ec4d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882817"
---
# <a name="realm-names"></a>Nomes de realm

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016


Você pode usar este tópico para uma visão geral do uso de nomes de território no processamento de solicitação de conexão do servidor de políticas de rede.

O atributo RADIUS de nome de usuário é uma cadeia de caracteres que geralmente contém um local da conta de usuário e um nome de conta de usuário. O local da conta de usuário também é chamado de território ou nome de realm e é sinônimo com o conceito de domínio, incluindo domínios DNS, Active Directory® domínios e domínios do Windows NT 4.0. Por exemplo, se uma conta de usuário está localizada no banco de dados de contas de usuário para um domínio denominado exemplo.com, exemplo.com é o nome de realm.

Em outro exemplo, se o atributo RADIUS de nome de usuário contém o nome de usuário user1@example.com, user1 é o nome da conta de usuário e exemplo.com é o nome de realm. Nomes de território podem ser apresentados no nome do usuário como um prefixo ou como um sufixo:

- **Example\user1**. Neste exemplo, o nome de realm **exemplo** é um prefixo; e também é o nome de um Active Directory&reg; serviços de domínio \(AD DS\) domínio.

- **user1@example.com**. Neste exemplo, o nome de realm **exemplo.com** é um sufixo; e é um nome de domínio DNS ou o nome de um domínio AD DS.

Você pode usar nomes de realm configurados em diretivas de solicitação de conexão durante a criação e implantação de sua infraestrutura RADIUS para garantir que as solicitações de conexão sejam roteadas de clientes RADIUS, também chamados de servidores de acesso de rede para servidores RADIUS que pode autenticar e autorizar a solicitação de conexão.

Quando o NPS é configurado como um servidor RADIUS com a diretiva de solicitação de conexão padrão, o NPS processa solicitações de conexão para o domínio no qual o NPS é um membro em domínios confiáveis.

Para configurar o NPS para atuar como um proxy RADIUS e as solicitações de conexão direta para domínios não confiáveis, você deve criar uma nova diretiva de solicitação de conexão. A nova diretiva de solicitação de conexão, você deve configurar o atributo de nome de usuário com o nome de território que estarão contido no atributo de nome de usuário de solicitações de conexão que você deseja encaminhar. Você também deve configurar a diretiva de solicitação de conexão com um grupo de servidores RADIUS remotos. A diretiva de solicitação de conexão permite que o NPS calcular qual conexão solicita para encaminhar para o grupo de servidores remotos RADIUS com base no realm do atributo de nome de usuário.

## <a name="acquiring-the-realm-name"></a>Ao adquirir o nome de realm

A parte do nome do território do nome de usuário é fornecido quando as credenciais de usuário do tipos baseado em senha durante uma conexão tentam ou quando um perfil do Gerenciador de Conexão (CM) no computador do usuário é configurado para fornecer o nome de realm automaticamente.

Você pode designar que os usuários da sua rede fornecer seu nome de realm, ao digitar suas credenciais durante as tentativas de conexão de rede.

Por exemplo, você pode exigir que os usuários a digitar seu nome de usuário, incluindo o nome da conta de usuário e o nome de realm, no **nome de usuário** na **Connect** caixa de diálogo ao fazer uma rede privada dial-up ou virtual (VPN) conexão.

Além disso, se você criar um pacote de discagem personalizado com Conexão Manager Administration Kit (CMAK), você pode ajudar os usuários, adicionando o nome do território automaticamente ao nome da conta de usuário em perfis de CM que estão instalados nos computadores dos usuários. Por exemplo, você pode especificar uma sintaxe de nome de usuário e o nome do território no perfil de CM para que o usuário precisa apenas especificar o nome da conta de usuário ao digitar as credenciais. Nessa circunstância, o usuário não precisa saber ou se lembrar de onde se encontra sua conta de usuário de domínio.

Durante o processo de autenticação, depois que os usuários digitam suas credenciais com base em senha, o nome de usuário é passado do cliente de acesso para o servidor de acesso de rede. O servidor de acesso de rede cria uma solicitação de conexão e inclui o nome do território dentro do atributo RADIUS de nome de usuário na mensagem de solicitação de acesso que é enviada para o servidor ou proxy RADIUS.

Se o servidor RADIUS for um NPS, a mensagem de solicitação de acesso é avaliada em relação ao conjunto de diretivas de solicitação de conexão configurada. As condições na política de solicitação de conexão podem incluir a especificação do conteúdo do atributo de nome de usuário.

Você pode configurar um conjunto de diretivas de solicitação de conexão que são específicas para o nome do território dentro do atributo de nome de usuário de mensagens de entrada. Isso permite que você crie regras de roteamento que encaminham mensagens RADIUS com um nome de realm específico a um conjunto específico de servidores RADIUS quando NPS é usado como um proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Regras de manipulação de atributos

Antes da mensagem RADIUS é processada localmente (quando NPS está sendo usado como um servidor RADIUS) ou encaminhada para outro servidor RADIUS (quando NPS está sendo usado como um proxy RADIUS), o atributo de nome de usuário na mensagem pode ser modificado por regras de manipulação de atributos. Você pode configurar regras de manipulação de atributos para o atributo de nome de usuário, selecionando **nome de usuário** sobre o **condições** guia nas propriedades de uma diretiva de solicitação de conexão. Regras de manipulação de atributos NPS usam sintaxe de expressão regular.

Você pode configurar regras de manipulação de atributos para o atributo de nome de usuário alterar o seguinte:

- Remover o nome de realm do nome de usuário \(também conhecido como retirada de território\). Por exemplo, o nome de usuário user1@example.com é alterado para o Usuário1.

- Altere o nome de realm, mas não sua sintaxe. Por exemplo, o nome de usuário user1@example.com é alterado para user1@wcoast.example.com.

- Altere a sintaxe do nome do território. Por exemplo, o example\user1 de nome de usuário é alterado para user1@example.com.

Depois que o atributo de nome de usuário é modificado de acordo com as regras de manipulação de atributo que você configura, configurações adicionais da primeira diretiva de solicitação de conexão correspondentes são usadas para determinar se:

- O NPS processa a mensagem de solicitação de acesso localmente (quando NPS está sendo usado como um servidor RADIUS).

- O NPS encaminha a mensagem para outro servidor RADIUS (quando NPS está sendo usado como um proxy RADIUS).

## <a name="configuring-the-nps-supplied-domain-name"></a>Configurar o nome de domínio fornecido pelo NPS

Quando o nome de usuário não contém um nome de domínio, NPS fornece um. Por padrão, o nome de domínio fornecido pelo NPS é o domínio do qual o NPS é um membro. Você pode especificar o nome de domínio fornecido pelo NPS por meio da configuração de registro a seguir:

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>A edição incorreta do registro pode danificar gravemente o sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

Alguns servidores de acesso de rede não-Microsoft excluir ou modificam o nome de domínio conforme especificado pelo usuário. Como resultado, a solicitação de acesso de rede é autenticada em relação ao domínio padrão, que pode não ser o domínio da conta do usuário. Para resolver esse problema, configure os servidores RADIUS para alterar o nome de usuário no formato correto com o nome de domínio correto.
