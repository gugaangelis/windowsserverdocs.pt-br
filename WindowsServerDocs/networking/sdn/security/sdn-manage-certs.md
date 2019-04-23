---
title: Gerenciar certificados para a rede definida pelo Software
description: Você pode usar este tópico para saber como gerenciar certificados para o controlador de rede Northbound e comunicação Southbound, quando você implanta Software Defined Networking (SDN) no Windows Server 2016 Datacenter.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: 618c2c4da60decc94f84c2a40cd4d2aa80d5f26b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827567"
---
# <a name="manage-certificates-for-software-defined-networking"></a>Gerenciar certificados para a rede definida pelo Software

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber como gerenciar certificados para comunicação Northbound de controlador de rede e Southbound quando você implanta a rede definida pelo Software \(SDN\) no Windows Server 2016 Datacenter e você estiver usando o sistema Center Virtual Machine Manager \(SCVMM\) como seu cliente de gerenciamento de SDN.

>[!NOTE]
>Para obter informações gerais sobre o controlador de rede, consulte [controlador de rede](../technologies/network-controller/Network-Controller.md).

Se você não estiver usando o Kerberos para proteger a comunicação de controlador de rede, você pode usar certificados X.509 para autenticação, autorização e criptografia.

A SDN no Windows Server 2016 Datacenter dá suporte a ambas as self\-assinado e autoridade de certificação \(autoridade de certificação\)-x. 509 certificados autoassinados. Este tópico fornece instruções passo a passo para criar esses certificados e aplicá-las para proteger o controlador de rede Northbound canais de comunicação com clientes de gerenciamento e comunicação Southbound com dispositivos de rede, como o Software Balanceador de carga \(SLB\).
.
Quando você estiver usando o certificado\-com base em autenticação, você deve registrar um certificado em nós de controlador de rede que é usado das seguintes maneiras.

1. Criptografar a comunicação Northbound com Secure Sockets Layer \(SSL\) entre nós de controlador de rede e clientes de gerenciamento, como o System Center Virtual Machine Manager.
2. Autenticação entre nós de controlador de rede e Southbound dispositivos e serviços, como hosts Hyper-V e balanceadores de carga de Software \(SLBs\).

## <a name="creating-and-enrolling-an-x509-certificate"></a>Criando e registrando um certificado X.509

Você pode criar e registrar qualquer um self\-assinada do certificado ou um certificado emitido por uma autoridade de certificação.

>[!NOTE]
>Quando você estiver usando o SCVMM para implantar o controlador de rede, você deve especificar o certificado X.509 que é usado para criptografar a comunicação Northbound durante a configuração do modelo de serviço do controlador de rede.

A configuração do certificado deve incluir os seguintes valores.

- O valor para o **RestEndPoint** caixa de texto deve ser a rede controlador nome totalmente qualificado \(FQDN\) ou o endereço IP. 
- O **RestEndPoint** valor deve corresponder ao nome da entidade \(Common Name, CN\) do certificado X.509.

### <a name="creating-a-self-signed-x509-certificate"></a>Criando um Self\-certificado X.509 assinado

Você pode criar um certificado X.509 autoassinado e exportá-lo com a chave privada \(protegida por senha\) seguindo estas etapas para única\-nó e vários\-implantações de nó do controlador de rede .

Quando você cria self\-assinado certificados, você pode usar as diretrizes a seguir.

- Você pode usar o endereço IP do ponto de extremidade de REST do controlador de rede para o parâmetro DnsName - mas isso não é recomendado porque ele requer que os nós de controlador de rede estão todas localizados em uma sub-rede de gerenciamento único \(, por exemplo, em um único rack\)
- Para várias implantações de nó NC, o nome DNS que você especifique se tornará o FQDN do Cluster de controlador de rede \(Host DNS de registros são criados automaticamente.\) 
- Para implantações de controlador de rede de nó único, o nome DNS pode ser o nome do host do controlador de rede seguido do nome de domínio completo.

