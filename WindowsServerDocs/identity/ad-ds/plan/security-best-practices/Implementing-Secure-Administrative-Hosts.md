---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: Implementar hosts administrativos seguros
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 63f4f805ca5326f480ce3496deecf04a40d5ffa4
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941426"
---
# <a name="implementing-secure-administrative-hosts"></a>Implementar hosts administrativos seguros

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os hosts administrativos seguros são estações de trabalho ou servidores que foram configurados especificamente para fins de criação de plataformas seguras, das quais as contas com privilégios podem executar tarefas administrativas no Active Directory ou em controladores de domínio, sistemas ingressados no domínio e aplicativos em execução em sistemas ingressados no domínio. Nesse caso, "contas privilegiadas" refere-se não apenas a contas que são membros dos grupos mais privilegiados em Active Directory, mas a qualquer conta que tenha sido delegada direitos e permissões que permitam a execução de tarefas administrativas.

Essas contas podem ser contas de suporte técnico que têm a capacidade de redefinir senhas para a maioria dos usuários em um domínio, contas que são usadas para administrar zonas e registros DNS, ou contas que são usadas para gerenciamento de configuração. Os hosts administrativos seguros são dedicados à funcionalidade administrativa e não executam software como aplicativos de email, navegadores da Web ou softwares de produtividade, como o Microsoft Office.

Embora as contas e os grupos "mais privilegiados" devam ser os mais rígidomente protegidos, isso não elimina a necessidade de proteger as contas e os grupos aos quais os privilégios acima daqueles das contas de usuário padrão foram concedidos.

Um host administrativo seguro pode ser uma estação de trabalho dedicada que é usada somente para tarefas administrativas, um servidor membro que executa a função de servidor Área de Trabalho Remota gateway e à qual os usuários de ti se conectam para executar a administração de hosts de destino ou um servidor que executa a função Hyper-V e fornece uma máquina virtual exclusiva para cada usuário de ti usar para suas tarefas administrativas. Em muitos ambientes, as combinações de todas as três abordagens podem ser implementadas.

A implementação de hosts administrativos seguros exige planejamento e configuração que sejam consistentes com o tamanho da sua organização, práticas administrativas, apetite de risco e orçamento. Considerações e opções para implementar hosts administrativos seguros são fornecidas aqui para você usar no desenvolvimento de uma estratégia administrativa adequada para sua organização.

## <a name="principles-for-creating-secure-administrative-hosts"></a>Princípios para a criação de hosts administrativos seguros
Para proteger sistemas com eficiência contra ataques, alguns princípios gerais devem ser mantidos em mente:

1.  Você nunca deve administrar um sistema confiável (ou seja, um servidor seguro, como um controlador de domínio) de um host menos confiável (ou seja, uma estação de trabalho que não esteja protegida no mesmo grau que os sistemas gerenciados por ele).

2.  Você não deve contar com um fator de autenticação único ao executar atividades privilegiadas; ou seja, as combinações de nome de usuário e senha não devem ser consideradas autenticação aceitável porque apenas um único fator (algo que você conhece) é representado. Você deve considerar onde as credenciais são geradas e armazenadas em cache ou armazenadas em cenários administrativos.

3.  Embora a maioria dos ataques no atual cenário de ameaças Aproveite malware e hackers mal-intencionados, não omita a segurança física ao projetar e implementar hosts administrativos seguros.

### <a name="account-configuration"></a>Configuração de conta
Mesmo que sua organização não use cartões inteligentes no momento, você deve considerar implementá-los para contas privilegiadas e hosts administrativos seguros. Os hosts administrativos devem ser configurados para exigir logon de cartão inteligente para todas as contas, modificando a configuração a seguir em um GPO vinculado às UOs que contêm hosts administrativos:

**Computador \ \ \ \ \ \ \ \ \ \ \ \ \ \ Options\Interactive logon:**

Essa configuração exigirá que todos os logons interativos usem um cartão inteligente, independentemente da configuração em uma conta individual no Active Directory.

