---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: Implementar hosts administrativos seguros
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ed2ff7bfa0cc3b27506b1ca324e819860eef314c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890417"
---
# <a name="implementing-secure-administrative-hosts"></a>Implementar hosts administrativos seguros

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Hosts administrativos seguros são servidores que foram configurados especificamente para fins de criação de plataformas seguras em que contas com privilégios podem executar tarefas administrativas no Active Directory ou em controladores de domínio ou estações de trabalho sistemas ingressados em domínio e aplicativos executados em sistemas ingressados em domínio. Nesse caso, "contas privilegiadas" se refere não apenas para contas que são membros dos grupos mais privilegiados no Active Directory, mas também para quaisquer contas que tenham sido delegados direitos e permissões para tarefas administrativas ser executada.  
  
Essas contas podem ser contas de Help Desk que têm a capacidade de redefinir senhas para a maioria dos usuários em um domínio, as contas que são usadas para administrar zonas e registros DNS ou contas que são usadas para gerenciamento de configuração. Hosts administrativos seguros são dedicados a funcionalidade administrativa, e eles não executam o software como aplicativos de email, navegadores da web ou software de produtividade como o Microsoft Office.  
  
Embora as contas e grupos de "mais privilegiada" adequadamente devem ser protegido de forma mais rigorosa, isso não elimina a necessidade de proteger quaisquer contas e grupos aos quais privilégios que os de usuário padrão contas foi concedidas.  
  
Um host administrativo seguro pode ser uma estação de trabalho dedicada que é usada apenas para tarefas administrativas, um servidor membro que executa a função de servidor de Gateway de área de trabalho remota e para quais usuários de TI se conectar para realizar a administração de hosts de destino ou um servidor que executa o a função Hyper-V e fornece uma máquina virtual exclusiva para cada usuário a ser usado para tarefas administrativas de TI. Em muitos ambientes, combinações de todas as três abordagens podem ser implementadas.  
  
Implementar hosts administrativos seguros requer planejamento e configuração que é consistente com o tamanho da sua organização, práticas administrativas, exposição ao risco e orçamento. Considerações e opções para implementar hosts administrativos seguros são fornecidas aqui para uso no desenvolvimento de uma estratégia administrativa adequada para sua organização.  
  
## <a name="principles-for-creating-secure-administrative-hosts"></a>Princípios para criar Hosts administrativos seguros  
Para proteger com eficiência os sistemas contra ataques, alguns princípios gerais devem ser mantidos em mente:  
  
1.  Você nunca deve administrar um sistema confiável (ou seja, um servidor seguro, como um controlador de domínio) de um host de menos confiáveis (ou seja, uma estação de trabalho que não é segura para o mesmo grau que os sistemas que ele gerencia).  
  
2.  Você não deve confiar em um único fator de autenticação ao executar atividades privilegiadas; ou seja, as combinações de nome e a senha do usuário não devem ser consideradas aceitável autenticação porque apenas um único fator (algo que você conhece) é representado. Você deve considerar onde as credenciais são geradas e armazenados em cache ou armazenadas em cenários administrativos.  
  
3.  Embora a maioria dos ataques no panorama de ameaças atual aproveitar malware e hackers mal-intencionados, não será preciso omita segurança física ao projetar e implementar hosts administrativos seguros.  
  
### <a name="account-configuration"></a>Configuração da conta  
Mesmo se sua organização não usa cartões inteligentes no momento, você deve considerar implementá-las para contas privilegiadas e hosts administrativos seguros. Hosts administrativos devem ser configurados para exigir o logon de cartão inteligente para todas as contas, modificando a configuração a seguir em um GPO que esteja vinculado às UOs que contém hosts administrativos:  
  
**Logon do computador \ Diretivas locais \ Diretivas Options\Interactive: Exigir um cartão inteligente**  
  
Essa configuração exige que todos os logons interativos para usar um cartão inteligente, independentemente da configuração em uma conta individual no Active Directory.  
  
