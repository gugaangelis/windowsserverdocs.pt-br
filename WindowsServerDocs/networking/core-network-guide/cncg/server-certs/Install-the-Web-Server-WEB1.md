---
title: Instalar o WEB1 de servidor da Web
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e818eac49719b394a2c73cc125a2e7ba9ea80c82
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-web-server-web1"></a>Instalar o WEB1 de servidor da Web

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

A função Web Server (IIS) no Windows Server 2016 fornece uma plataforma segura, fácil de gerenciar, modular e extensível para hospedar confiavelmente sites, serviços e aplicativos. Com o IIS, você pode compartilhar informações com usuários na Internet, uma intranet ou uma extranet. IIS é uma plataforma web unificado que se integra IIS, ASP.NET, serviços FTP, PHP e Windows Communication Foundation (WCF).  

Quando você implanta certificados de servidor, seu servidor Web fornece um local onde você pode publicar a lista de certificados revogados (CRL) para sua autoridade de certificação (CA). Após a publicação, a CRL é acessível a todos os computadores em sua rede para que eles podem usar essa lista durante o processo de autenticação para verificar se certificados apresentados por outros computadores não são revogados.   

Se um certificado estiver instalado em CRL como revogado, o esforço de autenticação falhará e o computador está protegido de confiar em uma entidade que tenha um certificado que não é mais válido.  

Antes de instalar a função Web Server (IIS), certifique-se de que você configurou o nome do servidor e o endereço IP e ingressar o computador no domínio.  

## <a name="to-install-the-web-server-iis-server-role"></a>Para instalar a função de servidor de Web Server (IIS)  
Para concluir este procedimento, você deve ser um membro do **administradores** grupo.  

>[!NOTE]  
>Para executar este procedimento usando o Windows PowerShell, abra o PowerShell, digite o seguinte comando e pressione ENTER.  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  No Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**. Abre o assistente Adicionar funções e recursos.  
2.  Em **Before You Begin**, clique em **próxima**.  

**Observação**   
O **Before You Begin** página do Assistente de recursos de adição de funções e não é exibida se você já executou a adição de funções e recursos do assistente e você selecionou **ignorar esta página por padrão** nesse momento.  

3.  Sobre o **tipo de instalação** página, clique em **próxima**.  
4.  Sobre o **seleção de servidor** página, clique em **próxima**.  
5.  Sobre o **funções de servidor** página, selecione **Web Server (IIS)**e clique em **próxima**.  
6.  Clique em **próxima** até que você aceitou todos padrão configurações de servidor web e, em seguida, clique em **instalar**.  
7.  Verificar se todas as instalações foram bem-sucedidas e clique em **fechar**.
