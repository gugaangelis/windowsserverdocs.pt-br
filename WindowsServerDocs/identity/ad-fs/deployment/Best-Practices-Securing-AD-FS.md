---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: Práticas recomendadas para proteger o AD FS e Proxy de aplicativo Web
description: Este documento fornece as práticas recomendadas para o planejamento de seguro e a implantação de serviços de Federação do Active Directory (AD FS) e o Proxy de aplicativo Web.
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 958bf8455d03ddc04395fafe83e70a49c7659c96
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192437"
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>Práticas recomendadas para proteger os serviços de Federação do Active Directory


Este documento fornece as práticas recomendadas para o planejamento de seguro e a implantação de serviços de Federação do Active Directory (AD FS) e o Proxy de aplicativo Web.  Ele contém informações sobre os comportamentos padrão desses componentes e recomendações para configurações de segurança adicionais para uma organização com os requisitos de segurança e casos de uso específicos.

Este documento se aplica ao AD FS e WAP no Windows Server 2012 R2 e Windows Server 2016 (versão prévia).  Essas recomendações podem ser usadas se a infraestrutura está implantada em uma rede local ou em um ambiente hospedado na nuvem, como o Microsoft Azure.

## <a name="standard-deployment-topology"></a>Topologia de implantação padrão
Para implantação em ambientes locais, é recomendável uma topologia de implantação padrão consiste em um ou mais servidores do AD FS na rede corporativa interna, com um ou mais servidores de Proxy de aplicativo da Web (WAP) em uma rede de Perímetro ou rede extranet.  Em cada camada, o AD FS e WAP, um balanceador de carga de hardware ou software é colocado na frente do farm de servidores e manipula o roteamento de tráfego.  Firewalls são colocados conforme necessários na frente do endereço IP externo do balanceador de carga na frente de cada farm (FS e proxy).

