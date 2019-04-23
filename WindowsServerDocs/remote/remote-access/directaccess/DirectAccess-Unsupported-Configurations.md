---
title: Configurações sem suporte do DirectAccess
description: Este tópico fornece uma lista de configurações do DirectAccess sem suporte no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-da
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 49fa86883e6a590ac8f7dcf0c724f87af419f88e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828847"
---
# <a name="directaccess-unsupported-configurations"></a>Configurações sem suporte do DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Examine a seguinte lista de configurações do DirectAccess sem suporte antes de iniciar sua implantação para evitar ter que iniciar a implantação novamente.  

## <a name="bkmk_frs"></a>Distribuição de replicação FRS (serviço) de arquivos de objetos de diretiva de grupo (replicações SYSVOL)  
Não implante o DirectAccess em ambientes em que os controladores de domínio estão executando a replicação FRS (serviço) para distribuição de objetos de diretiva de grupo (replicações SYSVOL). Implantação do DirectAccess não tem suporte quando você usa o FRS.  
  
Se você tiver controladores de domínio que executam o Windows Server 2003 ou Windows Server 2003 R2, você está usando FRS. Além disso, talvez você esteja usando FRS se você usou anteriormente o Windows 2000 Server ou controladores de domínio do Windows Server 2003 e você nunca migrados a replicação do SYSVOL do FRS para sistema de replicação de arquivos distribuído (DFS-R).  
  
Se você implantar o DirectAccess com a replicação do SYSVOL do FRS, o risco de exclusão não intencional de diretiva de grupo do DirectAccess objetos que contêm o servidor DirectAccess e as informações de configuração do cliente. Se esses objetos são excluídos, sua implantação do DirectAccess sofrerão uma interrupção, e computadores cliente que usam o DirectAccess não poderão se conectar à sua rede.  
  
Se você estiver planejando implantar o DirectAccess, você deve usar controladores de domínio que executam sistemas operacionais mais tarde do que o Windows Server 2003 R2, e você deve usar o DFS-R.  
  
