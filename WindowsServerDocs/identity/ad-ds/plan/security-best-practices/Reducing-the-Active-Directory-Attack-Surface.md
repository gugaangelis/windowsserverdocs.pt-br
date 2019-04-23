---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: Redução da superfície de ataque do Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d692641d316b5fe7206cc3f413bdcfc9b74675b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874147"
---
# <a name="reducing-the-active-directory-attack-surface"></a>Redução da superfície de ataque do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta seção se concentra nos controles técnicos para implementar para reduzir a superfície de ataque da instalação do Active Directory. A seção contém as seguintes informações:  
  
-   [Implementando modelos administrativos de privilégio mínimo](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) concentra-se sobre como identificar o risco de que o uso de altamente privilegiado de contas para a administração cotidiana apresenta, além de fornecer recomendações para implementar para reduzir o risco que privilégios contas presente.  
  
-   [Implementar Hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) descreve princípios para a implantação de sistemas administrativos dedicados e seguros, além do exemplo de algumas abordagens para uma implantação de hosts administrativos seguros.  
  
-   [Protegendo controladores de domínio contra ataque](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) discute as políticas e configurações que, embora seja semelhante para as recomendações para a implementação de hosts administrativos seguros, contêm algumas recomendações de específicos do controlador de domínio para ajudar a Certifique-se de que os controladores de domínio e os sistemas usados para gerenciá-los estão bem protegidos.  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Contas privilegiadas e grupos no Active Directory  
Esta seção fornece informações básicas sobre contas privilegiadas e grupos no Active Directory se destina a explicar as semelhanças e diferenças entre contas privilegiadas e grupos no Active Directory. Ao entender essas diferenças, se você implementar as recomendações contidas [Implementando modelos administrativos com menos privilégios](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) textuais ou escolher personalizá-los para sua organização, você tem as ferramentas que você precisa Proteja a cada grupo e conta adequadamente.  
  
### <a name="built-in-privileged-accounts-and-groups"></a>Internos privilegiados contas e grupos  
Active Directory facilita a delegação de administração e suporta o princípio de privilégios mínimos na atribuição de direitos e permissões. "Regulares" usuários com contas em um domínio são, por padrão, capaz de ler grande parte o que é armazenado no diretório, mas são capazes de alterar apenas um conjunto muito limitado de dados no diretório. Os usuários que exigem privilégios adicionais podem ser concedidos a associação em vários grupos "privilegiados" que são criados no diretório para que eles podem executar tarefas específicas relacionadas às suas funções, mas não é possível executar tarefas que não são relevantes para suas tarefas. As organizações também podem criar grupos que são adaptados às responsabilidades do trabalho específico e são concedidos direitos granulares e permissões que permitem que a equipe de TI realizar funções administrativas diárias sem conceder direitos e permissões que excedem o que é necessário para essas funções.  
  
Dentro do Active Directory, os três grupos internos são os grupos de privilégio mais altos no diretório: Administradores de empresa, Admins. do domínio e administradores. A configuração padrão e os recursos de cada um desses grupos são descritos nas seções a seguir:  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Grupos de privilégio mais altos no Active Directory  
  
##### <a name="enterprise-admins"></a>Administrador corporativo  
EA (administradores corporativos) é um grupo que existe somente no domínio raiz da floresta e, por padrão, ele é um membro do grupo Administradores em todos os domínios na floresta. A conta interna de administrador no domínio raiz da floresta é o único membro padrão do grupo EA. EAs recebem direitos e permissões que permitem implementar alterações de toda a floresta (ou seja, as alterações que afetam todos os domínios na floresta), como adição ou remoção de domínios, estabelecer relações de confiança de floresta ou aumentando os níveis funcionais de floresta. Em um modelo de delegação projetada e implementada corretamente, associação EA é necessária somente quando o primeiro construindo a floresta ou ao fazer determinadas alterações de toda a floresta, como estabelecer uma relação de confiança de floresta de saída. A maioria dos direitos e permissões concedidas ao grupo de EA pode ser delegada a usuários com menos privilégios e grupos.  
  
##### <a name="domain-admins"></a>Administradores do domínio  

Cada domínio em uma floresta tem seu próprio grupo de administradores de domínio (DA), que é um membro do grupo de administradores do domínio e um membro do grupo Administradores local em cada computador que ingressou no domínio. O único membro padrão do grupo de acelerador de desenvolvimento para um domínio é a conta interna de administrador para esse domínio. DAs são "poderosa" em seus domínios, enquanto o EAs ter privilégio de toda a floresta. Em um modelo de delegação projetada e implementada corretamente, a associação de Admins. do domínio deve ser necessária somente em cenários de "vigilância" (por exemplo, situações em que uma conta com altos níveis de privilégio em cada computador no domínio é necessária). Embora mecanismos nativos de delegação do Active Directory permitem delegação na medida em que é possível usar contas DA apenas em cenários de emergências, construir um modelo de delegação efetiva pode ser demorado e muitas organizações aproveitam ferramentas de terceiros para acelerar o processo.  
  
