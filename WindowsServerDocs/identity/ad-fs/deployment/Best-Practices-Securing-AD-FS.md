---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: "Práticas recomendadas para proteger o AD FS e Proxy de aplicativo Web"
description: "Este documento fornece as práticas recomendadas para o planejamento seguro e a implantação de serviços de Federação do Active Directory (AD FS) e o Proxy de aplicativo Web."
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6026246228b22fdea6001528ab7621a1704f1983
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>Práticas recomendadas para proteger os serviços de Federação do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este documento fornece as práticas recomendadas para o planejamento seguro e a implantação de serviços de Federação do Active Directory (AD FS) e o Proxy de aplicativo Web.  Ele contém informações sobre os comportamentos padrão desses componentes e recomendações para as configurações de segurança adicional de uma organização com requisitos de segurança e casos de uso específicos.

Este documento se aplica ao AD FS e WAP no Windows Server 2012 R2 e Windows Server 2016 (visualização).  Essas recomendações podem ser usadas se a infraestrutura é implantada em uma rede local ativado ou em um ambiente hospedado na nuvem, como Microsoft Azure.

## <a name="standard-deployment-topology"></a>Topologia de implantação padrão
Para implantação em ambientes locais, recomendamos uma topologia de implantação padrão que consiste em um ou mais servidores do AD FS na rede corporativa interna, com um ou mais servidores Proxy de aplicativos Web (WAP) em uma rede extranet ou DMZ.  Em cada camada, AD FS e WAP, um balanceador de carga de hardware ou software é colocado na frente do farm de servidores e manipula o roteamento de tráfego.  Firewalls são colocados conforme necessários na frente do endereço IP externo do balanceador de carga na frente de cada farm (FS e proxy).

