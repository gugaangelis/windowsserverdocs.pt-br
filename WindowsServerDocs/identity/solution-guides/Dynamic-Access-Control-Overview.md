---
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: "Visão geral de controle de acesso dinâmico"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5cf74042c9b511abb1fbeb88224dea0c7f2c8706
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="dynamic-access-control-overview"></a>Visão geral de controle de acesso dinâmico

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Este tópico de visão geral para o profissional de TI descreve o controle de acesso dinâmico e associado elementos, que foram introduzidos no Windows Server 2012 e Windows 8.  
  
Controle de acesso dinâmico baseado em domínio permite que os administradores aplicar as restrições com base nas regras bem definidas que podem incluir a sensibilidade os recursos, o trabalho ou a função do usuário e a configuração do dispositivo que é usado para acessar esses recursos e as permissões de controle de acesso.  
  
Por exemplo, um usuário pode ter permissões diferentes quando eles acessarem um recurso de seus computadores office versus quando ele estiver usando um computador portátil ao longo de uma rede virtual privada. Ou acesso pode ser permitido apenas se um dispositivo atenda aos requisitos de segurança que são definidos pelos administradores de rede. Quando o controle de acesso dinâmico é usado, as permissões de um usuário alterar dinamicamente sem a intervenção do administrador adicionais se trabalho ou da função do usuário for alterado (resultando em alterações nos atributos de conta do usuário no AD DS).  
  
Não há suporte para o controle de acesso dinâmico em sistemas de operacionais Windows anteriores ao Windows Server 2012 e Windows 8. Quando o controle de acesso dinâmico é definido em ambientes com versões com suporte e sem suporte do Windows, somente as versões compatíveis implementará as alterações.  
  
Recursos e conceitos associados ao controle de acesso dinâmico incluem:  
  
