---
title: Guias Complementares de Rede Principal
description: Este tópico fornece uma visão geral dos guias complementares para o guia de rede do Windows Server 2016 Core
manager: brianlic
ms.technology: networking
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b757e1914ee263a041f39e9767d3cb8af38403dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816797"
---
# <a name="core-network-companion-guidance"></a>Diretrizes de complementar de rede principal

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Embora o Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) fornece instruções sobre como implantar um novo Active Directory&reg; floresta com um novo domínio raiz e a infraestrutura de rede suporte, guias complementares fornecem a você com a capacidade de adicionar recursos à sua rede.

Cada guia complementar permite cumprir uma meta específica, depois que você implantou sua rede principal. Em alguns casos, existem vários guias complementarem que, quando implantados juntos e na ordem correta, permitem realizar objetivos muito complexos de forma medida, econômica e razoável.

Se você implantou sua rede principal e seu domínio do Active Directory antes de encontrar o Guia da Rede Principal, ainda pode usar os Guias Complementares para adicionar recursos à sua rede. Simplesmente use o Guia da Rede Principal como uma lista de pré-requisitos e saiba que, para implantar recursos adicionais com os Guias Complementares, a rede deve atender aos pré-requisitos que são fornecidos pelo Guia da Rede Principal.

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Guia complementar da rede principal: Implantar certificados de servidor para implantações com e sem fio do 802.1X 

Este guia complementar explica como construir em cima de rede principal com a implantação de certificados de servidor para computadores que estão executando o servidor de políticas de rede \(NPS\), serviço de acesso remoto \(RAS\), ou ambos.

Certificados de servidor são necessários quando você implanta os métodos de autenticação baseada em certificado com o protocolo de autenticação extensível \(EAP\) e o EAP protegido \(PEAP\) para autenticação de acesso de rede. Implantar certificados de servidor com serviços de certificados do Active Directory \(AD CS\) para autenticação baseada em certificados EAP e PEAP métodos oferece os seguintes benefícios:

- Associando a identidade do servidor NPS ou o acesso remoto a uma chave privada
- Um método eficiente e seguro para registrar automaticamente certificados para servidores NPS e RAS de membros do domínio
- Um método eficiente para o gerenciamento de certificados e autoridades de certificação
- Segurança fornecida por autenticação baseada em certificado
- A capacidade de expandir o uso de certificados para outras finalidades
  
Para obter instruções sobre como implantar certificados de servidor, consulte [implantar certificados de servidor para 802.1 X com fio e implantações sem fio](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md).  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>Guia complementar da rede principal: Implantar acesso sem fio autenticado 802.1X baseado em senha

Este guia complementar explica como construir em cima de rede principal, fornecendo instruções sobre como implantar o Institute of Electrical and Electronics Engineers \(IEEE\) 802.1 X\-autenticado sem fio do IEEE 802.11 acessar usando Protected Extensible autenticação Protocol\ – Microsoft Challenge Handshake Authentication Protocol versão 2 \(PEAP\-MS\-CHAP v2\).

O método de autenticação do PEAP\-MS\-CHAP v2 requer que autenticar servidores que executam o servidor de políticas de rede \(NPS\) apresentar os clientes sem fio com um certificado de servidor para comprovar a identidade do NPS para o cliente, no entanto, a autenticação de usuário não é executada usando um certificado - em vez disso, os usuários fornecer seu nome de usuário de domínio e senha.

Como PEAP\-MS\-CHAP v2 requer que os usuários forneçam credenciais baseadas em senha em vez de um certificado durante o processo de autenticação, é geralmente mais fácil e mais barato de implantar que o EAP\-TLS ou PEAP \-TLS.

Antes de usar este guia para implantar o acesso sem fio com o PEAP\-MS\-método de autenticação do protocolo CHAP v2, você deve fazer o seguinte:

1. Siga as instruções no guia da rede principal para implantar sua infraestrutura de rede central, ou já tem as tecnologias apresentadas no guia implantado em sua rede.
2. Siga as instruções no Core Network Companion guia implantar certificados de servidor para 802.1 X com fio e implantações sem fio ou já tem as tecnologias apresentadas no guia implantado em sua rede.

Para obter instruções sobre como implantar o acesso sem fio com o PEAP\-MS\-CHAP v2, consulte [baseado em senha implantar acesso autenticado 802.1 X sem fio](wireless/a-deploy-8021X-wireless-access.md).

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>Guia complementar da rede principal: Implantar o modo Cache Hospedado do BranchCache

Este guia complementar explica como implantar o BranchCache no modo de Cache hospedado em um ou mais filiais.

BranchCache é uma tecnologia de otimização de largura de banda (WAN) de rede de longa distância que é incluída em algumas edições dos sistemas operacionais Windows Server 2016 e Windows 10, bem como nas versões anteriores do Windows e Windows Server.

Quando você implanta o BranchCache no modo de cache hospedado, o cache de conteúdo em uma filial é hospedado em um ou mais servidores, que são chamados de servidores de cache hospedado. Servidores de cache hospedado podem executar cargas de trabalho além de hospedar o cache, que permite que você use o servidor para vários fins na filial.

O modo de cache hospedado do BranchCache aumenta a eficiência de cache porque o conteúdo estará disponível mesmo se o cliente que originalmente solicitado e os dados armazenados em cache está offline. Como o servidor de cache hospedado está sempre disponível, mais conteúdo é armazenado em cache, proporcionando maiores economias de largura de banda de WAN e aumentando a eficiência do BranchCache.

Quando você implanta o modo de cache hospedado, todos os clientes em uma filial de várias sub-redes podem acessar um único cache, o que é armazenado no servidor de cache hospedado, mesmo que os clientes estão em sub-redes diferentes.

Para obter instruções sobre como implantar o BranchCache no modo de Cache hospedado, consulte [implantar o BranchCache modo de Cache hospedado](bc-hcm/1-Deploy-Bc-Hcm.md).
