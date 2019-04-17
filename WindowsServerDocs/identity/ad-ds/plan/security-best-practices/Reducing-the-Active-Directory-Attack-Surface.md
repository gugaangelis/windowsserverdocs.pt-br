---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: "Reduzir a superfície de ataque do Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2de254076b10a1a75d658f006c2245d523de6b7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="reducing-the-active-directory-attack-surface"></a>Reduzir a superfície de ataque do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta seção se concentra em controles técnicos implementar para reduzir a superfície de ataque da instalação do Active Directory. A seção contém as seguintes informações:  
  
-   [Implementando modelos administrativos de privilégios mínimos](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) enfoca identificando o risco que apresenta o uso de contas altamente privilegiados para administração diária, além de fornecer recomendações para implementar para reduzir o risco de presentes de contas privilegiadas.  
  
-   [Implementando seguro Hosts administrativas](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) descreve princípios de implantação de sistemas administrativas dedicados, seguros, além de alguns amostra abordagens para uma implantação do host administrativas seguro.  
  
-   [Protegendo controladores de domínio contra ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) discute as políticas e configurações que, embora sejam semelhantes às recomendações para a implementação dos hosts administrativas seguras, contêm algumas recomendações específicos de controlador de domínio para ajudar a garantir que os controladores de domínio e os sistemas usados para gerenciá-las são bem protegidos.  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Contas privilegiadas e grupos no Active Directory  
Esta seção fornece informações básicas sobre contas privilegiadas e grupos no Active Directory como objetivo explicar as semelhanças e diferenças entre contas privilegiadas e grupos no Active Directory. Entendendo essas diferenças, se você implementa as recomendações [Implementando modelos administrativos de privilégios mínimos](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) textuais ou escolha personalizá-los para sua organização, você tem as ferramentas necessárias para proteger cada grupo e conta adequadamente.  
  
### <a name="built-in-privileged-accounts-and-groups"></a>Interno privilegiados contas e grupos  
Active Directory facilita a delegação de administração e compatível com o princípio de privilégios mínimos na atribuição de direitos e permissões. "Normais" usuários que possuem contas em um domínio são, por padrão, capaz de ler grande parte o que é armazenado no diretório, mas são capazes de alterar apenas um conjunto muito limitado de dados no diretório. Os usuários que exigem privilégios adicionais podem ser concedidos a participação em vários grupos de "privilegiados" que estão integrados ao diretório para que eles podem realizar tarefas específicas relacionadas às suas funções, mas não é possível executar tarefas que não são relevantes para suas obrigações. As organizações também podem criar grupos que são personalizados para responsabilidades específico e são concedidos granulares direitos e permissões que permitem que a equipe de TI executar as funções administrativas diárias sem conceder direitos e permissões que excedem o que é necessário para essas funções.  
  
No Active Directory, três grupos internos são os grupos de privilégio mais altos na pasta: administradores corporativos, Admins. do domínio e administradores. A configuração padrão e os recursos de cada um desses grupos são descritos nas seções a seguir:  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Grupos de privilégio mais altos no Active Directory  
  
##### <a name="enterprise-admins"></a>Administradores corporativos  
Administradores de empresa (EA) é um grupo que existe somente no domínio raiz da floresta e, por padrão, é um membro do grupo Administradores em todos os domínios na floresta. A conta de administrador interno no domínio raiz da floresta é o único membro padrão do grupo EA. EAs recebem direitos e permissões que permitam implementar mudanças de toda a floresta (ou seja, as alterações que afetam todos os domínios na floresta), por exemplo, adicionando ou removendo domínios, estabelecer relações de confiança de floresta ou níveis funcionais de floresta de acionamento. Em um modelo de delegação projetado e implementado corretamente, a associação ao EA é necessária somente quando o primeiro construção da floresta ou ao fazer determinadas alterações de toda a floresta como estabelecer uma relação de confiança de floresta de saída. A maioria dos direitos e permissões concedidas ao grupo EA pode ser delegada a menor privilégios usuários e grupos.  
  
##### <a name="domain-admins"></a>Administradores de domínio  

Cada domínio em uma floresta tem seu próprio grupo Administradores de domínio (DA), que é um membro do grupo de administradores do domínio e um membro do grupo local Administradores em cada computador que tenha ingressado no domínio. O único membro do grupo para um domínio DA padrão é a conta de administrador interno para esse domínio. DAs são "poderosa" dentro de seus domínios, enquanto EAs têm o privilégio de toda a floresta. Em um modelo de delegação projetado e implementado corretamente, a associação ao grupo Admins. do domínio deve ser necessária apenas em cenários de "vidro quebrar" (por exemplo, situações em que uma conta com altos níveis de privilégio em todos os computadores no domínio é necessária). Embora nativos mecanismos de delegação do Active Directory permitem delegação na medida em que é possível usar contas DA somente em situações de emergências, construir um modelo de delegação efetiva pode ser demorado e, muitas organizações aproveitam ferramentas de terceiros para agilizar o processo.  
  
