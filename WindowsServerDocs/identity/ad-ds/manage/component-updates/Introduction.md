---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: "Introdução"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dc89afc47eb78a388238e8edf5059b0bec3006ad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="introduction"></a>Introdução

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ataques contra computação infraestruturas, se simples ou complexas, existem desde que tenham computadores. No entanto, em última década, números de cada vez maiores de organizações de todos os tamanhos, em todas as partes do mundo foram atacados e comprometidos de maneiras que mudaram significativamente o cenário de ameaças. Warfare cibernéticos e CIBERCRIME aumentaram taxas de registro. "Hacktivism", na qual ataques são motivados pela activist posições, foi declarado conforme o esperado a motivação para um número de violações de expor informações secretas das organizações, para criar a negação de serviço, ou até mesmo para destruir a infraestrutura. Ataques contra instituições públicas e privadas com o objetivo de exfiltrating tenham sido onipresentes direitos de propriedade intelectual das organizações (IP).  
  
Nenhuma organização com uma infraestrutura de informática informações é imune contra ataques, mas se os controles, processos e políticas apropriadas estão implementados para proteger chaves segmentos de infraestrutura de computação da organização, escalonamento de ataques de penetração para comprometer completa pode ser evitáveis. Como o número e a escala de ataques proveniente de fora de uma organização tem ocultaram ameaça insider nos últimos anos, este documento discute geralmente invasores externos, em vez de uso indevido do ambiente por usuários autorizados. No entanto, os princípios e as recomendações fornecidas neste documento destinam-se para ajudar a proteger seu ambiente contra invasores externos e participantes mal orientados ou mal-intencionado.  
  
As informações e recomendações fornecidas neste documento são desenhadas de um número de fontes e derivadas práticas voltadas para proteger instalações do Active Directory contra comprometimento. Embora não seja possível evitar ataques, é possível reduzir a superfície de ataque do Active Directory e implementar os controles que tornam comprometimento do diretório muito mais difícil para invasores. Este documento apresenta os tipos mais comuns das vulnerabilidades que temos observado em ambientes comprometidos e as recomendações mais comuns que fizemos para os clientes para melhorar a segurança de suas instalações do Active Directory.  
  
## <a name="account-and-group-naming-conventions"></a>Convenções de nomeação de grupo e conta  
A tabela a seguir fornece um guia para as convenções de nomenclatura usada neste documento para os grupos e contas referenciadas em todo o documento. Incluído na tabela é o local de cada conta/grupo, seu nome e como essas contas/grupos são mencionados neste documento.  
  


|**Conta/grupo local**|**Nome da conta/grupo**|**Como é referenciado neste documento**|
| --- | --- | --- |   
|Active Directory - cada domínio|Administrador|Conta de administrador interno|  
|Active Directory - cada domínio|Administradores|Grupo de administradores (BA) interno|  
|Active Directory - cada domínio|Administradores de domínio|Grupo de administradores (DA) do domínio|  
|Active Directory - domínio raiz da floresta|Administradores corporativos|Grupo de administradores de empresa (EA)|  
|Banco de dados do computador local security contas manager (SAM) em computadores que executam o Windows Server e estações de trabalho que não são controladores de domínio|Administrador|Conta de administrador local|  
|Banco de dados do computador local security contas manager (SAM) em computadores que executam o Windows Server e estações de trabalho que não são controladores de domínio|Administradores|Grupo local Administradores|  
  
## <a name="about-this-document"></a>Sobre este documento  
A Microsoft informações de segurança e a organização de gerenciamento de risco (ISRM), que faz parte do Microsoft informações tecnologia (MSIT), funciona com unidades de negócios internos, os clientes externos e pares do setor para coletar, disseminem e definem políticas, práticas recomendadas e controles. Essas informações podem ser usadas pela Microsoft e nossos clientes para aumentar a segurança e reduzir a superfície de ataque de suas infraestruturas IT. As recomendações fornecidas neste documento se baseiam em um número de fontes de informações e práticas usadas dentro de MSIT e ISRM. As seções a seguir apresentam mais informações sobre as origens deste documento.  
  
### <a name="microsoft-it-and-isrm"></a>TI da Microsoft e ISRM  
Um número de controles e práticas recomendadas foram desenvolvido dentro MSIT e ISRM para proteger as florestas do Microsoft AD DS e domínios. Quando esses controles são amplamente aplicáveis, eles foram integrados neste documento. SAFE-T (aceleradores tecnologias emergentes) é uma equipe dentro ISRM cuja função é identificar novas tecnologias e para definir os requisitos de segurança e controles para agilizar a adoção.  
  
