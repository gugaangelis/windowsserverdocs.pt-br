---
title: Recursos removidos ou com remoção planejada no Windows Server 2019
description: Saiba mais sobre os recursos e funcionalidades removidos ou com remoção planejada começando com o Windows Server 2019.
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
ms.date: 08/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: 59a31d01d1c5775c837010eca964c72fad8b5c92
ms.sourcegitcommit: 6f8993e2180c4d3c177e3e1934d378959396b935
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000698"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Recursos removidos ou planejados para substituição começando no Windows Server 2019

>Aplica-se a: Windows Server 2019

Cada versão do Windows Server adiciona novos recursos e funcionalidades; removemos também ocasionalmente, recursos e funcionalidades, geralmente porque adicionamos a melhor opção. Veja os detalhes sobre os recursos e funcionalidades que foram removidos no Windows Server 2019.

> [!TIP]
> - Você pode obter acesso antecipado a builds do Windows Server ao ingressar no [Programa Windows Insider](https://insider.windows.com), essa é uma ótima maneira de testar as alterações de recurso.
> - Tem dúvidas sobre outras versões? Confira [Recursos removidos ou com substituição planejada no Windows Server](removed-features.md).

**A lista está sujeita a alteração e pode não incluir todos os recursos ou as funcionalidades afetadas.** 

## <a name="features-we-removed-in-this-release"></a>Recursos que removemos nesta versão

Estamos removendo os seguintes recursos e funcionalidades da imagem de produto instalada no Windows Server 2019. Aplicativos ou código que dependam esses recursos não funcionarão nesta versão, a menos que você use um método alternativo.

| Recurso   | Em vez disso, você pode usar... |
| --------- | -------------------- |
| Digitalização de Negócios, também chamada de DSM (Gerenciamento de Digitalização Distribuída)|Estamos removendo essa funcionalidade de gerenciamento do scanner e verificação de segurança: não há dispositivos compatíveis com esse recurso. |
| Componentes de impressão: agora um componente opcional para instalações Server Core|Em versões anteriores do Windows Server, os componentes de impressão estavam *desabilitados* por padrão na opção de instalação Server Core. Alteramos isso no Windows Server 2016, habilitando-os por padrão. No Windows Server de 2019, esses componentes de impressão novamente são desabilitados por padrão para o Server Core. Se você precisar habilitar os componentes de impressão, poderá fazer isso executando o cmdlet **Install-WindowsFeature Print-Server**. |
| [Agente de Conexão de Área de Trabalho Remota e Host de Virtualização de Área de Trabalho Remota](../remote/remote-desktop-services/desktop-hosting-service.md) em uma instalação Server Core|A maioria das implantações de Serviços de Área de Trabalho Remota têm essas funções colocalizadas com o RDSH (Host da Sessão da Área de Trabalho Remota), que requer um servidor com Experiência desktop; para ser consistente com RDSH, estamos alterando essas funções para também exigirem o servidor com Experiência desktop. Essas funções de RDS não estão mais disponíveis para uso em uma [instalação Server Core](../administration/server-core/what-is-server-core.md). Se você precisa [implantar essas funções como parte da sua infraestrutura de área de trabalho remota](../remote/remote-desktop-services/rds-deploy-infrastructure.md), você pode [instalá-los no Windows Server com Experiência desktop](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Essas funções também são incluídas na opção de instalação da Experiência desktop do Windows Server 2019. |

## <a name="features-were-no-longer-developing"></a>Recursos que não desenvolvemos mais

Não desenvolvemos mais ativamente esses recursos e podemos removê-los de uma atualização futura. Alguns recursos foram substituídos por outros recursos ou funcionalidades, enquanto outros agora estão disponíveis de fontes diferentes. 

Se tiver comentários sobre a substituição proposta de qualquer um desses recursos, você poderá usar o [aplicativo Hub de Comentários](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

| Recurso     | Em vez disso, você pode usar... |
| ----------- | --------------------- |
| Unidade de armazenamento de chaves no Hyper-V|Não estamos mais trabalhando no recurso de unidade de armazenamento de chaves no Hyper-V. Se você estiver usando VMs da geração 1, confira [Segurança da virtualização de VM da geração 1](../virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v.md) para obter informações sobre as opções no futuro. Se você estiver criando novas VMs, use máquinas virtuais da geração 2 com dispositivos TPM para uma solução mais segura. |
| Console de gerenciamento do TPM (Trusted Platform Module)|As informações disponíveis anteriormente no console de gerenciamento do TPM agora estão disponíveis na página [**Segurança do dispositivo**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) na [Central de Segurança do Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center). |
| Modo de atestado do Active Directory do Serviço Guardião de Host|Não estamos mais desenvolvendo o modo de atestado do Active Directory do Serviço Guardião de Host, em vez disso, adicionamos um novo modo de atestado, o [atestado de chaves de host](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), que é muito mais simples e igualmente compatível com o atestado baseado no Active Directory.  Esse novo modo fornece uma funcionalidade equivalente com uma experiência de instalação, gerenciamento mais simples e menos dependências de infraestrutura que o atestado do Active Directory. O atestado de chaves de host não tem nenhum requisito de hardware adicional além do que o atestado do Active Directory exigia, portanto, todos os sistemas existentes permanecerão compatíveis com o novo modo. Confira [Implantar hosts protegidos](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) para obter mais informações sobre suas opções de atestado. |
| Serviço OneSync | O serviço OneSync sincroniza os dados para os aplicativos de Email, Calendário e Pessoas. Adicionamos um mecanismo de sincronização ao aplicativo do Outlook que fornece a mesma sincronização. |
| Suporte à API de compactação diferencial remota | O suporte à API de compactação diferencial remota permite a sincronização de dados com uma fonte remota usando tecnologias de compactação, que minimiza a quantidade de dados enviada pela rede. |
| Extensão do comutador de filtro leve da WFP | A extensão do comutador de filtro leve da WFP permite que os desenvolvedores criem [extensões de filtragem de pacotes de rede simples para o comutador virtual do Hyper-V](https://docs.microsoft.com/windows-hardware/drivers/network/using-virtual-switch-filtering). Você pode obter a mesma funcionalidade criando uma extensão de filtragem completa. Dessa forma, removeremos essa extensão no futuro. |
