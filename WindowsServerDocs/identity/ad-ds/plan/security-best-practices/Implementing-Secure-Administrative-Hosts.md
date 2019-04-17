---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: Implementando Hosts administrativos seguros
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 42be89311f11ecc6a967b8b40600b53311253b89
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="implementing-secure-administrative-hosts"></a>Implementando Hosts administrativos seguros

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seguros hosts administrativos são estações de trabalho ou servidores que foram configurados especificamente para fins de criação de plataformas seguras do que contas privilegiadas podem executar tarefas administrativas no Active Directory ou em controladores de domínio, sistemas ingressados em domínio e aplicativos executados em sistemas ingressados em domínio. Nesse caso, "contas privilegiadas" refere-se não apenas para contas que são membros dos grupos mais privilegiados no Active Directory, mas para quaisquer contas que foram delegados direitos e permissões que permitem tarefas administrativas ser executada.  
  
Essas contas podem ser contas de suporte técnico que têm a capacidade de redefinir senhas para a maioria dos usuários em um domínio, contas que são usadas para administrar registros DNS e zonas ou contas que são usadas para gerenciamento de configuração. Seguros hosts administrativos são dedicados a funcionalidade administrativa, e elas não são executadas software, como aplicativos de email, navegadores da web ou software de produtividade como o Microsoft Office.  
  
Embora as contas e grupos "mais privilegiados" adequadamente devem ser mais rigorosamente protegido, isso não elimina a necessidade de proteger quaisquer contas e grupos aos quais privilégios acima aqueles de usuário padrão foram concedidas contas.  
  
Um host administrativo seguro pode ser uma estação de trabalho dedicada que é usada apenas para tarefas administrativas, um servidor membro que executa a função de servidor de Gateway de área de trabalho remota e para que os usuários de TI se conectar ao executar a administração dos hosts de destino ou um servidor que executa a função Hyper-V e fornece uma máquina virtual exclusiva para cada usuário de TI usar para suas tarefas administrativas. Em muitos ambientes, combinações de todos os três abordagens podem ser implementadas.  
  
Implementar hosts administrativos seguros requer planejamento e configuração que seja consistente com o tamanho da sua organização, práticas administrativas, apetite risco e orçamento. Considerações e opções para implementar seguros hosts administrativos são fornecidas aqui para uso em desenvolver uma estratégia administrativa adequada para sua organização.  
  
## <a name="principles-for-creating-secure-administrative-hosts"></a>Princípios de criação de segura Hosts administrativos  
Para proteger sistemas contra ataques com eficiência, alguns princípios gerais devem ser mantidos em mente:  
  
1.  Você nunca deve administrar um sistema confiável (ou seja, um servidor seguro, como um controlador de domínio) de um host menos confiável (ou seja, uma estação de trabalho que não esteja protegido com o mesmo grau os sistemas que ela gerencia).  
  
2.  Você não deve depender um fator de autenticação único ao realizar atividades privilegiadas; ou seja, as combinações de nome e a senha do usuário não devem ser consideradas aceitável autenticação porque apenas um único fator (algo que você conhece) é representado. Você deve considerar onde as credenciais são geradas e armazenado em cache ou armazenadas em cenários administrativos.  
  
3.  Embora a maioria dos ataques no cenário das ameaças atuais aproveitam malware e hackers mal-intencionados, não omita segurança física ao projetar e implementar hosts administrativos seguros.  
  
### <a name="account-configuration"></a>Configuração da conta  
Mesmo que sua organização não usa cartões inteligentes atualmente, você deve considerar implementá-las para contas privilegiadas e hosts administrativos seguros. Hosts administrativos devem ser configurados para exigir o logon com cartão inteligente para todas as contas, modificando a seguinte configuração em um GPO vinculado a UOs contendo hosts administrativos:  
  
**Logon do computador \ Diretivas Settings\Local Policies\Security Options\Interactive: requer um cartão inteligente**  
  
