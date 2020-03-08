---
title: Recursos avançados de VPN Always On
description: Além do cenário de implantação fornecido nessa implantação, você pode adicionar outros recursos avançados de VPN para melhorar a segurança e a disponibilidade de sua conexão VPN.
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 07/24/2019
ms.author: pashort, v-tea
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 73d64cd143b7bbd13e0eb9bb5fadfbb1e7c65416
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371828"
---
# <a name="advanced-features-of-always-on-vpn"></a>Recursos avançados do Always On VPN

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Saiba mais sobre a tecnologia de VPN Always On](../always-on-vpn-technology-overview.md)
- [**Em seguida:** Começar a planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md)

Além dos cenários de implantação que são fornecidos, você pode adicionar outros recursos avançados de VPN para melhorar a segurança e a disponibilidade de sua conexão VPN. Por exemplo, o servidor VPN pode usar esses recursos para ajudar a garantir que o cliente de conexão esteja íntegro antes de permitir uma conexão.

## <a name="high-availability"></a>Alta disponibilidade

A seguir estão as opções adicionais para alta disponibilidade.

|{1&gt;Opção&lt;1}  |Descrição  |
|---------|---------|
|Resiliência do servidor e balanceamento de carga     |Em ambientes que exigem alta disponibilidade ou que dão suporte a um grande número de solicitações, você pode aumentar o desempenho e a resiliência do acesso remoto usando o balanceamento de carga entre vários servidores que estão executando o NPS (servidor de diretivas de rede) e habilitando Clustering de servidores de acesso remoto.<p>Documentos relacionados:<ul><li>[Balanceamento de carga do servidor proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Implantar o acesso remoto em um cluster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Resiliência do site geográfico     |Para localização geográfica baseada em IP, você pode usar o Gerenciador de tráfego global com DNS no Windows Server 2016. Para um balanceamento de carga geográfico mais robusto, você pode usar soluções de balanceamento de carga de servidor global, como Gerenciador de Tráfego do Microsoft Azure.<p>Documentos relacionados:<ul><li>[Visão geral do Gerenciador de tráfego](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Gerenciador de Tráfego do Microsoft Azure](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

## <a name="advanced-authentication"></a>Autenticação avançada

A seguir estão as opções adicionais para autenticação.

|{1&gt;Opção&lt;1}  |Descrição  |
|---------|---------|
|Windows Hello para Empresas     |No Windows 10, o Windows Hello para empresas substitui senhas ao fornecer autenticação forte de dois fatores em PCs e dispositivos móveis. Essa autenticação consiste em um novo tipo de credencial de usuário que está vinculado a um dispositivo e usa um PIN (número de identificação pessoal) ou biométrico.<p>O cliente de VPN do Windows 10 é compatível com o Windows Hello para empresas. Depois que o usuário fizer logon usando um gesto, a conexão VPN usará o certificado do Windows Hello para empresas para autenticação baseada em certificado.<p>Documentos relacionados:<ul><li>[Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Estudo de caso técnico: [habilitando o acesso remoto com o Windows Hello para empresas no Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|MFA (autenticação multifator do Azure)     |O Azure MFA tem versões de nuvem e locais que você pode integrar com o mecanismo de autenticação de VPN do Windows.<p>Para obter mais informações sobre como esse mecanismo funciona, consulte [integrar a autenticação RADIUS com o Azure servidor de autenticação multifator](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

## <a name="advanced-vpn-features"></a>Recursos avançados de VPN

A seguir estão as opções adicionais para recursos avançados.

|{1&gt;Opção&lt;1}  |Descrição  |
|---------|---------|
|Filtragem de tráfego     |Se você precisar impor a opção de quais aplicativos os clientes VPN podem acessar, você pode Habilitar filtros de tráfego de VPN.<p>Para obter mais informações, consulte [recursos de segurança de VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN disparada por aplicativo     |Você pode configurar perfis VPN para se conectar automaticamente quando determinados aplicativos ou tipos de aplicativos forem iniciados.<p>Para obter mais informações sobre essa e outras opções de disparo, consulte [Opções de perfil de VPN disparadas automaticamente](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Acesso condicional VPN   |O acesso condicional e a conformidade do dispositivo podem exigir que os dispositivos gerenciados atendam aos padrões antes que possam se conectar à VPN. Um dos recursos avançados para acesso condicional VPN permite restringir as conexões VPN somente àquelas nas quais o certificado de autenticação de cliente contém o OID "AAD Conditional Access" de **1.3.6.1.4.1.311.87**.<p>Para restringir as conexões VPN, você precisa fazer o seguinte:<ol><li>No servidor NPS, abra o snap-in **servidor de políticas de rede** .</li><li>Expanda **políticas** > **políticas de rede**.</li><li>Clique com o botão direito do mouse na política de rede **conexões de VPN (rede virtual privada)** e selecione **Propriedades**.</li><li>Selecione a guia **configurações** .</li><li>Selecione **específico do fornecedor**e, em seguida, selecione **Adicionar**.</li><li>Selecione a opção **Allowed-Certificate-OID** e, em seguida, selecione **Adicionar**.</li><li>Cole o OID de acesso condicional do AAD de **1.3.6.1.4.1.311.87** como o valor do atributo e, em seguida, selecione **OK** duas vezes.</li><li>Selecione **fechar**e, em seguida, selecione **aplicar**.<p>Depois de seguir essas etapas, quando os clientes VPN tentarem se conectar usando qualquer certificado que não seja o certificado de nuvem de curta duração, a conexão falhará.</li></ol>Para obter mais informações sobre o acesso condicional, consulte [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |


---
## <a name="blocking-vpn-clients-that-use-revoked-certificates"></a>Bloqueando clientes VPN que usam certificados revogados
  
Depois de instalar as atualizações, o servidor RRAS pode impor a revogação de certificados para VPNs que usam IKEv2 e certificados de máquina para autenticação, como VPNs Always on do túnel de dispositivo. Isso significa que para essas VPNs, o servidor RRAS pode negar conexões VPN a clientes que tentam usar um certificado revogado.

**Disponibilidade**

A tabela a seguir lista as versões que contêm as correções para cada versão do Windows.

|Versão do sistema operacional |Versão  |
|---------|---------|
|Windows Server, versão 1903  |[KB4501375](https://support.microsoft.com/help/4501375/windows-10-update-kb4501375) |
|Windows Server 2019<br />Windows Server, versão 1809  |[KB4505658](https://support.microsoft.com/help/4505658/windows-10-update-kb4505658)  |
|Windows Server, versão 1803  |[KB4507466](https://support.microsoft.com/help/4507466/windows-10-update-kb4507466)  |
|Windows Server, versão 1709  |[KB4507465](https://support.microsoft.com/help/4507465/windows-10-update-kb4507465)  |
|Windows Server 2016, versão 1607  |[KB4503294](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) |

**Como configurar os pré-requisitos** 

1. Instale as atualizações do Windows à medida que elas se tornarem disponíveis.
1. Verifique se todos os certificados do cliente VPN e do servidor RRAS que você usa têm entradas de CDP e se o servidor RRAS pode acessar as respectivas CRLs.
1. No servidor RRAS, use o cmdlet **set-VpnAuthProtocol** do PowerShell para configurar o parâmetro **RootCertificateNameToAccept** .<br /><br />
   O exemplo a seguir lista os comandos para fazer isso. No exemplo, **CN = Contoso Root Certification Authority** representa o nome distinto da autoridade de certificação raiz. 
   ``` powershell
   $cert1 = ( Get-ChildItem -Path cert:LocalMachine\root | Where-Object -FilterScript { $_.Subject -Like "*CN=Contoso Root Certification Authority*" } )
   Set-VpnAuthProtocol -RootCertificateNameToAccept $cert1 -PassThru
   ```
**Como configurar o servidor RRAS para impor a revogação de certificado para conexões VPN baseadas em certificados de computador IKEv2**

1. Em uma janela de prompt de comando, execute o seguinte comando: 
   ```
   reg add HKLM\SYSTEM\CurrentControlSet\Services\RemoteAccess\Parameters\Ikev2 /f /v CertAuthFlags /t REG_DWORD /d "4"
   ```

1. Reinicie o serviço de **Roteamento e acesso remoto** .
  
Para desabilitar a revogação de certificados para essas conexões VPN, defina **CertAuthFlags = 2** ou remova o valor de **CertAuthFlags** e reinicie o serviço de **Roteamento e acesso remoto** . 

**Como revogar um certificado de cliente VPN para uma conexão VPN baseada em um certificado de computador IKEv2**
1. Revogue o certificado de cliente VPN da autoridade de certificação.
1. Publique uma nova CRL da autoridade de certificação.
1. No servidor RRAS, abra uma janela de prompt de comando administrativo e execute os seguintes comandos:
   ```
   certutil -urlcache * delete
   certutil -setreg chain\ChainCacheResyncFiletime @now
   ```

**Como verificar se a revogação de certificado para conexões VPN com base em certificado de máquina IKEv2 está funcionando**  
>[!Note]  
> Antes de usar este procedimento, certifique-se de habilitar o log de eventos operacional do CAPI2.
1. Siga as etapas anteriores para revogar um certificado de cliente VPN.
1. Tente se conectar à VPN usando um cliente que tenha o certificado revogado. O servidor RRAS deve recusar a conexão e exibir uma mensagem como "as credenciais de autenticação IKE são inaceitáveis".
1. No servidor RRAS, abra Visualizador de Eventos e navegue até **aplicativos e serviços Logs/Microsoft/Windows/CAPI2**. 
1. Procure um evento que tenha as seguintes informações:
   * Nome do log: **Microsoft-Windows-CAPI2/operacional Microsoft-Windows-CAPI2/Operational**
   * ID do evento: **41** 
   * O evento contém o seguinte texto: **Subject = "*FQDN do cliente*"** (o*FQDN do cliente* representa o nome de domínio totalmente qualificado do cliente que tem o certificado revogado.) 

   O campo **<Result>** dos dados do evento deve incluir **o certificado é revogado**. Por exemplo, consulte os trechos a seguir de um evento:
   ```xml
   Log Name:      Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational  
   Source:        Microsoft-Windows-CAPI2  
   Date:          5/20/2019 1:33:24 PM  
   Event ID:      41  
   ...  
   Event Xml:
   <Event xmlns="https://schemas.microsoft.com/win/2004/08/events/event">
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

### <a name="trusted-platform-module-tpm-key-attestation"></a>Atestado de chave de Trusted Platform Module (TPM)

Um certificado de usuário que tem uma chave atestada pelo TPM fornece maior garantia de segurança, feito backup por não exportabilidade, antifalsificação e isolamento de chaves fornecidas pelo TPM.

Para obter mais informações sobre o atestado de chave do TPM no Windows 10, consulte [atestado de chave do TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## <a name="next-step"></a>Próximas etapas

[Comece a planejar a implantação de VPN Always on](always-on-vpn-deploy-planning.md): antes de instalar a função de servidor de acesso remoto no computador que você planeja usar como um servidor VPN, execute as seguintes tarefas. Após o planejamento apropriado, você pode implantar Always On VPN e, opcionalmente, configurar o acesso condicional para conectividade VPN usando o Azure AD.  

## <a name="related-topics"></a>Tópicos relacionados
- [Balanceamento de carga do servidor proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): clientes do serviço RADIUS (RADIUS), que são servidores de acesso à rede, como servidores VPN (rede virtual privada) e pontos de acesso sem fio, criam solicitações de conexão e as enviam para servidores RADIUS, como o NPS. Em alguns casos, um servidor NPS pode receber muitas solicitações de conexão ao mesmo tempo, resultando em um desempenho degradado ou em uma sobrecarga.

- [Visão geral do Gerenciador de tráfego](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): Este tópico fornece uma visão geral do Gerenciador de tráfego do Azure, que permite controlar a distribuição do tráfego do usuário para pontos de extremidade de serviço. O Gerenciador de tráfego usa o DNS (sistema de nomes de domínio) para direcionar solicitações de cliente para o ponto de extremidade mais apropriado com base em um método de roteamento de tráfego e a integridade dos pontos. 

- [Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): Este tópico fornece os pré-requisitos, como implantações e implantações híbridas somente na nuvem.  Este tópico também lista as perguntas frequentes sobre o Windows Hello para empresas.

- [Estudo de caso técnico: Habilitando o acesso remoto com o Windows Hello para empresas no Windows 10](https://msdn.microsoft.com/library/mt728163.aspx): neste estudo de caso técnico, você aprende como a Microsoft implementa o acesso remoto com o Windows Hello para empresas.  O Windows Hello for Business é uma abordagem de autenticação baseada em certificado ou chave privada para organizações e consumidores que vai além de senhas. Essa forma de autenticação depende de credenciais de pares de chaves que podem substituir senhas e são resistentes a violações, roubos e phishing. 

- [Integrar a autenticação RADIUS com o azure servidor de autenticação multifator](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): Este tópico orienta você pela adição e configuração de uma autenticação de cliente RADIUS com o Azure servidor de autenticação multifator. RADIUS é um protocolo padrão para aceitar solicitações de autenticação e processar essas solicitações. O Servidor de Autenticação Multifator do Azure pode atuar como um servidor RADIUS. 

- [Recursos de segurança de VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): Este tópico fornece diretrizes de segurança de VPN para bloqueio de VPN, integração de WIP (proteção de informações do Windows) com VPN e filtros de tráfego. 

- [Opções de perfil de VPN disparadas automaticamente](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): Este tópico fornece opções de perfil de VPN disparadas automaticamente, como gatilho de aplicativo, gatilho baseado em nome e Always on.

- [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Este tópico fornece uma visão geral da plataforma de acesso condicional baseado em nuvem para fornecer uma opção de conformidade do dispositivo para clientes remotos. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD). 

- [Atestado de chave do TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): Este tópico fornece uma visão geral do Trusted Platform Module (TPM) e as etapas para implantar o atestado de chave do TPM. Você também pode encontrar informações de solução de problemas e etapas para resolver problemas.
