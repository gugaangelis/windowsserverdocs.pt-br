---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: Introdução
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ecfede36e855ed942d5d3f47be6dbe6bc4520a53
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941286"
---
# <a name="introduction"></a>Introdução

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ataques contra infraestruturas de computação, sejam simples ou complexos, têm existido desde que os computadores tenham. No entanto, na última década, números crescentes de empresas de todos os tamanhos, em todas as partes do mundo, são atacados e comprometidos de maneiras que alteram significativamente o panorama de ameaças. A guerra cibernética e o crime cibernético aumentaram a taxas recordes. "Hacktivism", em que os ataques são motivados por activist posições, foi reivindicada como a motivação de várias violações destinadas a expor as informações secretas das organizações, criar negações de serviço ou até mesmo destruir a infraestrutura. Ataques contra instituições públicas e privadas com o objetivo de invasãor a propriedade intelectual das organizações (IP) se tornaram onipresentes.

Nenhuma organização com uma infra-estrutura de ti (tecnologia da informação) está imune contra ataques, mas se as políticas, os processos e os controles apropriados forem implementados para proteger os principais segmentos da infraestrutura de computação de uma organização, o escalonamento de ataques contra a penetração para concluir o comprometimento pode ser impedida. Como o número e a escala de ataques provenientes de fora de uma organização têm ameaça de insider no ano recente, este documento geralmente aborda invasores externos, em vez de uso indevido do ambiente por usuários autorizados. No entanto, os princípios e as recomendações fornecidos neste documento destinam-se a ajudar a proteger seu ambiente contra invasores externos e informadores mal intencionados ou mal-intencionados.

As informações e recomendações fornecidas neste documento são desenhadas de várias fontes e derivadas de práticas criadas para proteger Active Directory instalações contra o comprometimento. Embora não seja possível evitar ataques, é possível reduzir a superfície de ataque Active Directory e implementar controles que tornam o comprometimento do diretório muito mais difícil para os invasores. Este documento apresenta os tipos mais comuns de vulnerabilidades que observamos em ambientes comprometidos e as recomendações mais comuns que fizemos aos clientes para melhorar a segurança de suas instalações de Active Directory.

## <a name="account-and-group-naming-conventions"></a>Convenções de nomenclatura de contas e grupos
A tabela a seguir fornece um guia para as convenções de nomenclatura usadas neste documento para os grupos e contas referenciados em todo o documento. Incluído na tabela está o local de cada conta/grupo, seu nome e como essas contas/grupos são referenciados neste documento.



|**Local da conta/grupo**|**Nome da conta/grupo**|**Como ele é referenciado neste documento**|
| --- | --- | --- |
|Active Directory-cada domínio|Administrador|Conta de administrador interna|
|Active Directory-cada domínio|Administradores|Grupo de administradores internos (BA)|
|Active Directory-cada domínio|Administradores de Domínio|Grupo de administradores de domínio (DA)|
|Active Directory-domínio raiz da floresta|Administradores Corporativos|Grupo de administração de empresa (EA)|
|Banco de dados SAM (Gerenciador de contas de segurança) do computador local em computadores que executam o Windows Server e estações de trabalho que não são controladores de domínio|Administrador|Conta de administrador local|
|Banco de dados SAM (Gerenciador de contas de segurança) do computador local em computadores que executam o Windows Server e estações de trabalho que não são controladores de domínio|Administradores|Grupo de administradores locais|

## <a name="about-this-document"></a>Sobre este documento
A organização do Microsoft Information Security and Risk Management (ISRM), que faz parte da tecnologia de informações da Microsoft (MSIT), funciona com unidades de negócios internas, clientes externos e colegas do setor para reunir, disseminar e definir políticas, práticas e controles. Essas informações podem ser usadas pela Microsoft e pelos nossos clientes para aumentar a segurança e reduzir a superfície de ataque de suas infraestruturas de ti. As recomendações fornecidas neste documento baseiam-se em várias fontes de informações e práticas usadas em MSIT e ISRM. As seções a seguir apresentam mais informações sobre as origens deste documento.

### <a name="microsoft-it-and-isrm"></a>TI e ISRM da Microsoft
Várias práticas e controles foram desenvolvidos em MSIT e ISRM para proteger as florestas e domínios do Microsoft AD DS. Onde esses controles são amplamente aplicáveis, eles foram integrados a este documento. SEGURA-T (Solution Accelerators para tecnologias emergentes) é uma equipe dentro de ISRM cujo compromisso é identificar as tecnologias emergentes e definir requisitos e controles de segurança para acelerar sua adoção.

