---
title: Funções de Serviços de Área de Trabalho Remota
description: Descreve os componentes de um serviço de hospedagem de área de trabalho.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: lizross
ms.openlocfilehash: 42116323dce36b071b2af20ff46330a8d39c13ed
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963058"
---
# <a name="remote-desktop-services-roles"></a>Funções de Serviços de Área de Trabalho Remota

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Este artigo descreve as funções dentro de um ambiente de Serviços de Área de Trabalho Remota.

## <a name="remote-desktop-session-host"></a>Host de Sessão de Área de Trabalho Remota

O Host da Sessão RD (Host da Sessão de Área de Trabalho Remota) mantém os aplicativos e as áreas de trabalho com base em sessão que você compartilha com usuários. Os usuários obtêm acesso a essas áreas de trabalho e aplicativos por meio de um dos clientes de Área de Trabalho Remota que são executados no Windows, MacOS, iOS e Android. Usuários também podem se conectar por meio de um navegador com suporte usando o cliente Web.

É possível organizar áreas de trabalho e aplicativos em um ou mais servidores de Host da Sessão RD, chamadas "coleções". É possível personalizar essas coleções para grupos específicos de usuários dentro de cada locatário. Por exemplo, é possível criar uma coleção em que um grupo de usuários específicos pode acessar aplicativos específicos, mas qualquer pessoa fora do grupo designado por você não será capaz de acessar esses aplicativos.

Para implantações pequenas, é possível instalar aplicativos diretamente nos servidores Host da Sessão da Área de Trabalho Remota. Para implantações maiores, é recomendável compilar uma imagem de base e provisionar máquinas virtuais a partir dessa imagem.

É possível expandir coleções adicionando máquinas virtuais do servidor Host da Sessão da Área de Trabalho Remota para um farm de coleção com cada máquina virtual de RDSH dentro de uma coleção atribuída ao mesmo conjunto de disponibilidade. Isso fornece maior disponibilidade de coleção e aumenta a escala para dar suporte a mais usuários ou aplicativos com recursos pesados.

Na maioria dos casos, vários usuários compartilham o mesmo servidor Host da Sessão da Área de Trabalho Remota, o que utiliza com mais eficiência os recursos do Azure para uma solução de hospedagem de área de trabalho. Nessa configuração, os usuários devem entrar em coleções com contas não administrativas. Também é possível conceder a alguns usuários acesso administrativo completo à área de trabalho remota, criando coleções de área de trabalho de sessão pessoal.

É possível personalizar áreas de trabalho ainda mais criando e carregando um disco rígido virtual com o SO Windows Server que você pode usar como modelo para a criação de novas máquinas virtuais de Host da Sessão RD.

Para obter mais informações, confira os seguintes artigos:

* [Serviços de Área de Trabalho Remota - Armazenamento de dados seguro](rds-plan-secure-data-storage.md)
* [Carregar um VHD generalizado e usá-lo para criar novas VMs no Azure](/azure/virtual-machines/windows/upload-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)
* [Atualizar a coleção de RDSH (modelo ARM)](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)

## <a name="remote-desktop-connection-broker"></a>Agente de Conexão de Área de Trabalho Remota

O Agente de Conexão RD (Agente de Conexão de Área de Trabalho Remota) gerencia conexões de entrada da área de trabalho remota para farms de servidores Host da Sessão RD. As conexões do Agente de Conexão de Área de Trabalho Remota gerenciam as conexões com ambas as coleções de áreas de trabalho completas e coleções de aplicativos remotos. O Agente de Conexão de Área de Trabalho Remota pode balancear a carga entre os servidores da coleção ao fazer novas conexões. Se uma sessão desconecta, o Agente de Conexão de Área de Trabalho Remota reconectará o usuário ao servidor Host da Sessão da Área de Trabalho Remota e a sessão interrompida, que ainda existe no farm de servidores Host da Sessão da Área de Trabalho Remota.

Será necessário instalar certificados digitais correspondentes no servidor do Agente de Conexão de Área de Trabalho Remota e o cliente para dar suporte a logon único e publicação de aplicativos. Ao desenvolver ou testar uma rede, é possível usar um certificado autoassinado e gerado automaticamente. No entanto, os serviços lançados exigem um certificado digital de uma autoridade de certificação confiável. O nome que você der o certificado deve ser o mesmo que o FQDN (Nome de Domínio Totalmente Qualificado) interno da máquina virtual do Agente de Conexão de Área de Trabalho Remota.