Você também deve configurar hosts administrativos seguros para permitir logons somente por contas autorizadas, que podem ser configuradas em:

**Computador \ \ \ \ \ \ \ \ Diretivas \ \ \ \ \**

Isso concede direitos de logon interativos (e, quando apropriado, Serviços de Área de Trabalho Remota) somente a usuários autorizados do host administrativo seguro.

### <a name="physical-security"></a>Segurança física
Para que os hosts administrativos sejam considerados confiáveis, eles devem ser configurados e protegidos no mesmo grau que os sistemas que eles gerenciam. A maioria das recomendações fornecidas na [proteção de controladores de domínio contra ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) também se aplica aos hosts que são usados para administrar controladores de domínio e o banco de dados AD DS. Um dos desafios de implementar sistemas administrativos seguros na maioria dos ambientes é que a segurança física pode ser mais difícil de implementar, pois esses computadores geralmente residem em áreas que não são tão seguras quanto os servidores hospedados em data centers, como áreas de trabalho de usuários administrativos.

A segurança física inclui o controle do acesso físico aos hosts administrativos. Em uma pequena organização, isso pode significar que você mantém uma estação de trabalho administrativa dedicada que é mantida bloqueada em um escritório ou uma gaveta de mesa quando não está em uso. Ou pode significar que, quando você precisar executar a administração de Active Directory ou de seus controladores de domínio, faça logon diretamente no controlador de domínio.

Em organizações de médio porte, você pode considerar a implementação de "servidores de salto" administrativos seguros que estão localizados em um local seguro em um escritório e são usados quando o gerenciamento de Active Directory ou de controladores de domínio é necessário. Você também pode implementar estações de trabalho administrativas que estejam bloqueadas em locais seguros quando não estiverem em uso, com ou sem servidores de salto.

Em grandes organizações, você pode implantar servidores de salto hospedados no datacenter que fornecem acesso estritamente controlado ao Active Directory; controladores de domínio; e servidores de arquivos, de impressão ou de aplicativos. A implementação de uma arquitetura de servidor de salto tem mais probabilidade de incluir uma combinação de estações de trabalho e servidores seguros em ambientes grandes.

Independentemente do tamanho da sua organização e do design dos seus hosts administrativos, você deve proteger os computadores físicos contra o acesso ou o roubo não autorizado e deve usar Criptografia de Unidade de Disco BitLocker para criptografar e proteger as unidades em hosts administrativos. Ao implementar o BitLocker em hosts administrativos, mesmo se um host for roubado ou seus discos forem removidos, você poderá garantir que os dados na unidade fiquem inacessíveis para usuários não autorizados.

### <a name="operating-system-versions-and-configuration"></a>Configurações e versões do sistema operacional
Todos os hosts administrativos, sejam servidores ou estações de trabalho, devem executar o sistema operacional mais recente em uso na sua organização pelos motivos descritos anteriormente neste documento. Ao executar sistemas operacionais atuais, sua equipe administrativa se beneficia dos novos recursos de segurança, do suporte total ao fornecedor e da funcionalidade adicional introduzida no sistema operacional. Além disso, ao avaliar um novo sistema operacional, implantando-o primeiro nos hosts administrativos, você precisará se familiarizar com os novos recursos, configurações e mecanismos de gerenciamento que ele oferece, o que pode ser subseqüentemente aproveitado no planejamento da implantação mais ampla do sistema operacional. Por fim, os usuários mais sofisticados em sua organização também serão os usuários que estão familiarizados com o novo sistema operacional e mais bem posicionados para dar suporte a ele.

