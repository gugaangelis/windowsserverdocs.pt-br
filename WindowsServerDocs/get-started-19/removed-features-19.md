---
title: Recursos removidos ou planejados para remoção no Windows Server 2019
description: Saiba mais sobre os recursos e funcionalidades removidos ou planejados para remoção, começando com o Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: jasongerend
ms.author: jgerend
manager: jasgro
ms.date: 05/21/2019
ms.localizationpriority: medium
ms.openlocfilehash: 820dfed8a0a58d3ccc64023325c373b761461ba8
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65976521"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Recursos removidos ou planejados para substituição iniciando o Windows Server 2019

>Aplica-se a: Windows Server 2019

Cada versão do Windows Server adiciona novos recursos e funcionalidades; removemos também ocasionalmente, recursos e funcionalidades, geralmente porque adicionamos a melhor opção. Aqui estão os detalhes sobre os recursos e funcionalidades que removemos no Windows Server 2019.

> [!TIP]
> - Você pode obter acesso antecipado ao Windows Server compilações ao ingressar no [programa Windows Insider](https://insider.windows.com) - isso é uma ótima maneira de testar as alterações de recurso.
> - Tem dúvidas sobre outras versões? Confira as informações para [o que há de novo no Windows Server](../get-started/whats-new-in-windows-server.md).

**A lista está sujeita a alterações e pode não incluir todos os recursos afetados ou funcionalidade.** 

## <a name="features-we-removed-in-this-release"></a>Recursos que removemos nesta versão

Podemos está removendo os seguintes recursos e funcionalidades da imagem do produto instalado no Windows Server 2019. Aplicativos ou código que dependam esses recursos não funcionarão nesta versão, a menos que você use um método alternativo.

|Recurso    |Em vez disso, você pode usar …|
|-----------|--------------------
|Verificação de negócios, também chamada de Gerenciamento de Digitalização Distribuída (DSM)|Podemos está removendo essa verificação segura e a capacidade de gerenciamento do scanner - não existem dispositivos que dão suporte a esse recurso.|
|Imprimir componentes - componente agora opcional para instalações do Server Core|Em versões anteriores do Windows Server, os componentes de impressão foram *desabilitada* por padrão na opção de instalação Server Core. Alteramos que no Windows Server 2016, permitindo que eles por padrão. No Windows Server de 2019, esses componentes de impressão novamente são desabilitados por padrão para o Server Core. Se você precisar habilitar os componentes de impressão, você pode fazer isso executando o **servidor de impressão de Install-WindowsFeature** cmdlet.|
|[Agente de conexão de área de trabalho remota e Host de virtualização de área de trabalho remota](../remote/remote-desktop-services/desktop-hosting-service.md) em uma instalação Server Core|A maioria das implantações de Serviços de área de trabalho remota têm essas funções colocalizadas com o Host da sessão de área de trabalho remota (RDSH), que requer um servidor com Experiência desktop; para ser consistente com RDSH, estamos alterando essas funções para também exigirem o servidor com Experiência desktop. Essas funções RDS não estão mais disponíveis para uso em um [instalação Server Core](../administration/server-core/what-is-server-core.md). Se você precisar [implantar essas funções como parte de sua infraestrutura de área de trabalho remota](../remote/remote-desktop-services/rds-deploy-infrastructure.md), você pode [instalá-los no Windows Server com experiência Desktop](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Essas funções também são incluídas na opção de instalação da Experiência desktop do Windows Server 2019. |

## <a name="features-were-no-longer-developing"></a>Recursos que não estamos mais desenvolvendo

Nós não é mais ativamente está desenvolvendo esses recursos e removê-las de uma atualização futura. Alguns recursos foram substituídos por outros recursos ou funcionalidades, enquanto outros agora estão disponíveis de fontes diferentes. 

Se tiver comentários sobre a substituição proposta de qualquer um desses recursos, você pode usar o [aplicativo Hub de Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

| Recurso   | Em vez disso, você pode usar … |
|-----------|---------------------|
| Unidade de armazenamento de chave no Hyper-V|Não estamos trabalhando sobre o recurso de unidade de armazenamento de chave no Hyper-V. Se você estiver usando VMs da geração 1, fazer check-out [geração 1 VM virtualização segurança](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) para obter informações sobre as opções no futuro. Se você estiver criando novas VMs usam máquinas virtuais de geração 2 com dispositivos TPM para uma solução mais segura. |
| Console de gerenciamento confiável Platform Module (TPM)|As informações disponíveis anteriormente no console de gerenciamento do TPM agora estão disponíveis na [ **segurança do dispositivo** ](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) página o [Central de segurança do Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center). |
| Modo de Atestado do Active Directory do serviço guardião de host|Não estamos desenvolvendo o modo de Atestado guardião de Host serviço do Active Directory – em vez disso, adicionamos um novo modo de Atestado [atestado de chaves de host](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), que é muito mais simples e igualmente como compatível, como o Active Directory com base Atestado.  Esse novo modo fornece funcionalidade equivalente com uma experiência de instalação, gerenciamento mais simples e menos dependências de infraestrutura que o Atestado do Active Directory. Atestado de chaves de host tem nenhum requisito de hardware adicional além do qual Atestado do Active Directory necessários, para que todos os sistemas existentes permanecerão compatíveis com o novo modo. Ver [implantar hosts protegidos do](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) para obter mais informações sobre suas opções de Atestado. |
| Serviço OneSync|O serviço OneSync sincroniza os dados para os aplicativos de email, calendário e pessoas. Adicionamos um mecanismo de sincronização para o aplicativo do Outlook que fornece a mesma sincronização. |
| Suporte a API de compactação diferencial remoto|Suporte a API de compactação diferencial remoto habilitado a sincronização de dados com uma fonte remota usando tecnologias de compactação, que a quantidade de dados enviados pela rede. Esse suporte não esteja sendo usado por qualquer produto da Microsoft. |
| Extensão do comutador WFP filtro simples|A extensão de comutador de filtro simples da WFP permite aos desenvolvedores compilar [extensões para o comutador virtual do Hyper-V de filtragem de pacotes de rede simples](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering). Você pode obter a mesma funcionalidade, criando uma extensão de filtragem completa. Dessa forma, podemos vai ser removendo essa extensão no futuro. |