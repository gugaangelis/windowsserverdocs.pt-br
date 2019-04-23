---
ms.assetid: 7a7ab95c-9cb3-4a7b-985a-3fc08334cf4f
title: Implementar modelos administrativos com menos privilégios
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 609ea0bd796a4d3696e14c7499be047ebdd83183
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868847"
---
# <a name="implementing-least-privilege-administrative-models"></a>Implementar modelos administrativos com menos privilégios

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O trecho a seguir é de [o administrador Accounts Security Planning Guide](https://technet.microsoft.com/library/cc162797.aspx), publicada pela primeira vez em 1º de abril de 1999:

> "Os cursos de treinamento e documentação mais relacionadas à segurança discutem a implementação de um princípio de privilégio mínimo, ainda que as organizações a seguir raramente. O princípio é simples, e o impacto de aplicá-la corretamente bastante aumenta a segurança e reduz o risco. O princípio declara que todos os usuários devem fazer logon com uma conta de usuário que tenha as permissões mínimas necessárias para concluir a tarefa atual e nada mais absolutas. Isso fornece proteção contra código mal-intencionado, entre outros ataques. Esse princípio se aplica a computadores e os usuários desses computadores.   
> "Um motivo que esse princípio funcione tão bem é que ela obriga você faça algumas pesquisas internas. Por exemplo, você deve determinar os privilégios de acesso que um computador ou usuário realmente precisa e, em seguida, implementá-los. Para muitas organizações, essa tarefa inicialmente pode parecer uma grande quantidade de trabalho; No entanto, é uma etapa essencial para proteger o seu ambiente de rede com êxito.
> "Você deve conceder a todos os usuários do administrador de domínio seus privilégios de domínio sob o conceito de privilégios mínimos. Por exemplo, se um administrador faz logon com uma conta com privilégios e inadvertidamente executa um programa de vírus, o vírus tem acesso administrativo ao computador local e no domínio inteiro. Se o administrador em vez disso, tinha feito logon com uma conta sem privilégios de (não administrativa), o escopo do vírus de danos seria apenas o computador local porque ele é executado como um usuário do computador local.
> "Em outro exemplo, contas para a qual você concede direitos de administrador de nível de domínio devem não ter direitos elevados em outra floresta, mesmo se houver uma relação de confiança entre as florestas. Essa tática ajuda a evitar danos extensivos, se um invasor comprometer uma floresta gerenciada. As organizações devem regularmente "auditoria" sua rede para proteger contra o escalonamento não autorizado de privilégios.  

O trecho a seguir é do [Microsoft Windows Security Resource Kit](https://www.microsoftpressstore.com/store/microsoft-windows-security-resource-kit-9780735621749), primeiro 2005 publicado em:  

> "Sempre pensam na segurança em termos de conceder o mínimo de privilégios necessários para executar a tarefa. Se um aplicativo que tenha muitos privilégios deve ser comprometido, o invasor pode ser capaz de expandir o ataque além do que seria se o aplicativo tivesse sido sob o mínimo de privilégios possíveis. Por exemplo, examine as consequências de um administrador de rede involuntariamente abrindo um anexo de email que inicia um vírus. Se o administrador faz logon usando a conta de administrador do domínio, o vírus terá privilégios de administrador em todos os computadores no domínio e, portanto, os acesso irrestrito a quase todos os dados na rede. Se o administrador faz logon usando uma conta de administrador local, o vírus terá privilégios de administrador no computador local e, portanto, seria capaz de acessar todos os dados no computador e instalar software mal-intencionado, como software de registro de chave de traço no o computador. Se o administrador faz logon usando uma conta de usuário normal, o vírus terão acesso somente aos dados do administrador e não poderão instalar o software mal-intencionado. Usando os privilégios mínimos necessários para ler o email, neste exemplo, o escopo de potencial de comprometimento é bastante reduzido."  

## <a name="the-privilege-problem"></a>O problema de privilégio

Princípios descritos os trechos anteriores não foram alterados, mas na avaliação de instalações do Active Directory, podemos encontrar invariavelmente números excessivos de contas que receberam direitos e permissões muito além daqueles necessários para executar diárias trabalho. O tamanho do ambiente afeta os números brutos de contas privilegiadas excessivamente, mas não os diretórios proportionmidsized podem ter dezenas de contas nos grupos de mais altamente privilegiados, enquanto grandes instalações podem ter centenas ou mesmo milhares. Com poucas exceções, independentemente da sofisticação de um invasor habilidades e arsenal, os invasores normalmente seguem o caminho de menor resistência. Eles aumentam a complexidade de suas ferramentas e a abordagem somente quando mais simples mecanismos falha ou barrado com os defensores.  

Infelizmente, o caminho de menor resistência em muitos ambientes provou para ser o uso excessivo de contas com privilégios de amplo e profundo. Privilégios amplos são direitos e permissões que permitem que uma conta executar atividades específicas em uma grande parte dos ambiente - por exemplo, a equipe de suporte técnico pode ser concedida permissões que permitem a eles redefinir as senhas em várias contas de usuário.  

Privilégios profunda privilégios avançados que são aplicados a um segmento estreito da população, tal dar um engenheiro de direitos de administrador em um servidor para que eles possam executar reparos. Nem o privilégio amplo nem o privilégio profunda é necessariamente perigoso, mas quando muitas contas no domínio permanentemente recebem privilégios amplo e profundo, se apenas uma das contas for comprometida, ela rapidamente pode ser usada para reconfigurar o ambiente para o finalidades do invasor ou até mesmo para destruir grandes segmentos da infraestrutura.  

Ataques Pass-the-hash, que são um tipo de ataque de roubo de credencial, são todos os lugares, porque as ferramentas para realizá-las estão livremente disponível e fácil de usar e porque muitos ambientes são vulneráveis a ataques. Ataques Pass-the-hash, no entanto, não são o problema real. O xis do problema é dupla:  

1. É geralmente mais fácil para um invasor obter privilégios profundo em um único computador e, em seguida, propagar esse privilégio em larga escala para outros computadores.  
2. Normalmente, há muitas contas permanentes com altos níveis de privilégio em todo o cenário de computação.

Mesmo que os ataques pass-the-hash são eliminados, os invasores simplesmente usaria táticas diferentes, não uma estratégia diferente. Em vez de implantem o malware que contém as ferramentas de roubo de credenciais, eles podem planta malware que registra em log pressionamentos de teclas ou utilizar qualquer quantidade de outras abordagens para capturar as credenciais que são eficientes no ambiente. Independentemente das táticas, os destinos permanecem os mesmos: contas com privilégios de amplo e profundo.  

Concessão de privilégios excessivos apenas não for encontrado no Active Directory em ambientes comprometidos. Quando uma organização desenvolveu o hábito de concedendo mais privilégios do que é necessário, ele é normalmente encontrado em toda a infraestrutura conforme discutido nas seções a seguir.  

## <a name="in-active-directory"></a>No Active Directory

No Active Directory, é comum encontrar se os grupos EA, acelerador de desenvolvimento e BA contêm números excessivos de contas. Mais comumente, grupo EA de uma organização contém os membros mais baixas, grupos de acelerador de desenvolvimento geralmente contêm um multiplicador do número de usuários no grupo de EA e grupos de administradores geralmente contêm mais membros que as populações de outros grupos combinados. Isso costuma ocorrer devido a uma crença de que os administradores de alguma forma, são "menos privilégios" que DAs ou EAs. Enquanto os direitos e permissões concedidas a cada um desses grupos forem diferentes, eles devem ser efetivamente considerados igualmente grupos poderosos porque um membro de um pode tornar-se um membro dos outros dois.  

## <a name="on-member-servers"></a>Em servidores membros

Quando podemos recuperar a associação de grupos de administradores locais em servidores membros em muitos ambientes, descobrimos que a associação que variam de um punhado de contas locais e de domínio, a dezenas de grupos aninhados que, quando expandida, revelar centenas, até mesmo milhares, de contas com privilégio de administrador local nos servidores. Em muitos casos, os grupos de domínio com grandes associações estão aninhados em grupos locais de administradores servidores de membro, sem considerar o fato de que qualquer usuário quem pode modificar as associações desses grupos no domínio pode obter controle administrativo de todos os sistemas no qual o grupo tem sido aninhado em um grupo de administradores local.  

## <a name="on-workstations"></a>Em estações de trabalho

Embora estações de trabalho normalmente têm muito menos membros em seus grupos de administradores locais do que servidores membro, em muitos ambientes, os usuários recebem a associação no grupo de administradores local em seus computadores pessoais. Quando isso ocorre, mesmo que o UAC está habilitado, os usuários apresentam um risco com privilégios elevados para a integridade de suas estações de trabalho.  

> [!IMPORTANT]  
> Você deve considerar cuidadosamente se os usuários exigem direitos administrativos em suas estações de trabalho, e se Sim, pode ser uma abordagem melhor criar uma conta local separada no computador que seja membro do grupo Administradores. Quando os usuários exigem elevação, eles podem apresentar as credenciais dessa conta local, elevação, mas porque a conta for local, ele não pode ser usado para comprometer outros computadores ou acessar recursos de domínio. Assim como acontece com todas as contas locais, no entanto, as credenciais para a conta com privilégios local devem ser exclusivas; Se você criar uma conta local com as mesmas credenciais em várias estações de trabalho, você pode expor os computadores para ataques pass-the-hash.  

## <a name="in-applications"></a>Em aplicativos

Em ataques em que o destino é de propriedade intelectual da organização, as contas que receberam privilégios avançados dentro dos aplicativos podem ser direcionadas para permitir a extração de dados. Embora as contas que têm acesso a dados confidenciais podem ter sido concedidas sem privilégios elevados no domínio ou o sistema operacional, as contas que podem manipular a configuração de um aplicativo ou fornece acesso às informações do aplicativo risco presente.  

## <a name="in-data-repositories"></a>Em repositórios de dados

Como acontece com outros destinos, os invasores buscando obter acesso à propriedade intelectual na forma de documentos e outros arquivos podem direcionar as contas que controle o acesso ao arquivo armazena contas que têm acesso direto aos arquivos, ou até mesmo grupos ou funções que têm acesso a arquivos. Por exemplo, se um servidor de arquivos é usado para armazenar documentos de contrato e o acesso é concedido aos documentos com o uso de um grupo do Active Directory, um invasor que pode modificar a associação do grupo pode adicionar contas comprometidas ao grupo e acessar o contrato documentos. Em casos em que o acesso a documentos é fornecido por aplicativos como o SharePoint, os invasores podem direcionar os aplicativos, conforme descrito anteriormente.  

## <a name="reducing-privilege"></a>Reduzindo privilégios

O maior e mais complexo, um ambiente mais difícil será gerenciar e proteger. Em organizações de pequenas porte, revisando e reduzindo o privilégio podem ser uma proposta relativamente simples, mas cada servidor adicional, a estação de trabalho, a conta de usuário e o aplicativo em uso em uma organização adiciona outro objeto que deve ser protegido. Como pode ser difícil ou até mesmo impossível corretamente proteger todos os aspectos de uma organização a infra-estrutura de TI, você deve se concentrar os esforços pela primeira vez em contas, cujo privilégio de criar o maior risco, que normalmente são contas com privilégios internos e grupos no Active Directory e contas privilegiadas de locais nos servidores de membro e estações de trabalho.  

### <a name="securing-local-administrator-accounts-on-workstations-and-member-servers"></a>Protegendo contas de Administrador Local nos servidores de membro e estações de trabalho

Embora este documento se concentra na segurança do Active Directory, conforme discutido anteriormente, a maioria dos ataques contra o diretório começam como ataques contra hosts individuais. Diretrizes de completas para proteger grupos locais em sistemas de membro não podem ser fornecidas, mas as recomendações a seguir podem ser usadas para ajudar a proteger as contas de administrador locais nos servidores de membro e estações de trabalho.  

#### <a name="securing-local-administrator-accounts"></a>Protegendo contas de Administrador Local

Em todas as versões do Windows atualmente em suporte base, a conta de administrador local está desabilitada por padrão, o que torna a conta inutilizável para pass-the-hash e outros ataques de roubo de credenciais. No entanto, em domínios que contêm os sistemas operacionais herdados ou em quais contas de administrador locais foram habilitadas, essas contas podem ser usadas conforme descrito anteriormente para propagar o comprometimento entre servidores membro e estações de trabalho. Por esse motivo, os controles a seguir são recomendados para todas as contas de administrador locais nos sistemas ingressados em domínio.  

Instruções detalhadas para implementar esses controles são fornecidas no [apêndice h: Protegendo grupos e contas de Administrador Local](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md). Antes de implementar essas configurações, no entanto, certifique-se de que as contas de administrador locais não são atualmente usadas no ambiente para executar serviços em computadores ou realizar outras atividades para as quais essas contas não devem ser usadas. Teste essas configurações antes de implementá-las em um ambiente de produção.  

#### <a name="controls-for-local-administrator-accounts"></a>Controles para contas de Administrador Local

Contas de administrador interno nunca devem ser usadas como contas de serviço em servidores membros, nem deve ser usados para fazer logon computadores locais (exceto no modo de segurança, que é permitido mesmo que a conta está desabilitada). O objetivo de implementar as configurações descritas aqui é impedir que conta de administrador local de cada computador que está sendo usado, a menos que os controles de proteção pela primeira vez são revertidos. Implementando esses controles e monitoramento de contas de administrador para que as alterações, você pode reduzir significativamente a probabilidade de êxito de um ataque que contas de administrador local de destinos.  

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-joined-systems"></a>Configurando GPOs para restringir contas de administrador em sistemas ingressados em domínio

Em um ou mais GPOs que você crie e vincule ao servidor de estação de trabalho e membro UOs em cada domínio, adicione a conta de administrador para os seguintes direitos de usuário no **Policies\ de configurações do computador \ Diretivas \ Configurações de segurança Atribuições de direitos do usuário**:  

- Negar o acesso a este computador a partir da rede
- Negar o logon como um trabalho em lotes
- Negar o logon como um serviço
- Negar o logon por meio dos Serviços de Área de Trabalho Remota

Quando você adiciona as contas de administrador a esses direitos de usuário, especifique se você está adicionando a conta de administrador local ou o administrador do domínio da conta, a propósito que você rotular a conta. Por exemplo adicionar a conta de administrador do domínio NWTRADERS esses negar direitos, você digitará a conta como **NWTRADERS\Administrador**, ou navegue até a conta de administrador no domínio NWTRADERS. Para garantir que você restrinja a conta de administrador local, digite **administrador** nessas configurações no Editor de objeto de diretiva de grupo de direitos do usuário.  

> [!NOTE]  
> Mesmo se as contas de administrador locais são renomeadas, as políticas ainda serão aplicadas.  

Essas configurações garantirão que conta de administrador do computador não pode ser usada para se conectar a outros computadores, mesmo se ele estiver inadvertida ou mal-intencionada habilitado. Os logons locais usando a conta de administrador local não pode ser completamente desabilitados, nem deve tentar fazer isso, porque a conta de administrador local do computador foi projetada para ser usado em cenários de recuperação de desastres.  

Deve um servidor membro ou estação de trabalho se tornar retirada do domínio com nenhum outro privilégio concedido administrativo de contas locais, o computador pode ser inicializado no modo de segurança, a conta de administrador pode ser habilitada e a conta, em seguida, pode ser usado para o efeito reparos no computador. Quando reparos forem concluídos, a conta de administrador deve ser desabilitada novamente.  

### <a name="securing-local-privileged-accounts-and-groups-in-active-directory"></a>Proteger contas privilegiadas locais e grupos no Active Directory

*Lei número seis: Um computador só é tão seguro quanto o administrador é confiável.* - [10 leis imutáveis de segurança (versão 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  

As informações fornecidas aqui destina-se a fornecer diretrizes gerais para proteger as contas internas de privilégio mais altas e grupos no Active Directory. Instruções passo a passo detalhadas também são fornecidas no [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md), [apêndice e: Protegendo grupos de administrador corporativo no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md), [apêndice f: Protegendo grupos de administradores de domínio do Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)e, em [apêndice g: Protegendo grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md).  

Antes de implementar qualquer uma dessas configurações, você também deve testar todas as configurações cuidadosamente para determinar se elas são adequadas para seu ambiente. Nem todas as organizações poderão implementar essas configurações.  

#### <a name="securing-built-in-administrator-accounts-in-active-directory"></a>Protegendo contas de administrador internas no Active Directory

Em cada domínio no Active Directory, uma conta de administrador é criada como parte da criação do domínio. Essa conta é um membro dos grupos de administrador no domínio e Admins. do domínio padrão e se o domínio for o domínio raiz da floresta, a conta também é membro do grupo Administradores corporativos. Uso da conta de administrador local do domínio deve ser reservado apenas para atividades de compilação inicial e, possivelmente, cenários de recuperação de desastres. Para garantir que uma conta de administrador interna pode ser usada para efetuar reparos que nenhuma outra conta pode ser usada, você não deve alterar a associação padrão da conta de administrador em qualquer domínio na floresta. Em vez disso, você deve seguir as diretrizes para ajudar a proteger a conta de administrador em cada domínio na floresta. Instruções detalhadas para implementar esses controles são fornecidas no [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para contas de administrador interno

O objetivo de implementar as configurações descritas aqui é impedir que a conta de administrador de cada domínio (não um grupo) seja utilizável, a menos que uma série de controles é revertida. Implementando esses controles e as contas de administrador para que as alterações de monitoramento, você pode reduzir significativamente a probabilidade de um ataque bem-sucedido, aproveitando a conta de administrador do domínio. Para a conta de administrador em cada domínio na floresta, você deve configurar as configurações a seguir.  

##### <a name="enable-the-account-is-sensitive-and-cannot-be-delegated-flag-on-the-account"></a>Habilitar o sinalizador "Conta é sigilosa e não pode ser delegada" na conta

Por padrão, todas as contas no Active Directory podem ser delegadas. A delegação permite que um computador ou serviço para apresentar as credenciais para uma conta que foi autenticado no computador ou em outros computadores para obter serviços em nome da conta. Quando você habilita o **conta é sigilosa e não pode ser delegada** atributo em uma conta de domínio, as credenciais da conta não podem ser apresentadas para outros computadores ou serviços na rede, o que limita os ataques que utilizam o delegação para usar as credenciais da conta em outros sistemas.  

##### <a name="enable-the-smart-card-is-required-for-interactive-logon-flag-on-the-account"></a>Habilitar o sinalizador "cartão inteligente é necessário para logon interativo" na conta

Quando você habilita o **cartão inteligente é necessário para logon interativo** atributo em uma conta, o Windows redefine a senha da conta para um valor aleatório de 120 caracteres. Ao definir esse sinalizador em contas de administrador internas, você garante que a senha para a conta é não apenas longo e complexo, mas não sejam conhecida por qualquer usuário. Não é tecnicamente necessário criar cartões inteligentes para as contas antes de habilitar esse atributo, mas se possível, os cartões inteligentes devem ser criados para cada conta de administrador antes de configurar as restrições de conta e os cartões inteligentes devem ser armazenados em locais seguros.  

Embora definir a **cartão inteligente é necessário para logon interativo** sinalizador redefine a senha da conta, ela não impede que um usuário com direitos para redefinir a senha da conta do definindo a conta para um valor conhecido e usando o nome da conta e nova senha para acessar recursos na rede. Por isso, você deve implementar os seguintes controles adicionais na conta.  

##### <a name="configuring-gpos-to-restrict-domains-administrator-accounts-on-domain-joined-systems"></a>Configurando GPOs para restringir de contas de administrador dos domínios em sistemas ingressados em domínio

Embora desabilitar a conta de administrador em um domínio inutiliza a conta com eficiência, você deve implementar restrições adicionais na conta, caso a conta esteja habilitada de maneira inadvertida ou mal-intencionada. Embora esses controles, por fim, podem ser revertidos pela conta de administrador, a meta é criar controles que mais lento o andamento de um invasor e limitar os danos a conta, poderá causar.  

Em um ou mais GPOs que você crie e vincule ao servidor UOs em cada domínio de estação de trabalho e o membro, adicionar a conta de administrador de cada domínio para os seguintes direitos de usuário no **configurações do computador \ Diretivas \ Configurações de segurança Atribuições de direitos de atribuição**:  

- Negar o acesso a este computador a partir da rede  
- Negar o logon como um trabalho em lotes  
- Negar o logon como um serviço  
- Negar o logon por meio dos Serviços de Área de Trabalho Remota  

> [!NOTE]  
> Quando você adiciona as contas de administrador locais para essa configuração, você deve especificar se você estiver configurando as contas de administrador locais ou contas de administrador de domínio. Por exemplo adicionar a conta de administrador local do domínio NWTRADERS esses negar direitos, você deve digitar a conta como **NWTRADERS\Administrador**, ou navegue até a conta de administrador local para o domínio NWTRADERS. Se você digitar **administrador** nessas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado.  
>
> É recomendável restringir contas de administrador locais nos servidores de membro e estações de trabalho da mesma maneira como as contas de administrador baseado em domínio. Portanto, você deve adicionar geralmente a conta de administrador para cada domínio na floresta e a conta de administrador para os computadores locais a essas configurações de direitos de usuário. 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-controllers"></a>Configurando GPOs para restringir as contas de administrador em controladores de domínio

Em cada domínio na floresta, a política de controladores de domínio padrão ou uma política vinculada à UO controladores de domínio deve ser modificada para adicionar a conta de administrador de cada domínio para os seguintes direitos de usuário no **Configuration\Policies\ do computador Atribuições de direitos do Windows \ Configurações de segurança locais \ atribuição**:  

- Negar o acesso a este computador a partir da rede  
- Negar o logon como um trabalho em lotes  
- Negar o logon como um serviço  
- Negar o logon por meio dos Serviços de Área de Trabalho Remota  

> [!NOTE]  
> Essas configurações garantirão que a conta de administrador local não pode ser usada para se conectar a um controlador de domínio, embora a conta, se habilitado, pode fazer logon localmente para controladores de domínio. Porque essa conta só deve estar habilitada e usada em cenários de recuperação de desastres, espera-se que o acesso físico a pelo menos um controlador de domínio estarão disponível ou que outras contas com permissões para acessar os controladores de domínio remotamente podem ser usado.  

##### <a name="configure-auditing-of-built-in-administrator-accounts"></a>Configurar a auditoria de contas de administrador interno

Quando você tiver protegido cada conta de administrador do e desabilitados, você deve configurar a auditoria para monitorar as alterações na conta. Se a conta está habilitada, sua senha for redefinida ou quaisquer outras modificações são feitas para a conta, alertas devem ser enviados aos usuários ou equipes responsáveis pela administração do AD DS, além das equipes de resposta a incidentes da sua organização.  

### <a name="securing-administrators-domain-admins-and-enterprise-admins-groups"></a>Protegendo a administradores, Admins. do domínio e grupos de administrador corporativo

#### <a name="securing-enterprise-admin-groups"></a>Protegendo grupos de administradores de empresa

O grupo de administradores de empresa, que está hospedado no domínio raiz da floresta, não deve conter nenhum usuário no dia a dia, com a possível exceção da conta de administrador local do domínio, desde que ele é protegido conforme descrito anteriormente e em [ Apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Quando for necessário o acesso EA, os usuários cujas contas exigem permissões e direitos EA devem ser colocados temporariamente para o grupo de administradores de empresa. Embora os usuários estão usando contas altamente privilegiadas, suas atividades devem ser auditadas e preferencialmente executadas com um usuário que está executando as alterações e outro usuário observar as alterações para minimizar a probabilidade de uso indevido acidental ou configuração incorreta . Quando as atividades tiverem sido concluídas, as contas devem ser removidas do grupo de EA. Isso pode ser obtido por meio de procedimentos manuais e documentadas processos, software PIM/do PAM (gerenciamento) de identidade/acesso privilegiado de terceiros ou uma combinação de ambos. Diretrizes para criação de contas que pode ser usado para controlar a associação de grupos privilegiados no Active Directory são fornecidas nos [contas atraentes para roubo de credenciais](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md) e instruções detalhadas são fornecidas no [Apêndice i: Criação de contas de gerenciamento para protegidos em grupos e contas no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Administradores de empresa são, por padrão, os membros do grupo Administradores interno em cada domínio na floresta. Removendo o grupo de administradores de empresa dos grupos de administradores em cada domínio é uma modificação inadequada, como no caso de um cenário de recuperação de desastres de floresta, os direitos EA provavelmente será necessários. Se tiver sido removido do grupo Administradores corporativos de grupos de administradores em uma floresta, ele deve ser adicionado ao grupo de administradores em cada domínio e os seguintes controles adicionais devem ser implementados:  

- Conforme descrito anteriormente, o grupo de administradores de empresa não deve conter nenhum usuário no dia a dia, com a possível exceção de conta de administrador do domínio raiz da floresta, que deve ser protegida conforme descrito em [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
- Nos GPOs vinculados a OUs que contenham servidores membro e estações de trabalho em cada domínio, o grupo EA deve ser adicionado para os seguintes direitos de usuário:  
   - Negar o acesso a este computador a partir da rede  
   - Negar o logon como um trabalho em lotes  
   - Negar o logon como um serviço  
   - Negar o logon localmente  
   - Nega logon pelos serviços de área de trabalho remota.  

Isso impedirá que os membros do grupo EA fazendo logon em estações de trabalho e servidores membro. Se os servidores de salto são usadas para administrar controladores de domínio e o Active Directory, certifique-se de que os servidores de salto estão localizados em uma unidade Organizacional ao qual os GPOs restritivos não estão vinculados.  

- A auditoria deve ser configurada para enviar alertas se todas as modificações são feitas para as propriedades ou a associação do grupo EA. Esses alertas devem ser enviados, no mínimo, a usuários ou as equipes responsáveis por resposta de incidente e administração do Active Directory. Você também deve definir processos e procedimentos para temporariamente popular o grupo EA, incluindo procedimentos de notificação quando legítimos população do grupo será executada.  
  
#### <a name="securing-domain-admins-groups"></a>Protegendo grupos de administradores de domínio

Como é o caso com o grupo de administradores de empresa, a associação em grupos de Admins. do domínio deve ser necessária apenas em cenários de compilação ou recuperação de desastres. Não deve haver nenhuma conta de usuário diárias no grupo de acelerador de desenvolvimento com a exceção a conta de administrador local para o domínio, se ele foi protegido conforme descrito em [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Quando for necessário o acesso de acelerador de desenvolvimento, as contas que precisam desse nível de acesso devem ser colocadas temporariamente no grupo de acelerador de desenvolvimento para o domínio em questão. Embora os usuários estão usando contas altamente privilegiadas, atividades devem ser auditadas e preferencialmente executadas com um usuário que está executando as alterações e outro usuário observar as alterações para minimizar a probabilidade de uso indevido acidental ou configuração incorreta. Quando as atividades tiverem sido concluídas, as contas devem ser removidas do grupo Admins. do domínio. Isso pode ser obtido por meio de procedimentos manuais e documentadas processos por meio do software do PAM (gerenciamento PIM /) de identidade/acesso privilegiado de terceiros, ou uma combinação de ambos. Diretrizes para criação de contas que pode ser usado para controlar a associação de grupos privilegiados no Active Directory são fornecidas no [i Apêndice: Criação de contas de gerenciamento para protegidos em grupos e contas no Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Os administradores de domínio são, por padrão, os membros dos grupos Administradores locais em todos os servidores membro e estações de trabalho em seus respectivos domínios. O aninhamento esse padrão não deve ser modificado porque ele afeta as opções de recuperação de desastres e a capacidade de suporte. Se os grupos de Admins. do domínio foram removidos dos grupos Administradores locais nos servidores membro, elas devem ser adicionadas ao grupo de administradores em cada servidor membro e a estação de trabalho no domínio por meio das configurações de grupo restrito em GPOs vinculados. Os seguintes controles gerais, que são descritos em detalhes na [apêndice f: Protegendo grupos Admins. do domínio do Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md) também deve ser implementada.  

Para o grupo Admins. do domínio em cada domínio na floresta:  

1. Remova todos os membros do grupo de acelerador de desenvolvimento, com a possível exceção da conta interna de administrador para o domínio, desde que ele foi protegido conforme descrito em [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
2. Nos GPOs vinculados a OUs que contenham servidores membro e estações de trabalho em cada domínio, o grupo DA deve ser adicionado para os seguintes direitos de usuário:  
   - Negar o acesso a este computador a partir da rede  
   - Negar o logon como um trabalho em lotes  
   - Negar o logon como um serviço  
   - Negar o logon localmente  
   - Negar o logon por meio dos Serviços de Área de Trabalho Remota  
  
   Isso impedirá que os membros do grupo DA fazendo logon em estações de trabalho e servidores membro. Se os servidores de salto são usadas para administrar controladores de domínio e o Active Directory, certifique-se de que os servidores de salto estão localizados em uma unidade Organizacional ao qual os GPOs restritivos não estão vinculados.  

3. A auditoria deve ser configurada para enviar alertas se todas as modificações são feitas para as propriedades ou a associação do grupo DA. Esses alertas devem ser enviados, no mínimo, a usuários ou as equipes responsáveis por resposta de incidente e administração do AD DS. Você também deve definir os processos e procedimentos para temporariamente popular o grupo DA, incluindo procedimentos de notificação quando legítimos população do grupo será executada.  

#### <a name="securing-administrators-groups-in-active-directory"></a>Protegendo grupos de administradores no Active Directory

Como acontece com os grupos EA e o Acelerador de desenvolvimento, a associação no grupo de administradores (BA) deve ser necessária somente em cenários de compilação ou recuperação de desastres. Não deve haver nenhuma conta de usuário diárias no grupo Administradores, exceto a conta de administrador local para o domínio, se ele foi protegido conforme descrito em [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Quando for necessário o acesso de administradores, as contas que precisam desse nível de acesso devem ser colocadas temporariamente no grupo de administradores do domínio em questão. Embora os usuários estão usando contas altamente privilegiadas, atividades devem ser auditadas e, preferencialmente, executadas com um usuário realizando as alterações e outro usuário observar as alterações para minimizar a probabilidade de uso indevido acidental ou configuração incorreta. Quando as atividades tiverem sido concluídas, as contas imediatamente devem ser removidas do grupo Administradores. Isso pode ser obtido por meio de procedimentos manuais e documentadas processos por meio do software do PAM (gerenciamento PIM /) de identidade/acesso privilegiado de terceiros, ou uma combinação de ambos.  

Os administradores são, por padrão, os proprietários da maioria dos objetos do AD DS em seus respectivos domínios. Associação neste grupo pode ser necessário em cenários de recuperação de desastre e de compilação no qual a propriedade ou a capacidade de assumir a propriedade de objetos é necessária. Além disso, DAs e EAs herdar um número de seus direitos e permissões por meio de sua associação padrão no grupo de administradores. Padrão de aninhamento de grupo para grupos privilegiados no Active Directory não devem ser modificados e grupo de administradores de cada domínio deve ser protegido conforme descrito em [apêndice g: Protegendo grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)e nas instruções gerais abaixo.  

1. Remova todos os membros do grupo de administradores, com a possível exceção da conta de administrador local para o domínio, desde que ele foi protegido conforme descrito em [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
2. Membros do grupo de administradores de domínio nunca devem precisar fazer logon em servidores membros ou estações de trabalho. Em um ou mais GPOs vinculados ao servidor de estação de trabalho e membro UOs em cada domínio, o grupo de administradores deve ser adicionado para os seguintes direitos de usuário:  
   - Negar o acesso a este computador a partir da rede  
   - Negar logon como um trabalho em lotes,  
   - Negar o logon como um serviço  
   - Isso impedirá que os membros do grupo Administradores do que está sendo usado para fazer logon ou conecte-se aos servidores membros ou estações de trabalho (a menos que vários controles foram violados primeiro), em que suas credenciais poderiam ser armazenado em cache e comprometidas assim. Uma conta privilegiada nunca deve ser usada para fazer logon um sistema com menos privilégios e imposição desses controles oferece proteção contra um número de ataques.  

3. AT os controladores de domínio OU em cada domínio na floresta, o grupo de administradores deve receber o seguinte usuário direitos (se eles ainda não tiver esses direitos), que permitirá que os membros do grupo Administradores para executar funções necessárias para um cenário de recuperação de desastre em toda a floresta:  
   - Acesso a este computador da rede  
   - Permitir logon localmente  
   - Permitir logon por meio dos Serviços de Área de Trabalho Remota  

4. A auditoria deve ser configurada para enviar alertas se todas as modificações são feitas para as propriedades ou a associação do grupo Administradores. Esses alertas devem ser enviados, no mínimo, a membros da equipe responsável pela administração do AD DS. Alertas também devem ser enviados para os membros da equipe de segurança e procedimentos devem ser definidos para modificar a associação do grupo Administradores. Especificamente, esses processos devem incluir um procedimento pelo qual a equipe de segurança é notificada quando o grupo de administradores está prestes a ser modificado de forma que quando os alertas são enviados, eles são esperados e não será gerado um alarme. Além disso, os processos para notificar a equipe de segurança quando o uso do grupo Administradores foi concluído e as contas usadas foram removidas do grupo devem ser implementados.  

> [!NOTE]  
> Quando você implementa restrições no grupo de administradores em GPOs, o Windows aplicam as configurações para os membros do grupo de administradores local do computador além do grupo de administradores do domínio. Portanto, você deve usar o cuidado ao implementar restrições no grupo de administradores. Embora proibir logons de rede, o lote e o serviço para os membros do grupo Administradores é aconselhável onde quer que seja viável para implementação, não restringir logons locais ou logons por meio dos serviços de área de trabalho remota. Esses tipos de logon de bloqueio pode bloquear administração legítima de um computador por membros do grupo Administradores local. Captura de tela a seguir mostra as definições de configuração que bloqueiam o uso indevido do interna local e domínio da conta do administrador, além de uso indevido dos grupos internos de administradores de domínio ou local. Observe que o **Negar logon pelos serviços de área de trabalho remota** direito de usuário não inclui o grupo de administradores, porque os incluí-lo nesta configuração também bloqueia esses logons para contas que são membros do computador local Grupo de administradores. Se serviços em computadores são configurados para executar no contexto de qualquer um dos grupos privilegiados descritos nesta seção, implementar essas configurações pode causar falhas em aplicativos e serviços. Portanto, assim como acontece com todas as recomendações nesta seção, você deve testar as configurações de aplicabilidade no seu ambiente.  
>
> ![modelos de administração de privilégio mínimo](media/Implementing-Least-Privilege-Administrative-Models/SAD_3.gif)  

### <a name="role-based-access-controls-rbac-for-active-directory"></a>Controles de acesso baseado em função (RBAC) para o Active Directory

Em termos gerais, controles de acesso baseado em função (RBAC) são um mecanismo para agrupar usuários e fornecer acesso a recursos com base nas regras de negócios. No caso do Active Directory, implementar o RBAC para o AD DS é o processo de criação de funções para o qual os direitos e permissões são delegadas para permitir que os membros da função executar tarefas administrativas diárias sem conceder privilégios excessivos. O RBAC para o Active Directory pode ser projetado e implementado por meio de interfaces e a capacidade das ferramentas nativa, aproveitando o software que você talvez já possui, adquirindo produtos de terceiros, ou qualquer combinação dessas abordagens. Esta seção fornece instruções passo a passo para implementar o RBAC para o Active Directory, mas em vez disso, discute os fatores que você deve considerar na escolha de uma abordagem para implementar o RBAC em suas instalações do AD DS.  

#### <a name="native-approaches-to-rbac-for-active-directory"></a>Abordagens de nativas para o RBAC para o Active Directory

Na implementação mais simples de RBAC, você pode implementar funções como grupos do AD DS e delegar direitos e permissões para os grupos que lhes permitem realizar a administração diária dentro do escopo designado da função.  

Em alguns casos, os grupos de segurança existentes no Active Directory podem ser usados para conceder direitos e permissões apropriadas para uma função de trabalho. Por exemplo, se os funcionários específicos na sua organização de TI serão responsáveis para o gerenciamento e manutenção de registros e zonas DNS, delegar essas responsabilidades pode ser tão simple quanto criar uma conta para cada administrador DNS e adicioná-lo para o DNS Grupo de administradores no Active Directory. O grupo de administradores de DNS, ao contrário de grupos mais altamente privilegiados, tem alguns direitos poderosos entre o Active Directory, embora os membros desse grupo têm sido delegadas permissões que lhes permitem administrar o DNS e é ainda sujeito a comprometimento e abuso pode resultar em elevação de privilégio.

Em outros casos, talvez você precise criar grupos de segurança e delegar direitos e permissões para objetos do Active Directory, objetos do sistema de arquivos e objetos do registro para permitir que os membros dos grupos para executar tarefas administrativas designadas. Por exemplo, se seus operadores de suporte técnico são responsáveis por redefinir senhas esquecidas, ajudando os usuários com problemas de conectividade e solução de problemas de configurações do aplicativo, você precisará combinar as configurações de delegação em objetos de usuário no Active Directory com privilégios que permitem aos usuários de assistência técnica para se conectar remotamente aos computadores de usuários para exibir ou modificar definições de configuração dos usuários. Para cada função que você definir, você deve identificar:  

1. Executam quais tarefas os membros da função no dia a dia e quais tarefas são executadas com menos frequência.  
2. Em quais sistemas e em que os aplicativos devem ter membros de uma função direitos e permissões.  
3. Quais usuários devem receber a associação em uma função.  
4. Como o gerenciamento de associações de função será executado.  

Em muitos ambientes, a criação manual de controles de acesso baseado em função para a administração de um ambiente do Active Directory pode ser um desafio para implementar e manter. Se você tiver definido claramente as funções e responsabilidades para administração de sua infraestrutura de TI, você talvez queira aproveitar ferramentas adicionais para ajudar na criação de uma implantação de RBAC nativo gerenciável. Por exemplo, se o Forefront Identity Manager (FIM) está em uso no seu ambiente, você pode usar o FIM para automatizar a criação e população de funções administrativas, que pode facilitar a administração contínua. Se você usar o System Center Configuration Manager (SCCM) e o System Center Operations Manager (SCOM), você pode usar funções específicas do aplicativo para delegar o gerenciamento e monitoramento de funções e também impor a consistência da configuração e a auditoria nos sistemas em o domínio. Se você tiver implementado uma infraestrutura de chave pública (PKI), você pode emitir e exigir cartões inteligentes para a equipe de TI responsável por administrar o ambiente. Com o gerenciamento de credencial de FIM (FIM CM), você pode até combinar o gerenciamento de funções e as credenciais para sua equipe administrativa.  

Em outros casos, pode ser preferível para uma organização a considerar a implantação de software RBAC de terceiros que fornece a funcionalidade de "out-of-box". Soluções (COTS) prontas para uso comerciais para o RBAC para Active Directory, Windows e os diretórios não Windows e sistemas operacionais são oferecidas por um número de fornecedores. Ao escolher entre soluções nativas e produtos de terceiros, você deve considerar os seguintes fatores:  

1. Orçamento: Ao investir no desenvolvimento de RBAC usando ferramentas que você já pode ter software, você pode reduzir os custos de software envolvidos na implantação de uma solução. No entanto, a menos que você tenha da equipe com experiência na criação e implantação de soluções nativas de RBAC, você precisa envolver recursos de consultoria para desenvolver sua solução. Você deve avaliar com cuidado os custos antecipados para uma solução personalizado com os custos para implantar uma solução de "out-of-box", especialmente se seu orçamento é limitado.  
2. Composição do ambiente de TI: Se seu ambiente é composto principalmente de sistemas Windows, ou se você já está aproveitando o Active Directory para gerenciamento de sistemas não Windows e contas, soluções nativas personalizadas podem fornecer a solução ideal para suas necessidades. Se sua infra-estrutura contém muitos sistemas que não estão executando o Windows e não são gerenciados pelo Active Directory, você precisa considerar as opções para gerenciamento de sistemas não Windows separadamente do ambiente do Active Directory.  
3. Modelo de privilégio da solução: Se um produto depende do posicionamento de suas contas de serviço em grupos altamente privilegiados no Active Directory e não as opções de oferta que não exigem privilégios excessivos concedidas para o software RBAC, não realmente reduziu seu ataque do Active Directory superfície você apenas alterou a composição dos grupos mais privilegiadas no diretório. A menos que um fornecedor de aplicativos pode fornecer controles para contas de serviço que minimizar a probabilidade de contas que estão sendo comprometida e usados maliciosamente, você poderá considerar outras opções.  

### <a name="privileged-identity-management"></a>Gerenciamento de identidades com privilégios

Com privilégios de gerenciamento de identidade (PIM), às vezes, conhecido como gerenciamento de conta privilegiada (PAM) ou gerenciamento de credenciais com privilégios (PCM) é o design, construção, e a implementação de abordagens para gerenciar com privilégios contas no seu infraestrutura. Em termos gerais, o PIM fornece mecanismos pelos quais contas são concedidas direitos de temporários e as permissões necessárias para executar a compilação ou interrupção corrigir funções, em vez de deixar permanentemente associados a contas de privilégios. Se a funcionalidade do PIM é criada manualmente ou é implementada por meio de implantação do software de terceiros um ou mais dos seguintes recursos pode estar disponível:  
  
- A credencial "cofres", onde as senhas para contas privilegiadas são "check-out" atribuídas uma senha inicial e "check-in" quando atividades tiverem sido concluídas, momento em que as senhas são redefinidas novamente nas contas.  
- Restrições de limite de tempo sobre o uso de credenciais com privilégios  
- Uma vez usar credenciais  
- Gerado pelo fluxo de trabalho de concessão de privilégio com monitoramento e emissão de relatórios de atividades executadas e remoção automática de privilégio quando atividades forem concluídas ou alocadas tempo expirou  
- Substituição de credenciais embutidas, como nomes de usuário e senhas em scripts com interfaces de programação de aplicativo (APIs) que permitem que as credenciais a ser recuperado de cofres, conforme necessário  
- Gerenciamento automático de credenciais da conta de serviço  

### <a name="creating-unprivileged-accounts-to-manage-privileged-accounts"></a>Criação de contas sem privilégios para gerenciar contas com privilégios

Um dos desafios no gerenciamento de contas com privilégios é que, por padrão, as contas que pode gerenciar contas com privilégios e protegidas e grupos são privilegiados e contas protegidas. Se você implementar soluções RBAC e PIM apropriadas para sua instalação do Active Directory, as soluções podem incluir as abordagens que permitem que você efetivamente depopulate a associação de grupos mais privilegiadas no diretório, preenchendo apenas os grupos temporariamente e quando necessário.  

Se você implementar o RBAC e PIM nativo, no entanto, você deve considerar a criação de contas que não têm nenhum privilégio e com a única função de preenchimento e depopulating com privilégios de grupos no Active Directory quando necessário. [Apêndice i: Criar contas de gerenciamento para contas e grupos no Active Directory protegidos](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md) fornece instruções passo a passo que você pode usar para criar contas para essa finalidade.  

### <a name="implementing-robust-authentication-controls"></a>Implementar controles de autenticação robusta

*Lei número seis: Realmente existe alguém out lá tentando adivinhar suas senhas.* - [10 leis imutáveis da segurança de administração](https://technet.microsoft.com/library/cc722488.aspx)  

Pass-the-hash e outros ataques de roubo de credenciais não são específicas para sistemas de operacionais do Windows, nem são novos. O ataque pass-the-hash primeiro foi criado em 1997. Historicamente, no entanto, esses ataques necessárias ferramentas personalizadas, foram tanto inexata no seu sucesso e necessário que os invasores têm um relativamente alto nível de habilidade. A introdução das ferramentas disponíveis gratuitamente, fácil de usar que extrai nativamente credenciais resultou em um aumento exponencial no número e o sucesso dos ataques de roubo de credenciais nos últimos anos. No entanto, ataques de roubo de credenciais não são os mecanismos de único pelo qual as credenciais são direcionadas e comprometidas.  

Embora você deve implementar controles para ajudar a proteger você contra ataques de roubo de credenciais, você deve também identifique as contas em seu ambiente que têm mais probabilidade de ser alvo de invasores e implementar controles de autenticação robusta para aqueles contas. Se suas contas privilegiadas mais estiver usando a autenticação de fator único como nomes de usuário e senhas (ambos são "algo que você conhece," que é um fator de autenticação), essas contas são protegidas sem rigidez. Tudo o que um invasor precisa é conhecimento sobre o nome de usuário e conhecimento da senha associada à conta e ataques pass-the-hash não são necessários o invasor pode autenticar como o usuário em todos os sistemas que aceitar credenciais de fator único.  

Embora a implementação da autenticação multifator não protege você contra ataques pass-the-hash, Implementando a autenticação multifator em combinação com sistemas protegidos podem. Para obter mais informações sobre como implementar sistemas protegidos são fornecidas em [implementando proteger Hosts administrativos](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md), e as opções de autenticação são discutidas nas seções a seguir.  

#### <a name="general-authentication-controls"></a>Controles de autenticação geral

Se você já não tiver implementado a autenticação multifator como cartões inteligentes, considere a possibilidade de fazer isso. Cartões inteligentes implementam a proteção imposta por hardware de chaves privadas em um par de chaves públicas-privadas, impedindo que a chave privada do usuário seja acessada ou usada, a menos que o usuário apresente o PIN adequada, senha ou identificador biométrico correto para o cartão inteligente. Mesmo se o PIN ou senha de um usuário é interceptada por um agente de pressionamento de tecla em um computador comprometido, um invasor reutilize o PIN ou senha, o cartão também deve estar fisicamente presente.

Em casos em que senhas longas e complexas comprovaram ser difíceis de implementar devido à resistência do usuário, os cartões inteligentes fornecem um mecanismo pelo qual os usuários podem implementar relativamente simples PINs ou senhas sem as credenciais estejam suscetíveis a força bruta ou tabela arco-íris. PINs dos cartões inteligentes não são armazenadas no Active Directory ou em bancos de dados SAM locais, embora os hashes de credencial ainda possam ser armazenados na memória protegida por LSASS em computadores em que foram usados cartões inteligentes para autenticação.  

#### <a name="additional-controls-for-vip-accounts"></a>Controles adicionais para contas de VIP

Outro benefício da implementação de cartões inteligentes ou outros mecanismos de autenticação baseada em certificado é a capacidade de aproveitar a garantia do mecanismo de autenticação para proteger dados confidenciais que seja acessíveis para usuários VIP. Garantia do mecanismo de autenticação está disponível em domínios em que o nível funcional é definido para o Windows Server 2012 ou Windows Server 2008 R2. Quando habilitada, garantia do mecanismo de autenticação adiciona uma associação de grupo global administrador designado para um token do usuário Kerberos quando as credenciais do usuário são autenticadas durante o logon usando um método de logon baseada em certificado.  

Isso possibilita que os administradores de recursos controlar o acesso a recursos, como arquivos, pastas e impressoras, com base em se o usuário fizer logon usando um método de logon baseada em certificado, além do tipo de certificado usado. Por exemplo, quando um usuário faz logon usando um cartão inteligente, o acesso do usuário aos recursos da rede pode ser especificado como diferentes do que o acesso é quando o usuário não usa um cartão inteligente (ou seja, quando o usuário faz logon, inserindo um nome de usuário e senha). Para obter mais informações sobre a garantia do mecanismo de autenticação, consulte o [garantia do mecanismo de autenticação do AD DS no guia passo a passo do Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897.aspx).  

#### <a name="configuring-privileged-account-authentication"></a>Configurando a autenticação de conta privilegiada

No Active Directory para todas as contas administrativas, habilitar o **requer um cartão inteligente para logon interativo** atributo e auditoria para que as alterações (no mínimo), qualquer um dos atributos na **conta** guia para a conta (por exemplo, cn, nome, sAMAccountName, userPrincipalName e userAccountControl) os objetos de usuário administrativo.  

Embora definir a **requer um cartão inteligente para logon interativo** em contas redefine a senha da conta para um valor aleatório de 120 caracteres e requer cartões inteligentes para logons interativos, a lata de atributo ainda ser substituído por os usuários com permissões que permitem alterar as senhas nas contas e as contas, em seguida, podem ser usados para estabelecer os logons não interativa com apenas o nome de usuário e a senha.  

Em outros casos, dependendo da configuração de contas nas configurações do Active Directory e o certificado em serviços de certificados do Active Directory (AD CS) ou uma PKI de terceiros, o nome Principal do usuário (UPN) atributos para administrativas ou contas VIP podem ser direcionadas para um tipo específico de ataque, conforme descrito aqui.  

##### <a name="upn-hijacking-for-certificate-spoofing"></a>Sequestro de UPN para a falsificação de certificado

Embora uma discussão mais aprofundada de ataques contra infraestruturas de chave pública (PKI) está fora do escopo deste documento, ataques contra PKIs públicas e privadas aumentaram exponencialmente desde 2008. Violações de PKIs públicas foram divulgadas em larga escala, mas ataques contra PKI interna de uma organização são talvez ainda mais produtivo. Um ataque desse tipo aproveita o Active Directory e certificados para permitir que um invasor falsificar as credenciais de outras contas de forma que podem ser difíceis de detectar.  

Quando um certificado é apresentado para a autenticação em um sistema de domínio, o conteúdo do assunto ou o atributo de nome alternativo da entidade (SAN) no certificado é usado para mapear o certificado para um objeto de usuário no Active Directory. Dependendo do tipo de certificado e como ele é criado, o atributo da entidade em um certificado normalmente contém CN (nome comum), um usuário conforme mostrado na seguinte captura de tela.  

![modelos de administração de privilégio mínimo](media/Implementing-Least-Privilege-Administrative-Models/SAD_4.gif)  

Por padrão, o Active Directory constrói CN um usuário concatenando o nome da conta + "" + Sobrenome. No entanto, componentes CN de objetos de usuário no Active Directory não são necessários ou exclusivo, e mover uma conta de usuário para um local diferente no diretório altera o nome da conta diferenciado (DN), que é o caminho completo para o objeto do diretório, conforme mostrado no painel inferior da captura de tela anterior.  

Porque não há garantia de que nomes de entidade do certificado ser estático ou exclusivo, o conteúdo de que o nome alternativo da entidade é geralmente usado para localizar o objeto de usuário no Active Directory. O atributo de SAN para certificados emitidos para usuários empresariais das autoridades de certificação (CAs integradas ao Active Directory) normalmente contém o endereço de email ou o UPN do usuário. Porque UPNs têm garantia de ser exclusivo em uma floresta do AD DS, a localização de um objeto de usuário por UPN normalmente é feito como parte da autenticação, com ou sem envolvidos no processo de autenticação de certificados.  

O uso de UPNs em atributos de SAN em certificados de autenticação pode ser aproveitado por invasores para obter certificados fraudulentos. Se um invasor comprometeu a uma conta que tenha a capacidade de ler e gravar UPNs em objetos de usuário, o ataque é implementado da seguinte maneira:  

O atributo UPN em um objeto de usuário (por exemplo, um usuário VIP) é temporariamente alterado para um valor diferente. O atributo de nome de conta SAM e CN também podem ser alterado no momento, embora normalmente isso não é necessário pelos motivos descritos anteriormente.  

Quando o atributo UPN na conta de destino tiver sido alterado, uma conta de usuário obsoletos, habilitado ou atributo UPN de uma conta usuário recém-criado é alterado para o valor que foi originalmente atribuído à conta de destino. Contas de usuário obsoletos, habilitado são contas que não fizeram logon por longos períodos de tempo, mas não foram desabilitadas. Eles são alvo de invasores que pretendem "Ocultar olhos vistos" pelos seguintes motivos:  

1. Como a conta está habilitada, mas ainda não foram usada recentemente, usando a conta é improvável que disparar alertas da maneira que pode habilitar uma conta de usuário desabilitada.  
2. Uso de uma conta existente não exige a criação de uma nova conta de usuário que pode ser observada por equipe administrativa.  
3. Contas de usuário obsoletos que ainda estão ativadas geralmente são membros de vários grupos de segurança e recebem acesso a recursos da rede, simplificando o acesso e "mistura" para uma população de usuários existentes.  

A conta de usuário no qual o UPN de destino tenha sido configurado é usada para solicitar um ou mais certificados do Active Directory Certificate Services.  

Quando certificados são obtidos para a conta do invasor, os UPNs na conta de "nova" e a conta de destino são retornados para seus valores originais.  

O invasor agora tem um ou mais certificados que podem ser apresentados para autenticação para aplicativos e recursos, como se o usuário é o VIP cuja conta temporariamente foi modificada. Embora uma discussão completa sobre todas as maneiras em que certificados e PKI podem ser alvo de invasores, está fora do escopo deste documento, esse mecanismo de ataque é fornecido para ilustrar por que você deve monitorar privilegiados e contas no AD DS para que as alterações de VIP , particularmente para alterações em qualquer um dos atributos na **conta** guia para a conta (por exemplo, cn, nome, sAMAccountName, userPrincipalName e userAccountControl). Além de monitorar as contas, você deve restringir quem pode modificar as contas a serem tão pequeno que um conjunto de usuários administrativos quanto possível. Da mesma forma, as contas de usuários administrativos devem ser protegidas e monitoradas quanto a alterações não autorizadas.  
