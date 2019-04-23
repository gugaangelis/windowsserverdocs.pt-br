---
title: Instalar o servidor Web WEB1
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ef4f10a6ac1998850758f2c9db86bfd950c1ad70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833277"
---
# <a name="install-the-web-server-web1"></a>Instalar o servidor Web WEB1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

A função de servidor Web (IIS) no Windows Server 2016 fornece uma plataforma segura, fácil de gerenciar, modular e extensível para a hospedagem confiável de sites, serviços e aplicativos. Com o IIS, você pode compartilhar informações com usuários na Internet, intranet ou extranet. O IIS é uma plataforma web unificada que integra IIS, ASP.NET, serviços FTP, PHP e Windows Communication Foundation (WCF).  

Quando você implantar certificados de servidor, o seu servidor Web fornece um local onde você pode publicar a lista de certificados revogados (CRL) para sua autoridade de certificação (CA). Após a publicação, a CRL é acessível a todos os computadores em sua rede para que eles podem usar essa lista durante o processo de autenticação para verificar que os certificados apresentados pelos outros computadores não são revogados.   

Se for um certificado da CRL como revogado, o esforço de autenticação falhará e o computador estiver protegido de confiar em uma entidade que tem um certificado que não é mais válido.  

Antes de instalar a função de servidor Web (IIS), certifique-se de que você configurou o endereço IP e o nome do servidor e ingressar o computador no domínio.  

## <a name="to-install-the-web-server-iis-server-role"></a>Para instalar a função de servidor Servidor Web (IIS)  
Para concluir este procedimento, é preciso ser um membro do grupo **Administradores**.  

>[!NOTE]  
>Para executar esse procedimento usando o Windows PowerShell, abra o PowerShell, digite o seguinte comando e pressione ENTER.  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  No Gerenciador do Servidor, clique em **Gerenciar**e depois em **Adicionar Funções e Recursos**. O Assistente para Adicionar Funções e Recursos é aberto.  
2.  Em **Antes de Começar**, clique em **Avançar**.  

**Observação**   
O **antes de começar** página do assistente Adicionar funções e recursos não será exibida se você tiver executado o assistente Adicionar funções e recursos anteriormente e você selecionou **pular esta página por padrão** nesse momento.  

3.  Na página **Tipo de Instalação**, clique em **Avançar**.  
4.  Sobre o **seleção de servidor** , clique em **próxima**.  
5.  Sobre o **funções de servidor** página, selecione **servidor Web (IIS)** e, em seguida, clique em **próxima**.  
6.  Clique em **Avançar** até ter aceitado todas as configurações padrão do servidor Web e clique em **Instalar**.  
7.  Confirme se todas as instalações tiveram êxito e clique em **Fechar**.