-   [Regras de acesso central](#BKMK_Rules)  
  
-   [Políticas de acesso central](#BKMK_Policies)  
  
-   [Requerimentos judiciais ou Extrajudiciais](#BKMK_Claims)  
  
-   [Expressões](#BKMK_Expressions2)  
  
-   [Permissões propostas](#BKMK_Permissions2)  
  
### <a name="BKMK_Rules"></a>Regras de acesso central  
Uma regra de acesso central é uma expressão de regras de autorização que pode incluir uma ou mais condições envolvendo os grupos de usuários, declarações do usuário, declarações de dispositivo e propriedades do recurso. Várias regras de acesso central podem ser combinadas em uma política de acesso central.  
  
Se uma ou mais regras de acesso central tiverem sido definidas para um domínio, administradores de compartilhamento de arquivos podem corresponder as regras específicas para recursos específicos e requisitos de negócios.  
  
### <a name="BKMK_Policies"></a>Políticas de acesso central  
Políticas de acesso central são políticas de autorização que incluem expressões condicionais. Por exemplo, digamos que uma organização tem um requisito de negócios para restringir o acesso a informações pessoalmente identificáveis (PII) nos arquivos somente o proprietário do arquivo e membros do departamento de recursos humanos (RH) que têm permissão para exibir as informações PII. Isso representa uma política de toda a organização que se aplica a arquivos PII onde quer que eles estão localizados em servidores de arquivos em toda a organização. Para implementar essa política, uma organização precisa ser capaz de:  
  
-   Identifique e marcar os arquivos que contêm as informações de identificação pessoal.  
  
-   Identifique o grupo de membros do RH que têm permissão para exibir as informações de PII.  
  
-   Adicione a política de acesso central para uma regra de acesso central e aplicar as regras de acesso central para todos os arquivos que contêm informações de identificação pessoal, onde quer que esteja localizados entre os servidores de arquivos em toda a organização.  
  
Políticas de acesso central atuam como guarda-Chuvas de segurança que uma organização aplica-se em seus servidores. Essas políticas são além (mas não substituir) as políticas de acesso local ou listas de controle de acesso discricionário (DACLs) que são aplicadas a arquivos e pastas.  
  
### <a name="BKMK_Claims"></a>Requerimentos judiciais ou Extrajudiciais  
Uma declaração é uma parte exclusiva de informações sobre um usuário, dispositivo ou recurso que foi publicado por um controlador de domínio. Título do usuário, a classificação do departamento de um arquivo ou o estado de integridade de um computador são válidos exemplos de uma reivindicação. Uma entidade pode envolver mais de uma declaração e qualquer combinação dos requerimentos judiciais ou Extrajudiciais pode ser usada para autorizar o acesso aos recursos. Os seguintes tipos de declarações estão disponíveis nas versões compatíveis do Windows:  
  
-   **Declarações do usuário** atributos do Active Directory que estão associados um usuário específico.  
  
-   **Declarações de dispositivo** atributos do Active Directory que estão associados um objeto de computador específico.  
  
-   **Atributos de recursos** propriedades do recurso Global que são marcadas para uso em decisões de autorização e publicadas no Active Directory.  
  
Requerimentos judiciais ou Extrajudiciais tornam possível para os administradores fazer declarações preciso ou enterprise-toda a organização sobre usuários, dispositivos e recursos que podem ser incorporados em expressões, as regras e políticas.  
  
### <a name="BKMK_Expressions2"></a>Expressões  
Expressões condicionais são um aprimoramento de gerenciamento de controle de acesso que permitir ou negar o acesso aos recursos somente quando determinadas condições forem atendidas, por exemplo, a associação ao grupo, local ou o estado de segurança do dispositivo. Expressões são gerenciadas por meio da caixa de diálogo Configurações de segurança avançadas do Editor de ACL ou o Editor de regras de acesso Central no Active Directory administrativas central (ADAC).  
  
Expressões ajudam os administradores a gerenciar o acesso a recursos confidenciais com condições flexíveis em ambientes de negócios de cada vez mais complexos.  
  
### <a name="BKMK_Permissions2"></a>Permissões propostas  
Proposta permissões permitem que um administrador modelar com mais precisão o impacto de possíveis alterações para acessar configurações de controle sem realmente alterá-los.  
  
Prever o acesso a um recurso eficaz ajuda a planejar e configurar permissões para esses recursos antes de implementar essas alterações.  
  
## <a name="additional-changes"></a>Alterações adicionais  
Melhorias adicionais nas versões compatíveis do Windows que dão suporte a controle de acesso dinâmico incluem:  
  
### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>Suporte no protocolo de autenticação Kerberos confiavelmente fornecer declarações do usuário, as declarações de dispositivo e grupos de dispositivos.  
Por padrão, os dispositivos que executam qualquer uma das versões compatíveis do Windows são capazes de processar tíquetes Kerberos relacionados ao controle de acesso dinâmico, que incluem dados necessários para a autenticação composta. Controladores de domínio são capazes de emitir e responder a tíquetes Kerberos com informações relacionadas à autenticação compostas. Quando um domínio está configurado para reconhecer o controle de acesso dinâmico, dispositivos recebem requerimentos judiciais ou Extrajudiciais de controladores de domínio durante a autenticação inicial, e eles receberem tíquetes de autenticação composta quando enviar solicitações de tíquete de serviço. A autenticação composta resulta em um token de acesso que inclui a identidade do usuário e o dispositivo sobre os recursos que reconhece o controle de acesso dinâmico.  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Suporte para utilizar a configuração de política de grupo de (KDC) de centro de distribuição de chave para habilitar o controle de acesso dinâmico para um domínio.  
Cada controlador de domínio precisa ter a mesma configuração de política de modelo administrativo, localizada em **controle de acesso dinâmico do computador configuração Templates\System\KDC\Support e proteção Kerberos**.  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Suporte para utilizar a configuração de política de grupo de (KDC) de centro de distribuição de chave para habilitar o controle de acesso dinâmico para um domínio.  
Cada controlador de domínio precisa ter a mesma configuração de política de modelo administrativo, localizada em **controle de acesso dinâmico do computador configuração Templates\System\KDC\Support e proteção Kerberos**.  
  
### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>Suporte no Active Directory para armazenar objetos de política de acesso central, propriedades do recurso e declarações de dispositivo e usuário.  
  
### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>Suporte para usar política de grupo para implantar objetos de política de acesso central.  
A seguinte configuração de política de grupo permite implantar objetos de política de acesso central em servidores de arquivos em sua organização: **política de acesso do computador Configuration\Policies\ Windows \ Configurações de segurança Settings\File System\Central**.  
  
### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>Suporte para autorização baseada em declarações de arquivo e auditoria para sistemas de arquivos usando a política de grupo e Global Object Access Auditing  
Você deve habilitar a auditoria de política de acesso de central preparado auditar o acesso efetivo da política de acesso central, usando permissões propostas. Você defina essa configuração para o computador em **configuração avançada de política de auditoria** no **configurações de segurança** de um objeto de política de grupo (GPO). Depois de definir a configuração de segurança no GPO, você pode implantar o GPO para computadores em sua rede.  
  
### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>Suporte para transformar ou filtragem de objetos de política de declaração percorram relações de confiança de floresta do Active Directory  
Você pode filtrar ou transformar declarações de entrada e saídas que percorram uma relação de confiança de floresta. Há três cenários básicos para filtragem e transformar declarações:  
  
-   **Filtragem baseada em valor** filtros podem ser baseados no valor de uma reivindicação. Isso permite que a floresta confiável evitar declarações com determinados valores sejam enviadas a floresta confiante. Controladores de domínio em florestas de confiança podem usar a filtragem de acordo com o valor para se proteger contra um ataque de elevação de privilégio filtrando as declarações de entrada com valores específicos de floresta confiável.  
  
-   **Filtragem baseada em tipo de declaração** filtros são baseados no tipo de declaração, em vez do valor da reivindicação. Identificar o tipo de declaração pelo nome da reivindicação. Use filtragem na floresta confiável baseada em tipo de declaração e ele impede que o Windows enviando declarações que divulguem informações de floresta confiável.  
  
-   **Declaração baseada em tipo de transformação** manipula uma reivindicação antes de enviá-lo para o destino pretendido. Você pode usar a transformação de baseada em tipo de declaração na floresta confiável para generalizar uma reivindicação conhecida que contém informações específicas. Você pode usar transformações para generalizar o tipo de declaração, o valor da declaração ou ambos.  
  
## <a name="software-requirements"></a>Requisitos de software  
Como declarações e autenticação composta para controle de acesso dinâmico exigem extensões de autenticação Kerberos, qualquer domínio compatíveis com o controle de acesso dinâmico deve ter suficiente controladores de domínio executando as versões compatíveis do Windows para dar suporte à autenticação de clientes de Kerberos com reconhecimento de controle de acesso dinâmico. Por padrão, os dispositivos devem usar controladores de domínio em outros sites. Se nenhum controlador de domínio estiver disponível, a autenticação falhará. Portanto, você deve dar suporte a uma das seguintes condições:  
  
-   Cada domínio que suporta o controle de acesso dinâmico deve ter suficiente controladores de domínio que executam as versões compatíveis do Windows Server para dar suporte à autenticação de todos os dispositivos que executam as versões compatíveis do Windows ou Windows Server.  
  
-   Dispositivos que executam as versões compatíveis do Windows ou que não proteger recursos usando declarações ou identidade composta, deve desabilitar o suporte do protocolo Kerberos para controle de acesso dinâmico.  
  
Para domínios que dão suporte a declarações do usuário, todos os controladores de domínio executando as versões compatíveis do Windows server devem ser configurados com a configuração apropriada para dar suporte a declarações e autenticação composta e para fornecer proteção Kerberos. Defina configurações na política de modelo administrativo KDC da seguinte maneira:  
  
-   **Sempre fornecer declarações** Use essa configuração se todos os controladores de domínio estiver executando as versões compatíveis do Windows Server. Além disso, defina o nível funcional do domínio para o Windows Server 2012 ou superior.  
  
-   **Suporte** quando você usa essa configuração, monitorar controladores de domínio para garantir que o número de controladores de domínio que executam as versões compatíveis do Windows Server é suficiente para o número de computadores cliente que precisam acessar recursos protegidos pelo controle de acesso dinâmico.  
  
Se o domínio do usuário e o domínio do servidor de arquivos estão em diferentes florestas, todos os controladores de domínio raiz da floresta do servidor de arquivos devem ser definidos no Windows Server 2012 ou maior nível funcional.  
  
Se os clientes não reconhecer o controle de acesso dinâmico, deve haver uma relação de confiança bidirecional entre as duas florestas.  
  
Se as declarações são transformadas quando saem uma floresta, todos os controladores de domínio raiz da floresta do usuário devem ser definidos no Windows Server 2012 ou maior nível funcional.  
  
Um servidor de arquivos executando o Windows Server 2012 ou Windows Server 2012 R2 deve ter uma configuração de política de grupo que especifica se ele precisa obter declarações do usuário para tokens de usuário que não transmitem requerimentos judiciais ou Extrajudiciais. Essa configuração é definida por padrão para **automática**, o que resulta nessa configuração de política de grupo para ser ligado **em** se há uma política central que contém declarações do usuário ou dispositivo para esse servidor de arquivos. Se o servidor de arquivos contém discricionárias ACLs que incluem declarações do usuário, você precisa definir essa política de grupo **em** para que o servidor sabe para solicitar declarações em nome dos usuários que não fornecem requerimentos judiciais ou Extrajudiciais quando acessam o servidor.  
  
## <a name="additional-resource"></a>Recursos adicionais  
Para obter informações sobre a implementação de soluções baseadas nessa tecnologia, consulte [controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md).  
  


