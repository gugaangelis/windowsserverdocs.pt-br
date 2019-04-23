---
title: Configurações com suporte para os serviços de área de trabalho remota
description: Fornece informações sobre configurações com suporte para RDS no Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 894ea8b134ae5b871a2978e3f72e683c12346fe5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850197"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>Configurações com suporte para serviços de área de trabalho remota no Windows Server 2016

> Aplica-se a: Windows Server 2016

Quando se trata de configurações com suporte para ambientes de serviços de área de trabalho remota, a maior preocupação tende a ser a interoperabilidade de versão. A maioria dos ambientes incluir várias versões do Windows Server – por exemplo, você pode ter uma implantação de RDS do Windows Server 2012 R2 existente mas deseja atualizar para o Windows Server 2016 para tirar proveito dos novos recursos (como suporte para OpenGL\OpenCL, Discrete Atribuição de dispositivo ou espaços de armazenamento diretos). A pergunta, em seguida, torna-se, quais componentes RDS podem trabalhar com diferentes versões e quais precisam ser o mesmo?

Portanto, com isso em mente, aqui estão as diretrizes básicas para configurações com suporte dos serviços de área de trabalho remota no Windows Server 2016.

> [!NOTE]
> Certifique-se de examinar a [requisitos de sistema do Windows Server 2016](../../get-started/system-requirements.md).

## <a name="best-practices"></a>Práticas recomendadas
- Use o Windows Server 2016 para sua infraestrutura de área de trabalho remota - o acesso via Web, Gateway, agente de Conexão e servidor de licença. Windows Server 2016 é compatível com versões anteriores para esses componentes - Portanto, um Host de sessão de área de trabalho do 2012 R2 pode se conectar a um agente de Conexão de área de trabalho de 2016, mas o inverso não é verdadeiro.

- Para Hosts de sessão de área de trabalho remota - todos os Hosts de sessão em uma coleção precisa ser o mesmo nível, mas você pode ter várias coleções. Você pode ter uma coleção com o host de sessão do Windows Server 2012 R2 e outra com Hosts de sessão do Windows Server 2016.

- Se você atualizar seu Host de sessão de área de trabalho remota para o Windows Server 2016, atualize também o servidor de licença. Lembre-se de que um servidor de licença 2016 pode processar CALs de todas as versões anteriores do Windows Server, para baixo até o Windows Server 2003.

