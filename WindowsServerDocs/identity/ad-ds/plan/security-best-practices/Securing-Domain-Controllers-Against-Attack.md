---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: Proteger controladores de domínio contra ataques
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6be2899e85b68578518d9de1805c287367608163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863087"
---
# <a name="securing-domain-controllers-against-attack"></a>Proteger controladores de domínio contra ataques

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Lei número três: Se uma pessoa mal-intencionada tiver acesso físico irrestrito ao seu computador, ele não é o computador mais.* - [10 leis imutáveis de segurança (versão 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Controladores de domínio fornecem o armazenamento físico para o banco de dados do AD DS, além de fornecer os serviços e dados que permitem que as empresas gerenciem com eficiência seus servidores, estações de trabalho, os usuários e aplicativos. Se o acesso privilegiado a um controlador de domínio é obtido por um usuário mal-intencionado, que o usuário pode modificar, corromper ou destruir o banco de dados do AD DS e, por extensão, todos os sistemas e as contas que são gerenciadas pelo Active Directory.  
  
Como controladores de domínio podem ler e gravar em qualquer coisa no banco de dados AD DS, o comprometimento de um controlador de domínio significa que sua floresta do Active Directory pode nunca ser considerada confiável novamente, a menos que você é capaz de recuperar usando um backup válido e, ao preencher as lacunas que permissão o comprometimento no processo.  
  
Dependendo da modificação de preparação, ferramentas e habilidades, ou até mesmo irreparável danos no AD DS de um invasor banco de dados pode ser concluído em minutos, horas, e não em dias ou semanas. O que não importa quanto tempo um invasor tem acesso privilegiado ao Active Directory, mas quanto o invasor foi planejado para o momento quando acesso privilegiado é obtido. Comprometer um controlador de domínio pode fornecer o caminho mais vantajoso para a propagação de acesso em larga escala, ou o caminho mais direto para a destruição de servidores membro, estações de trabalho e do Active Directory. Por isso, os controladores de domínio devem ser protegidos separadamente e de forma mais rigorosa do que a infra-estrutura geral do Windows.  

## <a name="physical-security-for-domain-controllers"></a>Segurança física dos controladores de domínio

Esta seção fornece informações sobre como proteger fisicamente os controladores de domínio, se os controladores de domínio são físicos ou máquinas virtuais, em locais de datacenter, filiais e locais remotos, mesmo com apenas os controles de infraestrutura básica.  
  
### <a name="datacenter-domain-controllers"></a>Controladores de domínio do Datacenter  
  
#### <a name="physical-domain-controllers"></a>Controladores de domínio físicos

Em data centers, controladores de domínio físicos devem ser instalados em racks seguros dedicados ou compartimentos são separados da população geral do servidor. Quando possível, controladores de domínio devem ser configurados com chips de Trusted Platform Module (TPM) e todos os volumes nos servidores de controlador de domínio devem ser protegidos por meio de criptografia de unidade de disco BitLocker. BitLocker geralmente adiciona sobrecarga de desempenho em porcentagens de dígito único, mas protege o diretório contra o comprometimento, mesmo se os discos sejam removidos do servidor. O BitLocker também pode ajudar a proteger sistemas contra ataques, como rootkits, porque a modificação de arquivos de inicialização fará com que o servidor para inicializar no modo de recuperação para que os binários originais podem ser carregados. Se um controlador de domínio está configurado para usar o software RAID SCSI anexado em série, o armazenamento SAN/NAS, ou volumes dinâmicos, o BitLocker não podem ser implementados, o armazenamento então conectado localmente (com ou sem o hardware de RAID) deve ser usado em controladores de domínio sempre que é possível.  
  
#### <a name="virtual-domain-controllers"></a>Controladores de domínio virtual 

Se você implementar controladores de domínio virtual, você deve garantir que os controladores de domínio executam em hosts físicos separados que outras máquinas virtuais no ambiente. Mesmo se você usar uma plataforma de virtualização de terceiros, considere a implantação de controladores de domínio virtual no servidor Hyper-V no Windows Server 2012 ou Windows Server 2008 R2, que fornece uma superfície de ataque mínima e podem ser gerenciados com os controladores de domínio que ele hospeda em vez de sendo gerenciadas com o restante dos hosts de virtualização. Se você implementar o System Center Virtual Machine Manager (SCVMM) para o gerenciamento de sua infraestrutura de virtualização, você pode delegar a administração para os hosts físicos nos quais máquinas de virtuais do controlador de domínio residem e os controladores de domínio si mesmos para os administradores autorizados. Você também deve considerar a separar o armazenamento dos controladores de domínio virtuais para impedir que os administradores de armazenamento acessem os arquivos de máquina virtual.  
  
### <a name="branch-locations"></a>Locais de filiais  
  
#### <a name="physical-domain-controllers-in-branches"></a>Controladores de domínio físicos em ramificações

Em locais em que vários servidores residem, mas não estejam fisicamente protegidos para o grau que os servidores do datacenter são protegidos, controladores de domínio físico devem ser configurados com chips do TPM e o BitLocker Drive Encryption para todos os volumes do servidor. Se um controlador de domínio não pode ser armazenado em uma sala trancada filiais, você deve considerar a implantação de RODCs nesses locais.  
  
#### <a name="virtual-domain-controllers-in-branches"></a>Controladores de domínio virtual em ramificações

Sempre que possível, você deve executar controladores de domínio virtual em filiais em hosts físicos separados que as outras máquinas virtuais no site. Em filiais em que os controladores de domínio virtual não podem executar em hosts físicos separados do restante da população do servidor virtual, você deve implementar chips do TPM e o BitLocker Drive Encryption em hosts nos quais os controladores de domínio virtual executam no mínimo, e todos os hosts se possível. Dependendo do tamanho da filial e a segurança dos hosts físicos, você deve considerar a implantação de RODCs em filiais.  
  
### <a name="remote-locations-with-limited-space-and-security"></a>Locais remotos com espaço limitado e segurança

Se sua infraestrutura inclui locais em que apenas um único servidor físico pode ser instalado, um servidor capaz de executar cargas de trabalho de virtualização deve ser instalado no local remoto e a criptografia de unidade de disco BitLocker deve ser configurada para proteger todos os volumes no servidor. Uma máquina virtual no servidor deve executar um RODC, com outros servidores em execução como máquinas virtuais separadas no host. Informações sobre o planejamento para implantação do RODC é fornecida na [guia de implantação e planejamento do controlador de domínio somente leitura](https://go.microsoft.com/fwlink/?LinkID=135993). Para obter mais informações sobre como implantar e proteger os controladores de domínio virtualizados, consulte [executando controladores de domínio no Hyper-V](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx) no site do TechNet. Para obter mais diretrizes para otimizar a segurança do Hyper-V, delegando o gerenciamento de máquina virtual e, em seguida, protegendo as máquinas virtuais, consulte a [guia de segurança do Hyper-V](https://www.microsoft.com/download/details.aspx?id=16650) Solution Accelerator, no site da Microsoft.  
  
## <a name="domain-controller-operating-systems"></a>Sistemas operacionais do controlador de domínio

Todos os controladores de domínio deve ser executado na versão mais recente do Windows Server que tem suporte em sua organização e priorizar o encerramento de sistemas operacionais herdados da população de controlador de domínio. Ao manter controladores de domínio herdado atual e a eliminação de seus controladores de domínio, você geralmente pode tirar proveito da nova funcionalidade e à segurança que pode não estar disponível em florestas ou domínios com controladores de domínio executando o sistema operacional herdado. Controladores de domínio devem ser recentemente instalados e promovidos em vez de atualizado de sistemas operacionais anteriores ou funções de servidor; ou seja, não execute atualizações in-loco de controladores de domínio ou executar o Assistente de instalação do AD DS nos servidores em que o sistema operacional não está instalado recentemente. Implementando controladores de domínio recém-instalada, verifique se as configurações e arquivos herdados não são inadvertidamente deixadas nos controladores de domínio, e é simplificar a imposição da configuração do controlador de domínio consistente e segura.  
  
## <a name="secure-configuration-of-domain-controllers"></a>Configuração segura de controladores de domínio

Várias ferramentas disponíveis gratuitamente, algumas das quais são instaladas por padrão no Windows, podem ser usada para criar uma linha de base de configuração de segurança iniciais para controladores de domínio que posteriormente pode ser imposta pelos GPOs. Essas ferramentas são descritas aqui.  
  
### <a name="security-configuration-wizard"></a>Assistente de Configuração de Segurança  

Todos os controladores de domínio devem ser bloqueados após a compilação inicial. Isso pode ser feito usando o Assistente de configuração de segurança que acompanha o modo nativo do Windows Server para configurar o serviço, registro, sistema e as configurações do WFAS em um controlador de domínio de "compilação básica". Configurações podem ser salvas e exportadas para um GPO que pode ser vinculado a UO de controladores de domínio em cada domínio na floresta para impor a consistência da configuração de controladores de domínio. Se seu domínio contém várias versões dos sistemas de operacionais do Windows, você pode configurar filtros de Windows Management Instrumentation (WMI) para aplicar GPOs apenas aos controladores de domínio executando a versão correspondente do sistema operacional.  
  
### <a name="microsoft-security-compliance-toolkit"></a>Kit de ferramentas de conformidade de segurança da Microsoft

[O Kit de ferramentas de conformidade de segurança Microsoft](https://www.microsoft.com/download/details.aspx?id=55319) configurações do controlador de domínio podem ser combinadas com as configurações do Assistente de configuração de segurança para gerar linhas de base de configuração abrangente para os controladores de domínio que são implantados e aplicadas pelos GPOs implantado na UO de controladores de domínio no Active Directory.  
  
### <a name="rdp-restrictions"></a>Restrições de RDP

Objetos de diretiva de grupo que se vinculam a todos os controladores de domínio UOs em uma floresta deve ser configurados para permitir conexões RDP somente de usuários autorizados e sistemas (por exemplo, servidores de salto). Isso pode ser obtido por meio de uma combinação de configurações de direitos de usuário e a configuração do WFAS e deve ser implementado em GPOs para que a política é aplicada de forma consistente. Se ele é ignorado, a próxima atualização da diretiva de grupo retorna o sistema para a configuração apropriada.  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>Gerenciamento de configuração para controladores de domínio e patches

Embora possa parecer contra-intuitivo, você deve considerar a aplicação de patch de controladores de domínio e outros componentes de infraestrutura crítica separadamente de sua infraestrutura geral do Windows. Se você utilizar software de gerenciamento de configuração do enterprise para todos os computadores na sua infraestrutura, o comprometimento do software de gerenciamento de sistemas pode ser usado para comprometer ou destruir todos os componentes de infraestrutura gerenciados pelo software. Ao separar o gerenciamento de patches e sistemas para controladores de domínio da população geral, você pode reduzir a quantidade de software instalado nos controladores de domínio, além de controlar rigidamente as de gerenciamento.
  
### <a name="blocking-internet-access-for-domain-controllers"></a>Bloqueando o acesso à Internet para controladores de domínio  

Uma das verificações que é executada como parte de uma avaliação de segurança do Active Directory é o uso e a configuração do Internet Explorer em controladores de domínio. Internet Explorer (ou qualquer outro navegador da web) não deve ser usado em controladores de domínio, mas a análise de milhares de controladores de domínio revelou numerosos casos em que os usuários com privilégios usaram o Internet Explorer para procurar a intranet da organização ou o Na Internet.  
  
Conforme descrito anteriormente na seção "Configuração incorreta" [alternativas ao comprometimento](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), navegação na Internet (ou uma intranet infectada) de um dos computadores mais poderosos em uma infraestrutura do Windows usando um altamente privilegiados conta (que são as únicas contas que tem permitidas para fazer logon localmente nos controladores de domínio por padrão) apresenta um risco extraordinário para segurança da sua organização. Se por meio de uma unidade por download ou por download de "utilitários" infectados por malware, os invasores podem ganhar acesso a tudo o que eles precisam para completamente comprometer ou destruir o ambiente do Active Directory.  
  
Embora o Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 e versões atuais do Internet Explorer oferecem uma série de proteções contra downloads mal-intencionados, na maioria dos casos em que domínio controladores e contas privilegiadas tinham sido usadas para navegar pela Internet, os controladores de domínio foram executando o Windows Server 2003 ou proteções oferecidas por mais recentes sistemas operacionais e navegadores tinham intencionalmente desabilitadas.  
  
Iniciar os navegadores da web em controladores de domínio deve ser proibido não apenas pela política, mas por controles técnicos e controladores de domínio não devem ter permissão para acessar a Internet. Se precisam de seus controladores de domínio replicar entre sites, você deve implementar conexões seguras entre os sites. Embora instruções de configuração detalhadas estejam fora do escopo deste documento, você pode implementar uma série de controles para restringir a capacidade dos controladores de domínio para ser usado indevidamente ou configurado incorretamente e subsequentemente comprometida.  
  
### <a name="perimeter-firewall-restrictions"></a>Restrições de Firewall de perímetro

Firewalls de perímetro devem ser configurados para bloquear conexões de saída de controladores de domínio para a Internet. Embora os controladores de domínio podem precisar se comunicar entre os limites de site, firewalls de perímetro podem ser configurados para permitir a comunicação entre sites, seguindo as diretrizes fornecidas nas [como configurar um firewall para domínios e relações de confiança](https://support.microsoft.com/kb/179442) no site da Microsoft Support.  
  
### <a name="dc-firewall-configurations"></a>Configurações de Firewall do controlador de domínio  

Conforme descrito anteriormente, você deve usar o Assistente de configuração de segurança para capturar as configurações de configuração para o Firewall do Windows com segurança avançada em controladores de domínio. Você deve examinar a saída do Assistente de configuração de segurança para garantir que as definições de configuração de firewall atender aos requisitos da sua organização e, em seguida, usam GPOs para impor definições de configuração.  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>Impedindo a navegação na Web de controladores de domínio

Você pode usar uma combinação de configuração do AppLocker, "buraco negro" configuração de proxy e configuração do WFAS para impedir que os controladores de domínio ao acessar a Internet e para evitar o uso de navegadores da web em controladores de domínio.
