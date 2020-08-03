---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: Redução da superfície de ataque do Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 6044c12744c29d5c13cef03c811349cfbb7d1ded
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87520125"
---
# <a name="reducing-the-active-directory-attack-surface"></a>Redução da superfície de ataque do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta seção aborda os controles técnicos a serem implementados para reduzir a superfície de ataque da instalação do Active Directory. A seção contém as seguintes informações:

- A [implementação de modelos administrativos de privilégios mínimos](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) se concentra na identificação do risco de que o uso de contas altamente privilegiadas para a administração diária apresenta, além de fornecer recomendações para implementar a fim de reduzir o risco de contas privilegiadas presentes.

- A [implementação de hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) descreve os princípios para a implantação de sistemas administrativos dedicados e seguros, além de algumas abordagens de exemplo para uma implantação segura de host administrativo.

- A [proteção de controladores de domínio contra ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) discute políticas e configurações que, embora semelhantes às recomendações para a implementação de hosts administrativos seguros, contêm algumas recomendações específicas do controlador de domínio para ajudar a garantir que os controladores de domínio e os sistemas usados para gerenciá-los estejam bem protegidos.

## <a name="privileged-accounts-and-groups-in-active-directory"></a>Contas privilegiadas e grupos no Active Directory
Esta seção fornece informações básicas sobre contas e grupos com privilégios no Active Directory destinado a explicar o semelhanças e as diferenças entre contas com privilégios e grupos no Active Directory. Ao compreender essas distinções, se você implementar as recomendações na [implementação de modelos administrativos de privilégios mínimos](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) ou optar por personalizá-los para sua organização, terá as ferramentas necessárias para proteger cada grupo e conta adequadamente.

### <a name="built-in-privileged-accounts-and-groups"></a>Contas e grupos com privilégios internos
Active Directory facilita a delegação de administração e dá suporte ao princípio de privilégios mínimos na atribuição de direitos e permissões. Os usuários "regulares" que têm contas em um domínio são, por padrão, capazes de ler grande parte do que está armazenado no diretório, mas podem alterar apenas um conjunto muito limitado de dados no diretório. Os usuários que exigem privilégios adicionais podem receber associação em vários grupos "privilegiados" que são criados no diretório para que possam executar tarefas específicas relacionadas às suas funções, mas não podem executar tarefas que não são relevantes para suas tarefas. As organizações também podem criar grupos que são personalizados para responsabilidades específicas de trabalho e recebem direitos e permissões granulares que permitem que a equipe de ti execute funções administrativas cotidianas sem conceder direitos e permissões que excedem o que é necessário para essas funções.

Em Active Directory, três grupos internos são os grupos de privilégios mais altos no diretório: administradores corporativos, administradores de domínio e administradores. A configuração padrão e os recursos de cada um desses grupos são descritos nas seções a seguir:

#### <a name="highest-privilege-groups-in-active-directory"></a>Grupos de privilégios mais altos no Active Directory

##### <a name="enterprise-admins"></a>Administradores Corporativos
O EA (Enterprise Admins) é um grupo que existe somente no domínio raiz da floresta e, por padrão, é um membro do grupo Administradores em todos os domínios na floresta. A conta interna de administrador no domínio raiz da floresta é o único membro padrão do grupo EA. EAs recebem direitos e permissões que permitem que eles implementem alterações em toda a floresta (ou seja, alterações que afetam todos os domínios na floresta), como adicionar ou remover domínios, estabelecer relações de confiança de floresta ou aumentar os níveis funcionais da floresta. Em um modelo de delegação projetado e implementado corretamente, a associação EA é necessária apenas ao construir pela primeira vez a floresta ou ao fazer determinadas alterações em toda a floresta, como estabelecer uma relação de confiança de floresta de saída. A maioria dos direitos e permissões concedidas ao grupo EA pode ser delegada a usuários e grupos com menos privilégios.

##### <a name="domain-admins"></a>Administradores de Domínio

Cada domínio em uma floresta tem seu próprio grupo de administradores de domínio (DA), que é um membro do grupo de administradores desse domínio e um membro do grupo de Administradores local em cada computador ingressado no domínio. O único membro padrão do grupo DA para um domínio é a conta de administrador interna para esse domínio. O DAs são "todos-poderosos" em seus domínios, enquanto EAs têm privilégios de toda a floresta. Em um modelo de delegação projetado e implementado corretamente, a associação de administradores de domínio deve ser necessária somente em cenários de "interrupção" (como situações em que uma conta com altos níveis de privilégio em cada computador no domínio é necessária). Embora os mecanismos de delegação de Active Directory nativos permitam a delegação na medida em que é possível usar contas DA dos somente em cenários de emergência, construir um modelo de delegação eficaz pode ser demorado e muitas organizações aproveitam ferramentas de terceiros para agilizar o processo.

