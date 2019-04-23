---
title: Funções de serviços de Área de Trabalho Remotas
description: Descreve os componentes de um serviço de hospedagem de área de trabalho.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: 48efbffa4f82b707b63e33e6416da43eb105221f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844717"
---
# <a name="remote-desktop-services-roles"></a>Funções de serviços de Área de Trabalho Remotas

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este artigo descreve as funções dentro de um ambiente de serviços de área de trabalho remota.

## <a name="remote-desktop-session-host"></a>Host de Sessão de Área de Trabalho Remota

O Host da sessão da área de trabalho remota (Host de sessão de área de trabalho remota) mantém os aplicativos baseados em sessão e compartilhar com usuários de desktops. Os usuários obtêm a essas áreas de trabalho e aplicativos por meio de um dos clientes de área de trabalho remota que são executados no Windows, MacOS, iOS e Android. Usuários também podem se conectar por meio de um navegador com suporte usando o cliente web.

Você pode organizar desktops e aplicativos em um ou mais servidores de Host de sessão de área de trabalho remota, chamados "coleções". Você pode personalizar essas coleções para grupos específicos de usuários dentro de cada locatário. Por exemplo, você pode criar uma coleção em que um grupo de usuários específicos pode acessar aplicativos específicos, mas qualquer pessoa fora do grupo designado por você não será capaz de acessar esses aplicativos.

Para implantações pequenas, você pode instalar aplicativos diretamente nos servidores Host de sessão de área de trabalho remota. Para implantações maiores, é recomendável criar uma imagem de base e provisionamento de máquinas virtuais a partir dessa imagem.

Você pode expandir coleções adicionando máquinas de virtuais do servidor de Host de sessão de área de trabalho remota para um farm de coleção com cada máquina virtual do RDSH dentro de uma coleção atribuída ao mesmo conjunto de disponibilidade. Isso fornece maior disponibilidade de coleção e aumenta a escala para dar suporte a mais usuários ou aplicativos com uso intenso de recursos.

Na maioria dos casos, vários usuários compartilham o mesmo servidor Host de sessão de área de trabalho remota, que utiliza com mais eficiência os recursos do Azure para uma solução de hospedagem da área de trabalho. Nessa configuração, os usuários devem entrar coleções com contas não administrativas. Você pode também dar alguns usuários acesso administrativo total à área de trabalho remota com a criação de coleções de área de trabalho de sessão pessoal.

Você pode personalizar ainda mais Criando e carregando um disco rígido virtual com o SO Windows Server que você pode usar como modelo para a criação de novas máquinas virtuais de Host de sessão de área de trabalho remota de áreas de trabalho.

Para obter mais informações, consulte os seguintes artigos:

* [Serviços de área de trabalho remota - armazenamento seguro de dados](rds-plan-secure-data-storage.md)
* [Carregar um VHD generalizado e usá-lo para criar novas VMs no Azure](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
* [Atualizar a coleção de RDSH (modelo de ARM)](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)

## <a name="remote-desktop-connection-broker"></a>Agente de Conexão de Área de Trabalho Remota

Agente de Conexão de área de trabalho remota (agente de Conexão RD) gerencia conexões da área de trabalho remotas de entrada para farms de servidores de Host de sessão de área de trabalho remota. Agente de Conexão de área de trabalho gerencia as conexões com ambas as coleções de áreas de trabalho completas e coleções de aplicativos remotos. Agente de Conexão de área de trabalho pode balancear a carga entre os servidores da coleção ao fazer novas conexões. Se desconecta uma sessão, o agente de Conexão de área de trabalho reconectará o usuário para o servidor Host de sessão de área de trabalho remota e sua sessão interrompida, ainda existe no farm do Host de sessão de área de trabalho remota.

Você precisará instalar certificados digitais no servidor do agente de Conexão de área de trabalho remota e o cliente para dar suporte a logon único e publicação de aplicativos de correspondência. Ao desenvolver ou testar uma rede, você pode usar um certificado gerado automaticamente e autoassinado. No entanto, os serviços lançados exigem um certificado digital da autoridade de certificação confiável. O nome que você fornecer o certificado deve ser o mesmo como o interno totalmente qualificado nome FQDN (domínio) da máquina de virtual do agente de Conexão de área de trabalho.

Você pode instalar o agente de Conexão de área de trabalho remota do Windows Server 2016 na mesma máquina virtual como o AD DS para reduzir os custos. Se você precisar escalar horizontalmente para mais usuários, você também pode adicionar máquinas virtuais de agente de Conexão de área de trabalho no mesma conjunto de disponibilidade para criar um cluster do agente de Conexão de área de trabalho.

Antes de criar um cluster do agente de Conexão de área de trabalho, você deve implantar um banco de dados do SQL Azure no ambiente do locatário ou criar um grupo de disponibilidade AlwaysOn do SQL Server.

Para obter mais informações, consulte os seguintes artigos:

* [Adicione o servidor do agente de Conexão de área de trabalho remota para a implantação e configurar a alta disponibilidade](rds-connection-broker-cluster.md)
* [Banco de dados SQL](desktop-hosting-service.md#sql-database) no serviço de hospedagem da área de trabalho.

## <a name="remote-desktop-gateway"></a>Gateway de Área de Trabalho Remota

Gateway de área de trabalho remota (Gateway RD) concede aos usuários acesso de redes públicas para áreas de trabalho do Windows e aplicativos hospedados nos serviços de nuvem do Microsoft Azure.

O componente de Gateway de área de trabalho remota usa Secure Sockets Layer (SSL) para criptografar o canal de comunicação entre clientes e o servidor. A máquina virtual de Gateway de área de trabalho remota deve ser acessível por meio de um endereço IP público que permite conexões de entrada TCP na porta 443 e conexões de UDP de entrada para a porta 3391. Isso permite que os usuários se conectam por meio da internet usando o protocolo de transporte de comunicação de HTTPS e o protocolo UDP, respectivamente.

Os certificados digitais instalados no servidor e cliente precisam corresponder para que isso funcione. Quando você estiver desenvolvendo ou testando uma rede, você pode usar um certificado gerado automaticamente e autoassinado. No entanto, um serviço lançado requer um certificado de autoridade de certificação confiável. O nome do certificado deve corresponder ao FQDN usado para acessar o Gateway de área de trabalho remota, se o FQDN é o nome DNS voltado para o exterior dos endereços IP públicos ou o registro DNS CNAME que aponta para o endereço IP público.

Para locatários com o menor número de usuários, as funções de acesso via Web RD e o Gateway de área de trabalho remota podem ser combinadas em uma única máquina virtual para reduzir o custo. Você também pode adicionar mais máquinas virtuais de Gateway de área de trabalho remota para um farm de servidores de Gateway de área de trabalho remota para aumentar a disponibilidade do serviço e escalar horizontalmente para mais usuários. Máquinas virtuais no maiores farms de servidores de Gateway de área de trabalho remota deve ser configuradas em um conjunto com balanceamento de carga. Afinidade de IP não é necessária quando você estiver usando o Gateway de área de trabalho remota em uma máquina virtual Windows Server 2016, mas é quando você o estiver executando em uma máquina de virtual do Windows Server 2012 R2.

Para obter mais informações, consulte os seguintes artigos:

* [Adicionar a alta disponibilidade para a frente da web via Web da área de trabalho remota e Gateway](rds-rdweb-gateway-ha.md)
* [Serviços de área de trabalho remota - acesso em qualquer lugar](rds-plan-access-from-anywhere.md)
* [Serviços de área de trabalho remota - a autenticação multifator](rds-plan-mfa.md)

## <a name="remote-desktop-web-access"></a>Acesso via Web da área de trabalho remota

Remotos da área de trabalho Web Access (acesso via Web RD) permite que os usuários acesse áreas de trabalho e aplicativos por meio de um portal da web e iniciá-los por meio do aplicativo de cliente de área de trabalho remota Microsoft nativo do dispositivo. Você pode usar o portal da web para publicar aplicativos para Windows e dispositivos clientes não Windows e áreas de trabalho do Windows, e você pode publicar também seletivamente áreas de trabalho ou aplicativos para usuários ou grupos específicos.

Acesso via Web RD precisa dos serviços de informações da Internet (IIS) para funcionar corretamente. Uma conexão HTTPS Hypertext Transfer Protocol Secure () fornece um canal de comunicações criptografadas entre os clientes e o servidor Web da área de trabalho remota. A máquina virtual de acesso via Web RD deve ser acessível por meio de um endereço IP público que permite conexões de TCP de entrada para a porta 443 para permitir que os usuários do locatário se conectam à internet usando o protocolo de transporte de comunicação HTTPS.

Certificados digitais de correspondência deve ser instalado no servidor e nos clientes. Para desenvolvimento e para fins de teste, isso pode ser um certificado gerado automaticamente e autoassinado. Para um serviço lançado, o certificado digital deve ser obtido de uma autoridade de certificação confiável. O nome do certificado deve corresponder o domínio nome FQDN (totalmente qualificado) usado para acessar o acesso via Web RD. FQDNs possíveis incluem o nome DNS voltado para o exterior para o endereço IP público e o registro DNS CNAME que aponta para o endereço IP público.

Para locatários com o menor número de usuários, você pode reduzir custos, combinando as cargas de trabalho do acesso via Web RD e o Gateway de área de trabalho remota em uma única máquina virtual. Você também pode adicionar máquinas virtuais de Web da área de trabalho remota a um farm de acesso via Web RD para aumentar a disponibilidade do serviço e escalar horizontalmente para mais usuários. Em um farm do acesso via Web RD com várias máquinas virtuais, você precisará configurar as máquinas virtuais em um conjunto com balanceamento de carga.

Para obter mais informações sobre como configurar o acesso via Web RD, consulte os seguintes artigos:

* [Configurar o cliente de web da área de trabalho remota para seus usuários](clients/remote-desktop-web-client-admin.md)
* [Criar e implantar uma coleção de serviços de área de trabalho remota](rds-create-collection.md)
* [Criar uma coleção de serviços de área de trabalho remota para áreas de trabalho e aplicativos para serem executados](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>Licenciamento de Área de Trabalho Remota

Servidores de licenciamento remota de área de trabalho (licenciamento RD) ativados permitir que os usuários se conectem os servidores de Host de sessão de área de trabalho remota que hospedam aplicativos e áreas de trabalho do locatário. Ambientes de locatário normalmente vêm com o servidor de licenciamento de área de trabalho remota já instalado, mas para ambientes hospedados, será necessário configurar o servidor no modo por usuário.

O provedor de serviços precisa de licenças de acesso de assinante (SALs) suficientes RDS para cobrir todos os exclusivos (não simultâneos) usuários autorizados que entrar serviço de cada mês. Provedores de serviços podem comprar serviços de infraestrutura do Microsoft Azure diretamente e podem adquirir SALs por meio do programa do contrato de licenciamento de provedor de serviço da Microsoft (SPLA). Clientes que buscam uma solução de área de trabalho hospedada devem comprar a solução completa de hospedado (do Azure e RDS) do provedor de serviço.

Locatários pequenos podem reduzir os custos ao combinar o servidor de arquivos e componentes de licenciamento de área de trabalho remota em uma única máquina virtual. Para fornecer maior disponibilidade do serviço, os locatários podem implantar duas máquinas virtuais de servidor de licenças de área de trabalho remota no mesmo conjunto de disponibilidade. Todos os servidores de área de trabalho remota no ambiente do locatário estão associados com ambos os servidores de licenças de área de trabalho remota para manter os usuários podem se conectar ao novas sessões, mesmo se um dos servidores fica inativo.

Para obter mais informações, consulte os seguintes artigos:

* [Licença de sua implantação do RDS com licenças de acesso para cliente (CALs)](rds-client-access-license.md)
* [Ativar o servidor de licença dos serviços de área de trabalho remota](rds-activate-license-server.md)
* [Acompanhar suas licenças de acesso de cliente dos serviços de área de trabalho remota (RDS CALs)](rds-track-cals.md)
* [Licenciamento por Volume da Microsoft: as opções para provedores de serviços de licenciamento](https://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)