---
title: Etapas para configurar o Test Lab
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7fc886d8b4f68c86885cbe5c032247722d88e269
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955195"
---
# <a name="steps-for-configuring-the-test-lab"></a>Etapas para configurar o Test Lab

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

As etapas a seguir descrevem como configurar a infraestrutura de acesso remoto, configurar os servidores de acesso remoto e os clientes e testar a conectividade do DirectAccess nas sub-redes Internet e HomeNet.

Neste guia de laboratório de teste, você criará uma implantação de acesso remoto multissite executando as seguintes etapas:

-   [Etapa 1: concluir a configuração de base](assetId:///9eb4a9ba-9118-4ea3-8963-e643ec81c3ed). Conclua todas as etapas no [Guia de laboratório de teste: demonstre a configuração do servidor único do DirectAccess com IPv4 e IPv6 mistos](https://go.microsoft.com/fwlink/p/?LinkId=237004).

-   [Etapa 2: instalar e configurar o ROUTER1](assetId:///e4b1a298-d5b0-410e-970b-c5358a9378f9). O ROUTER1 fornece a funcionalidade de roteamento e encaminhamento entre as sub-redes corpnet e 2-corpnet.

-   [Etapa 3: instalar e configurar o CLIENT2](assetId:///6cbee1b5-f6f6-443f-8fa9-31cc5c05a0ee). CLIENT2 é um computador cliente do Windows 7 que é usado para demonstrar a compatibilidade com versões anteriores de uma implantação de acesso remoto do Windows Server 2016, do Windows Server 2012 R2 ou do Windows Server 2012.

-   [Etapa 4: configurar o App1](assetId:///a0ee655e-c01e-4bf3-a7b3-064e9614f810). Configure o APP1 com ROUTER1 como o gateway padrão e 2-DC1 como o servidor DNS alternativo.

-   [Etapa 5: configurar DC1](assetId:///205ca795-93ce-4e53-aa6b-b44c87f0e14a). Configure o DC1 com um site Active Directory adicional e grupos de segurança adicionais para computadores cliente do Windows 7.

-   [Etapa 6: instalar e configurar o 2-DC1](assetId:///16752f61-edbf-4ff4-9d7a-e2077b66a127). Em uma implantação multissite, você tem dois ou mais domínios e sites. 2-DC1 fornece o controlador de domínio e os serviços DNS para o domínio corp2.corp.contoso.com.

-   [Etapa 7: instalar e configurar o 2-App1](assetId:///7d04b54e-590a-4d33-9766-415789859f29). 2-APP1 é um servidor de arquivos e Web na rede de 2 corpnet.

-   [Etapa 8: configurar o INET1](assetId:///8ecc0b63-8626-4939-8d26-3d51d051d231). INET1 simula a Internet neste guia de laboratório de teste. Você deve configurar uma entrada DNS que seja resolvida para o endereço IP público de 2-EDGE1.

-   [Etapa 9: configurar o EDGE1](assetId:///562744dc-30f6-42fa-bd5f-60a013b2179e). Configure o servidor DNS 2-corpnet e o roteamento no EDGE1.

-   [Etapa 10: instalar e configurar o 2-EDGE1](assetId:///1938c4f3-ca96-475d-9f2e-6bea3b7a4130). Dois servidores de acesso remoto são necessários em uma implantação multissite. 2-EDGE1 fornece serviços de acesso remoto para o segundo domínio.

-   [Etapa 11: configurar a implantação multissite](assetId:///537e4b68-043f-49c9-94d8-15ce8c4b18e2). Depois de configurar os servidores de acesso remoto, você pode configurar a implantação multissite.

-   [Etapa 12: testar a conectividade do DirectAccess](assetId:///aa293b5d-4b6f-4004-95f3-0ab54804b15c). Teste a conectividade do DirectAccess de ambos os computadores cliente da sub-rede da Internet por meio de EDGE1 e 2-EDGE1.

-   [Etapa 13: testar a conectividade do DirectAccess por trás de um dispositivo NAT](assetId:///41f8195b-00a1-4991-9db8-3703514dbe0c). Teste a conectividade do DirectAccess por trás de um dispositivo NAT.

-   [Etapa 14: instantâneo da configuração](assetId:///7b56d5c9-c334-463e-9e29-d652ca110d84). Depois de concluir o laboratório de teste, tire um instantâneo da implantação de multissite de acesso remoto em funcionamento para que você possa retornar a ele mais tarde para testar cenários adicionais.



