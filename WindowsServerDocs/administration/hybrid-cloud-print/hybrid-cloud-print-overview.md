---
title: Visão geral de impressão de nuvem híbrida do Windows Server
description: Impressão em nuvem híbrida permite que os profissionais de TI oferecer suporte a requisitos de impressão para BYOD ou domínio ingressado em dispositivos.
ms.prod: w10
ms.mktglfcycl: manage
ms.sitesec: library
ms.pagetype: store
ms.technology: server-general
author: TrudyHa
ms.author: TrudyHa
ms.date: 10/16/2017
ms.openlocfilehash: faa9fde857a9a4ee3f7c03f682b3dbced0340417
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878827"
---
# <a name="windows-server-hybrid-cloud-print-overview"></a>Visão geral de impressão de nuvem híbrida do Windows Server

**Aplica-se a**
-   Windows Server 2016

## <a name="what-is-hybrid-cloud-print"></a>O que é a impressão de nuvem híbrida?
**Impressão de nuvem híbrida** é um novo recurso do Windows Server 2016 disponível por meio **recursos sob demanda**. Ele permite que os profissionais de TI oferecer suporte a requisitos de impressão para as pessoas que trazem seus próprios dispositivos, ou usam dispositivos ingressados no Azure Active Directory. Isso inclui dispositivos móveis, como o Windows phone, laptops ou tablets que executam o Windows 10 ou Windows Mobile. Ele fornece suporte de impressão de qualquer lugar as pessoas têm acesso à Internet.

Para os administradores de TI **Hybrid Cloud Print** fornece acesso de usuário segura a impressoras locais usando a autenticação multifator do Azure para validar o acesso do usuário. Única funcionalidade logon único (SSO) simplifica a experiência do usuário. **Impressão de nuvem híbrida** se baseia no Windows **servidor de impressão** função, fornecendo os profissionais de TI uma experiência semelhante ao gerenciamento de impressoras e segurança de acesso do usuário.

**Impressão em nuvem híbrida** permite que as pessoas em sua organização para os dispositivos que usam para concluir seu trabalho – mesmo quando estiverem fora de sua mesa ou local de trabalho de impressão.

**Impressão de nuvem híbrida** é suportado no Windows 10 Creators Update e Windows 10 S.
 
## <a name="feature-summary"></a>Resumo de recursos
**Impressão de nuvem híbrida** consiste em dois componentes principais do lado do servidor: **Descoberta** serviço, e **impressão do Windows** service.
- **Descoberta** o ponto de extremidade de serviço está em execução em um serviço IIS padrão da indústria Mopria Alliance de suporte para a descoberta de impressora na nuvem.
- **Impressão do Windows** suporte do ponto de extremidade de serviço em execução em um serviço do IIS que dão suporte a indústria padrão protocolo IPP (Internet Printing) para garantir que o sistema operacional de cliente mais amplo.

## <a name="deployment"></a>Implantação
**Impressão de nuvem híbrida** dá suporte a algumas opções de implantação diferentes, dependendo de onde sua organização exige a autenticação do usuário. Aqui está o que poderia ser a aparência de uma implantação:

![Um diagrama que mostra uma representação gráfica da solução de impressão de nuvem híbrida](../media/hybrid-cloud-print/wshcp-deployment-options.png)

*Diagrama da solução de impressão de nuvem híbrida*

O diagrama mostra:
- **Impressão de nuvem híbrida** usando o Azure Active Directory como o provedor de identidade do usuário. 
- **Impressão do Windows** service e **descoberta** pontos de extremidade de serviço são registrados com o Azure Active Directory para habilitar o dispositivo cliente recuperar o token de autenticação de usuário necessário para usar em relação a esses serviços. 
- Um MDM de serviço, como **Microsoft Intune**, provisiona o dispositivo de cliente com as políticas necessárias para conectar o Azure Active Directory para **impressão do Windows** service e **descoberta**service.

Esta tabela tem mais informações sobre os elementos no diagrama.  

| Elemento | Descrição |
| ------- | ----------- |
| Azure Active Directory  | Fornece e controla a funcionalidade de identidade e autorização do usuário |
| Active Directory        | Fornece e controla a funcionalidade de identidade e autorização do usuário |
| Azure AD Connect  | Sincroniza as credenciais do usuário entre o Azure AD e AD local. |
| Serviço MDM (Intune) | Fornece a funcionalidade de provisionamento de política do dispositivo para garantir que o dispositivo de cliente (dispositivo BYOD) está em conformidade com políticas corporativas. |
| Proxy do Azure AD | Fornece uma conexão de longa duração que é estabelecido por trás do firewall para o Azure para permitir o tráfego específico de aplicativo configurado flua da Internet na rede corporativa. |
| Azure Web App | O núcleo da solução de impressão de nuvem híbrida. Fornece os pontos de extremidade da web necessária para descobrir impressoras e enviar o conteúdo impresso para não de domínio. |
| Dispositivo BYOD / Windows Server Spooler de impressão / impressora | Esses são como-está. Nenhuma alteração na funcionalidade da implantação. |

Há duas maneiras de instalar **Hybrid Cloud Print**:
- **Recursos sob demanda** -consulte [configurar recursos sob demanda no Windows Server](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) para saber mais sobre como adicionar e remover arquivos de funções e recursos. 
- **Configurações do Windows Server 2016** -os administradores podem ir para **configurações** -> **aplicativos** -> **gerenciar recursos opcionais**  ->  **Adicionar um recurso** e pesquise os recursos no pacote de demanda 
- Comandos do PowerShell - janela de administrador em um PowerShell, executados estes comandos:

```PowerShell
    Install-Module -Name PublishCloudPrinter
    Import-Module PublishCloudPrinter
    ```
