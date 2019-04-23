---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: Introdução
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e2717af6183944b26a71e55b36f31cef51cf2e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831867"
---
# <a name="introduction"></a>Introdução

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ataques contra infraestruturas de computação, simples ou complexos, existem desde que tenham de computadores. No entanto, na última década, números crescentes de empresas de todos os tamanhos, em todas as partes do mundo, são atacados e comprometidos de maneiras que alteram significativamente o panorama de ameaças. A guerra cibernética e o crime cibernético aumentaram a taxas recordes. O "Hacktivismo", em que os ataques são motivados por posições ativistas, foi declarado como se destina a motivação para um número de violações para expor informações secretas das organizações, para criar negações de serviço, ou mesmo destruir a infraestrutura. Ataques contra instituições públicas e privadas com o objetivo de se apropriar (IP) de propriedade intelectual das organizações têm se tornado onipresentes.  
  
Nenhuma organização com uma infra-estrutura de TI (tecnologia) de informações é imune a ataques, mas se as políticas adequadas, processos e controles são implementados para proteger segmentos chave da infraestrutura de computação da organização, de ataques de escalonamento de bloqueios a penetração ao comprometimento de concluir, pode ser impedida. Porque o número e a escala de ataques provenientes de fora de uma organização tem eclipsada ameaça interna nos últimos anos, este documento aborda muitas vezes invasores externos em vez de uso indevido do ambiente por usuários autorizados. No entanto, os princípios e as recomendações fornecidas neste documento destinam-se para ajudar a proteger seu ambiente contra invasores externos e internos extraviados mal-intencionado ou maliciosos.  
  
As informações e as recomendações fornecidas neste documento são extraídas de várias fontes e derivam das práticas projetadas para proteger instalações do Active Directory contra o comprometimento. Embora não seja possível evitar ataques, é possível reduzir a superfície de ataque do Active Directory e implementar controles que tornam o comprometimento do diretório muito mais difícil para invasores. Este documento apresenta os tipos mais comuns de vulnerabilidades que temos observado em ambientes comprometidos e as recomendações mais comuns, fizemos a clientes para melhorar a segurança de suas instalações do Active Directory.  
  
## <a name="account-and-group-naming-conventions"></a>Convenções de nomeação de grupo e conta  
A tabela a seguir fornece um guia para as convenções de nomenclatura usadas neste documento para os grupos e contas referenciadas em todo o documento. O local de cada conta/grupo, seu nome e como essas contas/grupos são referenciados neste documento está incluído na tabela.  
  


|**Conta/grupo local**|**Nome da conta/grupo**|**Como ele é referenciado neste documento**|
| --- | --- | --- |   
|Active Directory - cada domínio|Administrador|Conta de administrador interno|  
|Active Directory - cada domínio|Administradores|Grupo de administradores (BA) interno|  
|Active Directory - cada domínio|Administradores do domínio|Grupo de administradores (DA) de domínio|  
|Active Directory - domínio raiz da floresta|Administrador corporativo|Grupo de administradores Enterprise (EA)|  
|Banco de dados do computador local segurança contas SAM (gerente) em computadores que executam o Windows Server e estações de trabalho que não são controladores de domínio|Administrador|Conta de administrador local|  
|Banco de dados do computador local segurança contas SAM (gerente) em computadores que executam o Windows Server e estações de trabalho que não são controladores de domínio|Administradores|Grupo de administradores locais|  
  
## <a name="about-this-document"></a>Sobre este documento  
A organização de segurança da informação e gerenciamento de risco (ISRM), que faz parte da tecnologia de informações da Microsoft (MSIT), funciona com unidades de negócios internos, os clientes externos e colegas do setor para reunir, disseminar e definir políticas, práticas recomendadas e controles. Essas informações podem ser usadas pela Microsoft e nossos clientes para aumentar a segurança e reduzir a superfície de ataque de suas infra-estruturas de TI. As recomendações fornecidas neste documento baseiam-se em um número de fontes de informações e práticas usadas dentro do MSIT e ISRM. As seções a seguir apresentam mais informações sobre as origens deste documento.  
  
### <a name="microsoft-it-and-isrm"></a>A TI da Microsoft e ISRM  
Um número de práticas e controles foram desenvolvido em MSIT e ISRM para proteger os domínios e florestas do Microsoft AD DS. Quando esses controles são amplamente aplicáveis, eles foram integrados neste documento. SAFE-T (Solution Accelerators para tecnologias emergentes) é uma equipe dentro ISRM cuja função é identificar as tecnologias emergentes e para definir os requisitos de segurança e controles para acelerar a adoção.  
  