Você também deve configurar hosts administrativos seguros para permitir logons somente por contas autorizadas, que podem ser configuradas em:  
  
**Atribuição de direitos do computador \ Diretivas locais \ Diretivas locais \ atribuição**  
  
Isso concede interativo (e, quando apropriado, os serviços de área de trabalho remota) direitos de logon somente a usuários autorizados do host administrativo seguro.  
  
### <a name="physical-security"></a>Segurança física  
Para hosts administrativos ser considerado confiável, eles devem ser configurados e protegidos para o mesmo grau que os sistemas que gerenciam. A maioria das recomendações fornecidas em [Protegendo controladores de domínio contra ataque](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) também são aplicáveis para os hosts que são usados para administrar controladores de domínio e o banco de dados do AD DS. Um dos desafios da implementação de sistemas seguros de administrativos na maioria dos ambientes é que a segurança física pode ser mais difíceis de implementar porque esses computadores geralmente residem em áreas que não são tão seguras quanto servidores hospedados em data centers, tais como áreas de trabalho de usuários administrativos.  
  
Segurança física inclui controlar o acesso físico aos hosts administrativos. Em uma organização pequena, isso pode significar que você mantenha uma estação de trabalho administrativa dedicada que é mantida bloqueada em um escritório ou em uma gaveta quando não estiver em uso. Ou pode significar que quando você precisa realizar a administração do Active Directory ou seus controladores de domínio, você faça logon no controlador de domínio diretamente.  
  
Em organizações de médio porte, você pode considerar seguros administrativas "jump servidores" que estão localizados em um local seguro em um escritório e são usadas quando é necessário o gerenciamento de controladores de domínio do Active Directory. Você também pode implementar as estações de trabalho administrativas que são bloqueadas em locais seguros quando não estiver em uso, com ou sem servidores de salto.  
  
Em grandes organizações, você pode implantar servidores de salto hospedada em data center que fornecem acesso extremamente controlado para o Active Directory; controladores de domínio; e servidores de arquivos, impressão ou aplicativo. Implementação de uma arquitetura de servidor de salto é mais provável incluir uma combinação de servidores e estações de trabalho seguras em grandes ambientes.  
  
Independentemente do tamanho da sua organização e o design de seus hosts administrativos, você deve proteger os computadores físicos contra acesso não autorizado ou roubo e deve usar o BitLocker Drive Encryption para criptografar e proteger as unidades em hosts administrativos . Ao implementar o BitLocker em hosts administrativos, mesmo se um host for roubado ou seus discos são removidos, você pode garantir que os dados na unidade estão inacessíveis a usuários não autorizados.  
  
### <a name="operating-system-versions-and-configuration"></a>Configuração e as versões do sistema operacional  
Todos os hosts administrativos, se servidores ou estações de trabalho, deverá executar o sistema operacional mais recente em uso na sua organização pelos motivos descritos anteriormente neste documento. Executando sistemas operacionais atuais, os benefícios da sua equipe administrativa de novos recursos de segurança, suporte do fornecedor completa e funcionalidade adicional introduzidos no sistema operacional. Além disso, quando você estiver avaliando um novo sistema operacional, implantando-o primeiro para administrativas hosts, você precisará se familiarizar com os novos recursos, configurações e mecanismos de gerenciamento que ele oferece, que posteriormente podem ser utilizados no planejamento implantação mais ampla do sistema operacional. Até lá, os usuários mais sofisticados em sua organização também serão os usuários que estão familiarizados com o novo sistema operacional e melhor posicionada para dar suporte a ele.  
  
