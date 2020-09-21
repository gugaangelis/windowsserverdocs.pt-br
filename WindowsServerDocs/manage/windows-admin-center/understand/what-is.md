---
title: O que é o Windows Admin Center
description: O que é o Windows Admin Center (Projeto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: affbc610484abc5a4e45534a7f75e4f06efc23e9
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766959"
---
# <a name="what-is-windows-admin-center"></a>O que é o Windows Admin Center?

> Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Windows Admin Center é um novo conjunto de ferramentas de gerenciamento com base em navegador, implantado localmente, que permite a você gerenciar os servidores do Windows sem nenhuma dependência da nuvem ou do Azure. O Windows Admin Center oferece controle total sobre todos os aspectos da infraestrutura de servidor e é particularmente útil para gerenciar servidores em redes privadas que não estejam conectadas à Internet.

O Windows Admin Center é a evolução moderna das ferramentas de gerenciamento "nativas", como o Gerenciador de Servidores e o MMC. Ele complementa o System Center, mas não é uma substituição.

![Diagrama do Windows Admin Center trabalhando com outras soluções](../media/wac-complements.png)

## <a name="how-does-windows-admin-center-work"></a>Como funciona o Windows Admin Center?

O Windows Admin Center é executado em um navegador da Web e gerencia o Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Azure Stack HCI e mais por meio do **gateway do Windows Admin Center** instalado no Windows Server ou no Windows 10 ingressado em domínio. O gateway gerencia servidores por meio do PowerShell Remoto e WMI em WinRM. O gateway está incluído no Windows Admin Center em um único pacote .msi leve que você pode [baixar](../overview.md).

O gateway do Windows Admin Center, depois de publicado no DNS e ter acesso concedido através de firewalls corporativos correspondentes, permite a você conectar com segurança e gerenciar os servidores de qualquer lugar com o Microsoft Edge ou o Google Chrome.

![Diagrama da arquitetura do Windows Admin Center](../media/architecture.png)

## <a name="learn-how-windows-admin-center-improves-your-management-environment"></a>Saiba como o Windows Admin Center melhora o ambiente de gerenciamento

### <a name="familiar-functionality"></a>**Funcionalidade familiar**

O Windows Admin Center é a evolução das plataformas de gerenciamento já estabelecidas e conhecidas, tais como o MMC (Console de Gerenciamento Microsoft), criado do zero da maneira como os sistemas são criados e gerenciados hoje em dia. O Windows Admin Center contém muitas das ferramentas conhecidas que você usa atualmente para gerenciar os clientes e servidores do Windows.

### <a name="easy-to-install-and-use"></a>**Fácil de instalar e usar**

[Instale](../deploy/install.md) em um computador Windows 10 e comece a gerenciar em minutos ou instale em um servidor Windows 2016 atuando como um gateway para permitir que toda a organização gerencie computadores a partir do próprio navegador da Web.

### <a name="complements-existing-solutions"></a>**Complementa as soluções existentes**

O Windows Admin Center funciona com soluções, como o gerenciamento e a segurança do System Center e do Azure, acrescentando a seus recursos para executar tarefas de gerenciamento detalhadas e de computador individual.

### <a name="manage-from-anywhere"></a>**Gerencie de qualquer lugar**

Publique o servidor de gateway do Windows Admin Center na Internet pública e, em seguida, você poderá conectar e gerenciar servidores de qualquer lugar, tudo de forma segura.

### <a name="enhanced-security-for-your-management-platform"></a>**Segurança aprimorada para sua plataforma de gerenciamento**

O Windows Admin Center possui vários aprimoramentos para tornar a plataforma de gerenciamento [mais segura](../plan/user-access-options.md). O controle de acesso com base em função permite aos administradores ter acesso aos recursos de gerenciamento. As opções de autenticação de gateway incluem grupos locais, Active Directory com base em domínio local e Azure Active Directory com base em nuvem.  Além disso, a [obtenha insights](../use/logging.md) sobre as ações de gerenciamento executadas no ambiente.

### <a name="azure-integration"></a>**Integração com o Azure**

O Windows Admin Center tem vários pontos de [integração com os serviços do Azure](../azure/index.md), incluindo o Azure Active Directory, o Backup do Azure, o Azure Site Recovery e muito mais.

### <a name="deploy-hyper-converged-and-failover-clusters"></a>**Implantar clusters hiperconvergentes e de failover**

O Windows Admin Center permite a [implantação direta de clusters hiperconvergentes e de failover](../use/deploy-hyperconverged-infrastructure.md) por meio de um assistente fácil de usar.

### <a name="manage-hyper-converged-clusters"></a>**Gerencie clusters hiperconvergentes**

O Windows Admin Center oferece a melhor experiência para [gerenciar clusters hiperconvergentes](../use/manage-hyper-converged.md), incluindo componentes de rede, armazenamento e computação virtualizada.

### <a name="extensibility"></a>**Extensibilidade**

O Windows Admin Center foi criado com a extensibilidade em mente desde o início, permitindo que desenvolvedores da Microsoft e de terceiros construam ferramentas e soluções além das ofertas atuais. A Microsoft oferece um [SDK](../extend/extensibility-overview.md) que permite aos desenvolvedores criar suas próprias ferramentas para o Windows Admin Center.

> [!Tip]
> Pronto para instalar o Windows Admin Center? [Baixar agora](../overview.md)