Essa configuração exigirá que todos os logons interativos para usar um cartão inteligente, independentemente da configuração de uma conta individual no Active Directory.  
  
Você também deve definir seguros hosts administrativos para permitir logons apenas por contas autorizadas, que podem ser configuradas no:  
  
**Atribuição de direitos do computador \ Diretivas Settings\Local Policies\Security locais diretivas**  
  
Isso concede interativo (e, quando apropriado, serviços de área de trabalho remota) direitos de logon apenas a usuários autorizados do host administrativo seguro.  
  
### <a name="physical-security"></a>Segurança física  
Para hosts administrativos ser considerado confiável, eles devem ser configurados e protegidos com o mesmo grau os sistemas que gerenciam. A maioria das recomendações fornecidas no [proteger controladores de domínio contra ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) também são aplicáveis aos hosts que são usados para administrar controladores de domínio e o banco de dados do AD DS. Um dos desafios de se implementar sistemas seguros administrativos na maioria dos ambientes é que a segurança física pode ser mais difícil de implementar, pois esses computadores normalmente residem em áreas que não são tão seguras quanto servidores hospedados no data center, como áreas de trabalho de usuários administrativos.  
  
Segurança física inclui controlar o acesso físico ao hosts administrativos. Em uma pequena empresa, isso pode significar que você mantenha uma estação de trabalho administrativa dedicada que é mantida bloqueada em um escritório ou uma gaveta quando não estiver em uso. Ou, pode significar que quando você precisa executar a administração do Active Directory ou os controladores de domínio, você fizer logon no controlador de domínio diretamente.  
  
Em organizações médias, podem ser implementados seguro administrativas "saltar servidores" que estão localizados em um local seguro em um escritório e são usados ao gerenciamento do Active Directory ou controladores de domínio é necessário. Você também pode implementar administrativas estações de trabalho que estão bloqueadas em locais seguros quando não estiver em uso, com ou sem servidores de atalhos.  
  
Em grandes organizações, você pode implantar servidores alojados datacenter atalhos que fornecem acesso estritamente controlado ao Active Directory. controladores de domínio; e servidores de arquivos, imprimir ou aplicativo. Implementação de uma arquitetura de servidor atalhos tem mais chances de incluir uma combinação de servidores e estações de trabalho seguras em ambientes grandes.  
  
Independentemente do tamanho da sua organização e o design do seu hosts administrativas, você deve proteger computadores físicos contra acesso não autorizado ou roubo e deve usar a criptografia de unidade de disco BitLocker para criptografar e proteger as unidades em hosts administrativos. Implementando o BitLocker em hosts administrativos, mesmo se um host for roubado ou seus discos são removidos, você pode garantir que os dados na unidade estão inacessíveis para os usuários não autorizados.  
  
### <a name="operating-system-versions-and-configuration"></a>Versões do sistema operacional e configuração  
Todos os hosts administrativos, se servidores ou estações de trabalho, deve executar o sistema operacional mais recente em uso em sua organização pelos motivos descritos anteriormente neste documento. Ao executar sistemas operacionais atuais, sua equipe administrativa se beneficia de novos recursos de segurança, suporte do fornecedor completo e funcionalidade adicional introduzido no sistema operacional. Além disso, ao avaliar um novo sistema operacional, pelo implantá-lo pela primeira vez em hosts administrativos, você precisará se familiarizar com os novos recursos, configurações e mecanismos de gerenciamento que ele oferece, que podem ser empregados subsequentemente no planejamento de implantação mais ampla do sistema operacional. Então, os usuários mais sofisticados em sua organização também estará os usuários que estão familiarizados com o novo sistema operacional e melhor posicionados para dar suporte a ele.  
  
