---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Guia de Design do AD FS no Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1bc898ba858cf49a71edcb89b10981d0bc8c1986
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853069"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identificar as metas de implantação do AD FS

Identificar corretamente seu Serviços de Federação do Active Directory (AD FS) \(AD FS\) objetivos de implantação é essencial para o sucesso do seu projeto de AD FS design. Priorize e, possivelmente, Combine suas metas de implantação para que você possa projetar e implantar AD FS usando uma abordagem iterativa. Você pode aproveitar as metas de implantação de AD FS existentes, documentadas e predefinidas que são relevantes para os designs de AD FS e desenvolver uma solução de trabalho para sua situação.  
  
As versões anteriores do AD FS eram implantadas com mais frequência para obter o seguinte:  
  
-   Fornecer aos seus funcionários ou clientes um\-da Web com base na experiência de SSO ao acessar aplicativos baseados em declarações\-em sua empresa.  
  
-   Fornecer aos seus funcionários ou clientes um\-Web com base na experiência de SSO para acessar recursos em qualquer organização de parceiro de Federação.  
  
-   Fornecer aos seus funcionários ou clientes um\-da Web com base na experiência de SSO ao acessar remotamente sites ou serviços hospedados internamente.  
  
-   Fornecer aos seus funcionários ou clientes um\-da Web com base na experiência de SSO ao acessar recursos ou serviços na nuvem.  
  
Além disso, AD FS no Windows Server&reg; 2012 R2 adiciona funcionalidade que pode ajudá-lo a obter o seguinte:  
  
-   Ingresso no local de trabalho do dispositivo para SSO e autenticação de segundo fator contínua. Isso permite que as organizações permitam o acesso de dispositivos pessoais do usuário e gerenciem o risco ao fornecer esse acesso.  
  
-   Gerenciamento de riscos com o controle de acesso de vários\-factor. O AD FS fornece um nível sofisticado de autorização que controla quem tem acesso a quais aplicativos. Isso pode ser baseado em atributos de usuário \(UPN, email, associação de grupo de segurança, força de autenticação, etc.\), atributos de dispositivo \(se o dispositivo é ingressado no local\) ou atributos de solicitação \(local de rede, endereço IP ou\)de agente do usuário.  
  
-   Gerenciamento de riscos com a autenticação de vários fatores\-adicional para aplicativos confidenciais. AD FS permite que você controle as políticas para potencialmente exigir a autenticação de vários fatores\-globalmente ou por aplicativo. Além disso, o AD FS fornece pontos de extensibilidade para qualquer fornecedor de vários\-fatores se integrar profundamente para uma experiência de vários fatores de\-segura e direta para usuários finais.  
  
-   Fornecer recursos de autenticação e autorização para acessar recursos da Web da extranet que são protegidos pelo proxy de aplicativo Web.  
  
Para resumir, AD FS no Windows Server 2012 R2 podem ser implantados para atingir os seguintes objetivos em sua organização:  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Permitir que os usuários acessem recursos em seus dispositivos pessoais de qualquer lugar  
  
-   Ingresso no local de trabalho que permite aos usuários ingressar seus dispositivos pessoais no Active Directory corporativo e, assim, obter acesso e experiência ideal ao acessar recursos corporativos nesses dispositivos.  
  
-   Pré\-a autenticação de recursos dentro da rede corporativa que são protegidos pelo proxy de aplicativo Web e acessados pela Internet.  
  
-   Alteração de senha para permitir que os usuários alterem suas senhas de qualquer dispositivo ingressado no local de trabalho quando a senha expirar, de modo que eles possam continuar acessando os recursos.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Aprimore suas ferramentas de gerenciamento de riscos do controle de acesso  
Gerenciar riscos é um aspecto importante da governança e da conformidade em todas as organizações de TI. Há vários aprimoramentos de gerenciamento de riscos de controle de acesso em AD FS no Windows Server&reg; 2012 R2, incluindo o seguinte:  
  
-   Controles flexíveis com base no local de rede para controlar como um usuário é autenticado para acessar um aplicativo AD FS\-protegido.  
  
-   Política flexível para determinar se um usuário precisa executar a autenticação de vários fatores\-com base nos dados do usuário, nos dados do dispositivo e no local da rede.  
  
-   Por\-controle de aplicativo para ignorar o SSO e forçar o usuário a fornecer credenciais toda vez que acessarem um aplicativo confidencial.  
  
-   Flexível por\-política de acesso de aplicativo com base nos dados do usuário, nos dados do dispositivo ou no local da rede.  
  
-   O bloqueio de extranet do AD FS permite aos administradores proteger contas do Active Directory contra ataques de força bruta da Internet.  
  
-   Revogação de acesso para qualquer dispositivo ingressado no local de trabalho que esteja desabilitado ou excluído no Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Use AD FS para aprimorar o\-de entrada na experiência  
A seguir estão os novos recursos de AD FS no Windows Server&reg; 2012 R2 que permitem que o administrador personalize e aprimore o\-de entrada em experiência:  
  
-   Personalização unificada do serviço do AD FS, em que as alterações são feitas uma vez e propagadas automaticamente para o restante dos servidores de federação do AD FS em um farm determinado.  
  
-   O\-de entrada atualizado em páginas que parecem modernas e atendem automaticamente a fatores forma diferentes.  
  
-   Suporte para fallback automático para formulários\-autenticação baseada em dispositivos que não ingressaram no domínio corporativo, mas que ainda são usados para gerar solicitações de acesso de dentro da rede corporativa \(\)de intranet.  
  
-   Controles simples para personalizar o logotipo da empresa, a imagem de ilustração, os links padrão para suporte de TI, a página inicial, a privacidade, etc.  
  
-   Personalização de mensagens de descrição no\-de entrada em páginas.  
  
-   Personalização de temas da Web.  
  
-   Home Realm Discovery \(HRD\) com base no sufixo organizacional do usuário para a privacidade avançada dos parceiros da empresa.  
  
-   Filtragem de HRD por\-de aplicativo para escolher automaticamente um realm com base no aplicativo.  
  
-   Um\-clique em relatório de erros para facilitar a solução de problemas de ti.  
  
-   Mensagens de erro personalizáveis.  
  
-   Opção de autenticação de usuário quando houver mais de um provedor de autenticação disponível.  
  
## <a name="see-also"></a>Consulte também  
[Guia de design do AD FS no Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