![Topologia do AD FS padrão](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>Portas necessárias
O diagrama abaixo ilustra as portas de firewall devem ser habilitadas, entre e entre os componentes da implantação do AD FS e WAP.  Se a implantação não inclui o Azure AD / Office 365, os requisitos de sincronização pode ser desconsiderado.

>Observe que a porta 49443 só é necessário se autenticação de certificado de usuário é usada, o que é opcional para o Azure AD e o Office 365.

![Topologia do AD FS padrão](media/Best-Practices-Securing-AD-FS/adfssec2.png)

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect e servidores de Federação/WAP
Esta tabela descreve as portas e protocolos que são necessários para a comunicação entre o servidor do Azure AD Connect e servidores de Federação/WAP.  

Protocol |Portas |Descrição
--------- | --------- |---------
HTTP|80 (TCP/UDP)|Usado para baixar as CRLs (listas de certificados revogados) para verificar os certificados SSL.
HTTPS|443(TCP/UDP)|Usado para sincronizar com o Azure AD.
WinRM|5985| Ouvinte do WinRM

### <a name="wap-and-federation-servers"></a>Servidores de Federação e WAP
Esta tabela descreve as portas e protocolos que são necessários para a comunicação entre os servidores de Federação e WAP.

Protocol |Portas |Descrição
--------- | --------- |---------
HTTPS|443(TCP/UDP)|Usado para autenticação.

### <a name="wap-and-users"></a>WAP e usuários
Esta tabela descreve as portas e protocolos que são necessários para a comunicação entre usuários e os servidores WAP.

Protocol |Portas |Descrição
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|Usado para autenticação de dispositivo.
TCP|49443 (TCP)|Usado para autenticação de certificado.

Para obter mais informações sobre portas e protocolos necessários para implantações híbridas, consulte o documento [aqui](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-ports/).

Para obter informações detalhadas sobre portas e protocolos necessários para um Azure AD e a implantação do Office 365, consulte o documento [aqui](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Pontos de extremidade habilitados

Quando o AD FS e WAP estiverem instalado, um conjunto padrão de pontos de extremidade do AD FS estão habilitados no serviço de Federação e o proxy.  Esses padrões foram escolhidos com base em cenários com mais frequência do necessários e usados e não é necessário alterá-los.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[Opcional] Conjunto mínimo de proxy de pontos de extremidade habilitado para o Azure AD / Office 365
As empresas na implantação do AD FS e WAP somente para o Azure AD e cenários do Office 365 podem limitar ainda mais o número de pontos de extremidade do AD FS habilitados no proxy para alcançar uma superfície de ataque mínima mais.
Abaixo está a lista de pontos de extremidade que deve ser habilitado no proxy nestes cenários:

|Ponto de extremidade|Finalidade
|-----|-----
|/adfs/ls|Fluxos de autenticação baseada em navegador e as versões atuais do Microsoft Office usam esse ponto de extremidade do AD do Azure e autenticação do Office 365
|/adfs/services/trust/2005/usernamemixed|Usado para o Exchange Online com clientes do Office mais antigos que a atualização do Office 2013 de maio de 2015.  Clientes de versões posteriores usam o ponto de extremidade \adfs\ls passivo.
|/adfs/services/trust/13/usernamemixed|Usado para o Exchange Online com clientes do Office mais antigos que a atualização do Office 2013 de maio de 2015.  Clientes de versões posteriores usam o ponto de extremidade \adfs\ls passivo.
|/adfs/oauth2|Este é usado para todos os aplicativos modernos (localmente ou na nuvem) que você tiver configurado para autenticar diretamente para o AD FS (ou seja, não por meio do AAD)
|/adfs/services/trust/mex|Usado para o Exchange Online com clientes do Office mais antigos que a atualização do Office 2013 de maio de 2015.  Clientes de versões posteriores usam o ponto de extremidade \adfs\ls passivo.
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |Requisito para quaisquer fluxos passivos; e é usado pelo Office 365 / Azure AD para verificar os certificados do AD FS


Pontos de extremidade do AD FS podem ser desabilitados no proxy usando o seguinte cmdlet do PowerShell:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Por exemplo:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Proteção estendida para autenticação
Proteção estendida para autenticação é um recurso que atenua contra os ataques de interceptadores (MITM) e é habilitado por padrão com o AD FS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para verificar as configurações, você pode fazer o seguinte:
A configuração pode ser verificada usando o abaixo do cmdlet do PowerShell.  
    
   `PS:\>Get-ADFSProperties`

A propriedade é `ExtendedProtectionTokenCheck`.  A configuração padrão é permitir, para que os benefícios de segurança podem ser obtidos sem as preocupações de compatibilidade com navegadores que não suportam o recurso.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Controle de congestionamento para proteger o serviço de Federação
O proxy do serviço de Federação (parte do WAP) fornece controle de congestionamento para proteger o serviço do AD FS contra uma inundação de solicitações.  O Proxy de aplicativo Web rejeitará as solicitações de autenticação de cliente externo, se o servidor de Federação for sobrecarregado como detectado pela latência entre o Proxy de aplicativo Web e o servidor de Federação.  Este recurso é configurado por padrão com um nível de limite de latência recomendada.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para verificar as configurações, você pode fazer o seguinte:
1.  Em seu computador Proxy de aplicativo Web, abra uma janela de comando com privilégios elevados.
2.  Navegue até o diretório do AD FS, em % WINDIR%\adfs\config.
3.  Alterar as configurações de controle de congestionamento de seus valores padrão para '<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />'.
4.  Salve e feche o arquivo.
5.  Reinicie o serviço do AD FS, executando 'net stop adfssrv' e, em seguida, 'net start adfssrv'.
Para referência, orientações sobre esse recurso podem ser encontrada [aqui](https://msdn.microsoft.com/en-us/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Solicitação de HTTP padrão verifica no proxy
O proxy também executa as seguintes verificações padrão em relação a todo o tráfego:

- FS-P em si se autentica no AD FS por meio de um certificado de curta duração.  Em um cenário de suspeita de comprometimento dos servidores de rede de perímetro, o AD FS pode "revogar confiança de proxy" para que ele não confia mais em quaisquer solicitações de entrada potencialmente comprometidos proxies. Revogando a relação de confiança de proxy revoga cada certificado de proxy para que ele não pode se autenticar com êxito para qualquer finalidade, o servidor do AD FS
- FS-P encerra todas as conexões e cria uma nova conexão de HTTP para o serviço AD FS na rede interna. Isso fornece um buffer de nível de sessão entre dispositivos externos e o serviço AD FS. O dispositivo externo nunca se conecta diretamente ao serviço do AD FS.
- FS-P realiza a validação de solicitação HTTP que filtra especificamente os cabeçalhos HTTP que não são exigidos pelo serviço do AD FS.

## <a name="recommended-security-configurations"></a>Configurações de segurança recomendadas
Verifique se que todos os servidores do AD FS e WAP receberem as atualizações mais recentes, que a recomendação de segurança mais importante para sua infraestrutura do AD FS é para garantir que você tenha um meio em vigor para manter seus servidores AD FS e WAP atualizado com todas as atualizações de segurança, bem como esses opcional atualizações especificadas como importantes para o AD FS nesta página.

A maneira recomendada para clientes do AD do Azure monitorar e manter atualizados sua infraestrutura é por meio do Azure AD Connect Health para AD FS, um recurso do Azure AD Premium.  Azure AD Connect Health inclui monitores e alertas que são disparados se uma máquina do AD FS ou WAP não tiver uma das atualizações importantes especificamente para o AD FS e WAP.

Informações sobre como instalar o Azure AD Connect Health para AD FS podem ser encontrado [aqui](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configurações de segurança adicionais
Os seguintes recursos adicionais podem ser configurados opcionalmente para fornecer proteções adicionais para as oferecidas na implantação padrão.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Proteção de bloqueio de extranet de "soft" para contas
Com o recurso de bloqueio de extranet no Windows Server 2012 R2, um administrador do AD FS pode definir um número máximo de solicitações de autenticação com falha (ExtranetLockoutThreshold) e um ' da janela de observação do período de tempo (ExtranetObservationWindow). Quando é atingido o número máximo (ExtranetLockoutThreshold) de solicitações de autenticação, o AD FS para tentar autenticar as credenciais da conta fornecido com o AD FS para o período de tempo definido (ExtranetObservationWindow). Essa ação protege essa conta de um bloqueio de conta do AD, em outras palavras, ela protege essa conta de perder o acesso aos recursos corporativos que se baseiam no AD FS para autenticação do usuário. Essas configurações se aplicam a todos os domínios que o serviço AD FS pode autenticar.

Você pode usar o seguinte comando do Windows PowerShell para definir o bloqueio de extranet do AD FS (exemplo): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Para referência, a documentação pública desse recurso está [aqui](https://technet.microsoft.com/library/dn486806.aspx ). 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Diferenciar as políticas de acesso para acesso extranet e intranet
AD FS tem a capacidade de diferenciar as políticas de acesso para solicitações originadas nas solicitações de rede corporativa local vs recebidas da internet por meio do proxy.  Isso pode ser feito por aplicativo ou globalmente.  Para aplicativos de valor de negócios de alta ou aplicativos com informações sigilosas ou identificáveis pessoalmente, considere a possibilidade de exigir multi-factor authentication.  Isso pode ser feito por meio do snap-in Gerenciamento do AD FS.  

### <a name="require-multi-factor-authentication-mfa"></a>Exigir Multi-factor authentication (MFA)
O AD FS pode ser configurado para exigir autenticação forte (como multi-factor authentication), especificamente para solicitações recebidas por meio do proxy, para aplicativos individuais e para acesso condicional ao Azure AD / Office 365 e recursos locais.  Métodos com suporte do MFA incluem provedores do Microsoft Azure MFA e de terceiros.  O usuário é solicitado a fornecer as informações adicionais (como um texto SMS que contém um código de tempo), e o AD FS funciona com o plug-in para permitir o acesso do provedor específico.  

Provedores MFA externos com suporte incluem aqueles listados na [isso](https://technet.microsoft.com/library/dn758113.aspx) página, bem como HDI Global.

### <a name="hardware-security-module-hsm"></a>Módulo de segurança de hardware (HSM)
Em sua configuração padrão, os usos de chaves do AD FS para assinar tokens nunca deixem os servidores de federação na intranet.  Eles nunca estão presentes na rede de Perímetro ou nos computadores de proxy.  Opcionalmente, para fornecer proteção adicional, essas chaves podem ser protegidas em um módulo de segurança de hardware conectado ao AD FS.  Microsoft não produz um produto HSM, no entanto, há vários no mercado que dão suporte ao AD FS.  Para implementar essa recomendação, siga as orientações de fornecedor para criar o X509 certificados para assinatura e criptografia, em seguida, usar os cmdlets do powershell de instalação do AD FS, especificando seus certificados personalizados da seguinte maneira:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

onde:


- `CertificateThumbprint` é o seu certificado SSL
- `SigningCertificateThumbprint` é o seu certificado de autenticação (com a chave HSM protegido)
- `DecryptionCertificateThumbprint` é o certificado de criptografia (com a chave HSM protegido)



