---
title: Solucionar problemas de VPN Always On
description: Este tópico fornece instruções para verificar e solucionar problemas de implantação de VPN Always On no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 649fbc16e3dfef2ed1061d0ba6a5c22a8712b186
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404373"
---
# <a name="troubleshoot-always-on-vpn"></a>Solucionar problemas de VPN Always On 

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

Se a configuração de VPN Always On estiver falhando ao conectar clientes à sua rede interna, a causa provavelmente será um certificado VPN inválido, políticas de NPS incorretas ou problemas com os scripts de implantação do cliente ou no roteamento e acesso remoto. A primeira etapa na solução de problemas e no teste de sua conexão VPN é compreender os principais componentes da infraestrutura de VPN Always On. 

Você pode solucionar problemas de conexão de várias maneiras. Para problemas do lado do cliente e solução de problemas gerais, os logs do aplicativo em computadores cliente são inestimáveis. Para problemas específicos de autenticação, o log do NPS no servidor NPS pode ajudá-lo a determinar a origem do problema.

## <a name="error-codes"></a>Códigos de erro

### <a name="error-code-800"></a>Código de erro: 800

- **Descrição do erro.** A conexão remota não foi feita porque houve falha na tentativa de túneis VPN. O servidor VPN pode estar inacessível. Se essa conexão estiver tentando usar um túnel L2TP/IPsec, talvez os parâmetros de segurança necessários para a negociação IPsec não estejam configurados corretamente.

- **Possível causa.** Esse erro ocorre quando o tipo de túnel VPN é **automático** e a tentativa de conexão falha para todos os túneis de VPN.

- **Soluções possíveis:**

    - Se você souber qual túnel usar para a sua implantação, defina o tipo de VPN para aquele tipo de túnel específico no lado do cliente VPN.

    - Ao fazer uma conexão VPN com um tipo de túnel específico, sua conexão ainda falhará, mas isso resultará em um erro mais específico de túnel (por exemplo, "GRE bloqueado para PPTP").

    - Esse erro também ocorre quando o servidor VPN não pode ser acessado ou a conexão do túnel falha.

- **Certificar-se:**

    - As portas IKE (portas UDP 500 e 4500) não estão bloqueadas.

    - Os certificados corretos para IKE estão presentes no cliente e no servidor.

### <a name="error-code-809"></a>Código de erro: 809

- **Descrição do erro.**  Não foi possível estabelecer a conexão de rede entre o computador e o servidor VPN porque o servidor remoto não está respondendo. Isso pode ocorrer porque um dos dispositivos de rede (por exemplo, firewalls, NAT, roteadores) entre o computador e o servidor remoto não está configurado para permitir conexões VPN. Entre em contato com seu administrador ou seu provedor de serviços para determinar qual dispositivo pode estar causando o problema.

- **Possível causa.** Esse erro é causado por portas do UDP 500 ou 4500 bloqueadas no servidor VPN ou no firewall.

- **Solução possível.** Verifique se as portas UDP 500 e 4500 são permitidas por todos os firewalls entre o cliente e o servidor RRAS.

### <a name="error-code-812"></a>Código de erro: 812

- **Descrição do erro.** Não é possível conectar-se ao Always On VPN. A conexão foi impedida devido a uma política configurada no servidor RAS/VPN. Especificamente, o método de autenticação que o servidor usou para verificar seu nome de usuário e senha pode não corresponder ao método de autenticação configurado em seu perfil de conexão. Entre em contato com o administrador do servidor RAS e notifique-o desse erro.

- **Possíveis causas:**

    - A causa típica desse erro é que o NPS especificou uma condição de autenticação que o cliente não consegue atender. Por exemplo, o NPS pode especificar o uso de um certificado para proteger a conexão PEAP, mas o cliente está tentando usar EAP-MSCHAPv2.

    - O log de eventos 20276 é registrado no Visualizador de eventos quando a configuração do protocolo de autenticação do servidor VPN baseado em RRAS não corresponde à do computador cliente VPN.

