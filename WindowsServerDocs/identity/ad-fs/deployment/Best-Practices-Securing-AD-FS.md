---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: Práticas recomendadas para proteger o AD FS e o proxy de aplicativo Web
description: Este documento fornece as práticas recomendadas para o planejamento e a implantação seguros do Serviços de Federação do Active Directory (AD FS) (AD FS) e do proxy de aplicativo Web.
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6939373db678f1ca6be62711f1771b8f7019c312
ms.sourcegitcommit: c9ab5fbde1782a3a2bac2dbd45f3f178f7ae3c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68354639"
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>Práticas recomendadas para proteger Serviços de Federação do Active Directory (AD FS)


Este documento fornece as práticas recomendadas para o planejamento e a implantação seguros do Serviços de Federação do Active Directory (AD FS) (AD FS) e do proxy de aplicativo Web.  Ele contém informações sobre os comportamentos padrão desses componentes e recomendações para configurações de segurança adicionais para uma organização com casos de uso e requisitos de segurança específicos.

Este documento se aplica a AD FS e WAP no Windows Server 2012 R2 e no Windows Server 2016 (versão prévia).  Essas recomendações podem ser usadas se a infraestrutura for implantada em uma rede local ou em um ambiente hospedado na nuvem, como Microsoft Azure.

## <a name="standard-deployment-topology"></a>Topologia de implantação padrão
Para implantação em ambientes locais, recomendamos uma topologia de implantação padrão que consiste em um ou mais servidores de AD FS na rede corporativa interna, com um ou mais servidores de proxy de aplicativo Web (WAP) em uma rede DMZ ou Extranet.  Em cada camada, AD FS e WAP, um balanceador de carga de hardware ou software é colocado na frente do farm de servidores e lida com o roteamento de tráfego.  Os firewalls são colocados conforme necessário na frente do endereço IP externo do balanceador de carga na frente de cada farm (FS e proxy).

