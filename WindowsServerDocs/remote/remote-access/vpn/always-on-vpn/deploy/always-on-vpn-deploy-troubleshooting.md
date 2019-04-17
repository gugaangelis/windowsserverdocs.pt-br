---
title: Solucionar problemas VPN Always On
description: Este tópico fornece instruções para verificar e Solucionando problemas de implantação de VPN Always On no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 47d22d566407f45fe6ac78931ffea7b5b2854a1c
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066983"
---
# Solucionar problemas VPN Always On 

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

Se sua configuração de VPN Always On está falhando conectar clientes a sua rede interna, a causa é provavelmente um certificado VPN inválido, políticas NPS incorretas ou problemas com os scripts de implantação do cliente ou em roteamento e acesso remoto. A primeira etapa na solução de problemas e testar sua conexão VPN é entender os componentes principais da infraestrutura VPN Always On. 

Você pode solucionar problemas de conexão de várias maneiras. Para problemas do lado do cliente e solução de problemas gerais, os logs de aplicativos em computadores cliente são muito valiosos. Para problemas de autenticação específicos, o log do NPS no servidor NPS pode ajudá-lo a determinar a origem do problema.


## Códigos de erro


### Código de erro: 800

-   **Descrição do erro.** A conexão remota não foi feita porque os tentativas túneis VPN falhou. O servidor VPN pode estar inacessível. Se essa conexão está tentando usar um túnel IPsec/L2TP, os parâmetros de segurança necessário para IPsec negociação pode não estar configurada corretamente.


-   **Possível causa.** Esse erro ocorre quando o tipo de túnel VPN é **automática** e a tentativa de conexão falhar para todos os túneis VPN.

-   **Possíveis soluções:**

    -   Se você souber quais encapsulamento a ser usado para a implantação, defina o tipo de VPN para esse tipo de encapsulamento específico no lado do cliente VPN.

    -   Fazendo uma conexão VPN com um tipo específico de túnel, ainda haverá falha na sua conexão, mas isso resultará em um erro de encapsulamento mais específico (por exemplo, "GRE bloqueado para PPTP").

    -   Esse erro também ocorre quando o servidor VPN não pode ser alcançado ou a conexão de encapsulamento falha.

-   **Certificar-se:**

    -   Portas IKE (UDP ports500 e 4500) não serão bloqueados.

    -   Os certificados corretos para IKE estão presentes no cliente e o servidor.

### Código de erro: 809

-   **Descrição do erro.**  Não foi possível estabelecer a conexão de rede entre o computador e o servidor VPN porque o servidor remoto não está respondendo. Isso pode acontecer porque um dos dispositivos de rede \ (por exemplo, firewalls, NAT, routers\) entre o computador e o servidor remoto não está configurado para permitir conexões VPN. Entre em contato com o administrador ou o provedor de serviços para determinar qual dispositivo pode estar causando o problema.

-   **Possível causa.** Esse erro é causado por bloqueados UDP 500 ou 4500 portas no servidor VPN ou o firewall.

-   **Possíveis soluções.** Certifique-se de que 4500 e UDP ports500 são permitidos por meio de todos os firewalls entre o cliente e o servidor RRAS. 
    
    
### Código de erro: 812

-   **Descrição do erro.** Não é possível se conectar a VPN Always On. A conexão foi impedida devido a uma política configurada no servidor RAS/VPN. Especificamente, o método de autenticação que o servidor usado para verificar seu nome de usuário e senha pode não coincidir com o método de autenticação configurado em seu perfil de conexão. Contate o administrador do servidor RAS e notificá-lo desse erro.

-   **Possíveis causas:**

    -  A causa comum deste erro é que o NPS tenha especificado uma condição de autenticação que o cliente não possa atender aos. Por exemplo, o NPS pode especificar o uso de um certificado para proteger a conexão PEAP, mas o cliente está tentando usar o EAP-MSCHAPv2.

    -  Log de eventos 20276 é registrada para o Visualizador de eventos quando a configuração de protocolo de autenticação de servidor VPN com base em RRAS não corresponde do computador do cliente VPN.

-   **Possíveis soluções.** Certifique-se de que sua configuração de cliente coincide com as condições especificadas no servidor NPS.


### Código de erro: 13806

-   **Descrição do erro.** IKE falha ao localizar um certificado de máquina válido. Contate o administrador de segurança de rede sobre como instalar um certificado válido no repositório de certificados apropriado.

-   **Possível causa.** Esse erro normalmente ocorre quando nenhum certificado de máquina ou certificado de máquina raiz está presente no servidor VPN.

-   **Possíveis soluções.** Certifique-se de que os certificados descritos nesta implantação são instalados no computador cliente e o servidor VPN.

### Código de erro: 13801

-   **Descrição do erro.** Credenciais de autenticação IKE são inaceitáveis.