### <a name="microsoft-security-configuration-wizard"></a>Assistente de Configuração de Segurança da Microsoft
Se você implementar os servidores de salto como parte da sua estratégia de host administrativo, deverá usar o assistente de configuração de segurança interno para configurar o serviço, o registro, a auditoria e as configurações de firewall para reduzir a superfície de ataque do servidor. Quando as definições de configuração do assistente de configuração de segurança foram coletadas e configuradas, as configurações podem ser convertidas em um GPO usado para impor uma configuração de linha de base consistente em todos os servidores de salto. Você pode editar o GPO para implementar configurações de segurança específicas a servidores de salto e pode combinar todas as configurações com configurações de linha de base adicionais extraídas do Microsoft Security Compliance Manager.

### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager
O [Microsoft Security Compliance Manager](/previous-versions/tn-archive/cc677002(v=technet.10)) é uma ferramenta disponível gratuitamente que integra as configurações de segurança recomendadas pela Microsoft, com base na versão do sistema operacional e na configuração de função, e as coleta em uma única ferramenta e interface do usuário que podem ser usadas para criar e definir configurações de segurança de linha de base para controladores de domínio. Os modelos do Microsoft Security Compliance Manager podem ser combinados com as configurações do assistente de configuração de segurança para produzir linhas de base de configuração abrangentes para servidores de salto que são implantados e impostos por GPOs implantados nas UOs nas quais os servidores de salto estão localizados em Active Directory.

> [!NOTE]
> No momento em que este artigo foi escrito, o Microsoft Security Compliance Manager não inclui configurações específicas para servidores de salto ou outros hosts administrativos seguros, mas o SCM (Security Compliance Manager) ainda pode ser usado para criar linhas de base iniciais para seus hosts administrativos. No entanto, para proteger adequadamente os hosts, você deve aplicar configurações de segurança adicionais apropriadas a estações de trabalho e servidores altamente protegidos.

### <a name="applocker"></a>AppLocker
Hosts administrativos e machinesshould virtuais são configurados com scripts, ferramentas e listas de permissões de aplicativos por meio do AppLocker ou de um software de restrição de aplicativo de terceiros. Quaisquer aplicativos administrativos ou utilitários que não aderem às configurações seguras devem ser atualizados ou substituídos por ferramentas que aderem ao desenvolvimento e às práticas administrativas seguras. Quando são necessárias ferramentas novas ou adicionais em um host administrativo, os aplicativos e utilitários devem ser totalmente testados e, se as ferramentas forem adequadas para a implantação em hosts administrativos, poderão ser adicionados às listas de permissões dos sistemas.

### <a name="rdp-restrictions"></a>Restrições de RDP
Embora a configuração específica varie de acordo com a arquitetura de seus sistemas administrativos, você deve incluir restrições em quais contas e computadores podem ser usados para estabelecer conexões de protocolo RDP (RDP) com sistemas gerenciados, como usar os servidores de salto de Área de Trabalho Remota gateway (Gateway RD) para controlar o acesso a controladores de domínio e outros sistemas gerenciados de usuários e sistemas autorizados.

Você deve permitir logons interativos por usuários autorizados e deve remover ou até mesmo bloquear outros tipos de logon que não são necessários para o acesso ao servidor.

### <a name="patch-and-configuration-management"></a>Gerenciamento de patch e configuração
Organizações menores podem contar com ofertas como Windows Update ou [Windows Server Update Services](/windows/deployment/deploy-whats-new) (WSUS) para gerenciar a implantação de atualizações em sistemas Windows, enquanto as organizações maiores podem implementar software de gerenciamento de patches e configurações, como o Microsoft Endpoint Configuration Manager. Independentemente dos mecanismos que você usa para implantar atualizações em seu servidor geral e na população da estação de trabalho, você deve considerar implantações separadas para sistemas altamente seguros, como controladores de domínio, autoridades de certificação e hosts administrativos. Ao separar esses sistemas da infraestrutura de gerenciamento geral, se o software de gerenciamento ou as contas de serviço estiverem comprometidos, o comprometimento não poderá ser facilmente estendido para os sistemas mais seguros em sua infraestrutura.

