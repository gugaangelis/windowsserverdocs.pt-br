---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: Passo a passo - ingresso no local de trabalho para um dispositivo Android
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cfe26947b6b0de28ea50367f82d52815fff8f323
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Passo a passo: Workplace Join para um dispositivo Android

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2


## <a name="join-your-device-with-workplace-join"></a>Ingressar o dispositivo com o ingresso no local de trabalho

> [!NOTE]
> Ingresso no local de trabalho Android requer o serviço de registro de dispositivo do Active Directory do Azure. Para impor políticas de dispositivo condicional no local, a ferramenta de sincronização de diretório (DirSync) deve ser implantada com a opção de voltar de gravação de objeto de dispositivo habilitada. No momento, dispositivo gravação-voltar, ao Active Directory do Azure Active Directory pode levar até 3 horas. Dessa forma, os usuários devem aguardar para 3 horas acessar aplicativos da web de local, depois de criar uma conta corporativa. Para obter mais informações sobre a implantação do registro de dispositivo do Azure Active Directory de serviço, consulte [Azure Active Directory registro serviço visão geral do dispositivo](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Criar uma conta de trabalho que ingressa em seu dispositivo com workplace Join

1.  Você precisará instalar o aplicativo Azure Authenticator no seu dispositivo para criar uma conta de trabalho que associa seu dispositivo com o ingresso no local de trabalho. A seguinte URL tem instruções sobre como instalar o aplicativo Azure authenticator no seu dispositivo Android e adicionar uma conta corporativa. A conta corporativa torna seu dispositivo Android em um dispositivo confiável e fornece Single Sign-On (SSO) para os aplicativos no dispositivo. Você pode usar o dispositivo confiável para acessar aplicativos da web e aplicativos de linha de negócios modernos recomendados pelo seu administrador de TI. Para obter mais informações, consulte [Azure Authenticator para Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Consulte também
[Junte-se a empresa de qualquer dispositivo para SSO e perfeita segundo fator de autenticação em todos os aplicativos da empresa](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)<ph x="2">
</ph>[Configurando o acesso condicional no local usando o serviço de registro de dispositivo do Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