### <a name="microsoft-security-configuration-wizard"></a>Assistente de Configuração de Segurança da Microsoft  
Se você implementar servidores de salto como parte de sua estratégia de host administrativo, você deve usar o Assistente de configuração de segurança internas para configurar o serviço, registro, auditoria e configurações de firewall para reduzir a superfície de ataque do servidor. Quando as definições de configuração do Assistente de configuração de segurança foram coletadas e configuradas, as configurações podem ser convertidas em um GPO que é usado para impor uma configuração de linha de base consistente em todos os servidores de salto. Você pode editar ainda mais o GPO para implementar específico de configurações de segurança para servidores de atalho e pode combinar todas as configurações com as configurações de linha de base adicionais extraídas do Microsoft Security Compliance Manager.  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
O [Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) é uma ferramenta gratuita disponível que integra-se as configurações de segurança recomendadas pela Microsoft, com base na configuração de versão e a função do sistema operacional e os coleta em um uma única ferramenta e a interface do usuário que pode ser usado para criar e configurar as configurações de segurança de linha de base para controladores de domínio. Modelos do Microsoft Security Compliance Manager podem ser combinados com as configurações do Assistente de configuração de segurança para gerar linhas de base de configuração abrangente para servidores de salto que são implantados e aplicadas pelos GPOs implantadas em UOs no qual salto servidores são localizado no Active Directory.  
  
> [!NOTE]  
> Redação deste artigo, o Microsoft Security Compliance Manager não inclui configurações específicas para servidores de salto ou outros hosts administrativos seguros, mas ainda pode ser usado para criar linhas de base inicias para seu administrativas do Security Compliance Manager (SCM) hosts. Para proteger corretamente os hosts, no entanto, você deve aplicar as configurações de segurança adicionais apropriadas para servidores e estações de trabalho altamente seguras.  
  
### <a name="applocker"></a>AppLocker  
Hosts administrativos e machinesshould virtual ser configurado com script, ferramenta e listas de permissões de aplicativo por meio do AppLocker ou um aplicativo de terceiro restrição de software. Todos os aplicativos administrativos ou utilitários que não são compatíveis para configurações de segurança devem ser atualizados ou substituídos por ferramentas que obedece às práticas administrativas e de desenvolvimento seguro. Quando as ferramentas novas ou adicionais é necessária em um host administrativo, utilitários e aplicativos devem ser totalmente testados, e se as ferramentas são adequada para implantação nos hosts administrativos, podem ser adicionado a listas de permissões dos sistemas.  
  
### <a name="rdp-restrictions"></a>Restrições de RDP  
Embora a configuração específica vai variar dependendo da arquitetura dos seus sistemas administrativos, você deve incluir restrições em que as contas e computadores podem ser usados para estabelecer conexões de protocolo de área de trabalho remota (RDP) para sistemas gerenciados, como usar o Gateway de área de trabalho remota (Gateway RD) salta servidores para controlar o acesso aos controladores de domínio e outros sistemas gerenciados de sistemas e usuários autorizados.  
  
Você deve permitir logons interativos por usuários autorizados e deve remover ou até mesmo bloquear outros tipos de logon que não são necessárias para acesso ao servidor.  
  
