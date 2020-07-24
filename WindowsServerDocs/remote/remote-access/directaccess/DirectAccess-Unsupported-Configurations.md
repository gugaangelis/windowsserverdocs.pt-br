---
title: Configurações sem suporte do DirectAccess
description: Este tópico fornece uma lista de configurações do DirectAccess sem suporte no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e59fd85fae3333ec3a5751ba611b615f4af92cb0
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960238"
---
# <a name="directaccess-unsupported-configurations"></a>Configurações sem suporte do DirectAccess

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Examine a seguinte lista de configurações do DirectAccess sem suporte antes de iniciar a implantação para evitar ter que iniciar a implantação novamente.  

## <a name="file-replication-service-frs-distribution-of-group-policy-objects-sysvol-replications"></a><a name="bkmk_frs"></a>Distribuição de FRS (serviço de replicação de arquivo) de objetos de Política de Grupo (replicações de SYSVOL)  
Não implante o DirectAccess em ambientes em que os controladores de domínio estejam executando o FRS (serviço de replicação de arquivo) para distribuição de objetos de Política de Grupo (replicações de SYSVOL). Não há suporte para a implantação do DirectAccess quando você usa o FRS.  
  
Você está usando o FRS se tiver controladores de domínio que executam o Windows Server 2003 ou o Windows Server 2003 R2. Além disso, você pode estar usando o FRS se usou anteriormente os controladores de domínio do Windows 2000 Server ou do Windows Server 2003 e nunca migrou a replicação de SYSVOL do FRS para o Sistema de Arquivos Distribuído Replication (DFS-R).  
  
Se você implantar o DirectAccess com a replicação SYSVOL do FRS, você arriscará a exclusão não intencional de objetos do DirectAccess Política de Grupo que contêm as informações de configuração do cliente e do servidor DirectAccess. Se esses objetos forem excluídos, a implantação do DirectAccess sofrerá uma interrupção e os computadores cliente que usam o DirectAccess não poderão se conectar à sua rede.  
  
Se você estiver planejando implantar o DirectAccess, deverá usar controladores de domínio que executam sistemas operacionais posteriores ao Windows Server 2003 R2, e você deve usar o DFS-R.  
  
Para obter informações sobre como migrar do FRS para o DFS-R, consulte o [Guia de migração de replicação do SYSVOL: FRS para replicação do DFS](../../../storage/dfs-replication/migrate-sysvol-to-dfsr.md).  
  
## <a name="network-access-protection-for-directaccess-clients"></a><a name="bkmk_nap"></a>Proteção de acesso à rede para clientes do DirectAccess  
A NAP (proteção de acesso à rede) é usada para determinar se os computadores cliente remotos atendem às políticas de ti antes de receberem acesso à rede corporativa. A NAP foi preterida no Windows Server 2012 R2 e não está incluída no Windows Server 2016. Por esse motivo, não é recomendável iniciar uma nova implantação do DirectAccess com a NAP. É recomendado um método diferente de controle de ponto de extremidade para a segurança de clientes do DirectAccess.  
  
## <a name="multisite-support-for-windows-7-clients"></a><a name="bkmk_multi"></a>Suporte multissite para clientes do Windows 7  
Quando o DirectAccess é configurado em uma implantação multissite, &reg; os clientes Windows 10, windows &reg; 8,1 e Windows &reg; 8 têm a capacidade de se conectar ao site mais próximo.  &reg;Os computadores cliente do Windows 7 não têm o mesmo recurso. A seleção de site para clientes do Windows 7 é definida para um site específico no momento da configuração da política, e esses clientes sempre se conectarão a esse site designado, independentemente de sua localização.  
  
## <a name="user-based-access-control"></a><a name="bkmk_user"></a>Controle de acesso baseado no usuário  
As políticas do DirectAccess são baseadas em computador, não baseadas em usuário. Não há suporte para a especificação de políticas de usuário do DirectAccess para controlar o acesso à rede corporativa.  
  
## <a name="customizing-directaccess-policy"></a><a name="bkmk_policy"></a>Personalizando a política do DirectAccess  
O DirectAccess pode ser configurado usando o assistente de instalação do DirectAccess, o console de gerenciamento de acesso remoto ou os cmdlets do Windows PowerShell de acesso remoto. O uso de qualquer meio que não seja o assistente de instalação do DirectAccess para configurar o DirectAccess, como a modificação direta de objetos do DirectAccess Política de Grupo ou a modificação manual das configurações de política padrão no servidor ou cliente, não tem suporte. Essas modificações podem resultar em uma configuração inutilizável.  
  
