---
title: "Suporte para implantações de maiores"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c60e5f73c88a225fbd1067992894f9d20da745ad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
#<a name="support-for-larger-deployments"></a>Suporte para implantações de maiores

>Aplica-se a: Windows Server 2016 Essentials

> [!IMPORTANT]  
> Os recursos descritos neste tópico só funcionam no Windows Server 2016 com a função de experiência do Essentials habilitada e não com o Windows Server 2016 Essentials SKU.


Windows Server Essentials agora dá suporte a implementações maiores com:

- Vários domínios
- Vários controladores de domínio
- Capacidade para especificar um controlador de domínio designado
- suporte para até 500 usuários e 500 dispositivos

##<a name="support-for-multiple-domains"></a>Suporte para vários domínios

Windows server 2012 R2 Essentials oferece suporte apenas um domínio por servidor, o que for necessária, e o server Essentials deve ser a raiz da floresta. Enquanto uma floresta e domínio ainda forem necessárias, a função de experiência do Windows Server 2016 Essentials agora pode ser implantada no Windows Server 2016 Standard ou Datacenter para dar suporte a vários domínios.

## <a name="support-for-multiple-domain-controllers"></a>Suporte para vários controladores de domínio

 Windows Server Essentials 2012 R2 bloqueia todos os serviços que aproveitam o Active Directory do Azure, como o Office 365, onde mais de um controlador de domínio é implantado. O motivo é que a sincronização de conta e senha entre os controladores de domínio local e o Active Directory do Azure pode levar a credenciais da conta com senhas que estão fora de sincronia. Essa limitação foi removida no Windows Server 2016 Essentials.

##<a name="ability-to-specify-a-designated-domain-controller"></a>Capacidade para especificar um controlador de domínio designado

Agora você pode escolher um controlador de domínio designado que será melhorem o tempo de recuperação para objetos de domínio do Active Directory, bem como sincronização de alteração de conta de coordenadas em todos os outros controladores de domínio no domínio.

O padrão designado controlador de domínio será o mesmo servidor que está executando a função de servidor de experiência do Windows Server Essentials. Se esse servidor é um servidor membro, significando que não é um controlador de domínio, em seguida, o padrão de controlador de domínio designado será determinado automaticamente com base no teste qual controlador de domínio no domínio tem a mais baixa latência de rede para o servidor executa a função de servidor de experiência do Windows Server. Se você quiser alterar manualmente qual servidor é o controlador de domínio designado, você pode fazer isso no **configurações** no **painel do Windows Server Essentials** conforme mostrado abaixo.

![Uma captura de tela mostrando as configurações do painel de controle em primeiro plano e o painel do Windows Server Essentials em segundo plano. A página de controlador de domínio designado a configurações do painel de controle é selecionada no momento.](media/larger-deployments-1.PNG)

##<a name="support-for-500-users-and-500-devices"></a>Suporte para 500 usuários e 500 dispositivos
-------------------------------------

O número máximo de usuários com suporte e dispositivos no Windows Server 2012 R2 Essentials é 25 e 50, respectivamente. Com a introdução da função de servidor Windows Server Essentials Experience, esse limite foi aumentado para 100 usuários e 200 dispositivos.

Windows Server 2016 Essentials dá suporte a 500 usuários e 500 dispositivos. Fazendo isso possível é uma atualização para a estrutura de provedor e controles de lista de objeto para armazenar em cache e rápida renderização de listas grandes de objeto de usuário e dispositivo. Além disso, um recurso de pesquisa e filtro foi adicionado para encontrar rapidamente o usuário ou o dispositivo que talvez esteja à procura de (veja a Figura 5-2). Você achará a funcionalidade de pesquisa e filtro no **painel do Windows Server Essentials**, **acesso via Web remoto**e a loja do Windows e Windows Phone **meu servidor** aplicativos.

![Uma captura de tela mostrando o uso do recurso pesquisa do painel do Windows Server Essentials para pesquisar a cadeia de caracteres "d5c". Os resultados da pesquisa incluem dois arquivos e pastas e dois usuários.](media/larger-deployments-2.PNG)

Uma captura de tela mostrando o uso do recurso pesquisa do painel do Windows Server Essentials para pesquisar a cadeia de caracteres "d5c". Os resultados da pesquisa incluem dois arquivos e pastas e dois usuários.

> [!NOTE]  
> Enquanto aumentou o limite de usuários e dispositivos com suporte para a função de servidor do Windows Server Essentials, o limite com suporte para permanece de backup do cliente em 75.

<a name="see-also"></a>Consulte também
--------
[Introdução ao Windows Server Essentials](get-started.md)