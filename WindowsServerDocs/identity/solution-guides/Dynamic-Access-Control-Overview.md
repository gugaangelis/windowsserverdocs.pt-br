---
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: Visão geral do Controle de Acesso Dinâmico
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 343e51f113f54c3965ef45d49f5d8fd64c260991
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357515"
---
# <a name="dynamic-access-control-overview"></a>Visão geral do Controle de Acesso Dinâmico

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Este tópico de visão geral para profissionais de TI descreve o Controle de Acesso Dinâmico e seus elementos associados, que foram introduzidos no Windows Server 2012 e no Windows 8.  
  
O Controle de Acesso Dinâmico baseado em domínio permite que os administradores apliquem permissões de controle de acesso e restrições baseadas em regras bem definidas que podem incluir a sensibilidade dos recursos, o emprego ou a função do usuário e a configuração do dispositivo que é usado para acessar esses recursos.  
  
Por exemplo, um usuário podem ter permissões diferentes ao acessar um recurso de seu escritório diferentes de quando ele usa um computador portátil de uma rede privada virtual. Ou o acesso pode ser permitido somente quando um dispositivo atende aos requisitos de segurança definidos pelos administradores da rede. Quando o controle de acesso dinâmico é usado, as permissões de um usuário são alteradas dinamicamente sem intervenção adicional do administrador se o trabalho ou a função do usuário mudar (resultando em alterações nos atributos da conta do usuário no AD DS).  
  
Não há suporte para o Controle de Acesso Dinâmico em sistemas operacionais anteriores ao Windows Server 2012 e ao Windows 8. Quando o Controle de Acesso Dinâmico é configurado em ambientes com versões do Windows com suporte e sem suporte, somente as versões com suporte implementarão as alterações.  
  
Recursos e conceitos associados ao Controle de Acesso Dinâmico incluem:  
  