##### <a name="administrators"></a>Administradores
O terceiro grupo é o grupo de administradores locais de domínio (BA) interno no qual o DAs e EAs são aninhados. Esse grupo recebe muitos dos direitos e permissões diretos no diretório e nos controladores de domínio. No entanto, o grupo de administradores de um domínio não tem privilégios em servidores membros ou em estações de trabalho. É por meio da associação no grupo local de administradores do computador que o privilégio local é concedido.

> [!NOTE]
> Embora essas sejam as configurações padrão desses grupos com privilégios, um membro de qualquer um dos três grupos pode manipular o diretório para obter a associação em qualquer um dos outros grupos. Em alguns casos, é trivial obter a associação nos outros grupos, enquanto em outros é mais difícil, mas da perspectiva do privilégio potencial, todos os três grupos devem ser considerados efetivamente equivalentes.

##### <a name="schema-admins"></a>Administradores de esquemas

Um quarto grupo privilegiado, administradores de esquema (SA), existe apenas no domínio raiz da floresta e tem apenas a conta de administrador interno do domínio como um membro padrão, semelhante ao grupo Administradores de empresa. O grupo Administradores de esquema deve ser preenchido apenas temporariamente e ocasionalmente (quando a modificação do esquema de AD DS é necessária).

Embora o grupo SA seja o único grupo que pode modificar o esquema de Active Directory (ou seja, as estruturas de dados subjacentes do diretório, como objetos e atributos), o escopo dos direitos e permissões do grupo SA é mais limitado do que os grupos descritos anteriormente. Também é comum descobrir que as organizações desenvolveram as práticas apropriadas para o gerenciamento da associação do grupo SA, pois a associação no grupo normalmente é raramente necessária e apenas por longos períodos de tempo. Isso é tecnicamente verdadeiro nos grupos EA, DA e BA em Active Directory, mas é muito menos comum descobrir que as organizações implementaram práticas semelhantes para esses grupos como para o grupo SA.

#### <a name="protected-accounts-and-groups-in-active-directory"></a>Contas protegidas e grupos no Active Directory
No Active Directory, um conjunto padrão de contas com privilégios e grupos chamados de contas e grupos "protegidos" são protegidos de forma diferente dos outros objetos no diretório. Qualquer conta que tenha associação direta ou transitiva em qualquer grupo protegido (independentemente de a associação ser derivada de grupos de segurança ou de distribuição) herda essa segurança restrita.


Por exemplo, se um usuário é membro de um grupo de distribuição que, por sua vez, é um membro de um grupo protegido no Active Directory, esse objeto de usuário é sinalizado como uma conta protegida. Quando uma conta é sinalizada como uma conta protegida, o valor do atributo adminCount no objeto é definido como 1.

> [!NOTE]
> Embora a associação transitiva em um grupo protegido inclua distribuição aninhada e grupos de segurança aninhados, as contas que são membros de grupos de distribuição aninhados não receberão o SID do grupo protegido em seus tokens de acesso. No entanto, os grupos de distribuição podem ser convertidos em grupos de segurança no Active Directory, motivo pelo qual os grupos de distribuição são incluídos na enumeração de membro do grupo protegido. Se um grupo de distribuição aninhado protegido for convertido em um grupo de segurança, as contas que forem membros do grupo de distribuição anterior receberão posteriormente o SID do grupo protegido pai em seus tokens de acesso no próximo logon.

A tabela a seguir lista as contas e grupos protegidos padrão no Active Directory por versão do sistema operacional e nível de service pack.

**Contas e grupos protegidos por padrão no Active Directory pelo sistema operacional e versão do Service Pack (SP)**

|**Windows 2000 <SP4**|**Windows 2000 SP4-Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008 – Windows Server 2012**|
|--|--|--|--|
|Administradores|Opers. de contas|Opers. de contas|Opers. de contas|
||Administrador|Administrador|Administrador|
||Administradores|Administradores|Administradores|
|Administradores de Domínio|Operadores de cópia|Operadores de cópia|Operadores de cópia|
||Editores de Certificados|||
||Administradores de Domínio|Administradores de Domínio|Administradores de Domínio|
|Administradores Corporativos|Controladores de Domínio|Controladores de Domínio|Controladores de Domínio|
||Administradores Corporativos|Administradores Corporativos|Administradores Corporativos|
||Krbtgt|Krbtgt|Krbtgt|
||Operadores de Impressão|Operadores de Impressão|Operadores de Impressão|
||||Controladores de Domínio somente leitura|
||Replicador|Replicador|Replicador|
|Administradores de esquemas|Administradores de esquemas||Administradores de esquemas|

