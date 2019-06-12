---
title: Solucionar problemas de VPN Always On
description: Este tópico fornece instruções para verificar e Solucionando problemas de implantação de VPN Always On no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d9e0efede39f5a8189dbb3d62033210c393c424d
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749644"
---
# <a name="troubleshoot-always-on-vpn"></a>Solucionar problemas de VPN Always On 

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

Se sua configuração de VPN Always On está falhando conectar clientes a sua rede interna, a causa provável é um certificado inválido de VPN, políticas NPS incorretas ou problemas com os scripts de implantação de cliente ou no roteamento e acesso remoto. A primeira etapa na solução de problemas e teste sua conexão VPN é entender os componentes principais da infraestrutura de VPN Always On. 

Você pode solucionar problemas de conexão de várias maneiras. Para problemas do lado do cliente e solução de problemas gerais, os logs do aplicativo em computadores cliente são imprescindíveis. Para problemas específicos de autenticação, o log do NPS no servidor NPS pode ajudá-lo a determinar a origem do problema.

## <a name="error-codes"></a>Códigos de erro

### <a name="error-code-800"></a>Código de erro: 800

- **Descrição do erro.** A conexão remota não foi feita porque a tentativa de túneis VPN falhou. O servidor VPN talvez esteja inacessível. Se essa conexão está tentando usar um túnel L2TP IPsec, os parâmetros de segurança necessária para o IPsec negociação talvez não esteja configurada corretamente.

- **Possível causa.** Esse erro ocorre quando o tipo de túnel VPN está **automática** e a tentativa de conexão falhará para todos os túneis VPN.

- **Soluções possíveis:**

    - Se você souber qual túnel para usar para sua implantação, defina o tipo de VPN para esse tipo específico de túnel no lado do cliente VPN.

    - Ao fazer uma conexão VPN com um tipo específico de túnel, sua conexão ainda falhará, mas resultará em um erro mais específico de túnel (por exemplo, "GRE bloqueado para PPTP").

    - Esse erro também ocorre quando o servidor VPN não pode ser alcançado ou falha de conexão de túnel.

- **Certificar-se:**

    - IKE (portas UDP 500 e 4500) não serão bloqueadas.

    - Os certificados adequados para IKE estão presentes no cliente e o servidor.

### <a name="error-code-809"></a>Código de erro: 809

- **Descrição do erro.**  Não foi possível estabelecer a conexão de rede entre o computador e o servidor VPN porque o servidor remoto não está respondendo. Isso pode ser porque um dos dispositivos de rede (por exemplo, roteadores de firewalls, NATS,) entre o computador e o servidor remoto não está configurado para permitir conexões VPN. Entre em contato com o administrador ou o provedor de serviços para determinar qual dispositivo pode estar causando o problema.

- **Possível causa.** Esse erro é causado por 4500 portas no servidor VPN ou o firewall ou bloqueado UDP 500.

- **Solução possível.** Certifique-se de que as portas UDP 500 e 4500 são permitidas em todos os firewalls entre o cliente e o servidor RRAS.

### <a name="error-code-812"></a>Código de erro: 812

- **Descrição do erro.** Não é possível se conectar à VPN Always On. A conexão foi impedida devido a uma política configurada no servidor RAS/VPN. Especificamente, o método de autenticação que o servidor usado para verificar seu nome de usuário e senha pode não coincidir com o método de autenticação configurado no seu perfil de conexão. Entre em contato com o administrador do servidor RAS e notificá-lo desse erro.

- **Possíveis causas:**

    - A causa comum desse erro é que o NPS tenha especificado uma condição de autenticação que o cliente não puder atender. Por exemplo, o NPS pode especificar o uso de um certificado para proteger a conexão de PEAP, mas o cliente está tentando usar EAP-MSCHAPv2.

    - Log de eventos 20276 é registrada no Visualizador de eventos quando a configuração de protocolo de autenticação de servidor VPN baseado em RRAS não corresponde do computador de cliente VPN.

- **Solução possível.** Certifique-se de que sua configuração de cliente corresponder às condições especificadas no servidor NPS.

### <a name="error-code-13806"></a>Código de erro: 13806

- **Descrição do erro.** IKE falha ao localizar um certificado de computador válido. Entre em contato com seu administrador de segurança de rede sobre como instalar um certificado válido no repositório de certificados apropriado.

- **Possível causa.** Esse erro normalmente ocorre quando nenhum certificado do computador ou certificado de raiz do computador está presente no servidor VPN.

- **Solução possível.** Certifique-se de que os certificados que descritas nesta implantação são instalados no computador cliente e o servidor VPN.

### <a name="error-code-13801"></a>Código de erro: 13801

- **Descrição do erro.** Credenciais de autenticação IKE são inaceitáveis.

- **Possíveis causas.** Esse erro normalmente ocorre em um dos seguintes casos:

    - O certificado do computador usado para validação de IKEv2 no servidor de acesso remoto não tem **autenticação de servidor** sob **uso avançado de chave**.

    - O certificado do computador no servidor RAS expirou.

    - O certificado raiz ao validar o certificado do servidor de acesso remoto não estiver presente no computador cliente.

    - O nome do servidor VPN usado no computador cliente não corresponde a **subjectName** do certificado do servidor.

