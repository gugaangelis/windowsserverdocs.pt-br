---
title: Etapas de pós-implantação para Controlador de Rede
description: Este tópico fornece instruções de configuração de certificado para implantações de Kerberos que não seja do controlador de rede no Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f7d6bbd50537e24f392eabde7d103c91a4f07c90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871077"
---
# <a name="post-deployment-steps-for-network-controller"></a>Etapas de pós-implantação para Controlador de Rede

Quando você instala o controlador de rede, você pode escolher implantações Kerberos ou não Kerberos.

Para não\-implantações de Kerberos, você deve configurar certificados.

## <a name="configure-certificates-for-non-kerberos-deployments"></a>Configurar certificados para implantações não Kerberos

Se os computadores ou máquinas virtuais \(VMs\) para o controlador de rede e o cliente de gerenciamento não domínio\-unidas, você deve configurar o certificado\-autenticação baseada em concluindo as seguintes etapas.

- Crie um certificado no controlador de rede para a autenticação do computador. O nome de assunto do certificado deve ser o mesmo que o nome DNS do computador do controlador de rede ou na VM.

- Crie um certificado no cliente de gerenciamento. Esse certificado deve ser confiável pelo controlador de rede.
  
- Inscrever um certificado no computador do controlador de rede ou na VM. O certificado deve atender os requisitos a seguir.
  
    -  A finalidade de autenticação de servidor e a finalidade de autenticação de cliente devem ser configurados no uso avançado de chave \(EKU\) ou extensões de diretivas de aplicativo. O identificador de objeto para autenticação de servidor é 1.3.6.1.5.5.7.3.1. O identificador de objeto para autenticação de cliente é 1.3.6.1.5.5.7.3.2.
  
    - O nome de assunto do certificado deve resolver para:
  
        - O endereço IP do computador do controlador de rede ou VM, se o controlador de rede for implantado em um único computador ou VM.

        - O endereço IP de REST, se o controlador de rede for implantado em vários computadores, várias VMs ou ambos.
  
    - Esse certificado deve ser confiável por todos os clientes REST. O certificado também deve ser confiável pelo balanceamento de carga de Software (SLB) multiplexador (MUX) e os computadores host southbound que são gerenciados pelo controlador de rede.
  
    - O certificado pode ser registrado por uma autoridade de certificação (CA) ou pode ser um certificado autoassinado. Os certificados autoassinados não são recomendados para implantações de produção, mas são aceitáveis para ambientes de laboratório de teste.
  
    - O mesmo certificado deve ser provisionado em todos os nós de controlador de rede. Depois de criar o certificado em um nó, você pode exportar o certificado (com a chave privada) e importá-lo nos outros nós.

Para obter mais informações, consulte [Controlador de Rede](Network-Controller.md).