##### <a name="administrators"></a>Administradores  
O terceiro grupo é o grupo de administradores (BA) local do domínio interno no qual estão aninhado DAs e EAs. Esse grupo é concedido muitos diretos direitos e permissões no diretório e nos controladores de domínio. No entanto, o grupo de administradores para um domínio não terá privilégios em servidores membros ou estações de trabalho. É por meio da associação no grupo Administradores local dos computadores que privilégio local é concedido.  
  
> [!NOTE]  
> Embora essas sejam as configurações padrão desses grupos privilegiados, um membro de qualquer um dos três grupos pode manipular o diretório para ganhar associação em qualquer um dos outros grupos. Em alguns casos, é fácil obter membros em outros grupos, enquanto em outros, é mais difícil, mas da perspectiva de privilégio potencial, os todos os três grupos devem ser considerados equivalentes com eficiência.  
  
##### <a name="schema-admins"></a>Administradores de esquemas  

Um quarto privilegiado grupo, esquema Admins (SA), existe somente no domínio raiz da floresta e tem apenas desse domínio conta interna de administrador como um membro padrão, semelhante ao grupo Administradores corporativos. O grupo de administradores de esquema destina-se a ser preenchida apenas temporariamente e, ocasionalmente (quando modificação do esquema do AD DS é necessária).  
  
Embora o grupo SA é o único que pode modificar o esquema do Active Directory (ou seja, as estruturas de dados subjacentes do diretório, como objetos e atributos), o escopo do grupo SA direitos e permissões é mais limitado do que o descrito anteriormente grupos. Também é comum encontrar organizações desenvolveram práticas apropriadas para o gerenciamento de associação do grupo de associação de segurança porque a associação no grupo normalmente é necessário com pouca frequência e somente por curtos períodos de tempo. Isso é tecnicamente verdade dos EA, acelerador de desenvolvimento e BA grupos no Active Directory, também, mas é menos comum para encontrar o que as organizações implementou práticas semelhantes para esses grupos para o grupo SA.  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Contas protegidas e grupos no Active Directory  
Dentro do Active Directory, um conjunto padrão de contas privilegiadas e grupos chamados "protegidas" contas e grupos são protegidos de maneira diferente de outros objetos no diretório. Qualquer conta que tenha associação direta ou transitiva em qualquer grupo protegido (independentemente se a associação é derivada de grupos de segurança ou de distribuição) herda essa segurança restrito.  

  
Por exemplo, se um usuário for membro de um grupo de distribuição que é, por sua vez, um membro de um grupo protegido no Active Directory, esse objeto de usuário é sinalizado como uma conta protegida. Quando uma conta é sinalizada como uma conta protegida, o valor do atributo adminCount no objeto é definido como 1.  
  
> [!NOTE]
> Embora transitiva associação em um grupo protegido inclui distribuição aninhada e grupos de segurança aninhados, as contas que são membros de grupos de distribuição aninhado não receberão SID do grupo protegido em seus tokens de acesso. No entanto, os grupos de distribuição podem ser convertidos para grupos de segurança no Active Directory, por isso, os grupos de distribuição estão incluídos na enumeração de membro do grupo protegido. Um grupo de distribuição aninhado protegidos nunca deve ser convertido em um grupo de segurança, as contas que são membros do grupo de distribuição antigo subsequentemente receberá pai protegido SID de grupo em seus tokens de acesso no próximo logon.  
  
A tabela a seguir lista os grupos no Active Directory ao nível do sistema operacional versão e service pack e contas protegidas padrão.  
  