![Topologia do AD FS padrão](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>Portas necessárias
O diagrama abaixo mostra as portas de firewall que devem ser habilitadas entre e entre os componentes da implantação do AD FS e WAP.  Se a implantação não incluem o Azure AD / Office 365, os requisitos de sincronização pode ser ignorado.

>Observe que a porta 49443 só é necessário se autenticação de certificado de usuário é usada, o que é opcional para Azure AD e o Office 365.

![Topologia do AD FS padrão](media/Best-Practices-Securing-AD-FS/adfssec2.png)

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect e servidores de Federação/WAP
Esta tabela descreve as portas e os protocolos que são necessários para a comunicação entre o servidor do Azure AD Connect e os servidores de Federação/WAP.  

Protocolo |Portas |Descrição
--------- | --------- |---------
HTTP|80 (TCP/UDP)|Usado para baixar CRLs (listas de certificados revogados) para verificar os certificados SSL.
HTTPS|443(TCP/UDP)|Usado para sincronizar com o Azure AD.
WinRM|5985| Ouvinte WinRM

### <a name="wap-and-federation-servers"></a>WAP e servidores de Federação
Esta tabela descreve as portas e os protocolos que são necessários para a comunicação entre os servidores de Federação e WAP.

Protocolo |Portas |Descrição
--------- | --------- |---------
HTTPS|443(TCP/UDP)|Usado para autenticação.

### <a name="wap-and-users"></a>Usuários e WAP
Esta tabela descreve as portas e os protocolos que são necessários para a comunicação entre os usuários e os servidores WAP.

Protocolo |Portas |Descrição
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|Usado para autenticação de dispositivo.
TCP|49443 (TCP)|Usado para autenticação de certificado.

Para obter informações adicionais sobre portas necessárias e os protocolos necessários para implantações híbridas, consulte o documento [aqui](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-ports/).

Para obter informações detalhadas sobre portas e os protocolos necessários para um Azure AD e implantação do Office 365, consulte o documento [aqui](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Pontos de extremidade habilitados

Quando o AD FS e WAP são instalados, um conjunto de pontos de extremidade do AD FS padrão estão habilitados no serviço de Federação e no proxy.  Esses padrões foram escolhidos com base em cenários mais comumente necessários e usados e não é necessário alterá-las.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[Opcional] Conjunto mínimo de proxy de pontos de extremidade habilitado para Azure AD / Office 365
Organizações que estejam implantando o AD FS e WAP apenas para Azure AD e Office 365 cenários podem limitar ainda mais o número de pontos de extremidade do AD FS habilitado no proxy para alcançar uma superfície de ataque mais mínima.
Abaixo está a lista de pontos de extremidade que deve ser ativada no proxy nestes cenários:

|Ponto de extremidade|Finalidade
|-----|-----
|/ adfs/ls|Fluxos de autenticação baseada em navegador e nas versões atuais do Microsoft Office usarem esse ponto de extremidade para Azure AD e a autenticação do Office 365
|/ADFS/Services/Trust/2005/usernamemixed|Usado para o Exchange Online com clientes do Office mais antigos do que a atualização do Office 2013 maio de 2015.  Os clientes posteriores usam o ponto de extremidade \adfs\ls passivo.
|/ADFS/Services/Trust/13/usernamemixed|Usado para o Exchange Online com clientes do Office mais antigos do que a atualização do Office 2013 maio de 2015.  Os clientes posteriores usam o ponto de extremidade \adfs\ls passivo.
|/ adfs/oauth2|Este é usado para todos os aplicativos modernos (no local ou na nuvem) que você tiver configurado para autenticar diretamente para o AD FS (ou seja, não por meio do AAD)
|/ADFS/Services/Trust/MEX|Usado para o Exchange Online com clientes do Office mais antigos do que a atualização do Office 2013 maio de 2015.  Os clientes posteriores usam o ponto de extremidade \adfs\ls passivo.
|/ADFS/ls/federationmetadata/2007-06/federationmetadata.XML |Requisito para qualquer passivo aleatoriamente. e ser usados pelo Office 365 / Azure AD para verificar certificados do AD FS


Pontos de extremidade do AD FS podem ser desabilitados no proxy usando o seguinte cmdlet do PowerShell:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Por exemplo:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Proteção estendida para autenticação
Proteção estendida para autenticação é um recurso que reduz contra man ataques de interceptação (MITM) e é habilitado por padrão com o AD FS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para verificar as configurações, você pode fazer o seguinte:
A configuração pode ser verificada usando o abaixo cmdlet do PowerShell.  
    
   `PS:\>Get-ADFSProperties`

A propriedade é `ExtendedProtectionTokenCheck`.  A configuração padrão é permitir, para que os benefícios de segurança podem ser obtidos sem os problemas de compatibilidade com navegadores que não dão suporte a funcionalidade.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Controle de congestionamento para proteger o serviço de Federação
O proxy de serviço de Federação (parte do WAP) fornece controle de congestionamento para proteger o serviço do AD FS contra uma inundação de solicitações.  O Proxy de aplicativo Web rejeitarão solicitações de autenticação de cliente externo, se o servidor de Federação está sobrecarregado detectado pela latência entre o Proxy de aplicativo Web e o servidor de Federação.  Esse recurso está configurado por padrão com um nível de limite de latência recomendados.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Para verificar as configurações, você pode fazer o seguinte:
1.  Em seu computador de Proxy de aplicativo Web, inicie uma janela de comando com privilégios elevados.
2.  Navegue até o diretório do AD FS, em % WINDIR%\adfs\config.
3.  Alterar as configurações de controle de congestionamento de seus valores padrão para '<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />'.
4.  Salve e feche o arquivo.
5.  Reinicie o serviço do AD FS executando 'net stop adfssrv' e 'net start adfssrv'.
Para referência, orientações sobre esse recurso podem ser encontrada [aqui](https://msdn.microsoft.com/en-us/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Verificações de solicitação HTTP padrão do proxy
O proxy também executa as seguintes verificações padrão contra todo o tráfego:

- FS-P se autentica no AD FS por meio de um certificado de tempo de vida curto.  Em um cenário de suspeita de comprometimento de servidores dmz, o AD FS pode "revogar confiança proxy" para que ele não confia qualquer solicitações de entrada potencialmente comprometida proxies. Revogar a relação de confiança do proxy revoga cada certificado do proxy para que ele não conseguirem autenticar com êxito para qualquer finalidade para o servidor do AD FS
- FS-P encerra todas as conexões e cria uma nova conexão HTTP para o serviço do AD FS na rede interna. Isso proporciona um buffer de nível de sessão entre dispositivos externos e o serviço do AD FS. O dispositivo externo nunca se conecta diretamente para o serviço do AD FS.
- FS-P realiza a validação de solicitação HTTP que filtra especificamente os cabeçalhos HTTP que não são exigidos pelo serviço AD FS.

## <a name="recommended-security-configurations"></a>Configurações de segurança recomendadas
Garantir que todos os AD FS e servidores WAP recebem as atualizações mais recentes, que a recomendação de segurança mais importante para sua infraestrutura do AD FS é garantir que você tenha um significa que disponibilizamos para manter seus servidores AD FS e WAP atual com todas as atualizações de segurança, bem como as atualizações opcionais especificadas como importante do AD FS nesta página.

A maneira recomendada para clientes do Azure AD monitorar e implementem sua infraestrutura é por meio do Azure AD Connect integridade do AD FS, um recurso do Azure AD Premium.  Azure AD Connect Health inclui monitores e alertas que disparam se um computador do AD FS ou WAP estiver faltando uma das atualizações importantes especificamente para o AD FS e WAP.

Informações sobre como instalar o Azure AD Connect integridade para o AD FS podem ser encontrado [aqui](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configurações de segurança adicionais
Os seguintes recursos adicionais podem ser configurados opcionalmente para fornecer proteção adicional para aqueles oferecidos na implantação padrão.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Proteção de bloqueio "virtuais" extranet para contas
Com o recurso de bloqueio extranet no Windows Server 2012 R2, o administrador do AD FS pode definir um número máximo permitido de solicitações de autenticação com falha (ExtranetLockoutThreshold) e uma "janela de Observação período de tempo (ExtranetObservationWindow). Quando esse número máximo (ExtranetLockoutThreshold) de solicitações de autenticação é alcançado, o AD FS para tentar autenticar as credenciais da conta fornecido com o AD FS para o período de tempo definido (ExtranetObservationWindow). Essa ação impede essa conta um bloqueio de conta AD, em outras palavras, ele protege essa conta contra perda de acesso a recursos corporativos que dependem do AD FS para autenticação do usuário. Essas configurações se aplicam a todos os domínios que possa autenticar o serviço do AD FS.

Você pode usar o seguinte comando do Windows PowerShell para definir o bloqueio de extranet AD FS (exemplo): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Para referência, a documentação pública desse recurso é [aqui](https://technet.microsoft.com/en-us/library/dn486806.aspx ). 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Diferenciar as políticas de acesso de intranet e extranet acesso
AD FS tem a capacidade de diferenciar as políticas de acesso para as solicitações que são originadas em solicitações de vs o local de rede corporativa que vêm em da internet via proxy.  Isso pode ser feito por aplicativo ou globalmente.  Para aplicativos de valor de negócios alto ou aplicativos com informações confidenciais ou de identificação, considere a possibilidade de exigir autenticação de fator multi.  Isso pode ser feito por meio do snap-in Gerenciamento de AD FS.  

### <a name="require-multi-factor-authentication-mfa"></a>Exigir autenticação de fator Multi (MFA)
AD FS podem ser configurado para exigir autenticação forte (por exemplo, autenticação de fator multi) especificamente para solicitações por meio do proxy, para aplicativos individuais e de acesso condicional para Azure AD / Office 365 e nos recursos locais.  Métodos com suporte de MFA incluem provedores de MFA do Microsoft Azure e de terceiros.  O usuário é solicitado a fornecer informações adicionais (por exemplo, um texto SMS que contém um código de tempo), e do AD FS funciona com o plug-in para permitir o acesso de específico de provedor.  

Provedores MFA externos com suporte incluem aqueles listados nas [isso](https://technet.microsoft.com/en-us/library/dn758113.aspx) página, bem como HDI Global.

### <a name="hardware-security-module-hsm"></a>Módulo de segurança de hardware (HSM)
Na sua configuração padrão, os usos de chaves do AD FS entre tokens nunca deixe os servidores de federação na intranet.  Eles nunca estão presentes no DMZ ou nas máquinas proxy.  Opcionalmente, para fornecer proteção adicional, essas chaves podem ser protegidos em um módulo de segurança de hardware anexado ao AD FS.  Microsoft não produz um produto HSM, porém, existem várias no mercado que dão suporte a AD FS.  Para implementar essa recomendação, siga as orientações do fornecedor para criar o X509 certificados de assinatura e criptografia, em seguida, use o commandlets do powershell de instalação do AD FS, especificando os certificados personalizados da seguinte maneira:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

onde:


- `CertificateThumbprint` é o certificado SSL
- `SigningCertificateThumbprint` é o certificado de assinatura (com chaves de HSM protegido)
- `DecryptionCertificateThumbprint` é o certificado de criptografia (com chaves de HSM protegido)



