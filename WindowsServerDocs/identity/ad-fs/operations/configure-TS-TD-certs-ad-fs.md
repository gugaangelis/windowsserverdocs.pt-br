---
title: Obter e configurar certificados de autenticação e descriptografia de Token do AD FS
description: Este documento descreve como obter e configurar o TS e TD certificados para o AD FS
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d16a886c9fb8f88748ffe732a75f0fd6a32c3702
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820207"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Obter e configurar TS e TD certificados do AD FS

Este tópico descreve as tarefas e procedimentos que você pode executar para garantir que a autenticação de tokens do AD FS e os certificados de descriptografia de token são atualizados.

Certificados de assinatura de tokens são X509 padrão que são usados para assinar com segurança todos os tokens que o servidor de Federação emite certificados. Certificados de descriptografia de token são X509 padrão certificados que são usados para descriptografar todos os tokens recebidos. Eles também são publicados nos metadados da federação.

Para obter mais informações, consulte [requisitos de certificado](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Determinar se o AD FS renova os certificados automaticamente
Por padrão, o AD FS está configurado para gerar certificados de autenticação e descriptografia de token automaticamente, durante a configuração inicial e quando os certificados estão se aproximando da data de validade.

Você pode executar o seguinte comando do Windows PowerShell: `Get-AdfsProperties`.
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
A propriedade AutoCertificateRollover descreve se o AD FS está configurado para renovar a assinatura de token e o token de descriptografia certificados automaticamente.

Se AutoCertificateRollover está definido como TRUE, os certificados do AD FS serão renovados e configurados automaticamente no AD FS. Depois que o novo certificado estiver configurado, para evitar uma interrupção, você deve garantir a que cada parceiro de Federação (representado pela terceira parte confiável ou relações de confiança de provedor de declarações no farm do AD FS) está atualizado com esse novo certificado.
    
Se o AD FS não está configurado para renovar a assinatura de token e token descriptografar certificados automaticamente (se AutoCertificateRollover está definido como False), o AD FS será automaticamente gerar ou começar a usar a nova assinatura de token ou certificados de descriptografia de token. Você precisará realizar essas tarefas manualmente.
    
Se o AD FS está configurado para renovar a assinatura de token e a descriptografia certificados automaticamente de token (AutoCertificateRollover está definido como TRUE), você pode determinar quando serão renovadas:

CertificateGenerationThreshold descreve quantos dias de antecedência sobre a data do certificado não após um novo certificado será gerado.

CertificatePromotionThreshold determina o número de dias após o novo certificado é gerado que ele será promovido para ser o certificado primário (em outras palavras, o AD FS será iniciado usando-o para assinar tokens que ele emite e descriptografar tokens de provedores de identidade).

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
Se o AD FS está configurado para renovar a assinatura de token e a descriptografia certificados automaticamente de token (AutoCertificateRollover está definido como TRUE), você pode determinar quando serão renovadas:

 - **CertificateGenerationThreshold** descreve quantos dias de antecedência sobre a data do certificado não após um novo certificado será gerado.
 - **CertificatePromotionThreshold** determina o número de dias após o novo certificado é gerado que ele será promovido para ser o certificado primário (em outras palavras, o AD FS será iniciado usando-o para assinar tokens que ele emite e descriptografar tokens de identidade provedores).

## <a name="determine-when-the-current-certificates-expire"></a>Determinar quando expiram os certificados atuais
Você pode usar o procedimento a seguir para identificar os certificados de descriptografia de token e a autenticação de tokens primário e para determinar quando expiram os certificados atuais.

Você pode executar o seguinte comando do Windows PowerShell: `Get-AdfsCertificate –CertificateType token-signing` (ou `Get-AdfsCertificate –CertificateType token-decrypting `). Ou você pode examinar os certificados atuais no MMC: Serviço -> certificados.

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

O certificado para o qual o **IsPrimary** valor é definido como **verdadeiro** é o certificado que o AD FS está usando no momento.

A data apresentada para o **não após** é a data pelo qual um novo token primário assinar ou descriptografar o certificado deve ser configurado.

Para garantir a continuidade do serviço, todos os parceiros de Federação (representados pela terceira parte confiável ou relações de confiança de provedor de declarações no farm do AD FS) devem consumir os novos certificados de autenticação e descriptografia de token antes do vencimento. É recomendável que você comece a planejar para esse processo, pelo menos, 60 dias de antecedência.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Gerar um novo certificado autoassinado manualmente antes do término do período de avaliação
Você pode usar as etapas a seguir para gerar um novo certificado autoassinado manualmente antes do término do período de cortesia.

1. Certifique-se de que você está conectado ao servidor do AD FS primário.
2. Abra o Windows PowerShell e execute o seguinte comando: `Add-PSSnapin "microsoft.adfs.powershell"`
3. Opcionalmente, você pode verificar os certificados de autenticação atuais no AD FS. Para fazer isso, execute o seguinte comando: `Get-ADFSCertificate –CertificateType token-signing`. Examinar a saída do comando para ver as datas não após todos os certificados listados.
4. Para gerar um novo certificado, execute o seguinte comando para renovar e atualizar os certificados no servidor do AD FS: `Update-ADFSCertificate –CertificateType token-signing`.
5. Verifique se a atualização executando novamente o comando a seguir: `Get-ADFSCertificate –CertificateType token-signing`
6. Dois certificados deverão ser listados agora, um dos quais tem um **não após** data de aproximadamente um ano no futuro e para o qual o **IsPrimary** valor é **False**.

>[!IMPORTANT]
>Para evitar uma interrupção de serviço, atualize as informações do certificado no AD do Azure, executando as etapas em como a atualização do Azure AD com um certificado de autenticação de token válido.

## <a name="if-youre-not-using-self-signed-certificates"></a>Se você não estiver usando certificados autoassinados...
Se você estiver não usando o padrão gerado automaticamente, a assinatura de token autoassinado e certificados de descriptografia de token, você deve renovar e configurar esses certificados manualmente.

Primeiro, você deve obter um novo certificado da autoridade de certificação e importá-lo para o repositório de certificados pessoal do computador local em cada servidor de Federação. Para obter instruções, consulte a [importar um certificado](https://technet.microsoft.com/library/cc754489.aspx) artigo.

Em seguida, você deve configurar este certificado como o secundário do AD FS assinatura de token ou certificado de descriptografia. (Você configurá-lo como um certificado secundário para permitir que seus parceiros de Federação tempo suficiente para consumir este novo certificado antes de promovê-lo para o certificado primário).

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Para configurar um novo certificado como certificado secundário
1. Abra o PowerShell e execute o seguinte: `Set-ADFSProperties -AutoCertificateRollover $false`
2. Depois de importar o certificado. Abra o **gerenciamento do AD FS** console.
3. Expandir **Service** e, em seguida, selecione **certificados**.
4. No painel de ações, clique em **certificado de assinatura de Token adicionar**.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Selecione o novo certificado da lista de certificados exibida e, em seguida, clique em Okey.
6.  Abra o PowerShell e execute o seguinte: `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Verifique se que o novo certificado tem uma chave privada associada a ele e que a conta de serviço do AD FS é concedida permissões de leitura para a chave privada. Verifique isso em cada servidor de Federação. Para fazer isso, no snap-in de certificados, o novo certificado com o botão direito, clique em todas as tarefas e, em seguida, clique em gerenciar chaves privadas.

Depois que você autorizar tempo suficiente para os parceiros de Federação consumir o novo certificado (eles extrair os metadados de Federação ou enviá-los a chave pública do seu novo certificado), você deverá promover o certificado secundário para o certificado primário.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Para promover o novo certificado de secundário para primário

1. Abra o **gerenciamento do AD FS** console.
2. Expandir **Service** e, em seguida, selecione **certificados**.
3. Clique o certificado de assinatura de token secundário.
4. No **ações** painel, clique em **definir como principal**. Clique em Sim no prompt de confirmação.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>Atualizando os parceiros de Federação

### <a name="partners-who-can-consume-federation-metadata"></a>Parceiros que podem consumir os metadados de Federação
Se você tiver renovado e configurar uma nova assinatura de token ou certificado de descriptografia de token, certifique-se de que todos os seus parceiros de Federação (relações de confiança de terceiros da organização ou a conta de organização parceiros de recurso que são representados no AD FS por terceira parte confiável e relações de confiança de provedor de declarações) pegaram os novos certificados.

### <a name="partners-who-can-not-consume-federation-metadata"></a>Parceiros que podem não consomem metadados de Federação
Se seus parceiros de Federação não podem consumir os metadados de federação, você deve manualmente envie a chave pública do seu novo certificado de autenticação de token / descriptografia do token. Enviar seu novo certificado chave pública (arquivo. cer ou. p7b se você quiser incluir toda a cadeia) a todos da sua organização de recursos ou conta de parceiros da organização (representados por objetos de confiança e relações de confiança de provedor de declarações no AD FS). Ter os parceiros de implementar alterações em seu lado para os novos certificados de confiança.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Promover para primário (se AutoCertificateRollover é False)
Se **AutoCertificateRollover** é definido como **falso**, o AD FS não irá gerar automaticamente ou começar a usar novos de assinatura de token ou certificados de descriptografia de token. Você precisará realizar essas tarefas manualmente.
Depois de dar um período de tempo suficiente para todos os seus parceiros de federação para consumir o novo certificado secundário, promova esse certificado secundário para primário (no snap-in do MMC, clique a assinatura de token secundário de certificado e no painel de ações, clique em **Definir como primário**.)

## <a name="updating-azure-ad"></a>Atualizando do Azure AD
O AD FS fornece acesso de logon único para serviços de nuvem da Microsoft como o Office 365 por meio da autenticação de usuários por meio de suas credenciais existentes do AD DS.  Para obter mais informações sobre como usar certificados, consulte [renovar certificados de federação para o Office 365 e o Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).