- **Solução possível.** Verifique se o certificado do servidor inclui **autenticação de servidor** sob **uso avançado de chave**. Verifique se o certificado do servidor ainda é válido. Verifique se a autoridade de certificação usada é listada sob **autoridades de certificação raiz confiáveis** no servidor RRAS. Verifique se o cliente VPN conecta-se usando o FQDN do servidor VPN, conforme apresentada no certificado do servidor VPN.

### <a name="error-code-0x80070040"></a>Código de erro: 0x80070040

- **Descrição do erro.** O certificado do servidor não tem **autenticação de servidor** como uma das entradas de uso do seu certificado.

- **Possível causa.** Esse erro pode ocorrer se nenhum certificado de autenticação do servidor está instalado no servidor RAS.

- **Solução possível.** Certifique-se de que o certificado do computador que o servidor RAS usa para **IKEv2** tem **autenticação do servidor** como uma das entradas de uso de certificado.

### <a name="error-code-0x800b0109"></a>Código de erro: 0x800B0109

Em geral, a máquina de cliente VPN ingressou no domínio com base no Active Directory. Se você usar credenciais de domínio para fazer logon no servidor VPN, o certificado é instalado automaticamente em autoridades de certificação raiz confiáveis armazenar. No entanto, se o computador não estiver ingressado no domínio ou se você usar uma cadeia de certificados alternativo, você pode enfrentar esse problema.

- **Descrição do erro.** Uma cadeia de certificados foi processada mas terminou em um certificado raiz que o provedor de confiabilidade não confia.

- **Possível causa.** Esse erro pode ocorrer se o certificado de CA de raiz confiável adequado não estiver instalado em autoridades de certificação raiz armazenar no computador cliente.

- **Solução possível.** Certifique-se de que o certificado raiz está instalado no computador cliente no repositório de autoridades de certificação raiz confiáveis.

## <a name="logs"></a>Logs

### <a name="application-logs"></a>Logs de aplicativo

Os logs do aplicativo em computadores cliente registram a maioria dos detalhes do nível mais altos de eventos de conexão de VPN.

Procure eventos de fonte de cliente RAS. Todas as mensagens de erro retornam o código de erro no final da mensagem. Alguns dos códigos de erro mais comuns são detalhadas abaixo, mas uma lista completa está disponível no [roteamento e códigos de erro de acesso remoto](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx).

## <a name="nps-logs"></a>Logs NPS

O NPS cria e armazena os logs de contabilização do NPS. Por padrão, eles são armazenados em % SYSTEMROOT %\\System32\\Logfiles\\ em um arquivo chamado na*XXXX*. txt, onde *XXXX* é a data em que o arquivo foi criado.

Por padrão, esses logs estão no formato de valores separados por vírgulas, mas elas não incluem uma linha de cabeçalho. A linha de cabeçalho é:

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Se você colar essa linha de cabeçalho como a primeira linha do arquivo de log, em seguida, importe o arquivo para o Microsoft Excel, as colunas serão rotuladas corretamente.

