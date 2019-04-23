---
title: Suporte para implantações maiores
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860477"
---
#<a name="support-for-larger-deployments"></a>Suporte para implantações maiores

>Aplica-se a: Windows Server 2016 Essentials

> [!IMPORTANT]  
> Os recursos descritos neste tópico só funcionam no Windows Server 2016 com a função experiência do Essentials habilitada e não com o Windows Server 2016 Essentials SKU.


Agora, o Windows Server Essentials oferece suporte a implantações maiores com:

- vários domínios
- vários controladores de domínio
- capacidade de especificar um controlador de domínio designado
- suporte para até 500 usuários e dispositivos de 500

##<a name="support-for-multiple-domains"></a>Suporte para vários domínios

Servidor Windows 2012 R2 Essentials oferece suporte a apenas um domínio por servidor, que é necessário, e o servidor do Essentials deve ser a raiz da floresta. Enquanto um domínio e floresta ainda são necessárias, a função experiência do Windows Server 2016 Essentials agora pode ser implantada no Windows Server 2016 Standard ou Datacenter, para dar suporte a vários domínios.

## <a name="support-for-multiple-domain-controllers"></a>Suporte para vários controladores de domínio

 Windows Server Essentials 2012 R2 bloqueia todos os serviços que utilizam o Azure Active Directory, como o Office 365, onde mais de um controlador de domínio é implantado. O motivo é que a sincronização de conta e senha entre os controladores de domínio local e o Azure Active Directory pode levar a credenciais de conta com senhas que estão fora de sincronia. Essa limitação foi removida no Windows Server 2016 Essentials.

##<a name="ability-to-specify-a-designated-domain-controller"></a>Capacidade de especificar um controlador de domínio designado

Agora você pode escolher um controlador de domínio designado que irá melhorar os tempos de recuperação de objetos de domínio do Active Directory, bem como coordenar a sincronização de alteração da conta em outros controladores de domínio no domínio.

Seu padrão designado do controlador de domínio será o mesmo servidor que está executando a função de servidor experiência do Windows Server Essentials. Se esse servidor for um servidor membro, o que significa que não é um controlador de domínio e, em seguida, o padrão de controlador de domínio designada será determinado automaticamente com base nos testes qual controlador de domínio no domínio tem a menor latência de rede para o servidor que executa o Função de servidor experiência do Windows Server. Se você quiser alterar manualmente a qual servidor é o controlador de domínio designado, você pode fazer isso no **as configurações** na **painel de controle do Windows Server Essentials** conforme mostrado abaixo.

![Uma captura de tela mostrando as configurações do painel de controle em primeiro plano e o painel do Windows Server Essentials em segundo plano. A página de controlador de domínio designado de configurações do painel de controle está selecionada no momento.](media/larger-deployments-1.PNG)

##<a name="support-for-500-users-and-500-devices"></a>Suporte para dispositivos de 500 e 500 usuários
-------------------------------------

O número máximo de dispositivos no Windows Server 2012 R2 Essentials e usuários com suporte é 25 e 50, respectivamente. Com a introdução da função de servidor experiência do Windows Server Essentials, que o limite foi aumentado para 100 usuários e 200 dispositivos.

Windows Server 2016 Essentials oferece suporte a 500 usuários e dispositivos de 500. Fazendo isso possível é uma atualização para a estrutura do provedor e controles de lista de objetos para que eles armazenar em cache e processam rapidamente grandes listas de objetos de usuário e dispositivo. Além disso, um recurso de pesquisa e filtro foi adicionado para localizar rapidamente o usuário ou dispositivo que você pode estar procurando (consulte a Figura 5-2). Você encontrará a funcionalidade de pesquisa e filtro na **painel do Windows Server Essentials**, **acesso via Web remoto**e o armazenamento do Windows e Windows Phone **My Server** aplicativos.

![Uma captura de tela mostrando o uso do recurso de pesquisa do painel do Windows Server Essentials para pesquisar a cadeia de caracteres "d5c". Os resultados dessa pesquisa incluem dois arquivos e pastas e dois usuários.](media/larger-deployments-2.PNG)

Uma captura de tela mostrando o uso do recurso de pesquisa do painel do Windows Server Essentials para pesquisar a cadeia de caracteres "d5c". Os resultados dessa pesquisa incluem dois arquivos e pastas e dois usuários.

> [!NOTE]  
> Enquanto o limite de usuários e dispositivos com suporte aumentou para a função de servidor do Windows Server Essentials, o limite com suporte para o cliente permanece de backup em 75.

<a name="see-also"></a>Consulte também
--------
[Introdução ao Windows Server Essentials](get-started.md)