-   **Possíveis causas.** Esse erro normalmente ocorre em um dos seguintes casos:

    -   O certificado de máquina usado para validação IKEv2 no servidor RAS não tem **Autenticação de servidor** em **Uso avançado de chave**.

    -   O certificado de computador no servidor RAS expirou.

    -   O certificado raiz para validar o certificado do servidor RAS não está presente no computador cliente.

    -   O nome do servidor VPN usado no computador cliente não corresponde **subjectName** do certificado do servidor.

-   **Possíveis soluções.** Verifique se o certificado do servidor inclui a **Autenticação de servidor** em **Uso avançado de chave**. Verificar se o certificado do servidor ainda é válido. Verifique se a autoridade de certificação usada é listada em **Autoridades de certificação raiz confiáveis** no servidor RRAS. Verifique se que o cliente VPN se conecta usando o FQDN do servidor VPN conforme apresentado no certificado do servidor VPN.


### Código de erro: 0x80070040

-   **Descrição do erro.** O certificado do servidor não tem **Autenticação de servidor** como uma das suas entradas de uso do certificado.

-   **Possível causa.** Esse erro pode ocorrer se nenhum certificado de autenticação de servidor está instalado no servidor RAS.

-   **Possíveis soluções.** Certifique-se de que o certificado de computador que usa o servidor de acesso remoto para **IKEv2** tem **Autenticação de servidor** como uma das entradas de uso do certificado.

### Código de erro: 0x800B0109

Em geral, o computador do cliente VPN ingressou no domínio baseada no Active Directory. Se você usar credenciais de domínio para fazer logon no servidor VPN, o certificado é instalado automaticamente em autoridades de certificação raiz confiáveis da loja. No entanto, se o computador não tenha ingressado no domínio ou se você usar uma cadeia de certificados alternativa, você pode enfrentar esse problema.

-   **Descrição do erro.** Uma cadeia de certificados processada, mas terminou em um certificado raiz que o provedor de confiança não confia.

-   **Possível causa.** Esse erro pode ocorrer se o certificado apropriado autoridade de certificação raiz confiável não estiver instalado em autoridades de certificação raiz confiáveis armazenar no computador cliente.

-   **Possíveis soluções.** Certifique-se de que o certificado raiz é instalado no computador cliente no armazenamento de autoridades de certificação raiz confiáveis.

## Logs

### Logs de aplicativos

Os logs de aplicativos em computadores cliente registram a maioria dos detalhes de nível mais alto de eventos de conexão VPN.

Procure por eventos de fonte cliente RAS. Todas as mensagens de erro retornam o código de erro no final da mensagem. Alguns dos códigos de erro mais comuns são detalhadas abaixo, mas uma lista completa está disponível em [Roteamento e códigos de erro de acesso remoto](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx).

## Logs NPS
NPS cria e armazena os logs de estatísticas do NPS. Por padrão, eles são armazenados no %SYSTEMROOT%\\System32\\Logfiles\\ em um arquivo denominado no*XXXX.* txt, onde *XXXX* é a data em que o arquivo foi criado.

Por padrão, esses logs estão no formato de valores separados por vírgula, mas eles não incluem uma linha de cabeçalho. Na linha do título é:

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Se você colar essa linha de título como a primeira linha do arquivo de log, em seguida, importe o arquivo no Microsoft Excel, as colunas serão rotuladas corretamente.

