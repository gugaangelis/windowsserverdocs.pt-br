---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: dentify suas metas de implantação de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3705d9c1e5675c233e91df9b5f1e73f68ce4b34c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945298"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identificar as metas de implantação do AD FS

Identificar corretamente suas metas de implantação de Serviços de Federação do Active Directory (AD FS) \( AD FS \) é essencial para o sucesso de seu projeto de design de AD FS. Priorize e, possivelmente, Combine suas metas de implantação para que você possa projetar e implantar AD FS usando uma abordagem iterativa. Você pode aproveitar as metas de implantação de AD FS existentes, documentadas e predefinidas que são relevantes para os designs de AD FS e desenvolver uma solução de trabalho para sua situação.

As versões anteriores do AD FS eram implantadas com mais frequência para obter o seguinte:

-   Fornecer aos seus funcionários ou clientes uma \- experiência de SSO baseada na Web ao acessar \- aplicativos baseados em declarações em sua empresa.

-   Fornecer aos seus funcionários ou clientes uma \- experiência de SSO baseada na Web para acessar recursos em qualquer organização de parceiro de Federação.

-   Fornecer aos seus funcionários ou clientes uma \- experiência de SSO baseada na Web ao acessar remotamente sites ou serviços hospedados internamente.

-   Fornecer aos seus funcionários ou clientes uma \- experiência de SSO baseada na Web ao acessar recursos ou serviços na nuvem.

Além disso, AD FS no Windows Server &reg; 2012 R2 adiciona a funcionalidade que pode ajudá-lo a obter o seguinte:

-   Ingresso no local de trabalho do dispositivo para SSO e autenticação de segundo fator contínua. Isso permite que as organizações permitam o acesso de dispositivos pessoais do usuário e gerenciem o risco ao fornecer esse acesso.

-   Gerenciamento de riscos com o controle de acesso de vários \- fatores. O AD FS fornece um nível sofisticado de autorização que controla quem tem acesso a quais aplicativos. Isso pode ser baseado em atributos de usuário \( UPN, email, associação de grupo de segurança, força de autenticação, etc. \) , atributos \( de dispositivo se o dispositivo for ingressado no local de trabalho \) ou solicitar a localização de \( rede, o endereço IP ou o agente do usuário \) .

-   Gerenciamento de riscos com a \- autenticação multifator adicional para aplicativos confidenciais. AD FS permite que você controle as políticas para potencialmente exigir a \- autenticação multifator globalmente ou por aplicativo. Além disso, AD FS fornece pontos de extensibilidade para qualquer \- fornecedor multifator se integrar profundamente a uma \- experiência multifator segura e direta para usuários finais.

-   Fornecer recursos de autenticação e autorização para acessar recursos da Web da extranet que são protegidos pelo proxy de aplicativo Web.

Para resumir, AD FS no Windows Server 2012 R2 podem ser implantados para atingir os seguintes objetivos em sua organização:

### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Permitir que os usuários acessem recursos em seus dispositivos pessoais de qualquer lugar

-   Ingresso no local de trabalho que permite aos usuários ingressar seus dispositivos pessoais no Active Directory corporativo e, assim, obter acesso e experiência ideal ao acessar recursos corporativos nesses dispositivos.

-   Pré- \- autenticação de recursos dentro da rede corporativa que são protegidos pelo proxy de aplicativo Web e acessados pela Internet.

-   Alteração de senha para permitir que os usuários alterem suas senhas de qualquer dispositivo ingressado no local de trabalho quando a senha expirar, de modo que eles possam continuar acessando os recursos.

### <a name="enhance-your-access-control-risk-management-tools"></a>Aprimore suas ferramentas de gerenciamento de riscos do controle de acesso
Gerenciar riscos é um aspecto importante da governança e da conformidade em todas as organizações de TI. Há vários aprimoramentos de gerenciamento de riscos de controle de acesso em AD FS no Windows Server &reg; 2012 R2, incluindo o seguinte:

-   Controles flexíveis com base no local de rede para controlar como um usuário é autenticado para acessar um \- aplicativo AD FS protegido.

-   Política flexível para determinar se um usuário precisa executar a \- autenticação multifator com base nos dados do usuário, nos dados do dispositivo e no local da rede.

-   Por \- controle de aplicativo para ignorar o SSO e forçar o usuário a fornecer credenciais toda vez que acessarem um aplicativo confidencial.

-   Flexível por \- política de acesso de aplicativo com base nos dados do usuário, nos dados do dispositivo ou no local da rede.

-   O bloqueio de extranet do AD FS permite aos administradores proteger contas do Active Directory contra ataques de força bruta da Internet.

-   Revogação de acesso para qualquer dispositivo ingressado no local de trabalho que esteja desabilitado ou excluído no Active Directory.

### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Use AD FS para aprimorar a \- experiência de entrada
A seguir estão os novos recursos de AD FS no Windows Server &reg; 2012 R2 que permitem que o administrador personalize e aprimore a experiência de entrada \- :

-   Personalização unificada do serviço do AD FS, em que as alterações são feitas uma vez e propagadas automaticamente para o restante dos servidores de federação do AD FS em um farm determinado.

-   Páginas de entrada atualizadas \- que parecem modernas e atendem automaticamente a fatores forma diferentes.

-   Suporte para fallback automático para \- autenticação baseada em formulários para dispositivos que não ingressaram no domínio corporativo, mas que ainda são usados para gerar solicitações de acesso de dentro da intranet da rede corporativa \( \) .

-   Controles simples para personalizar o logotipo da empresa, a imagem de ilustração, os links padrão para suporte de TI, a página inicial, a privacidade, etc.

-   Personalização de mensagens de descrição nas páginas de entrada \- .

-   Personalização de temas da Web.

-   Home Realm Discovery \( HRD \) com base no sufixo organizacional do usuário para a privacidade avançada dos parceiros da empresa.

-   Filtragem de HRD por \- aplicativo para escolher automaticamente um realm com base no aplicativo.

-   Um \- erro de clique no relatório para facilitar a solução de problemas de ti.

-   Mensagens de erro personalizáveis.

-   Opção de autenticação de usuário quando houver mais de um provedor de autenticação disponível.

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)


