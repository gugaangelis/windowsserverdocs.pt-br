---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: Proteger controladores de domínio contra ataques
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b91164e14f3ae94f6f7d01c62125f45124df0907
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367624"
---
# <a name="securing-domain-controllers-against-attack"></a>Proteger controladores de domínio contra ataques

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Número da lei três: se uma pessoa mal-intencionada tiver acesso físico irrestrito ao seu computador, ele não será mais o seu computador.* - [dez leis imutáveis de segurança (versão 2,0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Os controladores de domínio fornecem o armazenamento físico para o banco de dados de AD DS, além de fornecer os serviços e os dados que permitem às empresas gerenciar efetivamente seus servidores, estações de trabalho, usuários e aplicativos. Se o acesso privilegiado a um controlador de domínio for obtido por um usuário mal-intencionado, esse usuário poderá modificar, corromper ou destruir o banco de dados AD DS e, por extensão, todos os sistemas e contas que são gerenciados pelo Active Directory.  
  
Como os controladores de domínio podem ler e gravar em qualquer coisa no banco de dados AD DS, o comprometimento de um controlador de domínio significa que sua Active Directory floresta nunca poderá ser considerada confiável novamente, a menos que você consiga se recuperar usando um backup bom conhecido e para Feche as lacunas que permitiam o comprometimento no processo.  
  
Dependendo da preparação, das ferramentas e das habilidades de um invasor, a modificação ou até mesmo danos irreparáveis ao banco de dados de AD DS podem ser concluídas em minutos a horas, não em dias ou semanas. O que importa não é por quanto tempo um invasor tem acesso privilegiado a Active Directory, mas quanto o invasor planejou durante o momento em que o acesso privilegiado é obtido. Comprometer um controlador de domínio pode fornecer o caminho mais vantajoso para a propagação de acesso em larga escala ou o caminho mais direto para a destruição de servidores membros, estações de trabalho e Active Directory. Por isso, os controladores de domínio devem ser protegidos separadamente e de forma mais restrita do que a infraestrutura geral do Windows.  

## <a name="physical-security-for-domain-controllers"></a>Segurança física para controladores de domínio

Esta seção fornece informações sobre a proteção física de controladores de domínio, se os controladores de domínio são máquinas físicas ou virtuais, em locais de datacenter, filiais e até mesmo locais remotos com apenas controles de infraestrutura básicos.  
  
### <a name="datacenter-domain-controllers"></a>Controladores de domínio do datacenter  
  
#### <a name="physical-domain-controllers"></a>Controladores de domínio físicos

Em data centers, os controladores de domínio físicos devem ser instalados em racks seguros dedicados ou em compartimentos separados da população geral do servidor. Quando possível, os controladores de domínio devem ser configurados com chips de Trusted Platform Module (TPM) e todos os volumes nos servidores do controlador de domínio devem ser protegidos via Criptografia de Unidade de Disco BitLocker. O BitLocker geralmente adiciona sobrecarga de desempenho em percentuais de dígito único, mas protege o diretório contra comprometimento, mesmo se os discos forem removidos do servidor. O BitLocker também pode ajudar a proteger sistemas contra ataques, como rootkits, pois a modificação de arquivos de inicialização fará com que o servidor inicialize no modo de recuperação para que os binários originais possam ser carregados. Se um controlador de domínio estiver configurado para usar RAID de software, SCSI anexado em série, armazenamento SAN/NAS ou volumes dinâmicos, o BitLocker não poderá ser implementado, portanto, o armazenamento localmente anexado (com ou sem RAID de hardware) deverá ser usado em controladores de domínio sempre que possível.  
  
#### <a name="virtual-domain-controllers"></a>Controladores de domínio virtual 

Se você implementar controladores de domínio virtual, deverá garantir que os controladores de domínio sejam executados em hosts físicos separados do que outras máquinas virtuais no ambiente. Mesmo que você use uma plataforma de virtualização de terceiros, considere implantar controladores de domínio virtuais no servidor Hyper-V no Windows Server 2012 ou no Windows Server 2008 R2, que fornece uma superfície de ataque mínima e pode ser gerenciado com os controladores de domínio que ele hospeda em vez de serem gerenciados com o restante dos hosts de virtualização. Se você implementar o System Center Virtual Machine Manager (SCVMM) para o gerenciamento de sua infraestrutura de virtualização, poderá delegar a administração para os hosts físicos nos quais as máquinas virtuais do controlador de domínio residem e os controladores de domínio a si mesmos para administradores autorizados. Você também deve considerar a separação do armazenamento de controladores de domínio virtuais para impedir que os administradores de armazenamento acessem os arquivos da máquina virtual.  
  
### <a name="branch-locations"></a>Localizações de filiais  
  
#### <a name="physical-domain-controllers-in-branches"></a>Controladores de domínio físico em branches

Em locais nos quais vários servidores residem, mas não estão fisicamente protegidos no grau em que os servidores de datacenter estão protegidos, os controladores de domínio físicos devem ser configurados com chips TPM e Criptografia de Unidade de Disco BitLocker para todos os volumes de servidor. Se um controlador de domínio não puder ser armazenado em uma sala bloqueada em locais de filiais, você deverá considerar a implantação de RODCs nesses locais.  
  
#### <a name="virtual-domain-controllers-in-branches"></a>Controladores de domínio virtual em branches

Sempre que possível, você deve executar controladores de domínio virtuais em filiais em hosts físicos separados do que as outras máquinas virtuais no site. Em filiais em que os controladores de domínio virtual não podem ser executados em hosts físicos separados do restante da população do Virtual Server, você deve implementar os chips do TPM e Criptografia de Unidade de Disco BitLocker em hosts nos quais os controladores de domínio virtual sejam executados no mínimo e todos os hosts, se possível. Dependendo do tamanho da filial e da segurança dos hosts físicos, você deve considerar a implantação de RODCs em locais de filiais.  
  
### <a name="remote-locations-with-limited-space-and-security"></a>Locais remotos com espaço limitado e segurança

Se sua infraestrutura incluir locais em que apenas um único servidor físico pode ser instalado, um servidor capaz de executar cargas de trabalho de virtualização deve ser instalado no local remoto e Criptografia de Unidade de Disco BitLocker deve ser configurado para proteger todos volumes no servidor. Uma máquina virtual no servidor deve executar um RODC, com outros servidores em execução como máquinas virtuais separadas no host. Informações sobre o planejamento da implantação do RODC são fornecidas no [Guia de planejamento e implantação do controlador de domínio somente leitura](https://go.microsoft.com/fwlink/?LinkID=135993). Para obter mais informações sobre como implantar e proteger controladores de domínio virtualizados, consulte [executando controladores de domínio no Hyper-V](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx) no site do TechNet. Para obter diretrizes mais detalhadas para proteger o Hyper-V, delegar o gerenciamento de máquinas virtuais e proteger máquinas virtuais, consulte o [Guia de segurança do Hyper-v](https://www.microsoft.com/download/details.aspx?id=16650) Solution Accelerator no site da Microsoft.  
  
## <a name="domain-controller-operating-systems"></a>Sistemas operacionais do controlador de domínio

Você deve executar todos os controladores de domínio na versão mais recente do Windows Server com suporte na sua organização e priorizar o encerramento de sistemas operacionais herdados na população do controlador de domínio. Ao manter os controladores de domínio atuais e eliminar os controladores de domínio herdados, você geralmente pode aproveitar as novas funcionalidades e a segurança que podem não estar disponíveis em domínios ou florestas com controladores de domínio que executam o sistema operacional herdado. Os controladores de domínio devem ser instalados e promovidos em vez de serem atualizados de sistemas operacionais ou funções de servidor anteriores; ou seja, não execute atualizações in-loco de controladores de domínio ou execute o assistente de instalação AD DS em servidores nos quais o sistema operacional não está instalado. Implementando os controladores de domínio instalados recentemente, você garante que os arquivos herdados e as configurações não sejam inadvertidamente deixados em controladores de domínio e você simplifique a imposição da configuração de controlador de domínio segura e consistente.  
  
## <a name="secure-configuration-of-domain-controllers"></a>Configuração segura de controladores de domínio

Várias ferramentas disponíveis gratuitamente, algumas das quais são instaladas por padrão no Windows, podem ser usadas para criar uma linha de base de configuração de segurança inicial para controladores de domínio que podem ser subsequentemente impostas por GPOs. Essas ferramentas são descritas aqui.  
  
### <a name="security-configuration-wizard"></a>Assistente de Configuração de Segurança  

Todos os controladores de domínio devem ser bloqueados na compilação inicial. Isso pode ser feito usando o assistente de configuração de segurança que é fornecido nativamente no Windows Server para configurar o serviço, o registro, o sistema e as configurações do WFAS em um controlador de domínio de "compilação básica". As configurações podem ser salvas e exportadas para um GPO que pode ser vinculado à UO Controladores de domínio em cada domínio na floresta para impor a configuração consistente dos controladores de domínio. Se o seu domínio contiver várias versões de sistemas operacionais Windows, você poderá configurar filtros Instrumentação de Gerenciamento do Windows (WMI) para aplicar GPOs somente aos controladores de domínio que executam a versão correspondente do sistema operacional.  
  
### <a name="microsoft-security-compliance-toolkit"></a>Kit de ferramentas de conformidade de segurança da Microsoft

As configurações do controlador de domínio [do Microsoft Security Compliance Toolkit](https://www.microsoft.com/download/details.aspx?id=55319) podem ser combinadas com as configurações do assistente de configuração de segurança para produzir linhas de base de configuração abrangentes para controladores de domínio implantados e impostos por GPOs implantados na UO Controladores de domínio em Active Directory.  
  
### <a name="rdp-restrictions"></a>Restrições de RDP

Política de Grupo objetos vinculados a todos os controladores de domínio UOs em uma floresta devem ser configurados para permitir conexões RDP somente de usuários e sistemas autorizados (por exemplo, servidores de salto). Isso pode ser obtido por meio de uma combinação de configurações de direitos de usuário e configuração do WFAS e deve ser implementado em GPOs para que a política seja aplicada consistentemente. Se ele for ignorado, o próximo Política de Grupo atualizar retornará o sistema para sua configuração apropriada.  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>Gerenciamento de patch e configuração para controladores de domínio

Embora possa parecer bem intuitivo, você deve considerar a aplicação de patches em controladores de domínio e outros componentes críticos da infraestrutura separadamente da infraestrutura geral do Windows. Se você aproveitar o software de gerenciamento de configuração empresarial para todos os computadores em sua infraestrutura, o comprometimento do software de gerenciamento de sistemas poderá ser usado para comprometer ou destruir todos os componentes de infraestrutura gerenciados por esse software. Ao separar o patch e o gerenciamento de sistemas para controladores de domínio da população geral, você pode reduzir a quantidade de software instalado em controladores de domínio, além de controlar rigidamente seu gerenciamento.
  
### <a name="blocking-internet-access-for-domain-controllers"></a>Bloqueando o acesso à Internet para controladores de domínio  

Uma das verificações executadas como parte de uma avaliação de segurança Active Directory é o uso e a configuração do Internet Explorer em controladores de domínio. O Internet Explorer (ou qualquer outro navegador da Web) não deve ser usado em controladores de domínio, mas a análise de milhares de controladores de domínio revelou vários casos em que usuários com privilégios usaram o Internet Explorer para navegar na intranet da organização ou na IP.  
  
Conforme descrito anteriormente na seção "configuração incorreta" de [caminhos para comprometer](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), navegar na Internet (ou em uma intranet infectada) de um dos computadores mais potentes em uma infraestrutura do Windows usando uma conta altamente privilegiada (que são as únicas contas permitidas para fazer logon localmente em controladores de domínio por padrão) apresenta um risco extraordinário à segurança de uma organização. Seja por meio de uma unidade por download ou por download de "utilitários" infectados por malware, os invasores podem obter acesso a tudo o que precisam para comprometer completamente ou destruir o ambiente de Active Directory.  
  
Embora o Windows Server 2012, o Windows Server 2008 R2, o Windows Server 2008 e as versões atuais do Internet Explorer ofereçam várias proteções contra downloads mal-intencionados, na maioria dos casos em que os controladores de domínio e contas com privilégios foram usados para Navegue pela Internet, os controladores de domínio estavam executando o Windows Server 2003 ou as proteções oferecidas por sistemas operacionais e navegadores mais recentes foram desabilitadas intencionalmente.  
  
Iniciar navegadores da Web em controladores de domínio deve ser proibido não apenas pela política, mas por controles técnicos e controladores de domínio não devem ter permissão para acessar a Internet. Se os controladores de domínio precisarem ser replicados entre sites, você deverá implementar conexões seguras entre os sites. Embora as instruções de configuração detalhadas estejam fora do escopo deste documento, você pode implementar vários controles para restringir a capacidade dos controladores de domínio de serem usados incorretamente ou que foram configurados de incorretas e, posteriormente, comprometidos.  
  
### <a name="perimeter-firewall-restrictions"></a>Restrições de firewall do perímetro

Os firewalls de perímetro devem ser configurados para bloquear conexões de saída de controladores de domínio para a Internet. Embora os controladores de domínio possam precisar se comunicar entre os limites do site, os firewalls de perímetro podem ser configurados para permitir a comunicação entre sites seguindo as diretrizes fornecidas em [como configurar um firewall para domínios e relações de confiança](https://support.microsoft.com/kb/179442) no site suporte da Microsoft.  
  
### <a name="dc-firewall-configurations"></a>Configurações de firewall do DC  

Conforme descrito anteriormente, você deve usar o assistente de configuração de segurança para capturar as definições de configuração do firewall do Windows com segurança avançada em controladores de domínio. Você deve examinar a saída do assistente de configuração de segurança para garantir que os parâmetros de configuração do firewall atendam aos requisitos da sua organização e, em seguida, use GPOs para impor as definições de configuração.  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>Impedindo a navegação na Web de controladores de domínio

Você pode usar uma combinação de configuração do AppLocker, configuração de proxy "buraco negro" e configuração do WFAS para impedir que os controladores de domínio acessem a Internet e para evitar o uso de navegadores da Web em controladores de domínio.