-   [Regras de acesso central](#BKMK_Rules)  
  
-   [Políticas de acesso central](#BKMK_Policies)  
  
-   [Declarações](#BKMK_Claims)  
  
-   [Expressões](#BKMK_Expressions2)  
  
-   [Permissões propostas](#BKMK_Permissions2)  
  
### <a name="BKMK_Rules"></a>Regras de acesso central  
Uma regra de acesso central é uma expressão de regras de autorização que pode incluir uma ou mais condições que envolvem grupos de usuários, declarações de usuário, declarações de dispositivo e propriedades de recurso. Várias regras de acesso central podem ser combinadas em uma política de acesso central.  
  
Se uma ou mais regras de acesso central foram definidas para um domínio, os administradores de compartilhamento de arquivos podem corresponder regras específicas a requisitos específicos de recursos e negócios.  
  
### <a name="BKMK_Policies"></a>Políticas de acesso central  
As políticas de acesso central são políticas de autorização que incluem expressões condicionais. Por exemplo, digamos que uma organização tenha um requisito de negócios para restringir o acesso a PII (informações de identificação pessoal) em arquivos apenas ao proprietário do arquivo e aos membros do departamento de RH (recursos humanos) que têm permissão para exibir informações de PII. Isso representa uma política em toda a organização que se aplica a todos os arquivos PII onde estiverem localizados em servidores de arquivos em toda a organização. Para implementar esta política, uma empresa precisa ser capaz de:  
  
-   Identificar e marcar os arquivos que contêm o PII.  
  
-   Identificar o grupo de membros de RH que têm permissão para exibir as informações PII.  
  
-   Adicionar a política de acesso central a uma regra de acesso central e aplicar a regra de acesso central a todos os arquivos que contêm o PII, independentemente de onde estiverem localizados entre os servidores de arquivos da organização.  
  
As políticas de acesso central atuam como protetores de segurança que uma organização aplica em seus servidores. Essas políticas complementam (mas não substituem) as políticas de acesso local ou as listas de controle de acesso discricionário (DACLs) que são aplicadas a arquivos e pastas.  
  
### <a name="BKMK_Claims"></a>Declarações  
Uma declaração é uma peça única de informação sobre um usuário, dispositivo ou recurso é publicada por um controlador de domínio. O título do usuário, a classificação de departamento de um arquivo ou o estado de integridade de um computador são exemplos válidos de uma declaração. Uma entidade pode envolver mais de uma declaração e qualquer combinação de declarações pode ser usada para autorizar o acesso a recursos. Os seguintes tipos de declarações estão disponíveis nas versões do Windows com suporte:  
  
-   **Declarações de usuário** Atributos do Active Directory que estão associados a um usuário específico.  
  
-   **Declarações de dispositivo** Atributos do Active Directory que estão associados a um objeto de computador específico.  
  
-   **Atributos de recursos** Propriedades de recursos global que são marcadas para uso em decisões de autorização e publicadas no Active Directory.  
  
As declarações tornam possível para os administradores fazerem demonstrações precisas em toda a organização ou empresa sobre os usuários, dispositivos e recursos que podem ser incorporados em expressões, regras e políticas.  
  
### <a name="BKMK_Expressions2"></a>Expressões  
As expressões condicionais são uma melhoria no gerenciamento de controle de acesso que concede ou nega o acesso a recursos somente quando determinadas condições são atendidas, por exemplo, associação a grupo, local ou estado de segurança do dispositivo. As expressões são gerenciadas por meio da caixa de diálogo Configurações de Segurança Avançadas do Editor ACL ou do Editor de Regras de Acesso Central no ADAC (Centro Administrativo do Active Directory).  
  
As expressões ajudam os administradores a gerenciar o acesso a recursos confidenciais com condições flexíveis em ambientes de negócios cada vez mais complexos.  
  
### <a name="BKMK_Permissions2"></a>Permissões propostas  
As permissões propostas permitem que um administrador modele com mais precisão o impacto de possíveis mudanças nas configurações de controle de acesso sem realmente alterá-las.  
  
Prever o acesso efetivo a um recurso ajuda você a planejar e configurar permissões para esses recursos antes de implementar essas alterações.  
  
## <a name="additional-changes"></a>Alterações adicionais  
Outras melhorias nas versões do Windows com suporte que dão suporte ao Controle de Acesso Dinâmico incluem:  
  
### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>Suporte ao protocolo de autenticação Kerberos para fornecer de forma confiável declarações de usuário, declarações de dispositivo e grupos de dispositivos.  
Por padrão, os dispositivos que executam qualquer uma das versões do Windows com suporte são capazes de processar os tíquetes Kerberos relacionados ao Controle de Acesso Dinâmico, que incluem os dados necessários para a autenticação composta. Os controladores de domínio são capazes de emitir e responder aos tíquetes Kerberos com informações relacionadas à autenticação composta. Quando um domínio é configurado para reconhecer o Controle de Acesso Dinâmico, os dispositivos recebem declarações dos controladores de domínio durante a autenticação inicial e eles recebem tíquetes de autenticação composta quando enviam solicitações de tíquete de serviços. A autenticação composta resulta em um token de acesso que inclui a identidade do usuário e o dispositivo nos recursos que reconhece o Controle de Acesso Dinâmico.  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Suporte para usar a configuração de Política de Grupo do KDC (Centro de Distribuição de Chaves) para habilitar o Controle de Acesso Dinâmico em um domínio.  
Todo controlador de domínio precisa ter a mesma configuração de política de Modelo Administrativo, que está localizada em **Configuração do Computador\Políticas\Modelos Administrativos\Sistema\KDC\Suporte ao Controle de Acesso Dinâmico e à proteção Kerberos**.  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Suporte para usar a configuração de Política de Grupo do KDC (Centro de Distribuição de Chaves) para habilitar o Controle de Acesso Dinâmico em um domínio.  
Todo controlador de domínio precisa ter a mesma configuração de política de Modelo Administrativo, que está localizada em **Configuração do Computador\Políticas\Modelos Administrativos\Sistema\KDC\Suporte ao Controle de Acesso Dinâmico e à proteção Kerberos**.  
  
### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>Suporte no Active Directory para armazenar as declarações de usuários e dispositivos, as propriedades de recursos e os objetos de política de acesso central.  
  
### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>Suporte para usar a Política de Grupo para implantar os objetos de política de acesso central.  
A configuração da Política de Grupo a seguir permite implantar objetos de política de acesso central em servidores de arquivos de sua organização: **Configuração do Computador\Políticas\Configurações do Windows\Configurações de Segurança\Sistema de Arquivos\Política de Acesso Central**.  
  
### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>Suporte à autorização de arquivo baseada em declarações e à auditoria de sistemas de arquivos usando a Política de Grupo e a Auditoria de Acesso a Objeto Global  
Você deve habilitar a auditoria de políticas de acesso central de preparo para fazer auditoria do acesso efetivo da política de acesso central usando as permissões propostas. Defina essa configuração para o computador em **Configuração Avançada de Política de Auditoria** nas **Configurações de Segurança** de um objeto de Política de Grupo (GPO). Depois de definir a configuração de segurança no GPO, você poderá implantar o GPO nos computadores em sua rede.  
  
### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>Suporte para transformar ou filtrar objetos de política de declaração que atravessam as relações de confiança de floresta do Active Directory  
Você pode filtrar ou transformar as declarações de entrada e saída que atravessam uma relação de confiança da floresta. Há três cenários básicos para filtragem e transformação de declarações:  
  
-   **Filtragem baseada em valor** Os filtros podem ser baseados no valor de uma declaração. Isso permite que a floresta confiável impeça o envio de declarações com determinados valores para a floresta confiante. Os controladores de domínio em florestas confiantes usam a filtragem baseada em valor para proteger contra um ataque de elevação de privilégio filtrando as declarações de entrada com valores específicos da floresta confiável.  
  
-   **Filtragem baseada no tipo de declaração** Filtros são baseados no tipo de declaração, em vez do valor da declaração. Você pode identificar o tipo de declaração pelo nome da declaração. Você pode usar a filtragem baseada no tipo de declaração na floresta confiável e impedir que o Windows envie declarações que divulguem informações para a floresta confiável.  
  
-   **Transformação baseada no tipo de declaração** Manipula uma declaração antes de enviá-la para o destino pretendido. Use a transformação baseada em tipo de declaração na floresta confiável para generalizar uma declaração conhecida que contém informações específicas. Você pode usar transformações para generalizar o tipo de declaração, o valor da declaração ou os dois.  
  
## <a name="software-requirements"></a>Requisitos de software  
Como as declarações e a autenticação composta para o Controle de Acesso Dinâmico exigem extensões de autenticação Kerberos, qualquer domínio que der suporte ao Controle de Acesso Dinâmico deverá ter controladores de domínio suficientes executando as versões do Windows com suporte para dar suporte à autenticação dos clientes Kerberos com reconhecimento de Controle de Acesso Dinâmico. Por padrão, os dispositivos devem usar controladores de domínio em outros sites. Se nenhum desses controladores de domínio estiver disponível, a autenticação falhará. Com isso, ofereça suporte a uma das seguintes condições:  
  
-   Cada domínio que oferece suporte ao controle de acesso dinâmico deve ter controladores de domínio suficientes executando as versões do Windows Server com suporte para oferecer suporte à autenticação de todos os dispositivos que executam as versões do Windows ou Windows Server com suporte.  
  
-   Os dispositivos que executam as versões do Windows com suporte ou que não protegem os recursos usando declarações ou identidade composta devem desabilitar o suporte ao protocolo Kerberos para o Controle de Acesso Dinâmico.  
  
Para domínios que oferecem suporte a declarações de usuário, cada controlador de domínio que executa as versões do Windows com suporte deve ser definido com a configuração apropriada para dar suporte a declarações, autenticação composta e fornecer proteção Kerberos. Defina as configurações na política de Modelo Administrativo do KDC da seguinte maneira:  
  
-   **Sempre fornecer declarações** Use esta configuração se todos os controladores de domínio estiverem executando as versões do Windows Server com suporte. Além disso, defina o nível funcional do domínio para Windows Server 2012 ou superior.  
  
-   **Com Suporte** Quando você usa esta configuração, monitore os controladores de domínio para garantir que o número de controladores de domínio que executa as versões do Windows Server com suporte é suficiente para o número de computadores clientes que precisam acessar os recursos protegidos pelo Controle de Acesso Dinâmico.  
  
Se o domínio de usuário e o domínio do servidor de arquivos estiverem em florestas diferentes, todos os controladores de domínio na raiz da floresta do servidor de arquivos deverão ser definidos no nível funcional do Windows Server 2012 ou superior.  
  
Se os clientes não reconhecerem o Controle de Acesso Dinâmico, deverá haver uma relação de confiança bidirecional entre duas florestas.  
  
Se as declarações forem transformadas quando saírem de uma floresta, todos os controladores de domínio na raiz da floresta do usuário deverão ser definidos no nível funcional do Windows Server 2012 ou superior.  
  
Um servidor de arquivos que executa o Windows Server 2012 ou Windows Server 2012 R2 deve ter uma configuração de Política de Grupo que especifica se é preciso obter declarações de usuário para tokens de usuário que não carregam declarações. Esta configuração é definida por padrão como **Automático**, o que resulta na **Habilitação** da configuração da Política de Grupo quando há uma política central que contém as declarações de usuário ou dispositivo para esse servidor de arquivos. Se o servidor de arquivos tiver ACLs discricionárias que incluem declarações de usuário, você precisará definir essa Política de Grupo como **Habilitada** para que o servidor saiba solicitar declarações em nome dos usuários que não as fornecem quando acessam o servidor.  
  
## <a name="additional-resource"></a>Recurso adicional  
Para obter informações sobre como implementar soluções baseadas nessa tecnologia, consulte [controle de acesso dinâmico: visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md).  
  