## <a name="kerbproxy-authentication"></a><a name="bkmk_kerb"></a>Autenticação KerbProxy  
Quando você configura um servidor DirectAccess com o assistente de Introdução, o servidor DirectAccess é configurado automaticamente para usar a autenticação KerbProxy para autenticação de computador e de usuário. Por isso, você deve usar apenas o assistente de Introdução para implantações de site único em que somente &reg; clientes Windows 10, Windows 8.1 ou Windows 8 são implantados.  
  
Além disso, os recursos a seguir não devem ser usados com a autenticação KerbProxy:  
  
-   Balanceamento de carga usando um balanceador externo de carga ou carga do Windows   
    Balanceador  
  
-   A autenticação de dois fatores em que os cartões inteligentes ou uma OTP (senha de uso único) são necessários  
  
Não há suporte para os seguintes planos de implantação se você habilitar a autenticação KerbProxy:  
  
-   Multissite.  
  
-   Suporte do DirectAccess para clientes do Windows 7.  
  
-   Túnel forçado. Para garantir que a autenticação KerbProxy não seja habilitada quando você usar o túnel forçado, configure os seguintes itens ao executar o assistente:  
  
    -   Habilitar a criação de túneis à força  
  
    -   Habilitar o DirectAccess para clientes do Windows 7  
  
> [!NOTE]  
> Para as implantações anteriores, você deve usar o assistente de configuração avançada, que usa uma configuração de dois túneis com um computador baseado em certificado e autenticação de usuário. Para obter mais informações, consulte [implantar um único servidor DirectAccess com configurações avançadas](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
## <a name="using-isatap"></a><a name="bkmk_isa"></a>Usando o ISATAP  
O ISATAP é uma tecnologia de transição que fornece conectividade IPv6 em redes corporativas somente IPv4. Ele é limitado a organizações de pequeno e médio porte com uma única implantação do servidor DirectAccess e permite o gerenciamento remoto de clientes DirectAccess. Se o ISATAP for implantado em um ambiente multissite, de balanceamento de carga ou de multidomínio, você deverá removê-lo ou movê-lo para uma implantação IPv6 nativa antes de configurar o DirectAccess.  
  
## <a name="iphttps-and-one-time-password-otp-endpoint-configuration"></a><a name="bkmk_iphttps"></a>Configuração de ponto de extremidade de OTP (senha de uso único) e IPHTTPS  
Quando você usa o IPHTTPS, a conexão IPHTTPS deve terminar no servidor DirectAccess, não em qualquer outro dispositivo, como um balanceador de carga. Da mesma forma, a conexão de protocolo SSL fora de banda (SSL) criada durante a autenticação de OTP (senha de uso único) deve terminar no servidor DirectAccess. Todos os dispositivos entre os pontos de extremidade dessas conexões devem ser configurados no modo de passagem.  
  
## <a name="force-tunnel-with-otp-authentication"></a><a name="bkmk_ft"></a>Forçar túnel com autenticação OTP  
Não implante um servidor DirectAccess com a autenticação de dois fatores com a OTP e o túnel forçado, ou a autenticação OTP falhará. Uma conexão de protocolo SSL (SSL) fora de banda é necessária entre o servidor DirectAccess e o cliente DirectAccess. Essa conexão requer uma isenção para enviar o tráfego fora do túnel do DirectAccess. Em uma configuração de túnel forçado, todo o tráfego deve fluir por um túnel do DirectAccess e nenhuma isenção é permitida depois que o túnel é estabelecido. Por isso, não há suporte para a autenticação de OTP em uma configuração de túnel forçado.  
  
## <a name="deploying-directaccess-with-a-read-only-domain-controller"></a><a name="bkmk_rodc"></a>Implantando o DirectAccess com um controlador de domínio somente leitura  
Os servidores DirectAccess devem ter acesso a um controlador de domínio de leitura/gravação e não funcionar corretamente com um RODC (controlador de domínio somente leitura).  
  
Um controlador de domínio de leitura/gravação é necessário por vários motivos, incluindo o seguinte:  
  
-   No servidor DirectAccess, um controlador de domínio de leitura/gravação é necessário para abrir o MMC (console de gerenciamento Microsoft) de acesso remoto.  
  
-   O servidor DirectAccess deve ler e gravar no cliente DirectAccess e no servidor DirectAccess Política de Grupo Objects (GPOs).  
  
-   O servidor DirectAccess lê e grava no GPO do cliente especificamente do emulador do controlador de domínio primário (PDCe).  
  
Devido a esses requisitos, não implante o DirectAccess com um RODC.  
  
