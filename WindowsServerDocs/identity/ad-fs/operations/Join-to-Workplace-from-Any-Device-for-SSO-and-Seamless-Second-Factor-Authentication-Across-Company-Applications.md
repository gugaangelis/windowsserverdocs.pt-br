---
ms.assetid: e22d84a5-113d-4bec-b484-036ed29f0c28
title: Junte-se a empresa de qualquer dispositivo para SSO e perfeito segundo fator de autenticação em todos os aplicativos da empresa
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4926eb32a0bbffb092ec02ca2508fe97d52d1466
ms.sourcegitcommit: 46439194e5deb0fa5f338b428f95dd6b5b799337
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2018
---
# <a name="join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications"></a>Junte-se a empresa de qualquer dispositivo para SSO e perfeito segundo fator de autenticação em todos os aplicativos da empresa

>Aplica-se a: Windows Server 2012 R2

O rápido aumento no número de dispositivos para consumidores e acesso às informações onipresentes está mudando a maneira como as pessoas percebem sua tecnologia. O uso constante de tecnologia da informação ao longo do dia, juntamente com acesso fácil de informações, é desfoque tradicionais limites entre trabalho e vida doméstica. Esses limites de deslocamento são acompanhados por uma crença esse pessoal selecionado de tecnologia e personalizado para caber personalidades dos usuários, atividades e agendas-se estenda na área de trabalho. Para acomodar o requisito de cada vez maior de dispositivos de consumidor pessoal se conectem a redes corporativas, estamos apresentando as propostas de valor a seguir:

-   Os administradores podem controlar quem tem acesso aos recursos da empresa que se baseiam no aplicativo, o usuário, o dispositivo e o local.

-   Os funcionários podem acessar aplicativos e dados em qualquer lugar, em qualquer dispositivo. Os funcionários podem usar o logon único em aplicativos de navegador ou aplicativos corporativos.

## <a name="key-concepts-introduced-in-the-solution"></a>Principais conceitos introduzidos na solução

### <a name="workplace-join"></a>Ingresso no local de trabalho
Operadores de informações usando o ingresso no local de trabalho, poderá participar de seus dispositivos pessoais com computadores de local de trabalho da sua empresa para acessar serviços e recursos da empresa. Quando você ingressar o dispositivo pessoal à área de trabalho, ele se torna um dispositivo conhecido e fornece perfeito segundo fator de autenticação e logon único para recursos de área de trabalho e aplicativos. Quando um dispositivo é associado ao Workplace Join, os atributos do dispositivo podem ser recuperados do diretório para acesso condicional de unidade para fins de autorizar emissão de tokens de segurança para aplicativos. Windows 8.1 e dispositivos iOS 6.0 + e Android 4.0 + podem ser adicionados usando Workplace Join.

### <a name="BKMK_DRS"></a>Serviço de registro de dispositivo do Active Directory do Azure
Ingresso no local de trabalho se torna possível pelo serviço de registro de dispositivo do Azure Active Directory. Quando um dispositivo é associado ao Workplace Join, o serviço fornece um objeto de dispositivo no Azure Active Directory e define uma chave no dispositivo local que é usado para representar a identidade do dispositivo. Essa identidade do dispositivo, em seguida, pode ser usada com as regras de controle de acesso para aplicativos que são hospedados na nuvem e locais.

Para obter mais informações sobre como habilitar o serviço de registro de dispositivo do Azure Active Directory, consulte [Azure Active Directory dispositivo registro visão geral do serviço](https://msdn.microsoft.com/6a14cb1f-a058-4453-8ede-d9f4a66a7073.aspx).

### <a name="workplace-join-as-a-seamless-second-factor-authentication"></a>Ingresso no local de trabalho como um segundo perfeito autenticação multifator
As empresas podem gerenciar o risco de que está relacionado ao acesso às informações e controle de unidade e conformidade ao conceder o consumidor dispositivos acesso a recursos corporativos. Ingresso no local de trabalho em dispositivos fornece os seguintes recursos para os administradores:

-   Identifica dispositivos conhecidos com a autenticação de dispositivo. Os administradores podem usar essas informações para orientar o acesso condicional e controlar o acesso aos recursos.

-   Fornece uma experiência de entrada mais tranquila para os usuários acessem recursos da empresa de dispositivos confiáveis.

### <a name="single-sign-on"></a>Logon único
Único logon (SSO) no contexto desse cenário é a funcionalidade que reduz o número de solicitações de senha que o usuário final tem de inserir para acessar os recursos de empresa de dispositivos conhecidos. Essa funcionalidade implica em que os usuários são avisados apenas uma vez durante a vida útil de SSO para acessar aplicativos da empresa e recursos deste dispositivo. Se um dispositivo usa Workplace Join, o usuário que foi registrado para usar este dispositivo chegar SSO persistente, por padrão para sete dias. Esse usuário tem uma entrada na experiência perfeita na mesma sessão ou em novas sessões.

## <a name="solution-overview"></a>Visão geral da solução
Como parte dessa solução, você aprenderá a usar Workplace Join em um dispositivo com suporte e a experiência de logon único para um recurso de empresa.

> [!NOTE]
> Para oferecer suporte ao Windows 8.1, dispositivos iOS 6.0 + e Android 4.0 +, você deve configurar registro de dispositivo do Azure Active Directory juntamente com o objeto de dispositivo gravação-voltar, consulte [guia passo a passo para acesso condicional no local usando o serviço de registro de dispositivo do Active Directory do Azure](https://msdn.microsoft.com/library/azure/dn788908.aspx)

Esta solução orienta leva você com as seguintes etapas do guia passo a passo:

1.  [Passo a passo: Associação de local de trabalho com um dispositivo Windows](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)

2.  [Passo a passo: Workplace Join com um dispositivo iOS](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

3.  [Passo a passo: Associação de local de trabalho com um dispositivo Android](../../ad-fs/operations/walkthrough--workplace-join-to-an-android-device.md)

## <a name="see-also"></a>Consulte também
[Configurar um servidor de federação com o serviço de registro de dispositivo](../deployment/configure-a-federation-server-with-device-registration-service.md)