![AD FS topologia padrão](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>Portas necessárias
O diagrama abaixo ilustra as portas de firewall que devem ser habilitadas entre e entre os componentes da implantação de AD FS e WAP.  Se a implantação não incluir o Azure AD/Office 365, os requisitos de sincronização poderão ser desconsiderados.

>Observe que a porta 49443 só será necessária se a autenticação de certificado do usuário for usada, o que é opcional para o Azure AD e o Office 365.

![AD FS topologia padrão](media/Best-Practices-Securing-AD-FS/adfssec2.png)

>[!NOTE]
> A porta 808 (Windows Server 2012R2) ou a porta 1501 (Windows Server 2016 +) é a porta Net. TCP que o AD FS usa para o ponto de extremidade do WCF local para transferir dados de configuração para o processo de serviço e o PowerShell. Essa porta pode ser vista executando Get-Adfsproperties | Selecione NetTcpPort. Essa é uma porta local que não precisará ser aberta no firewall, mas será exibida em uma verificação de porta. 

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect e servidores de Federação/WAP
Esta tabela descreve as portas e os protocolos necessários para a comunicação entre o servidor de Azure AD Connect e os servidores de Federação/WAP.  

Protocol |Portas |Descrição
--------- | --------- |---------
HTTP|80 (TCP/UDP)|Usado para baixar CRLs (listas de certificados revogados) para verificar certificados SSL.
HTTPS|443 (TCP/UDP)|Usado para sincronizar com o Azure AD.
WinRM|5985| Ouvinte do WinRM

### <a name="wap-and-federation-servers"></a>Servidores de Federação e WAP
Esta tabela descreve as portas e os protocolos necessários para a comunicação entre os servidores de Federação e os servidores WAP.

Protocol |Portas |Descrição
--------- | --------- |---------
HTTPS|443 (TCP/UDP)|Usado para autenticação.

### <a name="wap-and-users"></a>WAP e usuários
Esta tabela descreve as portas e os protocolos necessários para a comunicação entre os usuários e os servidores WAP.

Protocol |Portas |Descrição
--------- | --------- |--------- |
HTTPS|443 (TCP/UDP)|Usado para autenticação de dispositivo.
TCP|49443 (TCP)|Usado para autenticação de certificado.

Para obter informações adicionais sobre portas e protocolos necessários necessários para implantações híbridas, consulte o documento [aqui](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-ports/).

Para obter informações detalhadas sobre portas e protocolos necessários para uma implantação do Azure AD e do Office 365, consulte o documento [aqui](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Pontos de extremidade habilitados

Quando AD FS e WAP são instalados, um conjunto padrão de pontos de extremidade de AD FS é habilitado no serviço de Federação e no proxy.  Esses padrões foram escolhidos com base nos cenários mais comumente necessários e usados e não é necessário alterá-los.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>Adicional Conjunto mínimo de proxy de pontos de extremidade habilitado para o Azure AD/Office 365
As organizações que implantam AD FS e WAP somente para cenários do Azure AD e do Office 365 podem limitar ainda mais o número de pontos de extremidade de AD FS habilitados no proxy para alcançar uma superfície de ataque mais mínima.
Abaixo está a lista de pontos de extremidade que devem ser habilitados no proxy nestes cenários:

|Ponto de extremidade|Finalidade
|-----|-----
|/adfs/ls|Os fluxos de autenticação baseados em navegador e as versões atuais do Microsoft Office usam esse ponto de extremidade para a autenticação do Azure AD e do Office 365
|/adfs/services/trust/2005/usernamemixed|Usado para o Exchange Online com clientes do Office mais antigos do que o Office 2013 pode ser atualizado em 2015.  Os clientes posteriores usam o ponto de extremidade \adfs\ls passivo.
|/adfs/services/trust/13/usernamemixed|Usado para o Exchange Online com clientes do Office mais antigos do que o Office 2013 pode ser atualizado em 2015.  Os clientes posteriores usam o ponto de extremidade \adfs\ls passivo.
|/adfs/oauth2|Este é usado para todos os aplicativos modernos (no local ou na nuvem) que você configurou para autenticar diretamente no AD FS (ou seja, não por meio do AAD)
|/adfs/services/trust/mex|Usado para o Exchange Online com clientes do Office mais antigos do que o Office 2013 pode ser atualizado em 2015.  Os clientes posteriores usam o ponto de extremidade \adfs\ls passivo.
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |Requisito para qualquer fluxo passivo; e usados pelo Office 365/Azure AD para verificar AD FS certificados


AD FS pontos de extremidade podem ser desabilitados no proxy usando o seguinte cmdlet do PowerShell:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Por exemplo:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Proteção estendida para autenticação
A proteção estendida para autenticação é um recurso que mitiga contra ataques de interceptação (MITM) e é habilitado por padrão com AD FS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para verificar as configurações, você pode fazer o seguinte:
A configuração pode ser verificada usando o commandlet do PowerShell abaixo.  
    
   `PS:\>Get-ADFSProperties`

A propriedade é `ExtendedProtectionTokenCheck`.  A configuração padrão é permitir, para que os benefícios de segurança possam ser obtidos sem as preocupações com a compatibilidade com navegadores que não dão suporte à funcionalidade.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Controle de congestionamento para proteger o serviço de Federação
O proxy do serviço de Federação (parte do WAP) fornece controle de congestionamento para proteger o serviço de AD FS de uma inundação de solicitações.  O proxy de aplicativo Web rejeitará solicitações de autenticação de cliente externo se o servidor de Federação estiver sobrecarregado conforme detectado pela latência entre o proxy de aplicativo Web e o servidor de Federação.  Esse recurso é configurado por padrão com um nível de limite de latência recomendado.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para verificar as configurações, você pode fazer o seguinte:
1.  No computador proxy de aplicativo Web, inicie uma janela de comando com privilégios elevados.
2.  Navegue até o diretório do ADFS, em%WINDIR%\adfs\config.
3.  Altere as configurações de controle de congestionamento de seus valores padrão<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />para ' '.
4.  Salve e feche o arquivo.
5.  Reinicie o serviço AD FS executando ' net stop adfssrv ' e, em seguida, ' net start adfssrv '.
Para sua referência, a orientação sobre esse recurso pode ser encontrada [aqui](https://msdn.microsoft.com/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Verificações de solicitação HTTP padrão no proxy
O proxy também executa as seguintes verificações padrão em todo o tráfego:

- O FS-P em si é autenticado para AD FS por meio de um certificado de vida curta.  Em um cenário de comprometimento suspeito de servidores DMZ, AD FS pode "revogar a confiança de proxy" para que ele não confie mais em nenhuma solicitação de entrada de proxies potencialmente comprometidos. A revogação da confiança de proxy revoga o certificado próprio de cada proxy para que ele não possa ser autenticado com êxito para qualquer finalidade para o servidor de AD FS
- O FS-P encerra todas as conexões e cria uma nova conexão HTTP com o serviço de AD FS na rede interna. Isso fornece um buffer em nível de sessão entre dispositivos externos e o serviço de AD FS. O dispositivo externo nunca se conecta diretamente ao serviço de AD FS.
- O FS-P executa a validação de solicitação HTTP que filtra especificamente os cabeçalhos HTTP que não são exigidos pelo serviço AD FS.

## <a name="recommended-security-configurations"></a>Configurações de segurança recomendadas
Certifique-se de que todos os servidores AD FS e WAP recebam as atualizações mais recentes a recomendação de segurança mais importante para sua infraestrutura de AD FS é garantir que você tenha um meio em vigor para manter seus servidores AD FS e WAP atualizados com todas as atualizações de segurança, bem como aqueles opcionais atualizações especificadas como importantes para AD FS nesta página.

A maneira recomendada para os clientes do Azure AD monitorarem e manterem sua infraestrutura atual é por meio de Azure AD Connect Health para AD FS, um recurso do Azure AD Premium.  Azure AD Connect Health inclui monitores e alertas que disparam se um AD FS ou computador WAP não tem uma das atualizações importantes especificamente para AD FS e WAP.

Informações sobre como instalar Azure AD Connect Health para AD FS podem ser encontradas [aqui](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configurações de segurança adicionais
Os recursos adicionais a seguir podem ser configurados opcionalmente para fornecer proteções adicionais àquelas oferecidas na implantação padrão.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Proteção de bloqueio "flexível" da extranet para contas
Com o recurso de bloqueio de extranet no Windows Server 2012 R2, um administrador de AD FS pode definir um número máximo permitido de solicitações de autenticação com falha (ExtranetLockoutThreshold) e um ' período de tempo da janela de observação (ExtranetObservationWindow). Quando esse número máximo (ExtranetLockoutThreshold) de solicitações de autenticação for atingido, AD FS para tentar autenticar as credenciais de conta fornecidas em AD FS para o período de tempo definido (ExtranetObservationWindow). Essa ação protege essa conta de um bloqueio de conta do AD, em outras palavras, protege essa conta contra a perda de acesso a recursos corporativos que dependem de AD FS para autenticação do usuário. Essas configurações se aplicam a todos os domínios que o serviço de AD FS pode autenticar.

Você pode usar o seguinte comando do Windows PowerShell para definir o bloqueio de extranet AD FS (exemplo): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Para referência, a documentação pública desse recurso está [aqui](https://technet.microsoft.com/library/dn486806.aspx ). 

### <a name="disable-ws-trust-windows-endpoints-on-the-proxy-ie-from-extranet"></a>Desabilite pontos de extremidade do Windows do WS-Trust no proxy, ou seja, da extranet

Os pontos de extremidade do Windows do WS-Trust ( */ADFS/Services/Trust/2005/windowstransport* e */ADFS/Services/Trust/13/windowstransport*) são destinados apenas a pontos de extremidade voltados para a intranet que usam a associação WIA em https. Expô-los à extranet podem permitir solicitações nesses pontos de extremidade para ignorar as proteções de bloqueio. Esses pontos de extremidade devem ser desabilitados no proxy (ou seja, desabilitados da extranet) para proteger o bloqueio de conta do AD usando os comandos do PowerShell a seguir. Não há nenhum impacto conhecido do usuário final ao desabilitar esses pontos de extremidade no proxy.

    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/2005/windowstransport -Proxy $false
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/windowstransport -Proxy $false
    
### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Diferenciar políticas de acesso para acesso à intranet e extranet
AD FS tem a capacidade de diferenciar políticas de acesso para solicitações que se originam na rede corporativa local versus solicitações que chegam da Internet por meio do proxy.  Isso pode ser feito por aplicativo ou globalmente.  Para aplicativos de alto valor de negócios ou aplicativos com informações confidenciais ou de identificação pessoal, considere a necessidade de autenticação multifator.  Isso pode ser feito por meio do snap-in de gerenciamento de AD FS.  

### <a name="require-multi-factor-authentication-mfa"></a>Exigir autenticação multifator (MFA)
Os AD FS podem ser configurados para exigir autenticação forte (como autenticação multifator) especificamente para solicitações recebidas por meio do proxy, para aplicativos individuais e para acesso condicional para o Azure AD/Office 365 e recursos locais.  Os métodos com suporte do MFA incluem Microsoft Azure MFA e provedores de terceiros.  O usuário é solicitado a fornecer as informações adicionais (como um texto SMS que contém um código de uma vez) e AD FS trabalha com o plug-in específico do provedor para permitir o acesso.  

Os provedores de MFA externa com suporte incluem os listados [nesta página,](https://technet.microsoft.com/library/dn758113.aspx) bem como HDI global.

### <a name="hardware-security-module-hsm"></a>Módulo de segurança de hardware (HSM)
Em sua configuração padrão, as chaves que o AD FS usa para assinar tokens nunca deixam os servidores de Federação na intranet.  Eles nunca estão presentes na DMZ ou nas máquinas de proxy.  Opcionalmente, para fornecer proteção adicional, essas chaves podem ser protegidas em um módulo de segurança de hardware anexado a AD FS.  A Microsoft não produz um produto HSM, no entanto, há vários no mercado que dão suporte a AD FS.  Para implementar essa recomendação, siga as diretrizes do fornecedor para criar os certificados X509 para assinatura e criptografia e, em seguida, use o AD FS commandlets de instalação do PowerShell, especificando os certificados personalizados da seguinte maneira:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

em que:


- `CertificateThumbprint`é seu certificado SSL
- `SigningCertificateThumbprint`é seu certificado de autenticação (com chave protegida por HSM)
- `DecryptionCertificateThumbprint`é seu certificado de criptografia (com chave protegida por HSM)



