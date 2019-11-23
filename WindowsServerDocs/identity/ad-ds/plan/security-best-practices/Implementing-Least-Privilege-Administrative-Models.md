---
ms.assetid: 7a7ab95c-9cb3-4a7b-985a-3fc08334cf4f
title: Implementar modelos administrativos com menos privilégios
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c8ad9b00070d5daef2e5aee43cfdee2d192bddae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367735"
---
# <a name="implementing-least-privilege-administrative-models"></a>Implementar modelos administrativos com menos privilégios

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O trecho a seguir é do [Guia de planejamento de segurança de contas de administrador](https://technet.microsoft.com/library/cc162797.aspx), primeiro publicado em 1º de abril de 1999:

> "Os cursos e a documentação de treinamento mais relacionados à segurança discutem a implementação de um princípio de privilégios mínimos, mas as organizações raramente as acompanham. O princípio é simples, e o impacto de aplicá-lo corretamente aumenta muito sua segurança e reduz seu risco. O princípio determina que todos os usuários devem fazer logon com uma conta de usuário que tenha as permissões mínimas absolutas necessárias para concluir a tarefa atual e nada mais. Isso fornece proteção contra código mal-intencionado, entre outros ataques. Esse princípio se aplica aos computadores e aos usuários desses computadores.   
> "Um motivo pelo qual esse princípio funciona tão bem é que ele o força a fazer alguma pesquisa interna. Por exemplo, você deve determinar os privilégios de acesso que um computador ou usuário realmente precisa e, em seguida, implementá-los. Para muitas organizações, essa tarefa pode inicialmente parecer uma grande quantidade de trabalho; no entanto, é uma etapa essencial proteger com êxito seu ambiente de rede.
> "Você deve conceder a todos os usuários de administrador de domínio seus privilégios de domínio sob o conceito de privilégios mínimos. Por exemplo, se um administrador fizer logon com uma conta privilegiada e, inadvertidamente, executar um programa de vírus, o vírus terá acesso administrativo ao computador local e a todo o domínio. Se o administrador tiver feito logon com uma conta não privilegiada (não administrativa), o escopo de danos do vírus será apenas o computador local, pois ele é executado como um usuário do computador local.
> "Em outro exemplo, as contas às quais você concede direitos de administrador no nível de domínio não devem ter direitos elevados em outra floresta, mesmo que haja uma relação de confiança entre as florestas. Essa tática ajuda a evitar danos amplos se um invasor conseguir comprometer uma floresta gerenciada. As organizações devem auditar regularmente sua rede para proteger contra o escalonamento não autorizado de privilégios. "  

O trecho a seguir é do [Microsoft Windows Security Resource Kit](https://www.microsoftpressstore.com/store/microsoft-windows-security-resource-kit-9780735621749), primeiro publicado em 2005:  

> "Sempre considere a segurança em termos de conceder a menor quantidade de privilégios necessários para realizar a tarefa. Se um aplicativo que tem privilégios demais deve ser comprometido, o invasor poderá expandir o ataque além do que faria se o aplicativo estivesse com a menor quantidade de privilégios possível. Por exemplo, examine as consequências de um administrador de rede abrindo involuntariamente um anexo de email que inicia um vírus. Se o administrador tiver feito logon usando a conta de administrador de domínio, o vírus terá privilégios de administrador em todos os computadores do domínio e, portanto, acesso irrestrito a quase todos os dados na rede. Se o administrador tiver feito logon usando uma conta de administrador local, o vírus terá privilégios de administrador no computador local e, portanto, poderá acessar todos os dados no computador e instalar softwares mal-intencionados, como o software de registro de traço de chave em o computador. Se o administrador tiver feito logon usando uma conta de usuário normal, o vírus terá acesso apenas aos dados do administrador e não será capaz de instalar o software mal-intencionado. Usando os privilégios mínimos necessários para ler emails, neste exemplo, o escopo potencial do comprometimento é muito reduzido. "  

## <a name="the-privilege-problem"></a>O problema de privilégio

Os princípios descritos nos trechos anteriores não foram alterados, mas na avaliação de instalações Active Directory, invariavelmente encontramos números excessivos de contas que receberam direitos e permissões muito além daqueles necessários para executar o dia a dia funciona. O tamanho do ambiente afeta os números brutos de contas com privilégios elevados, mas não os diretórios proportionmidsized podem ter dezenas de contas nos grupos com maior privilégio, enquanto as instalações grandes podem ter centenas ou até milhares. Com poucas exceções, independentemente da sofisticação das habilidades e do arsenal de um invasor, os invasores normalmente seguem o caminho de menos resistência. Eles aumentam a complexidade de suas ferramentas e abordagem apenas se e quando mecanismos mais simples falharem ou forem frustrados por defensores.  

Infelizmente, o caminho de menos resistência em muitos ambientes provou ser o uso excessivo de contas com um privilégio amplo e profundo. Privilégios amplos são direitos e permissões que permitem que uma conta execute atividades específicas em uma grande seção cruzada do ambiente. por exemplo, a equipe de suporte técnico pode receber permissões que permitem redefinir as senhas em muitas contas de usuário.  

Privilégios profundos são privilégios poderosos que são aplicados a um segmento estreito da população, como conceder direitos de administrador de engenheiro a um servidor para que eles possam executar reparos. Nem privilégios amplos nem de um privilégio profundo são necessariamente perigosos, mas quando muitas contas no domínio recebem um privilégio amplo e profundo permanentemente, se apenas uma das contas for comprometida, ela poderá ser usada rapidamente para reconfigurar o ambiente para o objetivos do invasor ou mesmo para destruir grandes segmentos da infraestrutura.  

Os ataques Pass-the-hash, que são um tipo de ataque de roubo de credenciais, são onipresentes porque as ferramentas para realizá-los estão livremente disponíveis e fáceis de usar, e como muitos ambientes estão vulneráveis aos ataques. No entanto, os ataques Pass-the-hash não são o problema real. O crux do problema é duplo:  

1. Normalmente, é fácil para um invasor obter um privilégio profundo em um único computador e, em seguida, propagar esse privilégio em larga escala para outros computadores.  
2. Normalmente, há muitas contas permanentes com altos níveis de privilégio em todo o cenário de computação.

Mesmo que os ataques Pass-the-hash sejam eliminados, os invasores simplesmente usariam táticas diferentes, e não uma estratégia diferente. Em vez de implantar malwares que contenham ferramentas de roubo de credenciais, eles podem desenvolver malwares que registram pressionamentos de teclas ou aproveitar qualquer número de outras abordagens para capturar as credenciais que são poderosas em todo o ambiente. Independentemente das táticas, os destinos permanecem os mesmos: contas com um privilégio amplo e profundo.  

A concessão de privilégios excessivos não é encontrada apenas em Active Directory em ambientes comprometidos. Quando uma organização desenvolveu o hábito de conceder mais privilégios do que o necessário, normalmente ele é encontrado em toda a infraestrutura, conforme discutido nas seções a seguir.  

## <a name="in-active-directory"></a>Em Active Directory

Em Active Directory, é comum descobrir que os grupos EA, DA e BA contêm números excessivos de contas. Normalmente, o grupo EA de uma organização contém os menores membros, os grupos de das geralmente contêm um multiplicador do número de usuários no grupo EA e os grupos de administradores geralmente contêm mais membros do que as populações dos outros grupos combinados. Isso é muitas vezes devido a uma crença de que os administradores são de alguma forma "menos privilegiados" que o DAs ou EAs. Embora os direitos e as permissões concedidos a cada um desses grupos sejam diferentes, eles devem ser efetivamente considerados grupos igualmente poderosos, pois um membro de um pode se tornar um membro dos outros dois.  

## <a name="on-member-servers"></a>Em servidores membro

Quando recuperamos a associação de grupos de administradores locais em servidores membro em muitos ambientes, podemos encontrar associação que varia de várias contas locais e de domínio, a dezenas de grupos aninhados que, quando expandidos, revelam centenas, até milhares, de contas com privilégio de administrador local nos servidores. Em muitos casos, grupos de domínio com associações grandes são aninhados em grupos de administradores locais de servidores membros, sem considerar o fato de que qualquer usuário que possa modificar as associações desses grupos no domínio pode obter controle administrativo de todos os sistemas em que o grupo foi aninhado em um grupo de Administradores local.  

## <a name="on-workstations"></a>Em estações de trabalho

Embora as estações de trabalho normalmente tenham menos Membros em seus grupos de administradores locais do que os servidores membro, em muitos ambientes, os usuários recebem a associação no grupo Administradores local em seus computadores pessoais. Quando isso ocorre, mesmo que o UAC esteja habilitado, esses usuários apresentam um risco elevado à integridade de suas estações de trabalho.  

> [!IMPORTANT]  
> Você deve considerar atentamente se os usuários exigem direitos administrativos em suas estações de trabalho e, se fizerem isso, uma abordagem melhor pode ser criar uma conta local separada no computador que seja membro do grupo Administradores. Quando os usuários exigem elevação, eles podem apresentar as credenciais dessa conta local para elevação, mas como a conta é local, ela não pode ser usada para comprometer outros computadores ou acessar recursos de domínio. Assim como ocorre com qualquer conta local, as credenciais para a conta privilegiada local devem ser exclusivas; Se você criar uma conta local com as mesmas credenciais em várias estações de trabalho, exporte os computadores a ataques de passagem de hash.  

## <a name="in-applications"></a>Em aplicativos

Em ataques em que o destino é a propriedade intelectual de uma organização, contas que receberam privilégios poderosos em aplicativos podem ser direcionadas para permitir vazamento de dados. Embora as contas que têm acesso a dados confidenciais possam ter sido concedidas sem privilégios elevados no domínio ou no sistema operacional, as contas que podem manipular a configuração de um aplicativo ou acesso às informações que o aplicativo fornece apresente o risco.  

## <a name="in-data-repositories"></a>Em repositórios de dados

Como é o caso com outros destinos, os invasores que buscam acesso à propriedade intelectual na forma de documentos e outros arquivos podem direcionar as contas que controlam o acesso aos armazenamentos de arquivos, contas que têm acesso direto aos arquivos ou mesmo grupos ou funções que têm acesso aos arquivos. Por exemplo, se um servidor de arquivos for usado para armazenar documentos de contrato e o acesso for concedido aos documentos pelo uso de um grupo de Active Directory, um invasor que possa modificar a associação do grupo poderá adicionar contas comprometidas ao grupo e acessar o contrato documento. Em casos em que o acesso aos documentos é fornecido por aplicativos como o SharePoint, os invasores podem direcionar os aplicativos conforme descrito anteriormente.  

## <a name="reducing-privilege"></a>Reduzindo o privilégio

Quanto maior e mais complexo um ambiente, mais difícil é gerenciar e proteger. Em pequenas organizações, a revisão e a redução de privilégios podem ser uma proposta relativamente simples, mas cada servidor adicional, estação de trabalho, conta de usuário e aplicativo em uso em uma organização adiciona outro objeto que deve ser protegido. Como pode ser difícil ou até mesmo impossível proteger adequadamente todos os aspectos da infraestrutura de ti de uma organização, você deve concentrar os esforços primeiro nas contas cujo privilégio cria o maior risco, que normalmente são as contas com privilégios internas e grupos em Active Directory e contas locais privilegiadas em estações de trabalho e servidores membro.  

### <a name="securing-local-administrator-accounts-on-workstations-and-member-servers"></a>Protegendo contas de administrador local em estações de trabalho e servidores membro

Embora este documento se concentre em proteger Active Directory, como foi discutido anteriormente, a maioria dos ataques contra o diretório começa como ataques contra hosts individuais. As diretrizes completas para proteger grupos locais em sistemas Membros não podem ser fornecidas, mas as recomendações a seguir podem ser usadas para ajudá-lo a proteger as contas de administrador local em estações de trabalho e servidores membro.  

#### <a name="securing-local-administrator-accounts"></a>Protegendo contas de administrador local

Em todas as versões do Windows atualmente no suporte base, a conta de administrador local é desabilitada por padrão, o que torna a conta inutilizável para os ataques Pass-the-hash e outros roubos de credenciais. No entanto, em domínios que contêm sistemas operacionais herdados ou nos quais as contas de administrador local foram habilitadas, essas contas podem ser usadas conforme descrito anteriormente para propagar o comprometimento entre servidores membros e estações de trabalho. Por esse motivo, os seguintes controles são recomendados para todas as contas de administrador local em sistemas ingressados no domínio.  

Instruções detalhadas para implementar esses controles são fornecidas no [Apêndice H: Protegendo contas e grupos de administrador local](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md). No entanto, antes de implementar essas configurações, verifique se as contas de administrador local não estão sendo usadas no ambiente para executar serviços em computadores ou execute outras atividades para as quais essas contas não devem ser usadas. Teste essas configurações minuciosamente antes de implementá-las em um ambiente de produção.  

#### <a name="controls-for-local-administrator-accounts"></a>Controles para contas de administrador local

As contas de administrador internas nunca devem ser usadas como contas de serviço em servidores membros, nem devem ser usadas para fazer logon em computadores locais (exceto no modo de segurança, que é permitido mesmo que a conta esteja desabilitada). O objetivo de implementar as configurações descritas aqui é impedir que a conta de administrador local de cada computador seja utilizável, a menos que os controles de proteção sejam revertidos primeiro. Ao implementar esses controles e monitorar contas de administrador para alterações, você pode reduzir significativamente a probabilidade de sucesso de um ataque direcionado a contas de administrador local.  

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-joined-systems"></a>Configurando GPOs para restringir contas de administrador em sistemas ingressados no domínio

Em um ou mais GPOs que você cria e vincula a estações de trabalho e a UOs de servidor membro em cada domínio, adicione a conta de administrador aos seguintes direitos de usuário em **computador \** \ \ \ \ \ Configurações de direitos:  

- Negar o acesso a este computador a partir da rede
- Negar o logon como um trabalho em lotes
- Negar o logon como um serviço
- Negar o logon por meio dos Serviços de Área de Trabalho Remota

Ao adicionar contas de administrador a esses direitos de usuário, especifique se você está adicionando a conta de administrador local ou a conta de administrador do domínio da maneira que você rotula a conta. Por exemplo, para adicionar a conta de administrador do domínio NWTRADERS a esses direitos de negação, você deve digitar a conta como **Nwtraders\Administrador**ou navegar até a conta de administrador do domínio nwtraders. Para garantir que você restrinja a conta de administrador local, digite **administrador** nessas configurações de direitos de usuário na editor de objeto de política de grupo.  

> [!NOTE]  
> Mesmo se as contas de administrador local forem renomeadas, as políticas ainda serão aplicadas.  

Essas configurações garantirão que a conta de administrador de um computador não possa ser usada para se conectar aos outros computadores, mesmo que seja habilitada inadvertidamente ou maliciosamente. Os logons locais que usam a conta de administrador local não podem ser completamente desabilitados, nem você deve tentar fazer isso, pois a conta de administrador local de um computador foi projetada para ser usada em cenários de recuperação de desastres.  

Se um servidor membro ou estação de trabalho se tornar separado do domínio sem nenhuma outra conta local concedeu privilégios administrativos, o computador poderá ser inicializado no modo de segurança, a conta de administrador poderá ser habilitada e a conta poderá ser usada para entrar em vigor reparos no computador. Quando os reparos forem concluídos, a conta de administrador deverá ser desabilitada novamente.  

### <a name="securing-local-privileged-accounts-and-groups-in-active-directory"></a>Protegendo contas e grupos com privilégios locais no Active Directory

*Número da lei seis: um computador é tão seguro quanto o administrador é confiável.* - [dez leis imutáveis de segurança (versão 2,0)](https://technet.microsoft.com/security/hh278941.aspx)  

As informações fornecidas aqui se destinam a fornecer diretrizes gerais para proteger as contas e grupos internos de privilégio mais alto no Active Directory. Instruções passo a passo detalhadas também são fornecidas no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md), [Apêndice E: protegendo grupos de administradores corporativos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md), [Apêndice F: protegendo grupos de administradores de domínio no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)e no [Apêndice G: protegendo grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md).  

Antes de implementar qualquer uma dessas configurações, você também deve testar todas as configurações de forma completa para determinar se elas são apropriadas para o seu ambiente. Nem todas as organizações poderão implementar essas configurações.  

#### <a name="securing-built-in-administrator-accounts-in-active-directory"></a>Protegendo contas de administrador internas no Active Directory

Em cada domínio no Active Directory, uma conta de administrador é criada como parte da criação do domínio. Essa conta é, por padrão, um membro dos grupos admins. do domínio no domínio e, se o domínio for o domínio raiz da floresta, a conta também será um membro do grupo Administradores de empresa. O uso da conta de administrador local de um domínio deve ser reservado somente para atividades de compilação iniciais e, possivelmente, cenários de recuperação de desastres. Para garantir que uma conta de administrador interno possa ser usada para afetar os reparos no caso de nenhuma outra contas poder ser usada, você não deve alterar a associação padrão da conta de administrador em nenhum domínio na floresta. Em vez disso, você deve seguir as diretrizes para ajudar a proteger a conta de administrador em cada domínio na floresta. Instruções detalhadas para implementar esses controles são fornecidas no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para contas de administrador internas

O objetivo de implementar as configurações descritas aqui é impedir que a conta de administrador de cada domínio (não um grupo) seja utilizável, a menos que vários controles sejam revertidos. Ao implementar esses controles e monitorar as contas de administrador para alterações, você pode reduzir significativamente a probabilidade de um ataque bem-sucedido, aproveitando a conta de administrador de um domínio. Para a conta de administrador em cada domínio em sua floresta, você deve definir as configurações a seguir.  

##### <a name="enable-the-account-is-sensitive-and-cannot-be-delegated-flag-on-the-account"></a>Habilitar o sinalizador "a conta é confidencial e não pode ser delegado" na conta

Por padrão, todas as contas no Active Directory podem ser delegadas. A delegação permite que um computador ou serviço apresente as credenciais para uma conta que foi autenticada para o computador ou serviço para outros computadores para obter serviços em nome da conta. Quando você habilita a **conta é sensível e não pode ser** um atributo delegado em uma conta baseada em domínio, as credenciais da conta não podem ser apresentadas a outros computadores ou serviços na rede, o que limita os ataques que aproveitam a delegação para usar as credenciais da conta em outros sistemas.  

##### <a name="enable-the-smart-card-is-required-for-interactive-logon-flag-on-the-account"></a>Habilitar o sinalizador "cartão inteligente necessário para logon interativo" na conta

Quando você habilita o **cartão inteligente é necessário para o atributo de logon interativo** em uma conta, o Windows redefine a senha da conta para um valor aleatório de 120 caracteres. Ao definir esse sinalizador em contas de administrador internas, você garante que a senha da conta não seja apenas longa e complexa, mas não seja conhecida por nenhum usuário. Não é tecnicamente necessário criar cartões inteligentes para as contas antes de habilitar esse atributo, mas, se possível, os cartões inteligentes devem ser criados para cada conta de administrador antes de configurar as restrições da conta e os cartões inteligentes devem ser armazenados em locais seguros.  

Embora a definição do **cartão inteligente seja necessária para** que o sinalizador de logon interativo redefina a senha da conta, ela não impede que um usuário com direitos redefina a senha da conta de definir a conta como um valor conhecido e usando o nome da conta e a nova senha para acessar recursos na rede. Por isso, você deve implementar os seguintes controles adicionais na conta.  

##### <a name="configuring-gpos-to-restrict-domains-administrator-accounts-on-domain-joined-systems"></a>Configurando GPOs para restringir contas de administrador de domínios em sistemas ingressados no domínio

Embora a desabilitação da conta de administrador em um domínio torne a conta efetivamente inutilizável, você deve implementar restrições adicionais na conta caso a conta seja habilitada inadvertidamente ou maliciosamente. Embora esses controles possam ser eventualmente revertidos pela conta de administrador, o objetivo é criar controles que prejudiquem o progresso de um invasor e limitar o dano que a conta pode gerar.  

Em um ou mais GPOs que você cria e vincula a estações de trabalho e UOs de servidor membro em cada domínio, adicione a conta de administrador de cada domínio aos seguintes direitos de usuário no **computador \** \ \ \ \ Configurações de direitos:  

- Negar o acesso a este computador a partir da rede  
- Negar o logon como um trabalho em lotes  
- Negar o logon como um serviço  
- Negar o logon por meio dos Serviços de Área de Trabalho Remota  

> [!NOTE]  
> Ao adicionar contas de administrador local a essa configuração, você deve especificar se está configurando contas de administrador local ou contas de administrador de domínio. Por exemplo, para adicionar a conta de administrador local do domínio NWTRADERS a esses direitos de negação, você deve digitar a conta como **Nwtraders\Administrador**ou navegar até a conta de administrador local do domínio nwtraders. Se você digitar **administrador** nessas configurações de direitos de usuário na editor de objeto de política de grupo, restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado.  
>
> É recomendável restringir contas de administrador local em estações de trabalho e servidores membros da mesma maneira que as contas de administrador baseadas em domínio. Portanto, você geralmente deve adicionar a conta de administrador para cada domínio na floresta e a conta de administrador para os computadores locais a essas configurações de direitos de usuário. 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-controllers"></a>Configurando GPOs para restringir contas de administrador em controladores de domínio

Em cada domínio na floresta, a política de controladores de domínio padrão ou uma política vinculada à UO Controladores de domínio deve ser modificada para adicionar a conta de administrador de cada domínio aos seguintes direitos de usuário no computador \ \ \ \ \ \ \ \ \ **atribuições de direitos**:  

- Negar o acesso a este computador a partir da rede  
- Negar o logon como um trabalho em lotes  
- Negar o logon como um serviço  
- Negar o logon por meio dos Serviços de Área de Trabalho Remota  

> [!NOTE]  
> Essas configurações garantirão que a conta de administrador local não possa ser usada para se conectar a um controlador de domínio, embora a conta, se habilitada, possa fazer logon localmente nos controladores de domínio. Como essa conta só deve ser habilitada e usada em cenários de recuperação de desastres, é esperado que o acesso físico a pelo menos um controlador de domínio estará disponível ou que outras contas com permissões para acessar os controladores de domínio remotamente possam ser utiliza.  

##### <a name="configure-auditing-of-built-in-administrator-accounts"></a>Configurar a auditoria de contas de administrador internas

Quando você tiver protegido a conta de administrador de cada domínio e desabilitá-la, deverá configurar a auditoria para monitorar as alterações feitas na conta. Se a conta estiver habilitada, sua senha for redefinida ou outras modificações forem feitas na conta, os alertas deverão ser enviados aos usuários ou às equipes responsáveis pela administração do AD DS, além das equipes de resposta a incidentes em sua organização.  

### <a name="securing-administrators-domain-admins-and-enterprise-admins-groups"></a>Protegendo administradores, administradores de domínio e grupos de administradores de empresa

#### <a name="securing-enterprise-admin-groups"></a>Protegendo grupos de administradores corporativos

O grupo de administradores de empresa, que está hospedado no domínio raiz da floresta, não deve conter nenhum usuário em uma base diária, com a possível exceção da conta de administrador local do domínio, desde que ele seja protegido conforme descrito anteriormente e no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Quando o acesso EA é necessário, os usuários cujas contas exigem direitos de EA e permissões devem ser colocados temporariamente no grupo Administradores de empresa. Embora os usuários estejam usando contas altamente privilegiadas, suas atividades devem ser auditadas e, preferencialmente, executadas com um usuário executando as alterações e outro usuário observando as alterações para minimizar a probabilidade de uso indevido inadvertido ou configuração incorreta . Quando as atividades forem concluídas, as contas deverão ser removidas do grupo EA. Isso pode ser obtido por meio de procedimentos manuais e processos documentados, software PIM/PAM (gerenciamento de identidade/acesso) com privilégios de terceiros ou uma combinação de ambos. As diretrizes para criar contas que podem ser usadas para controlar a associação de grupos com privilégios no Active Directory são fornecidas em [contas atrativas para roubo de credenciais](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md) e instruções detalhadas são fornecidas no [Apêndice I: Criando contas de gerenciamento para contas e grupos protegidos no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Os administradores corporativos são, por padrão, membros do grupo de administradores internos em cada domínio na floresta. A remoção do grupo de administradores de empresa dos grupos de administradores em cada domínio é uma modificação inadequada porque, no caso de um cenário de recuperação de desastre da floresta, os direitos de EA provavelmente serão necessários. Se o grupo Administradores de empresa tiver sido removido dos grupos de administradores em uma floresta, ele deverá ser adicionado ao grupo Administradores em cada domínio e os seguintes controles adicionais deverão ser implementados:  

- Conforme descrito anteriormente, o grupo Administradores de empresa não deve conter nenhum usuário diariamente, com a possível exceção da conta de administrador do domínio raiz da floresta, que deve ser protegido conforme descrito no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
- Em GPOs vinculados a UOs que contêm servidores membros e estações de trabalho em cada domínio, o grupo EA deve ser adicionado aos seguintes direitos de usuário:  
   - Negar o acesso a este computador a partir da rede  
   - Negar o logon como um trabalho em lotes  
   - Negar o logon como um serviço  
   - Negar o logon localmente  
   - Negar logon por meio de Serviços de Área de Trabalho Remota.  

Isso impedirá que os membros do grupo EA efetuem logon em servidores membros e estações de trabalho. Se os servidores de salto forem usados para administrar controladores de domínio e Active Directory, verifique se os servidores de salto estão localizados em uma UO à qual os GPOs restritivos não estão vinculados.  

- A auditoria deve ser configurada para enviar alertas se qualquer modificação for feita nas propriedades ou na associação do grupo EA. Esses alertas devem ser enviados, no mínimo, a usuários ou equipes responsáveis por Active Directory a administração e a resposta a incidentes. Você também deve definir processos e procedimentos para preencher temporariamente o grupo EA, incluindo procedimentos de notificação quando a população legítima do grupo é executada.  
  
#### <a name="securing-domain-admins-groups"></a>Protegendo grupos de administradores de domínio

Como é o caso do grupo Administradores de empresa, a associação em grupos de administradores de domínio deve ser necessária somente em cenários de compilação ou recuperação de desastre. Não deve haver contas de usuário cotidianas no grupo DA com exceção da conta de administrador local para o domínio, se ele tiver sido protegido, conforme descrito no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Quando o acesso DA é necessário, as contas que precisam desse nível de acesso devem ser temporariamente colocadas no grupo DA para o domínio em questão. Embora os usuários estejam usando contas altamente privilegiadas, as atividades devem ser auditadas e, de preferência, executadas com um usuário executando as alterações e outro usuário observando as alterações para minimizar a probabilidade de uso indevido acidental ou configuração incorreta. Quando as atividades tiverem sido concluídas, as contas deverão ser removidas do grupo Admins. do domínio. Isso pode ser obtido por meio de procedimentos manuais e processos documentados, por meio de software de PIM/PAM (gerenciamento de identidade/acesso) com privilégios de terceiros ou uma combinação de ambos. As diretrizes para criar contas que podem ser usadas para controlar a associação de grupos com privilégios no Active Directory são fornecidas no [Apêndice I: Criando contas de gerenciamento para contas e grupos protegidos no Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Os administradores de domínio são, por padrão, membros dos grupos de administradores locais em todos os servidores membros e estações de trabalho em seus respectivos domínios. Esse aninhamento padrão não deve ser modificado porque afeta as opções de suporte e recuperação de desastre. Se os grupos Administradores de domínio tiverem sido removidos dos grupos de administradores locais nos servidores membros, eles deverão ser adicionados ao grupo Administradores em cada servidor membro e estação de trabalho no domínio por meio de configurações de grupo restritos em GPOs vinculados. Os seguintes controles gerais, que são descritos em detalhes no [Apêndice F: proteger grupos de administradores de domínio no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md) também devem ser implementados.  

Para o grupo Admins. do domínio em cada domínio na floresta:  

1. Remova todos os membros do grupo DA, com a possível exceção da conta interna de administrador do domínio, desde que ele tenha sido protegido, conforme descrito no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
2. Em GPOs vinculados a UOs que contêm servidores membros e estações de trabalho em cada domínio, o grupo DA deve ser adicionado aos seguintes direitos de usuário:  
   - Negar o acesso a este computador a partir da rede  
   - Negar o logon como um trabalho em lotes  
   - Negar o logon como um serviço  
   - Negar o logon localmente  
   - Negar o logon por meio dos Serviços de Área de Trabalho Remota  
  
   Isso impedirá membros do grupo da de fazer logon em servidores membros e estações de trabalho. Se os servidores de salto forem usados para administrar controladores de domínio e Active Directory, verifique se os servidores de salto estão localizados em uma UO à qual os GPOs restritivos não estão vinculados.  

3. A auditoria deve ser configurada para enviar alertas se qualquer modificação for feita nas propriedades ou associação do grupo DA. Esses alertas devem ser enviados, no mínimo, a usuários ou equipes responsáveis por AD DS a administração e a resposta a incidentes. Você também deve definir processos e procedimentos para popular temporariamente o grupo DA, incluindo procedimentos de notificação quando a população legítima do grupo é executada.  

#### <a name="securing-administrators-groups-in-active-directory"></a>Protegendo grupos de administradores no Active Directory

Como é o caso com os grupos EA e DA, a associação no grupo Administradores (BA) deve ser necessária somente em cenários de compilação ou recuperação de desastre. Não deve haver contas de usuário do dia a dia no grupo Administradores, com exceção da conta de administrador local do domínio, se ele tiver sido protegido, conforme descrito no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Quando o acesso de administradores é necessário, as contas que precisam desse nível de acesso devem ser temporariamente colocadas no grupo Administradores para o domínio em questão. Embora os usuários estejam usando contas altamente privilegiadas, as atividades devem ser auditadas e, preferencialmente, executadas com um usuário executando as alterações e outro usuário observando as alterações para minimizar a probabilidade de uso indevido inadvertido ou configuração incorreta. Quando as atividades forem concluídas, as contas deverão ser removidas imediatamente do grupo Administradores. Isso pode ser obtido por meio de procedimentos manuais e processos documentados, por meio de software de PIM/PAM (gerenciamento de identidade/acesso) com privilégios de terceiros ou uma combinação de ambos.  

Os administradores são, por padrão, os proprietários da maioria dos objetos AD DS em seus respectivos domínios. A associação a esse grupo pode ser necessária em cenários de compilação e recuperação de desastres nos quais a propriedade ou a capacidade de apropriar-se de objetos é necessária. Além disso, o DAs e EAs herdam vários direitos e permissões em virtude de sua associação padrão no grupo Administradores. O aninhamento de grupo padrão para grupos com privilégios no Active Directory não deve ser modificado, e o grupo de administradores de cada domínio deve ser protegido, conforme descrito no [Apêndice G: protegendo grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)e nas instruções gerais abaixo.  

1. Remova todos os membros do grupo Administradores, com a possível exceção da conta de administrador local para o domínio, desde que ele tenha sido protegido, conforme descrito no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
2. Os membros do grupo Administradores do domínio nunca devem precisar fazer logon em servidores membros ou estações de trabalho. Em um ou mais GPOs vinculados a UOs de estação de trabalho e servidor membro em cada domínio, o grupo Administradores deve ser adicionado aos seguintes direitos de usuário:  
   - Negar o acesso a este computador a partir da rede  
   - Negar logon como um trabalho em lotes,  
   - Negar o logon como um serviço  
   - Isso impedirá que os membros do grupo Administradores sejam usados para fazer logon ou se conectar a servidores membros ou estações de trabalho (a menos que vários controles sejam violados pela primeira vez), onde suas credenciais podem ser armazenadas em cache e, portanto, comprometidas. Uma conta privilegiada nunca deve ser usada para fazer logon em um sistema com menos privilégios, e impor esses controles proporciona proteção contra vários ataques.  

3. Na UO Controladores de domínio em cada domínio na floresta, o grupo Administradores deve receber os seguintes direitos de usuário (se eles ainda não tiverem esses direitos), o que permitirá que os membros do grupo Administradores executem funções necessárias para um cenário de recuperação de desastre em toda a floresta:  
   - Acesso a este computador da rede  
   - Permitir logon localmente  
   - Permitir logon por meio dos Serviços de Área de Trabalho Remota  

4. A auditoria deve ser configurada para enviar alertas se qualquer modificação for feita nas propriedades ou na associação do grupo Administradores. Esses alertas devem ser enviados, no mínimo, aos membros da equipe responsável pela administração de AD DS. Os alertas também devem ser enviados aos membros da equipe de segurança e os procedimentos devem ser definidos para modificar a associação do grupo de administradores. Especificamente, esses processos devem incluir um procedimento pelo qual a equipe de segurança seja notificada quando o grupo de administradores for modificado para que, quando os alertas forem enviados, eles sejam esperados e um alarme não seja gerado. Além disso, os processos para notificar a equipe de segurança quando o uso do grupo de administradores foi concluído e as contas usadas foram removidas do grupo.  

> [!NOTE]  
> Quando você implementa restrições no grupo Administradores em GPOs, o Windows aplica as configurações aos membros do grupo de administradores locais de um computador, além do grupo de administradores do domínio. Portanto, você deve ter cuidado ao implementar restrições no grupo Administradores. Embora seja recomendável proibir logons de rede, lote e serviço para membros do grupo Administradores, sempre que for viável implementar, não restrinja logons locais ou logons por meio de Serviços de Área de Trabalho Remota. O bloqueio desses tipos de logon pode bloquear a administração legítima de um computador por membros do grupo local de administradores. A captura de tela a seguir mostra as definições de configuração que bloqueiam o uso indevido de contas internas de administrador de domínio e locais, além do uso indevido de grupos internos de administradores de domínio ou locais. Observe que o direito de usuário **Negar logon pelo serviços de área de trabalho remota** não inclui o grupo Administradores, pois incluí-lo nessa configuração também bloquearia esses logons para contas que são membros do grupo Administradores do computador local. Se os serviços em computadores estiverem configurados para serem executados no contexto de qualquer um dos grupos privilegiados descritos nesta seção, a implementação dessas configurações poderá fazer com que os serviços e aplicativos falhem. Portanto, assim como todas as recomendações nesta seção, você deve testar exaustivamente as configurações de aplicabilidade em seu ambiente.  
>
> ![modelos de administrador de privilégios mínimos](media/Implementing-Least-Privilege-Administrative-Models/SAD_3.gif)  

### <a name="role-based-access-controls-rbac-for-active-directory"></a>RBAC (controles de acesso baseado em função) para Active Directory

Em termos gerais, os RBAC (controles de acesso baseado em função) são um mecanismo para agrupar usuários e fornecer acesso a recursos com base em regras de negócio. No caso do Active Directory, a implementação de RBAC para AD DS é o processo de criação de funções às quais direitos e permissões são delegadas para permitir que os membros da função executem tarefas administrativas cotidianas sem conceder a eles privilégios excessivos. O RBAC para Active Directory pode ser projetado e implementado por meio de ferramentas e interfaces nativas, aproveitando o software que você já tem, adquirindo produtos de terceiros ou qualquer combinação dessas abordagens. Esta seção não fornece instruções passo a passo para implementar o RBAC para Active Directory, mas, em vez disso, aborda fatores que você deve considerar ao escolher uma abordagem para implementar o RBAC em suas instalações de AD DS.  

#### <a name="native-approaches-to-rbac-for-active-directory"></a>Abordagens nativas para RBAC para Active Directory

Na implementação RBAC mais simples, você pode implementar funções como grupos de AD DS e delegar direitos e permissões para os grupos que permitem que eles executem a administração diária dentro do escopo designado da função.  

Em alguns casos, os grupos de segurança existentes no Active Directory podem ser usados para conceder direitos e permissões apropriadas a uma função de trabalho. Por exemplo, se os funcionários específicos em sua organização de ti forem responsáveis pelo gerenciamento e manutenção de zonas e registros DNS, delegar essas responsabilidades poderá ser tão simples quanto criar uma conta para cada administrador DNS e adicioná-la ao DNS Grupo de administradores no Active Directory. O grupo de administradores de DNS, ao contrário de grupos mais altamente privilegiados, tem poucos direitos poderosos em Active Directory, embora os membros desse grupo tenham sido delegadas permissões que permitem administrar o DNS e ainda estão sujeitos a comprometimento e abuso podem resultar em elevação de privilégio.

Em outros casos, talvez seja necessário criar grupos de segurança e delegar direitos e permissões para Active Directory objetos, objetos do sistema de arquivos e objetos do registro para permitir que os membros dos grupos executem tarefas administrativas designadas. Por exemplo, se os operadores de suporte técnico forem responsáveis por redefinir senhas esquecidas, auxiliar os usuários com problemas de conectividade e solucionar problemas de configurações de aplicativo, talvez seja necessário combinar configurações de delegação em objetos de usuário no Active Directory com privilégios que permitem que os usuários de suporte técnico se conectem remotamente a computadores dos usuários para exibir ou modificar as definições de configuração dos usuários. Para cada função que você definir, você deve identificar:  

1. As tarefas que os membros da função desempenham diariamente e quais tarefas são executadas com menos frequência.  
2. Em quais sistemas e em quais aplicativos os membros de uma função devem receber direitos e permissões.  
3. Quais usuários devem receber a associação em uma função.  
4. Como o gerenciamento de associações de função será executado.  

Em muitos ambientes, a criação manual de controles de acesso baseado em função para a administração de um ambiente de Active Directory pode ser desafiadora para implementar e manter. Se você tiver funções e responsabilidades claramente definidas para a administração da sua infraestrutura de ti, talvez queira aproveitar ferramentas adicionais para ajudá-lo a criar uma implantação RBAC nativa gerenciável. Por exemplo, se o FIM (Forefront Identity Manager) estiver em uso no seu ambiente, você poderá usar o FIM para automatizar a criação e a população de funções administrativas, o que pode facilitar a administração contínua. Se você usar System Center Configuration Manager (SCCM) e System Center Operations Manager (SCOM), poderá usar funções específicas do aplicativo para delegar funções de gerenciamento e monitoramento e também impor configuração e auditoria consistentes entre sistemas no o domínio. Se você tiver implementado uma PKI (infraestrutura de chave pública), poderá emitir e exigir cartões inteligentes para a equipe de ti responsável pela administração do ambiente. Com o fim CM (gerenciamento de credenciais do FIM), você pode até mesmo combinar o gerenciamento de funções e credenciais para sua equipe administrativa.  

Em outros casos, pode ser preferível para uma organização considerar a implantação de software RBAC de terceiros que fornece funcionalidade "pronta". As soluções comerciais, COTS (contínuas) para RBAC para diretórios Active Directory, Windows e não Windows e sistemas operacionais são oferecidas por vários fornecedores. Ao escolher entre soluções nativas e produtos de terceiros, você deve considerar os seguintes fatores:  

1. Orçamento: ao investir em desenvolvimento de RBAC usando software e ferramentas que talvez você já tenha, você pode reduzir os custos de software envolvidos na implantação de uma solução. No entanto, a menos que você tenha uma equipe que tenha experiência em criar e implantar soluções de RBAC nativas, talvez seja necessário envolver os recursos de consultoria para desenvolver sua solução. Você deve avaliar cuidadosamente os custos previstos de uma solução desenvolvida com os custos para implantar uma solução "pronta", especialmente se o seu orçamento for limitado.  
2. Composição do ambiente de ti: se o seu ambiente for composto principalmente de sistemas Windows, ou se você já estiver aproveitando Active Directory para o gerenciamento de sistemas e contas que não são do Windows, as soluções nativas personalizadas poderão fornecer a solução ideal para suas necessidades. Se sua infraestrutura contiver muitos sistemas que não estão executando o Windows e não forem gerenciados pelo Active Directory, talvez seja necessário considerar as opções de gerenciamento de sistemas não Windows separadamente do ambiente de Active Directory.  
3. Modelo de privilégio na solução: se um produto depender do posicionamento de suas contas de serviço em grupos altamente privilegiados no Active Directory e não oferecer opções que não exigem privilégios excessivos para o software RBAC, você não terá realmente reduzido sua Active Directory superfície de ataque você alterou a composição dos grupos mais privilegiados no diretório. A menos que um fornecedor de aplicativos possa fornecer controles para contas de serviço que minimizem a probabilidade das contas serem comprometidas e usadas de forma mal-intencionada, talvez você queira considerar outras opções.  

### <a name="privileged-identity-management"></a>Gerenciamento de identidades com privilégios

O PIM (Privileged Identity Management), às vezes chamado de PAM (gerenciamento de conta privilegiada) ou PCM (gerenciamento de credenciais privilegiadas), é o design, a construção e a implementação de abordagens para gerenciar contas privilegiadas em seu infra-estruturas. Em termos gerais, o PIM fornece mecanismos pelos quais as contas recebem direitos e permissões temporárias necessárias para executar funções de correção de compilação ou interrupção, em vez de deixar os privilégios anexados permanentemente às contas. Se a funcionalidade PIM é criada manualmente ou implementada por meio da implantação de software de terceiros, um ou mais dos recursos a seguir podem estar disponíveis:  
  
- "Cofres" de credencial, em que as senhas para contas com privilégios são "extraídas" e atribuídas a uma senha inicial e, em seguida, "check-in" quando as atividades foram concluídas, quando as senhas são redefinidas novamente nas contas.  
- Restrições de limite de tempo no uso de credenciais com privilégios  
- Credenciais de uso único  
- Concessão de privilégio gerada por fluxo de trabalho com monitoramento e relatório de atividades executadas e remoção automática de privilégio quando as atividades são concluídas ou o tempo alocado expirou  
- Substituição de credenciais embutidas em código, como nomes de usuário e senhas em scripts com APIs (interfaces de programação de aplicativo) que permitem que as credenciais sejam recuperadas dos cofres conforme necessário  
- Gerenciamento automático de credenciais de conta de serviço  

### <a name="creating-unprivileged-accounts-to-manage-privileged-accounts"></a>Criando contas sem privilégios para gerenciar contas com privilégios

Um dos desafios de gerenciar contas privilegiadas é que, por padrão, as contas que podem gerenciar contas e grupos com privilégios e protegidos são contas privilegiadas e protegidas. Se você implementar soluções de RBAC e PIM apropriadas para sua instalação do Active Directory, as soluções podem incluir abordagens que permitem que você despreencha efetivamente a Associação dos grupos mais privilegiados no diretório, preenchendo apenas os grupos temporariamente e quando necessário.  

No entanto, se você implementar o RBAC nativo e o PIM, deverá considerar a criação de contas que não têm nenhum privilégio e com a única função de popular e depopularização de grupos privilegiados em Active Directory quando necessário. [Apêndice I: a criação de contas de gerenciamento para contas e grupos protegidos no Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md) fornece instruções passo a passo que você pode usar para criar contas para essa finalidade.  

### <a name="implementing-robust-authentication-controls"></a>Implementando controles de autenticação robustos

*Número da lei seis: realmente há alguém tentando adivinhar suas senhas.* - [10 leis imutáveis da administração de segurança](https://technet.microsoft.com/library/cc722488.aspx)  

Os ataques Pass-the-hash e outros roubos de credenciais não são específicos dos sistemas operacionais Windows, nem são novos. O primeiro ataque Pass-the-hash foi criado em 1997. No entanto, historicamente, esses ataques exigiam ferramentas personalizadas, foram atingidos ou perdem em seu sucesso e os invasores precisavam ter um grau relativamente alto de habilidade. A introdução de ferramentas gratuitas disponíveis e fáceis de usar que extraia nativamente as credenciais resultou em um aumento exponencial do número e do sucesso dos ataques de roubo de credenciais nos últimos anos. No entanto, os ataques de roubo de credenciais não são os únicos mecanismos pelos quais as credenciais são destinadas e comprometidas.  

Embora você deva implementar controles para ajudar a protegê-lo contra ataques de roubo de credenciais, você também deve identificar as contas em seu ambiente que são mais prováveis de serem direcionadas por invasores e implementar controles de autenticação robustos para esses as. Se suas contas mais privilegiadas estiverem usando a autenticação de fator único, como nomes de usuário e senhas (ambos são "algo que você sabe", que é um fator de autenticação), essas contas serão protegidas de forma fraca. Tudo o que um invasor precisa é conhecer o nome de usuário e o conhecimento da senha associada à conta, e os ataques de passagem de hash não são necessários para que o invasor possa se autenticar como o usuário em qualquer sistema que aceite credenciais de fator único.  

Embora a implementação da autenticação multifator não proteja você contra ataques Pass-the-hash, a implementação da autenticação multifator em combinação com sistemas protegidos pode. Mais informações sobre a implementação de sistemas protegidos são fornecidas na [implementação de hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md), e as opções de autenticação são discutidas nas seções a seguir.  

#### <a name="general-authentication-controls"></a>Controles de autenticação gerais

Se você ainda não implementou a autenticação multifator, como cartões inteligentes, considere fazer isso. Os cartões inteligentes implementam a proteção imposta por hardware de chaves privadas em um par de chaves pública-privada, impedindo que a chave privada de um usuário seja acessada ou usada, a menos que o usuário apresente o PIN, a senha ou o identificador biométrico adequado ao cartão inteligente. Mesmo se o PIN ou a senha de um usuário for interceptado por um agente de log de pressionamentos de tecla em um computador comprometido, para que um invasor reutilize o PIN ou a senha, o cartão também deverá estar fisicamente presente.

Em casos em que senhas longas e complexas comprovadamente são difíceis de implementar devido à resistência do usuário, os cartões inteligentes fornecem um mecanismo pelo qual os usuários podem implementar PINs relativamente simples ou senhas sem que as credenciais sejam suscetíveis à força bruta ou ataques de mesa Rainbow. Os PINs de cartão inteligente não são armazenados em Active Directory ou em bancos de dados SAM locais, embora os hashes de credenciais ainda possam ser armazenados na memória protegida do LSASs em computadores nos quais os cartões inteligentes foram usados para autenticação.  

#### <a name="additional-controls-for-vip-accounts"></a>Controles adicionais para contas VIP

Outro benefício da implementação de cartões inteligentes ou outros mecanismos de autenticação baseados em certificado é a capacidade de aproveitar a garantia do mecanismo de autenticação para proteger dados confidenciais que podem ser acessados por usuários VIP. A garantia do mecanismo de autenticação está disponível em domínios nos quais o nível funcional está definido como Windows Server 2012 ou Windows Server 2008 R2. Quando habilitada, a garantia do mecanismo de autenticação adiciona uma associação de grupo global designada pelo administrador ao token Kerberos de um usuário quando as credenciais do usuário são autenticadas durante o logon usando um método de logon baseado em certificado.  

Isso possibilita que os administradores de recursos controlem o acesso a recursos, como arquivos, pastas e impressoras, com base em se o usuário faz logon usando um método de logon baseado em certificado, além do tipo de certificado usado. Por exemplo, quando um usuário faz logon usando um cartão inteligente, o acesso do usuário aos recursos na rede pode ser especificado como sendo diferente do que o acesso é quando o usuário não usa um cartão inteligente (ou seja, quando o usuário faz logon digitando um nome de usuário e senha). Para obter mais informações sobre a garantia do mecanismo de autenticação, consulte a [garantia do mecanismo de autenticação para AD DS no guia passo a passo do Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897.aspx).  

#### <a name="configuring-privileged-account-authentication"></a>Configurando a autenticação de conta privilegiada

Em Active Directory para todas as contas administrativas, habilite o atributo **exigir cartão inteligente para logon interativo** e auditoria de alterações (no mínimo), qualquer um dos atributos na guia **conta** para a conta (por exemplo, CN, nome, sAMAccountName, userPrincipalName e userAccountControl) objetos de usuário administrativo.  

Embora a definição do **cartão inteligente exigir logon interativo** em contas redefine a senha da conta para um valor aleatório de 120 caracteres e requer cartões inteligentes para logons interativos, o atributo ainda pode ser substituído por usuários com permissões que permitem alterar as senhas nas contas, e as contas podem ser usadas para estabelecer logons não interativos com apenas o nome de usuário e a senha.  

Em outros casos, dependendo da configuração de contas nas configurações de Active Directory e certificado nos serviços de certificados Active Directory (AD CS) ou em uma PKI de terceiros, os atributos de UPN (nome principal do usuário) para contas administrativas ou VIP podem ser direcionados para um tipo específico de ataque, conforme descrito aqui.  

##### <a name="upn-hijacking-for-certificate-spoofing"></a>Seqüestro de UPN para falsificação de certificado

Embora uma discussão completa sobre ataques contra infraestruturas de chave pública (PKIs) esteja fora do escopo deste documento, os ataques contra PKIs pública e privada aumentaram exponencialmente desde 2008. As violações das PKIs públicas foram amplamente divulgadas, mas os ataques contra a PKI interna de uma organização são, talvez, ainda mais produtivos. Um desses ataques aproveita Active Directory e certificados para permitir que um invasor falsifique as credenciais de outras contas de uma maneira que pode ser difícil de detectar.  

Quando um certificado é apresentado para autenticação em um sistema ingressado no domínio, o conteúdo do assunto ou o atributo SAN (nome alternativo da entidade) no certificado são usados para mapear o certificado para um objeto de usuário no Active Directory. Dependendo do tipo de certificado e de como ele é construído, o atributo Subject em um certificado normalmente contém o CN (nome comum) de um usuário, conforme mostrado na captura de tela a seguir.  

![modelos de administrador de privilégios mínimos](media/Implementing-Least-Privilege-Administrative-Models/SAD_4.gif)  

Por padrão, Active Directory constrói o CN de um usuário concatenando o primeiro nome + "" + sobrenome da conta. No entanto, os componentes CN de objetos de usuário no Active Directory não são necessários ou têm a garantia de serem exclusivos e a movimentação de uma conta de usuário para um local diferente no diretório altera o DN (nome distinto) da conta, que é o caminho completo para o objeto no , conforme mostrado no painel inferior da captura de tela anterior.  

Como os nomes de entidades de certificado não têm garantia de serem estáticos ou exclusivos, o conteúdo do nome alternativo da entidade geralmente é usado para localizar o objeto de usuário em Active Directory. O atributo SAN para certificados emitidos para usuários de autoridades de certificação corporativas (Active Directory CAs integradas) normalmente contém o UPN ou o endereço de email do usuário. Como os UPNs têm a garantia de serem exclusivos em uma floresta AD DS, localizar um objeto de usuário pelo UPN é normalmente executado como parte da autenticação, com ou sem certificados envolvidos no processo de autenticação.  

O uso de UPNs em atributos de SAN em certificados de autenticação pode ser aproveitado por invasores para obter certificados fraudulentos. Se um invasor tiver comprometido uma conta que tenha a capacidade de ler e escrever UPNs em objetos de usuário, o ataque será implementado da seguinte maneira:  

O atributo UPN em um objeto de usuário (como um usuário VIP) é temporariamente alterado para um valor diferente. O atributo nome da conta SAM e CN também podem ser alterados no momento, embora isso geralmente não seja necessário pelos motivos descritos anteriormente.  

Quando o atributo UPN na conta de destino tiver sido alterado, uma conta de usuário obsoleta ou habilitada, ou um atributo UPN da conta de usuário criada recentemente, será alterado para o valor originalmente atribuído à conta de destino. Contas de usuário obsoletas, habilitadas são contas que não fizeram logon por longos períodos de tempo, mas não foram desabilitadas. Eles são direcionados por invasores que pretendem "ocultar em visão simples" pelos seguintes motivos:  

1. Como a conta está habilitada, mas não foi usada recentemente, o uso da conta é improvável de disparar alertas da forma como a habilitação de uma conta de usuário desabilitada pode.  
2. O uso de uma conta existente não requer a criação de uma nova conta de usuário que possa ser observada pela equipe administrativa.  
3. Contas de usuário obsoletas que ainda estão habilitadas geralmente são membros de vários grupos de segurança e recebem acesso a recursos na rede, simplificando o acesso e "mesclando" em uma população de usuário existente.  

A conta de usuário na qual o UPN de destino foi configurado agora é usada para solicitar um ou mais certificados de Active Directory serviços de certificados.  

Quando os certificados são obtidos para a conta do invasor, os UPNs na conta "New" e na conta de destino são retornados aos seus valores originais.  

O invasor agora tem um ou mais certificados que podem ser apresentados para autenticação para recursos e aplicativos como se o usuário fosse o usuário VIP cuja conta foi modificada temporariamente. Embora uma discussão completa de todas as maneiras em que os certificados e a PKI possam ser direcionados por invasores esteja fora do escopo deste documento, esse mecanismo de ataque é fornecido para ilustrar por que você deve monitorar contas privilegiadas e de VIP em AD DS para alterações, especialmente para alterações em qualquer um dos atributos na guia **conta** da conta (por exemplo, CN, nome, sAMAccountName, userPrincipalName e userAccountControl). Além de monitorar as contas, você deve restringir quem pode modificar as contas para o menor conjunto de usuários administrativos possível. Da mesma forma, as contas de usuários administrativos devem ser protegidas e monitoradas para alterações não autorizadas.  