**Contas protegidas e grupos no Active Directory pelo sistema operacional e versão do Service Pack (SP) padrão**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 <SP4**|**Windows 2000 SP4 -Windows Server 2003**|**Windows Server 2003 SP1+**|**Windows Server 2008 -Windows Server 2012**|  
|Administradores|Opers. de contas|Opers. de contas|Opers. de contas|  
||Administrador|Administrador|Administrador|  
||Administradores|Administradores|Administradores|  
|Administradores do domínio|Operadores de cópia|Operadores de cópia|Operadores de cópia|  
||Editores de certificados|||  
||Administradores do domínio|Administradores do domínio|Administradores do domínio|  
|Administrador corporativo|Controladores de Domínio|Controladores de Domínio|Controladores de Domínio|  
||Administrador corporativo|Administrador corporativo|Administrador corporativo|  
||Krbtgt|Krbtgt|Krbtgt|  
||Operadores de Impressão|Operadores de Impressão|Operadores de Impressão|  
||||Controladores de Domínio somente leitura|  
||Duplicador|Duplicador|Duplicador|  
|Administradores de esquemas|Administradores de esquemas||Administradores de esquemas|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder e SDProp  
No contêiner do sistema de cada domínio do Active Directory, um objeto chamado AdminSDHolder é criado automaticamente. A finalidade do objeto AdminSDHolder é garantir que as permissões em grupos e contas protegidas são aplicadas de forma consistente, independentemente de onde as contas e grupos protegidos estão localizadas no domínio.  

A cada 60 minutos (por padrão), um processo conhecido como o propagador do descritor de segurança (SDProp) é executado no controlador de domínio que detém a função de emulador do PDC do domínio. SDProp compara as permissões no objeto de AdminSDHolder do domínio com as permissões a contas e grupos protegidos no domínio. Se as permissões em qualquer um dos grupos e contas protegidas não coincidem com as permissões no objeto AdminSDHolder, as permissões a contas e grupos protegidos serão redefinidas para coincidir com aqueles do objeto de AdminSDHolder do domínio.  
  
Herança de permissões é desabilitada em grupos protegidos e contas, o que significa que mesmo se as contas ou grupos são movidos para locais diferentes no diretório, eles não herdam permissões seus novos objetos pai. Herança também será desabilitada no objeto AdminSDHolder, para que as alterações de permissões aos objetos pai não alteram as permissões de AdminSDHolder.  
  
> [!NOTE]
> Quando uma conta é removida de um grupo protegido, ele não é mais considerado uma conta protegida, mas seu permanece de atributo adminCount definido como 1 se não for alterado manualmente. O resultado dessa configuração é que as ACLs do objeto não são atualizadas pelo SDProp, mas o objeto ainda não herda as permissões de seu objeto pai. Portanto, o objeto pode residir em uma unidade de organizacional (UO) para o qual tenham sido delegadas permissões, mas o objeto anteriormente protegido não herdarão essas permissões delegadas. Um script para localizar e redefinir a objetos protegidos anteriormente no domínio pode ser encontrado na [artigo do Microsoft Support 817433](https://support.microsoft.com/?id=817433).  
  
###### <a name="adminsdholder-ownership"></a>Propriedade AdminSDHolder  
A maioria dos objetos no Active Directory são propriedade do grupo do domínio BA. No entanto, o objeto AdminSDHolder, por padrão, pertencente do grupo do domínio DA. (Isso é uma circunstância na qual DAs não derivar seus direitos e permissões por meio da associação do grupo de administradores do domínio).  
  
Nas versões do Windows anteriores ao Windows Server 2008, os proprietários de um objeto podem alterar permissões do objeto, incluindo conceder a mesmos permissões que ele originalmente não tem. Portanto, as permissões padrão no objeto AdminSDHolder de um domínio impedir que os usuários que são membros dos grupos de BA ou EA alterem as permissões de objeto AdminSDHolder de um domínio. No entanto, os membros do grupo Administradores do domínio podem assumir a propriedade do objeto e conceder a mesmos permissões adicionais, que significa que essa proteção é rudimentar e protege apenas o objeto em relação a modificação acidental por usuários não os membros do grupo no domínio. Além disso, o EA e BA (quando aplicável) grupos têm permissão para alterar os atributos do objeto AdminSDHolder no domínio local (domínio raiz para EA).  
  
> [!NOTE]  
> Um atributo no objeto AdminSDHolder, dSHeuristics, permite que a personalização limitada (remoção) de grupos que são considerados grupos protegidos e são afetados por AdminSDHolder e SDProp. Essa personalização deve ser cuidadosamente considerada se ele for implementado, embora haja válidas circunstâncias em que a modificação de dSHeuristics no AdminSDHolder é útil. Mais informações sobre modificação do atributo dSHeuristics em um objeto AdminSDHolder podem ser encontradas nos artigos do Microsoft Support [817433](https://support.microsoft.com/?id=817433) e [973840](https://support.microsoft.com/kb/973840)e, em [apêndice c: Grupos no Active Directory e contas protegidas](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Embora os mais privilegiados grupos no Active Directory são descritos aqui, há uma série de outros grupos que receberam elevados níveis de privilégio. Para obter mais informações sobre todos os grupos padrão e internos no Active Directory e os direitos de usuário atribuídos a cada um, consulte [apêndice b: Privilegiado contas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md).  
  