Os logs do NPS podem ser úteis para diagnosticar problemas relacionados à política. Para obter mais informações sobre os logs de NPS, consulte [Interpretar arquivos de Log formato de banco de dados de NPS](https://technet.microsoft.com/library/cc771748.aspx).

## Problemas de script VPN_Profile.ps1
Os problemas mais comuns ao executar manualmente o script VPN_ Profile.ps1 incluem:

- Você usa uma ferramenta de conexão remota?  Certifique-se de não usar RDP ou outro método de conexão remota conforme ele confunde com detecção de logon do usuário.

- O usuário é um administrador da máquina local?  Certifique-se de que, ao executar o script VPN_Profile.ps1 que o usuário tem privilégios de administrador.

- Você tem recursos de segurança adicionais do PowerShell habilitados? Certifique-se de que a política de execução do PowerShell não está bloqueando o script. Você pode considerar desativar modo de linguagem restrita, se habilitado, antes de executar o script. Você pode ativar o modo de linguagem restrita depois que o script for concluído com êxito.

## Problemas de conexão do cliente VPN Always On
Um erro de configuração pequeno pode fazer com que a conexão de cliente falha e pode ser um desafio para encontrar a causa.  Um cliente VPN Always On passa por várias etapas antes de estabelecer uma conexão. Ao solucionar problemas de conexão do cliente, passar pelo processo de eliminação com o seguinte:


1. A máquina de modelo externamente está conectada? Uma verificação de **whatismyip** deve mostrar um endereço IP público que não pertença a você.

2. Você pode resolver o nome do servidor de acesso remoto/VPN para um endereço IP? No **Painel de controle** \> **rede** e **Internet** \> **Conexões de rede**, abra as propriedades para o seu perfil de VPN. O valor na guia **Geral** deve ser publicamente resolvido por meio do DNS.

3. Você pode acessar o servidor VPN de uma rede externa? Considere abrindo o protocolo de mensagem de controle da Internet (ICMP) para a interface externa e ping o nome do cliente remoto. Depois que um ping for bem-sucedida, você pode remover o ICMP permitir regra.

4. Você tem as NICs internas e externas no servidor VPN configurado corretamente? Eles estão em sub-redes diferentes? A placa de rede externa se conectar à interface correta no firewall?

5. Estão UDP 500 e 4500 portas abertas do cliente para a interface externa do servidor VPN? Verifique o firewall do cliente, servidor firewall e quaisquer firewalls de hardware. IPSEC usa UDP porta 500, restauração, verifique se que você não tenha IPEC desativado ou bloqueado em qualquer lugar.

7. Validação do certificado está falhando? Verifique se que o servidor NPS tem um certificado de autenticação de servidor pode atender às solicitações de IKE. Certifique-se de que você tenha o IP do servidor VPN correto especificado como um cliente do NPS. Verifique se você estiver autenticando com o PEAP, e as propriedades de EAP protegido só devem permitir a autenticação com um certificado. Você pode verificar os logs de eventos do NPS para falhas de autenticação. Para obter mais detalhes, consulte [instalar e configurar o servidor NPS](vpn-deploy-nps.md)

8. Você se conectar, mas não têm acesso à rede de Internet/local? Verifique os pools IP do servidor DHCP/VPN problemas de configuração.

9.  Você está conectando-se e ter um IP interno válido mas que não têm acesso a recursos locais?  Verifique se que os clientes saibam como obter a esses recursos. Você pode usar o servidor VPN para rotear solicitações.


## Problemas de conexão de acesso condicional do AD Azure

### Opa - você não consiga acessar isso ainda

-   **Descrição do erro.** Quando a política de acesso condicional não estiver satisfeita, bloqueando a conexão VPN, mas se conecta após o usuário clica em **X** para fechar a mensagem.  Clicar **Okey** faz com que outra tentativa de autenticação, que termina em outra mensagem _Oops_ . Esses eventos são registrados no log de eventos operacionais AAD do cliente. 

-   **Possível causa.** 

    - O usuário tem um certificado de autenticação de cliente válido em seus certificados pessoais armazenar que não foi emitido pelo Azure AD.

    - O perfil VPN \ < TLSExtensions\ > seção está ausente ou não conter não o **\ < EKUName\ > acesso condicional AAD < / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ > \ < EKUName > acesso condicional do AAD < / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ >** entradas. O \ < EKUName > e \ entradas < EKUOID > informam o cliente VPN qual o certificado para recuperar de repositório de certificados do usuário ao passar o certificado para o servidor VPN. Sem isso, o cliente VPN usa qualquer certificado de autenticação de cliente válido é no repositório de certificados do usuário e a autenticação é bem-sucedida. 

    - O servidor RADIUS (NPS) não tiver sido configurado para aceitar somente certificados de cliente que contêm o **Acesso condicional AAD** OID.

-   **Possíveis soluções.** Para que esse loop, faça o seguinte:

    1. No Windows PowerShell, execute o cmdlet **Get-WmiObject** para despejar a configuração de perfil VPN. 
    2. Verifique o **\ < TLSExtensions >**, **\ < EKUName >**, e **\ < EKUOID >** seções existe e mostra o nome correto e OID. 
        ```
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

    3. Para determinar se há certificados válidos no repositório de certificados do usuário, execute o comando **Certutil** :

       ```
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
       >Se um certificado de emissor **CN = geração de autoridade de certificação raiz de VPN da Microsoft 1** está presente no armazenamento pessoal do usuário, mas o usuário obtenha acesso clicando **X** para fechar a mensagem Oops, coletar logs de eventos CAPI2 para verificar se o certificado usado para autenticar foi um certificado de autenticação de cliente válido que não foi emitido da CA raiz VPN da Microsoft.

   4. Se um certificado de autenticação de cliente válido existir no armazenamento pessoal do usuário, a conexão falhará (como deveria) depois que o usuário clica no **X** e se o **\ < TLSExtensions >**, **\ < EKUName >**, e **\ < EKUOID >** seções existirem e contenham as informações corretas.<p>O _não pôde ser encontrado um certificado que pode ser usado com o protocolo de autenticação extensível._ mensagem de erro aparece.

### Não é possível excluir o certificado na folha de conectividade do VPN

-   **Descrição do erro.** Certificados na folha de conectividade VPN não podem ser excluídos.

-   **Possível causa.** O certificado é definido como **principal**.

-   **Possíveis soluções.** 

    1. Na folha conectividade VPN, selecione o certificado.
    2. Em **principal**, selecione **não** e clique em **Salvar**.
    3. Na folha conectividade VPN, selecione o certificado novamente.
    4. Clique em **Excluir**.


---
