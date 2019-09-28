---
title: Configurações compatíveis para os Serviços de Área de Trabalho Remota
description: Fornece informações sobre configurações compatíveis para o RDS no Windows Server 2016.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/20/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: 7363fe3eee33a5345a25c8df9e4216b9eda7e3b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403803"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>Configurações compatíveis para os Serviços de Área de Trabalho Remota no Windows Server 2016

> Aplica-se a: Windows Server 2016

Quando se trata de configurações compatíveis para ambientes de Serviços de Área de Trabalho Remota, a maior preocupação tende a ser a interoperabilidade de versões. A maioria dos ambientes incluem várias versões do Windows Server – por exemplo, você pode ter uma implantação de RDS do Windows Server 2012 R2 existente mas deseja atualizar para o Windows Server 2016 para tirar proveito dos novos recursos (tais como suporte para OpenGL\OpenCL, Atribuição de Dispositivo Discreto ou Espaços de Armazenamento Diretos). A pergunta então torna-se: quais componentes do RDS podem trabalhar com diferentes versões em quais a versão precisa ser a mesma?

Portanto, com isso em mente, aqui estão as diretrizes básicas para configurações compatíveis dos Serviços de Área de Trabalho Remota no Windows Server 2016.

> [!NOTE]
> Não deixe de analisar os [requisitos do sistema para o Windows Server 2016](../../get-started/system-requirements.md).

## <a name="best-practices"></a>Práticas recomendadas
- Use o Windows Server 2016 para sua infraestrutura de Área de Trabalho Remota – Acesso via Web, Gateway, Agente de Conexão e servidor de licença. O Windows Server 2016 é compatível com versões anteriores para esses componentes, portanto, um Host da Sessão RD do 2012 R2 pode se conectar a um Agente de Conexão de Área de Trabalho Remota 2016, mas o inverso não é verdadeiro.

- Para Hosts da Sessão RD – todos os Hosts da Sessão em uma coleção precisam estar no mesmo nível, mas você pode ter várias coleções. Você pode ter uma coleção com Hosts da Sessão do Windows Server 2012 R2 e outra com Hosts da Sessão do Windows Server 2016.

- Se você atualizar seu Host da Sessão RD para o Windows Server 2016, atualize também o servidor de licença. Lembre-se de que um servidor de licença 2016 pode processar CALs de todas as versões anteriores do Windows Server desde o Windows Server 2003.

