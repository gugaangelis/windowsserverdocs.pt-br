---
title: Solução de problemas de ativação de volume do Windows
description: Lista recursos que fornecem informações sobre as melhores práticas para ativação de volume e informações sobre como solucionar problemas de ativação
ms.topic: troubleshooting
ms.date: 09/24/2019
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: ce6c2e830e7c30e24112854b54e12909c43fa6f7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972323"
---
# <a name="troubleshooting-windows-volume-activation"></a>Solução de problemas de ativação de volume do Windows

A ativação do produto é o processo de validação de software após a instalação em um computador específico. A ativação confirma que o produto é original (não uma cópia fraudulenta) e que a chave do produto (Product Key) ou o número de série são válidos e não foram comprometidos nem revogados. A ativação também estabelece um link ou relacionamento entre a chave do produto (Product Key) e a instalação.

A ativação de volume é o processo de ativar produtos licenciados por volume. Para tornar-se um cliente de licenciamento por volume, a organização precisa celebrar um contrato de licenciamento por volume com a Microsoft. A Microsoft oferece programas de licenciamento por volume personalizados de acordo com o tamanho e a preferência de compra da organização. Para obter mais informações, confira o [Centro de Serviços de Licenciamento por Volume da Microsoft](https://www.microsoft.com/Licensing/servicecenter/default.aspx).

O [Guia de ativação do Windows Server 2016](server-2016-activation.md) concentra-se na tecnologia de ativação do KMS (Serviço de Gerenciamento de Chaves). Esta seção aborda problemas comuns e fornece diretrizes de solução de problemas para o KMS e várias outras tecnologias de ativação de volume.

## <a name="best-practices-for-volume-activation"></a>Melhores práticas para ativação de volume

Os artigos a seguir fornecem informações técnicas e melhores práticas para as tecnologias de ativação de volume da Microsoft.

### <a name="key-management-service-kms"></a>KMS (Serviço de Gerenciamento de Chaves)

- [Planejamento da ativação de volume](/windows/deployment/volume-activation/plan-for-volume-activation-client)
- [Noções básicas sobre o KMS](/previous-versions/tn-archive/ff793434(v=technet.10))
- [Implantação da ativação do KMS](/previous-versions/tn-archive/ff793409%28v=technet.10%29)
- [Configuração de hosts KMS](/previous-versions/tn-archive/ff793407%28v%3dtechnet.10%29)
- [Configuração do DNS](/previous-versions/tn-archive/ff793405%28v%3dtechnet.10%29)
- [Ativar usando o Serviço de Gerenciamento de Chaves](/windows/deployment/volume-activation/activate-using-key-management-service-vamt)

### <a name="active-directory-based-activation-adba"></a>ADBA (ativação baseada no Active Directory)

- [Implantar ativação baseada no Active Directory](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502534%28v%3dws.11%29)
- [Ativar usando a ativação baseada no Active Directory](/windows/deployment/volume-activation/activate-using-active-directory-based-activation-client)
- [Visão geral da ativação baseada no Active Directory](/windows/deployment/volume-activation/active-directory-based-activation-overview)

### <a name="multiple-activation-key-mak-activation"></a>Ativação da MAK (chave de ativação múltipla)

- [Uso da ativação da MAK](/previous-versions/tn-archive/ff793438%28v=technet.10%29)
- [Noções básicas sobre a ativação da MAK](/previous-versions/tn-archive/ff793435%28v%3dtechnet.10%29)
- [Ativação de clientes MAK](/previous-versions/tn-archive/ff793398%28v%3dtechnet.10%29)

### <a name="subscription-activation"></a>Ativação de assinatura

- [Ativação de assinatura do Windows 10](/windows/deployment/windows-10-subscription-activation)
- [Implantar licenças do Windows 10 Enterprise](/windows/deployment/deploy-enterprise-licenses)
- [Windows 10 Enterprise E3 em CSP](/windows/deployment/windows-10-enterprise-e3-overview)

## <a name="resources-for-troubleshooting-activation-issues"></a>Recursos para solução de problemas de ativação

Os artigos a seguir fornecem diretrizes e informações sobre ferramentas para solucionar problemas de ativação de volume:

- [Diretrizes para solucionar problemas do KMS (Serviço de Gerenciamento de Chaves)](activation-troubleshoot-kms-general.md)
- [Opções Slmgr.vbs para obter informações de ativação de volume](activation-slmgr-vbs-options.md)
- [Exemplo: Solução de problemas de clientes ADBA que não são ativados](activation-troubleshoot-adba-clients.md)

Os artigos a seguir fornecem orientações para tratar de problemas de ativação mais específicos:

- [Resolução de códigos de erro de ativação comuns](activation-error-codes.md)
- [Ativação do KMS: problemas conhecidos](activation-troubleshoot-KMS-issues.md)
- [Ativação da MAK: problemas conhecidos](activation-troubleshoot-MAK-issues.md)
- [Diretrizes para solução de problemas de ativação relacionados ao DNS](common-troubleshooting-procedures-kms-dns.md)
- [Como recompilar o arquivo Tokens.dat](activation-rebuild-tokens-dat-file.md)