### <a name="active-directory-security-assessments"></a>Avaliações de segurança do Active Directory  
No Microsoft ISRM, a avaliação, consultoria e equipe de engenharia (ACE) funciona com unidades de negócios internas da Microsoft e os clientes externos para avaliar o aplicativo e da segurança da infraestrutura e fornecer orientação tática e estratégica para aumentar a postura de segurança da organização. Uma oferta de serviço ACE é o Active Directory segurança avaliação (ADSA), que é uma avaliação holística de ambiente AD DS de uma organização que avalia a pessoas, processos e tecnologia e produz recomendações específicas do cliente. Os clientes são fornecidos com recomendações com base nas características exclusivas da organização, práticas e apetite de risco. ADSAs foram executadas para instalações do Active Directory na Microsoft além daqueles de nossos clientes. Ao longo do tempo, um número de recomendações foram encontrado são aplicáveis em todos os clientes de tamanhos e setores.  
  
### <a name="content-origin-and-organization"></a>Organização e a origem do conteúdo  
Boa parte do conteúdo deste documento é derivada do ADSA e outras avaliações de equipe ACE executadas para clientes comprometidos e que não encontraram comprometimento significativo. Embora os dados do cliente individuais não foi usados para criar este documento, as vulnerabilidades mais comumente exploradas identificamos que coletamos em nossas avaliações e as recomendações que fizemos para os clientes para melhorar a segurança de suas instalações do AD DS. Nem todas as vulnerabilidades são aplicáveis a todos os ambientes, nem são todas as recomendações viável para implementar em cada organização.  
  
Este documento é organizado da seguinte maneira:  
  
## <a name="executive-summary"></a>Resumo executivo  
O executivo resumo, que podem ser lidas como um documento autônomo ou em combinação com o documento completa, fornece um resumo de alto nível deste documento. Incluído no resumo executivo são os vetores de ataque mais comuns que observamos usados para comprometer ambientes de clientes, resumidas recomendações para proteger instalações do Active Directory e os objetivos básicos para clientes que planejam implantar novas florestas do AD DS agora ou no futuro.  
  
### <a name="introduction"></a>Introdução  
Esta é a seção que você está lendo agora.  
  
### <a name="avenues-to-compromise"></a>Caminhos para comprometer  
Esta seção fornece informações sobre alguns dos mais comumente utilizados vulnerabilidades descobrimos a ser usada por invasores para comprometer infraestruturas dos clientes. Esta seção começa com categorias gerais de vulnerabilidades e como eles são utilizados para inicialmente penetrar infraestruturas dos clientes, propagar comprometimento em sistemas adicionais e, eventualmente, direcionar o AD DS e controladores de domínio para obter controle completo de florestas das organizações.  
  
Esta seção não fornece recomendações detalhadas sobre como lidar com cada tipo de vulnerabilidade, principalmente nas áreas em que as vulnerabilidades não são usadas para direcionar diretamente do Active Directory. No entanto, para cada tipo de vulnerabilidade, fornecemos links para informações adicionais que você pode usar para desenvolver contramedidas e reduzir a superfície de ataque da sua organização.  
  
### <a name="reducing-the-active-directory-attack-surface"></a>Reduzir a superfície de ataque do Active Directory  
Esta seção começa, fornecendo informações básicas sobre contas privilegiadas e grupos no Active Directory para fornecer as informações que ajuda a esclarecer os motivos para as recomendações para proteger e gerenciamento de contas e grupos privilegiados subsequentes. Em seguida, vamos discutir abordagens para reduzir a necessidade de usar contas altamente privilegiadas para administração diária, que não requer o nível de privilégio é concedido a grupos como os grupos de administradores de empresa (EA), os administradores de domínio (DA) e os administradores interno (BA) no Active Directory. Em seguida, fornecemos diretrizes para proteger as contas e grupos privilegiados e para a implementação de sistemas e práticas administrativas seguras.  
  