### <a name="active-directory-security-assessments"></a>Active Directory avaliações de segurança
No Microsoft ISRM, a equipe de avaliação, consultoria e engenharia (ACE) trabalha com unidades de negócios da Microsoft internas e clientes externos para avaliar a segurança do aplicativo e da infraestrutura e fornecer orientação tática e estratégica para aumentar a postura de segurança da organização. Uma oferta de serviço ACE é a Active Directory Security Assessment (ADSA), que é uma avaliação holística do ambiente de AD DS de uma organização que avalia pessoas, processos e tecnologias e produz recomendações específicas do cliente. Os clientes são fornecidos com recomendações baseadas nas características, práticas e apetite de risco exclusivos da organização. ADSAs foram executadas para instalações de Active Directory na Microsoft, além das de nossos clientes. Ao longo do tempo, foram encontradas várias recomendações para serem aplicáveis aos clientes de diferentes tamanhos e setores.

### <a name="content-origin-and-organization"></a>Origem e organização do conteúdo
Grande parte do conteúdo deste documento é derivada do ADSA e outras avaliações da equipe do ACE executadas para clientes comprometidos e clientes que não sofreram um comprometimento significativo. Embora os dados individuais do cliente não tenham sido usados para criar este documento, coletamos as vulnerabilidades mais comuns que identificamos em nossas avaliações e as recomendações que fizemos aos clientes para melhorar a segurança de suas instalações de AD DS. Nem todas as vulnerabilidades são aplicáveis a todos os ambientes, e nem todas as recomendações são viáveis para implementação em todas as organizações.

Este documento é organizado da seguinte maneira:

## <a name="executive-summary"></a>Resumo executivo
O resumo executivo, que pode ser lido como um documento autônomo ou em combinação com o documento completo, fornece um resumo de alto nível deste documento. Incluídos no resumo executivo estão os vetores de ataque mais comuns que observamos usados para comprometer ambientes de clientes, recomendações de resumo para proteger Active Directory instalações e objetivos básicos para clientes que planejam implantar novas florestas de AD DS agora ou no futuro.

### <a name="introduction"></a>Introdução
Esta é a seção que você está lendo agora.

### <a name="avenues-to-compromise"></a>Alternativas ao comprometimento
Esta seção fornece informações sobre algumas das vulnerabilidades mais usadas que encontramos para ser usadas pelos invasores para comprometer as infraestruturas dos clientes. Esta seção começa com as categorias gerais de vulnerabilidades e como elas são utilizadas para penetrar inicialmente nas infraestruturas dos clientes, propagar o comprometimento entre sistemas adicionais e, eventualmente, direcionar AD DS e controladores de domínio para obter o controle total das florestas das organizações.

Esta seção não fornece recomendações detalhadas sobre como abordar cada tipo de vulnerabilidade, especialmente nas áreas em que as vulnerabilidades não são usadas para direcionar diretamente Active Directory. No entanto, para cada tipo de vulnerabilidade, fornecemos links para informações adicionais que você pode usar para desenvolver medidas defensivas e reduzir a superfície de ataque da organização.

### <a name="reducing-the-active-directory-attack-surface"></a>Redução da superfície de ataque do Active Directory
Esta seção começa fornecendo informações básicas sobre contas e grupos com privilégios no Active Directory para fornecer as informações que ajudam a esclarecer os motivos das recomendações subsequentes para proteger e gerenciar contas e grupos com privilégios. Em seguida, discutiremos as abordagens para reduzir a necessidade de usar contas altamente privilegiadas para a administração diária, o que não exige o nível de privilégio concedido a grupos como o EA (Enterprise Admins), os administradores de domínio (DA) e os grupos de administradores internos (BA) no Active Directory. Em seguida, fornecemos diretrizes para proteger grupos e contas com privilégios e para implementar sistemas e práticas administrativas seguras.

Embora esta seção forneça informações detalhadas sobre essas definições de configuração, também incluímos apêndices para cada recomendação que fornece instruções de configuração passo a passo que podem ser usadas "no estado em que se encontram" ou podem ser modificadas para as necessidades da organização. Esta seção termina fornecendo informações para implantar e gerenciar controladores de domínio com segurança, que devem estar entre os sistemas de segurança mais rígidos na infraestrutura.

### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitorar o Active Directory em busca de sinais de comprometimento
Se você tiver implementado informações de segurança robustas e monitoramento de eventos (SIEM) em seu ambiente ou estiver usando outros mecanismos para monitorar a segurança da infraestrutura, esta seção fornece informações que podem ser usadas para identificar eventos em sistemas Windows que podem indicar que uma organização está sendo atacada. Discutimos políticas de auditoria tradicionais e avançadas, incluindo configuração efetiva de subcategorias de auditoria nos sistemas operacionais Windows 7 e Windows Vista. Esta seção inclui listas abrangentes de objetos e sistemas para auditoria, e um apêndice associado lista os eventos para os quais você deve monitorar se o objetivo é detectar tentativas de comprometimento.

### <a name="planning-for-compromise"></a>Planejar para comprometimento
Esta seção começa por "reversão" de detalhes técnicos para se concentrar em princípios e processos que podem ser implementados para identificar os usuários, aplicativos e sistemas que são mais críticos não apenas para a infra-estrutura de ti, mas para os negócios. Depois de identificar o que é mais importante para a estabilidade e as operações da sua organização, você pode se concentrar em separar e proteger esses ativos, sejam eles propriedade intelectual, pessoas ou sistemas. Em alguns casos, separar e proteger os ativos podem ser executados em seu ambiente de AD DS existente, enquanto em outros casos, você deve considerar a implementação de "células" pequenas e separadas que permitem estabelecer um limite seguro em relação a ativos críticos e monitorar esses ativos de forma mais rigorosa do que os componentes menos críticos. Um conceito chamado "destruição criativa", que é um mecanismo pelo qual aplicativos e sistemas herdados podem ser eliminados com a criação de novas soluções, e a seção termina com recomendações que podem ajudar a manter um ambiente mais seguro combinando informações de negócios e de ti para construir uma visão detalhada do que é um estado operacional normal. Sabendo o que é normal para uma organização, as anormalidades que podem indicar ataques e comprometimentos podem ser identificadas com mais facilidade.

### <a name="summary-of-best-practice-recommendations"></a>Resumo de recomendações de práticas recomendadas
Esta seção fornece uma tabela que resume as recomendações feitas neste documento e as ordena por prioridade relativa, além de fornecer links para onde mais informações sobre cada recomendação podem ser encontradas no documento e em seus apêndices.

### <a name="appendices"></a>Apêndices
Os apêndices estão incluídos neste documento para aumentar as informações contidas no corpo do documento. A lista de apêndices e uma breve descrição de cada um deles estão incluídas na tabela a seguir.


|**Apêndice**|**Descrição**|
| --- | --- |
|[Apêndice B: Contas privilegiadas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|Fornece informações básicas que ajudam a identificar os usuários e grupos nos quais você deve se concentrar na proteção, pois eles podem ser aproveitados por invasores para comprometer e até mesmo destruir sua instalação de Active Directory.|
|[Apêndice C: Contas protegidas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|Contém informações sobre grupos protegidos no Active Directory. Ele também contém informações para personalização limitada (remoção) de grupos que são considerados grupos protegidos e são afetados por AdminSDHolder e SDProp.|
|[Apêndice D: Como proteger contas de administrador interno no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger a conta de administrador em cada domínio na floresta.|
|[Apêndice E: Como proteger grupos de administrador corporativo no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger o grupo de administradores de empresa na floresta.|
|[Apêndice F: Como proteger grupos de administrador de domínio no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger o grupo Admins. do domínio em cada domínio na floresta.|
|[Apêndice G: Como proteger grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|Contém diretrizes para ajudar a proteger o grupo de administradores internos em cada domínio na floresta.|
|[Apêndice H: Como proteger contas e grupos de administrador local](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|Contém diretrizes para ajudar a proteger contas de administrador local e grupos de administradores em estações de trabalho e servidores ingressados no domínio.|
|[Apêndice I: Como criar o gerenciamento de contas para contas e grupos protegidos no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|Fornece informações para criar contas que têm privilégios limitados e que podem ser rigorosamente controladas, mas podem ser usadas para popular grupos com privilégios em Active Directory quando a elevação temporária é necessária.|
|[Apêndice L: Eventos a serem monitorados](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|Lista os eventos que você deve monitorar em seu ambiente.|
|[Apêndice M: Links de documentos e leitura recomendada](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|Contém uma lista de leituras recomendadas. Também contém uma lista de links para documentos externos e suas URLs para que os leitores de cópias de hardware deste documento possam acessar essas informações.|