### <a name="patch-and-configuration-management"></a>Gerenciamento de configuração e patches  
As organizações menores podem contar com ofertas como o Windows Update ou [Windows Server Update Services](https://technet.microsoft.com/windowsserver/bb332157) (WSUS) para gerenciar a implantação de atualizações nos sistemas Windows, enquanto que as organizações maiores podem implementar patch enterprise e software de gerenciamento de configuração, como o System Center Configuration Manager. Independentemente dos mecanismos usados para implantar atualizações em seu servidor geral e a população de estação de trabalho, você deve considerar implantações separadas para sistemas altamente seguros, como controladores de domínio, autoridades de certificação e hosts administrativos. Segregando esses sistemas contra a infraestrutura de gerenciamento geral, se suas contas de software ou serviço de gerenciamento estiverem comprometidas, o comprometimento não pode ser facilmente estendido para os sistemas mais seguros em sua infraestrutura.  
  
Embora você não deve implementar processos de atualização manual para sistemas seguros, você deve configurar uma infraestrutura separada para a atualização de sistemas seguros. Até mesmo em grandes organizações, essa infraestrutura geralmente pode ser implementada por meio de servidores dedicados do WSUS e GPOs para sistemas protegidos.  
  
### <a name="blocking-internet-access"></a>Bloqueando o acesso à Internet  
Hosts administrativos não devem ter permissão para acessar a Internet nem deveriam estar capazes de procurar a intranet da organização. Navegadores da Web e aplicativos semelhantes não devem ter permissão nos hosts administrativos. Você pode bloquear o acesso à Internet para hosts seguros por meio de uma combinação de configurações de firewall de perímetro, configuração do WFAS e configuração de proxy "buraco negro" hosts seguros. Você também pode usar a lista de permissões do aplicativo para impedir que os navegadores da web que está sendo usado em hosts administrativos.  
  
### <a name="virtualization"></a>Virtualização  
Sempre que possível, considere a implementação de máquinas virtuais como hosts administrativos. Usando a virtualização, você pode criar por usuário administrativos sistemas que são armazenados e gerenciados centralmente e que pode ser facilmente desligado quando não estiver em uso, garantindo que as credenciais não permanecem ativos nos sistemas administrativos. Você também pode exigir que hosts administrativos virtuais são redefinidos para um instantâneo inicial após cada uso, garantindo que as máquinas virtuais permaneçam puro. Para obter mais informações sobre opções de virtualização de hosts administrativos são fornecidas na seção a seguir.  
  
## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Exemplo de abordagens para implementar a proteger Hosts administrativos  
Independentemente de como projetar e implantar sua infraestrutura de host administrativo, você deve ter em mente as diretrizes fornecidas no "Princípios para criação de proteger Hosts administrativos" anteriormente neste tópico. Cada uma das abordagens descritas aqui fornece informações gerais sobre como você pode separar "administrativa" e sistemas de "produtividade" usados por sua equipe de TI. Sistemas de produtividade são computadores que os administradores de TI usam para verificar email, navegar pela Internet e usar o software de produtividade geral como o Microsoft Office. Sistemas administrativos são computadores que são protegidos e dedicados para usar a administração cotidiana de um ambiente de TI.  
  
A maneira mais simples de implementar hosts administrativos seguros é fornecer a sua equipe de TI com estações de trabalho protegidas do qual eles podem executar tarefas administrativas. Em uma implementação somente na estação de trabalho, cada estação de trabalho administrativa é usada para iniciar as ferramentas de gerenciamento e conexões de RDP para gerenciar servidores e outra infraestrutura. Implementações de estação de trabalho podem ser eficazes em organizações menores, embora infra-estruturas maiores e mais complexas podem se beneficiar de um design distribuído para hosts administrativos na qual dedicada administrativas servidores e estações de trabalho são usadas, como descrito em "Implementando Secure administrativas estações de trabalho e saltar servidores" neste tópico.  
  
### <a name="implementing-separate-physical-workstations"></a>Implementação de estações de trabalho físicas separadas  
Uma maneira que você pode implementar hosts administrativos é emitir duas estações de trabalho de cada usuário de TI. Uma estação de trabalho é usada com uma conta de usuário "normal" para executar atividades como verificação de email e usar aplicativos de produtividade, enquanto a segunda estação de trabalho é dedicada exclusivamente a funções administrativas.  
  
Para a estação de trabalho de produtividade, a equipe de TI pode receber as contas de usuário regular em vez de usar contas com privilégios para fazer logon computadores não protegidos. A estação de trabalho administrativa deve ser configurada com uma configuração controlada de forma rigorosa e a equipe de TI deve usar uma conta diferente para fazer logon na estação de trabalho administrativa.  
  
Se você tiver implementado a cartões inteligentes, estações de trabalho administrativas devem ser configuradas para exigir cartões inteligentes e a equipe de TI deve receber contas separadas para uso administrativo, também é configurado para exigir cartões inteligentes para logon interativo. O host administrativo deve ser protegido conforme descrito anteriormente, e somente usuários designados de TI devem ter permissão para fazer logon localmente na estação de trabalho administrativa.  
  
#### <a name="pros"></a>Vantagens  
Com a implementação de sistemas físicos separados, você pode garantir que cada computador está configurado adequadamente para sua função e que os usuários de TI não podem inadvertidamente expor sistemas administrativos a um risco.  
  
#### <a name="cons"></a>Desvantagens  
  
-   Implementação de computadores físicos separados aumenta os custos de hardware.  
  
-   Fazendo logon em um computador físico com as credenciais que são usadas para administrar sistemas remotos armazena em cache as credenciais na memória.  
  
-   Se estações de trabalho administrativas não são armazenadas com segurança, eles podem ser vulneráveis a comprometimento por meio de mecanismos como keyloggers hardware físico ou outros ataques físicos.  
  
### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implementando uma estação de trabalho física segura com uma estação de trabalho virtualizada de produtividade  
Nessa abordagem, os usuários de TI recebem uma estação de trabalho administrativa segura do qual eles podem executar funções administrativas diárias, usando ferramentas de administração de servidor remoto (RSAT) ou conexões RDP com servidores no seu escopo de responsabilidade. Quando os usuários de TI precisam para realizar tarefas de produtividade, eles podem se conectar via RDP para uma estação de trabalho de produtividade remota em execução como uma máquina virtual. Credenciais separadas devem ser usadas para cada estação de trabalho e controles, como cartões inteligentes devem ser implementados.  
  
#### <a name="pros"></a>Vantagens  
  
-   Estações de trabalho administrativas e estações de trabalho de produtividade são separadas.  
  
-   Usando estações de trabalho seguras para conectar-se em estações de trabalho de produtividade de equipe de TI pode usar credenciais separadas e cartões inteligentes e credenciais com altos privilégios não são depositadas no computador menos seguras.  
  
#### <a name="cons"></a>Desvantagens  
  
-   Implementação da solução exige design e opções de virtualização robusta e o trabalho de implementação.  
  
-   Se as estações de trabalho físicas não são armazenadas com segurança, eles podem estar vulneráveis a ataques físicos que comprometam o hardware ou o sistema operacional e torná-los suscetível à interceptação de comunicações.  
  
### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implementando uma única estação de trabalho segura com as conexões para separar "Produtividade" e "Administrativa" máquinas de virtuais  
Nessa abordagem, você pode emitir os usuários de TI uma única estação física que está bloqueado conforme descrito anteriormente e, em que os usuários de TI não tem acesso privilegiado. Você pode fornecer conexões de serviços de área de trabalho remota para máquinas virtuais hospedadas em servidores dedicados, fornecendo a equipe de TI com uma máquina virtual que executa o email e outros aplicativos de produtividade e uma segunda máquina virtual que está configurada como o usuário host administrativo dedicado.  
  
Você deve exigir cartão inteligente ou outro logon de vários fatores para as máquinas virtuais, usando contas separadas que não seja a conta que é usada para fazer logon no computador físico. Depois que um usuário de TI faz logon um computador físico, eles podem usar o cartão inteligente de produtividade para se conectar ao seu computador de produtividade remota e uma conta separada e cartão inteligente para se conectar ao seu computador do administrador remoto.  
  
#### <a name="pros"></a>Vantagens  
  
-   Os usuários de TI podem usar uma única estação de trabalho física.  
  
-   Por que exigem contas separadas para os hosts virtuais e usar conexões de serviços de área de trabalho remota para as máquinas virtuais, credenciais de usuários de TI não estão em cache na memória no computador local.  
  
-   O host físico pode ser protegido com o mesmo grau hosts administrativos, reduzindo a probabilidade de comprometimento do computador local.  
  
-   Em casos em que sua máquina virtual administrativa ou máquina de virtual de produtividade do usuário de TI pode ter sido comprometida, a máquina virtual podem ser redefinida facilmente para um estado de "boas".  
  
-   Se o computador físico for comprometido, sem credenciais com privilégios serão armazenados em cache na memória e o uso de cartões inteligentes pode impedir o comprometimento de credenciais por agentes de pressionamento de tecla.  
  
#### <a name="cons"></a>Desvantagens  
  
-   Implementação da solução exige design e opções de virtualização robusta e o trabalho de implementação.  
  
-   Se as estações de trabalho físicas não são armazenadas com segurança, eles podem estar vulneráveis a ataques físicos que comprometam o hardware ou o sistema operacional e torná-los suscetível à interceptação de comunicações.  
  
### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implementação de estações de trabalho administrativas seguras e servidores de salto  
Como uma alternativa para proteger as estações de trabalho administrativas ou em combinação com eles, você pode implementar servidores de salto segura, e os usuários administrativos podem se conectar aos servidores de salto usando RDP e cartões inteligentes para executar tarefas administrativas.  
  
Servidores de salto devem ser configurados para executar a função de Gateway de área de trabalho remota para que você possa implementar restrições em conexões com o servidor de salto e para servidores de destino que serão gerenciados a partir dele. Se possível, você também deve instalar a função Hyper-V e criar [áreas de trabalho virtuais pessoais](https://technet.microsoft.com/library/dd759174.aspx) ou outras máquinas de virtuais por usuário para usuários administrativos a ser usado para suas tarefas nos servidores de salto.  
  
Dando os usuários administrativos máquinas de virtuais por usuário no servidor de salto, fornecem segurança física para estações de trabalho administrativas e os usuários administrativos podem redefinir ou desligar suas máquinas virtuais quando não estiver em uso. Se você preferir não instalar a função Hyper-V e a função de Gateway de área de trabalho remota no mesmo host administrativo, você pode instalá-los em computadores separados.  
  
Sempre que possível, as ferramentas de administração remota devem ser usadas para gerenciar servidores. O recurso ferramentas de administração de servidor remoto (RSAT) deve ser instalado em máquinas virtuais de usuários (ou o servidor de salto, se você não estiver implementando máquinas de virtuais por usuário para a administração), e a equipe administrativa deve se conectar via RDP para seus máquinas virtuais para executar tarefas administrativas.  
  
Em casos quando um usuário administrativo deve se conectar via RDP para um servidor de destino para gerenciá-lo diretamente, o Gateway de área de trabalho remota deve ser configurado para permitir a conexão a ser feita apenas se o usuário apropriado e o computador são usadas para estabelecer a conexão para o destino servidor. Execução do RSAT (ou semelhante) ferramentas devem ser proibidas em sistemas que não estejam designados para sistemas de gerenciamento, como estações de trabalho de uso geral e servidores de membro que são não servidores de atalho.  
  
#### <a name="pros"></a>Vantagens  
  
-   Criar servidores de salto permite mapear servidores específicos para "zonas" (coleções de sistemas com requisitos semelhantes de configuração, conexão e segurança) em sua rede e exigem que a administração de cada zona é obtida com a equipe administrativa Conectando-se de hosts administrativos seguros em um servidor designado "zona".  
  
-   Mapeando os servidores de salto para zonas, você pode implementar controles granulares para os requisitos de configuração e propriedades de conexão e pode facilmente identificar tentativas de conexão de sistemas não autorizados.  
  
-   Com a implementação de máquinas de virtuais por administrador nos servidores de salto, você pode impor desligamento e a redefinição das máquinas virtuais para um estado limpo conhecido quando as tarefas administrativas são concluídas. Impondo desligamento (ou reinicialização) das máquinas virtuais quando as tarefas administrativas são concluídas, as máquinas virtuais não pode ser alvo de invasores, nem são ataques de roubo de credencial viável porque as credenciais em cache de memória não persistem além de uma reinicialização.  
  
#### <a name="cons"></a>Desvantagens  
  
-   Servidores dedicados são necessários para servidores de salto, seja ele físico ou virtual.  
  
-   Implementando designado saltar servidores e estações de trabalho administrativas requer um planejamento cuidadoso e configuração que é mapeado para todas as zonas de segurança configuradas no ambiente.  
  


