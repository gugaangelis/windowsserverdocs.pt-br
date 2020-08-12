---
title: Configurações compatíveis para os Serviços de Área de Trabalho Remota
description: Fornece informações sobre configurações com suporte do RDS no Windows Server 2016 e no Windows Server 2019.
ms.author: elizapo
ms.date: 07/14/2020
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: 47aa9327e70d07ce46477024fb0c734ea1d64603
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954833"
---
# <a name="supported-configurations-for-remote-desktop-services"></a>Configurações compatíveis para os Serviços de Área de Trabalho Remota

> Aplica-se a: Windows Server 2016, Windows Server 2019

Quando se trata de configurações compatíveis para ambientes de Serviços de Área de Trabalho Remota, a maior preocupação tende a ser a interoperabilidade de versões. A maioria dos ambientes incluem várias versões do Windows Server – por exemplo, você pode ter uma implantação de RDS do Windows Server 2012 R2 existente mas deseja atualizar para o Windows Server 2016 para tirar proveito dos novos recursos (tais como suporte para OpenGL\OpenCL, Atribuição de Dispositivo Discreto ou Espaços de Armazenamento Diretos). A pergunta então torna-se: quais componentes do RDS podem trabalhar com diferentes versões em quais a versão precisa ser a mesma?

Portanto, com isso em mente, aqui estão as diretrizes básicas para configurações com suporte dos Serviços de Área de Trabalho Remota no Windows Server.

> [!NOTE]
> Não se esqueça de examinar os [requisitos do sistema do Windows Server 2016](../../get-started/system-requirements.md) e do [Windows Server 2019](../../get-started-19/sys-reqs-19.md).

## <a name="best-practices"></a>Práticas recomendadas

- Use o Windows Server 2019 para sua infraestrutura de Área de Trabalho Remota (Acesso via Web, Gateway, Agente de Conexão e servidor de licença). O Windows Server 2019 é compatível com versões anteriores desses componentes, o que significa que o um Host de Sessão do Windows Server 2016 ou do Windows Server 2012 R2 RD pode se conectar a um Agente de Conexão do 2019 RD, mas o contrário não é possível.

- Para Hosts da Sessão RD – todos os Hosts da Sessão em uma coleção precisam estar no mesmo nível, mas você pode ter várias coleções. É possível ter uma coleção com Hosts da Sessão do Windows Server 2016 e outra com Hosts da Sessão do Windows Server 2019.

- Se você atualizar o Host da Sessão RD para o Windows Server 2019, atualize também o servidor de licença. Lembre-se de que um servidor de licença 2019 pode processar CALs de todas as versões anteriores do Windows Server, desde o Windows Server 2003.