Embora você não deva implementar processos de atualização manual para sistemas seguros, configure uma infraestrutura separada para a atualização de sistemas seguros. Mesmo em organizações muito grandes, essa infraestrutura normalmente pode ser implementada por meio de servidores WSUS e GPOs dedicados para sistemas protegidos.

### <a name="blocking-internet-access"></a>Bloqueando o acesso à Internet
Os hosts administrativos não devem ter permissão para acessar a Internet nem podem procurar a intranet de uma organização. Navegadores da Web e aplicativos semelhantes não devem ser permitidos em hosts administrativos. Você pode bloquear o acesso à Internet para hosts seguros por meio de uma combinação de configurações de firewall do perímetro, configuração do WFAS e configuração de proxy "buraco negro" em hosts seguros. Você também pode usar a lista de permissões do aplicativo para impedir que os navegadores da Web sejam usados em hosts administrativos.

### <a name="virtualization"></a>Virtualização
Sempre que possível, considere implementar máquinas virtuais como hosts administrativos. Usando a virtualização, você pode criar sistemas administrativos por usuário que são armazenados e gerenciados centralmente e que podem ser facilmente desligados quando não estiverem em uso, garantindo que as credenciais não sejam deixadas ativas nos sistemas administrativos. Você também pode exigir que os hosts administrativos virtuais sejam redefinidos para um instantâneo inicial após cada uso, garantindo que as máquinas virtuais permaneçam original. Mais informações sobre as opções de virtualização de hosts administrativos são fornecidas na seção a seguir.

## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Exemplos de abordagens para implementar hosts administrativos seguros
Independentemente de como você cria e implanta sua infraestrutura de host administrativo, lembre-se das diretrizes fornecidas em "princípios para a criação de hosts administrativos seguros", anteriormente neste tópico. Cada uma das abordagens descritas aqui fornece informações gerais sobre como você pode separar sistemas "administrativos" e "produtividade" usados por sua equipe de ti. Os sistemas de produtividade são computadores que os administradores de ti empregam para verificar email, navegar pela Internet e usar software de produtividade geral, como o Microsoft Office. Os sistemas administrativos são computadores que são protegidos e dedicados a serem usados para a administração diária de um ambiente de ti.

A maneira mais simples de implementar hosts administrativos seguros é fornecer à sua equipe de ti as estações de trabalho protegidas das quais eles podem executar tarefas administrativas. Em uma implementação somente de estação de trabalho, cada estação de trabalho administrativa é usada para iniciar as ferramentas de gerenciamento e conexões RDP para gerenciar servidores e outras infraestruturas. Implementações somente de estação de trabalho podem ser eficazes em organizações menores, embora infraestruturas maiores e mais complexas possam se beneficiar de um design distribuído para hosts administrativos nos quais servidores administrativos e estações de trabalho dedicados são usados, conforme descrito em "Implementando estações de trabalho administrativas e servidores de salto seguros" posteriormente neste tópico.

### <a name="implementing-separate-physical-workstations"></a>Implementando estações de trabalho físicas separadas
Uma maneira de implementar hosts administrativos é emitir cada usuário de duas estações de trabalho. Uma estação de trabalho é usada com uma conta de usuário "regular" para executar atividades como a verificação de email e o uso de aplicativos de produtividade, enquanto a segunda estação de trabalho é dedicada estritamente a funções administrativas.

Para a estação de trabalho de produtividade, a equipe de ti pode receber contas de usuário regulares em vez de usar contas privilegiadas para fazer logon em computadores não seguros. A estação de trabalho administrativa deve ser configurada com uma configuração rigorosamente controlada e a equipe de ti deve usar uma conta diferente para fazer logon na estação de trabalho administrativa.

Se você tiver implementado cartões inteligentes, as estações de trabalho administrativas deverão ser configuradas para exigir logons de cartão inteligente, e a equipe de ti deverá receber contas separadas para uso administrativo, também configurada para exigir cartões inteligentes para logon interativo. O host administrativo deve ser protegido conforme descrito anteriormente e somente usuários de ti designados devem ter permissão para fazer logon localmente na estação de trabalho administrativa.