- Siga a ordem de atualização recomendada na [atualizando o ambiente de serviços de área de trabalho remota](upgrade-to-rds.md#flow-for-deployment-upgrades). 

- Se você estiver criando um ambiente altamente disponível, todos os seus agentes de Conexão precisam ter o mesmo nível de sistema operacional.

## <a name="rd-connection-brokers"></a>Agentes de Conexão de área de trabalho remota

Windows Server 2016 remove a restrição para o número de agentes de Conexão, você pode ter em uma implantação ao usar Hosts de sessão de área de trabalho remota (RDSH) e remoto Hosts de virtualização de área de trabalho (RDVH) que executam o Windows Server 2016 também. A tabela a seguir mostra quais versões de trabalho de componentes RDS com as versões de 2016 e 2012 R2 do agente de Conexão em uma implantação altamente disponível com três ou mais agentes de Conexão.

| 3 + agentes de Conexão no HA              | RDSH 2016 | RDVH 2016 | RDSH 2012 R2  | RDVH 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Agente de Conexão do Windows Server 2016    | Com suporte | Com suporte | Com suporte     | Com suporte     |
| Agente de Conexão do Windows Server 2012 R2 | N/D       | N/D       | Com suporte     | Com suporte     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>Suporte para aceleração de GPU com Hyper-V
A tabela a seguir fornece detalhes sobre o suporte para aceleração de GPU em máquinas virtuais. Ver [qual tecnologia de virtualização de gráficos é ideal para você?](rds-graphics-virtualization.md) para descobrir o que você precisa de Ajuda. Para obter informações específicas sobre DDA, fazer check-out [planejar a implantação de atribuição de dispositivo discretos](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

|Sistema operacional convidado VM  |Windows Server 2012 R2 ou Windows Server 2016<br> VGPU do RemoteFX do Hyper-V (VM de geração 1) |  VGPU do RemoteFX do Windows Server 2016 Hyper-V (VM Gen 2) |  Atribuição de dispositivo discretos do Windows Server 2016 Hyper-V (VM Ger 2) |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | Sim                                                        | Não                                                     | Não                                                                  |
| Windows 8.1                 | Sim                                                        | Não                                                     | Não                                                                  |
| Windows 10 1511 Update      | Sim                                                        | Sim                                                    | Sim                                                                 |
| Windows Server 2012 R2      | Sim                                                        | Não                                                     | Sim (requer 3133690 KB)                                           |
| Windows Server 2016         | Sim                                                        | Sim                                                    | Sim                                                                 |
| Windows Server 2012 R2 RDSH | Não                                                         | Não                                                     | Sim (requer 3133690 KB)                                           |
| Windows Server 2016 RDSH    | Não                                                         | Não                                                     | Sim                                                                 |
## <a name="vdi-deployment--supported-guest-oss"></a>Implantação de VDI – sistemas operacionais 
Servidores Host de virtualização de área de trabalho remota do Windows Server 2016 dão suporte o sistemas de operacionais de convidados seguintes:

- Windows 10 Enterprise
- Windows 8.1 Enterprise 
- Windows 8 Enterprise 
- Windows 7 SP1 Enterprise 

A tabela a seguir mostra as combinações de sistema operacional convidado e sistemas operacionais de Hosts de virtualização de área de trabalho remota:

| Versão do sistema operacional RDVH        | Versão do SO convidado           |
| ------------- |-------------|
| Windows Server 2016      | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012 R2   | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012      | Windows 7 SP1, Windows 8, Windows 8.1 |

> [!NOTE]  
> - Serviços de área de trabalho remota do Windows Server 2016 não oferece suporte a coleções heterogêneas. Todas as VMs em uma coleção devem ser a mesma versão do sistema operacional. 
> - Você pode ter coleções homogêneas separadas com versões do SO convidado diferentes no mesmo host. 
> - Modelos de VM devem ser criados em um host do Windows Server 2016 Hyper-V para ser usado como o sistema operacional convidado em um host do Windows Server 2016 Hyper-V.

## <a name="single-sign-on-sso"></a>Logon único (SSO)
RDS do Windows Server 2016 dá suporte a duas principais experiências SSO:

 - No aplicativo (aplicativo de área de trabalho remota no Windows, iOS, Android e Mac)
 - SSO da Web
 
Usando o aplicativo de área de trabalho remota, você pode armazenar credenciais como parte das informações de conexão ([Mac](clients\remote-desktop-mac.md)) ou como parte das contas gerenciadas ([iOS](clients\remote-desktop-ios.md#manage-your-user-accounts), [Android](clients\remote-desktop-android.md#manage-your-user-accounts), Windows) com segurança por meio de mecanismos exclusivos para cada sistema operacional.

Para conectar-se a áreas de trabalho e RemoteApps com o SSO por meio da caixa de entrada do cliente Conexão de área de trabalho remota no Windows, você deve se conectar à página da Web de área de trabalho remota por meio do Internet Explorer. As opções de configuração a seguir são necessárias no lado do servidor. Outras configurações não têm suporte para SSO da Web:

 - Conjunto de Web da área de trabalho remota para autenticação baseada em formulários (padrão)
 - Gateway de área de trabalho remota para autenticação de senha (padrão)
 - Implantação do RDS é definido como "Credenciais de Gateway de área de trabalho de uso para computadores remotos" (padrão) nas propriedades de Gateway de área de trabalho remota

> [!NOTE]
> Devido às opções de configuração necessárias, não há suporte para SSO da Web com cartões inteligentes. Usuários que o logon por meio de cartões inteligentes poderá enfrentar vários prompts para fazer logon.

Para obter mais informações sobre a criação de implantação de VDI dos serviços de área de trabalho remota, fazer check-out [as configurações de segurança de suporte para Windows 10 para VDI de serviços de área de trabalho remota](rds-vdi-supported-config.md).

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>Usando os serviços de área de trabalho remota com serviços de proxy de aplicativo

Você pode usar os serviços de área de trabalho remota, exceto para o cliente da web, com [Proxy de aplicativo do Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop). Serviços de área de trabalho remota não oferece suporte ao uso [Proxy de aplicativo Web](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server), que está incluído no Windows Server 2016 e versões anteriores.
