---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: Walkthrough-Workplace Join a um dispositivo Android
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a52c5b6e808094e1ef31c800e8a724dd0a37f3d3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816079"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Walkthrough: Workplace Join para um dispositivo Android



## <a name="join-your-device-with-workplace-join"></a>Una seu dispositivo com Ingresso no Local de Trabalho

> [!NOTE]
> O ingresso no Android Workplace requer Registro de Dispositivos do Azure Active Directory serviço. Para impor as políticas de dispositivo condicional locais, a ferramenta de sincronização de diretório (DirSync) deve ser implantada com a opção de write-back do objeto de dispositivo habilitada. No momento, o Write-back do dispositivo para Active Directory de Azure Active Directory pode levar até 3 horas. Assim, os usuários devem aguardar três horas para acessar os aplicativos Web locais, depois de criar uma conta corporativa. Para obter mais informações sobre como implantar Registro de Dispositivos do Azure Active Directory serviço, consulte [registro de dispositivos do Azure Active Directory visão geral do serviço](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Criar uma conta de trabalho que ingresse em seu dispositivo com o ingresso no local de trabalho

1.  Você precisará instalar Azure Authenticator aplicativo em seu dispositivo para criar uma conta de trabalho que ingresse em seu dispositivo com o ingresso no local de trabalho. A URL a seguir tem instruções sobre como instalar o aplicativo Azure Authenticator em seu dispositivo Android e adicionar uma conta de trabalho. A conta corporativa torna seu dispositivo Android em um dispositivo confiável e fornece logon único (SSO) para os aplicativos no dispositivo. Você pode usar o dispositivo confiável para acessar aplicativos Web e aplicativos de linha de negócios modernos, conforme recomendado pelo administrador de ti. Para obter mais informações, consulte [Azure Authenticator para Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Consulte também
[Ingresse no local de trabalho de qualquer dispositivo para SSO e autenticação de segundo fator direta entre aplicativos da empresa](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[Configurar o acesso condicional local usando o serviço registro de dispositivos do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


