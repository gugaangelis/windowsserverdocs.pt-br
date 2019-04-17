---
title: Pós-Post-Deployment Etapas para o controlador de rede
description: Este tópico fornece instruções de configuração de certificado para implantações de Kerberos não do controlador de rede no Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f7d6bbd50537e24f392eabde7d103c91a4f07c90
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="post-deployment-steps-for-network-controller"></a>Pós-Post-Deployment Etapas para o controlador de rede

Quando você instala o controlador de rede, você pode escolher implantações Kerberos ou não Kerberos.

Para implantações de non\ Kerberos, você deve configurar certificados.

## <a name="configure-certificates-for-non-kerberos-deployments"></a>Configurar certificados para implantações de Kerberos não

Se o \(VMs\) computadores ou máquinas virtuais para o controlador de rede e o cliente de gerenciamento não estiverem ingressados em domain\, você deve configurar a autenticação baseada em certificate\ concluindo as seguintes etapas.

- Crie um certificado no controlador de rede para autenticação do computador. Nome do requerente do certificado deve ser igual ao nome DNS do computador de controlador de rede ou VM.

- Crie um certificado no cliente de gerenciamento. Esse certificado deve ser confiável com o controlador de rede.
  
- Registre um certificado no computador do controlador de rede ou VM. O certificado deve atender aos seguintes requisitos.
  
    -  A finalidade de autenticação de servidor e a finalidade de autenticação de cliente devem ser configurados no uso avançado de chave \(EKU\) ou extensões de políticas de aplicativos. O identificador de objeto para autenticação de servidor é 1.3.6.1.5.5.7.3.1. O identificador de objeto para autenticação de cliente é 1.3.6.1.5.5.7.3.2.
  
    - Nome do requerente do certificado deve ser resolvida em:
  
        - O endereço IP do computador de controlador de rede ou VM, se o controlador de rede é implantado em um único computador ou a VM.

        - O endereço IP de REST, se o controlador de rede é implantado em vários computadores, várias VMs ou ambos.
  
    - Esse certificado deve ser confiável pelos todos os clientes REST. O certificado também deve ser confiável pelos balanceamento de carga de Software (SLB) multiplexador (Multiplexador) e os computadores host southbound que são gerenciados pelo controlador de rede.
  
    - O certificado pode ser registrado por uma autoridade de certificação (CA) ou pode ser um certificado autoassinado. Certificados auto-assinados não são recomendados para implantações de produção, mas são aceitáveis para ambientes de laboratório de teste.
  
    - O mesmo certificado deve ser provisionado em todos os nós de controlador de rede. Depois de criar o certificado em um nó, você pode exportar o certificado (com a chave privada) e importá-la em outros nós.

Para obter mais informações, consulte [rede controlador](Network-Controller.md).