#### <a name="multiple-node"></a>De vários nós

Você pode usar o [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando do Windows PowerShell para criar um\-certificado autoassinado.

**Sintaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Exemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Nó único

Você pode usar o [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando do Windows PowerShell para criar um\-certificado autoassinado.

**Sintaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Exemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Criação de uma autoridade de certificação\-certificado X.509 assinado

Para criar um certificado usando uma autoridade de certificação, você já deve ter implantado uma infraestrutura de chave pública \(PKI\) com o Active Directory Certificate Services \(AD CS\). 

>[!NOTE]
>Você pode usar autoridades de certificação de terceiros ou ferramentas, como openssl, para criar um certificado para uso com o controlador de rede, no entanto, as instruções neste tópico são específicas ao AD CS. Para saber como usar uma autoridade de certificação de terceiros ou a ferramenta, consulte a documentação do software que você está usando.

Criar um certificado com uma autoridade de certificação inclui as etapas a seguir.

1. Você ou sua organização domínio ou administrador de segurança configura o modelo de certificado
2. Você ou sua organização administrador de controlador de rede ou administrador do SCVMM solicita um novo certificado de autoridade de certificação.

#### <a name="certificate-configuration-requirements"></a>Requisitos de configuração de certificado

Quando você estiver configurando um modelo de certificado na próxima etapa, certifique-se de que o modelo que você configure inclui os seguintes elementos necessários.

1. O nome de assunto do certificado deve ser o FQDN do host do Hyper-V
2. O certificado deve ser colocado no repositório pessoal do computador local (Meu – cert: \localmachine\my.)
3. O certificado deve ter tanto a servidor de autenticação (EKU: 1.3.6.1.5.5.7.3.1) e autenticação de cliente (EKU: 1.3.6.1.5.5.7.3.2) as políticas de aplicativo.

>[!NOTE]
>Se o pessoal \(meu – cert: \localmachine\my.\) repositório de certificados a Hyper\-host V tem mais de um certificado X.509 com o nome da entidade (CN) como o nome de domínio totalmente qualificado do host \(FQDN\), Certifique-se de que o certificado que será usado por SDN tem uma propriedade de uso avançado de chave personalizada adicional com o OID 1.3.6.1.4.1.311.95.1.1.1. Caso contrário, a comunicação entre o controlador de rede e o host pode não funcionar.

#### <a name="to-configure-the-certificate-template"></a>Para configurar o modelo de certificado
  
>[!NOTE]
>Antes de executar este procedimento, você deve examinar os requisitos de certificado e os modelos de certificado disponíveis no console de modelos de certificado. Você pode modificar um modelo existente ou criar uma duplicata de um modelo existente e, em seguida, modifique sua cópia do modelo. É recomendável criar uma cópia de um modelo existente.

1. No servidor onde o AD CS está instalado, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **autoridade de certificação**. Console de gerenciamento da Microsoft de autoridade de certificação \(MMC\) é aberta. 
2. No MMC, clique duas vezes o nome da autoridade de certificação, clique com botão direito **modelos de certificado**e, em seguida, clique em **gerenciar**.
3. Abre o console de modelos de certificado. Todos os modelos de certificado são exibidos no painel de detalhes.
4. No painel de detalhes, clique no modelo que você deseja duplicar.
5.  Clique o **ação** menu e clique **Duplicar modelo**. O modelo **propriedades** caixa de diálogo é aberta.
6.  No modelo **propriedades** caixa de diálogo do **nome da entidade** , clique em **fornecer na solicitação**. \(Essa configuração é necessária para certificados SSL de controlador de rede.\)
7.  No modelo **propriedades** caixa de diálogo do **tratamento de solicitação** guia, certifique-se de que **permitir que a chave privada seja exportada** está selecionado. Além disso, verifique as **assinatura e criptografia** finalidade foi selecionada.
8.  No modelo **propriedades** caixa de diálogo do **extensões** guia, selecione **uso chave**e, em seguida, clique em **editar**.
9.  Na **assinatura**, certifique-se de que **Assinatura Digital** está selecionado.
10.  No modelo **propriedades** caixa de diálogo do **extensões** guia, selecione **políticas de aplicativo**e, em seguida, clique em **editar**.
11.  Na **políticas de aplicativo**, certifique-se de que **autenticação do cliente** e **autenticação de servidor** estão listados.
12.  Salvar a cópia do modelo de certificado com um nome exclusivo, como **modelo de controlador de rede**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Para solicitar um certificado da autoridade de certificação

Você pode usar o snap-in de certificados para solicitar certificados. Você pode solicitar qualquer tipo de certificado que tenha sido pré-configurado e disponibilizado por um administrador da autoridade de certificação que processa a solicitação de certificado.

**Os usuários** ou local **administradores** é a associação de grupo mínima necessária para concluir este procedimento.

1. Abra o snap-in de certificados para um computador.
2. Na árvore de console, clique em **certificados \(computador Local\)**. Selecione o **pessoais** repositório de certificados.
3. Sobre o **ação** , aponte para * * todas as tarefas * * e clique **Solicitar novo certificado** para iniciar o Assistente de registro de certificado. Clique em **Avançar**.
4. Selecione o **configurado pelo seu administrador** política de registro de certificado e clique em **próxima**.
5. Selecione o **política de registro do Active Directory** \(com base no modelo de autoridade de certificação que você configurou na seção anterior\).
6. Expanda o **detalhes** seção e configurar os itens a seguir.
    1. Certifique-se de que **uso de chave** inclui tanto * * Assinatura Digital * * e **codificação de chave**.
    2. Certifique-se de que **políticas de aplicativo** inclui tanto **autenticação do servidor** \(1.3.6.1.5.5.7.3.1\) e **autenticação de cliente** \(1.3.6.1.5.5.7.3.2\).
7. Clique em **Propriedades**.
8. Sobre o **assunto** guia **nome da entidade**, na **tipo**, selecione **nome comum**. Em valor, especifique **ponto de extremidade de REST de controlador de rede**.
9. Clique em **Aplicar**e clique em **OK**.
10. Clique em **Registrar**.

No MMC de certificados, clique no repositório pessoal para ver o certificado que você registrou da autoridade de certificação.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Exportar e copiar o certificado para a biblioteca SCVMM

Depois de criar qualquer um self\-assinado ou a autoridade de certificação\-assinou o certificado, você deve exportar o certificado com a chave privada \(no formato. pfx\) e sem a chave privada \(no formato. cer de Base 64\) o snap-in de certificados. 

Você deve copiar os dois arquivos exportados para o **ServerCertificate.cr** e **NCCertificate.cr** pastas que você especificou no momento em que você importou o modelo de serviço do NC.

1. Abra o snap-in de certificados (certlm) e localize o certificado no repositório de certificados pessoal do computador local.
2. À direita\-clique no certificado, clique em **todas as tarefas**e, em seguida, clique em **exportar**. O Assistente para Exportação de Certificados é aberto. Clique em **Avançar**.
3. Selecione **Yes**, exportar a opção de chave privada e clique em **próxima**.
4. Escolha **troca de informações pessoais - PKCS #12 (. PFX)** e aceite o padrão **incluem todos os certificados no caminho de certificação** se possível.
5. Atribuir os usuários/grupos e uma senha para o certificado que você está exportando, clique em **próxima**.
6. No arquivo para a página de exportação, procure o local onde você deseja colocar o arquivo exportado e nomeá-lo.
7. Da mesma forma, exporte o certificado no. Formato CER. Observação: Para exportar para o. Formato CER, desmarque a opção Sim, exportar a opção de chave privada.
8. Copiar o. PFX para a pasta ServerCertificate.cr.
9. Copiar o. Arquivo CER para a pasta NCCertificate.cr.

Quando você terminar, atualize essas pastas na biblioteca do SCVMM e certifique-se de que você tem esses certificados copiados. Continue com a configuração do modelo de serviço de controlador de rede e a implantação.

## <a name="authenticating-southbound-devices-and-services"></a>Autenticação de serviços e dispositivos Southbound 

Comunicação de controlador de rede com hosts e dispositivos de SLB MUX usa certificados para autenticação. A comunicação com os hosts é através do protocolo OVSDB enquanto a comunicação com os dispositivos de SLB MUX é através do protocolo do WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Comunicação de Host Hyper-V com o controlador de rede

Para comunicação com os hosts do Hyper-V sobre OVSDB, controlador de rede precisa apresentar um certificado para as máquinas de host. Por padrão, o SCVMM selecionará o certificado SSL configurado no controlador de rede e usa-o para a comunicação southbound com hosts.

Que é o motivo por que o certificado SSL deve ter o EKU de autenticação de cliente configurado. Esse certificado é configurado em "Servidores" REST resource \(hosts Hyper-V são representados no controlador de rede como um recurso de servidor\)e podem ser exibidas ao executar o comando do Windows PowerShell  **Get-NetworkControllerServer**.

A seguir está um exemplo parcial do recurso REST do servidor.

      "resourceId": "host31.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "host31.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

Para autenticação mútua, o host Hyper-V também deve ter um certificado para se comunicar com o controlador de rede. 

Você pode registrar o certificado de uma autoridade de certificação \(autoridade de certificação\). Se um certificado de autoridade de certificação com base não for encontrado no computador host, o SCVMM cria um certificado autoassinado e provisiona a ele na máquina host.

Controlador de rede e os certificados de host do Hyper-V devem ser de confiança entre si. Certificado de raiz do certificado de host do Hyper-V deve estar presente no repositório de rede controlador raiz autoridades de certificação confiáveis para o computador Local e vice-versa. 

Quando você estiver usando self\-assinado certificados, SCVMM garante que os certificados necessários estão presentes no repositório de autoridades de certificação raiz confiáveis para o computador Local. 

Se você estiver usando certificados de autoridade de certificação com base para os hosts do Hyper-V, você precisa garantir que o certificado da AC raiz está presente no repositório de autoridades de certificação raiz confiáveis do controlador de rede para o computador Local.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Software de Balanceador de carga MUX a comunicação com o controlador de rede

O Multiplexador de Balanceador de carga de Software \(MUX\) e controlador de rede se comunicar através do protocolo do WCF, usando certificados para autenticação.

Por padrão, o SCVMM selecionará o certificado SSL configurado no controlador de rede e usa para comunicação southbound com os dispositivos de Mux. Esse certificado é configurado em "NetworkControllerLoadBalancerMux" REST de recursos e podem ser exibidas ao executar o cmdlet do Powershell **Get-NetworkControllerLoadBalancerMux**.

Exemplo de recurso REST MUX \(parcial\):

      "resourceId": "slbmux1.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "slbmux1.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

Para autenticação mútua, você também deve ter um certificado nos dispositivos SLB MUX. Esse certificado é automaticamente configurado pelo SCVMM, quando você implanta o balanceador de carga de software usando o SCVMM.

>[!IMPORTANT]
>No host e nós SLB, é essencial que o repositório de certificados de autoridades de certificação raiz confiáveis não inclui qualquer certificado onde "Emitido para" não é igual a "Emitido por". Se isso ocorrer, a comunicação entre o controlador de rede e o dispositivo southbound falhará.

Controlador de rede e os certificados de SLB MUX devem ser de confiança entre si \(certificado de raiz do certificado SLB MUX deve estar presente no computador do controlador de rede de repositório de autoridades de certificação raiz confiáveis e vice-versa\). Quando você estiver usando self\-assinado certificados, SCVMM garante que os certificados necessários estão presentes no em autoridades de certificação raiz confiáveis armazenar para o computador Local.