##### <a name="administrators"></a>Administradores  
O terceiro grupo é interno grupo de domínio local Administradores (BA) em que são aninhados DAs e EAs. Esse grupo é concedido muitos dos direitos diretos e permissões no diretório e em controladores de domínio. No entanto, o grupo de administradores para um domínio não terá privilégios em servidores membro ou em estações de trabalho. É por meio de associação ao grupo Administradores local dos computadores que privilégio local é concedido.  
  
> [!NOTE]  
> Embora essas são as configurações padrão desses grupos privilegiado, um membro de qualquer um dos três grupos pode manipular o diretório para obter a participação em qualquer um dos outros grupos. Em alguns casos, é fácil obter participação em outros grupos, enquanto em outros é mais difícil, mas da perspectiva do privilégio potencial, todos os três grupos considerados efetivamente equivalentes.  
  
##### <a name="schema-admins"></a>Administradores de esquema  

Uma quarta privilegiados grupo, esquema administradores (SA), existe apenas no domínio raiz da floresta e tem apenas desse domínio conta de administrador interno como um membro de padrão, semelhante ao grupo Administradores corporativos. O grupo Administradores de esquema destina-se para ser preenchidos somente temporariamente e ocasionalmente (quando modificação de esquema do AD DS é necessária).  
  
Embora o grupo SA é o único grupo que pode modificar o esquema do Active Directory (ou seja, estruturas de dados subjacente do diretório como atributos e objetos), o escopo de direitos e permissões do grupo SA é mais limitado do que os grupos descritos anteriormente. Também é comum para encontrar as organizações desenvolveram práticas apropriadas para o gerenciamento de associação do grupo SA porque a associação ao grupo com pouca frequência costuma ser necessária e somente por períodos curtos. Isso é verdadeiro tecnicamente dos EA, DA e BA grupos no Active Directory, também, mas é muito menos comum para encontrar o que as organizações implementou práticas semelhante para esses grupos para o grupo SA.  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Protegido contas e grupos no Active Directory  
No Active Directory, um conjunto padrão de contas privilegiadas e grupos chamado "protegidas" contas e grupos são protegidos de maneira diferente dos outros objetos no diretório. Qualquer conta que tenha participação direta ou transitiva em qualquer grupo protegido (independentemente se a associação é derivada de grupos de segurança ou distribuição) herda essa segurança restrita.  

  
Por exemplo, se um usuário for membro de um grupo de distribuição ou seja, por sua vez, um membro de um grupo protegido no Active Directory, esse objeto de usuário sinalizado como uma conta protegida. Quando uma conta é sinalizada como uma conta protegida, o valor do atributo adminCount no objeto é definido como 1.  
  
> [!NOTE]
> Embora transitiva participação em um grupo protegido inclui distribuição aninhada e grupos de segurança aninhados, contas que são membros de grupos de distribuição aninhados não receberá o SID do grupo protegido em seus tokens de acesso. No entanto, os grupos de distribuição podem ser convertidos para grupos de segurança no Active Directory, por isso, grupos de distribuição estão incluídos na enumeração de membro do grupo protegido. Um grupo de distribuição aninhado protegido nunca deve ser convertido em um grupo de segurança, as contas que são membros do grupo de distribuição anterior subsequentemente receberá o pai protegidos SID do grupo nos respectivos tokens de acesso no próximo logon.  
  
A tabela a seguir lista as contas padrão protegido e grupos no Active Directory por nível do sistema operacional versão e o service pack.  
  