- Siga a ordem de atualização recomendada em [Atualizar o ambiente de Serviços de Área de Trabalho Remota](upgrade-to-rds.md#flow-for-deployment-upgrades).

- Se você estiver criando um ambiente altamente disponível, todos os seus Agentes de Conexão precisarão ter o mesmo nível de sistema operacional.

## <a name="rd-connection-brokers"></a>Agentes de Conexão de Área de Trabalho Remota

O Windows Server 2016 remove a restrição para o número de Agentes de Conexão que você pode ter em uma implantação ao usar RDSHs (Hosts da Sessão da Área de Trabalho Remota) e RDVHs (Hosts de Virtualização de Área de Trabalho Remota) que também executam o Windows Server 2016. A tabela a seguir mostra quais versões de componentes do RDS funcionam com as versões 2016 e 2012 R2 do Agente de Conexão em uma implantação altamente disponível com três ou mais Agentes de Conexão.

|Três ou mais Agentes de Conexão em HA|RDSH ou RDVH 2019|RDSH ou RDVH 2016|RDSH ou RDVH 2012 R2|
|---|---|---|---|
 |Agente de Conexão do Windows Server 2019|Suportado|Suportado|Suportado|
 |Agente de Conexão do Windows Server 2016|N/D|Suportado|Suportado|
 |Agente de Conexão do Windows Server 2012 R2|N/D|N/D|Sem suporte|

## <a name="support-for-graphics-processing-unit-gpu-acceleration"></a>Suporte à aceleração de GPU (unidade de processamento gráfico)

Sistemas de suporte dos Serviços de Área de Trabalho Remota equipados com GPUs. Aplicativos que exigem uma GPU podem ser usados pela conexão remota. Além disso, a renderização e a codificação com aceleração de GPU podem ser habilitadas para melhorar o desempenho e a escalabilidade do aplicativo.

Os Hosts de Sessão dos Serviços de Área de Trabalho Remota e sistemas operacionais de cliente de sessão única podem aproveitar as GPUs físicas ou virtuais apresentadas ao sistema operacional de várias formas, incluindo os [tamanhos de máquinas virtuais otimizadas da GPU do Azure](/en-us/azure/virtual-machines/windows/sizes-gpu), as GPUs disponíveis para o servidor físico do RDSH e GPUs apresentadas às VMs por hipervisores com suporte.

Veja [Qual tecnologia de virtualização de gráficos é ideal para você?](rds-graphics-virtualization.md) a fim de obter ajuda para descobrir o que você precisa. Para obter informações específicas sobre DDA, confira [Planejar a implantação de Atribuição de Dispositivo Discreto](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

Os fornecedores de GPU podem ter um esquema de licenças diferente para cenários de RDSH ou restringir o uso da GPU no sistema operacional do servidor, verifique os requisitos do seu fornecedor preferencial.

As GPUs apresentadas por um hipervisor que não é da Microsoft ou do Cloud Platform precisam ter drivers assinados digitalmente pelo WHQL e fornecidos pelo fornecedor da GPU.

### <a name="remote-desktop-session-host-support-for-gpus"></a>Suporte do Host de Sessão da Área de Trabalho Remota para GPUs

A tabela a seguir mostra os cenários com suporte de diferentes versões de hosts de RDSH.

|Recurso|Windows Server 2008 R2|Windows Server 2012 R2|Windows Server 2016|Windows Server 2019|
|---|---|---|---|---|
|Uso de GPU de hardware para todas as sessões RDP|Não|Sim|Sim|Sim|
|Codificação de hardware H.264/AVC (se houver suporte da GPU)|Não|Não|Sim|Sim|
|Balanceamento de carga entre várias GPUs apresentadas ao sistema operacional|Não|Não|Não|Sim|
|Otimizações de codificação H.264/AVC para minimizar o uso de largura de banda|Não|Não|Não|Sim|
|Suporte do H.264/AVC para resolução de 4K|Não|Não|Não|Sim|

### <a name="vdi-support-for-gpus"></a>Suporte da VDI para GPUs

A tabela a seguir mostra o suporte aos cenários de GPU no sistema operacional do cliente.

|Recurso|Windows 7 SP1|Windows 8.1|Windows 10|
|---|---|---|---|
|Uso de GPU de hardware para todas as sessões RDP|Não|Sim|Sim|
|Codificação de hardware H.264/AVC (se houver suporte da GPU)|Não|Não|Windows 10 1703 e posterior|
|Balanceamento de carga entre várias GPUs apresentadas ao sistema operacional|Não|Não|Windows 10 1803 e posterior|
|Otimizações de codificação H.264/AVC para minimizar o uso de largura de banda|Não|Não|Windows 10 1803 e posterior|
|Suporte do H.264/AVC para resolução de 4K|Não|Não|Windows 10 1803 e posterior|

### <a name="remotefx-3d-video-adapter-vgpu-support"></a>Suporte para a vGPU (Adaptador de vídeo RemoteFX 3D)

> [!NOTE]
> Devido a questões de segurança, a vGPU RemoteFX está desabilitada por padrão em todas as versões do Windows desde a Atualização de Segurança de 14 de julho de 2020. Para saber mais, confira [KB 4570006](https://support.microsoft.com/help/4570006).

Os Serviços de Área de Trabalho Remota oferecem suporte para as vGPUs do RemoteFX quando uma VM está em execução como um convidado do Hyper-V no Windows Server 2012 R2 ou Windows Server 2016. Os seguintes sistemas operacionais convidados oferecem suporte para a vGPU do RemoteFX:

- Windows 7 SP1
- Windows 8.1
- Windows 10 1703 ou posterior
- Windows Server 2016 em uma implantação de somente uma sessão

### <a name="discrete-device-assignment-support"></a>Suporte à Atribuição de Dispositivo Discreto

Os Serviços de Área de Trabalho Remota oferecem suporte para as GPUs físicas apresentadas com Atribuição de Dispositivo Discreto dos hosts de Hyper-V do Windows Server 2016 ou do Windows Server 2019. Confira [Planejar a implantação da Atribuição de Dispositivo Discreto](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md) para saber mais detalhes.


## <a name="vdi-deployment--supported-guest-oses"></a>Implantação de VDI – sistemas operacionais convidados com suporte

Servidores do Host de Virtualização de Área de Trabalho Remota do Windows Server 2016 e do Windows Server 2019 oferecem suporte aos seguintes sistemas operacionais convidados:

- Windows 10 Enterprise
- Windows 8.1 Enterprise
- Windows 7 SP1 Enterprise

> [!NOTE]
> - Os Serviços de Área de Trabalho Remota não oferecem suporte às coleções de sessão heterogêneas. Todas as VMs em uma coleção precisam ter a mesma versão de sistema operacional.
> - Você pode ter coleções homogêneas separadas com versões do SO convidado diferentes no mesmo host.
> - O host do Hyper-V usado para executar VMs precisa ter a mesma versão do host do Hyper-V usado para criar os modelos de VM originais.

## <a name="single-sign-on"></a>Logon único

O RDS do Windows Server 2016 e do Windows Server 2019 oferece suporte para duas experiências principais de SSO:

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

Você pode usar os Serviços de Área de Trabalho Remota com o [Proxy de Aplicativo do Azure AD](/azure/active-directory/application-proxy-publish-remote-desktop). Os Serviços de Área de Trabalho Remota não dão suporte ao uso do [Proxy de Aplicativo Web](../remote-access/web-application-proxy/web-application-proxy-windows-server.md), que está incluído no Windows Server 2016 e versões anteriores.