### <a name="microsoft-security-configuration-wizard"></a>Assistente de configuração de segurança da Microsoft  
Se você implementar atalhos servidores como parte da estratégia de host administrativas, você deve usar o Assistente de configuração de segurança interna para configurar o serviço, registro, auditoria e configurações de firewall para reduzir a superfície de ataque do servidor. Quando as configurações do Assistente de configuração de segurança foram coletadas e configuradas, as configurações podem ser convertidas para um GPO que é usado para aplicar uma configuração de linha de base consistente em todos os servidores de atalhos. Você pode editar ainda mais o GPO para implementar específicas de configurações de segurança para servidores de atalhos e pode combinar todas as configurações com as configurações de linha de base adicionais extraídas do Microsoft Security Compliance Manager.  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
O [Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) é uma ferramenta gratuita disponível que se integra as configurações de segurança recomendadas pela Microsoft, com base na configuração de versão e a função do sistema operacional, e coleta-los em uma única ferramenta e a interface do usuário que pode ser usado para criar e definir configurações de segurança de linha de base para controladores de domínio. Modelos do Microsoft Security Compliance Manager podem ser combinados com as configurações do Assistente de configuração de segurança para produzir linhas de base de configuração abrangente para servidores de atalhos que são implantados e impostas pelos GPOs implantados nas UOs no qual atalhos servidores estão localizados no Active Directory.  
  
> [!NOTE]  
> Redação deste texto, o Microsoft Security Compliance Manager não inclui configurações específicas para servidores de atalhos ou outros hosts administrativos seguros, mas ainda pode ser usado para criar linhas de base iniciais para os hosts administrativos do Security Compliance Manager (SCM). Para proteger corretamente os hosts, no entanto, você deve aplicar configurações de segurança adicionais apropriadas para servidores e estações de trabalho altamente seguras.  
  
### <a name="applocker"></a>AppLocker  
Hosts administrativos e machinesshould virtual ser configurado com script, ferramenta e brancas de aplicativo por meio do AppLocker ou um software de restrição de aplicativo de terceiros. Quaisquer aplicativos administrativos ou utilitários que não são compatíveis para proteger as configurações devem ser atualizados ou substituídos com ferramentas que segue desenvolvimento seguro e práticas administrativas. Quando ferramentas novas ou adicional é necessária em um host administrativo, aplicativos e utilitários devem ser totalmente testados e se as ferramentas são adequadas para a implantação em hosts administrativas, ele pode ser adicionado ao brancas os sistemas.  
  
### <a name="rdp-restrictions"></a>Restrições de RDP  
Embora a configuração específica varia de acordo com a arquitetura de seus sistemas administrativas, você deve incluir restrições nos quais contas e computadores podem ser usados para estabelecer conexões de protocolo de área de trabalho remota (RDP) para sistemas gerenciados, como o uso de área de trabalho remota (Gateway RD) atalhos servidores para controlar o acesso aos controladores de domínio e gerenciados por outros sistemas de sistemas e os usuários autorizados.  
  
Você deve permitir logons interativos por usuários autorizados e devem remover ou até mesmo bloquear outros tipos de logon não são necessárias para acesso ao servidor.  
  