- Siga a ordem de atualização recomendada em [Atualizar o ambiente de Serviços de Área de Trabalho Remota](upgrade-to-rds.md#flow-for-deployment-upgrades). 

- Se você estiver criando um ambiente altamente disponível, todos os seus Agentes de Conexão precisarão ter o mesmo nível de sistema operacional.

## <a name="rd-connection-brokers"></a>Agentes de Conexão de Área de Trabalho Remota

O Windows Server 2016 remove a restrição para o número de Agentes de Conexão que você pode ter em uma implantação ao usar RDSHs (Hosts da Sessão da Área de Trabalho Remota) e RDVHs (Hosts de Virtualização de Área de Trabalho Remota) que também executam o Windows Server 2016. A tabela a seguir mostra quais versões de componentes do RDS funcionam com as versões 2016 e 2012 R2 do Agente de Conexão em uma implantação altamente disponível com três ou mais Agentes de Conexão.

| Três ou mais Agentes de Conexão em HA              | RDSH 2016 | RDVH 2016 | RDSH 2012 R2  | RDVH 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Agente de Conexão do Windows Server 2016    | Com suporte | Com suporte | Com suporte     | Com suporte     |
| Agente de Conexão do Windows Server 2012 R2 | N/D       | N/D       | Com suporte     | Com suporte     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>Suporte para aceleração de GPU com Hyper-V
A tabela a seguir fornece detalhes sobre o suporte para aceleração de GPU em máquinas virtuais. Veja [Qual tecnologia de virtualização de gráficos é ideal para você?](rds-graphics-virtualization.md) a fim de obter ajuda para descobrir o que você precisa. Para obter informações específicas sobre DDA, confira [Planejar a implantação de Atribuição de Dispositivo Discreto](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

|Sistema operacional convidado da VM  |Windows Server 2012 R2 ou Windows Server 2016<br> vGPU RemoteFX do Hyper-V (VM Geração 1) |  vGPU RemoteFX do Hyper-V no Windows Server 2016 (VM Geração 2) |  Atribuição de dispositivo discreto do Hyper-V no Windows Server 2016 (VM Geração 2) |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | Sim                                                        | Não                                                     | Não                                                                  |
| Windows 8.1                 | Sim                                                        | Não                                                     | Não                                                                  |
| Windows 10 Atualização 1511      | Sim                                                        | Sim                                                    | Sim                                                                 |
| Windows Server 2012 R2      | Sim                                                        | Não                                                     | Sim (requer o KB 3133690)                                           |
| Windows Server 2016         | Sim                                                        | Sim                                                    | Sim                                                                 |
| Windows Server 2012 R2 RDSH | Não                                                         | Não                                                     | Sim (requer o KB 3133690)                                           |
| Windows Server 2016 RDSH    | Não                                                         | Não                                                     | Sim                                                                 |
## <a name="vdi-deployment--supported-guest-oss"></a>Implantação de VDI – sistemas operacionais convidados compatíveis 
Servidores do Host de Virtualização de Área de Trabalho Remota do Windows Server 2016 dão suporte aos seguintes sistemas operacionais convidados:

- Windows 10 Enterprise
- Windows 8.1 Enterprise 
- Windows 8 Enterprise 
- Windows 7 SP1 Enterprise 

A tabela a seguir mostra as combinações de sistemas operacionais convidados e sistemas operacionais de Hosts de Virtualização de Área de Trabalho Remota com suporte:

| Versão do SO do RDVH        | Versão do SO convidado           |
| ------------- |-------------|
| Windows Server 2016      | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012 R2   | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012      | Windows 7 SP1, Windows 8, Windows 8.1 |

> [!NOTE]  
> - Os Serviços de Área de Trabalho Remota do Windows Server 2016 não são compatíveis com coleções heterogêneas. Todas as VMs em uma coleção precisam ter a mesma versão de sistema operacional. 
> - Você pode ter coleções homogêneas separadas com versões do SO convidado diferentes no mesmo host. 
> - Modelos de VM devem ser criados em um host Hyper-V do Windows Server 2016 para serem usados como o sistema operacional convidado em um host Hyper-V do Windows Server 2016.

## <a name="single-sign-on-sso"></a>Logon único (SSO)
O RDS do Windows Server 2016 dá suporte a duas experiências principais de SSO:

 - No aplicativo (aplicativo de Área de Trabalho Remota no Windows, iOS, Android e Mac)
 - SSO da Web
 
Usando o aplicativo de Área de Trabalho Remota, você pode armazenar credenciais como parte das informações de conexão ([Mac](clients/remote-desktop-mac.md)) ou como parte das contas gerenciadas ([iOS](clients/remote-desktop-ios.md#manage-your-user-accounts), [Android](clients/remote-desktop-android.md#manage-your-user-accounts), Windows) com segurança por meio de mecanismos exclusivos para cada sistema operacional.

Para se conectar a áreas de trabalho e RemoteApps com o SSO por meio do cliente de Conexão de Área de Trabalho Remota nativo no Windows, você precisa se conectar à página da Web de Área de Trabalho Remota por meio do Internet Explorer. As opções de configuração a seguir são necessárias no lado do servidor. Nenhuma outra configuração é compatível com o SSO da Web:

 - Web de Área de Trabalho Remota definida para autenticação baseada em formulários (padrão)
 - Gateway de Área de Trabalho Remota definido para autenticação de senha (padrão)
 - Implantação do RDS definida para "Usar credenciais de Gateway de Área de Trabalho Remota para computadores remotos" (padrão) nas propriedades de Gateway de Área de Trabalho Remota

> [!NOTE]
> Devido às opções de configuração necessárias, o SSO da Web não é compatível com cartões inteligentes. Os usuários que fazem logon por meio de cartões inteligentes podem encontrar vários prompts nesse processo.

Para obter mais informações sobre a criação de implantação de VDI dos Serviços de Área de Trabalho Remota, confira [Configurações de segurança do Windows 10 compatíveis com o VDI dos Serviços de Área de Trabalho Remota](rds-vdi-supported-config.md).

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>Usar os Serviços de Área de Trabalho Remota com serviços de proxy de aplicativo

Você pode usar os Serviços de Área de Trabalho Remota, exceto o cliente da Web, com o [Proxy de Aplicativo do Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop). Os Serviços de Área de Trabalho Remota não dão suporte ao uso do [Proxy de Aplicativo Web](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server), que está incluído no Windows Server 2016 e versões anteriores.
