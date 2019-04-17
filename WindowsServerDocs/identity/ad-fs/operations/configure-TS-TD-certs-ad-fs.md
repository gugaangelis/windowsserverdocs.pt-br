---
title: Obter e configurar o Token de assinatura e certificados de descriptografia de Token do AD FS
description: Este documento descreve como obter e configurar o TS e TD certificados do AD FS
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0d7f8ba4efb74a377f9f9d748949a934398ebefc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>Obter e configurar TS e TD certificados do AD FS

Este tópico descreve as tarefas e procedimentos que podem ser executadas para garantir que o AD FS token de assinatura e certificados de descriptografia token estejam atualizados.

Token de certificados de assinatura são X509 padrão certificados que são usados para entrar com segurança todos os tokens que o servidor de Federação emite. Certificados de token de descriptografia são X509 padrão certificados que são usados para descriptografar qualquer tokens de entrada. Eles também são publicados nos metadados de Federação.

Para obter informações adicionais, consulte [requisitos de certificado](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>Determinar se o AD FS renova automaticamente os certificados
Por padrão, o AD FS está configurado para gerar o token de assinatura e certificados de descriptografia token automaticamente, o momento de configuração inicial e quando os certificados se aproximando sua data de validade.

Você pode executar o seguinte comando do Windows PowerShell:`Get-AdfsProperties`.
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
A propriedade AutoCertificateRollover descreve se AD FS está configurado para renovar o token de assinatura e o token de descriptografia de certificados automaticamente.

Se AutoCertificateRollover for definido como TRUE, os certificados do AD FS serão renovados e configurados no AD FS automaticamente. Depois que o novo certificado estiver configurado, para evitar uma interrupção, você deve garantir a que cada parceiro de Federação (representado pela terceira relações de confiança de terceiros ou relações de confiança de provedor de declarações no farm AD FS) é atualizado com este novo certificado.
    
Se o AD FS não estiver configurado para renovar o token de assinatura e token descriptografando certificados automaticamente (se AutoCertificateRollover é definido como False), AD FS não automaticamente irá gerar ou começar a usar o novo token de assinatura ou token de descriptografia de certificados. Você precisará executar essas tarefas manualmente.
    
Se o AD FS está configurado para renovar o token de assinatura e o token de descriptografia de certificados automaticamente (AutoCertificateRollover é definido como TRUE), você pode determinar quando serão renovadas:

CertificateGenerationThreshold descreve quantos dias antes que a data do certificado não após um novo certificado será gerado.

CertificatePromotionThreshold determina o número de dias após o novo certificado é gerado que ele será promovido para ser o principal certificado (em outras palavras, o AD FS começará a usá-lo para entre tokens ela emite e descriptografar tokens de provedores de identidade).

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
Se o AD FS está configurado para renovar o token de assinatura e o token de descriptografia de certificados automaticamente (AutoCertificateRollover é definido como TRUE), você pode determinar quando serão renovadas:

 - **CertificateGenerationThreshold** descreve quantos dias antes que a data do certificado não após um novo certificado será gerado.
 - **CertificatePromotionThreshold** determina o número de dias após o novo certificado é gerado que ele será promovido para ser o principal certificado (em outras palavras, o AD FS começará a usá-lo para entre tokens ela emite e descriptografar tokens de identidade provedores).

## <a name="determine-when-the-current-certificates-expire"></a>Determinar quando os certificados atuais expira
Você pode usar o procedimento a seguir para identificar a assinatura de token primário e o token de descriptografia de certificados e determinar quando os certificados atuais expiram.

Você pode executar o seguinte comando do Windows PowerShell: `Get-AdfsCertificate –CertificateType token-signing` (ou `Get-AdfsCertificate –CertificateType token-decrypting `). Ou você pode examinar os certificados atuais no MMC: serviço -> certificados.

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

O certificado para o qual o **IsPrimary** valor for definido como **True** é o certificado que o AD FS está usando atualmente.

A data mostrada para o **não após** é a data em que um novo token primário assinar ou descriptografar o certificado deve ser configurado.

Para garantir a continuidade do serviço, todos os parceiros de Federação (representados pela terceira relações de confiança de terceiros ou relações de confiança de provedor de declarações no farm AD FS) devem consumir o novo token de assinatura e certificados de descriptografia token antes deste vencimento. Recomendamos que você começar a planejar para esse processo pelo menos 60 dias de antecedência.

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>Gerar um novo certificado auto-assinado manualmente antes do fim do período de carência
Você pode usar as etapas a seguir para gerar um novo certificado auto-assinado manualmente antes do fim do período de cortesia.

1. Certifique-se de que você está conectado ao servidor AD FS primário.
2. Abra o Windows PowerShell e execute o seguinte comando: `Add-PSSnapin "microsoft.adfs.powershell"`
3. Opcionalmente, você pode verificar os certificados de assinatura atuais no AD FS. Para fazer isso, execute o seguinte comando:`Get-ADFSCertificate –CertificateType token-signing`. Examinar a saída do comando para ver as datas não depois de todos os seus certificados listados.
4. Para gerar um novo certificado, execute o seguinte comando para renovar e atualizar os certificados no servidor do AD FS:`Update-ADFSCertificate –CertificateType token-signing`.
5. Verifique se a atualização executando o seguinte comando novamente: `Get-ADFSCertificate –CertificateType token-signing`
6. Dois certificados serão anunciados agora, uma delas tem um **não após** data de aproximadamente um ano no futuro e para o qual o **IsPrimary** valor é **False**.

>[!IMPORTANT]
>Para evitar uma queda de serviço, atualize as informações de certificado no Azure AD, executando as etapas em como a atualização do Azure AD com um certificado válido de assinatura de token.

## <a name="if-youre-not-using-self-signed-certificates"></a>Se você não estiver usando certificados auto-assinados...
Se você não esteja usando o padrão gerada automaticamente, autoassinados token de assinatura e certificados de descriptografia token, você deve renovar e configurar esses certificados manualmente.

Primeiro, você deve obter um novo certificado da sua autoridade de certificação e importá-lo para o repositório de certificados pessoais de máquina local em cada servidor de Federação. Para obter instruções, consulte o [importar um certificado](https://technet.microsoft.com/library/cc754489.aspx) artigo.

Em seguida, você deve configurar esse certificado conforme o secundário AD FS token de assinatura ou um certificado de descriptografia. (Você configurá-lo como um certificado secundário para permitir que seus parceiros de Federação tempo suficiente para consumir esse novo certificado antes de você promovê-lo para o certificado principal).

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>Para configurar um novo certificado como um certificado secundário
1. Abra o PowerShell e execute o seguinte: `Set-ADFSProperties -AutoCertificateRollover $false`
2. Uma vez você tiver importado o certificado. Abrir o **AD FS gerenciamento** console.
3. Expanda **serviço** e, em seguida, selecione **certificados**.
4. No painel Ações, clique em **certificado de assinatura de adicionar Token**.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. Selecione o novo certificado da lista de certificados exibidos e, em seguida, clique em Okey.
6.  Abra o PowerShell e execute o seguinte: `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>Certifique-se de que o novo certificado tem uma chave privada associada a ela e que a conta de serviço do AD FS é concedida permissões de leitura para a chave privada. Verifique se isso em cada servidor de Federação. Para fazer isso, no snap-in de certificados, clique com botão direito do novo certificado, clique em todas as tarefas e, em seguida, clique em gerenciar chaves privadas.

Depois que você permitiu que o tempo suficiente para que seus parceiros de federação para consumir o novo certificado (eles puxe seus metadados de Federação ou enviá-los a chave pública do seu novo certificado), você deve promover o certificado secundário ao certificado principal.

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>Para promover o novo certificado de secundárias em relação à principal

1. Abrir o **AD FS gerenciamento** console.
2. Expanda **serviço** e, em seguida, selecione **certificados**.
3. Clique no certificado de assinatura de token secundário.
4. No **ações** painel, clique em **definir como principal**. Clique em Sim no prompt de confirmação.
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>Atualizando os parceiros de Federação

### <a name="partners-who-can-consume-federation-metadata"></a>Parceiros que podem consumir metadados de Federação
Se você tiver renovado e configurar um novo token de assinatura ou um certificado de descriptografia token, você deve certificar-se de que todos os seus parceiros de Federação (organização ou da conta da organização parceiros de recurso que são representados no seu AD FS contando relações de confiança de terceiros e relações de confiança de provedor de declarações) têm captados os novos certificados.

### <a name="partners-who-can-not-consume-federation-metadata"></a>Parceiros que não podem consumir metadados de Federação
Se seus parceiros de Federação não podem consumir seus metadados de federação, você deve manualmente enviar a chave pública do seu novo certificado de assinatura de token / descriptografia de token. Enviar sua nova chave pública do certificado (.cer arquivo ou se você deseja incluir toda a cadeia Policy. p7b) para todos os seus parceiros de organização de organização ou conta do recurso (representado no seu AD FS contando terceiros relações de confiança e declarações do provedor relações de confiança). Ter os parceiros implementar mudanças em seu lado de confiar nos certificados de novo.

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>Promover principal (se AutoCertificateRollover é False)
Se **AutoCertificateRollover** é definido como **False**, AD FS não irá gerar automaticamente ou iniciar usando o novo token de assinatura ou certificados de descriptografia de token. Você precisará executar essas tarefas manualmente.
Depois de permitir que um período de tempo suficiente para todos os seus parceiros de federação para consumir o novo certificado secundário, promover esse certificado secundário para principal (no snap-in do MMC, clique a secundário token de assinatura de certificado e no painel Ações, clique em **Definir como principal**.)

## <a name="updating-azure-ad"></a>Atualizando do Azure AD
AD FS oferece acesso de logon único aos serviços de nuvem da Microsoft como o Office 365, autenticação de usuários por meio de suas credenciais do AD DS existentes.  Para obter informações adicionais sobre como usar certificados, consulte [renovar federação certificados para o Office 365 e o Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-o365-certs).