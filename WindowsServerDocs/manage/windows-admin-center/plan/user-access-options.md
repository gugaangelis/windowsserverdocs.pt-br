---
title: Opções de acesso do usuário com o Windows Admin Center
description: Opções de acesso do usuário e provedores de identidade com o Windows Admin Center (Project Honolulu)
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: 184eca56dc14e91220a7fb7eb196c48706562ff7
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766709"
---
# <a name="user-access-options-with-windows-admin-center"></a>Opções de acesso do usuário com o Windows Admin Center

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Quando implantado no Windows Server, o Windows Admin Center fornece um ponto de gerenciamento centralizado para o seu ambiente de servidor. Ao controlar o acesso ao Windows Admin Center, você pode aprimorar a segurança do seu cenário de gerenciamento.

## <a name="gateway-access-roles"></a>Funções de acesso do gateway

O Windows Admin Center define duas funções para acesso ao serviço de gateway: usuários de gateway e administradores de gateway.

> [!NOTE]
> O acesso ao gateway não implica o acesso a servidores alvo visíveis pelo gateway. Para gerenciar um servidor alvo, um usuário deve conectar-se com credenciais que tenham privilégios administrativos no servidor alvo.

Os **usuários de gateway** podem se conectar ao serviço de gateway do Windows Admin Center para gerenciar servidores por meio desse gateway, mas não podem alterar permissões de acesso nem o mecanismo de autenticação usado para autenticação no gateway.

Os **administradores de gateway** podem configurar quem obtém acesso e como os usuários se autenticam no gateway.

>[!NOTE]
> Se não houver grupos de acesso definidos no Windows Admin Center, as funções refletirão o acesso à conta do Windows para o servidor de gateway.

[Configure o acesso de usuário e administrador do gateway no Windows Admin Center.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Opções do provedor de identidade

Os administradores de gateway podem escolher um dos seguintes:

 - [Grupos de computadores locais/Active Directory](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory como o provedor de identidade para o Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Autenticação de cartão inteligente

Ao usar o Active Directory ou grupos de computadores locais como o provedor de identidade, você pode impor a autenticação de cartão inteligente exigindo que os usuários que acessam o Windows Admin Center sejam membros de grupos de segurança adicionais baseados em cartão inteligente. [Configurar a autenticação de cartão inteligente no Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Acesso condicional e autenticação multifator

Ao exigir a autenticação do Azure AD para o gateway, você pode usar os recursos de segurança adicionais como o acesso condicional e a autenticação multifator fornecidos pelo Azure AD. [Saiba mais sobre como configurar o acesso condicional com o Azure Active Directory.](/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Controle de acesso baseado em função

Por padrão, os usuários exigem privilégios totais de administrador local nos computadores que desejam gerenciar usando o Windows Admin Center.
Isso permite que eles se conectem ao computador remotamente e garantem que tenham permissões suficientes para exibir e modificar as configurações do sistema.
No entanto, o trabalho de alguns usuários pode não exigir acesso irrestrito ao computador.
Você pode usar o **controle de acesso baseado em função** no Windows Admin Center para fornecer a tais usuários acesso limitado ao computador, em vez de torná-los administradores locais completos.

O controle de acesso baseado em função no Windows Admin Center funciona configurando cada servidor gerenciado com um ponto de extremidade de [Administração Suficiente](/powershell/scripting/learn/remoting/jea/overview) do PowerShell.
Esse ponto de extremidade define as funções, incluindo quais aspectos do sistema cada função tem permissão para gerenciar e quais usuários são atribuídos à função.
Quando um usuário se conecta ao ponto de extremidade restrito, uma conta de administrador local temporária é criada para gerenciar o sistema em nome dele.
Isso garante que até mesmo as ferramentas que não têm o próprio modelo de delegação ainda possam ser gerenciadas com o Windows Admin Center.
A conta temporária é removida automaticamente quando o usuário deixa de gerenciar o computador por meio do Windows Admin Center.

Quando um usuário se conecta a um computador configurado com o controle de acesso baseado em função, o Windows Admin Center primeiro verifica se eles são um administrador local.
Se forem, receberão a experiência completa do Windows Admin Center sem restrições.
Caso contrário, o Windows Admin Center verificará se o usuário pertence a alguma das funções predefinidas.
Tal usuário deverá ter *acesso limitado* se ele pertencer a uma função do Windows Admin Center, mas não for um administrador completo.
Por fim, se o usuário não for um administrador nem um membro de uma função, ele terá o acesso negado para gerenciar o computador.

O controle de acesso baseado em função está disponível para as soluções de Gerenciador do Servidor e Cluster de Failover.

### <a name="available-roles"></a>Funções disponíveis

O Windows Admin Center é compatível com as seguintes funções de usuário final:

Nome da função | Uso pretendido
----------|-------------
Administradores | Permite que os usuários usem a maioria dos recursos no Windows Admin Center sem conceder a eles acesso à Área de Trabalho Remota ou ao PowerShell. Essa função é boa para cenários de "servidor de salto" em que você deseja limitar os pontos de entrada de gerenciamento em um computador.
Leitores | Permite que os usuários vejam informações e configurações no servidor, mas não que façam alterações.
Administradores do Hyper-V | Permite que os usuários façam alterações em máquinas virtuais e comutadores do Hyper-V, mas limitam outros recursos a acesso somente leitura.

As seguintes extensões internas têm funcionalidade reduzida quando um usuário se conecta com acesso limitado:

- Arquivos (sem upload nem download de arquivo)
- PowerShell (não disponível)
- Área de Trabalho Remota (não disponível)
- Réplica de Armazenamento (não disponível)

Neste momento, você não pode criar funções personalizadas para sua organização, mas pode escolher quais usuários recebem acesso a cada função.

### <a name="preparing-for-role-based-access-control"></a>Como preparar-se para o controle de acesso baseado em função

Para usar as contas locais temporárias, cada computador alvo precisa ser configurado para oferecer suporte ao controle de acesso baseado em função no Windows Admin Center.
O processo de configuração envolve a instalação de scripts do PowerShell e um ponto de extremidade de Administração Suficiente no computador que usa a Desired State Configuration.

Se você tiver apenas alguns computadores, poderá facilmente aplicar a configuração individualmente a cada um deles usando a página controle de acesso baseado em função no Windows Admin Center.
Quando você configura o controle de acesso baseado em função em um computador individual, os grupos de segurança locais são criados para controlar o acesso a cada função.
Você pode permitir acesso a usuários ou a outros grupos de segurança adicionando-os como membros dos grupos de segurança de função.

Para uma implantação empresarial em vários computadores, você pode baixar o script de configuração do gateway e distribuí-lo para seus computadores usando um servidor de pull da Desired State Configuration, a Automação do Azure ou suas ferramentas de gerenciamento preferenciais.