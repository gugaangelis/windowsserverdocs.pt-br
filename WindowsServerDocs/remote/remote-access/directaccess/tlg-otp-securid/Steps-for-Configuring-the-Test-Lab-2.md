---
title: Etapas para configurar o Test Lab
description: Este tópico faz parte do guia de laboratório de teste - demonstrar o DirectAccess com autenticação OTP e SecurID de RSA para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 991f06568a16f7e8bcce3bc6c86eef3467ea31e1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826287"
---
# <a name="steps-for-configuring-the-test-lab"></a>Etapas para configurar o Test Lab

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

As etapas a seguir descrevem como configurar a infraestrutura de acesso remoto, configure o servidor de acesso remoto e o cliente e testar a conectividade do DirectAccess em sub-redes da rede doméstica e a Internet.  
  
Este guia de laboratório de teste, você criará um acesso remoto com o ambiente de OTP, executando as seguintes etapas:  
  
-   [ETAPA 1: Concluir a configuração do DirectAccess](assetId:///4dbf877f-02fb-439b-907a-f5b3f1d8afa6). Conclua todas as etapas a [guia do laboratório de teste: Demonstrar a configuração de servidor único do DirectAccess com misto de IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [ETAPA 2: Configurar o APP1](assetId:///c1bb590f-91d4-4ed5-bceb-b0e36eabd4ff). Configure o APP1 com modelos de certificado OTP para uso pelo EDGE1.  
  
-   [ETAPA 3: Configurar o DC1](assetId:///904a6edc-a771-45ed-9630-a34a680bb522). Verifique se o que nome da entidade de usuário definidos no DC1.  
  
-   [ETAPA 7: Instalar e configurar o RSA](assetId:///baa4c28c-add7-42e2-8afd-ccc7a559406a). Instalar e configurar o servidor RSA, um RADIUS e OTP e configurar EDGE1 para OTP.  
  
-   [ETAPA 11: Verificar a integridade OTP em EDGE1](assetId:///3b397a4a-8478-47f2-a932-9e8e048c14ba). Certifique-se de que o status de OTP está adequado no servidor de acesso remoto.  
  
-   [ETAPA 8: Testar a conectividade do DirectAccess da sub-rede da rede doméstica](assetId:///ba1652a6-0692-4add-91ca-34a84956ba14). Testar a funcionalidade de OTP do DirectAccess por trás de um dispositivo NAT.  
  
-   [ETAPA 10: Testar a conectividade do DirectAccess da Internet](assetId:///321149eb-5f23-4a0b-b8fb-1244540126e9). Teste a conectividade de cliente do DirectAccess da Internet.  
  
-   [ETAPA 12: Instantâneo da configuração](assetId:///8a51ed3c-9c32-402f-85d1-617ce46845b4). Depois de concluir o laboratório de teste, tire um instantâneo do funcionamento do DirectAccess com a configuração de OTP para que você possa retornar a ele mais tarde para testar cenários adicionais.  
  