É possível instalar o Agente de Conexão de Área de Trabalho Remota do Windows Server 2016 na mesma máquina virtual que o AD DS para reduzir os custos. Se for necessário escalar horizontalmente para mais usuários, também é possível adicionar máquinas virtuais de Agente de Conexão de Área de Trabalho Remota no mesmo conjunto de disponibilidade para criar um cluster do Agente de Conexão RD.

Antes de criar um cluster do Agente de Conexão RD, é necessário implantar um Banco de Dados SQL do Azure no ambiente do locatário ou criar um Grupo de Disponibilidade AlwaysOn do SQL Server.

Para obter mais informações, confira os seguintes artigos:

* [Adicionar o servidor do Agente de Conexão de Área de Trabalho Remota à implantação e configurar a alta disponibilidade](rds-connection-broker-cluster.md)
* [Banco de Dados SQL](desktop-hosting-service.md#sql-database) no serviço de hospedagem da área de trabalho.

## <a name="remote-desktop-gateway"></a>Gateway de Área de Trabalho Remota

O Gateway RD (Gateway de Área de Trabalho Remota) concede aos usuários de redes públicas acesso a áreas de trabalho do Windows e aplicativos hospedados nos serviços de nuvem do Microsoft Azure.

O componente de Gateway de Área de Trabalho Remota usa SSL (Secure Sockets Layer) para criptografar o canal de comunicação entre clientes e o servidor. A máquina virtual de Gateway de Área de Trabalho Remota deve ser acessível por meio de um endereço IP público que permite conexões de entrada TCP na porta 443 e conexões UDP de entrada na porta 3391. Isso permite que usuários conectem por meio da internet usando o protocolo de transporte de comunicação HTTPS e o protocolo UDP, respectivamente.

Os certificados digitais instalados no servidor e no cliente precisam ser correspondentes para que isso funcione. Ao desenvolver ou testar uma rede, é possível usar um certificado autoassinado e gerado automaticamente. No entanto, um serviço lançado exige um certificado de uma autoridade de certificação confiável. O nome do certificado deve corresponder ao FQDN usado para acessar o Gateway de Área de Trabalho Remota, quer seja o FQDN o nome DNS voltado ao exterior do endereço IP público ou o registro DNS CNAME que aponta para o endereço IP público.

Para locatários com o menor número de usuários, as funções de Acesso via Web da Área de Trabalho Remota e Gateway de Área de Trabalho Remota podem ser combinadas em uma única máquina virtual para reduzir os custos. Também é possível adicionar mais máquinas virtuais de Gateway de Área de Trabalho Remota a um farm de servidores de Gateway de Área de Trabalho Remota para aumentar a disponibilidade do serviço e escalar horizontalmente para mais usuários. Máquinas virtuais em farms de servidores de Gateway de Área de Trabalho Remota maiores devem ser configuradas em um conjunto com balanceamento de carga. Afinidade de IP não é necessária ao usar o Gateway de Área de Trabalho Remota em uma máquina virtual do Windows Server 2016, mas sim quando você estiver executando-o em uma máquina virtual do Windows Server 2012 R2.

Para obter mais informações, confira os seguintes artigos:

* [Adicionar alta disponibilidade ao front Gateway Web e Web da Área de Trabalho Remota](rds-rdweb-gateway-ha.md)
* [Serviços da Área de Trabalho Remota – Acesso de qualquer lugar](rds-plan-access-from-anywhere.md)
* [Serviços de Área de Trabalho Remota – Autenticação multifator](rds-plan-mfa.md)

## <a name="remote-desktop-web-access"></a>Acesso via Web à Área de Trabalho Remota

O Acesso via Web à RD (Acesso via Web à Área de Trabalho Remota) permite que os usuários acessem áreas de trabalho e aplicativos por meio de um portal da Web e os inicie usando o aplicativo cliente da Área de Trabalho Remota da Microsoft nativo do dispositivo. É possível usar o portal da Web para publicar áreas de trabalho e aplicativos para dispositivos clientes Windows e não Windows, sendo que também é possível publicar seletivamente áreas de trabalho ou aplicativos para usuários ou grupos específicos.

Acesso via Web à Área de Trabalho Remota precisa de IIS (Serviços de Informações da Internet) para funcionar corretamente. Uma conexão HTTPS (Hypertext Transfer Protocol Secure) fornece um canal de comunicações criptografadas entre os clientes e o servidor Web da Área de Trabalho Remota. A máquina virtual de Acesso via Web à Área de Trabalho Remota deve ser acessível por meio de um endereço IP público que permita conexões TCP de entrada na porta 443 para permitir que os usuários do locatário conectem a partir da internet usando o protocolo de transporte de comunicação HTTPS.

Certificados digitais correspondentes devem ser instalado no servidor e nos clientes. Para fins de desenvolvimento e teste, isso pode ser um certificado autoassinado e gerado automaticamente. Para um serviço lançado, o certificado digital deve ser obtido de uma autoridade de certificação confiável. O nome da entidade do certificado deve corresponder ao FQDN (Nome de Domínio Totalmente Qualificado) usado para acessar Acesso via Web da Área de Trabalho Remota. FQDNs possíveis incluem o nome DNS voltado ao exterior para o endereço IP público e o registro DNS CNAME que aponta para o endereço IP público.

Para locatários com menor número de usuários, é possível reduzir custos combinando as cargas de trabalho do Acesso via Web da Área de Trabalho Remota e do Gateway de Área de Trabalho Remota em uma única máquina virtual. Também é possível adicionar mais máquinas virtuais de Web RD a um farm de Acesso via Web da Área de Trabalho Remota para aumentar a disponibilidade do serviço e escalar horizontalmente para mais usuários. Em um farm de Acesso via Web da Área de Trabalho Remota com várias máquinas virtuais, será necessário configurar as máquinas virtuais em um conjunto com balanceamento de carga.

Para obter mais informações sobre como configurar o Acesso via Web da Área de Trabalho Remota, consulte os seguintes artigos:

* [Configurar o cliente Web da Área de Trabalho Remota para seus usuários](clients/remote-desktop-web-client-admin.md)
* [Criar e implantar uma coleção de Serviços de Área de Trabalho Remota](rds-create-collection.md)
* [Criar uma coleção de Serviços de Área de Trabalho Remota para aplicativos e áreas de trabalho para execução](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>Licenciamento de Área de Trabalho Remota

Servidores de Licenciamento RD (Licenciamento da Área de Trabalho Remota) ativados permitem que os usuários conectem aos servidores Host da Sessão da Área de Trabalho que hospedam os aplicativos e as áreas de trabalho do locatário. Ambientes de locatário normalmente vêm com o servidor de Licenciamento da Área de Trabalho Remota já instalado, mas para ambientes hospedados, será necessário configurar o servidor no modo por usuário.

O provedor de serviços necessita de SALs (Licenças de Acesso ao Assinante) de RDS suficientes para cobrir todos os usuários exclusivos autorizados (não simultâneos) que entram no serviço todos os meses. Provedores de serviços podem comprar Serviços de Infraestrutura do Microsoft Azure diretamente e podem adquirir SALs por meio do programa SPLA (Contrato de Licenciamento do Provedor de Serviços) da Microsoft. Clientes que buscam uma solução de área de trabalho hospedada devem comprar a solução hospedada completa (Azure e RDS) do provedor de serviço.

Locatários pequenos podem reduzir os custos ao combinar o servidor de arquivos e componentes de Licenciamento de Área de Trabalho Remota em uma única máquina virtual. Para fornecer maior disponibilidade do serviço, os locatários podem implantar duas máquinas virtuais do servidor de Licenças RD no mesmo conjunto de disponibilidade. Todos os servidores de área de trabalho remota no ambiente do locatário são associados com ambos os servidores de Licenças RD para manter os usuários capazes de conectar a novas sessões, mesmo se um dos servidores ficar inativo.

Para obter mais informações, confira os seguintes artigos:

* [Licença da implantação do RDS com CALs (Licenças de acesso para cliente)](rds-client-access-license.md)
* [Ativar o servidor de licenças de Serviços de Área de Trabalho Remota](rds-activate-license-server.md)
* [Controlar suas RDS CALs (CALs para Serviços de Área de Trabalho Remota)](rds-track-cals.md)
* [Licenciamento por Volume da Microsoft: opções de licenciamento para provedores de serviços](https://www.microsoft.com/Licensing/licensing-programs/spla-program.aspx)
