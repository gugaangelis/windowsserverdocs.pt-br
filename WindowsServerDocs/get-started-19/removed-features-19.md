---
title: Recursos removidos ou planejados para remoção no Windows Server 2019
description: Saiba mais sobre os recursos e funcionalidades removidos ou planejados para remoção a partir do Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: jasgro
ms.date: 03/29/2019
ms.localizationpriority: medium
ms.openlocfilehash: 470857616a9b36d238de031b4ccf80a68eff1e61
ms.sourcegitcommit: 971f6538e8d89af84ef50fc8aab2188bdf6f47cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/02/2019
ms.locfileid: "9279127"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Recursos removidos ou planejado para substituição a partir do Windows Server 2019

>Aplica-se a: Windows Server 2019

Cada versão do Windows Server adiciona novos recursos e funcionalidades; removemos também ocasionalmente, recursos e funcionalidades, geralmente porque adicionamos a melhor opção. Aqui estão os detalhes sobre os recursos e funcionalidades que foram removidos no Windows Server 2019.   

> [!TIP]
> - Você pode obter acesso antecipado ao Windows Server compilações ao ingressar no [programa Windows Insider](https://insider.windows.com) - isso é uma ótima maneira de testar as alterações de recurso.
> - Tem dúvidas sobre outras versões? Confira as informações para o [Windows Server, versão 1803](../get-started/windows-server-1803-removed-features.md), [Windows Server, versão 1709](../get-started/removed-features-1709.md)e [Windows Server 2016](../get-started/deprecated-features.md).

**A lista está sujeita a alteração e pode não incluir todos os recursos ou funcionalidades afetados.** 

## <a name="features-we-removed-in-this-release"></a>Recursos que removemos nesta versão

Nós está removendo os seguintes recursos e funcionalidades da imagem do produto instalado no Windows Server 2019. Aplicativos ou código que dependam esses recursos não funcionarão nesta versão, a menos que você use um método alternativo.   

|Recurso    |Em vez disso, você pode usar …|
|-----------|--------------------
|Verificação de negócios, também chamada de Gerenciamento de Digitalização Distribuída (DSM)|Nós está removendo essa verificação segura e a funcionalidade de gerenciamento de scanner - não existem dispositivos que dão suporte a esse recurso.|
|Imprimir componentes - componente opcional agora para instalações do Server Core|Em versões anteriores do Windows Server, os componentes de impressão foram *desabilitadas* por padrão na opção de instalação Server Core. Alteramos que no Windows Server 2016, permitindo que eles por padrão. No Windows Server 2019, esses componentes de impressão mais uma vez são desabilitados por padrão para o Server Core. Se você precisar habilitar os componentes de impressão, você pode fazer isso executando o cmdlet de **Instalação-WindowsFeature servidor de impressão** .|
|[Agente de conexão de área de trabalho remota e Host de virtualização de área de trabalho remota](../remote/remote-desktop-services/desktop-hosting-service.md) em uma instalação Server Core|A maioria das implantações de Serviços de área de trabalho remota têm essas funções colocalizadas com o Host da sessão de área de trabalho remota (RDSH), que requer um servidor com Experiência desktop; para ser consistente com RDSH, estamos alterando essas funções para também exigirem o servidor com Experiência desktop. Essas funções RDS não estão mais disponíveis para uso em uma [instalação Server Core](../administration/server-core/what-is-server-core.md). Se você precisar [implantar essas funções como parte de sua infraestrutura de área de trabalho remota](../remote/remote-desktop-services/rds-deploy-infrastructure.md), você pode [instalá-las no Windows Server com experiência Desktop](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Essas funções também são incluídas na opção de instalação da Experiência desktop do Windows Server 2019. |



## <a name="features-were-no-longer-developing"></a>Recursos que não estamos mais desenvolvendo

Nós estiver não desenvolvemos mais ativamente esses recursos e podemos removê-los de uma atualização futura. Alguns recursos foram substituídos por outros recursos ou funcionalidades, enquanto outros agora estão disponíveis de fontes diferentes. 

Se tiver comentários sobre a substituição proposta de qualquer um desses recursos, você pode usar o [aplicativo Hub de Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

|Recurso    |Em vez disso, você pode usar …|
|-----------|---------------------|
|Unidade de armazenamento de chave no Hyper-V|Não estamos trabalhando o recurso de unidade de armazenamento de chave no Hyper-V. Se você estiver usando VMs de geração 1, confira [Segurança de virtualização de VM da geração 1](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) para obter informações sobre opções no futuro. Se você estiver criando novas VMs usam máquinas virtuais de geração 2 com dispositivos do TPM para uma solução mais segura. |
|Console de gerenciamento do Trusted Platform Module (TPM)|As informações anteriormente disponíveis no console de gerenciamento do TPM agora estão disponíveis na página de [**segurança do dispositivo**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) na [Central de segurança do Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center).|
|Modo de Atestado Active Directory do serviço de guardião de host|Não estamos desenvolvendo modo de Atestado do Active Directory do Host guardião serviço - em vez disso, adicionamos um novo modo de Atestado, [atestado de chave do host](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), que é muito mais simples e igualmente compatível como Atestado com base no Active Directory.  Esse novo modo fornece funcionalidade equivalente com uma experiência de instalação, gerenciamento mais simples e menos dependências de infraestrutura de Atestado o Active Directory. Atestado de chave de host tem sem requisitos de hardware adicionais além do qual Atestado do Active Directory necessária, portanto, todos os sistemas existentes permanecerá compatíveis com o novo modo. Consulte [implantar hosts de malha](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) para obter mais informações sobre suas opções de Atestado.|
|Serviço de OneSync|O serviço OneSync sincroniza dados para os aplicativos de email, calendário e pessoas. Adicionamos um mecanismo de sincronização para o aplicativo Outlook que fornece a mesma sincronização.|
|Suporte a API de compactação diferencial remoto|Suporte a API de compactação diferencial remoto habilitado a sincronização de dados com uma fonte remota usando tecnologias de compactação, que minimizar a quantidade de dados enviados pela rede. Esse suporte atualmente não é usada por qualquer produto da Microsoft.|
|Extensão de comutador WFP filtro simples|A extensão do comutador de filtro leve WFP permite aos desenvolvedores criar [extensões de filtragem de pacote simples de rede para o Hyper-V virtual switch](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering). Você pode obter a mesma funcionalidade, criando uma extensão de filtragem completa. Dessa forma, podemos estará removendo essa extensão no futuro.|