##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder e SDProp
No contêiner do sistema de cada domínio Active Directory, um objeto chamado AdminSDHolder é criado automaticamente. A finalidade do objeto AdminSDHolder é garantir que as permissões em contas e grupos protegidos sejam aplicadas de forma consistente, independentemente de onde estão localizadas as contas e grupos protegidos no domínio.

A cada 60 minutos (por padrão), um processo conhecido como SDProp (propagador de descritor de segurança) é executado no controlador de domínio que contém a função de emulador de PDC do domínio. SDProp compara as permissões no objeto AdminSDHolder do domínio com as permissões nas contas e grupos protegidos no domínio. Se as permissões em qualquer uma das contas e grupos protegidos não corresponderem às permissões no objeto AdminSDHolder, as permissões nas contas e grupos protegidos serão redefinidas para corresponder às do objeto AdminSDHolder do domínio.

A herança de permissões é desabilitada em contas e grupos protegidos, o que significa que, mesmo que as contas ou grupos sejam movidos para locais diferentes no diretório, eles não herdam permissões de seus novos objetos pai. A herança também é desabilitada no objeto AdminSDHolder para que as alterações de permissões para os objetos pai não alterem as permissões do AdminSDHolder.

> [!NOTE]
> Quando uma conta é removida de um grupo protegido, ela não é mais considerada uma conta protegida, mas seu atributo adminCount permanece definido como 1 se não for alterado manualmente. O resultado dessa configuração é que as ACLs do objeto não são mais atualizadas pelo SDProp, mas o objeto ainda não herda as permissões de seu objeto pai. Portanto, o objeto pode residir em uma UO (unidade organizacional) para a qual as permissões foram delegadas, mas o objeto protegido anteriormente não herdará essas permissões delegadas. Um script para localizar e redefinir objetos anteriormente protegidos no domínio pode ser encontrado no [artigo Suporte da Microsoft 817433](https://support.microsoft.com/?id=817433).

###### <a name="adminsdholder-ownership"></a>Propriedade do AdminSDHolder
A maioria dos objetos em Active Directory pertence ao grupo de BA do domínio. No entanto, o objeto AdminSDHolder é, por padrão, de Propriedade do grupo DA dos do domínio. (Essa é uma circunstância na qual o DAs não deriva seus direitos e permissões por meio da associação no grupo Administradores para o domínio.)

Em versões do Windows anteriores ao Windows Server 2008, os proprietários de um objeto podem alterar as permissões do objeto, incluindo a concessão de permissões que não tinham originalmente. Portanto, as permissões padrão no objeto AdminSDHolder de um domínio impedem que os usuários que são membros de grupos de BA ou EA alterem as permissões para o objeto AdminSDHolder de um domínio. No entanto, os membros do grupo Administradores do domínio podem apropriar-se do objeto e conceder permissões adicionais, o que significa que essa proteção é rudimentar e só protege o objeto contra a modificação acidental por usuários que não são membros do grupo DA no domínio. Além disso, os grupos BA e EA (onde aplicável) têm permissão para alterar os atributos do objeto AdminSDHolder no domínio local (domínio raiz para EA).

> [!NOTE]
> Um atributo no objeto AdminSDHolder, dSHeuristics, permite a personalização limitada (remoção) de grupos que são considerados grupos protegidos e são afetados pelo AdminSDHolder e pelo SDProp. Essa personalização deve ser considerada cuidadosamente se for implementada, embora haja circunstâncias válidas nas quais a modificação do dSHeuristics em AdminSDHolder seja útil. Mais informações sobre a modificação do atributo dSHeuristics em um objeto AdminSDHolder podem ser encontradas nos artigos da Suporte da Microsoft [817433](https://support.microsoft.com/?id=817433) e [973840](https://support.microsoft.com/kb/973840)e no [Apêndice C: contas e grupos protegidos no Active Directory](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).

Embora os grupos mais privilegiados no Active Directory sejam descritos aqui, há vários outros grupos que receberam níveis elevados de privilégio. Para obter mais informações sobre todos os grupos padrão e internos no Active Directory e os direitos de usuário atribuídos a cada um, consulte o [Apêndice B: contas e grupos com privilégios no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md).