- **Solução possível.** Verifique se a configuração do cliente corresponde às condições especificadas no servidor NPS.

### <a name="error-code-13806"></a>Código de erro: 13806

- **Descrição do erro.** Falha de IKE ao localizar um certificado de máquina válido. Entre em contato com o administrador de segurança de rede para instalar um certificado válido no repositório de certificados apropriado.

- **Possível causa.** Esse erro normalmente ocorre quando nenhum certificado de computador ou de computador raiz está presente no servidor VPN.

- **Solução possível.** Verifique se os certificados descritos nessa implantação estão instalados no computador cliente e no servidor VPN.

### <a name="error-code-13801"></a>Código de erro: 13801

- **Descrição do erro.** As credenciais de autenticação IKE são inaceitáveis.

- **Possíveis causas.** Esse erro normalmente ocorre em um dos seguintes casos:

    - O certificado de computador usado para validação IKEv2 no servidor RAS não tem **autenticação de servidor** sob **uso avançado de chave**.

    - O certificado do computador no servidor RAS expirou.

    - O certificado raiz para validar o certificado do servidor RAS não está presente no computador cliente.

    - O nome do servidor VPN usado no computador cliente não corresponde ao **assuntoname** do certificado do servidor.

- **Solução possível.** Verifique se o certificado do servidor inclui **autenticação de servidor** sob **uso avançado de chave**. Verifique se o certificado do servidor ainda é válido. Verifique se a autoridade de certificação usada está listada em **autoridades de certificação raiz confiáveis** no servidor RRAS. Verifique se o cliente VPN se conecta usando o FQDN do servidor VPN, conforme apresentado no certificado do servidor VPN.

### <a name="error-code-0x80070040"></a>Código de erro: 0x80070040

- **Descrição do erro.** O certificado do servidor não tem **autenticação de servidor** como uma de suas entradas de uso de certificado.

- **Possível causa.** Esse erro pode ocorrer se nenhum certificado de autenticação de servidor estiver instalado no servidor RAS.

- **Solução possível.** Verifique se o certificado do computador que o servidor RAS usa para **IKEv2** tem **autenticação de servidor** como uma das entradas de uso de certificado.

### <a name="error-code-0x800b0109"></a>Código de erro: 0x800B0109

Em geral, o computador cliente VPN é adicionado ao domínio baseado em Active Directory. Se você usar credenciais de domínio para fazer logon no servidor VPN, o certificado será instalado automaticamente no repositório autoridades de certificação raiz confiáveis. No entanto, se o computador não tiver ingressado no domínio ou se você usar uma cadeia de certificados alternativa, poderá enfrentar esse problema.

- **Descrição do erro.** Uma cadeia de certificados foi processada, mas terminada em um certificado raiz que o provedor de confiança não confia.

- **Possível causa.** Esse erro pode ocorrer se o certificado de autoridade de certificação raiz confiável apropriado não estiver instalado no repositório de autoridades de certificação raiz confiáveis no computador cliente.

- **Solução possível.** Verifique se o certificado raiz está instalado no computador cliente no repositório de autoridades de certificação raiz confiáveis.

## <a name="logs"></a>Logs

### <a name="application-logs"></a>Logs de aplicativo

Os logs de aplicativo em computadores cliente registram a maioria dos detalhes de nível superior dos eventos de conexão VPN.

Procure eventos da origem RasClient. Todas as mensagens de erro retornam o código de erro no final da mensagem. Alguns dos códigos de erro mais comuns são detalhados abaixo, mas uma lista completa está disponível nos [códigos de erro de roteamento e acesso remoto](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx).

## <a name="nps-logs"></a>Logs do NPS

O NPS cria e armazena os logs de contabilidade do NPS. Por padrão, eles são armazenados em% SYSTEMROOT% \\System32 @ no__t-1Logfiles @ no__t-2 em um arquivo chamado em*xxxx*. txt, em que *xxxx* é a data em que o arquivo foi criado.

