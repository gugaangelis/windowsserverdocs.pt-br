---
title: O que é o Windows Admin Center
description: O que é o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 99f1a9a32ef69ba8322b2dba902003f8a750a4d2
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811649"
---
# <a name="what-is-windows-admin-center"></a>O que é o Windows Admin Center?

> Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center é um conjunto de ferramentas de gerenciamento novo implantado localmente, baseada em navegador que permite a você gerenciar seus servidores do Windows com nenhuma dependência do Azure ou nuvem. O Windows Admin Center oferece controle total sobre todos os aspectos da sua infraestrutura de servidor e é útil para gerenciar servidores em redes privadas que não estão conectadas à Internet.

O Windows Admin Center é a evolução moderna das ferramentas de gerenciamento "nativas", como o Gerenciador do Servidor e o MMC. Ele complementa o System Center - não é uma substituição.

![](../media/wac-complements.png)

## <a name="how-does-windows-admin-center-work"></a>Como funciona o Windows Admin Center?

Windows Admin Center é executado em um navegador da web e gerencia 2019 do Windows Server, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 10 e muito mais por meio de **gateway do Windows Admin Center** instalado no Windows Server ou Windows 10. O gateway gerencia servidores por meio do PowerShell remoto e WMI em WinRM. O gateway é incluído no Windows Admin Center em um pacote .msi leve único que você pode [baixar](https://aka.ms/windowsadmincenter).

O gateway do Windows Admin Center, depois de publicado no DNS e com acesso através de firewalls corporativos correspondentes, permite que você conecte-se com segurança e gerencie os servidores de qualquer lugar com o Microsoft Edge ou o Google Chrome.

![](../media/architecture.png)

## <a name="learn-how-windows-admin-center-improves-your-management-environment"></a>Saiba como o Windows Admin Center melhora seu ambiente de gerenciamento

### <a name="familiar-functionality"></a>**Funcionalidade familiar**

O Windows Admin Center é a evolução das plataformas de gerenciamento já estabelecidas e conhecidas como o Console de Gerenciamento Microsoft (MMC), construído do zero para a maneira como os sistemas são criados e gerenciados hoje. O Windows Admin Center contém muitas das ferramentas conhecidas que você usa atualmente para gerenciar os servidores do Windows e clientes.

### <a name="easy-to-install-and-use"></a>**Fácil de instalar e usar**

[Instale](../deploy/install.md) em um computador Windows 10 e comece a gerenciar em minutos ou instale em um servidor Windows 2016 atuando como um gateway para permitir que toda a sua organização gerencie computadores do seu navegador da Web.

### <a name="complements-existing-solutions"></a>**Complementa as soluções existentes**

Windows Admin Center funciona com soluções como o gerenciamento do System Center e o Azure e segurança, adicionando a seus recursos para executar detalhado, as tarefas de gerenciamento de computador único.

### <a name="manage-from-anywhere"></a>**Gerenciar de qualquer lugar**

Publique seu servidor de gateway do Windows Admin Center na rede pública e, em seguida, você pode se conectar e gerenciar seus servidores de qualquer lugar, tudo de forma segura.

### <a name="enhanced-security-for-your-management-platform"></a>**Segurança aprimorada para sua plataforma de gerenciamento**

O Windows Admin Center possui vários aprimoramentos para tornar a plataforma de gerenciamento [mais seguro](../plan/user-access-options.md). O controle de acesso com base em função permite que os administradores tenham acesso aos recursos de gerenciamento. As opções de autenticação de gateway incluem grupos locais, Active Directory com base em domínio local e Azure Active Directory com base em nuvem.  Além disso, a [obtenha informações](../use/logging.md) sobre as ações de gerenciamento executadas em seu ambiente.

### <a name="azure-integration"></a>**Integração do Azure**

Windows Admin Center tem muitos pontos de [integração com serviços do Azure](../plan/azure-integration-options.md), incluindo o Azure Active Directory, o Backup do Azure, Azure Site Recovery e muito mais.

### <a name="manage-hyper-converged-clusters"></a>**Gerenciar clusters hiperconvergente**

O Windows Admin Center oferece a melhor experiência para [gerenciar clusters com hiperconvergência](../use/manage-hyper-converged.md), incluindo computação virtualizada, armazenamento e componentes de rede.

### <a name="extensibility"></a>**Extensibilidade**

O Windows Admin Center foi criado com a extensibilidade em mente desde o início, permitindo que desenvolvedores da Microsoft e de terceiros construam ferramentas soluções além das ofertas atuais. A Microsoft oferece um [SDK](../extend/extensibility-overview.md) que permite aos desenvolvedores criar suas próprias ferramentas para o Windows Admin Center.

> [!Tip]
> Pronto para instalar o Windows Admin Center? [Baixar agora](https://aka.ms/windowsadmincenter)