Para obter informações sobre a migração de FRS para DFS-R, consulte o [guia de migração de replicação SYSVOL: Replicação FRS para DFS](https://technet.microsoft.com/library/dd640019(v=ws.10).aspx).  
  
## <a name="bkmk_nap"></a>Proteção de acesso à rede para clientes do DirectAccess  
Proteção de acesso de rede (NAP) é usado para determinar se computadores cliente remotos que atendem às políticas de TI antes de conceder acesso à rede corporativa. NAP foi preterido no Windows Server 2012 R2 e não está incluído no Windows Server 2016. Por esse motivo, não é recomendável iniciar uma nova implantação do DirectAccess com NAP. Recomenda-se um método diferente de controle de ponto de extremidade para a segurança dos clientes DirectAccess.  
  
## <a name="bkmk_multi"></a>Suporte multissite para clientes do Windows 7  
Quando o DirectAccess é configurado em uma implantação multissite, Windows 10&reg;, Windows&reg; 8.1 e Windows&reg; 8 clientes têm a capacidade de se conectar ao site mais próximo.  Windows 7&reg; computadores cliente não tem a mesma funcionalidade. Seleção de site para clientes do Windows 7 é definida para um site específico no momento da configuração de política, e esses clientes sempre se conectará a esse site designado, independentemente de sua localização.  
  
## <a name="bkmk_user"></a>Controle de acesso baseado em usuário  
As políticas do DirectAccess são computador com base, não do usuário com base. Não há suporte para a especificação de políticas de usuário do DirectAccess para controlar o acesso à rede corporativa.  
  
## <a name="bkmk_policy"></a>Personalizando a política do DirectAccess  
O DirectAccess pode ser configurado usando o Assistente de instalação do DirectAccess, o console de gerenciamento de acesso remoto ou os cmdlets de acesso remoto do Windows PowerShell. Usar qualquer meio que não seja o Assistente de instalação do DirectAccess para configurar o DirectAccess, como modificando diretamente os objetos de diretiva de grupo do DirectAccess ou modificar manualmente as configurações de política padrão no servidor ou cliente, não há suporte. Essas modificações podem resultar em uma configuração inutilizável.  
  
## <a name="bkmk_kerb"></a>Autenticação KerbProxy  
Quando você configura um servidor DirectAccess com o Assistente de Introdução, o servidor DirectAccess é automaticamente configurado para usar a autenticação de KerbProxy para o computador e autenticação de usuário. Por isso, você deve apenas usar o Assistente de Introdução para implantações de único site em que apenas Windows 10&reg;, Windows 8.1 ou clientes do Windows 8 são implantados.  
  
Além disso, os recursos a seguir não devem ser usados com a autenticação KerbProxy:  
  
-   Balanceando a carga por usando um balanceador externo de carga ou carregar o Windows   
    Balanceador  
  
-   Autenticação de dois fatores em que os cartões inteligentes ou uma senha única (OTP) são necessários  
  
Não há suporte para os seguintes planos de implantação se você habilitar a autenticação de KerbProxy:  
  
-   Multissite.  
  
-   O DirectAccess oferece suporte para clientes do Windows 7.  
  
-   Túnel à força. Para garantir que a autenticação KerbProxy não está habilitada quando você usa a opção Criar túneis à força, configure os seguintes itens ao executar o Assistente:  
  
    -   Habilitar a criação de túneis à força  
  
    -   Permitir que os clientes do DirectAccess para Windows 7  
  
> [!NOTE]  
> Para as implantações anteriores, você deve usar o Assistente de configuração avançada, que usa uma configuração de dois túneis com uma autenticação de computador e usuário baseada em certificado. Para obter mais informações, consulte [implantar um único servidor DirectAccess com configurações avançadas](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
## <a name="bkmk_isa"></a>Usando o ISATAP  
ISATAP é uma tecnologia de transição que fornece conectividade IPv6 em redes corporativas somente IPv4. Ele é limitado a organizações de pequeno e médio porte com uma única implantação de servidor do DirectAccess e permite o gerenciamento remoto de clientes do DirectAccess. Se o ISATAP é deployedin um multissite, balanceamento de carga ou ambiente de vários domínios, você deve removê-lo ou movê-lo para uma implantação do IPv6 nativo, antes de configurar o DirectAccess.  
  
## <a name="bkmk_iphttps"></a>IPHTTPS e configuração de ponto de extremidade de senha única (OTP)  
Quando você usa IPHTTPS, a conexão IPHTTPS deve terminar no servidor do DirectAccess, não em qualquer outro dispositivo, como um balanceador de carga. Da mesma forma, a conexão de Secure Sockets Layer (SSL) fora de banda que é criado durante a autenticação de senha única (OTP) deve terminar no servidor DirectAccess. Todos os dispositivos entre os pontos de extremidade dessas conexões devem ser configurados no modo de passagem.  
  
## <a name="bkmk_ft"></a>Túnel à força com autenticação OTP  
Não implante um servidor DirectAccess com autenticação de dois fatores com OTP e criar túneis à força ou a autenticação de OTP falhará. Uma conexão de Secure Sockets Layer (SSL) fora de banda é necessária entre o servidor DirectAccess e o cliente do DirectAccess. Esta conexão requer uma isenção para enviar o tráfego de fora do túnel do DirectAccess. Em uma configuração de túnel à força, todo o tráfego deve fluir por meio de um túnel do DirectAccess e não há isenção é permitida depois que o túnel for estabelecido. Por isso, não há suporte a autenticação de OTP em uma configuração de túnel forçado.  
  
## <a name="bkmk_rodc"></a>Implantação do DirectAccess com um controlador de domínio somente leitura  
Os servidores DirectAccess devem ter acesso a um controlador de domínio somente leitura e não funcionam corretamente com um controlador de domínio de somente leitura (RODC).  
  
É necessário um controlador de domínio leitura-gravação por vários motivos, incluindo o seguinte:  
  
-   No servidor do DirectAccess, um controlador de domínio leitura-gravação é necessária para abrir o Remote Access Microsoft Management Console (MMC).  
  
-   O servidor DirectAccess deve de leitura e gravação para o cliente DirectAccess e o servidor DirectAccess objetos de diretiva de grupo (GPOs).  
  
-   O servidor DirectAccess lê e grava o GPO do cliente especificamente no emulador do controlador de domínio primário (PDCe).  
  
Devido a esses requisitos, não implanta o DirectAccess com um RODC.  
  


