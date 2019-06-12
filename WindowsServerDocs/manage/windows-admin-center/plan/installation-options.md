---
title: Que tipo de instalação é ideal para você
description: Este tópico descreve as opções de instalação diferente para o Windows Admin Center, incluindo a instalação em um computador Windows 10 ou um servidor do Windows para uso por vários administradores.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 9b26ce28d8b3f74c26adab87e68b9985f2be5361
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811820"
---
# <a name="what-type-of-installation-is-right-for-you"></a>Que tipo de instalação é ideal para você?

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Este tópico descreve as opções de instalação diferente para o Windows Admin Center, incluindo a instalação em um computador Windows 10 ou um servidor do Windows para uso por vários administradores. Para instalar o Windows Admin Center em uma VM no Azure, consulte [Deploy Windows Admin Center no Azure](../azure/deploy-wac-in-azure.md).

## <a name="supported-operating-systems-installation"></a>Sistemas operacionais com suporte: Instalação

Você pode **instalar** Windows Admin Center nos seguintes sistemas operacionais Windows:

| **Version**  | **Modo de instalação** |
| -------------| -----------------------|
| Windows 10, versão 1709 ou mais recente | Modo desktop |
| Canal semestral do Windows Server | Modo de gateway |
| Windows Server 2016 | Modo de gateway |
| Windows Server 2019 | Modo de gateway |

**Modo de área de trabalho:** Inicie a partir do Menu Iniciar e conectar-se ao gateway do Windows Admin Center no mesmo computador no qual ele está instalado (ou seja, `https://localhost:6516`)

**Modo de gateway:** Conectar-se ao gateway do Windows Admin Center de um navegador cliente em um computador diferente (ou seja, `https://servername.contoso.com`) 

> [!WARNING]
> Não há suporte para a instalação do Windows Admin Center em um controlador de domínio. [Leia mais sobre a segurança do controlador de domínio práticas recomendadas](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack). 

> [!IMPORTANT]
> Não é possível usar o Internet Explorer para gerenciar o Windows Admin Center - em vez disso, você precisa usar um [navegador com suporte](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
).  Se você estiver instalando o Windows Admin Center no Windows Server, é recomendável gerenciar conectando remotamente com o Windows 10 e borda.  Como alternativa, você pode gerenciar localmente no servidor do Windows se você tiver instalado um navegador com suporte.

## <a name="supported-operating-systems-management"></a>Sistemas operacionais com suporte: Management

Você pode **gerenciar** os seguintes sistemas operacionais de Windows usando o Windows Admin Center:

| Version | Gerencie *nó* por meio de *Gerenciador do servidor* | Gerencie *cluster* por meio de *Gerenciador de Cluster de Failover* | Gerencie *HCI* por meio de *HCI Gerenciador de Cluster* |
| ------------------------- |--------------- | ----- | ------------------------ |
| Windows 10, versão 1709 ou mais recente | Sim (por meio do gerenciamento do computador) | N/D | N/D |
| Canal semestral do Windows Server | Sim | Sim | N/D |
| Windows Server 2019 | Sim | Sim | Sim |
| Windows Server 2016 | Sim | Sim | Sim, com [atualização cumulativa mais recente](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Sim | Sim | N/D |
| Windows Server 2012 R2 | Sim | Sim | N/D |
| Microsoft Hyper-V Server 2012 R2 | Sim | Sim | N/D |
| Windows Server 2012 | Sim | Sim | N/D |
| Windows Server 2008 R2 | Sim, funcionalidade limitada | N/D | N/D |

> [!NOTE]
> Windows Admin Center requer recursos do PowerShell que não estão incluídos no Windows Server 2008 R2, 2012 e 2012 R2. Se você gerenciar esses recursos com Windows Admin Center, você precisará instalar nesses servidores Windows Management Framework (WMF) versão 5.1 ou posterior.
> 
> Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior. 
> 
> Se o WMF não estiver instalado, você poderá [baixar o WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## <a name="deployment-options"></a>Opções de implantação

| ![img](../media/deployment-options/W10.png) | ![img](../media/deployment-options/gateway.png) | ![img](../media/deployment-options/node.png) | ![img](../media/deployment-options/HA.png) |
| --------------------------------------------- | ------------------------------------------------- |----------------------------------------------|-------------------------------------------- |
|                                             |                                                 |                                              |                                            |

| Local do cliente | Servidor de Gateway | Servidor Gerenciado | Cluster de failover |
| --- | --- | --- | --- |
| Instale em um cliente local do Windows 10 que tenha conectividade com servidores gerenciados.  Ótimo para início rápido, teste, ad-hoc ou cenários de pequena escala. |Instalar em um servidor de gateway designado e acessar em qualquer navegador do cliente com conectividade para o servidor de gateway.  Muito bem para cenários de larga escala. | Instale diretamente em um servidor gerenciado para fins de gerenciamento em si ou um cluster no qual ele é um nó de membro.  Muito bem para cenários de distribuição. | Implante em um cluster de failover para habilitar a alta disponibilidade do serviço de gateway. Ótimo para ambientes de produção garantir a resiliência do seu serviço de gerenciamento. |

## <a name="high-availability"></a>Alta disponibilidade

Você pode habilitar a alta disponibilidade do serviço de gateway com a implantação do Windows Admin Center em um modelo ativo-passivo em um cluster de failover. Se um de nós do cluster falhar, Windows Admin Center normalmente fará failover para outro nó, permitindo que você continuar a gerenciar os servidores em seu ambiente perfeitamente.

[Saiba como implantar o Windows Admin Center com alta disponibilidade.](../deploy/high-availability.md)

> [!Tip]
> Pronto para instalar o Windows Admin Center? [Baixar agora](https://aka.ms/windowsadmincenter)