#### <a name="pros"></a>Vantagens
Ao implementar sistemas físicos separados, você pode garantir que cada computador esteja configurado adequadamente para sua função e que os usuários de ti não possam expor inadvertidamente os sistemas administrativos a riscos.

#### <a name="cons"></a>Desvantagens

-   A implementação de computadores físicos separados aumenta os custos de hardware.

-   Fazer logon em um computador físico com credenciais que são usadas para administrar sistemas remotos armazena em cache as credenciais na memória.

-   Se as estações de trabalho administrativas não forem armazenadas de forma segura, elas poderão estar vulneráveis a comprometimento por meio de mecanismos como agentes de chave de hardware físico ou outros ataques físicos.

### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implementando uma estação de trabalho física segura com uma estação de trabalho de produtividade virtualizada
Nessa abordagem, os usuários de ti recebem uma estação de trabalho administrativa segura da qual podem executar funções administrativas cotidianas, usando conexões Ferramentas de Administração de Servidor Remoto (RSAT) ou RDP para servidores dentro de seu escopo de responsabilidade. Quando os usuários precisam executar tarefas de produtividade, eles podem se conectar via RDP a uma estação de trabalho de produtividade remota em execução como uma máquina virtual. Credenciais separadas devem ser usadas para cada estação de trabalho e controles como cartões inteligentes devem ser implementados.

#### <a name="pros"></a>Vantagens

-   Estações de trabalho administrativas e estações de trabalho de produtividade são separadas.

-   A equipe de ti que usa estações de trabalho seguras para se conectar a estações de trabalho de produtividade pode usar credenciais separadas e cartões inteligentes, e as credenciais privilegiadas não são depositadas no computador menos seguro.

#### <a name="cons"></a>Desvantagens

-   A implementação da solução requer o trabalho de design e implementação e opções de virtualização robustas.

-   Se as estações de trabalho físicas não forem armazenadas com segurança, elas poderão estar vulneráveis a ataques físicos que comprometem o hardware ou o sistema operacional e os tornam suscetíveis à interceptação de comunicações.

### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implementando uma única estação de trabalho segura com conexões para separar máquinas virtuais de "produtividade" e "administrativas"
Nessa abordagem, você pode emitir para os usuários de ti uma única estação de trabalho física bloqueada conforme descrito anteriormente, e em que os usuários de ti não têm acesso privilegiado. Você pode fornecer Serviços de Área de Trabalho Remota conexões com máquinas virtuais hospedadas em servidores dedicados, fornecendo à equipe de ti uma máquina virtual que executa email e outros aplicativos de produtividade, além de uma segunda máquina virtual configurada como o host administrativo dedicado do usuário.

Você deve exigir um cartão inteligente ou outro logon multifator para as máquinas virtuais, usando contas separadas diferentes da conta usada para fazer logon no computador físico. Depois que um usuário de ti faz logon em um computador físico, ele pode usar o cartão inteligente de produtividade para se conectar ao computador de produtividade remota e uma conta separada e um cartão inteligente para se conectar ao seu computador administrativo remoto.

#### <a name="pros"></a>Vantagens

-   Os usuários de ti podem usar uma única estação de trabalho física.

-   Ao exigir contas separadas para os hosts virtuais e usar Serviços de Área de Trabalho Remota conexões com as máquinas virtuais, as credenciais dos usuários de ti não são armazenadas em cache na memória no computador local.

-   O host físico pode ser protegido ao mesmo grau que os hosts administrativos, reduzindo a probabilidade de comprometimento do computador local.

-   Em casos em que a máquina virtual de produtividade de um usuário de ti ou sua máquina virtual administrativa pode ter sido comprometida, a máquina virtual pode ser facilmente redefinida para um estado "bom".

