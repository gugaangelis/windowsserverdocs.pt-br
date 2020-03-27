---
title: Etapas de pós-implantação para Controlador de Rede
description: Este tópico fornece instruções de configuração de certificado para implantações não Kerberos do controlador de rede no Windows Server 2016 datacenter.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7a26966fc724aa881e5e20caf6609eaec164ccb0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317040"
---
# <a name="post-deployment-steps-for-network-controller"></a>Etapas de pós-implantação para Controlador de Rede

Ao instalar o controlador de rede, você pode escolher implantações Kerberos ou não Kerberos.

Para implantações não\-Kerberos, você deve configurar certificados.

## <a name="configure-certificates-for-non-kerberos-deployments"></a>Configurar certificados para implantações não Kerberos

Se os computadores ou máquinas virtuais \(VMs\) para o controlador de rede e o cliente de gerenciamento não forem ingressados no domínio\-, você deverá configurar a autenticação baseada em\-de certificado ao concluir as etapas a seguir.

- Crie um certificado no controlador de rede para autenticação do computador. O nome da entidade do certificado deve ser o mesmo que o nome DNS do computador do controlador de rede ou da VM.

- Crie um certificado no cliente de gerenciamento. Esse certificado deve ser confiável pelo controlador de rede.
  
- Registre um certificado no computador ou VM do controlador de rede. O certificado deve atender aos seguintes requisitos.
  
    -  Tanto a finalidade de autenticação do servidor quanto a finalidade da autenticação do cliente devem ser configuradas no uso avançado de chave \(EKU\) ou extensões de políticas de aplicativo. O identificador de objeto para autenticação de servidor é 1.3.6.1.5.5.7.3.1. O identificador de objeto para autenticação de cliente é 1.3.6.1.5.5.7.3.2.
  
    - O nome da entidade do certificado deve ser resolvido para:
  
        - O endereço IP do computador do controlador de rede ou da VM, se o controlador de rede for implantado em um único computador ou VM.

        - O endereço IP REST, se o controlador de rede for implantado em vários computadores, em várias VMs ou em ambos.
  
    - Esse certificado deve ser confiável para todos os clientes REST. O certificado também deve ser confiável pelo multiplexador do SLB (balanceamento de carga de software) e pelos computadores host Southbound gerenciados pelo controlador de rede.
  
    - O certificado pode ser registrado por uma autoridade de certificação (CA) ou pode ser um certificado autoassinado. Os certificados autoassinados não são recomendados para implantações de produção, mas são aceitáveis para ambientes de laboratório de teste.
  
    - O mesmo certificado deve ser provisionado em todos os nós do controlador de rede. Depois de criar o certificado em um nó, você pode exportar o certificado (com a chave privada) e importá-lo nos outros nós.

Para obter mais informações, consulte [Controlador de Rede](Network-Controller.md).