Os logs NPS podem ser úteis para diagnosticar problemas relacionados à política. Para obter mais informações sobre os logs do NPS, consulte [interpretar arquivos de Log de formato de banco de dados de NPS](https://technet.microsoft.com/library/cc771748.aspx).

## <a name="vpnprofileps1-script-issues"></a>Problemas de script VPN_Profile.ps1

Os problemas mais comuns ao executar manualmente o script de VPN_ perfil.ps1 incluem:

- Você usa uma ferramenta de conexão remota?  Lembre-se de usar RDP ou outro método de conexão remota, conforme ele ficará prejudicado com detecção de logon do usuário.

- É o usuário administrador da máquina local?  Certifique-se de que, durante a execução do script VPN_Profile.ps1 que o usuário tem privilégios de administrador.

- Você tem recursos de segurança adicionais do PowerShell habilitados? Certifique-se de que a política de execução do PowerShell não está bloqueando o script. Você pode considerar desativar modo de linguagem restrita, se habilitado, antes de executar o script. Você pode ativar o modo de linguagem restrita depois que o script for concluído com êxito.

## <a name="always-on-vpn-client-connection-issues"></a>Problemas de conexão de cliente VPN Always On

Um pequeno erro de configuração pode fazer com que a conexão de cliente falhe e pode ser um desafio para encontrar a causa.  Um cliente de VPN Always On passa por várias etapas antes de estabelecer uma conexão. Ao solucionar problemas de conexão de cliente, percorra o processo de eliminação com o seguinte:

1. A máquina de modelo externamente está conectada? Um **whatismyip** verificação deve mostrar um endereço IP público que não pertence a você.

2. Você pode resolver o nome do servidor de acesso remoto/VPN para um endereço IP? Na **painel de controle** > **rede** e **Internet** > **conexões de rede**, abra as propriedades para o seu perfil VPN. O valor de **geral** guia deve ser resolvível publicamente por meio do DNS.

3. Você pode acessar o servidor VPN em uma rede externa? Considere abrindo controle mensagem ICMP (Internet Protocol) para a interface externa e executar o ping para o nome do cliente remoto. Após um ping bem-sucedido, você pode remover o ICMP a regra de permissão.

4. Você tem as NICs internas e externas no servidor VPN configurado corretamente? Eles estão em sub-redes diferentes? A NIC externa conectar-se à interface correta no firewall?

5. Estão UDP 500 e 4500 portas abertas do cliente para a interface externa do servidor VPN? Verifique o firewall do cliente, o firewall do servidor e os firewalls de hardware. IPSEC usa UDP porta 500, restauração, verifique se que você não tenha IPEC desabilitada ou bloqueada em qualquer lugar.

6. Validação de certificado está falhando? Verifique se que o servidor NPS tem um certificado de autenticação de servidor que pode atender a solicitações de IKE. Certifique-se de que você tem o IP do servidor VPN correto especificado como um cliente do NPS. Certifique-se de que você estiver autenticando com o PEAP e as propriedades EAP protegidas só devem permitir a autenticação com um certificado. Você pode verificar os logs de eventos do NPS para falhas de autenticação. Para obter mais detalhes, consulte [instalar e configurar o servidor NPS](vpn-deploy-nps.md)

7. Você se conectar, mas não tem acesso à rede de Internet/local? Verifique seus pools IP do servidor DHCP/VPN para problemas de configuração.

8. Você está conectando e ter um endereço IP interno válido, mas não a não ter acesso a recursos locais?  Verifique se que os clientes sabem como obter a esses recursos. Você pode usar o servidor VPN para rotear solicitações.

## <a name="azure-ad-conditional-access-connection-issues"></a>Problemas de conexão de acesso condicional do AD do Azure

### <a name="oops---you-cant-get-to-this-yet"></a>Opa - não é possível obter a este ainda

- **Descrição do erro.** Quando a política de acesso condicional é não atendida, bloqueando a conexão VPN, mas se conecta depois que o usuário seleciona **X** para fechar a mensagem.  Selecionando **Okey** faz com que outra tentativa de autenticação, que termina em outra mensagem "OPS". Esses eventos são registrados no log de evento operacional do AAD do cliente.

- **Possível causa**

  - O usuário tem um certificado de autenticação de cliente válido no seu certificado pessoal armazenar que não foi emitido pelo AD do Azure.

  - O perfil de VPN \<TLSExtensions\> seção está ausente ou não não conter o **\<EKUName\>acesso condicional do AAD\</EKUName\> \< EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>\<EKUName > acesso condicional do AAD < / EKUName\>\<EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>** entradas. O \<EKUName > e \<EKUOID > entradas informar ao cliente VPN qual certificado para recuperar do repositório de certificados do usuário ao passar o certificado para o servidor VPN. Sem isso, o cliente VPN usa qualquer certificado de autenticação de cliente válido está no repositório de certificados do usuário e a autenticação for bem-sucedida. 

  - O servidor RADIUS (NPS) não tem sido configurado para aceitar somente os certificados de cliente que contêm o **acesso condicional do AAD** OID.

- **Solução possível.** Para sair desse loop, faça o seguinte:

  1. No Windows PowerShell, execute as **Get-WmiObject** cmdlet para despejar a configuração do perfil VPN. 
  2. Verifique se que o  **\<TLSExtensions >** ,  **\<EKUName >** , e  **\<EKUOID >** seções existe e mostra a pasta correta o nome e OID.
      
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

  3. Para determinar se há certificados válidos no repositório de certificados do usuário, execute as **Certutil** comando:

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
     >Se um certificado do emissor **CN = gen de autoridade de certificação raiz de VPN do Microsoft 1** está presente no repositório pessoal do usuário, mas o usuário obteve acesso selecionando **X** para fechar a mensagem de OPS, coletar logs de eventos de CAPI2 para verificar o certificado usado para autenticar era um certificado de autenticação de cliente válido que não foi emitido da autoridade de certificação de raiz de VPN do Microsoft.

  4. Se houver um certificado de autenticação de cliente válido no repositório pessoal do usuário, a conexão falha (como deveria) depois que o usuário seleciona o **X** e, se o  **\<TLSExtensions >** ,  **\<EKUName >** , e  **\<EKUOID >** seções existe e contém as informações corretas.
   
     É exibida uma mensagem de erro que diz "um certificado pode não ser encontrado que pode ser usado com o protocolo de autenticação extensível".

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>Não é possível excluir o certificado de folha de conectividade de VPN

- **Descrição do erro.** Certificados na folha de conectividade de VPN não podem ser excluídos.

- **Possível causa.** O certificado é definido como **primário**.

- **Solução possível.**

    1. Na folha de conectividade de VPN, selecione o certificado.
    2. Sob **primário**, selecione **nenhuma**, em seguida, selecione **salvar**.
    3. Na folha de conectividade de VPN, selecione o certificado novamente.
    4. Selecione **excluir**.
