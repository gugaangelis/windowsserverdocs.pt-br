---
title: Que tipo de instalação é ideal para você?
description: Este tópico descreve as diferentes opções de instalação do Centro de Administração do Windows, incluindo a instalação do em um PC com Windows 10 ou um Windows Server para uso por vários administradores.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 12/02/2019
ms.openlocfilehash: 503cd64cac0673829fe21bc15e8ad9d6a83bbb15
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950511"
---
# <a name="what-type-of-installation-is-right-for-you"></a>Que tipo de instalação é ideal para você?

Este tópico descreve as diferentes opções de instalação do Centro de Administração do Windows, incluindo a instalação do em um PC com Windows 10 ou um Windows Server para uso por vários administradores. Para instalar o Windows Admin Center em uma VM no Azure, confira [Implantar o Windows Admin Center no Azure](../azure/deploy-wac-in-azure.md).

## <a name="installation-types"></a>Instalação: Tipos

![img](../media/deployment-options/install-options.PNG)

| Cliente local                                | Servidor Gateway                                  | Servidor gerenciado                               | Cluster de failover                           |
|---------------------------------------------|-------------------------------------------------|----------------------------------------------|--------------------------------------------|
| [Instale](../deploy/install.md) em um cliente local do Windows 10 que tenha conectividade com os servidores gerenciados.  Ótimo para cenários de início rápido, teste, ad hoc ou de pequena escala. |[Instale](../deploy/install.md) em um servidor de gateway designado e acesse de qualquer navegador cliente com conectividade com o servidor de gateway.  Ótimo para cenários em grande escala. | [Instale](../deploy/install.md) diretamente em um servidor gerenciado para gerenciar a si mesmo ou um cluster no qual ele seja um nó de membro.  Ótimo para cenários distribuídos. | [Implante](#high-availability) em um cluster de failover para habilitar a alta disponibilidade do serviço de gateway. Ótimo para ambientes de produção para garantir a resiliência do seu serviço de gerenciamento. |

## <a name="installation-supported-operating-systems"></a>Instalação: Sistemas operacionais compatíveis

Você pode **instalar** o Windows Admin Center nos seguintes sistemas operacionais Windows:

| **Plataforma**                       | **Modo de instalação** |
| -----------------------------------| --------------------- |
| Windows 10                         | Cliente local |
| Windows Server Canal Semestral | Servidor de gateway, servidor gerenciado, cluster de failover |
| Windows Server 2016                | Servidor de gateway, servidor gerenciado, cluster de failover |
| Windows Server 2019                | Servidor de gateway, servidor gerenciado, cluster de failover |

Para operar o Windows Admin Center:

- **Em cenário de cliente local:** inicie o gateway do centro de administração do Windows no menu Iniciar e conecte-se a ele em um navegador da Web do cliente acessando `https://localhost:6516`.
- **Em outros cenários:** conecte-se ao gateway do Windows Admin Center em um computador diferente de um navegador cliente por meio de sua URL, por exemplo, `https://servername.contoso.com`

> [!WARNING]
> Não há suporte para a instalação do Windows Admin Center em um controlador de domínio. [Leia mais sobre as melhores práticas de segurança do controlador de domínio](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack).

## <a name="installation-supported-web-browsers"></a>Instalação: Navegadores da Web compatíveis

O Microsoft Edge (incluindo o [Microsoft Edge Insider](https://microsoftedgeinsider.com)) e o Google Chrome são testados e compatíveis com o Windows 10. Outros navegadores da Web, incluindo o Internet Explorer e o Firefox, atualmente não fazem parte da matriz de testes, portanto, não são *oficialmente* compatíveis. Esses navegadores podem ter problemas ao executar o Windows Admin Center. Por exemplo, o Firefox tem o próprio repositório de certificados, portanto, você deve importar o certificado do `Windows Admin Center Client` no Firefox para usar o Windows Admin Center no Windows 10. Para obter mais detalhes, confira [problemas conhecidos específicos do navegador](../support/known-issues.md#browser-specific-issues).

## <a name="management-target-supported-operating-systems"></a>Alvo do gerenciamento: Sistemas operacionais compatíveis

Você pode **gerenciar** os seguintes sistemas operacionais Windows usando o Windows Admin Center:

| Versão | Gerencie o *Nó* por meio do *Gerenciador do Servidor* | Gerencie por meio do *Gerenciador de Cluster* |
| ------------------------- |--------------- | ----- |
| Windows 10 | Sim (por meio do Gerenciamento do Computador) | N/D |
| Windows Server Canal Semestral | Sim | Sim |
| Windows Server 2019 | Sim | Sim |
| Windows Server 2016 | Sim | Sim, com a [atualização cumulativa mais recente](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Sim | Sim |
| Windows Server 2012 R2 | Sim | Sim |
| Microsoft Hyper-V Server 2012 R2 | Sim | Sim |
| Windows Server 2012 | Sim | Sim |
| Windows Server 2008 R2 | Sim, funcionalidade limitada | N/D |

> [!NOTE]
> Requer os recursos do PowerShell para Windows Admin Center que não estão incluídos no Windows Server 2008 R2, 2012 e 2012 R2. Se você for gerenciá-los com o Windows Admin Center, precisará instalar o Windows Management Framework (WMF) versão 5.1 ou superior nesses servidores.
> 
> Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior. 
> 
> Se o WMF não estiver instalado, você poderá [baixar o WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

## <a name="high-availability"></a>Alta disponibilidade

Você pode habilitar a alta disponibilidade do serviço de gateway implantando o Windows Admin Center em um modelo ativo-passivo em um cluster de failover. Se um dos nós no cluster falhar, o Windows Admin Center fará failover de maneira suave para outro nó, permitindo que você continue gerenciando os servidores em seu ambiente de maneira contínua.

[Saiba como implantar o Windows Admin Center com alta disponibilidade.](../deploy/high-availability.md)

> [!Tip]
> Pronto para instalar o Windows Admin Center? [Baixar agora](https://aka.ms/windowsadmincenter)
