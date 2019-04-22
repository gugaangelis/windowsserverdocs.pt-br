---
title: Opções de acesso do usuário com o Windows Admin Center
description: Opções de acesso do usuário e provedores de identidade com Windows Admin Center (projeto Paulo)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9adea736d6e7ae181bdfe50289564083146f5b30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825887"
---
# <a name="user-access-options-with-windows-admin-center"></a>Opções de acesso do usuário com o Windows Admin Center

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Quando implantado no Windows Server, Windows Admin Center fornece um ponto centralizado de gerenciamento para o seu ambiente de servidor. Controlando o acesso ao Windows Admin Center, você pode melhorar a segurança de seu cenário de gerenciamento.

## <a name="gateway-access-roles"></a>Funções de acesso do gateway

Windows Admin Center define duas funções para o acesso para o serviço de gateway: usuários do gateway e os administradores de gateway.

> [!NOTE]
> Acesso ao gateway não implica em acesso aos servidores de destino visíveis pelo gateway. Para gerenciar um servidor de destino, um usuário deve se conectar com as credenciais que possuam privilégios administrativos no servidor de destino.

**Usuários do gateway** podem se conectar ao serviço de gateway do Windows Admin Center para gerenciar servidores por meio do gateway, mas eles não podem alterar as permissões de acesso nem o mecanismo de autenticação usado para autenticar para o gateway.

**Os administradores de gateway** pode configurar a quem obtém acesso, bem como os usuários serão autenticados para o gateway.

>[!NOTE]
> Se não houver nenhum grupo de acesso definido no Windows Admin Center, as funções refletirá o acesso de conta do Windows para o servidor de gateway. 

[Configure o acesso de usuário e administrador do gateway no Windows Admin Center.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Opções de provedor de identidade

Os administradores de gateway podem escolher qualquer uma das seguintes opções:

 - [Grupos de computadores do Active Directory/local](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [O Azure Active Directory como o provedor de identidade para o Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Autenticação de cartão inteligente

Ao usar o Active Directory ou grupos de computadores locais como o provedor de identidade, você pode impor a autenticação de cartão inteligente, exigindo que os usuários que acessam a Windows Admin Center para ser um membro dos grupos de segurança adicional com base em cartão inteligente. [Configure a autenticação de cartão inteligente no Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Acesso condicional e autenticação multifator

Ao solicitar a autenticação do Azure AD para o gateway, você pode aproveitar os recursos de segurança adicionais, como acesso condicional e autenticação multifator fornecidos pelo Azure AD. [Saiba mais sobre como configurar o acesso condicional ao Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Controle de acesso baseado em função

Por padrão, os usuários exigem privilégios totais de administrador local nos computadores que desejam gerenciar usando o Windows Admin Center.
Isso lhes permite se conectar remotamente à máquina e garante que eles têm permissões suficientes para exibir e modificar as configurações do sistema.
No entanto, alguns usuários talvez não precise de acesso irrestrito à máquina para realizar seus trabalhos.
Você pode usar **controle de acesso baseado em função** no Windows Admin Center para fornecer a esses usuários com acesso limitado à máquina em vez de administradores locais completo-los de fazer.

Controle de acesso baseado em função no Windows Admin Center funciona com a configuração de cada servidor gerenciado com o PowerShell [Just Enough Administration](https://aka.ms/jeadocs) ponto de extremidade.
Esse ponto de extremidade define as funções, incluindo quais aspectos do sistema de cada função tem permissão para gerenciar e quais usuários são atribuídos à função.
Quando um usuário se conecta ao ponto de extremidade restrito, uma conta de administrador local temporário é criada para gerenciar o sistema em seu nome.
Isso garante que até mesma ferramentas que não têm seu próprio modelo de delegação ainda podem ser gerenciadas com o Windows Admin Center.
A conta temporária é removida automaticamente quando o usuário para gerenciar a máquina por meio do Windows Admin Center.

Quando um usuário se conecta a um computador configurado com controle de acesso baseado em função, o Windows Admin Center verificará primeiro se ele forem um administrador local.
Se elas forem, eles receberão a experiência completa do Windows Admin Center sem restrições.
Caso contrário, o Windows Admin Center verificará se o usuário pertence a qualquer uma das funções predefinidas.
Um usuário é considerado como tendo *acesso limitado* se eles pertencem a uma função do Windows Admin Center, mas não for um administrador completo.
Por fim, se o usuário não é um administrador nem um membro de uma função, eles serão negados acesso para gerenciar o computador.

Controle de acesso baseado em função está disponível para as soluções do Gerenciador do servidor e Cluster de Failover.

### <a name="available-roles"></a>Funções disponíveis

Windows Admin Center suporta as seguintes funções de usuário final:

Nome da função | Uso pretendido
----------|-------------
Administradores | Permite que os usuários usem a maioria dos recursos no Windows Admin Center sem conceder a eles acesso à área de trabalho remota ou o PowerShell. Essa função é boa para cenários de "servidor de salto" onde você deseja limitar os pontos de entrada de gerenciamento em um computador.
Leitores | Permite aos usuários exibir informações e configurações no servidor, mas não fazer alterações.
Administradores do Hyper-V | Permite que os usuários façam alterações em máquinas virtuais Hyper-V e comutadores, mas limita a outros recursos para acesso somente leitura.

As seguintes extensões internas têm funcionalidade reduzida quando um usuário se conecta com acesso limitado:

- Arquivos (sem carregamento do arquivo ou download)
- PowerShell (não disponível)
- Área de trabalho remota (não disponível)
- Réplica de armazenamento (não disponível)

Neste momento, você não pode criar funções personalizadas para sua organização, mas você pode escolher quais usuários recebem acesso a cada função.

### <a name="preparing-for-role-based-access-control"></a>Preparando para o controle de acesso baseado em função

Para aproveitar as contas locais temporárias, cada computador de destino precisa ser configurado para dar suporte a controle de acesso baseado em função no Windows Admin Center.
O processo de configuração envolve a instalação de scripts do PowerShell e um ponto de extremidade de administração Just Enough no computador usando o Desired State Configuration.

Se você tiver apenas alguns computadores, você pode facilmente aplicar a configuração individualmente em cada computador usando a página de controle de acesso baseado em função no Windows Admin Center.
Quando você define o controle de acesso baseado em função em um computador individual, grupos de segurança locais são criados para controlar o acesso a cada função.
Você pode conceder acesso a usuários ou outros grupos de segurança ao adicioná-los como membros dos grupos de segurança de função.

Para uma implantação de toda a empresa em vários computadores, você pode baixar o script de configuração do gateway e distribuí-lo para os computadores usando um servidor de pull de Desired State Configuration, automação do Azure ou suas ferramentas de gerenciamento preferenciais.
