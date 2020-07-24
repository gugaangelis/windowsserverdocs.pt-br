---
ms.assetid: bdb9ad4b-139c-4031-8f26-827432779829
title: Guias de soluções e cenário
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 02f0bb99b4948810c153bf1109ba4f04151b3030
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954188"
---
# <a name="solutions-and-scenario-guides"></a>Guias de soluções e cenário

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
 
  
Com as soluções de proteção de informações e acesso da Microsoft, você pode implantar e configurar o acesso a recursos corporativos em seu ambiente local e aplicativos de nuvem. E você pode fazer isso enquanto protege as informações corporativas.  
  
Proteção de informações e acesso  
  
|Guia|Como este guia pode ajudá-lo                                                                                                                                                                                                                                                                                                                                                                                                    
|-----|-----  
| [Acesso seguro aos recursos corporativos de qualquer local e em qualquer dispositivo](/previous-versions/windows/it-pro/solutions-guidance/dn550982(v=ws.11))|Este guia mostra como permitir que os funcionários usem dispositivos pessoais e da empresa para acessar dados e aplicativos corporativos com segurança.                                                                                                                                                                                    
| [Ingresso no Local de Trabalho em qualquer dispositivo de SSO e autenticação de dois fatores contínua em aplicativos da empresa](../ad-fs/operations/join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications.md) | Os funcionários podem acessar aplicativos e dados de qualquer lugar, em qualquer dispositivo. Os funcionários podem usar Logon Único em aplicativos de navegador ou aplicativos empresariais. Os administradores podem controlar quem tem acesso a recursos da empresa com base no aplicativo, usuário, dispositivo e local.                                        
| [Gerenciar riscos com Autenticação Multifator adicional para aplicativos confidenciais](../ad-fs/operations/manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications.md)| Nesse cenário, você habilita a MFA com base nos dados de associação de grupo do usuário para um aplicativo específico. Em outras palavras, você definirá uma política de autenticação no servidor de federação para exigir MFA quando os usuários que pertencem a um determinado grupo solicitarem acesso a um aplicativo específico que está hospedado em um servidor Web.  
| [Gerenciar risco com o Controle de Acesso Condicional](../ad-fs/operations/manage-risk-with-conditional-access-control.md) | O controle de acesso no AD FS é implementado com regras de declaração de autorização de emissão que são usadas para emitir uma permissão ou negar declarações que determinarão se um usuário ou grupo de usuários terá permissão para acessar recursos protegidos por AD FS ou não. As regras de autorização só podem ser definidas em objetos de confiança da terceira parte confiável.
|[Configurar o Serviço Web de Registro de Certificado para renovação baseada em chave de certificados em uma porta personalizada](certificate-enrollment-certificate-key-based-renewal.md)|Este artigo fornece instruções passo a passo para implementar o Serviço Web de Registro de Certificado (ou o CEP (política de registro de certificado)/CES (serviço de registro de certificado) em uma porta personalizada diferente de 443 para a renovação baseada em chave de certificado para aproveitar o recurso de renovação automática do CEP e do CES. |
