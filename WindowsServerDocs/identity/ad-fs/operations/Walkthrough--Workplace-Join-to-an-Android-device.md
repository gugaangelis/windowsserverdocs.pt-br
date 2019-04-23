---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: 'Passo a passo: ingresso no local para um dispositivo Android'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cfe26947b6b0de28ea50367f82d52815fff8f323
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873827"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Passo a passo: Ingresso no local para um dispositivo Android

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2


## <a name="join-your-device-with-workplace-join"></a>Una seu dispositivo com Ingresso no Local de Trabalho

> [!NOTE]
> Ingresso no Android exige o serviço de registro de dispositivo do Active Directory do Azure. Para impor políticas de dispositivo condicional no local, a ferramenta de sincronização de diretório (DirSync) deve ser implantada com a opção de write-back de objeto do dispositivo habilitada. No momento, write-back de dispositivo para o Active Directory do Azure Active Directory pode levar até 3 horas. Dessa forma, os usuários precisam esperar por 3 horas acessar a aplicativos da web no local, depois de criar uma conta de trabalho. Para obter mais informações sobre como implantar o registro de dispositivo do Active Directory do Azure, consulte, [do Azure Active Directory dispositivo serviço de visão geral do registro](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Criar uma conta de trabalho que ingressa seu dispositivo com o workplace Join

1.  Você precisará instalar o aplicativo Azure Authenticator em seu dispositivo para criar uma conta de trabalho que ingressa seu dispositivo com ingresso no local. A URL a seguir tem instruções sobre como instalar o aplicativo Microsoft authenticator em seu dispositivo Android e adicionar uma conta corporativa. A conta de trabalho faz com que seu dispositivo Android em um dispositivo confiável e fornece Single Sign-On (SSO) para os aplicativos no dispositivo. Você pode usar o dispositivo confiável para acessar aplicativos web e aplicativos de linha de negócios modernos conforme recomendado pelo seu administrador de TI. Para obter mais informações, consulte [Azure Authenticator para Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Consulte também
[Ingresse no local de trabalho de qualquer dispositivo para SSO e contínuo segundo fator de autenticação em aplicativos da empresa](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[Configurando o acesso condicional no local, usando o serviço de registro de dispositivo do Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


