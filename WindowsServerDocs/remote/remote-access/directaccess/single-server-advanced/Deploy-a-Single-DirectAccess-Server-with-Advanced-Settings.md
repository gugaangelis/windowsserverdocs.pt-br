---
title: Implantar um servidor DirectAccess único com configurações avançadas
description: Este tópico faz parte do guia implantar um único servidor DirectAccess com as configurações avançadas do Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b211a9ca-1208-4e1f-a0fe-26a610936c30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8ccb91973dfb3493b534bdbc8fc4e2bcb26b26b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404953"
---
# <a name="deploy-a-single-directaccess-server-with-advanced-settings"></a>Implantar um servidor DirectAccess único com configurações avançadas

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico fornece uma introdução ao cenário do DirectAccess que usa um único servidor DirectAccess e permite que você implante o DirectAccess com configurações avançadas.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Antes de iniciar a implantação, consulte a lista de configurações sem suporte, de problemas conhecidos e de pré-requisitos  
Você pode usar os tópicos a seguir para examinar os pré-requisitos e outras informações antes de implantar o DirectAccess.  
  
-   [Configurações do DirectAccess sem suporte](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [Pré-requisitos para implantação do DirectAccess](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="BKMK_OVER"></a>Descrição do cenário  
Nesse cenário, um único computador executando o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012, é configurado como um servidor DirectAccess com configurações avançadas.  
  
> [!NOTE]  
> Se você deseja configurar uma implantação básica somente com configurações simples, consulte [Implantar um único servidor DirectAccess usando o Assistente de Introdução](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md). No cenário simples, o DirectAccess é configurado com definições padrão usando um assistente, sem exigir a configuração de definições de infraestrutura, como uma AC (autoridade de certificação) ou grupos de segurança do Active Directory.  
  
## <a name="in-this-scenario"></a>Neste cenário  
Para configurar um servidor único de DirectAccess com configurações avançadas, será necessário concluir diversas etapas de planejamento e implantação.  
  
### <a name="prerequisites"></a>Pré-requisitos  
Antes de começar, analise os requisitos a seguir.  
  
-   O Firewall do Windows deve estar habilitado em todos os perfis.  
  
-   O servidor DirectAccess é o servidor de local de rede.  
  
-   Você deseja que todos os computadores sem fio no domínio onde o servidor DirectAccess foi instalado estejam habilitados para o DirectAccess. Quando você implanta o DirectAccess, ele fica habilitado automaticamente em todos os computadores móveis no domínio atual.  
  
> [!IMPORTANT]  
> Algumas tecnologias e configurações não são compatíveis ao implantar o DirectAccess.  
>   
> -   O protocolo ISATAP não é compatível na rede corporativa. Se você estiver usando o ISATAP, deverá removê-lo e usar o IPv6 nativo.  
  
### <a name="planning-steps"></a>Etapas de planejamento  
O planejamento está dividido em duas fases:  
  
1.  **Planejando para a infraestrutura do DirectAccess**. Esta fase descreve o planejamento necessário para configurar a infraestrutura de rede antes de começar a implantação do DirectAccess. Ela inclui planejar a topologia de rede e servidor, o planejamento de certificados, o DNS, a configuração do Active Directory e do GPO (Objeto de Política de Grupo) e o servidor de local de rede do DirectAccess.  
  
2.  **Planejando a implantação do DirectAccess**. Esta fase descreve as etapas de planejamento necessárias para preparar a implantação do DirectAccess. Ela inclui o planejamento para computadores cliente de DirectAccess, requisitos de autenticação de servidor e cliente, configurações de VPN, servidores de infraestrutura e servidores de gerenciamento e de aplicativos.  
  
### <a name="deployment-steps"></a>Etapas de implantação  
A implantação está dividida em três fases:  
  
1.  **Configurando a infraestrutura do DirectAccess**. Esta fase inclui a configuração da rede e do roteamento, das definições de firewall se necessário, de certificados, de servidores DNS, das definições do Active Directory e do GPO e do servidor de local de rede do DirectAccess.  
  
2.  **Definindo as configurações do servidor do DirectAccess**. Esta fase inclui etapas para configurar os computadores cliente de DirectAccess, o servidor do DirectAccess, os servidores de infraestrutura e os servidores de gerenciamento e de aplicativos.  
  
3.  **Verificando a implantação**. Esta fase inclui etapas para verificar a implantação do DirectAccess.  
  
Para etapas detalhadas de implantação, consulte [Install and Configure Advanced DirectAccess](../../../remote-access/directaccess/single-server-advanced/Install-and-Configure-Advanced-DirectAccess.md).  
  
## <a name="BKMK_APP"></a>Aplicativos práticos  
A implantação de um só servidor de DirectAccess oferece:  
  
-   **Facilidade de acesso**. Os computadores cliente gerenciados que executam o Windows 10, Windows 8.1, Windows 8 e Windows 7 podem ser configurados como computadores cliente do DirectAccess. Esses clientes podem acessar os recursos da rede interna por meio do DirectAccess sempre que estiverem localizados na Internet sem precisar fazer logon em uma conexão VPN. Computadores cliente que não executam um desses sistemas operacionais podem se conectar à rede interna por meio de VPN.  
  
-   **Facilidade de gerenciamento**. Os computadores cliente do DirectAccess localizados na Internet podem ser gerenciados remotamente por administradores de Acesso Remoto pelo DirectAccess, mesmo quando não estão localizados na rede corporativa interna. Os computadores cliente que não atendem aos requisitos corporativos podem ser corrigidos automaticamente por servidores de gerenciamento. O DirectAccess e a VPN são gerenciados no mesmo console e com o mesmo conjunto de assistentes. Além disso, um ou mais servidores de DirectAccess podem ser gerenciados em um único console de Gerenciamento de Acesso Remoto.  
  
## <a name="BKMK_NEW"></a>Funções e recursos necessários para este cenário  
A tabela a seguir lista funções e recursos necessários para este cenário:  
  
|Função/recurso|Como este cenário tem suporte|  
|---------|-----------------|  
|Função Acesso Remoto|A função é instalada e desinstalada usando o console de Gerenciador do Servidor ou o Windows PowerShell. Esta função abrange o DirectAccess e o RRAS (Serviços de Roteamento e Acesso Remoto). A função Acesso Remoto consiste em dois componentes:<br/><br/>1.  DirectAccess e VPN RRAS. O DirectAccess e a VPN são gerenciados juntos no console de gerenciamento de acesso remoto.<br/>2.  Roteamento RRAS. Os recursos de roteamento do RRAS são gerenciados no console de roteamento e acesso remoto herdado.<br /><br />A função de servidor Acesso Remoto depende dos seguintes recursos/funções de servidor:<br/><br/> -Serviços de Informações da Internet (IIS) servidor Web-esse recurso é necessário para configurar o servidor de local de rede no servidor DirectAccess e a investigação da Web padrão.<br/> -Banco de dados interno do Windows. Usado para contabilização local no servidor DirectAccess.|  
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<br /><br />-Ele é instalado por padrão em um servidor DirectAccess quando a função de acesso remoto é instalada e dá suporte à interface do usuário do console de gerenciamento remoto e aos cmdlets do Windows PowerShell.<br />-Ele pode ser instalado opcionalmente em um servidor que não está executando a função de servidor DirectAccess. Neste caso, ele é usado para gerenciamento remoto de um computador de Acesso Remoto que executa o DirectAccess e VPN.<br /><br />O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<br /><br />-Interface gráfica do usuário do acesso remoto (GUI)<br />-Módulo de acesso remoto para Windows PowerShell<br /><br />As dependências incluem:<br /><br />-Console de Gerenciamento de Política de Grupo<br />-Kit de administração do Gerenciador de conexões RAS (CMAK)<br />-Windows PowerShell 3,0<br />-Infraestrutura e ferramentas de gerenciamento gráfico|  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
Os requisitos de hardware para este cenário incluem o seguinte:  
  
-   Requisitos de servidor:  
  
    -   Um computador que atenda aos requisitos de hardware do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
    -   O servidor deve ter pelo menos um adaptador de rede instalado, habilitado e conectado à rede interna. Quando são usados dois adaptadores, um deles deve estar conectado à rede corporativa interna e o outro, à rede externa (Internet ou rede privada).  
  
    -   Se for necessário Teredo como um protocolo de transição de IPv4 para IPv6, o adaptador externo do servidor exigirá dois endereços IPv4 públicos consecutivos. Se um único endereço IP estiver disponível, apenas IP-HTTPS poderá ser usado como o protocolo de transição.  
  
    -   Pelo menos um controlador de domínio. O servidor do DirectAccess e os clientes do DirectAccess devem ser membros do domínio.  
  
    -   Uma AC (autoridade de certificação) é necessária se não desejar utilizar certificados autoassinados para IP-HTTPS ou servidor de local de rede, ou se desejar utilizar certificados de cliente para autenticação IPsec do cliente. Você também pode solicitar certificados de uma AC pública.  
  
    -   Se o servidor de local de rede não estiver localizado no servidor de DirectAccess, será necessário um servidor Web isolado para executá-lo.  
  
-   Requisitos do cliente:  
  
    -   Um computador cliente deve estar executando o Windows 10, o Windows 8 ou o Windows 7.  
  
        > [!NOTE]  
        > Os seguintes sistemas operacionais podem ser usados como clientes DirectAccess: Windows 10, Windows Server 2012 R2, Windows Server 2012, Windows 8 Enterprise, Windows 7 Enterprise ou Windows 7 Ultimate.  
  
-   Requisitos do servidor de infraestrutura e gerenciamento:  
  
    -   Durante o gerenciamento remoto de computadores cliente do DirectAccess, os clientes iniciam as comunicações com os servidores de gerenciamento como controladores de domínio, Servidores de Configuração do System Center e servidores de Autoridade de Registro de Integridade (HRA) para serviços que incluem atualizações do Windows e de antivírus e conformidade de cliente de Proteção de Acesso à Rede (NAP). Os servidores necessários devem ser implantados antes de começar a implantação do Acesso Remoto.  
  
    -   Se o Acesso Remoto exigir conformidade de NAP do cliente, os servidores NPS e HRS deverão ser implantados antes do início da implantação de acesso remoto  
  
    -   Se o VPN estiver habilitado, um servidor DHCP será necessário para alocar automaticamente os endereços IP a clientes VPN, se um pool de endereços estáticos não for usado.  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Há diversos requisitos para este cenário:  
  
-   Requisitos de servidor:  
  
    -   O servidor do DirectAccess deve ser um membro do domínio. O servidor pode ser implantado na borda da rede interna, ou atrás de um firewall de borda ou outro dispositivo.  
  
    -   Se o servidor do DirectAccess estiver localizado atrás de um firewall de borda ou dispositivo NAT, o dispositivo deverá ser configurado para permitir o tráfego de/para o servidor de DirectAccess.  
  
    -   A pessoa que implanta o acesso remoto no servidor precisa de permissões de administrador local no servidor e permissões de usuário de domínio. Além disso, o administrador precisa de permissões para os GPOs utilizados na implantação do DirectAccess. Para aproveitar os recursos que restringem a implantação do DirectAccess somente a computadores móveis, são necessárias permissões para criar um filtro WMI no controlador de domínio.  
  
-   Requisitos de cliente de Acesso Remoto:  
  
    -   Os clientes do DirectAccess devem ser membros do domínio. Os domínios que contiverem clientes podem pertencer à mesma floresta do servidor do DirectAccess, ou ter uma relação de confiança bidirecional com o domínio ou a floresta de servidor DirectAccess.  
  
    -   Um grupo de segurança do Active Directory é necessário para conter os computadores que serão configurados como clientes do DirectAccess. Se um grupo de segurança não for especificado ao configurar as definições de cliente do DirectAccess, por padrão o GPO do cliente será aplicado em todos os computadores laptop no grupo de segurança de Computadores de Domínio.  
  
        > [!NOTE]  
        > É recomendado criar um grupo de segurança para cada domínio que contém computadores cliente do DirectAccess.  
  
        > [!IMPORTANT]  
        > Se você tiver habilitado o Teredo em sua implantação do DirectAccess e desejar fornecer acesso aos clientes do Windows 7, verifique se os clientes são atualizados para o Windows 7 com SP1. Os clientes que usam o Windows 7 RTM não poderão se conectar via Teredo. Contudo, tais clientes ainda poderão conectar-se à rede corporativa com IP-HTTPS.  
  
## <a name="BKMK_LINKS"></a>Consulte também  
A tabela a seguir fornece links para recursos adicionais.  
  
|Tipo de conteúdo|Referências|  
|--------|-------|  
|**Implantação**|[Caminhos de implantação do DirectAccess no Windows Server](../../../remote-access/directaccess/DirectAccess-Deployment-Paths-in-Windows-Server.md)<br /><br />[Implantar um único servidor DirectAccess usando o assistente de Introdução](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|  
|**Ferramentas e configurações**|[Cmdlets do PowerShell de acesso remoto](https://technet.microsoft.com/library/hh918399.aspx)|  
|**Recursos da comunidade**|[Guia de sobrevivência do DirectAccess](https://social.technet.microsoft.com/wiki/contents/articles/23210.directaccess-survival-guide.aspx)<br /><br />[Entradas do wiki do DirectAccess](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**Tecnologias relacionadas**|[Como funciona o IPv6](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  


