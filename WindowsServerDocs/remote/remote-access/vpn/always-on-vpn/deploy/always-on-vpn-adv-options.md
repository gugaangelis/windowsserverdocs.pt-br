---
title: Recursos avançados de VPN Always On
description: Além do cenário de implantação fornecido nessa implantação, você pode adicionar outros recursos avançados de VPN para melhorar a segurança e a disponibilidade de sua conexão VPN.
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 5f43d64dc7642ef67da03fec989909bc4f2f14ae
ms.sourcegitcommit: a3c9a7718502de723e8c156288017de465daaf6b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2019
ms.locfileid: "67263032"
---
# <a name="advanced-features-of-always-on-vpn"></a>Recursos avançados de VPN Always On

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Saiba mais sobre a tecnologia VPN Always On](../always-on-vpn-technology-overview.md)
- [**Avançar:** Comece a planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md)

Além de cenários de implantação fornecidos, você pode adicionar outros recursos avançados de VPN para melhorar a segurança e a disponibilidade de sua conexão VPN. Por exemplo, esses componentes podem ajudar a garantir que o cliente de conexão esteja íntegro antes de permitir uma conexão.

## <a name="high-availability"></a>Alta disponibilidade

A seguir estão as opções adicionais para alta disponibilidade.

|Opção  |Descrição  |
|---------|---------|
|Balanceamento de carga e resiliência de servidor     |Em ambientes que exigem alta disponibilidade ou o suporte a grandes números de solicitações, você pode aumentar o desempenho e a resiliência do acesso remoto usando o balanceamento de carga entre vários servidores que estão executando o servidor de diretivas de rede (NPS) e habilitação remota Clusters de servidores de acesso.<p>Documentos relacionados:<ul><li>[Balanceamento de carga do servidor de Proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Implantar o acesso remoto em um cluster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Resiliência local geográfico     |Para geolocalização com base em IP, você pode usar o Gerenciador de tráfego Global com o DNS no Windows Server 2016. Mais robusta geográfica para balanceamento de carga, você pode usar soluções de balanceamento de carga Global de servidor, como o Gerenciador de tráfego do Microsoft Azure.<p>Documentos relacionados:<ul><li>[Visão geral do Gerenciador de tráfego](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Gerenciador de tráfego do Microsoft Azure](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

## <a name="advanced-authentication"></a>Autenticação avançada

A seguir estão as opções adicionais para autenticação.

|Opção  |Descrição  |
|---------|---------|
|Windows Hello para Empresas     |No Windows 10, o Windows Hello para Empresas substitui senhas por autenticação forte de dois fatores em computadores e dispositivos móveis. Essa autenticação consiste em um novo tipo de credencial do usuário que está vinculado a um dispositivo e usa um biométricos ou número de identificação pessoal (PIN).<p>O cliente de VPN do Windows 10 é compatível com o Windows Hello para empresas. Depois que o usuário faz logon com um gesto, a conexão VPN usa o Windows Hello para negócios certificado para autenticação baseada em certificado.<p>Documentos relacionados:<ul><li>[Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Estudo de caso técnico: [Habilitando o acesso remoto com o Windows Hello for Business no Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure Multifactor Authentication (MFA)     |O MFA do Azure tem a nuvem e versões que você pode integrar com o mecanismo de autenticação de VPN do Windows no local.<p>Para obter mais informações sobre como esse mecanismo funciona, consulte [integrar a autenticação RADIUS com o servidor de autenticação multifator do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

## <a name="advanced-vpn-features"></a>Recursos VPN avançadas

A seguir estão as opções adicionais para recursos avançados.

|Opção  |Descrição  |
|---------|---------|
|Filtragem de tráfego     |Se você precisar impor a quais aplicativos VPN que os clientes podem acessar, você pode habilitar filtros de tráfego de VPN.<p>Para obter mais informações, consulte [recursos de segurança VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN disparada por aplicativo     |Você pode configurar perfis VPN para se conectar automaticamente quando começa a certos aplicativos ou os tipos de aplicativos.<p>Para obter mais informações sobre esta e outras opções de disparo, consulte [opções disparada automaticamente o perfil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Acesso condicional do VPN   |Conformidade de dispositivo e acesso condicional pode exigir que os dispositivos gerenciados para atender aos padrões antes que eles possam se conectar à VPN. Um dos recursos avançados para acesso condicional do VPN permite restringir as conexões VPN a apenas aqueles em que o certificado de autenticação de cliente contém o 'da organização de acesso condicional da AAD OID de 1.3.6.1.4.1.311.87'.<p>Para restringir as conexões VPN, você precisa:<ol><li>No servidor NPS, abra o **Network Policy Server** snap-in.</li><li>Expandir **diretivas** > **políticas de rede**.</li><li>Clique com botão direito do **conexões de rede Virtual privada (VPN)** política de rede e selecione **propriedades**.</li><li>Selecione o **configurações** guia.</li><li>Selecione **específicas de fornecedor** e selecione **Add**.</li><li>Selecione o **OID de certificado permitido** opção e, em seguida, selecione **Add**.</li><li>Cole o OID de acesso condicional do AAD da **1.3.6.1.4.1.311.87** como o valor do atributo e, em seguida, selecione **Okey** duas vezes.</li><li>Selecione **feche** e, em seguida **aplicar**.<p>Agora, quando os clientes VPN tentam se conectar usando qualquer certificado que não seja o certificado de nuvem e de curta duração, a conexão falhará.</li></ol>Para obter mais informações sobre o acesso condicional, consulte [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |


---
## <a name="blocking-vpn-clients-that-use-revoked-certificates"></a>Bloqueio de clientes VPN que usam certificados revogados
  
Depois de instalar atualizações, o servidor RRAS pode impor a revogação de certificado para as VPNs que usam o IKEv2 e certificados de máquina para autenticação, como o dispositivo de túnel sempre em VPNs. Isso significa que, para essas VPNs, o servidor RRAS pode negar conexões VPN para clientes que tentam usar um certificado revogado.

**Disponibilidade**

A tabela a seguir lista as datas de lançamento aproximado das correções para cada versão do Windows.

|Versão do sistema operacional |Data de lançamento * |
|---------|---------|
|Windows Server, versão 1903  |Q2, 2019  |
|Windows Server 2019<br />Windows Server, versão 1809  |Q3, 2019  |
|Windows Server, versão 1803  |Q3, 2019  |
|Windows Server, versão 1709  |Q3, 2019  |
|Windows Server 2016, versão 1607  |Q2, 2019  |
  
\* Todas as datas de lançamento estão listadas nos trimestres do calendário. As datas são aproximadas e podem ser alteradas sem aviso prévio.

**Como configurar pré-requisitos** 

1. Instale as atualizações do Windows como eles se tornam disponíveis.
1. Certifique-se de que todos os certificados de servidor RRAS que você usa e do cliente VPN tem entradas de CPD e que o servidor RRAS pode alcançar os respectivos CRLs.
1. No servidor RRAS, use o **Set-VpnAuthProtocol** cmdlet do PowerShell para configurar a **RootCertificateNameToAccept** parâmetro.<br /><br />
   O exemplo a seguir lista os comandos para fazer isso. No exemplo, **CN = autoridade de certificação de raiz de Contoso** representa o nome diferenciado da autoridade de certificação raiz. 
   ``` powershell
   $cert1 = ( Get-ChildItem -Path cert:LocalMachine\root | Where-Object -FilterScript { $_.Subject -Like "*CN=Contoso Root Certification Authority,*" } )
   Set-VpnAuthProtocol -RootCertificateNameToAccept $cert1 -PassThru
   ```
**Como configurar o servidor RRAS para impor a revogação de certificados para conexões VPN com base em certificados do computador IKEv2**

1. Em uma janela de Prompt de comando, execute o seguinte comando: 
   ```
   reg add HKLM\SYSTEM\CurrentControlSet\Services\RemoteAccess\Parameters\Ikev2 /f /v CertAuthFlags /t REG_DWORD /d "4"
   ```

1. Reinicie o **roteamento e acesso remoto** service.
  
Para desabilitar a revogação de certificado para essas conexões VPN, defina **CertAuthFlags = 2** ou remova o **CertAuthFlags** valor e, em seguida, reinicie o **roteamento e acesso remoto**service. 

**Como revogar um certificado de cliente VPN para uma conexão VPN com base em um certificado do computador IKEv2**
1. Revogar o certificado do cliente VPN da autoridade de certificação.
1. Publica uma nova CRL da autoridade de certificação.
1. No servidor RRAS, abra uma janela de Prompt de comando administrativa e, em seguida, execute os seguintes comandos:
   ```
   certutil -urlcache * delete
   certutil -setreg chain\ChainCacheResyncFiletime @now
   ```

**Como verificar que a revogação de certificado para conexões de VPN baseada em certificado do IKEv2 máquina está funcionando**  
>[!Note]  
> Antes de usar este procedimento, certifique-se de que você habilite o log de eventos operacional CAPI2.
1. Siga as etapas anteriores para revogar um certificado de cliente VPN.
1. Tente se conectar à VPN usando um cliente que tem o certificado revogado. O servidor RRAS deve se recusar a conexão e exibir uma mensagem, como "credenciais de autenticação IKE são inaceitáveis."
1. No servidor RRAS, abra o Visualizador de eventos e navegue até **aplicativos e serviços Logs/Microsoft/Windows/CAPI2**. 
1. Procure um evento que tem as seguintes informações:
   * Nome do Log: **Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational**
   * ID de Evento: **41** 
   * O evento contém o seguinte texto: **assunto = "*FQDN do cliente*"** (*FQDN do cliente* representa o nome de domínio totalmente qualificado do cliente que tem o revogado certificado). 

   O **<Result>** campo dos dados do evento deve incluir **o certificado é revogado**. Por exemplo, consulte os seguintes trechos de um evento:
   ```xml
   Log Name:      Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational  
   Source:        Microsoft-Windows-CAPI2  
   Date:          5/20/2019 1:33:24 PM  
   Event ID:      41  
   ...  
   Event Xml:
   <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
    <UserData>  
     <CertVerifyRevocation>  
      <Certificate fileRef="C97AE73E9823E8179903E81107E089497C77A720.cer" subjectName="client01.corp.contoso.com" />  
      <IssuerCertificate fileRef="34B1AE2BD868FE4F8BFDCA96E47C87C12BC01E3A.cer" subjectName="Contoso Root Certification Authority" />
      ...
      <Result value="80092010">The certificate is revoked.</Result>
     </CertVerifyRevocation>
    </UserData>
   </Event>
   ```

---
## <a name="additional-protection"></a>Proteção adicional

### <a name="trusted-platform-module-tpm-key-attestation"></a>Atestado de chaves do Trusted Platform Module (TPM)

Um certificado de usuário com uma chave de Atestado por TPM oferece maior garantia de segurança, backup não exportabilidade, insistindo em anti e isolamento de chaves fornecidas pelo TPM.

Para obter mais informações sobre o atestado de chaves do TPM no Windows 10, consulte [atestado de chaves do TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## <a name="next-step"></a>Próximas etapas

[Comece a planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md): Antes de instalar a função de servidor de acesso remoto no computador que você estiver planejando usar como um servidor VPN, execute as seguintes tarefas. Após o planejamento adequado, você pode implantar a VPN Always On e, opcionalmente, configure o acesso condicional para conectividade VPN usando o Azure AD.  

## <a name="related-topics"></a>Tópicos relacionados
- [Balanceamento de carga do servidor de Proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): Clientes remotos autenticação Dial-In do serviço RADIUS (User), que são servidores de acesso de rede, como servidores de rede virtual privada (VPN) e pontos de acesso sem fio, criar solicitações de conexão e os enviam aos servidores RADIUS, como NPS. Em alguns casos, um servidor NPS pode receber muitas solicitações de conexão de uma vez, resultando em desempenho degradado ou uma sobrecarga.

- [Visão geral do Gerenciador de tráfego](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): Este tópico fornece uma visão geral do Azure Traffic Manager, que lhe permite controlar a distribuição do tráfego do usuário para pontos de extremidade de serviço. O Gerenciador de tráfego usa o sistema de nome de domínio (DNS) para direcionar solicitações de cliente para o ponto de extremidade mais apropriado com base em um método de roteamento de tráfego e a integridade dos pontos de extremidade. 

- [Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): Este tópico fornece os pré-requisitos, como implantações somente em nuvem e implantações híbridas.  Este tópico também lista as perguntas frequentes sobre o Windows Hello para empresas.

- [Estudo de caso técnico: Habilitando o acesso remoto com o Windows Hello for Business no Windows 10](https://msdn.microsoft.com/library/mt728163.aspx): Este estudo de caso técnico você aprenderá como a Microsoft implementa o acesso remoto com o Windows Hello para empresas.  Windows Hello para empresas é uma chave privada/pública ou uma abordagem de autenticação baseada em certificado para empresas e consumidores que vai além de senhas. Essa forma de autenticação se baseia nas credenciais de par de chaves que podem substituir senhas e são resistentes a violações, roubos e phishing. 

- [Integrar a autenticação RADIUS com o servidor de autenticação multifator do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): Este tópico orienta você por meio de adicionar e configurar uma autenticação de cliente RADIUS com o servidor de autenticação multifator do Azure. RADIUS é um protocolo padrão para aceitar solicitações de autenticação e processar essas solicitações. O servidor de autenticação multifator do Azure pode atuar como um servidor RADIUS. 

- [Recursos de segurança VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): Este tópico fornece diretrizes de segurança VPN para VPN LockDown, integração de proteção de informações do Windows (WIP) com VPN e filtros de tráfego. 

- [Opções do VPN disparada automaticamente perfil](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): Este tópico fornece opções de disparada automaticamente o perfil VPN, como o gatilho de aplicativo, com base no nome do gatilho e Always On.

- [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Este tópico fornece uma visão geral da plataforma de acesso condicional baseado em nuvem para fornecer uma opção de conformidade do dispositivo para clientes remotos. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD). 

- [Atestado de chaves do TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): Este tópico fornece uma visão geral do Trusted Platform Module (TPM) e as etapas para implantar o atestado de chaves do TPM. Você também pode encontrar informações de solução de problemas e etapas para resolver problemas.
