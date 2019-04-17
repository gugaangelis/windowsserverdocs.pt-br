---
title: Que tipo de instalação é ideal para você
description: Este tópico descreve as opções de instalação diferente para o Windows Admin Center, incluindo a instalação em um computador Windows 10 ou um servidor do Windows para uso por vários administradores.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: cc6f9e6c2f2572618c1c5fdae00047d24fbeb3cf
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296658"
---
# Que tipo de instalação é ideal para você?

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Este tópico descreve as opções de instalação diferente para o Windows Admin Center, incluindo a instalação em um computador Windows 10 ou um servidor do Windows para uso por vários administradores. Para instalar o Windows Admin Center em uma VM no Azure, consulte [Implantar o Windows Admin Center no Azure](../azure/deploy-wac-in-azure.md).

## Sistemas operacionais suportados: instalação

Você pode **instalar o** Windows Admin Center nos seguintes sistemas operacionais Windows:

| **Versão** | **Modo de instalação** |
|-------------|-----------------------|
|Windows 10, versão 1709 ou mais recente | Modo desktop |
|Canal semestral do Windows Server | Modo de gateway |
|Windows Server 2016 | Modo de gateway |
|Windows Server 2019 | Modo de gateway |

**Modo área de trabalho:** Iniciar a partir do Menu Iniciar e conecte-se ao gateway do Windows Admin Center no mesmo computador no qual ele está instalado (ou seja, `https://localhost:6516`)

**Modo de gateway:** Conectar-se ao gateway do Windows Admin Center de um navegador cliente em um computador diferente (ou seja, `https://servername.contoso.com`) 

> [!WARNING]
> Não há suporte para instalar o Windows Admin Center em um controlador de domínio. [Leia mais sobre as práticas recomendadas de segurança de controlador de domínio](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack). 

> [!IMPORTANT]
> Você não pode usar o Internet Explorer para gerenciar o Windows Admin Center - em vez disso, você precisará usar um [navegador de suporte](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
).  Se você estiver instalando o Windows Admin Center no Windows Server, recomendamos gerenciar conectando-se remotamente com o Windows 10 e Edge.  Como alternativa, você pode gerenciar localmente no servidor do Windows se você tiver instalado um navegador compatível.

## Sistemas operacionais suportados: gerenciamento

Você pode **Gerenciar** os seguintes sistemas operacionais de Windows usando o Windows Admin Center:

| Versão | Gerenciar o *nó* por meio do *Gerenciador do servidor* | Gerenciar um *cluster* por meio do *Gerenciador de Cluster de Failover* | Gerenciar *HCI* por meio do *Gerenciador de Cluster HCI*|
|-------------------------|---------------|-----|------------------------|
| Windows 10, versão 1709 ou mais recente | Sim (por meio do gerenciamento do computador) | N/A | N/A |
| Canal semestral do Windows Server | Sim | Sim | N/A |
| Windows Server 2019 | Sim | Sim | Sim |
| Windows Server 2016 | Sim | Sim | Sim, com a [atualização cumulativa mais recente](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Sim | Sim | N/A |
| Windows Server 2012 R2 | Sim | Sim | N/A |
| Microsoft Hyper-V Server 2012 R2 | Sim | Sim | N/A |
| Windows Server 2012 | Sim | Sim | N/A |
| Windows Server 2008 R2 | Sim, funcionalidade limitada | N/A | N/A |

> [!NOTE]
> Windows Admin Center exige recursos do PowerShell que não estão incluídos no Windows Server 2008 R2, 2012 e 2012 R2. Se você deseja gerenciar esses com o Windows Admin Center, você precisará instalar o Windows Management Framework (WMF) versão 5.1 ou posterior nesses servidores.

>Digite `$PSVersiontable` no PowerShell para verificar se o WMF está instalado e se a versão é 5.1 ou superior. 

>Se o WMF não estiver instalado, você pode [baixar o WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## Opções de implantação

| ![img](../media/deployment-options/W10.png) | ![img](../media/deployment-options/gateway.png) | ![img](../media/deployment-options/node.png) | ![img](../media/deployment-options/HA.png) |
|---|---|---|---|

| Cliente local | Servidor de gateway | Servidor gerenciado | Cluster de failover |
| --- | --- | --- | --- |
| Instale em um cliente Windows 10 local que tenha conectividade com os servidores gerenciados.  Excelente para o início rápido, teste, ad hoc ou cenários de pequena escala. |Instalar em um servidor de gateway designado e acesso de qualquer navegador de cliente com conectividade com o servidor de gateway.  Excelente para cenários em grande escala. | Instale diretamente em um servidor gerenciado para fins de gerenciamento autônomo ou um cluster no qual ele é um nó de membro.  Excelente para cenários de distribuição. | Implante em um cluster de failover para habilitar a alta disponibilidade do serviço de gateway. Excelente para ambientes de produção garantir que a resiliência do seu serviço de gerenciamento. |

## Alta disponibilidade

Você pode habilitar a alta disponibilidade do serviço de gateway ao implantar o Windows Admin Center em um modelo ativo-passivo em um cluster de failover. Se um de nós do cluster falhar, o Windows Admin Center normalmente failover para outro nó, permitindo que você continue a gerenciar os servidores no seu ambiente perfeitamente.

[Saiba como implantar o Windows Admin Center com alta disponibilidade.](../deploy/high-availability.md)

> [!Tip]
> Pronto para instalar o Windows Admin Center? [Baixe agora](https://aka.ms/windowsadmincenter)
