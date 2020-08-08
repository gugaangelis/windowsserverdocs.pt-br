---
title: Obter e configurar certificados de assinatura de token e de descriptografia de token para AD FS
description: Este documento descreve como obter e configurar os certificados TS e TD para AD FS
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.openlocfilehash: a96b256fbd2f1a5ce3db71bd11de8715eccf60e9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966903"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Obter e configurar certificados TS e TD para AD FS

Este tópico descreve as tarefas e os procedimentos que você pode executar para garantir que seus AD FS certificados de assinatura de token e de descriptografia de token estejam atualizados.

Certificados de autenticação de tokens são certificados X509 padrão que são usados para assinar com segurança todos os tokens que o servidor de federação emite. Certificados de descriptografia de token são certificados X509 padrão usados para descriptografar todos os tokens de entrada. Eles também são publicados em metadados de Federação.

Para obter informações adicionais, consulte [requisitos de certificado](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Determinar se AD FS renova os certificados automaticamente
Por padrão, o AD FS está configurado para gerar os certificados de autenticação e descriptografia de tokens automaticamente durante a configuração inicial e quando os certificados estiverem próximos do vencimento.

Você pode executar o seguinte comando do Windows PowerShell: `Get-AdfsProperties` .

  ![Obter-ADFSproperties](media/configure-TS-TD-certs-ad-fs/ts1.png)

A propriedade AutoCertificateRollover descreve se AD FS está configurado para renovar a assinatura de token e certificados de descriptografia de token automaticamente.

Se AutoCertificateRollover for definido como TRUE, os certificados de AD FS serão renovados e configurados em AD FS automaticamente. Depois que o novo certificado for configurado, para evitar uma interrupção, você deve garantir que cada parceiro de Federação (representado em seu farm de AD FS por meio de confianças de terceira parte confiável ou de confiança do provedor de declarações) seja atualizado com esse novo certificado.

Se AD FS não estiver configurado para renovar a assinatura de token e descriptografar certificados automaticamente (se AutoCertificateRollover for definido como false), AD FS não irá gerar automaticamente ou começar a usar a assinatura de token ou certificados de descriptografia de token. Você precisará executar essas tarefas manualmente.

Se AD FS estiver configurado para renovar certificados de assinatura de token e de descriptografia de token automaticamente (AutoCertificateRollover estiver definido como TRUE), você poderá determinar quando eles serão renovados:

CertificateGenerationThreshold descreve quantos dias antes do certificado não após a data em que um novo certificado será gerado.

CertificatePromotionThreshold determina quantos dias após a geração do novo certificado que ele será promovido para ser o certificado principal (em outras palavras, AD FS começará a usá-lo para assinar tokens que emite e descriptografar tokens de provedores de identidade).

![Obter-ADFSproperties](media/configure-TS-TD-certs-ad-fs/ts2.png)

Se AD FS estiver configurado para renovar certificados de assinatura de token e de descriptografia de token automaticamente (AutoCertificateRollover estiver definido como TRUE), você poderá determinar quando eles serão renovados:

 - **CertificateGenerationThreshold** descreve quantos dias antes do certificado não após a data em que um novo certificado será gerado.
 - **CertificatePromotionThreshold** determina quantos dias após a geração do novo certificado que ele será promovido para ser o certificado principal (em outras palavras, AD FS começará a usá-lo para assinar tokens que emite e descriptografar tokens de provedores de identidade).

## <a name="determine-when-the-current-certificates-expire"></a>Determinar quando os certificados atuais expiram
Você pode usar o procedimento a seguir para identificar os certificados de assinatura de token primário e de descriptografia de token e para determinar quando os certificados atuais expiram.

Você pode executar o seguinte comando do Windows PowerShell: `Get-AdfsCertificate –CertificateType token-signing` (ou `Get-AdfsCertificate –CertificateType token-decrypting ` ). Ou você pode examinar os certificados atuais no MMC: certificados de >de serviço.

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

O certificado para o qual o valor **IsPrimary** está definido como **true** é o certificado que AD FS está usando no momento.

A data mostrada para **não após** é a data na qual um novo certificado primário de assinatura ou descriptografia de token deve ser configurado.

Para garantir a continuidade do serviço, todos os parceiros de Federação (representados em seu farm de AD FS por meio de relações de confiança de terceira parte confiável ou de provedor de declarações) devem consumir os novos certificados de assinatura de token e de descriptografia de token antes dessa expiração. Recomendamos que você comece a planejar esse processo pelo menos 60 dias de antecedência.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Gerando um novo certificado autoassinado manualmente antes do final do período de carência
Você pode usar as etapas a seguir para gerar um novo certificado autoassinado manualmente antes do fim do período de carência.

1. Verifique se você está conectado ao servidor de AD FS primário.
2. Abra o Windows PowerShell e execute o seguinte comando:`Add-PSSnapin "microsoft.adfs.powershell"`
3. Opcionalmente, você pode verificar os certificados de autenticação atuais em AD FS. Para fazer isso, execute o seguinte comando: `Get-ADFSCertificate –CertificateType token-signing` . Examine a saída do comando para ver as datas não posteriores de quaisquer certificados listados.
4. Para gerar um novo certificado, execute o seguinte comando para renovar e atualizar os certificados no servidor de AD FS: `Update-ADFSCertificate –CertificateType token-signing` .
5. Verifique a atualização executando o seguinte comando novamente:`Get-ADFSCertificate –CertificateType token-signing`
6. Dois certificados devem ser listados agora, um dos quais não tem uma data **posterior** de aproximadamente um ano no futuro e para o qual o valor **IsPrimary** é **false**.

>[!IMPORTANT]
>Para evitar uma interrupção do serviço, atualize as informações do certificado no Azure AD executando as etapas em como atualizar o Azure AD com um certificado de assinatura de token válido.

## <a name="if-youre-not-using-self-signed-certificates"></a>Se você não estiver usando certificados autoassinados...
Se você não estiver usando o padrão gerado automaticamente, assinatura de token autoassinado e certificados de descriptografia de token, será necessário renovar e configurar esses certificados manualmente.

Primeiro, você deve obter um novo certificado de sua autoridade de certificação e importá-lo para o repositório de certificados pessoais do computador local em cada servidor de Federação. Para obter instruções, consulte o artigo [importar um certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754489(v=ws.11)) .

Em seguida, você deve configurar esse certificado como o certificado de assinatura ou descriptografia de token de AD FS secundário. (Você o configura como um certificado secundário para permitir que seus parceiros de Federação tenham tempo suficiente para consumir esse novo certificado antes de promovê-lo para o certificado primário).

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Para configurar um novo certificado como um certificado secundário
1. Abra o PowerShell e execute o seguinte:`Set-ADFSProperties -AutoCertificateRollover $false`
2. Depois de importar o certificado. Abra o console de **Gerenciamento do AD FS** .
3. Expanda **Serviço** e, em seguida, selecione **Certificados**.
4. No painel Ações, clique em **Adicionar certificado de autenticação de tokens**.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Selecione o novo certificado a partir da lista de certificados mostrados e, em seguida, clique em OK.
6.  Abra o PowerShell e execute o seguinte:`Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Verifique se o novo certificado tem uma chave privada associada a ele e se a conta de serviço de AD FS recebeu permissões de leitura para a chave privada. Verifique isso em cada servidor de Federação. Para isso, no snap-in Certificados, clique duas vezes no novo certificado, clique Todas as Tarefas e, em seguida, clique em Gerenciar Chaves Privadas.

Depois de ter permitido tempo suficiente para que seus parceiros de Federação consumam seu novo certificado (eles recebem seus metadados de Federação ou enviam a chave pública do novo certificado), você deve promover o certificado secundário para o certificado primário.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Para promover o novo certificado de secundário para primário

1. Abra o console de **Gerenciamento do AD FS** .
2. Expanda **Serviço** e, em seguida, selecione **Certificados**.
3. Clique no certificado de autenticação de token secundário.
4. No painel **Ações**, clique em **Configurar como Primário**. Clique Sim no prompt de confirmação.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>Atualizando parceiros de Federação

### <a name="partners-who-can-consume-federation-metadata"></a>Parceiros que podem consumir metadados de Federação
Se você tiver renovado e configurado um novo certificado de assinatura de token ou de descriptografia de token, certifique-se de que todos os seus parceiros de Federação (parceiros de recursos ou da organização de conta que são representados no seu AD FS por confianças de terceira parte confiável e confianças de provedor de declarações) tenham selecionado os novos certificados.

### <a name="partners-who-can-not-consume-federation-metadata"></a>Parceiros que não podem consumir metadados de Federação
Se seus parceiros de Federação não puderem consumir seus metadados de Federação, você deverá enviá-los manualmente para a chave pública do novo certificado de assinatura de token/descriptografia de token. Envie sua nova chave pública de certificado (arquivo. cer ou. p7b se desejar incluir toda a cadeia) em todos os parceiros de organização de recursos ou de organizações de conta (representados em sua AD FS por relações de confiança de terceira parte confiável e provedor de declarações). Os parceiros implementam alterações em seu lado para confiar nos novos certificados.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Promover para primário (se AutoCertificateRollover for false)
Se **AutoCertificateRollover** for definido como **false**, AD FS não gerará automaticamente ou começará a usar novos certificados de assinatura de token ou de descriptografia de token. Você precisará executar essas tarefas manualmente.
Depois de permitir um período de tempo suficiente para que todos os seus parceiros de Federação consumam o novo certificado secundário, promova esse certificado secundário para primário (no snap-in do MMC, clique no certificado de autenticação de tokens secundário e, no painel Ações, clique em **definir como primário**.)

## <a name="updating-azure-ad"></a>Atualizando o Azure AD
AD FS fornece acesso de logon único aos serviços de nuvem da Microsoft, como o Office 365, autenticando usuários por meio de suas credenciais de AD DS existentes.  Para obter informações adicionais sobre como usar certificados, consulte [renovar certificados de Federação para o Office 365 e o Azure ad](/azure/active-directory/connect/active-directory-aadconnect-o365-certs).
