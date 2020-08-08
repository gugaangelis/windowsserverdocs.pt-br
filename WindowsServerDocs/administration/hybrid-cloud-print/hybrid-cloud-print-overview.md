---
title: Visão geral da impressão de nuvem híbrida do Windows Server
description: A impressão em nuvem híbrida permite que os profissionais de ti ofereçam suporte aos requisitos de impressão para BYOD ou dispositivos ingressados no domínio.
ms.topic: conceptual
author: trudyha
ms.author: trudyha
ms.date: 10/16/2017
ms.openlocfilehash: 49c5ee234a6983902e7eb2f68e64a058167a3182
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993420"
---
# <a name="windows-server-hybrid-cloud-print-overview"></a>Visão geral da impressão de nuvem híbrida do Windows Server

**Aplica-se a**
-   Windows Server 2016

## <a name="what-is-hybrid-cloud-print"></a>O que é a impressão em nuvem híbrida?
A **impressão em nuvem híbrida** é um novo recurso do Windows Server 2016 disponível por meio **de recursos sob demanda**. Ele permite que os profissionais de ti ofereçam suporte a requisitos de impressão para pessoas que trazem seus próprios dispositivos ou usem dispositivos ingressados na sua Azure Active Directory. Isso inclui dispositivos móveis, como Windows Phone, laptops ou tablets que executam o Windows 10 ou o Windows Mobile. Ele fornece suporte de impressão de qualquer lugar que as pessoas tenham acesso à Internet.

Para administradores de ti, a **impressão de nuvem híbrida** fornece acesso de usuário seguro a impressoras locais usando a autenticação multifator do Azure para validar o acesso do usuário. A funcionalidade de logon único (SSO) simplifica a experiência do usuário. A **impressão em nuvem híbrida** baseia-se na função de **servidor de impressão** do Windows, proporcionando aos profissionais de ti uma experiência semelhante ao gerenciamento de impressoras e à segurança de acesso do usuário.

A **impressão em nuvem híbrida** permite que as pessoas em sua organização imprimam os dispositivos que usam para concluir seu trabalho, mesmo quando estão fora de sua mesa ou local de trabalho.

A **impressão em nuvem híbrida** tem suporte na atualização do Windows 10 para criadores e no Windows 10 S.

## <a name="feature-summary"></a>Resumo de recursos
A **impressão em nuvem híbrida** consiste em dois componentes principais do lado do servidor: serviço de **descoberta** e serviço de **impressão do Windows** .
- Ponto de extremidade do serviço de **descoberta** em execução em um serviço IIS que dá suporte ao padrão do setor da Aliança Mopria para a descoberta de impressora na nuvem.
- Ponto de extremidade do serviço de **impressão do Windows** em execução em um serviço IIS com impressão via Internet suporte para o protocolo IPP para garantir o suporte mais amplo do so cliente.

## <a name="deployment"></a>Implantação
A **impressão em nuvem híbrida** dá suporte a algumas opções de implantação diferentes, dependendo de onde sua organização requer autenticação de usuário. Veja como seria a aparência de uma implantação:

![Um diagrama que mostra uma representação gráfica da solução de impressão de nuvem híbrida](../media/hybrid-cloud-print/wshcp-deployment-options.png)

*Diagrama da solução de impressão de nuvem híbrida*

O diagrama mostra:
- **Impressão em nuvem híbrida** usando Azure Active Directory como o provedor de identidade do usuário.
- Os pontos de extremidade do serviço de **impressão do Windows** e do serviço de **descoberta** são registrados com Azure Active Directory para permitir que o dispositivo cliente recupere o token de autenticação de usuário necessário para usar nesses serviços.
- Um serviço de MDM, como o **Microsoft Intune**, provisiona o dispositivo cliente com políticas necessárias para conectar Azure Active Directory ao serviço de **impressão do Windows** e serviço de **descoberta** .

Esta tabela tem mais informações sobre os elementos no diagrama.

| Elemento | Descrição |
| ------- | ----------- |
| Azure Active Directory  | Fornece e controla a identidade do usuário e a funcionalidade de autorização |
| Active Directory        | Fornece e controla a identidade do usuário e a funcionalidade de autorização |
| Azure AD Connect  | Sincroniza as credenciais do usuário entre o Azure AD e o AD local. |
| Serviço MDM (Intune) | Fornece a funcionalidade de provisionamento de política de dispositivo para garantir que o dispositivo cliente (dispositivo BYOD) esteja em conformidade com as políticas corporativas. |
| Proxy do Azure AD | Fornece uma conexão de longa duração que é estabelecida por trás do firewall para o Azure para permitir que o tráfego de aplicativo configurado específico flua da Internet para a rede corporativa. |
| Aplicativo Web do Azure | O núcleo da solução de impressão de nuvem híbrida. Fornece os pontos de extremidade da Web necessários para descobrir impressoras e enviar conteúdo de impressão para dispositivos não ingressados no domínio. |
| Dispositivo BYOD/Windows Servidor de Impressão spooler/impressora | Esses são os mesmos. Nenhuma alteração na funcionalidade na implantação. |

Há duas maneiras de instalar a **impressão de nuvem híbrida**:
- * * Recursos sob demanda, que confira [configurar recursos sob demanda no Windows Server](../server-manager/configure-features-on-demand-in-windows-server.md) para saber mais sobre como adicionar e remover arquivos de função e recurso.
- * * Configurações do Windows Server 2016, que os administradores podem acessar **configurações**  ->  **aplicativos**  ->  **gerenciar recursos opcionais**  ->  **Adicionar um recurso** e pesquisar o pacote de recursos sob demanda
- Comandos do PowerShell – em uma janela de administrador do PowerShell, execute estes comandos:

```PowerShell
    Install-Module -Name PublishCloudPrinter
    Import-Module PublishCloudPrinter
```