Por padrão, esses logs estão em formato de valores separados por vírgula, mas não incluem uma linha de cabeçalho. A linha do cabeçalho é:

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Se você colar essa linha de título como a primeira linha do arquivo de log e, em seguida, importar o arquivo para o Microsoft Excel, as colunas serão rotuladas corretamente.

Os logs do NPS podem ser úteis para diagnosticar problemas relacionados à política. Para obter mais informações sobre logs do NPS, consulte [interpretar arquivos de log de formato de banco de dados NPS](https://technet.microsoft.com/library/cc771748.aspx).

## <a name="vpn_profileps1-script-issues"></a>Problemas de script VPN_Profile. ps1

Os problemas mais comuns ao executar manualmente o script VPN_ Profile. ps1 incluem:

- Você usa uma ferramenta de conexão remota?  Certifique-se de não usar o RDP ou outro método de conexão remota, pois ele se bagunça com a detecção de logon do usuário.

- O usuário é um administrador do computador local?  Certifique-se de que, ao executar o script VPN_Profile. ps1, que o usuário tenha privilégios de administrador.

- Você tem recursos de segurança adicionais do PowerShell habilitados? Verifique se a política de execução do PowerShell não está bloqueando o script. Você pode considerar desativar o modo de linguagem restrita, se habilitado, antes de executar o script. Você pode ativar o modo de linguagem restrita depois que o script for concluído com êxito.

## <a name="always-on-vpn-client-connection-issues"></a>Problemas de conexão de cliente VPN Always On

Uma pequena configuração incorreta pode fazer com que a conexão do cliente falhe e possa ser desafiadora para encontrar a causa.  Um cliente VPN Always On passa por várias etapas antes de estabelecer uma conexão. Ao solucionar problemas de conexão de cliente, passe pelo processo de eliminação com o seguinte:

1. O computador de modelo está conectado externamente? Uma verificação de **whatismyip** deve mostrar um endereço IP público que não pertence a você.

2. Você pode resolver o nome do servidor de acesso remoto/VPN para um endereço IP? Em**conexões**de rede de **Internet** > e**rede** do **painel** > de controle, abra as propriedades do seu perfil de VPN. O valor na guia **geral** deve ser pode ser resolvido publicamente por meio do DNS.

3. Você pode acessar o servidor VPN de uma rede externa? Considere abrir o protocolo ICMP para a interface externa e executar o ping do nome do cliente remoto. Depois que um ping for bem-sucedido, você poderá remover a regra de permissão ICMP.

4. Você tem as NICs internas e externas no servidor VPN configuradas corretamente? Eles estão em sub-redes diferentes? A NIC externa se conecta à interface correta no seu firewall?

5. As portas UDP 500 e 4500 são abertas do cliente para a interface externa do servidor VPN? Verifique o Firewall do cliente, o Firewall do servidor e os firewalls de hardware. O IPSEC usa a porta UDP 500, portanto, certifique-se de que você não tenha o IPEC desabilitado ou bloqueado em qualquer lugar.

6. A validação do certificado está falhando? Verifique se o servidor NPS tem um certificado de autenticação de servidor que pode atender a solicitações IKE. Verifique se você tem o IP do servidor VPN correto especificado como um cliente NPS. Certifique-se de que você está Autenticando com o PEAP e que as propriedades EAP protegidas só devem permitir a autenticação com um certificado. Você pode verificar os logs de eventos do NPS quanto a falhas de autenticação. Para obter mais detalhes, consulte [instalar e configurar o servidor NPS](vpn-deploy-nps.md)

7. Você está se conectando, mas não tem acesso à rede local/Internet? Verifique os pools de IPS do servidor DHCP/VPN para problemas de configuração.

8. Você está se conectando e tem um IP interno válido, mas não tem acesso aos recursos locais?  Verifique se os clientes sabem como chegar a esses recursos. Você pode usar o servidor VPN para rotear solicitações.

## <a name="azure-ad-conditional-access-connection-issues"></a>Problemas de conexão de acesso condicional do Azure AD

### <a name="oops---you-cant-get-to-this-yet"></a>Opa – você não pode acessar isso ainda

- **Descrição do erro.** Quando a política de acesso condicional não é satisfeita, bloqueando a conexão VPN, mas se conecta depois que o usuário seleciona **X** para fechar a mensagem.  A seleção de **OK** causa outra tentativa de autenticação, que termina em outra mensagem "Ops". Esses eventos são registrados no log de eventos operacionais do AAD do cliente.

- **Causa possível**

  - O usuário tem um certificado de autenticação de cliente válido em seu repositório de certificados pessoal que não foi emitido pelo Azure AD.

  - A seção TLSExtensions \<\> do perfil VPN está ausente ou não contém o **\</EKUName\>\>\<de acesso\<condicional EKUName AAD EKUOID\>1.3.6.1.4.1.311.87 </EKUOID\>\>\<\>EKUName > AAD Conditional Access </EKUName EKUOID 1.3.6.1.4.1.311.87 </EKUOID\> \<** entradas. As \<entradas EKUName > \<e EKUOID > dizem ao cliente VPN qual certificado recuperar do repositório de certificados do usuário ao passar o certificado para o servidor VPN. Sem isso, o cliente VPN usa qualquer certificado de autenticação de cliente válido que esteja no repositório de certificados do usuário e a autenticação seja realizada com sucesso. 

  - O servidor RADIUS (NPS) não foi configurado para aceitar somente certificados de cliente que contenham o OID de **acesso condicional do AAD** .

- **Solução possível.** Para escapar desse loop, faça o seguinte:

  1. No Windows PowerShell, execute o cmdlet **Get-WmiObject** para despejar a configuração do perfil VPN. 
  2. Verifique se as  **\<seções TLSExtensions >** ,  **\<EKUName >** e  **\<EKUOID >** existem e mostram o nome e o OID corretos.
      
      ```powershell
      PS C:\> Get-WmiObject -Class MDM_VPNv2_01 -Namespace root\cimv2\mdm\dmmap

      __GENUS                 : 2
      __CLASS                 : MDM_VPNv2_01
      __SUPERCLASS            :
      __DYNASTY               : MDM_VPNv2_01
      __RELPATH               : MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VPNv2"
      __PROPERTY_COUNT        : 10
      __DERIVATION            : {}
      __SERVER                : DERS2
      __NAMESPACE             : root\cimv2\mdm\dmmap
      __PATH                  : \\DERS2\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VP
                                  Nv2"
      AlwaysOn                :
      ByPassForLocal          :
      DnsSuffix               :
      EdpModeId               :
      InstanceID              : AlwaysOnVPN
      LockDown                :
      ParentID                : ./Vendor/MSFT/VPNv2
      ProfileXML              : <VPNProfile><RememberCredentials>false</RememberCredentials><DeviceCompliance><Enabled>true</
                                  Enabled><Sso><Enabled>true</Enabled></Sso></DeviceCompliance><NativeProfile><Servers>derras2.
                                  corp.deverett.info;derras2.corp.deverett.info</Servers><RoutingPolicyType>ForceTunnel</Routin
                                  gPolicyType><NativeProtocolType>Ikev2</NativeProtocolType><Authentication><UserMethod>Eap</Us
                                  erMethod><MachineMethod>Eap</MachineMethod><Eap><Configuration><EapHostConfig
                                  xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config
                                  xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.
                                  com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.mic
                                  rosoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptFor
                                  ServerValidation>true</DisableUserPromptForServerValidation><ServerNames></ServerNames></Serv
                                  erValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Ea
                                  p xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type>
                                  <EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><Credenti
                                  alsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore
                                  ></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUse
                                  rPromptForServerValidation><ServerNames></ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b
                                  1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>fal
                                  se</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/E
                                  apTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://ww
                                  w.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</AcceptServerName><TLSExtens
                                  ions
                                  xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xml
                                  ns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><
                                  EKUName>AAD Conditional
                                  Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList
                                  Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></Client
                                  AuthEKUList></FilteringInfo></TLSExtensions></EapType></Eap><EnableQuarantineChecks>false</En
                                  ableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><Perfo
                                  rmServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2"
                                  >false</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisionin
                                  g/MsPeapConnectionPropertiesV2">false</AcceptServerName></PeapExtensions></EapType></Eap></Co
                                  nfig></EapHostConfig></Configuration></Eap></Authentication></NativeProfile></VPNProfile>
      RememberCredentials     : False
      TrustedNetworkDetection :
      PSComputerName          : DERS2
      ```

  3. Para determinar se há certificados válidos no repositório de certificados do usuário, execute o comando **certutil** :

     ```powershell
     C:\>certutil -store -user My

      My "Personal"
      ================ Certificate 0 ================
      Serial Number: 32000000265259d0069fa6f205000000000026
      Issuer: CN=corp-DEDC0-CA, DC=corp, DC=deverett, DC=info
       NotBefore: 12/8/2017 8:07 PM
       NotAfter: 12/8/2018 8:07 PM
      Subject: E=winfed@deverett.info, CN=WinFed, OU=Users, OU=Corp, DC=corp, DC=deverett, DC=info
      Certificate Template Name (Certificate Type): User
      Non-root Certificate
      Template: User
      Cert Hash(sha1): a50337ab015d5612b7dc4c1e759d201e74cc2a93
        Key Container = a890fd7fbbfc072f8fe045e680c501cf_5834bfa9-1c4a-44a8-a128-c2267f712336
        Simple container name: te-User-c7bcc4bd-0498-4411-af44-da2257f54387
        Provider = Microsoft Enhanced Cryptographic Provider v1.0
      Encryption test passed
        
      ================ Certificate 1 ================
      Serial Number: 367fbdd7e6e4103dec9b91f93959ac56
      Issuer: CN=Microsoft VPN root CA gen 1
       NotBefore: 12/8/2017 6:24 PM
       NotAfter: 12/8/2017 7:29 PM
      Subject: CN=WinFed@deverett.info
      Non-root Certificate
      Cert Hash(sha1): 37378a1b06dcef1b4d4753f7d21e4f20b18fbfec
        Key Container = 31685cae-af6f-48fb-ac37-845c69b4c097
        Unique container name: bf4097e20d4480b8d6ebc139c9360f02_5834bfa9-1c4a-44a8-a128-c2267f712336
        Provider = Microsoft Software Key Storage Provider
      Private key is NOT exportable
      Encryption test passed
     ```
     >[!NOTE]
     >Se um certificado do emissor **CN = Microsoft VPN root CA Gen 1** estiver presente no armazenamento pessoal do usuário, mas o usuário obtiver acesso selecionando **X** para fechar a mensagem do Ops, coletar logs de eventos CAPI2 para verificar se o certificado usado para autenticar foi um certificado de autenticação de cliente válido que não foi emitido pela AC raiz de VPN da Microsoft.

  4. Se um certificado de autenticação de cliente válido existir no armazenamento pessoal do usuário, a conexão falhará (como deveria) depois que o usuário selecionar o **X** e se  **\<TLSExtensions >** ,  **\<EKUName >** e  **As\<seções EKUOID >** existem e contêm as informações corretas.
   
     Uma mensagem de erro que diz "um certificado não encontrado que pode ser usado com o protocolo de autenticação extensível" é exibida.

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>Não é possível excluir o certificado da folha de conectividade VPN

- **Descrição do erro.** Certificados na folha conectividade VPN não podem ser excluídos.

- **Possível causa.** O certificado está definido como **primário**.

- **Solução possível.**

    1. Na folha conectividade VPN, selecione o certificado.
    2. Em **primário**, selecione **não**e, em seguida, selecione **salvar**.
    3. Na folha conectividade VPN, selecione o certificado novamente.
    4. Selecione **excluir**.
