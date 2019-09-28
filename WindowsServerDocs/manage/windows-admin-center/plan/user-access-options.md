---
title: Opções de acesso do usuário com o centro de administração do Windows
description: Opções de acesso do usuário e provedores de identidade com o centro de administração do Windows (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 084cdae0bf8ca0eb3aff1f4679d30978b860efef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356921"
---
# <a name="user-access-options-with-windows-admin-center"></a>Opções de acesso do usuário com o centro de administração do Windows

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Quando implantado no Windows Server, o centro de administração do Windows fornece um ponto de gerenciamento centralizado para o seu ambiente de servidor. Ao controlar o acesso ao centro de administração do Windows, você pode melhorar a segurança do seu cenário de gerenciamento.

## <a name="gateway-access-roles"></a>Funções de acesso do gateway

O centro de administração do Windows define duas funções para acesso ao serviço de gateway: usuários de gateway e administradores de gateway.

> [!NOTE]
> O acesso ao gateway não implica o acesso aos servidores de destino visíveis pelo Gateway. Para gerenciar um servidor de destino, um usuário deve se conectar com credenciais que têm privilégios administrativos no servidor de destino.

**Os usuários de gateway** podem se conectar ao serviço de gateway do centro de administração do Windows para gerenciar servidores por meio desse gateway, mas não podem alterar as permissões de acesso nem o mecanismo de autenticação usado para autenticar no gateway.

**Os administradores de gateway** podem configurar quem obtém acesso e como os usuários serão autenticados no gateway.

>[!NOTE]
> Se não houver grupos de acesso definidos no centro de administração do Windows, as funções refletirão o acesso à conta do Windows para o servidor de gateway. 

[Configure o acesso de usuário e administrador do gateway no centro de administração do Windows.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Opções do provedor de identidade

Os administradores de gateway podem escolher um dos seguintes:

 - [Grupos de computadores Active Directory/locais](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory como o provedor de identidade para o centro de administração do Windows](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Autenticação de cartão inteligente

Ao usar Active Directory ou grupos de computadores locais como o provedor de identidade, você pode impor a autenticação de cartão inteligente exigindo que os usuários que acessam o centro de administração do Windows sejam membros de grupos de segurança adicionais baseados em cartão inteligente. [Configurar a autenticação de cartão inteligente no centro de administração do Windows.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Acesso condicional e autenticação multifator

Ao exigir a autenticação do Azure AD para o gateway, você pode aproveitar os recursos de segurança adicionais como o acesso condicional e a autenticação multifator fornecida pelo Azure AD. [Saiba mais sobre como configurar o acesso condicional com o Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Controle de acesso baseado em função

Por padrão, os usuários exigem privilégios totais de administrador local nas máquinas que desejam gerenciar usando o centro de administração do Windows.
Isso permite que eles se conectem ao computador remotamente e garantem que eles tenham permissões suficientes para exibir e modificar as configurações do sistema.
No entanto, alguns usuários podem não precisar de acesso irrestrito ao computador para executar seus trabalhos.
Você pode usar o **controle de acesso baseado em função** no centro de administração do Windows para fornecer aos usuários acesso limitado ao computador em vez de torná-los administradores locais completos.

O controle de acesso baseado em função no centro de administração do Windows funciona configurando cada servidor gerenciado com um ponto de extremidade de [Administração suficiente](https://aka.ms/jeadocs) do PowerShell.
Esse ponto de extremidade define as funções, incluindo quais aspectos do sistema cada função tem permissão para gerenciar e quais usuários são atribuídos à função.
Quando um usuário se conecta ao ponto de extremidade restrito, uma conta de administrador local temporária é criada para gerenciar o sistema em seu nome.
Isso garante que até mesmo as ferramentas que não têm seu próprio modelo de delegação ainda podem ser gerenciadas com o centro de administração do Windows.
A conta temporária é removida automaticamente quando o usuário para de gerenciar o computador por meio do centro de administração do Windows.

Quando um usuário se conecta a um computador configurado com o controle de acesso baseado em função, o centro de administração do Windows primeiro verificará se eles são um administrador local.
Se estiverem, eles receberão a experiência completa do centro de administração do Windows sem restrições.
Caso contrário, o centro de administração do Windows verificará se o usuário pertence a qualquer uma das funções predefinidas.
Um usuário deve ter *acesso limitado* se ele pertencer a uma função do centro de administração do Windows, mas não for um administrador completo.
Por fim, se o usuário não for um administrador nem um membro de uma função, ele terá o acesso negado para gerenciar o computador.

O controle de acesso baseado em função está disponível para as soluções de Gerenciador do Servidor e cluster de failover.

### <a name="available-roles"></a>Funções disponíveis

O centro de administração do Windows dá suporte às seguintes funções de usuário final:

Nome da função | Uso pretendido
----------|-------------
Administradores | Permite que os usuários usem a maioria dos recursos no centro de administração do Windows sem conceder a eles acesso ao Área de Trabalho Remota ou ao PowerShell. Essa função é boa para cenários de "servidor de salto" em que você deseja limitar os pontos de entrada de gerenciamento em um computador.
Leitores | Permite que os usuários exibam informações e configurações no servidor, mas não façam alterações.
Administradores do Hyper-V | Permite que os usuários façam alterações em máquinas virtuais e comutadores do Hyper-V, mas limitam outros recursos ao acesso somente leitura.

As seguintes extensões internas têm funcionalidade reduzida quando um usuário se conecta com acesso limitado:

- Arquivos (sem carregamento ou download de arquivo)
- PowerShell (não disponível)
- Área de Trabalho Remota (não disponível)
- Réplica de armazenamento (não disponível)

Neste momento, você não pode criar funções personalizadas para sua organização, mas pode escolher quais usuários recebem acesso a cada função.

### <a name="preparing-for-role-based-access-control"></a>Preparando para o controle de acesso baseado em função

Para aproveitar as contas locais temporárias, cada computador de destino precisa ser configurado para oferecer suporte ao controle de acesso baseado em função no centro de administração do Windows.
O processo de configuração envolve a instalação de scripts do PowerShell e um ponto de extremidade de administração suficiente no computador usando a configuração de estado desejado.

Se você tiver apenas alguns computadores, poderá facilmente aplicar a configuração individualmente a cada computador usando a página controle de acesso baseado em função no centro de administração do Windows.
Quando você configura o controle de acesso baseado em função em um computador individual, os grupos de segurança locais são criados para controlar o acesso a cada função.
Você pode conceder acesso a usuários ou a outros grupos de segurança adicionando-os como membros dos grupos de segurança de função.

Para uma implantação de toda a empresa em vários computadores, você pode baixar o script de configuração do gateway e distribuí-lo para seus computadores usando um servidor de pull de configuração de estado desejado, a automação do Azure ou suas ferramentas de gerenciamento preferenciais.