### <a name="active-directory-security-assessments"></a>Avaliações de segurança do Active Directory  
Dentro do Microsoft ISRM, a avaliação, consultoria e equipe Engineering (ACE) funciona com unidades de negócios internas da Microsoft e os clientes externos para avaliar a segurança de aplicativo e a infraestrutura e fornecer orientações táticas e estratégicas para aumentar o postura de segurança da organização. Uma oferta de serviço ACE é o Active Directory Security avaliação (ADSA), que é uma avaliação holística do ambiente da organização do AD DS que avalia a pessoas, processos e tecnologia e produz recomendações específicas do cliente. Os clientes recebem recomendações com base em características exclusivas da organização, práticas e exposição ao risco. ADSAs foram executadas para instalações do Active Directory na Microsoft, além daqueles de nossos clientes. Ao longo do tempo, uma série de recomendações foram encontrada ser aplicáveis a clientes de diferentes tamanhos e setores.  
  
### <a name="content-origin-and-organization"></a>Origem e organização do conteúdo  
Grande parte do conteúdo deste documento é derivado do ADSA e outras avaliações da equipe ACE executadas para clientes comprometidos e os clientes que não sofreram comprometimento significativo. Embora os dados do cliente individual não foi usados para criar este documento, coletamos as vulnerabilidades mais comumente exploradas identificamos nossas avaliações e as recomendações, fizemos a clientes para melhorar a segurança do seu AD DS instalações. Nem todas as vulnerabilidades são aplicáveis a todos os ambientes, e nem todas as recomendações são viáveis para implementação em todas as organizações.  
  
Este documento está organizado da seguinte maneira:  
  
## <a name="executive-summary"></a>Síntese  
O executivo resumo, que pode ser lido como um documento autônomo ou em combinação com o documento completo, fornece um resumo de alto nível deste documento. Incluídas no resumo executivo são os vetores de ataque mais comuns que observamos usados para comprometer ambientes de clientes, resumidas recomendações para proteger instalações do Active Directory e os objetivos básicos para os clientes que planejam implantar o AD DS novo florestas agora ou no futuro.  
  
### <a name="introduction"></a>Introdução  
Esta é a seção que você está lendo agora.  
  
### <a name="avenues-to-compromise"></a>Alternativas ao comprometimento  
Esta seção fornece informações sobre alguns dos mais comumente utilizados vulnerabilidades que encontramos a ser usado pelos invasores para comprometer as infraestruturas de clientes. Esta seção começa com categorias gerais de vulnerabilidades e como eles serão utilizados para penetrar em infra-estruturas de clientes, propagar comprometimento entre sistemas adicionais e, eventualmente, AD DS e controladores de domínio para obter completa de destino controle de florestas das organizações.  
  
Esta seção fornece recomendações detalhadas sobre como tratar cada tipo de vulnerabilidade, especialmente nas áreas em que as vulnerabilidades não são usadas para destinar diretamente o Active Directory. No entanto, para cada tipo de vulnerabilidade, fornecemos links para informações adicionais que você pode usar para desenvolver contramedidas e reduzir a superfície de ataque da sua organização.  
  
### <a name="reducing-the-active-directory-attack-surface"></a>Redução da superfície de ataque do Active Directory  
Essa seção inicia, fornecendo informações básicas sobre contas privilegiadas e grupos no Active Directory para fornecer as informações que ajuda a esclarecer os motivos para as recomendações subsequentes para proteger e gerenciar grupos privilegiados e contas. Em seguida, discutiremos abordagens para reduzir a necessidade de usar contas com altos privilégios para administração diária, que não exige que o nível de privilégio concedido aos grupos como Admins. do EA (Enterprise), os administradores de domínio (DA) e interno Grupos de administradores (BA) no Active Directory. Em seguida, fornecemos diretrizes para proteger as contas e grupos privilegiados e para a implementação de sistemas e práticas administrativas seguras.  
  