-   Se o computador físico estiver comprometido, nenhuma credencial privilegiada será armazenada em cache na memória e o uso de cartões inteligentes poderá impedir o comprometimento de credenciais por agentes de pressionamento de tecla.

#### <a name="cons"></a>Desvantagens

-   A implementação da solução requer o trabalho de design e implementação e opções de virtualização robustas.

-   Se as estações de trabalho físicas não forem armazenadas com segurança, elas poderão estar vulneráveis a ataques físicos que comprometem o hardware ou o sistema operacional e os tornam suscetíveis à interceptação de comunicações.

### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implementando estações de trabalho administrativas seguras e servidores de salto
Como alternativa para proteger estações de trabalho administrativas, ou em combinação com elas, você pode implementar servidores de salto seguro e os usuários administrativos podem se conectar aos servidores de salto usando o RDP e cartões inteligentes para executar tarefas administrativas.

Os servidores de salto devem ser configurados para executar a função de gateway Área de Trabalho Remota para permitir que você implemente restrições em conexões com o servidor de destino e com os servidores que serão gerenciados a partir dele. Se possível, você também deve instalar a função Hyper-V e criar [áreas de trabalho virtuais pessoais](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759174(v=ws.11)) ou outras máquinas virtuais por usuário para que os usuários administrativos usem para suas tarefas nos servidores de salto.

Ao fornecer aos usuários administrativos máquinas virtuais por usuário no servidor de salto, você fornece segurança física para as estações de trabalho administrativas, e os usuários administrativos podem redefinir ou desligar suas máquinas virtuais quando não estiverem em uso. Se preferir não instalar a função Hyper-V e a função de gateway Área de Trabalho Remota no mesmo host administrativo, você poderá instalá-las em computadores separados.

Sempre que possível, as ferramentas de administração remota devem ser usadas para gerenciar servidores. O recurso de Ferramentas de Administração de Servidor Remoto (RSAT) deve ser instalado nas máquinas virtuais dos usuários (ou no servidor de salto se você não estiver implementando máquinas virtuais por usuário para administração) e a equipe administrativa deve se conectar via RDP a suas máquinas virtuais para executar tarefas administrativas.

Em casos em que um usuário administrativo deve se conectar via RDP a um servidor de destino para gerenciá-lo diretamente, o gateway de área de trabalho remota deve ser configurado para permitir que a conexão seja feita somente se o usuário e o computador apropriados forem usados para estabelecer a conexão com o servidor de destino. A execução de ferramentas do RSAT (ou semelhantes) deve ser proibida em sistemas não designados para sistemas de gerenciamento, como estações de trabalho de uso geral e servidores membro que não são servidores de salto.

#### <a name="pros"></a>Vantagens

-   A criação de servidores de salto permite mapear servidores específicos para "zonas" (coleções de sistemas com requisitos de configuração, conexão e segurança semelhantes) em sua rede e exigir que a administração de cada zona seja obtida pela equipe administrativa que se conecta de hosts administrativos seguros a um servidor de "zona" designado.

-   Ao mapear servidores de salto para zonas, você pode implementar controles granulares para propriedades de conexão e requisitos de configuração e pode facilmente identificar tentativas de conexão de sistemas não autorizados.

-   Ao implementar máquinas virtuais por administrador em servidores de salto, você impõe o desligamento e a redefinição das máquinas virtuais para um estado claro conhecido quando as tarefas administrativas são concluídas. Ao impor o desligamento (ou reiniciar) das máquinas virtuais quando as tarefas administrativas são concluídas, as máquinas virtuais não podem ser direcionadas por invasores, nem os ataques de roubo de credenciais são viáveis porque as credenciais em cache de memória não persistem além de uma reinicialização.

#### <a name="cons"></a>Desvantagens

-   Os servidores dedicados são necessários para servidores de salto, sejam físicos ou virtuais.

-   A implementação de servidores de salto designados e estações de trabalho administrativas requer planejamento cuidadoso e configuração que mapeia para qualquer zona de segurança configurada no ambiente.

