---
title: Suporte para implantações maiores
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 07d0c4c6-3e92-4969-82b8-105e46ab8d97
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d98ab8b203bc73da4129d63b5a2b7518742a3667
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181642"
---
# <a name="support-for-larger-deployments"></a>Suporte para implantações maiores

>Aplica-se a: Windows Server 2016 Essentials

> [!IMPORTANT]
> Os recursos descritos neste tópico funcionam apenas no Windows Server 2016 com a função de experiência do Essentials habilitada e não com a SKU do Windows Server 2016 Essentials.


O Windows Server Essentials agora dá suporte a implantações maiores com:

- vários domínios
- vários controladores de domínio
- capacidade de especificar um controlador de domínio designado
- suporte para até 500 usuários e 500 dispositivos

## <a name="support-for-multiple-domains"></a>Suporte para vários domínios

O Windows Server 2012 R2 Essentials dá suporte apenas a um domínio por servidor, o que é necessário e o servidor Essentials deve ser a raiz da floresta. Embora um domínio e uma floresta ainda sejam necessários, a função de experiência do Windows Server 2016 Essentials agora pode ser implantada no Windows Server 2016 Standard ou Datacenter para dar suporte a vários domínios.

## <a name="support-for-multiple-domain-controllers"></a>Suporte para vários controladores de domínio

 O Windows Server Essentials 2012 R2 bloqueia todos os serviços que aproveitam Azure Active Directory, como o Office 365, no qual mais de um controlador de domínio é implantado. O motivo é que a sincronização de conta e senha entre os controladores de domínio locais e Azure Active Directory pode levar a credenciais de conta com senhas que estão fora de sincronia. Essa limitação foi removida no Windows Server 2016 Essentials.

## <a name="ability-to-specify-a-designated-domain-controller"></a>Capacidade de especificar um controlador de domínio designado

Agora você pode escolher um controlador de domínio designado que melhorará os tempos de recuperação para Active Directory objetos de domínio, além de coordenar a sincronização de alteração de conta em outros controladores de domínio no domínio.

O controlador de domínio designado padrão será o mesmo servidor que está executando a função de servidor de experiência do Windows Server Essentials. Se esse servidor for um servidor membro, o que significa que ele não é um controlador de domínio, o controlador de domínio padrão designado será determinado automaticamente com base no teste de qual controlador de domínio no domínio tem a menor latência de rede para o servidor que está executando a função de servidor Windows Server Experience. Se desejar alterar manualmente qual servidor é o controlador de domínio designado, você pode fazer isso em **configurações** no painel do **Windows Server Essentials** , conforme mostrado abaixo.

![Uma captura de tela mostrando o painel de controle de configurações em primeiro plano e o painel do Windows Server Essentials em segundo plano. A página controlador de domínio designado do painel de controle configurações está selecionada no momento.](media/larger-deployments-1.PNG)

## <a name="support-for-500-users-and-500-devices"></a>Suporte para 500 usuários e 500 dispositivos
-------------------------------------

O número máximo de usuários e dispositivos com suporte no Windows Server 2012 R2 Essentials é 25 e 50, respectivamente. Com a introdução da função de servidor Windows Server Essentials Experience, esse limite foi aumentado para 100 usuários e 200 dispositivos.

O Windows Server 2016 Essentials dá suporte a 500 usuários e 500 dispositivos. Tornar isso possível é uma atualização dos controles de estrutura de provedor e de lista de objetos para que eles armazenem em cache e processem rapidamente grandes listas de objetos de usuários e dispositivos. Além disso, um recurso de pesquisa e filtro foi adicionado para localizar rapidamente o usuário ou dispositivo que você pode estar procurando (consulte a Figura 5-2). Você encontrará a funcionalidade de pesquisa e filtro no **painel do Windows Server Essentials**, **acesso via Web remotos**e as janelas e Windows Phone armazenar meus aplicativos de **servidor** .

![Uma captura de tela mostrando o uso do recurso de pesquisa do painel do Windows Server Essentials para pesquisar a cadeia de caracteres "d5c". Os resultados dessa pesquisa incluem dois arquivos e pastas e dois usuários.](media/larger-deployments-2.PNG)

Uma captura de tela mostrando o uso do recurso de pesquisa do painel do Windows Server Essentials para pesquisar a cadeia de caracteres "d5c". Os resultados dessa pesquisa incluem dois arquivos e pastas e dois usuários.

> [!NOTE]
> Embora o limite de usuários e dispositivos com suporte tenha aumentado para a função de servidor do Windows Server Essentials, o limite com suporte para o backup do cliente permanece em 75.

<a name="see-also"></a>Confira também
--------
[Introdução ao Windows Server Essentials](get-started.md)