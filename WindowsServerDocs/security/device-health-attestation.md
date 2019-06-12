---
title: Atestado de integridade de dispositivo
H1: NA
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e7b77a4-1c6a-4c21-8844-0df89b63f68d
author: brianlic-msft
ms.date: 10/12/2016
ms.openlocfilehash: 7c2d7113847cc44f18c5234502b58becde1dcb9f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446515"
---
# <a name="device-health-attestation"></a>Atestado de integridade de dispositivo

>Aplica-se a: Windows Server 2016

Introduzido no Windows 10, versão 1507, o Atestado de Integridade do Dispositivo (DHA) incluía o seguinte:

-   Integra-se à estrutura de Gerenciamento de Dispositivo Móvel (MDM) do Windows 10 em alinhamento com os [padrões Open Mobile Alliance (OMA)](http://openmobilealliance.org/).

-   Oferece suporte a dispositivos que têm um Trusted Module Platform (TPM) provisionado em um formato discreto ou firmware.

-   Habilita as empresas a elevarem o nível de segurança de suas organizações para hardwares monitorados e segurança atestada, com pouco ou nenhum impacto no custo da operação.

A partir do Windows Server 2016, é possível executar o serviço de DHA como uma função de servidor na sua organização. Use este tópico para aprender a instalar e configurar a função de servidor Atestado de Integridade do Dispositivo.

## <a name="overview"></a>Visão geral

Você pode usar o DHA para avaliar a integridade do dispositivo para:
  
-   Dispositivos Windows 10 e Windows 10 Mobile que oferecem suporte a TPM 1.2 ou 2.0.  
-   Dispositivos locais que são gerenciados por meio do Active Directory com acesso à Internet, dispositivos que são gerenciados por meio do Active Directory sem acesso à Internet, dispositivos gerenciados pelo Azure Active Directory ou uma implantação híbrida usando o Active Directory e o Azure Active Directory.


### <a name="dha-service"></a>Serviço de DHA

O serviço de DHA valida os logs de TPM e PCR para um dispositivo e, em seguida, emite um relatório do DHA. A Microsoft oferece o serviço de DHA de três maneiras:

- **Serviço de nuvem de DHA** Um serviço de DHA gerenciado pela Microsoft que é gratuito, balanceado para carga geográfica e otimizado para acesso de diferentes regiões do mundo.

- **Serviço local de DHA** Uma nova função de servidor introduzida no Windows Server 2016. Ele está disponível gratuitamente para os clientes que têm uma licença do Windows Server 2016.

- **Serviço de nuvem do Azure de DHA** Um host virtual no Microsoft Azure. Para fazer isso, você precisa de um host virtual e licenças para o serviço local de DHA.

O serviço de DHA integra-se com soluções do MDM e fornece o seguinte: 

-   Combine as informações que recebem dos dispositivos (por meio de canais de comunicação de gerenciamento de dispositivo existentes) com o relatório de DHA
-   Tome uma decisão de segurança mais segura e confiável, com base em atestado de hardware e dados protegidos

Aqui está um exemplo que mostra como você pode usar o DHA para ajudar a elevar o nível de proteção de segurança para os ativos da sua organização.

1. Crie uma política que verifica os seguintes atributos/configuração de inicialização:
   - Inicialização Segura
   - BitLocker
   - ELAM
2. A solução MDM impõe essa política e dispara uma ação corretiva com base nos dados do relatório de DHA.  Por exemplo, ela pode verificar o seguinte:
   - A Inicialização Segura foi habilitada, o dispositivo carregou um código confiável autêntico e o carregador de inicialização do Windows não foi violado.
   - A Inicialização Confiável verificou com sucesso a assinatura digital do kernel do Windows e os componentes que foram carregados enquanto o dispositivo foi iniciado.
   - A Inicialização Medida criou uma trilha de auditoria protegida por TPM que pode ser verificada remotamente.
   - O BitLocker foi habilitado e protegeu os dados quando o dispositivo foi desativado.
   - O ELAM foi habilitado nos primeiros estágios de inicialização e está monitorando o tempo de execução.
  
#### <a name="dha-cloud-service"></a>Serviço de nuvem de DHA

O serviço de nuvem de DHA oferece os seguintes benefícios:

-   Analisa os logs de inicialização de dispositivo TCG e PCR que recebe de um dispositivo registrado com uma solução de MDM. 
-   Cria um relatório de inviolabilidade ou resistência a ataques (relatório de DHA) que descreve como o dispositivo foi iniciado com base nos dados coletados e protegidos pelo chip de TPM de um dispositivo. 
-   Envia o relatório de DHA para o servidor de MDM que solicitou o relatório em um canal de comunicação protegido.

#### <a name="dha-on-premises-service"></a>Serviço local de DHA

O serviço local de DHA oferece todos os recursos que são oferecidos pelo serviço de nuvem de DHA.  Também permite que os clientes:

 - Otimizem o desempenho executando o serviço de DHA em seu próprio data center
 - Garantam que o relatório de DHA não saia da sua rede

#### <a name="dha-azure-cloud-service"></a>Serviço de nuvem DHA do Azure

Este serviço fornece a mesma funcionalidade que o serviço local de DHA, exceto que o serviço de nuvem DHA do Azure é executado como um host virtual no Microsoft Azure.

### <a name="dha-validation-modes"></a>Modos de validação de DHA

Você pode configurar o serviço local de DHA para ser executado no modo de validação EKCert ou AIKCert. Quando o serviço de DHA emite um relatório, indica se ele foi emitido no modo de validação AIKCert ou EKCert. Os modos de validação EKCert e AIKCert oferecem a mesma garantia de segurança, desde que a cadeia de EKCert de confiança seja mantida atualizada.

#### <a name="ekcert-validation-mode"></a>Modo de validação EKCert

O modo de validação EKCert é otimizado para dispositivos em organizações que não estão conectados à Internet. Os dispositivos que se conectam a um serviço de DHA em execução no modo de validação EKCert **não** têm acesso direto à Internet.

Quando o DHA está em execução no modo de validação EKCert, ele depende de uma cadeia de confiança gerenciada da empresa que precisa ser atualizada ocasionalmente (aproximadamente de 5 a 10 vezes por ano). 

A Microsoft publica pacotes agregados de raízes confiáveis e autoridade de certificação intermediárias para fabricantes de TPM aprovados (quando estiverem disponíveis) em um arquivo morto acessível publicamente no arquivo .cab. Você precisa baixar o feed, validar sua integridade e instalá-lo no servidor que executa o Atestado de Integridade do Dispositivo.

Um arquivo de exemplo [ https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab ](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab).

#### <a name="aikcert-validation-mode"></a>Modos de validação de AIKCert

O modo de validação AIKCert é otimizado para ambientes operacionais que têm acesso à Internet. Os dispositivos que se conectam a um serviço DHA em execução no modo de validação AIKCert devem ter acesso direto à Internet e conseguem obter um certificado de AIK da Microsoft. 

## <a name="install-and-configure-the-dha-service-on-windows-server-2016"></a>Instalar e configurar o serviço DHA no Windows Server 2016

Use as seções a seguir para obter o DHA instalado e configurado no Windows Server 2016.

### <a name="prerequisites"></a>Pré-requisitos

Para configurar e verificar um serviço local de DHA, você precisa de:

- Um servidor executando o Windows Server 2016.
- Um (ou mais) dispositivos clientes do Windows 10 com um TPM (1.2 ou 2.0) em um estado limpo/pronto executando a compilação mais recente do Windows Insider.
- Decida se você pretende executar em modo de validação EKCert ou AIKCert.
- Os certificados a seguir:
  - **Certificado SSL DHA** Um certificado SSL x.509 que liga-se a uma raiz confiável corporativa com uma chave privada exportável. Esse certificado protege as comunicações de dados de DHA em trânsito incluindo comunicações de servidores para servidores (serviço de DHA e servidor MDM) e servidor para cliente (serviço de DHA e um dispositivo com Windows 10).
  - **Certificado de autenticação de DHA** Um certificado x.509 que liga-se a uma raiz confiável corporativa com uma chave privada exportável. O serviço de DHA usa esse certificado para autenticação digital. 
  - **Certificado de criptografia de DHA** Um certificado x.509 que encadeia para uma raiz confiável corporativa com uma chave privada exportável. O serviço de DHA também usa esse certificado para criptografia. 


### <a name="install-windows-server-2016"></a>Instalar o Windows Server 2016

Instale o Windows Server 2016 usando o método preferencial de instalação, como os serviços de implantação do Windows ou executando o instalador da mídia inicializável, da unidade USB ou do sistema de arquivos local. Se esta for a primeira vez que você está configurando o serviço local de DHA, deverá instalar o Windows Server 2016 usando a opção de instalação **Experiência Desktop**.

### <a name="add-the-device-health-attestation-server-role"></a>Adicionar a função de servidor Atestado de Integridade do Dispositivo

Você pode instalar a função de servidor Atestado de Integridade do Dispositivo e suas dependências usando o Gerenciador do Servidor. 

Depois de instalar o Windows Server 2016, o dispositivo é reiniciado e abre o Gerenciador do Servidor. Se o Gerenciador do Servidor não for iniciado automaticamente, clique em **Iniciar** e em **Gerenciador do Servidor**.

1.  Clique em **Adicionar funções e recursos**.
2.  Na página **Antes de começar** , clique em **Avançar**.
3.  Na página **Selecionar tipo de instalação**, clique em **Instalação baseada em função ou recurso** e em **Avançar**.
4.  Na página **Selecionar servidor de destino**, clique em **Selecionar um servidor no pool de servidores**, escolha o servidor e clique em **Avançar**.
5.  Na página **Selecionar funções de servidor**, marque a caixa de seleção **Atestado de Integridade do Dispositivo**.
6.  Clique em **Adicionar Recursos** para instalar outros serviços de função e recursos necessários.
7.  Clique em **Avançar**.
8.  Na página **Selecionar recursos**, clique em **Avançar**.
9.  Na página **Função de Servidor Web (IIS)** , clique em **Avançar**.
10. Na página **Selecionar serviços de função**, clique em **Avançar**.
11. Na página **Serviço de Atestado de Integridade do Dispositivo**, clique em **Avançar**.
12. Na página **Confirmar seleções de instalação** , clique em **Instalar**.
13. Quando a instalação estiver concluída, clique em **Fechar**.

### <a name="install-the-signing-and-encryption-certificates"></a>Instalar os certificados de autenticação e criptografia

Use o seguinte script do Windows PowerShell para instalar os certificados de autenticação e criptografia. Para obter mais informações sobre a impressão digital, consulte [como: Recuperar a impressão digital de um certificado](https://msdn.microsoft.com/library/ms734695.aspx).

```
$key = Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.Thumbprint -like "<thumbprint>"}
$keyname = $key.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
$keypath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $keyname
icacls $keypath /grant <username>`:R
  
#<thumbprint>: Certificate thumbprint for encryption certificate or signing certificate
#<username>: Username for web service app pool, by default IIS_IUSRS
```

### <a name="install-the-trusted-tpm-roots-certificate-package"></a>Instalar o pacote de certificado raiz confiável de TPM

Para instalar o pacote de certificado raiz confiável de TPM, é preciso extraí-lo, remover todas as cadeias confiáveis que não são confiáveis para sua organização e, em seguida, executar setup.cmd.

#### <a name="download-the-trusted-tpm-roots-certificate-package"></a>Baixar o pacote de certificado raiz confiável de TPM

Antes de instalar o pacote de certificado, você pode baixar a lista mais recente de raízes confiáveis de TPM [ https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab ](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab).

> **Importante:** Antes de instalar o pacote, verifique se que ele é assinado digitalmente pela Microsoft.

#### <a name="extract-the-trusted-certificate-package"></a>Extrair o pacote de certificado confiável
Extraia o pacote de certificado confiável executando os comandos a seguir.
```
mkdir .\TrustedTpm
expand -F:* .\TrustedTpm.cab .\TrustedTpm
```

#### <a name="remove-the-trust-chains-for-tpm-vendors-that-are-not-trusted-by-your-organization-optional"></a>Remova as cadeias de confiança para fornecedores de TPM que *não* são confiáveis para sua organização (opcional)

Exclua as pastas de quaisquer cadeias de confiança de fornecedor de TPM que não são confiáveis para sua organização.

> **Observação:** Se usar o modo de certificado AIK, a pasta Microsoft é necessária para validar certificados AIK emitidos pela Microsoft.

#### <a name="install-the-trusted-certificate-package"></a>Instalar o pacote de certificado confiável
Instale o pacote de certificado confiável executando o script de instalação do arquivo .cab.

```
.\setup.cmd
```

### <a name="configure-the-device-health-attestation-service"></a>Configurar o serviço de Atestado de Integridade do Dispositivo

Você pode usar o Windows PowerShell para configurar o serviço local de DHA.

```
Install-DeviceHealthAttestation -EncryptionCertificateThumbprint <encryption> -SigningCertificateThumbprint <signing> -SslCertificateStoreName My -SslCertificateThumbprint <ssl> -SupportedAuthenticationSchema "<schema>"

#<encryption>: Thumbprint of the encryption certificate
#<signing>: Thumbprint of the signing certificate
#<ssl>: Thumbprint of the SSL certificate
#<schema>: Comma-delimited list of supported schemas including AikCertificate, EkCertificate, and AikPub
```

### <a name="configure-the-certificate-chain-policy"></a>Configurar a política da cadeia de certificados

Configure a política de cadeia de certificados executando o script do Windows PowerShell a seguir.

```
$policy = Get-DHASCertificateChainPolicy
$policy.RevocationMode = "NoCheck"
Set-DHASCertificateChainPolicy -CertificateChainPolicy $policy
```

## <a name="dha-management-commands"></a>Comandos de gerenciamento de DHA

Apresentamos a seguir alguns exemplos do Windows PowerShell que podem ajudá-lo a gerenciar o serviço de DHA.

### <a name="configure-the-dha-service-for-the-first-time"></a>Configurar o serviço de DHA pela primeira vez

```
Install-DeviceHealthAttestation -SigningCertificateThumbprint "<HEX>" -EncryptionCertificateThumbprint "<HEX>" -SslCertificateThumbprint "<HEX>" -Force
```

### <a name="remove-the-dha-service-configuration"></a>Remover a configuração do serviço de DHA

```
Uninstall-DeviceHealthAttestation -RemoveSslBinding -Force
```
### <a name="get-the-active-signing-certificate"></a>Obter o certificado de autenticação ativo

```
Get-DHASActiveSigningCertificate
```
### <a name="set-the-active-signing-certificate"></a>Definir o certificado de autenticação ativo

```
Set-DHASActiveSigningCertificate -Thumbprint "<hex>" -Force
```

> **Observação:** Esse certificado deve ser implantado no servidor que executa o serviço DHA **LocalMachine\My** repositório de certificados. Quando o certificado de autenticação ativo estiver definido, o certificado de autenticação ativo existente será movido para a lista de certificados de autenticação inativos.

### <a name="list-the-inactive-signing-certificates"></a>Listar os certificados de autenticação inativos
```
Get-DHASInactiveSigningCertificates
```

### <a name="remove-any-inactive-signing-certificates"></a>Remover os certificados de autenticação inativos
```
Remove-DHASInactiveSigningCertificates -Force
Remove-DHASInactiveSigningCertificates  -Thumbprint "<hex>" -Force
```

> **Observação:** Somente *um* certificado inativo (de qualquer tipo) pode existir no serviço a qualquer momento. Os certificados devem ser removidos da lista de certificados inativos depois que não forem mais necessários.

### <a name="get-the-active-encryption-certificate"></a>Obter o certificado de criptografia ativo

```
Get-DHASActiveEncryptionCertificate
```

### <a name="set-the-active-encryption-certificate"></a>Definir o certificado de criptografia ativo

```
Set-DHASActiveEncryptionCertificate -Thumbprint "<hex>" -Force
```

O certificado deve ser implantado no servidor no repositório de certificados **LocalMachine\My**. 

Quando o certificado de criptografia ativo estiver definido, o certificado de criptografia ativo existente será movido para a lista de certificados de criptografia inativos.

### <a name="list-the-inactive-encryption-certificates"></a>Listar os certificados de criptografia inativos

```
Get-DHASInactiveEncryptionCertificates
```
### <a name="remove-any-inactive-encryption-certificates"></a>Remover os certificados de criptografia inativos

```
Remove-DHASInactiveEncryptionCertificates -Force
Remove-DHASInactiveEncryptionCertificates -Thumbprint "<hex>" -Force 
```

### <a name="get-the-x509chainpolicy-configuration"></a>Obter a configuração de X509ChainPolicy 

```
Get-DHASCertificateChainPolicy
```
### <a name="change-the-x509chainpolicy-configuration"></a>Alterar a configuração de X509ChainPolicy

```
$certificateChainPolicy = Get-DHASInactiveEncryptionCertificates
$certificateChainPolicy.RevocationFlag = <X509RevocationFlag>
$certificateChainPolicy.RevocationMode = <X509RevocationMode>
$certificateChainPolicy.VerificationFlags = <X509VerificationFlags>
$certificateChainPolicy.UrlRetrievalTimeout = <TimeSpan>
Set-DHASCertificateChainPolicy = $certificateChainPolicy
```

## <a name="dha-service-reporting"></a>Relatórios do serviço de DHA

Apresentamos a seguir uma lista de mensagens que são relatadas pelo serviço de DHA para a solução MDM: 

- **200** HTTP OK. O certificado é retornado.
- **400** Solicitação inválida. Formato de solicitação inválido, certificado de integridade inválido, a assinatura do certificado não coincide, blob de atestado de integridade inválido ou blob de status de integridade inválido. A resposta também contém uma mensagem, conforme descrito pelo esquema de resposta, com um código de erro e uma mensagem de erro que podem ser usadas para diagnósticos.
- **500** Erro Interno do Servidor. Isso pode ocorrer se houver problemas que impedem o serviço de emitir certificados.
- **503** A limitação está rejeitando as solicitações para impedir a sobrecarga do servidor. 