Embora esta seção fornece informações detalhadas sobre essas definições de configuração, também incluímos os apêndices para cada recomendação que fornecem instruções passo a passo de configuração que podem ser usado "como está", ou podem ser modificadas para o necessidades da organização. Esta seção termina, fornecendo informações para implantar e gerenciar controladores de domínio, que devem ser entre os sistemas de forma mais rigorosa protegidos na infraestrutura com segurança.  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitorar o Active Directory em busca de sinais de comprometimento  
Se você tiver implementado o monitoramento (SIEM) em seu ambiente de eventos e informações de segurança robusta ou estiver usando outros mecanismos para monitorar a segurança da infraestrutura, esta seção fornece informações que podem ser usadas para identificar os eventos no Windows sistemas que podem indicar que uma organização estiver sendo atacada. Discutiremos políticas de auditoria tradicionais e avançadas, incluindo configuração efetiva de subcategorias de auditoria nos sistemas operacionais Windows 7 e Windows Vista. Esta seção inclui listas abrangentes de objetos e sistemas de audit e um apêndice associado lista eventos para os quais você deve monitorar se o objetivo é detectar tentativas de comprometimento.  
  
### <a name="planning-for-compromise"></a>Planejar para comprometimento  
Esta seção começa com "recuando" de detalhes técnicos para se concentrar em princípios e processos que podem ser implementados para identificar os usuários, aplicativos e sistemas que são mais críticos não apenas para a infraestrutura de TI, mas para os negócios. Depois de identificar o que é mais importante para a estabilidade e operações da sua organização, você pode se concentrar em separar e proteger esses ativos, independentemente de estarem sistemas, pessoas ou propriedade intelectual. Em alguns casos, isolar e proteger ativos podem ser executadas em seu ambiente existente do AD DS, enquanto em outros casos, você deve considerar implementar pequenas e separadas "células" que permitem que você estabeleça um limite seguro em torno de ativos críticos e monitorá-los ativos de forma mais rigorosa do que os componentes menos críticos. Um conceito chamado "destruição creative", que é um mecanismo pelo qual os sistemas e aplicativos herdados podem ser eliminados, criando novas soluções é discutido, e a seção termina com as recomendações que podem ajudar a manter um ambiente mais seguro por Combinando comercial e informações de TI para construir uma imagem detalhada do que é um estado operacional normal. Sabendo o que é normal para uma organização, que podem indicar ataques e comprometimentos anormalidades podem ser identificadas com mais facilidade.  
  
### <a name="summary-of-best-practice-recommendations"></a>Resumo das recomendações de práticas recomendadas  
Esta seção fornece uma tabela que resume as recomendações feitas neste documento e as classifica por prioridade relativa, além de fornecer links para onde obter mais informações sobre cada recomendação podem ser encontradas no documento e seus anexos.  
  
### <a name="appendices"></a>Apêndices  
Apêndices estão incluídos neste documento para aumentar as informações contidas no corpo do documento. A lista de apêndices e uma breve descrição de cada um é incluída a tabela a seguir.  
  
 
|**Apêndice**|**Descrição**|
| --- | --- | 
|[Apêndice b: Contas privilegiadas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Fornece informações de plano de fundo que ajudam a identificar os usuários e grupos, que você deve se concentrar em proteger porque eles podem ser aproveitados por invasores de comprometer e até mesmo destruir a instalação do Active Directory.|  
|[Apêndice C: Contas protegidas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contém informações sobre grupos protegidos no Active Directory. Ele também contém informações de personalização limitada (remoção) de grupos que são considerados grupos protegidos e são afetados por AdminSDHolder e SDProp.|  
|[Apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger a conta de administrador em cada domínio na floresta.|  
|[Apêndice e: Protegendo grupos de administrador corporativo no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger o grupo de administradores corporativos na floresta.|  
|[Apêndice f: Protegendo grupos de administradores de domínio do Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger o grupo Admins. do domínio em cada domínio na floresta.|  
|[Apêndice g: Protegendo grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger o grupo de administradores interno em cada domínio na floresta.|  
|[Apêndice h: Protegendo grupos e contas de Administrador Local](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contém diretrizes para ajudar a proteger contas de administrador locais e grupos de administradores nos servidores de domínio e estações de trabalho.|  
|[Apêndice i: Criando o gerenciamento de contas para contas protegidas e grupos no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Fornece informações para criar as contas que têm privilégios limitados e podem ser controladas de forma rigorosa, mas podem ser usadas para preencher grupos privilegiados no Active Directory quando a elevação temporária é necessária.|  
|[Apêndice l: Eventos a serem monitorados](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Lista de eventos para os quais você deve monitorar em seu ambiente.|  
|[Apêndice m: Links de documentos e leitura recomendada](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contém uma lista de leitura recomendada. Também contém uma lista de links para documentos externos e suas URLs para que os leitores de cópias impressas deste documento possam acessar essas informações.|  
  


