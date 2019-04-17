---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: "Protegendo controladores de domínio contra ataques"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc40d0ac536b128b799006214e360fa4d991ee44
ms.sourcegitcommit: 06a84f5caeab49b8480d6eed037aebbb59c77a9f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/14/2018
---
# <a name="securing-domain-controllers-against-attack"></a>Protegendo controladores de domínio contra ataques

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Lei número três: se uma pessoa mal-intencionada tem acesso físico irrestrito ao seu computador, ele não é mais o seu computador.* - [Dez leis imutáveis de segurança (versão 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Controladores de domínio fornecem o armazenamento físico para o banco de dados do AD DS, além de fornecer os serviços e os dados que permitem que as empresas gerenciem efetivamente seus servidores, estações de trabalho, os usuários e aplicativos. Se o acesso privilegiado para um controlador de domínio é obtido por um usuário mal-intencionado, que o usuário pode modificar, corromper ou destruir o banco de dados do AD DS e, por extensão, todos os sistemas e contas que são gerenciadas pelo Active Directory.  
  
Como controladores de domínio podem ler e gravar a nada no banco de dados do AD DS, o comprometimento de um controlador de domínio significa que floresta do Active Directory pode nunca ser considerada confiável novamente, a menos que você é capaz de recuperar usando um bom backup e para preencher as lacunas que permitia o comprometimento no processo.  
  
Dependendo de preparação, ferramentas e habilidades, modificação ou danos irreparáveis mesmo no AD DS de um invasor banco de dados pode ser concluído em minutos, horas, e não em dias ou semanas. O que realmente importa não está quanto tempo um invasor tem privilegiados acesso ao Active Directory, mas quanto o invasor planejou para o momento quando privilegiado acessa é obtido. Comprometer um controlador de domínio pode fornecer o caminho mais apropriado para a propagação de grande escala de acesso, ou o caminho mais direto para destruição do Active Directory, servidores membros e estações de trabalho. Por isso, os controladores de domínio devem ser protegidos separadamente e de forma mais rigorosa do que a infraestrutura geral do Windows.  

  
## <a name="physical-security-for-domain-controllers"></a>Segurança física para controladores de domínio  
Esta seção fornece informações sobre como proteger fisicamente controladores de domínio, se os controladores de domínio são físicos ou máquinas virtuais, em locais de datacenter, filiais e locais remotos até mesmo com apenas os controles de infraestrutura básica.  
  
### <a name="datacenter-domain-controllers"></a>Controladores de domínio de Datacenter  
  
#### <a name="physical-domain-controllers"></a>Controladores de domínio físico  
Em data centers, controladores de domínio físicos devem ser instalados em racks seguros dedicados ou que é separados da população geral do servidor. Quando possível, controladores de domínio devem ser configurados com chips Trusted Platform Module (TPM) e todos os volumes nos servidores do controlador de domínio devem ser protegidos por meio de criptografia de unidade de disco BitLocker. BitLocker geralmente adiciona sobrecarga de desempenho em porcentagens de único dígito, mas protege o diretório contra comprometimento, mesmo se os discos são removidos do servidor. O BitLocker também pode ajudar a proteger sistemas contra ataques como rootkits porque a modificação dos arquivos de inicialização fará com que o servidor ser inicializado no modo de recuperação para que os binários originais podem ser carregados. Se um controlador de domínio está configurado para usar o software RAID, SCSI anexado a série, armazenamento de SAN/NAS, ou volumes dinâmicos, o BitLocker não podem ser implementados, armazenamento anexado então localmente (com ou sem hardware RAID) deve ser usado em controladores de domínio sempre que possível.  
  
#### <a name="virtual-domain-controllers"></a>Controladores de domínio virtual  
Se você implementar controladores de domínio virtual, você deve garantir que os controladores de domínio executam em hosts físicos separados que outras máquinas virtuais no ambiente. Mesmo se você usar uma plataforma de virtualização de terceiros, pense em implantar controladores de domínio virtual no Hyper-V Server no Windows Server 2012 ou Windows Server 2008 R2, que fornece uma superfície de ataque mínimo e podem ser gerenciados com os controladores de domínio nele em vez de sendo gerenciados com o restante dos hosts virtualização. Se você implementar um sistema Center Virtual Machine Manager (SCVMM) para gerenciamento de sua infraestrutura de virtualização, você pode delegar a administração para os hosts físicos nas quais máquinas de virtuais de controlador de domínio residem e os controladores de domínio propriamente ditos aos administradores autorizados. Você também deve considerar separando o armazenamento dos controladores de domínio virtual para impedir que os administradores de armazenamento acessem os arquivos de máquina virtual.  
  
### <a name="branch-locations"></a>Filiais  
  
#### <a name="physical-domain-controllers"></a>Controladores de domínio físico  
Em locais em que vários servidores residem mas não são protegidos fisicamente para o nível de datacenter servidores são protegidos, controladores de domínio físicos devem ser configurados com chips TPM e criptografia de unidade de disco BitLocker para todos os volumes do servidor. Se um controlador de domínio não pode ser armazenado em uma sala trancada em filiais, você deve considerar a implantação RODCs nesses locais.  
  
#### <a name="virtual-domain-controllers"></a>Controladores de domínio virtual  
Sempre que possível, você deve executar controladores de domínio virtual em filiais em hosts físicos separados que as outras máquinas virtuais no site. Em filiais em que os controladores de domínio virtual não é possível executar em hosts físicos separados do restante da população de servidores virtuais, você deve implementar chips TPM e criptografia de unidade de disco BitLocker em hosts no qual os controladores de domínio virtual executam no mínimo e todos os hosts se possível. Dependendo do tamanho da filial e a segurança dos hosts físicos, você deve considerar a implantação RODCs em filiais.  
  
### <a name="remote-locations-with-limited-space-and-security"></a>Locais remotos com espaço limitado e segurança  
Se sua infraestrutura inclui locais em que somente um único servidor físico pode ser instalado, um servidor capaz de executar cargas de trabalho de virtualização deve ser instalado no local remoto e a criptografia de unidade de disco BitLocker devem ser configurada para proteger todos os volumes no servidor. Uma máquina virtual no servidor deve executar um RODC, com outros servidores que executam como máquinas virtuais separadas no host. Informações sobre o planejamento de implantação do RODC é fornecida no [planejamento de controlador de domínio somente leitura e guia de implantação](https://go.microsoft.com/fwlink/?LinkID=135993). Para obter mais informações sobre como implantar e proteger controladores de domínio virtualizado, consulte [controladores de domínio em execução em Hyper-V](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx) no site do TechNet. Para obter mais orientações para proteger o Hyper-V, delegando o gerenciamento de máquina virtual e proteção de máquinas virtuais, consulte o [guia de segurança do Hyper-V](https://www.microsoft.com/download/details.aspx?id=16650) Solution Accelerator no site da Microsoft.  
  
## <a name="domain-controller-operating-systems"></a>Sistemas de operacionais do controlador de domínio  
Você deve executar todos os controladores de domínio na versão mais recente do Windows Server que tem suporte em sua organização e priorizar a desativação de sistemas operacionais herdados da população de controlador de domínio. Mantendo os controladores de domínio controladores de domínio herdado atuais e eliminando, muitas vezes você pode tirar proveito de novos recursos e segurança que pode não estar disponível em domínios ou florestas com controladores de domínio que executam o sistema operacional herdado. Controladores de domínio devem ser recém-instalados e promovidos em vez de atualizado de sistemas operacionais anteriores ou funções de servidor; ou seja, não executar atualizações in-loco de controladores de domínio ou executar o Assistente de instalação do AD DS em servidores nos quais o sistema operacional não é instalado recentemente. Implementando controladores de domínio recém-instalado, você pode garantir que as configurações e arquivos herdados não são inadvertidamente deixadas em controladores de domínio e simplificar a imposição de configuration do controlador de domínio consistente e segura.  
  
## <a name="secure-configuration-of-domain-controllers"></a>Configuração segura de controladores de domínio  
Diversas ferramentas disponíveis gratuitamente, alguns dos quais são instalados por padrão no Windows, podem ser usada para criar uma linha de base de configuração de segurança inicial em controladores de domínio que posteriormente pode ser aplicada por GPOs. Essas ferramentas estão descritas aqui.  
  
### <a name="security-configuration-wizard"></a>Assistente de configuração de segurança  

Todos os controladores de domínio devem ser bloqueados após a compilação inicial. Isso pode ser feito usando o Assistente de configuração de segurança fornecido nativamente no Windows Server para configurar o serviço, do registro, sistema e configurações de WFAS em um controlador de domínio "compilação base". As configurações podem ser salvos e exportadas para um GPO que possa ser vinculado à UO de controladores de domínio em cada domínio na floresta para aplicar uma configuração consistente de controladores de domínio. Se seu domínio contém várias versões dos sistemas operacionais Windows, você pode configurar os filtros do Windows Management Instrumentation (WMI) para aplicar GPOs apenas em controladores de domínio executando a versão correspondente do sistema operacional.  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
[Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) as configurações do controlador de domínio podem ser combinadas com as configurações do Assistente de configuração de segurança para produzir linhas de base de configuração abrangente para controladores de domínio que são implantados e impostas pelos GPOs implantados na UO de controladores de domínio no Active Directory.  
  
### <a name="applocker"></a>AppLocker  
O AppLocker ou uma ferramenta de lista de exceções do aplicativo de terceiros deve ser usada para configurar serviços e aplicativos que têm permissão para executar em controladores de domínio, e esses serviços e aplicativos permitidos devem ser formados por apenas do que é necessário para o computador host AD DS e possivelmente DNS, além de qualquer software de segurança do sistema como um software antivírus. Pela lista branca permitida aplicativos em controladores de domínio, uma camada adicional de segurança é adicionada para que mesmo se um aplicativo não autorizado estiver instalado em um controlador de domínio, o aplicativo não pode ser executado.  
  
### <a name="rdp-restrictions"></a>Restrições de RDP  
Objetos de política de grupo vinculados a todos os controladores de domínio UOs em uma floresta deve ser configurados para permitir conexões RDP somente de sistemas (por exemplo, servidores de atalhos) e os usuários autorizados. Isso pode ser obtido por meio de uma combinação de configurações de direitos de usuário e a configuração de WFAS e deve ser implementado em GPOs para que a política é aplicada consistentemente. Se ela é ignorada, a próxima atualização da política de grupo retorna o sistema para sua configuração adequada.  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>Patch e gerenciamento de configuração para controladores de domínio  
Embora possa parecer contra-intuitivo, você deve considerar a aplicação de patch controladores de domínio e outros componentes de infraestrutura críticos separadamente de sua infraestrutura geral do Windows. Se você aproveitar o software de gerenciamento de configuração empresarial para todos os computadores em sua infraestrutura, comprometimento do software de gerenciamento de sistemas pode ser usado para comprometer ou destruir todos os componentes de infraestrutura gerenciados pelo software. Separando o gerenciamento de patches e sistemas para controladores de domínio da população geral, você pode reduzir a quantidade de software instalado em controladores de domínio, além de controlar totalmente o gerenciamento.  
  
### <a name="blocking-internet-access-for-domain-controllers"></a>Bloquear o acesso à Internet para controladores de domínio  

Uma das verificações que é realizada como parte de uma avaliação de segurança Active Directory é o uso e a configuração do Internet Explorer em controladores de domínio. Internet Explorer (ou qualquer outro navegador da web) não deve ser usado em controladores de domínio, mas a análise de milhares de controladores de domínio revelou vários casos em que os usuários com privilégios usados Internet Explorer para navegar intranet da empresa ou da Internet.  
  
Conforme descrito anteriormente na seção "Definição incorreta" [caminhos para comprometer](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), navegando na Internet (ou em uma intranet infectada) de um dos computadores mais eficientes em uma infraestrutura do Windows usando uma conta altamente privilegiada (que são as únicas contas permitidas para fazer logon localmente controladores de domínio por padrão) apresenta um risco à segurança de uma organização extraordinário. Seja por meio de uma unidade por download ou download do infectado por malware "utilitários", os invasores podem ter acesso a tudo o que eles precisam completamente comprometer ou destruir o ambiente do Active Directory.  
  
Embora o Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 e versões atuais do Internet Explorer oferecem um número de proteções contra downloads mal-intencionados, na maioria dos casos em que domínio controladores e contas privilegiadas tinham sido usadas para navegar na Internet, os controladores de domínio estavam executando o Windows Server 2003 ou proteções oferecidas por navegadores e sistemas operacionais mais recentes foram desabilitadas intencionalmente.  
  
Iniciar navegadores da web em controladores de domínio deve ser proibida não apenas por política, mas por controles técnicos e controladores de domínio não devem ter permissão para acessar a Internet. Se os controladores de domínio precisam replicar em sites, você deve implementar conexões seguras entre os sites. Embora as instruções de configuração detalhadas estão fora do escopo deste documento, você pode implementar uma série de controles para restringir a capacidade de controladores de domínio para ser usado incorretamente ou definidas incorretamente e subsequentemente comprometido.  
  
### <a name="perimeter-firewall-restrictions"></a>Restrições de Firewall do perímetro  
Firewalls do perímetro devem ser configurados para bloquear as conexões de saída de controladores de domínio com a Internet. Embora os controladores de domínio podem precisar se comunicar em fronteiras do site, firewalls de perímetro podem ser configurados para permitir a comunicação entre sites seguindo as orientações fornecidas na [como configurar um firewall para domínios e relações de confiança](https://support.microsoft.com/kb/179442) no site da Microsoft Support.  
  
### <a name="dc-firewall-configurations"></a>Configurações de Firewall de DC  

Conforme descrito anteriormente, você deve usar o Assistente de configuração de segurança para capturar as configurações do Firewall do Windows com segurança avançada em controladores de domínio. Você deve examinar a saída do Assistente de configuração de segurança para garantir que as configurações de firewall atender aos requisitos da sua organização e, em seguida, usam GPOs para aplicar configurações.  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>Impedindo a navegação na Web de controladores de domínio  
Você pode usar uma combinação de configuração do AppLocker, a configuração de proxy "buraco negro" e configuração WFAS para impedir que os controladores de domínio de acessar a Internet e para evitar o uso de navegadores da web em controladores de domínio.  
  


