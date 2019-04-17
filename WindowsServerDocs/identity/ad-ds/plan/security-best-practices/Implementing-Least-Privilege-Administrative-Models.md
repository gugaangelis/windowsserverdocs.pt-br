---
ms.assetid: 7a7ab95c-9cb3-4a7b-985a-3fc08334cf4f
title: "Implementando modelos administrativos de privilégios mínimos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9716d442446b2705dfd2803d061cb884a5e72fbe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="implementing-least-privilege-administrative-models"></a>Implementando modelos administrativos de privilégios mínimos

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O trecho a seguir é de [o administrador de contas de segurança guia de planejamento](https://technet.microsoft.com/library/cc162797.aspx), publicado pela primeira vez em 1º de abril de 1999:  
  
> "Mais relacionadas à segurança cursos de treinamento e documentação discutem a implementação de um princípio de privilégios mínimos, mas as organizações raramente seguem. O princípio é simple, e o impacto de aplicá-lo corretamente consideravelmente aumenta a segurança e reduz o risco. O princípio afirma que todos os usuários devem fazer logon com uma conta de usuário que tenha o mínimo necessário de permissões para concluir a tarefa atual e nada mais. Isso oferece proteção contra códigos mal-intencionados, entre outros ataques. Este princípio se aplica a computadores e os usuários desses computadores.   
> "Este princípio funciona tão bem um dos motivos é que ele força a fazer alguma pesquisa interna. Por exemplo, você deve determinar os privilégios de acesso que um computador ou usuário realmente precisa e, em seguida, implementá-los. Para muitas organizações, essa tarefa inicialmente pode parecer uma grande quantidade de trabalho; No entanto, é uma etapa essencial para proteger o seu ambiente de rede com êxito.   
> "Você deve conceder todos os usuários de administradores de domínio seus privilégios de domínio sob o conceito de privilégios mínimos. Por exemplo, se um administrador faz logon com uma conta com privilégios e inadvertidamente executa um programa de vírus, vírus tem acesso administrativo ao computador local e a todo o domínio. Se o administrador em vez disso, tinha logon com uma conta sem privilégios (não administrativa), escopo do vírus danos seria somente no computador local porque ele é executado como um usuário do computador local.   
> "Em outro exemplo, para que você concede direitos de administrador de nível de domínio de contas devem não ter direitos elevados em outra floresta, mesmo se houver uma relação de confiança entre as florestas. Essa tática ajuda a evitar danos amplo se um invasor conseguir comprometer uma floresta gerenciada. As organizações devem realizar auditorias regularmente sua rede para se proteger contra não autorizado escalonamento de privilégios."  
  
O trecho a seguir é do [Microsoft Windows Security Resource Kit](https://www.microsoft.com/learning/en/us/book.aspx?ID=6815&locale=en-us), primeiro 2005 publicado em:  
  
> "Sempre pensar em segurança em termos de conceder o mínimo de privilégios necessários para realizar a tarefa. Se um aplicativo que tenha privilégios muitos deve ser comprometido, o invasor poderá expandir o ataque além do que seria se o aplicativo tivesse sido sob o mínimo de privilégios possíveis. Por exemplo, examine as consequências de um administrador de rede inadvertidamente abrir um anexo de email que inicia um vírus. Se o administrador tiver feito logon usando a conta de administrador do domínio, o vírus tem privilégios de administrador em todos os computadores no domínio e acesso irrestrito, portanto, a praticamente todos os dados na rede. Se o administrador tiver feito logon usando uma conta de administrador local, o vírus terá privilégios de administrador no computador local e, portanto, seria capaz de acessar os dados no computador e instalar um software mal-intencionado, como software de traço de chave de registro em log no computador. Se o administrador tiver feito logon usando uma conta de usuário normal, o vírus terá acesso apenas aos dados do administrador e não será capaz de instalar um software mal-intencionado. Usando os privilégios mínimos necessários para ler emails, neste exemplo, o escopo do comprometimento potencial é consideravelmente reduzido."  
  
## <a name="the-privilege-problem"></a>O problema de privilégio  
Princípios descritos os trechos anteriores não tem sido alterada, mas na avaliação instalações do Active Directory, invariavelmente encontrarmos número excessivo de contas que foram concedidas direitos e permissões muito além dos necessários para executar suas tarefas diárias. O tamanho do ambiente de afeta os números brutos contas muito privilegiados, mas não os diretórios proportionmidsized podem ter dezenas de contas nos grupos de mais altamente privilegiados, enquanto instalações grandes podem ter centenas ou milhares mesmo. Com algumas exceções, independentemente da sofisticação dos habilidades e arsenal, um invasor invasores normalmente seguem o caminho mais fácil. Eles aumentam a complexidade de suas ferramentas e abordagem somente quando mais simples mecanismos falha ou impedido pela defensores.  
  
Infelizmente, o caminho de menos resistência em muitos ambientes provou para ser o uso excessivo de contas com o privilégio ampla e profunda. Privilégios amplos são direitos e permissões que permitem uma conta realizar atividades específicas em uma grande parte do ambiente - por exemplo, a equipe de suporte técnico pode ser concedida permissões que permitam redefinir as senhas em várias contas de usuário.  
  
Privilégios profundos são privilégios poderosos que são aplicados a um segmento da população estreito, tais dar um engenheiro de direitos de administrador em um servidor para que ele podem realizar reparos. Amplo privilégio nem privilégio profundo é perigoso necessariamente, mas quando muitas contas do domínio permanentemente são concedidas privilégios amplo e profundo, se somente uma das contas for comprometida, ele pode rapidamente ser usado para reconfigurar o ambiente para fins do invasor ou até mesmo para destruir grandes segmentos da infraestrutura.  
  
Ataques Pass-the-hash, que são um tipo de ataque de roubo de credenciais, são onipresentes porque as ferramentas para realizá-las é livremente disponíveis e fácil de usar e porque muitos ambientes são vulneráveis a ataques. Ataques Pass-the-hash, no entanto, não são o problema real. O x do problema é dupla:  
  
1.  É relativamente fácil para um invasor obter privilégio profundo em um único computador e, em seguida, propagar esse privilégio amplamente em outros computadores.  
  
2.  Geralmente, há muitas contas permanentes com altos níveis de privilégio em todo o cenário de computação.  
  
Mesmo que os ataques pass-the-hash são eliminados, os invasores simplesmente usaria táticas diferentes, não uma estratégia diferente. Em vez de plantio malware que contém ferramentas de roubo de credenciais, elas podem plante malware que registra em log pressionamentos de teclas ou aproveitar inúmeras outras abordagens para capturar as credenciais que são eficientes em todo o ambiente. Independentemente das táticas, os alvos permanecem os mesmos: contas com o privilégio ampla e profunda.  
  
Concessão de privilégios excessivos apenas não for encontrado no Active Directory em ambientes comprometidos. Quando uma organização desenvolveu o hábito de concessão mais privilégios do que é necessário, ele geralmente é encontrado em toda a infraestrutura conforme discutido nas seções a seguir.  
  
## <a name="in-active-directory"></a>No Active Directory  
No Active Directory, é comum para encontrar os grupos EA, DA e BA contêm número excessivo de contas. Mais comumente, grupo EA de uma organização contém os membros menor, DA grupos geralmente contêm um multiplicador do número de usuários do grupo EA e grupos de administradores geralmente contêm mais membros do que a população dos outros grupos combinados. Isso geralmente é devido a uma crença de que os administradores de alguma forma são "menos privilegiados" que DAs ou EAs. Enquanto os direitos e permissões concedidas para cada um desses grupos são diferentes, elas devem ser efetivamente consideradas igualmente grupos poderosos como um membro de um pode tornar-se um membro dos outros dois.  
  
## <a name="on-member-servers"></a>Em servidores membro  
Quando podemos recuperar a associação dos grupos de administradores locais em servidores membro em muitos ambientes, achamos que a associação ao grupo desde algumas contas locais e de domínio, a dezenas de grupos aninhados que, quando expandida, revelar centenas, até mesmo milhares de contas com privilégios de administrador local nos servidores. Em muitos casos, os grupos de domínio com associações grandes são aninhados em grupos de administradores locais dos servidores membro, sem considerar ao fato de que qualquer usuário que pode modificar as associações desses grupos no domínio pode obter o controle administrativo de todos os sistemas em que o grupo foi aninhado em um grupo local Administradores.  
  
## <a name="on-workstations"></a>Em estações de trabalho  
Embora estações de trabalho normalmente têm membros significativamente menos em seus grupos de administradores locais que servidores membros, em muitos ambientes, os usuários são concedidos a associação ao grupo local Administradores em seus computadores pessoais. Quando isso ocorre, mesmo se o UAC está habilitado, os usuários representam um risco com privilégios elevados para a integridade de suas estações de trabalho.  
  
> [!IMPORTANT]  
> Você deve considerar cuidadosamente se os usuários exigem direitos administrativos em suas estações de trabalho, e se eles o façam, uma melhor abordagem pode ser criar uma conta local separada no computador que é um membro do grupo Administradores. Quando os usuários exigem elevação, eles podem apresentar as credenciais dessa conta local de elevação, mas como a conta é local, ele não pode ser usado para comprometer a outros computadores ou acessar recursos do domínio. Assim como com qualquer contas locais, no entanto, as credenciais da conta privilegiado local devem ser exclusivas; Se você criar uma conta local com as mesmas credenciais em várias estações de trabalho, você pode expor os computadores a ataques pass-the-hash.  
  
## <a name="in-applications"></a>Em aplicativos  
Em ataques em que o destino é de propriedade intelectual da organização, contas que foram concedidas privilégios avançados dentro de aplicativos podem ser direcionadas para permitir que roubar os dados de dados. Embora as contas que têm acesso a dados confidenciais podem ter recebidas sem privilégios elevados no domínio ou o sistema operacional, contas que podem manipular a configuração de um aplicativo ou o acesso às informações do aplicativo fornece risco presente.  
  
## <a name="in-data-repositories"></a>Em repositórios de dados  
Como é o caso com outros destinos, os invasores que buscam acesso à propriedade intelectual na forma de documentos e outros arquivos podem ter como alvo as contas que controlar o acesso ao arquivo armazena as contas que têm acesso direto aos arquivos, ou até mesmo grupos ou funções que têm acesso aos arquivos. Por exemplo, se um servidor de arquivos é usado para armazenar documentos de contrato e acesso é concedido para os documentos com o uso de um grupo do Active Directory, um invasor pode modificar os membros do grupo pode adicionar contas comprometidas ao grupo e acessar os documentos de contrato. Em casos em que acesso a documentos é fornecido por aplicativos, como o SharePoint, os invasores podem apontar os aplicativos conforme descrito anteriormente.  
  
## <a name="reducing-privilege"></a>Redução de privilégio  
O maior e mais complexo um ambiente mais difícil é para gerenciar e proteger. Em pequenas empresas, analisar e reduzir privilégio podem ser uma proposta relativamente simples, mas cada servidor adicionais, a estação de trabalho, a conta de usuário e o aplicativo em uso em uma organização adiciona outro objeto que deve ser protegido. Como ele pode ser difícil ou impossível corretamente segura todos os aspectos de uma organização infraestrutura de TI, você deve se concentrar esforços primeiro sobre as contas cujo privilégio criar o maior risco, que normalmente são integrados privilegiados contas e grupos no Active Directory e contas locais privilegiado em estações de trabalho e servidores membro.  
  
### <a name="securing-local-administrator-accounts-on-workstations-and-member-servers"></a>Protegendo contas de Administrador Local em servidores membros e estações de trabalho  
Embora este documento trata da segurança do Active Directory, conforme discutido anteriormente, a maioria dos ataques contra o diretório começam como ataques contra hosts individuais. Diretrizes completas para proteger grupos locais em sistemas de membro não podem ser fornecidas, mas as recomendações a seguir podem ser usadas para ajudar a proteger as contas de administrador locais em servidores membros e estações de trabalho.  
  
#### <a name="securing-local-administrator-accounts"></a>Protegendo contas de Administrador Local  
Em todas as versões do Windows no momento de suporte base, a conta de administrador local é desabilitada por padrão, o que torna a conta inutilizável para pass-the-hash e outros ataques de roubo de credenciais. No entanto, em domínios que contém os sistemas operacionais herdados ou em quais contas de administrador locais tiverem sido habilitadas, essas contas podem ser usadas conforme descrito anteriormente para propagar comprometimento em estações de trabalho e servidores membro. Por esse motivo, os seguintes controles são recomendados para todas as contas de administrador locais em sistemas ingressados em domínio.  
  
Instruções detalhadas para implementar esses controles são fornecidas em [apêndice h: Protegendo Administrador Local contas e grupos](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md). Antes de implementar essas configurações, no entanto, garantir que as contas de administrador locais não são atualmente usadas no ambiente para executar serviços em computadores ou executar outras atividades que essas contas não devem ser usadas. Teste essas configurações exaustivamente antes de implementá-los em um ambiente de produção.  
  
#### <a name="controls-for-local-administrator-accounts"></a>Controles para contas de Administrador Local  
Contas de administrador interno nunca devem ser usadas como contas de serviço em servidores membro, nem deve ser usados para fazer logon computadores locais (exceto no modo de segurança, que é permitido mesmo que a conta está desativada). O objetivo da implementação das configurações descritas aqui é impedir a conta de administrador local de cada computador seja utilizável, a menos que controles protetoras primeiro são revertidos. Implementando esses controles e contas de administrador para alterações de monitoramento, você pode reduzir significativamente a probabilidade de sucesso de um ataque de contas de administrador local que destinos.  
  
##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-joined-systems"></a>Configurando GPOs restringir contas de administrador em sistemas ingressados em domínio  
Em um ou mais GPOs que você crie e vincule a estação de trabalho e membro do servidor UOs em cada domínio, adicione a conta de administrador para os seguintes direitos de usuário no **atribuições de direitos de locais do computador \ Diretivas \ Configurações de segurança locais**:  
  
-   Negar acesso a este computador pela rede  
  
-   Negar logon como um trabalho em lotes  
  
-   Negar logon como um serviço  
  
-   Negar logon pelos serviços de área de trabalho remota  
  
Quando você adicionar contas de administrador para esses direitos de usuário, especifica se você estiver adicionando a conta de administrador local ou o administrador do domínio da conta a propósito que você rotular a conta. Por exemplo adicionar a conta de administrador do domínio NWTRADERS para estes negar direitos, digite a conta como **NWTRADERS\Administrador**, ou navegue até a conta de administrador no domínio NWTRADERS. Para garantir que você restringir a conta de administrador local, digite **administrador** nessas configurações no Editor de objeto de diretiva de grupo de direitos do usuário.  
  
> [!NOTE]  
> Mesmo que as contas de administrador locais são renomeadas, as políticas ainda serão aplicadas.  
  
Essas configurações garantirá que conta de administrador do computador não pode ser usada para se conectar a outros computadores, mesmo que ele esteja inadvertidamente ou propositadamente habilitado. Logons locais usando a conta de administrador local não pode ser completamente desabilitadas, nem deve tentar fazer isso, como conta de administrador local do computador é projetada para ser usada em cenários de recuperação de desastres.  
  
Deve um servidor membro ou estação de trabalho se tornam retirada do domínio sem outras privilégios administrativos concedido de contas locais, o computador pode ser inicializado no modo de segurança, a conta de administrador pode ser habilitada e a conta pode ser usada para efetuar reparos no computador. Quando reparos são concluídos, a conta de administrador deve ser desabilitada novamente.  
  
### <a name="securing-local-privileged-accounts-and-groups-in-active-directory"></a>Protegendo contas privilegiadas locais e os grupos no Active Directory  
*Lei número seis: um computador é tão seguro quanto o administrador é confiável.* - [Dez leis imutáveis de segurança (versão 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
As informações fornecidas aqui se destina a dar diretrizes gerais para proteger as contas internas de privilégio mais altos e os grupos no Active Directory. Instruções passo a passo detalhadas também são fornecidas em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md), [apêndice e: proteção empresarial administradores grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md), [apêndice f: Protegendo administradores grupos de domínio no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)e em [apêndice g: Protegendo grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md).  
  
Antes de implementar qualquer uma dessas configurações, você também deve testar todas as configurações exaustivamente para determinar se eles são apropriados para seu ambiente. Nem todas as organizações poderão implementar essas configurações.  
  
#### <a name="securing-built-in-administrator-accounts-in-active-directory"></a>Protegendo contas de administrador interno no Active Directory  
Em cada domínio no Active Directory, uma conta de administrador é criada como parte da criação do domínio. Essa conta é um membro dos grupos de administrador no domínio e administradores de domínio padrão e se o domínio for domínio raiz da floresta, a conta também é um membro do grupo Administradores corporativos. Uso da conta de administrador do domínio local deve ser reservado somente para atividades de compilação inicial e, possivelmente, cenários de recuperação de desastres. Para garantir que uma conta de administrador interno pode ser usada para efetuar reparos que podem ser usadas sem outras contas, você não deve alterar a associação padrão da conta do administrador em qualquer domínio na floresta. Em vez disso, você deve seguir as diretrizes para ajudar a proteger a conta de administrador em cada domínio na floresta. Instruções detalhadas para implementar esses controles são fornecidas em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para contas de administrador interno  
O objetivo da implementação das configurações descritas aqui é impedir que a conta de administrador do domínio cada (não a um grupo) seja utilizável, a menos que um número de controles é revertido. Implementando esses controles e as contas de administrador para alterações de monitoramento, você pode reduzir significativamente a probabilidade de um ataque bem-sucedido, aproveitando a conta de administrador do domínio. Para a conta de administrador em cada domínio na floresta, você deve configurar as seguintes configurações.  
  
##### <a name="enable-the-account-is-sensitive-and-cannot-be-delegated-flag-on-the-account"></a>Habilitar o sinalizador de "Conta é confidencial e não pode ser delegada" da conta  
Por padrão, todas as contas no Active Directory podem ser recebidas. Delegação permite que um computador ou o serviço para apresentar as credenciais de uma conta que autenticou no computador ou em outros computadores para obter serviços em nome da conta. Quando você habilita o **conta é confidencial e não pode ser delegada** atributo em uma conta baseada em domínio, as credenciais da conta não podem ser apresentadas para outros computadores ou serviços na rede, que limita a ataques que aproveitam delegação para usar as credenciais da conta em outros sistemas.  
  
##### <a name="enable-the-smart-card-is-required-for-interactive-logon-flag-on-the-account"></a>Habilitar o sinalizador "cartão inteligente é necessário para logon interativo" da conta  
Quando você habilita o **cartão inteligente é necessário para logon interativo** atributo em uma conta, Windows redefine a senha da conta como um valor de 120 caracteres aleatório. Definindo esse sinalizador em contas de administrador internas, você pode garantir que a senha da conta é não só longas e complexas, mas não é conhecida por qualquer usuário. Não é tecnicamente necessário para criar cartões inteligentes para as contas antes de habilitar esse atributo, mas se possível, cartões inteligentes deve ser criados para cada conta de administrador antes de configurar as restrições de conta e os cartões inteligentes devem ser armazenados em locais seguros.  
  
Embora definir a **cartão inteligente é necessário para logon interativo** sinalizador redefine a senha da conta, ela não impede que um usuário com direitos para redefinir a senha da conta de configuração da conta para um valor conhecido e usando o nome e a nova senha para acessar os recursos da conta na rede. Por isso, você deve implementar os seguintes controles adicionais na conta.  
  
##### <a name="disable-the-account"></a>Desabilitar a conta  
Se a conta de administrador já não estiver desabilitada, desativá-lo quando você tiver concluído a configuração das propriedades da conta. Isso impede que a conta está sendo usado para qualquer finalidade, a menos que ele é ativado pela primeira vez. Em um cenário de recuperação de desastres em que nenhuma conta está disponível para realizar reparos do ambiente AD DS, você pode inicializar um controlador de domínio no modo de segurança, faça logon localmente com a conta de administrador interno (que nunca é impedida de logon local) e habilitar a conta de administrador do domínio, se necessário.  
  
##### <a name="configuring-gpos-to-restrict-domains-administrator-accounts-on-domain-joined-systems"></a>Configurando GPOs restringir de contas de administrador domínios em sistemas ingressados em domínio  
Embora desabilitar a conta de administrador em um domínio faz com que a conta efetivamente inutilizável, você deve implementar restrições adicionais na conta, caso a conta está habilitada de maneira mal-intencionada ou inadvertidamente. Embora esses controles em última análise, podem ser revertidos pela conta de administrador, a meta é criar controles que retardam o progresso de um invasor e causar limite os danos que a conta.  
  
Em um ou mais GPOs que você crie e vincule a estação de trabalho e membro do servidor UOs em cada domínio, adicionar conta de administrador do domínio cada para os seguintes direitos de usuário no **atribuições de direitos de locais do computador \ Diretivas \ Configurações de segurança locais**:  
  
-   Negar acesso a este computador pela rede  
  
-   Negar logon como um trabalho em lotes  
  
-   Negar logon como um serviço  
  
-   Negar logon pelos serviços de área de trabalho remota  
  
> [!NOTE]  
> Quando você adicionar contas de administrador locais para essa configuração, você deve especificar se você estiver configurando contas administrador locais ou contas de administrador do domínio. Por exemplo adicionar a conta de administrador local do domínio NWTRADERS para estes negar direitos, é necessário digitar a conta como **NWTRADERS\Administrador**, ou navegue até a conta de administrador local para o domínio NWTRADERS. Se você digitar **administrador** essas configurações de direitos de usuário no Editor de objeto de diretiva de grupo, você restringirá a conta de administrador local em cada computador ao qual o GPO é aplicado.  
>   
> É recomendável restringir contas administrador locais em servidores membros e estações de trabalho da mesma maneira como contas de administrador baseado em domínio. Portanto, geralmente, você deve adicionar a conta de administrador para cada domínio na floresta e a conta de administrador para os computadores locais para essas configurações de direitos de usuário. Captura de tela a seguir mostra um exemplo de configuração esses direitos de usuário bloquear contas administrador locais e conta de administrador do domínio executem logons que não deve ser necessário para essas contas.  

>   
> ![modelos de administração do menor privilégio](media/Implementing-Least-Privilege-Administrative-Models/SAD_20.gif)  
  
##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-controllers"></a>Configurando GPOs restringir contas de administrador em controladores de domínio  
Em cada domínio na floresta, a política de controladores de domínio padrão ou uma política vinculado à UO de controladores de domínio deve ser modificada para adicionar a conta de administrador do domínio cada para os seguintes direitos de usuário no **atribuições de direitos de locais do computador \ Diretivas \ Configurações de segurança locais**:  
  
-   Negar acesso a este computador pela rede  
  
-   Negar logon como um trabalho em lotes  
  
-   Negar logon como um serviço  
  
-   Negar logon pelos serviços de área de trabalho remota  
  
> [!NOTE]  
> Essas configurações garantirá que a conta de administrador local não pode ser usada para se conectar a um controlador de domínio, embora a conta, se habilitada, pode logon localmente em controladores de domínio. Porque essa conta só deve ser habilitada e usada em cenários de recuperação de desastres, antecipa-se que o acesso físico a pelo menos um controlador de domínio estará disponível ou que outras contas com permissões para acessar remotamente controladores de domínio podem ser usadas.  
  
##### <a name="configure-auditing-of-built-in-administrator-accounts"></a>Configurar a auditoria de contas de administrador interno  
Quando você tê protegido conta de administrador do domínio cada e desabilitado-lo, você deve configurar a auditoria para monitorar as alterações feitas na conta. Se a conta está habilitada, é redefinir sua senha ou quaisquer outras modificações feitas na conta, alertas devem ser enviados para os usuários ou equipes responsáveis por administração do AD DS, além de equipes de resposta a incidentes em sua organização.  
  
### <a name="securing-administrators-domain-admins-and-enterprise-admins-groups"></a>Protegendo administradores, Admins. do domínio e grupos de administradores de empresa  
  
#### <a name="securing-enterprise-admin-groups"></a>Protegendo grupos de administração empresarial  
O grupo Administradores corporativos, que se encontra em domínio raiz da floresta, não deve conter nenhum usuário no dia a dia, com a possível exceção de conta de administrador local do domínio, desde que esteja protegido conforme descrito anteriormente e em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Quando o acesso ao EA for necessário, os usuários cujas contas exigem permissões e direitos EA devem ser colocados temporariamente no grupo Administradores corporativos. Embora os usuários estão usando as contas altamente privilegiadas, suas atividades devem ser auditadas e preferencialmente realizadas com um usuário que está executando as alterações e observar as alterações de outro usuário para minimizar a probabilidade de uso indevido inadvertido ou de configuração. Quando tiverem concluídas as atividades, as contas devem ser removidas do grupo EA. Isso pode ser feito via procedimentos manuais e documentados processos, software de gerenciamento (PIM/PAM) de terceiros privilegiados identidade/acesso ou uma combinação de ambos. Diretrizes para criar contas que pode ser usado para controlar a associação dos grupos privilegiados no Active Directory são fornecidas em [contas atraente de roubo de credenciais](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md) e instruções detalhadas são fornecidas no [apêndice i: criação de gerenciamento de contas para protegido contas e grupos no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Administradores corporativos são, por padrão, os membros do grupo Administradores interno em cada domínio na floresta. Remover do grupo Administradores corporativos dos grupos Administradores em cada domínio é uma modificação inadequada porque no caso de um cenário de recuperação de desastres floresta, direitos EA provavelmente será necessários. Se tiver sido removido do grupo Administradores corporativos de grupos de administradores em uma floresta, ele deve ser adicionado ao grupo Administradores em cada domínio e os seguintes controles adicionais devem ser implementados:  
  
-   Conforme descrito anteriormente, do grupo Administradores corporativos não deve conter nenhum usuário no dia a dia, com a possível exceção da conta de administrador do domínio raiz da floresta, que deve ser protegida conforme descrito em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
-   Em GPOs vinculados a UOs contendo servidores membros e estações de trabalho em cada domínio, o grupo EA deve ser adicionado à seguintes direitos do usuário:  
  
    -   Negar acesso a este computador pela rede  
  
    -   Negar logon como um trabalho em lotes  
  
    -   Negar logon como um serviço  
  
    -   Negar logon localmente  
  
    -   Nega logon pelos serviços de área de trabalho remota.  
  
Isso impedirá que membros do grupo EA fazer logon em estações de trabalho e servidores membro. Se os servidores de atalhos são usados para administrar controladores de domínio e o Active Directory, certifique-se de que os servidores de atalhos estão localizados em uma UO à qual os GPOs restritivos não são vinculados.  
  
-   Auditoria deve ser configurada para enviar alertas se quaisquer modificações feitas para as propriedades ou associação do grupo EA. Esses alertas devem ser enviados, no mínimo, os usuários ou equipes responsáveis por resposta de administração e incidente do Active Directory. Você também deve definir processos e procedimentos para popular temporariamente o grupo EA, incluindo procedimentos de notificação quando legítima população do grupo é executada.  
  
#### <a name="securing-domain-admins-groups"></a>Protegendo grupos de administradores de domínio  
Como é o caso com o grupo Administradores corporativos, participação em grupos de administradores de domínio deve ser solicitada apenas em cenários de compilação ou recuperação de desastres. Não deve haver nenhuma conta de usuário diárias no grupo DA exceto a conta de administrador local para o domínio, se ele tiver sido presa conforme descrito em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Quando o acesso DA for necessário, as contas necessidade esse nível de acesso devem ser colocadas temporariamente no grupo para o domínio em questão. Embora os usuários estão usando as contas altamente privilegiadas, atividades devem ser auditadas e preferencialmente realizadas com um usuário que está executando as alterações e observar as alterações de outro usuário para minimizar a probabilidade de uso indevido inadvertido ou de configuração. Quando tiverem concluídas as atividades, as contas devem ser removidas do grupo Admins. do domínio. Isso pode ser feito via procedimentos manuais e documentados processos, por meio do software de gerenciamento (PIM/PAM) de terceiros privilegiados identidade/acesso, ou uma combinação de ambos. Diretrizes para criar contas que pode ser usado para controlar a associação dos grupos privilegiados no Active Directory são fornecidas em [apêndice i: criação de gerenciamento de contas para protegido contas e grupos no Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Administradores de domínio são, por padrão, os membros do grupo Administradores local em todos os servidores membros e estações de trabalho em seus respectivos domínios. Não deve ser modificado aninhamento esse padrão porque ele afeta as opções de recuperação de desastres e suporte. Se grupos de administradores de domínio foram removidos do grupo Administradores local nos servidores membros, eles devem ser adicionados ao grupo Administradores em cada servidor membro e estação de trabalho no domínio por meio de configurações de grupo restrito em GPOs vinculados. Os seguintes controles gerais, que são descritos em detalhes no [apêndice f: Protegendo administradores grupos de domínio no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md) também deve ser implementada.  
  
Para o grupo Admins. do domínio em cada domínio na floresta:  
  
1.  Remova todos os membros do grupo DA, com a possível exceção de conta de administrador interno para o domínio, desde que ela tiver sido presa conforme descrito em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
2.  Em GPOs vinculados a UOs contendo servidores membros e estações de trabalho em cada domínio, o grupo DA deve ser adicionado à seguintes direitos do usuário:  
  
    -   Negar acesso a este computador pela rede  
  
    -   Negar logon como um trabalho em lotes  
  
    -   Negar logon como um serviço  
  
    -   Negar logon localmente  
  
    -   Negar logon pelos serviços de área de trabalho remota  
  
    Isso impedirá que membros do grupo DA fazer logon em estações de trabalho e servidores membro. Se os servidores de atalhos são usados para administrar controladores de domínio e o Active Directory, certifique-se de que os servidores de atalhos estão localizados em uma UO à qual os GPOs restritivos não são vinculados.  
  
3.  Auditoria deve ser configurada para enviar alertas se quaisquer modificações feitas para as propriedades ou associação do grupo DA. Esses alertas devem ser enviados, no mínimo, os usuários ou equipes responsáveis por resposta de administração e incidente do AD DS. Você também deve definir processos e procedimentos para popular temporariamente o grupo DA, incluindo procedimentos de notificação quando legítima população do grupo é executada.  
  
#### <a name="securing-administrators-groups-in-active-directory"></a>Protegendo grupos de administradores no Active Directory  
Como é o caso com os grupos EA e DA, associação ao grupo Administradores (BA) deve ser necessária apenas em cenários de compilação ou recuperação de desastres. Não deve haver nenhuma conta de usuário diárias no grupo Administradores, exceto a conta de administrador local para o domínio, se ele tiver sido presa conforme descrito em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Quando os administradores acesso é necessário, as contas necessidade esse nível de acesso devem ser colocadas temporariamente no grupo Administradores do domínio em questão. Embora os usuários estão usando as contas altamente privilegiadas, atividades devem ser auditadas e, preferencialmente, realizadas com um usuário que está executando as alterações e observar as alterações de outro usuário para minimizar a probabilidade de uso indevido inadvertido ou de configuração. Quando tiverem concluídas as atividades, as contas imediatamente devem ser removidas do grupo Administradores. Isso pode ser feito via procedimentos manuais e documentados processos, por meio do software de gerenciamento (PIM/PAM) de terceiros privilegiados identidade/acesso, ou uma combinação de ambos.  
  
Os administradores estão, por padrão, os proprietários da maioria dos objetos do AD DS em seus respectivos domínios. A associação ao grupo esse grupo pode ser necessário em cenários de recuperação de desastres e compilação em que a propriedade ou a capacidade de apropriar-se de objetos é necessária. Além disso, DAs e EAs herdam um número de seus direitos e permissões em virtude de sua participação padrão no grupo Administradores. Padrão de aninhamento de grupo para grupos privilegiados no Active Directory não devem ser modificados e grupo de administradores de cada domínio deve ser protegido, conforme descrito em [apêndice g: Protegendo grupos de administradores no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)e nas instruções gerais abaixo.  
  
1.  Remover todos os membros do grupo Administradores, com a possível exceção da conta do administrador local para o domínio, desde que ela tiver sido presa conforme descrito em [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
2.  Membros do grupo de administradores do domínio nunca devem precisar fazer logon em servidores membros ou estações de trabalho. Em um ou mais GPOs vinculados ao servidor de estação de trabalho e o membro UOs em cada domínio, o grupo Administradores deve ser adicionado para os seguintes direitos de usuário:  
  
    -   Negar acesso a este computador pela rede  
  
    -   Negar logon como um trabalho em lotes,  
  
    -   Negar logon como um serviço  
  
    -   Isso impedirá que membros do grupo Administradores sendo usado para logon ou se conectar aos servidores membros ou estações de trabalho (a menos que vários controles são violados primeiro), onde suas credenciais podem ser armazenadas em cache e, assim, comprometidas. Uma conta com privilégios nunca deve ser usada para fazer logon um sistema com menos privilégios e impor esses controles proporciona uma proteção contra ataques de diversas.  
  
3.  AT os controladores de domínio OU em cada domínio na floresta, o grupo Administradores deve ser concedido o usuário seguir direitos (caso já não tenham esses direitos), que permitirá que os membros do grupo Administradores para executar as funções necessárias para um cenário de recuperação de desastres de toda a floresta:  
  
    -   Acessar este computador pela rede  
  
    -   Permitir logon localmente  
  
    -   Permitir logon pelos serviços de área de trabalho remota  
  
4.  Auditoria deve ser configurada para enviar alertas se quaisquer modificações feitas para as propriedades ou os membros do grupo Administradores. Esses alertas devem ser enviados, no mínimo, aos membros da equipe responsável para administração do AD DS. Alertas também devem ser enviados aos membros da equipe de segurança e procedimentos devem ser definidos para modificar os membros do grupo Administradores. Especificamente, esses processos devem incluir um procedimento pelo qual a equipe de segurança seja notificada quando o grupo de administradores vai ser modificado para que quando alertas são enviados, eles são esperados e não é gerado um alarme. Além disso, os processos para notificar a equipe de segurança quando o uso do grupo Administradores foi concluído e as contas usadas foram removidas do grupo devem ser implementados.  
  
> [!NOTE]  
> Quando você implementa restrições no grupo Administradores em GPOs, o Windows aplica as configurações aos membros do grupo Administradores local de um computador além do grupo de administradores do domínio. Portanto, você deve usar cuidado ao implementar restrições no grupo Administradores. Embora proibir logons de rede, o lote e o serviço para membros do grupo Administradores é aconselhado onde é possível implementar, não restringem logons locais ou logons pelos serviços de área de trabalho remota. Bloquear esses tipos de logon pode bloquear legítima administração de um computador por membros do grupo Administradores local. Captura de tela a seguir mostra as definições de configuração que bloqueiam o uso indevido das interno local e domínio da conta do administrador, além de uso indevido dos grupos de administradores local ou de domínio internos. Observe que o **Negar logon pelos serviços de área de trabalho remota** direito de usuário não inclui o grupo de administradores, pois incluindo-a nessa configuração também bloqueia esses logons para contas que são membros do grupo de administradores do computador local. Se os serviços em computadores configurados para serem executados no contexto de qualquer um dos grupos privilegiados descritos nesta seção, implementar essas configurações pode causar serviços e aplicativos falhar. Portanto, como ocorre com todas as recomendações nesta seção, você deve testar as configurações de aplicabilidade em seu ambiente.  

>   
> ![modelos de administração do menor privilégio](media/Implementing-Least-Privilege-Administrative-Models/SAD_3.gif)  
  
### <a name="role-based-access-controls-rbac-for-active-directory"></a>Controles de acesso baseado em função (RBAC) para o Active Directory  
De modo geral, os controles de acesso baseado em função (RBAC) são um mecanismo para agrupar os usuários e fornecer acesso aos recursos com base nas regras de negócios. No caso do Active Directory, a implementação de RBAC para o AD DS é o processo de criação de funções para os quais os direitos e permissões são delegadas para permitir que os membros da função realizar tarefas administrativas diárias sem Concedendo privilégios excessivos a elas. RBAC para o Active Directory pode ser projetado e implementado por meio de ferramentas nativa e interfaces, aproveitando o software que você talvez já possui, ao comprar produtos de terceiros ou qualquer combinação dessas abordagens. Esta seção não fornece instruções passo a passo para implementar RBAC do Active Directory, mas em vez disso, aborda fatores que deve considerar ao escolher uma abordagem para implementar RBAC em suas instalações do AD DS.  
  
#### <a name="native-approaches-to-rbac-for-active-directory"></a>Abordagens nativas para RBAC do Active Directory  
Na implementação do RBAC mais simples, você pode implementar funções como grupos do AD DS e delegar direitos e permissões para os grupos que permitem que eles executem administração diária dentro do escopo designado da função.  
  
Em alguns casos, os grupos de segurança existentes no Active Directory podem ser usados para conceder direitos e permissões apropriadas para uma função de trabalho. Por exemplo, se os funcionários específicos em sua organização de TI serão responsáveis pelo gerenciamento e manutenção de zonas de DNS e registros, delegar essas responsabilidades pode ser tão simple quanto a criação de uma conta para cada administrador DNS e adicioná-lo ao grupo Administradores de DNS no Active Directory. O grupo de administradores DNS, ao contrário de grupos mais altamente privilegiados, tem alguns direitos poderosos em Active Directory, embora os membros deste grupo tem sido recebido permissões que permitam administrar DNS.  
  
Em outros casos, talvez você precise criar grupos de segurança e delegar direitos e permissões para objetos do Active Directory, objetos do sistema de arquivos e objetos do registro para permitir que os membros dos grupos para executar tarefas administrativas designadas. Por exemplo, se os operadores de suporte técnico são responsáveis por redefinir senhas esquecidas, ajudando os usuários com problemas de conectividade e solucionar problemas de configurações de aplicativo, você talvez seja necessário combinar as configurações de delegação em objetos de usuário no Active Directory com privilégios que permitem que os usuários de suporte técnico para se conectar remotamente a computadores dos usuários para exibir ou modificar configurações dos usuários. Para cada função que definir, você deve identificar:  
  
1.  Quais tarefas os membros da função executam no dia a dia e quais tarefas são executadas com menos frequência.  
  
2.  Em quais sistemas e em que os aplicativos membros de uma função devem ser concedidos direitos e permissões.  
  
3.  Quais usuários devem ser concedidos a associação ao grupo em uma função.  
  
4.  Como gerenciamento de associações de função será executado.  
  
Em muitos ambientes, manualmente a criação de controles de acesso baseado em função de administração de um ambiente do Active Directory pode ser um desafio para implementar e manter. Se você definiu claramente funções e responsabilidades de administração de sua infraestrutura de TI, convém aproveitar ferramentas adicionais para ajudá-lo na criação de uma implantação RBAC nativa gerenciável. Por exemplo, se Forefront Identity Manager (FIM) está em uso em seu ambiente, você pode usar o FIM para automatizar a criação e a população de funções administrativas, que podem facilitar a administração contínua. Se você usar o System Center Configuration Manager (SCCM) e o System Center Operations Manager (SCOM), você pode usar funções específicas do aplicativo para delegar gerenciamento e monitoramento de funções e também impor a consistência da configuração e auditoria entre sistemas no domínio. Se você implementou uma infraestrutura de chave pública (PKI), você pode emitir e exigir cartões inteligentes para a equipe de TI responsável para administrar o ambiente. Com o gerenciamento de credenciais de FIM FIM CM (), você pode combinar até gerenciamento de funções e credenciais para sua equipe administrativa.  
  
Em outros casos, é preferível para uma organização considerar a implantação de softwares RBAC de terceiros que forneça funcionalidade "out-of-box". Soluções de (COTS) comerciais, prontas para RBAC para Active Directory, Windows e não Windows diretórios e sistemas operacionais são oferecidas por uma série de fornecedores de. Ao escolher entre soluções nativas e produtos de terceiros, você deve considerar os seguintes fatores:  
  
1.  Orçamento: Investindo em desenvolvimento de RBAC usando software e as ferramentas que você talvez já possui, você pode reduzir os custos de software envolvidos na implantação de uma solução. No entanto, a menos que tenha equipe experientes na criação e implantação de soluções RBAC nativas, você terá de envolver recursos de consultoria para desenvolver sua solução. Cuidadosamente, você deve avaliar os custos antecipados para uma solução desenvolvida personalizado com os custos para implantar uma solução de "out-of-box", principalmente se o seu orçamento é limitado.  
  
2.  Composição do ambiente de TI: se o seu ambiente é composto basicamente de sistemas do Windows, ou se você já está aproveitando do Active Directory para gerenciamento de contas e não Windows sistemas, soluções personalizadas nativas podem fornecer a solução ideal para suas necessidades. Se sua infraestrutura contém muitos sistemas que não estão executando o Windows e não são gerenciados pelo Active Directory, talvez seja necessário considerar opções para gerenciamento de sistemas não Windows separadamente do ambiente do Active Directory.  
  
3.  Modelo de privilégio na solução: se um produto depende de posicionamento de suas contas de serviço em grupos altamente privilegiados no Active Directory e não oferecem opções que não exigem privilégios excessivos ser concedido ao software RBAC, você realmente não reduzida Active Directory ataque surfaceyou apenas alterei a composição dos grupos mais privilegiados no diretório. A menos que um fornecedor de aplicativos pode fornecer controles para contas de serviço que reduzem a probabilidade de que as contas sejam comprometidos e usados de maneira mal-intencionada, convém considerar outras opções.  

  
### <a name="privileged-identity-management"></a>Gerenciamento de identidade privilegiado  
Privilegiados identidade PIM (gerenciamento), às vezes, conhecido como conta com privilégios gerenciamento (PAM) ou o gerenciamento de credenciais com privilégios (PCM) é o design, construção, e privilegiados de implementação de abordagens para gerenciar contas em sua infraestrutura. De modo geral, PIM fornece mecanismos pelos quais contas são concedidas temporários direitos e permissões necessárias para executar a compilação ou quebra corrija funções, em vez de deixar privilégios conectados permanentemente à contas. Se a funcionalidade PIM é criada manualmente ou é implementada por meio de implantação de software de terceiros um ou mais dos seguintes recursos pode estar disponível:  
  

-   "Cofres", onde as senhas de contas privilegiadas são "check-out" e atribuídas uma senha inicial e depois "check-in" quando atividades foram concluídas, no momento em que as senhas são redefinidas novamente nas contas de credenciais.  
  
-   Restrições de tempo associado sobre o uso de credenciais com privilégios  
  
-   Uma vez usar credenciais  
  
-   Gerados de fluxo de trabalho de concessão de privilégio com o monitoramento e relatório de atividades realizadas e remoção automática de privilégio quando atividades sejam concluídas ou alocadas tempo expirou  
  
-   Substituição de credenciais embutidas, como nomes de usuário e senhas em scripts com interfaces de programação de aplicativo (APIs) que permitem que as credenciais a serem recuperados de cofres conforme necessário  
  
-   Gerenciamento automático de credenciais de conta de serviço  
  
### <a name="creating-unprivileged-accounts-to-manage-privileged-accounts"></a>Criando contas sem privilégios para gerenciar contas privilegiadas  
Um dos desafios em Gerenciando contas privilegiadas é que, por padrão, as contas que podem gerenciar contas privilegiadas e protegidas e grupos são privilegiados e protegidos contas. Se você implementar soluções RBAC e PIM apropriadas para a sua instalação do Active Directory, as soluções podem incluir abordagens que permitem que você depopulate efetivamente a associação dos grupos mais privilegiados no diretório, preenchendo os grupos somente temporariamente e quando necessários.  
  
Se você implementar RBAC e PIM nativos, no entanto, você deve considerar a criação de contas que não têm nenhum privilégio e com a única função de preenchimento e depopulating privilegiados agrupa no Active Directory quando necessário. [Apêndice i: gerenciamento criando contas para protegido contas e grupos no Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md) fornece instruções passo a passo que você pode usar para criar contas para essa finalidade.  
  
### <a name="implementing-robust-authentication-controls"></a>Implementando controles de autenticação robusta  
*Lei número seis: há alguém exista adivinhar suas senhas tentar realmente.* - [10 leis imutáveis de administração de segurança](https://technet.microsoft.com/library/cc722488.aspx)  
  
Pass-the-hash e outros ataques de roubo de credenciais não são específicos para sistemas operacionais Windows, nem são novos. O primeiro ataque pass-the-hash foi criado em 1997. Historicamente, no entanto, esses ataques necessárias ferramentas personalizadas, foram hit-or-miss no sucesso deles e necessário invasores tenham um relativamente alto grau de habilidades. A introdução de ferramentas disponíveis gratuitamente e fácil de usar que extrai nativamente credenciais resultou em um aumento exponencial no número e o sucesso de ataques de roubo de credenciais nos últimos anos. No entanto, ataques de roubo de credenciais são certamente os mecanismos somente pelo qual as credenciais são direcionadas e comprometidas.  
  
Embora você deve implementar controles para ajudar a proteger você contra ataques de roubo de credenciais, também deve identificar as contas em seu ambiente que têm mais probabilidade de ser atacado por invasores e implementar controles de autenticação robusta para essas contas. Se suas contas mais privilegiadas estiver usando a autenticação de fator único, como nomes de usuário e senhas (ambos são "algo que você sabe" que é um fator de autenticação), essas contas são protegidas levemente. Tudo o que você precisa de um invasor é conhecimento do nome do usuário e dados de Conhecimento da senha associada à conta, e os ataques pass-the-hash não são requiredthe invasor possa se autenticar como o usuário para todos os sistemas que aceite as credenciais de fator único.  
  
Embora a implementação da autenticação multifator não protege você contra ataques pass-the-hash, implementar a autenticação multifator em combinação com sistemas protegidos can. Mais informações sobre a implementação de sistemas protegidos são fornecidas nas [implementando segura Hosts administrativas](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md), e opções de autenticação são discutidas nas seções a seguir.  
  
#### <a name="general-authentication-controls"></a>Controles de autenticação geral  
Se você não tiver implementado a autenticação multifator, como cartões inteligentes, considere a possibilidade de fazer isso. Cartões inteligentes implementar proteção imposta pelo hardware de chaves privadas em um par de chaves pública – privada, impedindo que a chave privada do usuário sejam acessadas ou usado, a menos que o usuário apresenta o PIN correto, senha ou identificador biométrico para o cartão inteligente. Mesmo que um usuário o PIN ou senha é interceptada por um agente de log de pressionamento de teclas em um computador comprometido, para um invasor reutilizar o PIN ou a senha, o cartão também deve estar fisicamente presente.  
  
Em casos em que senhas longas e complexas mostraram difícil de implementar devido a resistência de usuário, cartões inteligentes fornecem um mecanismo pelo qual os usuários podem implementar PINs relativamente simples ou códigos sem as credenciais sendo vulneráveis a ataques de força bruta ou ataques de tabela do arco-íris. Pinos de cartão inteligente não são armazenados no Active Directory ou em bancos de dados locais do SAM, embora hashes de credenciais ainda podem ser armazenados na memória LSASS protegido em computadores nos quais foram usados cartões inteligentes para autenticação.  
  
#### <a name="additional-controls-for-vip-accounts"></a>Controles adicionais para contas de VIP  
Outra vantagem da implementação de cartões inteligentes ou outros mecanismos de autenticação baseada em certificado é a capacidade de utilizar garantia do mecanismo de autenticação para proteger dados confidenciais que sejam acessíveis para usuários VIP. Garantia do mecanismo de autenticação está disponível em domínios em que o nível funcional é definido como Windows Server 2012 ou Windows Server 2008 R2. Quando ele está habilitado, garantia do mecanismo de autenticação adiciona uma associação de grupo global com administrador designado para um token do usuário Kerberos quando as credenciais do usuário são autenticadas durante o logon usando um método de logon baseada em certificado.  
  
Isso possibilita que os administradores de recurso controlar o acesso a recursos, como arquivos, pastas e impressoras, com base em se o usuário fizer logon usando um método de logon baseada em certificado, além do tipo de certificado usado. Por exemplo, quando um usuário faz logon usando um cartão inteligente, o acesso do usuário aos recursos na rede pode ser especificado como diferente do que o acesso é quando o usuário não usa um cartão inteligente (ou seja, quando o usuário faz logon, inserindo um nome de usuário e senha). Para obter mais informações sobre garantia do mecanismo de autenticação, consulte o [garantia do mecanismo de autenticação para AD DS no guia passo a passo do Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897.aspx).  
  
#### <a name="configuring-privileged-account-authentication"></a>Configurando a autenticação de conta com privilégios  
No Active Directory para todas as contas administrativas, habilite o **requer um cartão inteligente para logon interativo** atributo e auditoria para alterações (no mínimo), qualquer um dos atributos no **conta** guia da conta (por exemplo, cn, nome, sAMAccountName, userPrincipalName e userAccountControl) em objetos de usuário administrativo.  
  
Embora definir a **requer um cartão inteligente para logon interativo** nas contas redefine a senha da conta como um valor de 120 caracteres aleatório e exigir cartões inteligentes para logons interativos, o atributo ainda pode ser substituídos por usuários com permissões que permitem que eles alterem as senhas das contas e as contas, em seguida, podem ser usadas para estabelecer logons interativos com apenas o nome de usuário e senha.  
  
Em outros casos, dependendo da configuração de contas em configurações do Active Directory e o certificado em serviços de certificados do Active Directory (AD CS) ou uma PKI de terceiros, o nome Principal do usuário (UPN) atributos para administrativos ou contas VIP podem ser direcionadas para um tipo específico de ataque, conforme descrito aqui.  
  
##### <a name="upn-hijacking-for-certificate-spoofing"></a>UPN sequestro de falsificação de certificado  
Embora uma discussão completa de ataques contra infraestruturas de chave pública (PKI) está fora do escopo deste documento, ataques contra PKI pública e privada aumentaram exponencialmente desde 2008. Violações de PKI pública tiverem sido publicadas amplamente, mas ataques contra PKI interna de uma organização talvez são ainda mais produtivo. Um tal ataque aproveita o Active Directory e certificados para permitir que um invasor falsifique as credenciais de outras contas de forma que podem ser difíceis de detectar.  
  
Quando um certificado é apresentado para autenticação em um sistema de domínio, o conteúdo de assunto ou o atributo de nome de alternativa de assunto (SAN) no certificado é usado para mapear o certificado para um objeto de usuário no Active Directory. Dependendo do tipo de certificado e como ele é criado, o atributo de assunto em um certificado normalmente contém um nome de usuário comum (CN), conforme mostrado na seguinte captura de tela.  
  
![modelos de administração do menor privilégio](media/Implementing-Least-Privilege-Administrative-Models/SAD_4.gif)  
  
Por padrão, o Active Directory constrói CN do usuário concatenando o nome da conta + "" + Sobrenome. No entanto, componentes CN dos objetos de usuário no Active Directory não são necessários ou garantidamente exclusivos e mover uma conta de usuário para um local diferente no diretório muda o nome da conta diferenciado (DN), que é o caminho completo para o objeto no diretório, conforme mostrado no painel inferior da captura de tela anterior.  
  
Como nomes de assunto do certificado não têm garantia de ser estáticos ou exclusivo, o conteúdo do nome do requerente alternativa geralmente é usado para localizar o objeto de usuário no Active Directory. O atributo de SAN para certificados emitidos para usuários de autoridades de certificação corporativas (Active Directory integrado CAs) normalmente contém do usuário UPN ou endereço de email. Porque UPNs são garantidamente exclusivos em uma floresta do AD DS, a localização de um objeto de usuário pelo nome UPN normalmente é feito como parte da autenticação, com ou sem envolvido no processo de autenticação de certificados.  
  
O uso de UPNs em atributos de SAN em certificados de autenticação pode ser aproveitado por invasores para obter certificados fraudulentos. Se um invasor tiver comprometido uma conta que tem a capacidade de ler e gravar UPNs em objetos de usuário, o ataque é implementado da seguinte maneira:  
  
O atributo de nome UPN em um objeto de usuário (por exemplo, um usuário VIP) temporariamente é alterado para um valor diferente. O atributo de nome de conta SAM e CN também podem ser alterado no momento, embora geralmente isso não é necessário para os motivos descritos anteriormente.  
  
Quando o atributo de nome UPN da conta de destino foi alterado, uma conta de usuário obsoleto, habilitado ou o atributo de nome UPN de uma conta usuário recém-criado é alterado para o valor que foi originalmente atribuído à conta de destino. Contas de usuário obsoletos e habilitado são contas que não fez logon por longos períodos de tempo, mas não foram desabilitadas. Eles são atacados por invasores que pretendem "ocultar em plena Vista" pelos seguintes motivos:  
  
1.  Porque a conta está habilitada, mas ainda não foram usada recentemente, usando a conta é improvável que acionam alertas da maneira que pode habilitar uma conta de usuário desabilitada.  
  
2.  Uso de uma conta existente não requer a criação de uma nova conta de usuário que pode ser percebida pela equipe administrativa.  
  
3.  Contas de usuário obsoletos que ainda estão habilitadas geralmente são membros dos vários grupos de segurança e terão acesso aos recursos da rede, simplificando o acesso e "mistura" para um preenchimento de usuário existentes.  
  
A conta de usuário no qual o destino UPN agora foi configurado é usada para solicitar um ou mais certificados de serviços de certificados do Active Directory.  
  
Quando certificados tem sido obtidos para a conta do invasor, UPNs sobre a conta "nova" e a conta de destino são retornados os valores originais.  
  
O invasor agora tem um ou mais certificados que podem ser apresentados para autenticação de recursos e aplicativos como se o usuário é o VIP cujas contas foram modificadas temporariamente. Embora uma discussão completa de todas as maneiras em que certificados e PKI podem ser atacado por invasores está fora do escopo deste documento, esse mecanismo de ataque é fornecido para ilustrar por que você monitore privilegiados e contas de VIP no AD DS para alterações, especialmente para alterações em qualquer um dos atributos no **conta** guia da conta (por exemplo cn, nome, sAMAccountName, userPrincipalName e userAccountControl). Além de monitoramento as contas, você deve restringir quem pode modificar as contas para como pequeno um conjunto de usuários administrativos quanto possível. Da mesma forma, as contas de usuários administrativos que devem ser protegidas e monitoradas alterações não autorizadas.  
  


