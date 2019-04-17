---
title: Guias de complementar de rede principal
description: Este tópico fornece uma visão geral das guias de complemento ao guia de rede do Windows Server 2016 Core
manager: brianlic
ms.technology: networking
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3c272c51cc69017b75e50e79e58186c0ea7c6391
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-companion-guides"></a>Guias de complementar de rede principal

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Enquanto o Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) fornece instruções sobre como implantar um novo Active Directory&reg; floresta com um novo domínio raiz e a infraestrutura de rede suporte, guias do complemento para fornecer a capacidade de adicionar recursos à sua rede.

Cada guia complementar permite que você realize uma meta específica depois que você tiver implantado sua rede principal. Em alguns casos, há que vários complementar orienta que, quando implantados juntos e na ordem correta, permitem que você realizar muito complexas metas de uma forma de medida, econômico e razoável.

Se tiver implantado sua rede de domínio e núcleo do Active Directory antes de encontrar o guia da rede principal, você ainda pode usar os guias de complemento para adicionar recursos a sua rede. Basta usar a guia da rede principal como uma lista dos pré-requisitos e souber que implantar recursos adicionais com os guias de complemento, sua rede deve atender aos pré-requisitos que são fornecidos pela guia da rede principal.

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Guia complementar da rede principal: Implantar certificados de servidor para 802.1 X implantações com e sem fio 

Este guia prático explica como se basear a rede principal implantando certificados de servidor para computadores que executam o servidor de política de rede \(NPS\), serviço de acesso remoto \(RAS\) ou ambos.

Certificados de servidor são necessários ao implantar métodos de autenticação baseada em certificado com Extensible Authentication Protocol \(EAP\) e \(PEAP\) EAP protegido para autenticação de acesso de rede. Implantando certificados de servidor com serviços de certificados do Active Directory \(AD CS\) para métodos de autenticação baseada em certificado EAP e PEAP fornece os seguintes benefícios:

- Associando a identidade do servidor NPS ou RAS a uma chave privada
- Um método seguro e eficientes de custo para registrar automaticamente certificados para servidores NPS e RAS de membro do domínio
- Um método eficiente para o gerenciamento de certificados e autoridades de certificação
- Segurança fornecida pelos autenticação baseada em certificado
- A capacidade para expandir o uso de certificados para fins de adicionais
  
Para obter instruções sobre como implantar certificados de servidor, consulte [implantar certificados de servidor para 802.1 X com e sem fio implantações](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md).  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>Guia complementar da rede principal: Implantar baseada em senha acesso autenticado 802.1 X sem fio

Este guia prático explica como se basear a rede principal, fornecendo instruções sobre como implantar Institute of Electrical e engenheiros de aparelhos eletrônicos \(IEEE\) 802.1X\-acesso sem fio 802.11 IEEE usando protegido Extensible Authentication Protocol\ – Microsoft Challenge Handshake Authentication Protocol versão 2 autenticado \ (PEAP\-MS\-CHAP v2\).

O método de autenticação PEAP\-MS\-CHAP v2 requer que porém autenticando servidores que executam o servidor de política de rede \(NPS\) presentes clientes sem fio com um certificado de servidor para provar a identidade do servidor NPS para o cliente, autenticação de usuário não é executada usando um certificado – em vez disso, os usuários fornecem o nome de usuário de domínio e a senha.

Como PEAP\-MS\-CHAP v2 exige que os usuários forneçam credenciais baseada em senha, em vez de um certificado durante o processo de autenticação, é geralmente mais fácil e mais econômica implantar que EAP\ TLS ou PEAP\-TLS.

Antes de usar este guia para implantar o acesso sem fio com o método de autenticação PEAP\-MS\-CHAP v2, você deve fazer o seguinte:

1. Siga as instruções no guia de rede principal para implantar a infraestrutura da rede principal ou já tem as tecnologias apresentadas desse guia implantado em sua rede.
2. Siga as instruções nos principais rede complementar guia implantar certificados do servidor para 802.1 X com e sem fio implantações ou já tem as tecnologias apresentadas desse guia implantado em sua rede.

Para obter instruções sobre como implantar o acesso sem fio com PEAP\-MS\-CHAP v2, consulte [baseada em senha implantar 802.1 X autenticado acesso sem fio](wireless/a-deploy-8021X-wireless-access.md).

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>Guia complementar da rede principal: Implantar o modo de Cache hospedado BranchCache

Este guia complementar explica como implantar BranchCache no modo de Cache hospedado em um ou mais filiais.

BranchCache é uma tecnologia de otimização de largura de banda (WAN) de rede de longa distância que está incluída em algumas edições dos sistemas operacionais Windows Server 2016 e o Windows 10, bem como nas versões anteriores do Windows e Windows Server.

Quando você implanta BranchCache no modo de cache hospedado, o cache de conteúdo em uma filial é hospedado em um ou mais computadores de servidor, que são chamados de servidores de cache. Servidores de cache hospedado podem executar cargas de trabalho além de cache, que permite que você use o servidor para várias finalidades na filial de hospedagem.

Modo de cache BranchCache hospedado aumenta a eficiência do cache porque o conteúdo está disponível, mesmo se o cliente que originalmente solicitado e os dados armazenados em cache estiver offline. Como o servidor de cache hospedado sempre está disponível, mais conteúdo é armazenado em cache, fornecendo maior economia de largura de banda WAN, e a eficiência BranchCache é aprimorada.

Quando você implanta o modo de cache hospedado, todos os clientes em uma filial múltiplo sub-rede podem acessar um cache único, que é armazenado no servidor de cache hospedado, mesmo que os clientes estão em sub-redes diferentes.

Para obter instruções sobre como implantar BranchCache no modo de Cache hospedado, consulte [implantar BranchCache hospedado Cache modo](bc-hcm/1-Deploy-Bc-Hcm.md).
