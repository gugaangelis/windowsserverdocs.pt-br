---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Guia de Design do AD FS no Windows Server 2012 R2
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f618add4c4803142b3bd7278908834a412f30999
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identificar as metas de implantação do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Identificar corretamente suas metas de implantação de \(AD FS\) de serviços de Federação do Active Directory é essencial para o sucesso do seu projeto de design do AD FS. Priorizar e, possivelmente, combine as metas de implantação para que você possa projetar e implantar o AD FS usando uma abordagem iterativa. Você pode tirar proveito de existentes, documentadas e predefinidas metas de implantação do AD FS que são relevantes para os designs do AD FS e desenvolvem uma solução de trabalho para sua situação.  
  
As versões anteriores do AD FS mais comumente foram implantadas para conseguir o seguinte:  
  
-   Fornecer a seus funcionários ou clientes com um web\ baseado, experiência de SSO ao acessar aplicativos baseados em claims\ dentro de sua empresa.  
  
-   Fornecendo uma experiência SSO baseado em web\ para acessar recursos em qualquer organização de parceiros de Federação funcionários ou clientes.  
  
-   Fornecer a seus funcionários ou clientes com um Web\ baseado, experiência de SSO quando o acesso remoto internamente hospedado sites ou serviços.  
  
-   Fornecendo seus funcionários ou clientes uma experiência de SSO baseado em web\ ao acessar recursos ou serviços na nuvem.  
  
Além dessas, AD FS no Windows Server® 2012 R2 adiciona funcionalidade que pode ajudá-lo a alcançar o seguinte:  
  
-   Ingresso do dispositivo local de trabalho para SSO e perfeito segundo autenticação multifator. Isso permite que as organizações permitir o acesso de dispositivos pessoais do usuário e gerenciar o risco ao fornecer esse acesso.  
  
-   Gerenciamento de risco com controle de acesso multi\ fator. AD FS fornece um nível avançado de autorização que controla quem tem acesso a quais aplicativos. Isso pode ser com base em atributos de usuário \ (UPN, email, a associação ao grupo de segurança, intensidade de autenticação, etc. \), atributos do dispositivo \ (se o dispositivo é o local de trabalho joined\) ou atributos de solicitação \ (rede local, endereço IP ou usuário agent\).  
  
-   Gerenciamento de risco com autenticação de fator de multi\ adicional para aplicativos confidenciais. AD FS permite que você controle políticas para exigir autenticação de fator multi\ globalmente ou em uma base por aplicativo. Além disso, o AD FS fornece pontos de extensibilidade de qualquer fornecedor multi\ fator integrar profundamente para uma experiência de fator multi\ seguro e perfeita para os usuários finais.  
  
-   Fornecer recursos de autenticação e autorização para acessar os recursos da web de extranet que estão protegidos pelo Proxy de aplicativo Web.  
  
Para resumir, AD FS no Windows Server 2012 R2 pode ser implantado para atingir as seguintes metas em sua organização:  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Permitir que os usuários acessem os recursos em seus dispositivos pessoais em qualquer lugar  
  
-   Ingresso no local de trabalho que permite aos usuários ingressar em seus dispositivos pessoais ao Active Directory corporativa e como resultado obter acesso e experiências de forma totalmente integradas ao acessar recursos corporativos desses dispositivos.  
  
-   Autenticação Pre\ de recursos dentro da rede corporativa que são protegidos pelo proxy de aplicativo Web e acessado a partir da internet.  
  
-   Alteração de senha para permitir que os usuários alterem as senhas de qualquer dispositivo associado ao local de trabalho quando sua senha expirou para que eles podem continuar a acessar os recursos.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Aprimorar suas ferramentas de gerenciamento de risco de controle de acesso  
Gerenciamento de risco é um aspecto importante de controle e conformidade em cada organização de TI. Há controle de acesso várias melhorias de gerenciamento de risco no AD FS no Windows Server® 2012 R2, incluindo o seguinte:  
  
-   Controles flexíveis com base no local de rede para controlar como um usuário é autenticado para acessar um aplicativo protegido FS\ de anúncios.  
  
-   Política flexível para determinar se um usuário precisa executar a autenticação de fator multi\ com base no local de rede, os dados do dispositivo e dados do usuário.  
  
-   Controle de aplicativo Per\ ignorar SSO e forçar o usuário forneça credenciais sempre que acessarem uma confidenciais de aplicativos.  
  
-   Política de acesso do aplicativo per\ flexível com base em dados do usuário, dados do dispositivo ou local de rede.  
  
-   AD FS Extranet bloqueio, que permite aos administradores proteger contas do Active Directory contra ataques de força bruta da internet.  
  
-   Revogação do acesso para qualquer dispositivo associado ao local de trabalho que estiver desativado ou excluído no Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Usar o AD FS para aprimorar a experiência de sign\  
Estes são os novos recursos do AD FS no Windows Server® 2012 R2 que permitem que o administrador personalizar e aprimorar a experiência de sign\:  
  
-   Personalização unificada do serviço do AD FS, onde as alterações são feitas de uma vez e propagadas automaticamente para o restante dos servidores de Federação do AD FS em um determinado farm.  
  
-   Atualizado sign\ em páginas que têm aparência moderna e cuidar de diferentes fatores forma automaticamente.  
  
-   Suporte para fallback automática para autenticação baseada em forms\ para dispositivos que não tenham ingressado no domínio corporativo, mas ainda são usados gerar solicitações de acesso de dentro de \(intranet\) a rede corporativa.  
  
-   Controles simples para personalizar o logotipo da empresa, a imagem de ilustração, o padrão links de suporte de TI, página inicial, privacidade, etc.  
  
-   Personalização das mensagens de descrição nas páginas de sign\.  
  
-   Personalização de temas da web.  
  
-   Doméstica território descoberta \(HRD\) com base em sufixo organizacional do usuário de privacidade avançados de parceiros da empresa.  
  
-   HRD filtragem com base em uma aplicativo per\ para selecionar automaticamente um território baseado no aplicativo.  
  
-   Clique One\ relatórios de erros para facilitar IT solução de problemas.  
  
-   Mensagens de erro personalizáveis.  
  
-   Opção de autenticação do usuário quando houver mais de um provedor de autenticação.  
  
## <a name="see-also"></a>Consulte também  
[Guia de Design do AD FS no Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

