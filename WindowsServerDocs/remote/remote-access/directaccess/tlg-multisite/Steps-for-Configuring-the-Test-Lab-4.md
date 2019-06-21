---
title: Etapas para configurar o Test Lab
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3d01dd8002e28fb127ac6b1b4cea25c58953521
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281390"
---
# <a name="steps-for-configuring-the-test-lab"></a>Etapas para configurar o Test Lab

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

As etapas a seguir descrevem como configurar a infraestrutura de acesso remoto, configure os servidores de acesso remoto e clientes e testar a conectividade do DirectAccess em sub-redes da rede doméstica e de Internet.  
  
Este guia de laboratório de teste, você criará uma implantação multissite do acesso remoto executando as seguintes etapas:  
  
-   [ETAPA 1: Concluir a configuração de Base](assetId:///9eb4a9ba-9118-4ea3-8963-e643ec81c3ed). Conclua todas as etapas a [guia do laboratório de teste: Demonstrar a configuração de servidor único do DirectAccess com misto de IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [ETAPA 2: Instalar e configurar o roteador 1](assetId:///e4b1a298-d5b0-410e-970b-c5358a9378f9). Roteador 1 fornece o roteamento e encaminhamento de funcionalidade entre as sub-redes da rede corporativa e da rede corporativa de 2.  
  
-   [ETAPA 3: Instalar e configurar CLIENT2](assetId:///6cbee1b5-f6f6-443f-8fa9-31cc5c05a0ee). CLIENT2 é um computador de cliente do Windows 7 é usado para demonstrar as versões anteriores a compatibilidade de uma implantação do Windows Server 2016, o Windows Server 2012 R2 ou o acesso remoto do Windows Server 2012.  
  
-   [ETAPA 4: Configurar o APP1](assetId:///a0ee655e-c01e-4bf3-a7b3-064e9614f810). Configure o APP1 como o gateway padrão e 2-DC1 como o servidor DNS alternativo com o roteador 1.  
  
-   [ETAPA 5: Configurar o DC1](assetId:///205ca795-93ce-4e53-aa6b-b44c87f0e14a). Configure o DC1 com um site adicional do Active Directory e os grupos de segurança adicionais para computadores cliente do Windows 7.  
  
-   [ETAPA 6: Instalar e configurar o DC1 2](assetId:///16752f61-edbf-4ff4-9d7a-e2077b66a127). Em uma implantação multissite, você tem dois ou mais domínios e sites. 2-DC1 fornece serviços DNS para o domínio corp2.corp.contoso.com e controlador de domínio.  
  
-   [ETAPA 7: Instalar e configurar o APP1 2](assetId:///7d04b54e-590a-4d33-9766-415789859f29). 2-APP1 é um servidor web e de arquivos na rede da rede corporativa de 2.  
  
-   [ETAPA 8: Configurar INET1](assetId:///8ecc0b63-8626-4939-8d26-3d51d051d231). INET1 simula a Internet neste guia de laboratório de teste. Você deve configurar uma entrada DNS que aponta para o endereço IP público de EDGE1 2.  
  
-   [ETAPA 9: Configurar EDGE1](assetId:///562744dc-30f6-42fa-bd5f-60a013b2179e). Configure o roteamento em EDGE1 e o servidor DNS da rede corporativa de 2.  
  
-   [ETAPA 10: Instalar e configurar 2 EDGE1](assetId:///1938c4f3-ca96-475d-9f2e-6bea3b7a4130). Dois servidores de acesso remoto são necessários em uma implantação multissite. 2-EDGE1 fornece serviços de acesso remoto para o domínio de segundo.  
  
-   [ETAPA 11: Configurar a implantação multissite](assetId:///537e4b68-043f-49c9-94d8-15ce8c4b18e2). Depois de configurar ambos os servidores de acesso remoto, você pode configurar sua implantação multissite.  
  
-   [ETAPA 12: Testar a conectividade do DirectAccess](assetId:///aa293b5d-4b6f-4004-95f3-0ab54804b15c). Teste a conectividade do DirectAccess em ambos os computadores cliente da sub-rede da Internet por meio de EDGE1 e EDGE1 2.  
  
-   [ETAPA 13: Testar a conectividade do DirectAccess por trás de um dispositivo NAT](assetId:///41f8195b-00a1-4991-9db8-3703514dbe0c). Teste a conectividade do DirectAccess por trás de um dispositivo NAT.  
  
-   [ETAPA 14: Instantâneo da configuração](assetId:///7b56d5c9-c334-463e-9e29-d652ca110d84). Depois de concluir o laboratório de teste, tire um instantâneo de trabalho implantação multissite do acesso remoto para que você possa retornar a ele mais tarde para testar cenários adicionais.  
  