**Padrão protegido contas e grupos no Active Directory por sistema operacional e versão do Service Pack (SP)**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4-Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008 - Windows Server 2012**|  
|Administradores|Operadores de conta|Operadores de conta|Operadores de conta|  
||Administrador|Administrador|Administrador|  
||Administradores|Administradores|Administradores|  
|Administradores de domínio|Operadores de backup|Operadores de backup|Operadores de backup|  
||Editores de certificados|||  
||Administradores de domínio|Administradores de domínio|Administradores de domínio|  
|Administradores corporativos|Controladores de domínio|Controladores de domínio|Controladores de domínio|  
||Administradores corporativos|Administradores corporativos|Administradores corporativos|  
||Krbtgt|Krbtgt|Krbtgt|  
||Operadores de impressão|Operadores de impressão|Operadores de impressão|  
||||Controladores de domínio somente leitura|  
||Replicator|Replicator|Replicator|  
|Administradores de esquema|Administradores de esquema||Administradores de esquema|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder e SDProp  
O contêiner do sistema de cada domínio do Active Directory, um objeto chamado AdminSDHolder automaticamente é criado. A finalidade do objeto AdminSDHolder é garantir que as permissões em protegido contas e grupos são aplicadas de forma consistente, independentemente de onde estão localizadas as contas e grupos protegidos no domínio.  

Cada 60 minutos (por padrão), um processo conhecido como propagador do descritor de segurança (SDProp) é executado no controlador de domínio que mantém a função de emulador do PDC do domínio. SDProp compara as permissões AdminSDHolder objeto do domínio com as permissões no protegido contas e grupos no domínio. Se as permissões em qualquer um dos grupos e contas protegidas não coincidem as permissões no objeto AdminSDHolder, as permissões em protegido contas e grupos são redefinidas para que correspondam do objeto de AdminSDHolder do domínio.  
  
Herança de permissões está desabilitada em grupos protegidos e contas, o que significa que mesmo que as contas ou grupos são movidos para locais diferentes no diretório, elas não herdam as permissões dos novos objetos pai. Herança também está desabilitada no objeto AdminSDHolder para que as alterações de permissões aos objetos pai não alteram as permissões de AdminSDHolder.  
  
> [!NOTE]
> Quando uma conta é removida de um grupo protegido, ele não é considerado uma conta protegida, mas sua permanece de atributo adminCount definido como 1 se não for alterado manualmente. O resultado dessa configuração é que as ACLs do objeto não são atualizadas pelo SDProp, mas o objeto ainda não herda as permissões de seu objeto pai. Portanto, o objeto pode residir em uma unidade de organizacional (UO) para que as permissões foram delegadas, mas o objeto protegido anteriormente não herdará essas permissões delegadas. Um script para localizar e restaurar objetos protegidos anteriormente no domínio pode ser encontrado no [artigo Microsoft Support 817433](https://support.microsoft.com/?id=817433).  
  
###### <a name="adminsdholder-ownership"></a>Propriedade AdminSDHolder  
A maioria dos objetos no Active Directory pertencem ao grupo de BA do domínio. No entanto, o objeto AdminSDHolder, por padrão, pertencem ao DA grupo do domínio. (Esta é uma circunstância em que DAs não são derivados seus direitos e permissões por meio de associação ao grupo Administradores do domínio.)  
  
Em versões do Windows anteriores ao Windows Server 2008, os proprietários de um objeto podem alterar as permissões do objeto, inclusive conceder permissões que eles não originalmente tinha a propriamente ditos. Portanto, as permissões padrão no objeto de AdminSDHolder do domínio impedir que os usuários que são membros dos grupos BA ou EA alterem as permissões para o objeto de AdminSDHolder do domínio. No entanto, os membros do grupo Administradores do domínio podem assumir a propriedade do objeto e conceder a mesmos permissões adicionais, que significa que essa proteção é bastante rudimentar e só protege o objeto contra a modificação acidental pelos usuários que não sejam membros do grupo no domínio. Além disso, o BA e EA (quando aplicável) grupos têm permissão para alterar os atributos do objeto AdminSDHolder no domínio local (domínio raiz para EA).  
  
> [!NOTE]  
> Um atributo no objeto AdminSDHolder, dSHeuristics, permite a personalização limitada (remoção) de grupos que são consideradas grupos protegidos e são afetados por AdminSDHolder e SDProp. Essa personalização deve ser considerada cuidadosamente se ele for implementado, apesar de haver válidas circunstâncias em que a modificação de dSHeuristics em AdminSDHolder é útil. Mais informações sobre modificação do atributo dSHeuristics em um objeto AdminSDHolder podem ser encontradas nos artigos Microsoft Support [817433](https://support.microsoft.com/?id=817433) e [973840](https://support.microsoft.com/kb/973840)e em [apêndice c: protegido contas e grupos no Active Directory](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Embora os grupos mais privilegiados no Active Directory são descritos aqui, há um número de outros grupos que foram concedidos elevados níveis de privilégio. Para obter mais informações sobre todos os padrão e grupos internos no Active Directory e os direitos de usuário atribuídos a cada um, veja [apêndice b: privilegiados contas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md).  
  