### <a name="patch-and-configuration-management"></a>Patch e gerenciamento de configuração  
Organizações menores podem depender de ofertas como o Windows Update ou [Windows Server Update Services](https://technet.microsoft.com/windowsserver/bb332157) (WSUS) para gerenciar a implantação de atualizações para sistemas Windows, enquanto as organizações maiores podem implementar patch e configuração do software de gerenciamento empresarial, como o System Center Configuration Manager. Independentemente dos mecanismos de que você usar para implantar atualizações em seu servidor geral e população de estação de trabalho, você deve considerar implantações separadas para sistemas altamente seguras, como controladores de domínio, autoridades de certificação e hosts administrativos. Separando esses sistemas contra a infraestrutura de gerenciamento geral, se suas contas de software ou serviço de gerenciamento são comprometidas, o comprometimento não pode ser facilmente estendido para os sistemas mais seguros em sua infraestrutura.  
  
Embora você não deve implementar processos de atualização manual de proteger seus sistemas, você deve configurar uma infraestrutura separada para atualizar sistemas seguros. Até mesmo em organizações muito grandes, essa infraestrutura geralmente pode ser implementada por meio de GPOs e servidores WSUS dedicados para sistemas seguros.  
  
### <a name="blocking-internet-access"></a>Bloquear o acesso à Internet  
Hosts administrativos não devem ter permissão para acessar a Internet, nem devem eles poderão navegar intranet de uma organização. Navegadores da Web e aplicativos similares não devem ser permitidos em hosts administrativos. Você pode bloquear o acesso à Internet para hosts seguros por meio de uma combinação de configurações do firewall do perímetro, a configuração WFAS e configuração de proxy de "buraco negro" em hosts seguros. Você também pode usar o aplicativo lista de exceções para impedir que os navegadores da web seja usado em hosts administrativos.  
  
### <a name="virtualization"></a>Virtualização  
Quando possível, considere a implementação de máquinas virtuais como hosts administrativos. Usando a virtualização, você pode criar por usuário administrativos sistemas que são armazenadas e gerenciadas centralmente e que pode ser facilmente desligado quando não estiver em uso, garantindo que as credenciais não permanecem ativas nos sistemas administrativos. Você também pode exigir que hospeda administrativas virtual é redefinidas para um instantâneo inicial após cada uso, garantindo que as máquinas virtuais permanecem impecáveis. Para obter mais informações sobre opções de virtualização dos hosts administrativas são fornecidas na seção a seguir.  
  
## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Abordagens de exemplo à implementação segura Hosts administrativos  
Independentemente de como criar e implantar sua infraestrutura de host administrativas, você deve ter em mente as orientações fornecidas na "Princípios para criar segura administrativas Hosts" anteriormente contidas neste tópico. Cada uma das abordagens descritas aqui fornece informações gerais sobre como você pode separar "administrativas" e sistemas de "produtividade" usados por sua equipe de TI. Sistemas de produtividade são computadores que os administradores de TI empregam para verificar emails, navegar na Internet e usar o software de produtividade geral como o Microsoft Office. Sistemas administrativos são computadores que estão protegidos e dedicados para usar para administração diária de um ambiente de TI.  
  
A maneira mais simples de implementar hosts administrativos seguros é fornecer sua equipe de TI com estações de trabalho protegidas em que eles podem executar tarefas administrativas. Em uma implementação somente estação de trabalho, cada estação de trabalho administrativa é usada para iniciar as ferramentas de gerenciamento e conexões RDP para gerenciar servidores e outra infraestrutura. Implementações de estação de trabalho somente podem ser eficazes para organizações menores, embora infraestruturas maiores e mais complexas podem se beneficiar com um design distribuído para hosts administrativos em que são usadas estações de trabalho e servidores administrativas dedicados, conforme descrito em "Implementando administrativas estações de trabalho e atalhos servidores seguros" neste tópico.  
  
### <a name="implementing-separate-physical-workstations"></a>Implementando estações de trabalho físicas separadas  
Uma maneira que você pode implementar hosts administrativos é emitir cada usuário de TI duas estações de trabalho. Uma estação de trabalho é usada com uma conta de usuário "normal" para realizar atividades como verificar emails e usando aplicativos de produtividade, enquanto a segunda estação de trabalho é dedicada estritamente a funções administrativas.  
  
Para a estação de trabalho de produtividade, a equipe de TI pode receber contas de usuário normal em vez de usar contas privilegiadas para fazer logon computadores não protegidos. A estação de trabalho administrativa deve ser configurada com uma configuração rigorosamente controlada e a equipe de TI deve usar uma conta diferente para fazer logon na estação de trabalho administrativo.  
  
Se você implementou cartões inteligentes, estações de trabalho administrativas devem ser configuradas para exigir logons de cartão inteligente e equipe de TI deve ter contas separadas para uso administrativo, também é configurado para exigir cartões inteligentes para logon interativo. O host administrativo deve ser robusto conforme descrito anteriormente e apenas usuários de TI designado devem ter permissão para fazer logon localmente a estação de trabalho administrativa.  
  
#### <a name="pros"></a>Profissionais  
Implementando sistemas físicos separados, você pode garantir que cada computador está configurado adequadamente para sua função e que usuários não podem inadvertidamente expor administrativos sistemas de risco.  
  
#### <a name="cons"></a>Contras  
  
-   Implementar computadores físicos separados aumenta os custos de hardware.  
  
-   Fazer logon em um computador físico com as credenciais que são usados para administrar sistemas remotos armazena em cache as credenciais na memória.  
  
-   Se estações de trabalho administrativas não são armazenadas com segurança, eles podem estar vulneráveis de comprometer por meio de mecanismos como registradores de chaves de hardware físico ou outros ataques físicos.  
  
### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implementando uma estação de trabalho física segura com uma estação de trabalho de produtividade virtualizada  
Nesta abordagem, usuários recebem uma estação de trabalho administrativa segura de que eles podem executar funções administrativas diárias, usando ferramentas de administração de servidor remoto (RSAT) ou conexões RDP para servidores no seu escopo de responsabilidade. Quando os usuários de TI precisam para realizar tarefas de produtividade, ele podem se conectar via RDP uma estação de trabalho remota produtividade em execução como uma máquina virtual. Credenciais separadas devem ser usadas para cada estação de trabalho e controles, como cartões inteligentes devem ser implementados.  
  
#### <a name="pros"></a>Profissionais  
  
-   Estações de trabalho administrativas e estações de trabalho de produtividade são separadas.  
  
-   Equipe de TI usando estações de trabalho seguras para se conectar a estações de trabalho de produtividade pode usar credenciais separadas e cartões inteligentes e credenciais com privilégios não são depositadas no computador menos seguro.  
  
#### <a name="cons"></a>Contras  
  
-   Implementando a solução requer o design e o trabalho de implementação e opções de virtualização robusta.  
  
-   Se as estações de trabalho físicas não são armazenadas com segurança, eles podem estar vulneráveis a ataques físicos que comprometer o hardware ou o sistema operacional e torná-los suscetível a interceptação de comunicações.  
  
### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implementando uma estação de trabalho segura única com conexões para separar "Produtividade" e "Administrativas" máquinas virtuais  
Nesta abordagem, você pode emitir uma única física estação de trabalho que estiver bloqueada conforme descrito anteriormente e, em que usuários não têm acesso privilegiado usuários de TI. Você pode fornecer conexões de serviços de área de trabalho remota para máquinas virtuais hospedadas em servidores dedicados, fornecendo a equipe de TI com uma máquina virtual que executa o email e outros aplicativos de produtividade e uma segunda máquina virtual que esteja configurada como host administrativas dedicado do usuário.  
  
Você deve exigir um cartão inteligente ou outro logon multifator para máquinas virtuais, usando contas separadas além da conta que é usado para fazer logon no computador físico. Depois que um usuário de TI faz logon um computador físico, eles podem usar seu cartão inteligente de produtividade para se conectar ao computador remoto produtividade e uma conta separada e cartão inteligente para se conectar ao seu computador remoto administrativo.  
  
#### <a name="pros"></a>Profissionais  
  
-   Os usuários de TI podem usar uma única estação de trabalho física.  
  
-   Por que exigem contas separadas para os hosts virtuais e usar conexões de serviços de área de trabalho remota para máquinas virtuais, as credenciais dos usuários IT não são armazenadas em cache no computador local.  
  
-   O host físico pode ser protegido com o mesmo grau hosts administrativos, reduzindo a probabilidade de comprometimento do computador local.  
  
-   Em casos em que VM de produtividade do usuário um IT ou sua máquina virtual administrativa podem ter sido comprometida, a máquina virtual pode ser facilmente redefinida para um estado de "boas".  
  
-   Se o computador físico for comprometido, sem credenciais privilegiados serão armazenados em cache na memória e o uso de cartões inteligentes pode impedir o comprometimento de credenciais pelo digitação.  
  
#### <a name="cons"></a>Contras  
  
-   Implementando a solução requer o design e o trabalho de implementação e opções de virtualização robusta.  
  
-   Se as estações de trabalho físicas não são armazenadas com segurança, eles podem estar vulneráveis a ataques físicos que comprometer o hardware ou o sistema operacional e torná-los suscetível a interceptação de comunicações.  
  
### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implementar atalhos servidores e estações de trabalho administrativas seguras  
Como uma alternativa para proteger estações de trabalho administrativas ou em combinação com eles, você pode implementar atalhos seguro servidores e usuários administrativos que podem se conectar aos servidores do salto usando RDP e cartões inteligentes para realizar tarefas administrativas.  
  
Servidores de atalhos devem ser configurados para executar a função de Gateway de área de trabalho remota para permitir que você implementar restrições em conexões para o servidor de atalhos e servidores de destino que serão gerenciados dele. Se possível, você também deve instalar a função Hyper-V e criar [áreas de trabalho virtuais pessoais](https://technet.microsoft.com/library/dd759174.aspx) ou outras máquinas de virtuais por usuário para usuários administrativos que usar para suas tarefas nos servidores de atalhos.  
  
Ao dar os usuários administrativos máquinas virtuais de cada usuário no servidor de atalhos, você fornecer segurança física para as estações de trabalho administrativas e usuários administrativos podem redefinir ou desligar as máquinas virtuais quando não estiver em uso. Se você preferir não instalar a função Hyper-V e a função de Gateway de área de trabalho remota no mesmo host administrativo, você pode instalá-los em computadores separados.  
  
Sempre que possível, ferramentas de administração remota devem ser usadas para gerenciar servidores. O recurso ferramentas de administração de servidor remoto (RSAT) deve ser instalado em máquinas virtuais dos usuários (ou o servidor de atalhos, se você não estiver implementando máquinas de virtuais por usuário para administração), e a equipe administrativa deve se conectar via RDP suas máquinas virtuais para executar tarefas administrativas.  
  
Em casos quando um usuário administrativo deve conectar via RDP para um servidor de destino para gerenciá-lo diretamente, Gateway de área de trabalho remota deve ser configurado para permitir que a conexão a serem tomadas somente se o usuário apropriado e o computador são usadas para estabelecer a conexão ao servidor de destino. Execução de RSAT (ou semelhantes) ferramentas devem ser proibidas em sistemas que não são designados sistemas de gerenciamento, como estações de trabalho de uso geral e servidores membro que são atalhos não servidores.  
  
#### <a name="pros"></a>Profissionais  
  
-   Criando servidores de atalhos permite mapear servidores específicos para "zonas" (conjuntos de sistemas com os requisitos de segurança, configuração e conexão semelhante) em sua rede e exigir que a administração de cada zona é alcançada pela equipe administrativa conectando-se de seguros hosts administrativas para um servidor designado "zona".  
  
-   Mapeando servidores de atalhos para zonas, você pode implementar controles granulares para propriedades de conexão e os requisitos de configuração e pode identificar facilmente tenta se conectar de sistemas não autorizados.  
  
-   Implementando máquinas de virtuais por administradores em servidores de atalhos, você pode impor desligamento e redefinir as máquinas virtuais para um estado conhecido limpo quando as tarefas administrativas são concluídas. Aplicando desligamento (ou reiniciar) de máquinas virtuais quando tarefas administrativas são concluídas, as máquinas virtuais não pode ser atacadas por invasores, nem são ataques de roubo de credenciais viável porque as credenciais armazenadas em cache memória não persistirão após uma reinicialização.  
  
#### <a name="cons"></a>Contras  
  
-   Servidores dedicados são necessários para servidores de atalhos, seja físico ou virtual.  
  
-   Implementando designado atalhos servidores e estações de trabalho administrativas requer planejamento cuidadoso e configuração que é mapeado para qualquer zonas de segurança configuradas no ambiente.  
  