Embora esta seção fornece informações detalhadas sobre essas configurações, também incluímos apêndices para cada recomendação que fornecem instruções passo a passo de configuração que podem ser usados "como está" ou podem ser modificadas para as necessidades da organização. Esta seção é concluído, fornecendo informações para implantar e gerenciar controladores de domínio, o que devem ser entre os sistemas seguros mais severamente na infraestrutura de segurança.  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>O monitoramento do Active Directory de sinais de comprometimento  
Se você implementou monitoração (SIEM) em seu ambiente de eventos e informações de segurança robusta ou estiver usando outros mecanismos para monitorar a segurança da infraestrutura, esta seção fornece informações que podem ser usadas para identificar eventos em sistemas do Windows que podem indicar que uma organização estiver sendo atacada. Vamos discutir políticas de auditoria avançada e tradicionais, incluindo configuração efetiva de subcategorias de auditoria nos sistemas operacionais Windows 7 e Windows Vista. Esta seção inclui as listas abrangentes de objetos e sistemas de auditoria, e um apêndice associado lista eventos para os quais você deve monitorar se o objetivo é detectar tentativas de comprometer.  
  
### <a name="planning-for-compromise"></a>Planejamento de comprometimento  
Esta seção começa passando"Voltar" do detalhes técnicos para focalizar princípios e processos que podem ser implementados para identificar os usuários, aplicativos e sistemas que são mais importantes não apenas para a infraestrutura de TI, mas para os negócios. Depois de identificar o que é mais crítico para a estabilidade e operações da sua organização, você pode focalizar separar e proteger esses ativos, se eles são de propriedade intelectual, pessoas ou sistemas. Em alguns casos, podem ser realizadas separar e proteger os ativos em seu ambiente AD DS existente, enquanto em outros casos, convém implementar pequenas, separadas "células" que permitem que você estabeleça um limite de seguro em torno de ativos críticos e monitorar esses ativos de forma mais rigorosa do que componentes menos críticos. Um conceito chamado "destruição criativa", que é um mecanismo pelo qual sistemas e aplicativos herdados podem ser eliminados através da criação de novas soluções é discutido e a seção termina com recomendações que podem ajudar a manter um ambiente mais seguro, combinando negócios e informações de TI para construir uma imagem detalhada do que é um estado de operação normal. Sabendo o que é normal para uma organização, anormalidades que possam indicar ataques e comprometimentos podem ser identificadas mais facilmente.  
  
### <a name="summary-of-best-practice-recommendations"></a>Resumo das práticas recomendadas  
Esta seção fornece uma tabela que resume as recomendações feitas neste documento e pedidos-los por prioridade relativa, além de fornecer links para onde obter mais informações sobre cada recomendação podem ser encontradas no documento e seus apêndices.  
  
### <a name="appendices"></a>Apêndices  
Apêndices estão incluídos neste documento para aumentar as informações contidas no corpo do documento. A lista de apêndices e uma breve descrição de cada um está incluída a tabela a seguir.  
  
 
|**Apêndice**|**Descrição**|
| --- | --- | 
|[Apêndice b: privilegiadas contas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Fornece informações de plano de fundo que ajudam a identificar os usuários e grupos, que você deve se concentrar na proteção porque eles podem ser aproveitados por invasores comprometer e até mesmo destruir a instalação do Active Directory.|  
|[Apêndice c: protegidas contas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contém informações sobre grupos protegidos no Active Directory. Ele também contém informações para a personalização limitada (remoção) de grupos que são consideradas grupos protegidos e são afetados por AdminSDHolder e SDProp.|  
|[Apêndice d: proteção de contas de administrador interno no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger a conta de administrador em cada domínio na floresta.|  
|[Apêndice e: Protegendo grupos de administradores de empresa no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger o grupo Administradores corporativos na floresta.|  
|[Apêndice f: Protegendo grupos de administradores de domínio no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger o grupo Admins. do domínio em cada domínio na floresta.|  
|[Apêndice g: Protegendo grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger o grupo Administradores interno em cada domínio na floresta.|  
|[Apêndice h: Protegendo grupos e contas de Administrador Local](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contém diretrizes para ajudar a contas de administrador locais seguras e grupos de administradores em servidores de domínio e estações de trabalho.|  
|[Apêndice i: criação de gerenciamento de contas para protegido contas e grupos no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Fornece informações para criar contas que têm privilégios limitados e podem ser controladas severamente, mas podem ser usadas para popular grupos privilegiados no Active Directory quando temporária elevação é necessária.|  
|[Apêndice l: eventos de Monitor](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Lista os eventos para os quais você deve monitorar em seu ambiente.|  
|[Apêndice m: Links do documento e leitura recomendada](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contém uma lista de leitura recomendada. Também contém uma lista de links para documentos externos e suas URLs para que os leitores de impressões deste documento podem acessar essas informações.|  
  


