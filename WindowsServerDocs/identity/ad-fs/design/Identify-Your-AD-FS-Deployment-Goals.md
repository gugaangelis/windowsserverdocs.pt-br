---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Guia de Design do AD FS no Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f618add4c4803142b3bd7278908834a412f30999
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862567"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identificar as metas de implantação do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Identificar corretamente os seus serviços de Federação do Active Directory \(do AD FS\) metas de implantação é essencial para o sucesso do seu projeto de design do AD FS. Priorize e, possivelmente, combine suas metas de implantação para que você pode criar e implantar o AD FS usando uma abordagem iterativa. Pode tirar proveito de existentes, documentadas e predefinidos metas de implantação do AD FS que são relevantes para os designs do AD FS e desenvolvem uma solução para a sua situação.  
  
Em versões anteriores do AD FS foram implantadas com mais frequência para obter o seguinte:  
  
-   Fornecendo aos funcionários ou clientes com uma web\-com base, experiência de logon único ao acessar declarações\-com base em aplicativos dentro de sua empresa.  
  
-   Fornecendo aos funcionários ou clientes com uma web\-com base em experiência de SSO para acessar os recursos em qualquer organização de parceiro de Federação.  
  
-   Fornecendo aos funcionários ou clientes com uma Web\-com base, experiência de logon único quando internamente acessando remoto hospedado em sites ou serviços.  
  
-   Fornecendo aos funcionários ou clientes com uma web\-com base, experiência de logon único ao acessar recursos ou serviços na nuvem.  
  
Além dessas, o AD FS no Windows Server® 2012 R2 adiciona funcionalidade que pode ajudá-lo a obter o seguinte:  
  
-   Ingresso no local de trabalho do dispositivo para SSO e autenticação de segundo fator contínua. Isso permite que as organizações permitir o acesso de dispositivos pessoais do usuário e gerenciar o risco ao fornecer esse acesso.  
  
-   Gerenciamento de riscos com várias\-fatorar o controle de acesso. O AD FS fornece um nível sofisticado de autorização que controla quem tem acesso a quais aplicativos. Isso pode ser baseado em atributos de usuário \(UPN, email, associação de grupo de segurança, nível de autenticação, etc.\), atributos do dispositivo \(se o dispositivo é ingressado no local de trabalho\) ou solicitar atributos \(local de rede, o endereço IP ou o agente do usuário\).  
  
-   Gerenciamento de riscos com várias adicionais\-autenticação multifator para aplicativos confidenciais. O AD FS permite controlar políticas para potencialmente exigir multi\-autenticação fatores globalmente ou em uma base por aplicativo. Além disso, o AD FS fornece pontos de extensibilidade para qualquer multi\-fornecedor multifator integrar profundamente para uma de várias segura e transparente\-fatorar a experiência dos usuários finais.  
  
-   Fornecendo recursos de autenticação e autorização para acessar recursos da web da extranet protegidos pelo Proxy de aplicativo Web.  
  
Para resumir, o AD FS no Windows Server 2012 R2 podem ser implantado para atingir as metas a seguir em sua organização:  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Permitir que os usuários acessem os recursos em seus dispositivos pessoais de qualquer lugar  
  
-   Ingresso no local de trabalho que permite aos usuários ingressar seus dispositivos pessoais no Active Directory corporativo e, assim, obter acesso e experiência ideal ao acessar recursos corporativos nesses dispositivos.  
  
-   Pré\-autenticação de recursos dentro da rede corporativa que são protegidos pelo proxy de aplicativo Web e acessados pela internet.  
  
-   Alteração de senha para permitir que os usuários alterem suas senhas de qualquer dispositivo ingressado no local de trabalho quando a senha expirar, de modo que eles possam continuar acessando os recursos.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Aprimore suas ferramentas de gerenciamento de riscos de controle de acesso  
Gerenciar riscos é um aspecto importante da governança e da conformidade em todas as organizações de TI. Há vários acesso controle aprimoramentos de gerenciamento de riscos no AD FS no Windows Server® 2012 R2, incluindo o seguinte:  
  
-   Controles flexíveis com base no local de rede para determinar como um usuário é autenticado para acessar um AD FS\-aplicativo protegido.  
  
-   Políticas flexíveis para determinar se um usuário precisa executar várias\-autenticação multifator com base em dados do usuário, dados do dispositivo e local de rede.  
  
-   Por\-controle de aplicativo para ignorar SSO e forçar o usuário a fornecer credenciais sempre que acessar um aplicativo confidencial.  
  
-   Flexível por\-política de acesso do aplicativo com base em dados de usuário, dados do dispositivo ou local de rede.  
  
-   O bloqueio de extranet do AD FS permite aos administradores proteger contas do Active Directory contra ataques de força bruta da Internet.  
  
-   Revogação de acesso para qualquer dispositivo ingressado no local de trabalho que esteja desabilitado ou excluído no Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Usar o AD FS para aprimorar o sinal\-na experiência  
A seguir é novos recursos do AD FS no Windows Server® 2012 R2 que permitem que o administrador a personalizar e aprimorar o sinal\-na experiência:  
  
-   Personalização unificada do serviço do AD FS, em que as alterações são feitas uma vez e propagadas automaticamente para o restante dos servidores de federação do AD FS em um farm determinado.  
  
-   Atualizado o sinal\-nas páginas que têm aparência moderna e atendem a diferentes fatores forma automaticamente.  
  
-   Suporte para fallback automático para formulários\-com base em autenticação para dispositivos que não ingressaram no domínio corporativo, mas ainda são utilizados geram solicitações de acesso de dentro da rede corporativa \(intranet\).  
  
-   Controles simples para personalizar o logotipo da empresa, a imagem de ilustração, os links padrão para suporte de TI, a página inicial, a privacidade, etc.  
  
-   Mensagens de personalização da descrição no sinal\-nas páginas.  
  
-   Personalização de temas da Web.  
  
-   Home Realm Discovery \(HRD\) com base em sufixo organizacional do usuário para privacidade aprimorada dos parceiros da empresa.  
  
-   Filtragem de HRD um por\-base do aplicativo para selecionar automaticamente um território com base no aplicativo.  
  
-   Um\-clique em relatórios fáceis de IT de solução de problemas.  
  
-   Mensagens de erro personalizáveis.  
  
-   Opção de autenticação de usuário quando houver mais de um provedor de autenticação disponível.  
  
## <a name="see-also"></a>Consulte também  
[Guia de Design do AD FS no Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

