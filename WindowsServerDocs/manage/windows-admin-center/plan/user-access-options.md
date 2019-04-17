---
title: Opções de acesso do usuário com o Windows Admin Center
description: Opções de acesso do usuário e provedores de identidade com o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9adea736d6e7ae181bdfe50289564083146f5b30
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2019
ms.locfileid: "9151991"
---
# Opções de acesso do usuário com o Windows Admin Center

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Quando implantado no Windows Server, o Windows Admin Center fornece um ponto centralizado de gerenciamento para seu ambiente de servidor. Controlando o acesso ao centro de administração do Windows, você pode melhorar a segurança de seu cenário de gerenciamento.

## Funções de acesso do gateway

Windows Admin Center define duas funções para acesso para o serviço de gateway: gateway usuários e administradores de gateway.

> [!NOTE]
> Acesso ao gateway do não implica acesso aos servidores de destino visíveis pelo gateway. Para gerenciar um servidor de destino, um usuário deve se conectar com as credenciais que têm privilégios administrativos no servidor de destino.

**Usuários gateway** pode se conectar para o serviço de gateway do Windows Admin Center para gerenciar servidores por meio desse gateway, mas não podem alterar as permissões de acesso nem o mecanismo de autenticação usado para autenticar para o gateway.

**Os administradores de gateway** pode definir quem tem acesso, bem como os usuários se o gateway.

>[!NOTE]
> Se não houver nenhum grupos de acesso definidos no Centro de administração do Windows, as funções refletirão acesso à conta do Windows para o servidor de gateway. 

[Configure o acesso de usuário e administrador de gateway no Windows Admin Center.](../configure/user-access-control.md)

## Opções de provedor de identidade

Os administradores de gateway podem escolher um destes procedimentos:

 - [Grupos de computadores do Active Directory/local](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory como o provedor de identidade para o Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### Autenticação de cartão inteligente

Ao usar o Active Directory ou grupos do computador local como o provedor de identidade, você pode impor a autenticação de cartão inteligente exigindo que os usuários que acessam o Windows Admin Center para ser um membro dos grupos de segurança baseada em cartão inteligente adicionais. [Configure a autenticação de cartão inteligente no Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### Acesso condicional e a autenticação multifator

Ao exigir autenticação do Azure AD para o gateway, você pode aproveitar os recursos de segurança adicionais como acesso condicional e a autenticação multifator fornecida pelo Azure AD. [Saiba mais sobre como configurar o acesso condicional com o Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## Controle de acesso baseado em função

Por padrão, os usuários exigem privilégios de administrador local completo nos computadores que desejam gerenciar usando o Windows Admin Center.
Isso permite que eles se conectar remotamente ao computador e garante que eles tenham permissões suficientes para exibir e modificar as configurações do sistema.
No entanto, alguns usuários talvez não precise acesso irrestrito ao computador para executar seus trabalhos.
Você pode usar o **controle de acesso baseado em função** no Centro de administração do Windows para fornecer esses usuários com acesso limitado ao computador, em vez de fazer administradores locais completo-los.

Controle de acesso baseado em função no Windows Admin Center funciona ao configurar cada servidor gerenciado com um ponto de extremidade do PowerShell [Administração Just enough](https://aka.ms/jeadocs) .
Esse ponto de extremidade define as funções, incluindo os aspectos do sistema que cada função tem permissão para gerenciar e quais usuários são atribuídos à função.
Quando um usuário se conecta ao ponto de extremidade restrito, uma conta de administrador local temporário é criada para gerenciar o sistema no nome dele.
Isso garante que o mesmo ferramentas que não têm seu próprio modelo de delegação ainda podem ser gerenciadas com o Windows Admin Center.
A conta temporária é removida automaticamente quando o usuário para gerenciar o computador por meio do Windows Admin Center.

Quando um usuário se conecta a um computador configurado com controle de acesso baseado em função, o Windows Admin Center primeiro verificará se eles são um administrador local.
Se eles forem, elas receberão a experiência completa do Windows Admin Center sem restrições.
Caso contrário, o Windows Admin Center verificará se o usuário pertence a qualquer uma das funções predefinidas.
Um usuário deve ter *acesso limitado* se eles pertencem a uma função do Windows Admin Center, mas não forem um administrador completo.
Por fim, se o usuário não for um administrador nem um membro de uma função, eles serão negados acesso para gerenciar o computador.

Controle de acesso baseado em função está disponível para as soluções do Gerenciador do servidor e Cluster de Failover.

### Funções disponíveis

Windows Admin Center oferece suporte a funções de usuário final a seguir:

Nome da função | Uso pretendido
----------|-------------
Administradores | Permite que os usuários usem a maioria dos recursos no Windows Admin Center sem conceder o acesso à área de trabalho remota ou o PowerShell. Essa função é ideal para cenários de "saltar server" em que você deseja limitar os pontos de entrada de gerenciamento em um computador.
Leitores | Permite aos usuários exibir informações e configurações no servidor, mas não fazer alterações.
Administradores do Hyper-V | Permite que os usuários façam alterações em máquinas virtuais Hyper-V e switches, mas limita a outros recursos para acesso somente leitura.

As seguintes extensões internas têm funcionalidade reduzida quando um usuário se conecta com acesso limitado:

- Arquivos (sem upload do arquivo ou download)
- PowerShell (não disponível)
- Área de trabalho remota (não disponível)
- Réplica de armazenamento (não disponível)

Neste momento, você não pode criar funções personalizadas para sua organização, mas você pode escolher quais usuários recebem acesso para cada função.

### Preparando-se para o controle de acesso baseado em função

Para aproveitar as contas locais temporárias, cada computador de destino precisa ser configurado para dar suporte ao controle de acesso baseado em função no Windows Admin Center.
O processo de configuração envolve instalar scripts do PowerShell e um ponto de extremidade de administração Just enough no computador usando a configuração de estado desejado.

Se você tiver apenas alguns computadores, você pode facilmente aplicar a configuração individualmente para cada computador usando a página de controle de acesso baseado em função no Windows Admin Center.
Ao configurar o controle de acesso baseado em função em um computador individual, grupos de segurança local são criados para controlar o acesso a cada função.
Você pode conceder acesso a usuários ou outros grupos de segurança, adicionando-as como membros dos grupos de segurança de função.

Para uma implantação em toda a empresa em vários computadores, você pode baixar o script de configuração do gateway e distribuí-lo para seus computadores usando um servidor de recepção de configuração de estado desejado, automação do Azure ou suas ferramentas de gerenciamento preferencial.
