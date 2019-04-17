---
title: Gerenciar certificados de Software definidos rede
description: Você pode usar este tópico para saber como gerenciar certificados de controlador em sentido norte da rede e comunicações Southbound quando você implanta Software definido de rede (SDN) no Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3036de9161cabc2f3a85a1d3b2ce7739f0ff6bd3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-for-software-defined-networking"></a>Gerenciar certificados de Software definidos rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como gerenciar certificados de controlador em sentido norte da rede e comunicações Southbound quando você implanta \(SDN\) Software de rede definidos no Windows Server 2016 Datacenter e você estiver usando o System Center Virtual Machine Manager \(SCVMM\) como seu cliente de gerenciamento SDN.

>[!NOTE]
>Para obter informações gerais sobre o controlador de rede, consulte [rede controlador](../technologies/network-controller/Network-Controller.md).

Se você não estiver usando Kerberos para proteger a comunicação de controlador de rede, você pode usar certificados x. 509 para autenticação, autorização e criptografia.

SDN no Windows Server 2016 Datacenter dá suporte a ambas self\ assinado e autoridade de certificação \ (CA\)-x. 509 certificados assinados. Este tópico fornece instruções passo a passo para criar esses certificados e aplicá-las para proteger os canais de comunicação de rede controlador em sentido norte com clientes de gerenciamento e comunicações Southbound com dispositivos de rede, como o \(SLB\) balanceador de Software.
.
Quando você estiver usando autenticação baseada em certificate\, você deve registrar um certificado em nós de controlador de rede que é usado das seguintes maneiras.

1. Criptografar a comunicação em sentido norte com Secure Sockets Layer \(SSL\) entre nós de controlador de rede e clientes de gerenciamento, como o System Center Virtual Machine Manager.
2. Autenticação entre nós de controlador de rede e Southbound dispositivos e serviços, como hosts do Hyper-V e \(SLBs\) balanceadores de carga de Software.

## <a name="creating-and-enrolling-an-x509-certificate"></a>Criando e registrando-se um certificado x. 509

Você pode criar e registrar um certificado assinado self\ ou um certificado emitido por uma autoridade de certificação.

>[!NOTE]
>Quando você estiver usando SCVMM para implantar o controlador de rede, você deve especificar o certificado x. 509 que é usado para criptografar a comunicação em sentido norte durante o passo de configuração do modelo de serviço do controlador de rede.

A configuração do certificado deve incluir os seguintes valores.

- O valor para o **RestEndPoint** caixa de texto deve ser a rede controlador nome totalmente qualificado domínio \(FQDN\) ou o endereço IP. 
- O **RestEndPoint** valor deve corresponder ao nome do assunto \ (nome comum, CN\) do certificado x. 509.

### <a name="creating-a-self-signed-x509-certificate"></a>Criação de um certificado x. 509 assinado Self\

Você pode criar um certificado x. 509 autoassinado e exportá-la com a chave privada \ (protegido com uma password\) seguindo estas etapas para implantações de nó single\ e nós multiple\ do controlador de rede.

Quando você cria certificados assinados self\, você pode usar as diretrizes a seguir.

- Você pode usar o endereço IP do ponto de extremidade de REST de controlador de rede para o parâmetro DnsName - mas isso não é recomendável porque isso requer que os nós de controlador de rede estão localizados em uma sub-rede gerenciamento único \ (por exemplo, em um único rack\)
- Para implantações de vários nó NC, o nome DNS que você especificar se tornará o FQDN do Cluster controlador rede \ (registros de Host DNS são criados automaticamente. \) 
- Para implantações de controlador de rede único nó, o nome DNS pode ser o nome do host do controlador de rede seguido pelo nome do domínio completo.

#### <a name="multiple-node"></a>Nó múltiplo

Você pode usar o [nova SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando do Windows PowerShell para criar um certificado assinado self\.

**Sintaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Exemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Único nó

Você pode usar o [nova SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando do Windows PowerShell para criar um certificado assinado self\.

**Sintaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Exemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Criação de um certificado x. 509 assinado CA\

Para criar um certificado usando uma autoridade de certificação, você já deve ter implantado \(PKI\) uma infraestrutura de chave pública com serviços de certificados do Active Directory \(AD CS\). 

>[!NOTE]
>Você pode usar autoridades de certificação de terceiros ou ferramentas, como openssl, para criar um certificado para uso com o controlador de rede, mas as instruções neste tópico são específicas para o AD CS. Para saber como usar uma autoridade de certificação de terceiros ou a ferramenta, consulte a documentação do software que você está usando.

Criando um certificado com uma autoridade de certificação inclui as seguintes etapas.

1. Você ou sua organização domínio ou administrador de segurança configura o modelo de certificado
2. Você ou sua organização controlador administrador da rede ou administrador SCVMM solicita um novo certificado da CA.

#### <a name="certificate-configuration-requirements"></a>Requisitos de configuração de certificado

Enquanto você estiver configurando um modelo de certificado na próxima etapa, certifique-se de que o modelo que você configurar inclui os seguintes elementos necessários.

1. Nome do requerente do certificado deve ser o FQDN do host do Hyper-V
2. O certificado deve ser colocado no armazenamento pessoal do computador local (Meu – cert: \localmachine\my)
3. O certificado deve ter as duas autenticação de servidor (EKU: 1.3.6.1.5.5.7.3.1) e a autenticação de cliente (EKU: 1.3.6.1.5.5.7.3.2) políticas de aplicativos.

>[!NOTE]
>Se o pessoal \ (Meu – cert: \localmachine\my\) certificado loja no host Hyper\-V tem x. 509 mais de um certificado com o nome do assunto (CN) como o nome de domínio totalmente qualificado \(FQDN\) host, certifique-se de que o certificado que será usado pelo SDN tem uma propriedade de uso avançado de chave personalizada adicional com o OID 1.3.6.1.4.1.311.95.1.1.1. Caso contrário, a comunicação entre o host e o controlador de rede talvez não funcionem.

#### <a name="to-configure-the-certificate-template"></a>Para configurar o modelo de certificado
  
>[!NOTE]
>Antes de executar este procedimento, você deve examinar os requisitos de certificado e os modelos de certificado disponíveis no console de modelos de certificado. Você pode modificar um modelo existente ou criar uma cópia de um modelo existente e, em seguida, modifique sua cópia do modelo. É recomendável criar uma cópia de um modelo existente.

1. No servidor onde AD CS é instalado, no Gerenciador do servidor, clique em **ferramentas**e clique em **autoridade de certificação**. O Console de gerenciamento do Microsoft de autoridade de certificação \(MMC\) é aberta. 
2. No MMC, clique duas vezes o nome de autoridade de certificação, clique com botão direito **modelos de certificado**e clique em **gerenciar**.
3. O console de modelos de certificado é aberto. Todos os modelos de certificado são exibidos no painel de detalhes.
4. No painel de detalhes, clique no modelo que você deseja duplicar.
5.  Clique no **ação** menu e, em seguida, clique **modelo duplicado**. O modelo **propriedades** caixa de diálogo é aberta.
6.  No modelo **propriedades** caixa de diálogo, pela **nome do requerente** , clique em **fornecer na solicitação**. \ (Essa configuração é necessária para certificados SSL do controlador de rede. \)
7.  No modelo **propriedades** caixa de diálogo, pela **tratamento de solicitação** guia, certifique-se de que **permitir que a chave privada seja exportada** é selecionado. Também verifique o **assinatura e criptografia** finalidade é selecionada.
8.  No modelo **propriedades** caixa de diálogo, pela **extensões** , selecione **uso da chave**e clique em **editar**.
9.  Em **assinatura**, certifique-se de que **Assinatura Digital** é selecionado.
10.  No modelo **propriedades** caixa de diálogo, pela **extensões** , selecione **políticas de aplicativo**e clique em **editar**.
11.  Em **políticas de aplicativo**, certifique-se de que **autenticação de cliente** e **autenticação de servidor** estão listados.
12.  Salvar a cópia do modelo de certificado com um nome exclusivo, como **modelo controlador de rede**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Para solicitar um certificado da CA

Você pode usar o snap-in de certificados para solicitar certificados. Você pode solicitar qualquer tipo de certificado que tenha sido pré-configurado e disponibilizado por um administrador da autoridade de certificação que processa a solicitação de certificado.

**Os usuários** local ou **administradores** é a associação de grupo mínima necessária para concluir este procedimento.

1. Abra o snap-in de certificados para um computador.
2. Na árvore de console, clique em **certificados \(Local Computer\)**. Selecione o **pessoais** repositório de certificados.
3. Sobre o **ação** , aponte para * * todas as tarefas * * e clique **Solicitar novo certificado** para iniciar o Assistente de registro de certificado. Clique em **próxima**.
4. Selecione o **configurado pelo seu administrador** política de registro de certificado e clique em **próxima**.
5. Selecione o **diretiva de registro do Active Directory** \ (com base no modelo de autoridade de certificação que você configurou no section\ a anterior).
6. Expanda o **detalhes** seção e configurar os itens a seguir.
    1. Certifique-se de que **chave uso** inclui tanto * * Assinatura Digital * * e **codificação de chave**.
    2. Certifique-se de que **políticas de aplicativo** inclui tanto **autenticação de servidor** \(1.3.6.1.5.5.7.3.1\) e **autenticação de cliente** \(1.3.6.1.5.5.7.3.2\).
7. Clique em **propriedades**.
8. Sobre o **assunto** guia, **nome do requerente**, na **tipo**, selecione **nome comum**. Em valor, especifique **o ponto de extremidade de REST de controlador de rede**.
9. Clique em **aplicar**e clique em **Okey**.
10. Clique em **registrar**.

No MMC certificados, clique em armazenamento pessoal para exibir o certificado que você tenha registrado da autoridade de certificação.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Exportando e copiar o certificado para a biblioteca SCVMM

Depois de criar um certificado assinado self\ ou CA\ assinado, você deve exportar o certificado com a chave privada \(in.pfx format\) e sem a chave privada \ (em Base 64. cer format\) o snap-in de certificados. 

Em seguida, você deve copiar os dois arquivos exportados para o **ServerCertificate.cr** e **NCCertificate.cr** pastas que você especificou no momento quando você importou o modelo de serviço NC.

1. Abra o snap-in de certificados (certlm.msc) e localize o certificado no repositório de certificados pessoais no computador local.
2. Clique Right\ o certificado, clique em **todas as tarefas**e clique em **exportar**. Abre o Assistente de exportação de certificado. Clique em **próxima**.
3. Selecione **Sim**, exporte a opção de chave privada, clique em **próxima**.
4. Escolha **troca de informações pessoais - PKCS #12 (. PFX)** e aceitar o padrão para **incluir todos os certificados no caminho de certificação** se possível.
5. Atribuir os grupos de usuários/e uma senha para o certificado estiver exportando, clique em **próxima**.
6. No arquivo para Exportar página, procure o local onde você quer colocar o arquivo exportado e nomeá-lo.
7. Da mesma forma, exporte o certificado no. Formato CER. Observação: Para exportar para. Formato CER, desmarque o Sim, exportar a opção de chave privada.
8. Copiar o. PFX para a pasta ServerCertificate.cr.
9. Copiar o. Arquivo CER para a pasta NCCertificate.cr.

Quando terminar, atualizar essas pastas da biblioteca de SCVMM e certifique-se de que você tenha esses certificados copiados. Continue com a configuração de modelo de serviço de controlador de rede e a implantação.

## <a name="authenticating-southbound-devices-and-services"></a>Autenticação de serviços e dispositivos Southbound 

Comunicação de controlador de rede com hosts e dispositivos de SLB MUX usa certificados para autenticação. Comunicação com os hosts é via protocolo OVSDB enquanto comunicação com os dispositivos SLB MUX está sobre o protocolo WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Comunicação de Host do Hyper-V com o controlador de rede

Para comunicação com os hosts Hyper-V sobre OVSDB, controlador de rede deve apresentar um certificado para os computadores host. Por padrão, SCVMM pega o certificado SSL configurado no controlador de rede e a usa para comunicação southbound com os hosts.

Isso é o motivo por que o certificado SSL deve ter o EKU de autenticação de cliente configurado. Esse certificado é configurado em REPOUSO "servidores" recurso \ (hosts Hyper-V são representados no controlador de rede como um servidor resource\) e podem ser visualizados executando o comando do Windows PowerShell **Get-NetworkControllerServer**.

A seguir está um exemplo parcial do servidor recurso REST.

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

Para autenticação mútua, o host do Hyper-V também deve ter um certificado para se comunicar com o controlador de rede. 

Você pode registrar o certificado de \(CA\) uma autoridade de certificação. Se não for encontrado um certificado de autoridade de certificação com base no computador host, SCVMM cria um certificado autoassinado e provisiona-lo no computador host.

Controlador de rede e os certificados de host do Hyper-V devem ser confiável por uns aos outros. Certificado de raiz do certificado de host do Hyper-V deve estar presente na rede controlador autoridades de certificação confiáveis armazenar para o computador Local e vice-versa. 

Quando você estiver usando certificados assinados self\, SCVMM garante que os certificados necessários estão presentes no repositório autoridades de certificação confiáveis para o computador Local. 

Se você estiver usando certificados de autoridade de certificação com base para os hosts Hyper-V, você precisa garantir que o certificado da CA raiz está presente no repositório de autoridades de certificação confiáveis do controlador de rede para o computador Local.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Software Load balanceador Multiplexador comunicação com o controlador de rede

O Software Load balanceador multiplexador parelalas \(MUX\) e o controlador de rede se comunicar por meio do protocolo WCF, uso de certificados para autenticação.

Por padrão, SCVMM pega o certificado SSL configurado no controlador de rede e a usa para comunicação southbound com os dispositivos de multiplexador. Esse certificado é configurado no "NetworkControllerLoadBalancerMux" resto do recurso e podem ser visualizados executando o cmdlet do Powershell **Get-NetworkControllerLoadBalancerMux**.

Exemplo de Multiplexador REST recurso \(partial\):

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

Para autenticação mútua, você também deve ter um certificado nos dispositivos SLB MUX. Esse certificado é configurado automaticamente pelo SCVMM quando você implanta balanceador software usando SCVMM.

>[!IMPORTANT]
>No host e nós SLB, é fundamental que o repositório de certificados de autoridades de certificação confiáveis não inclui qualquer certificado onde "Emitido para" não é igual a "Emitido por". Se isso ocorrer, a comunicação entre o controlador de rede e o dispositivo southbound falha.

Controlador de rede e os certificados SLB MUX devem ser confiável por uns aos outros \ (certificado de raiz do certificado SLB MUX deve estar presente na máquina de controlador de rede, armazenar e vice-versa versa\ autoridades de certificação raiz confiável). Quando você estiver usando certificados assinados self\, SCVMM garante que os certificados necessários estão presentes no em autoridades de certificação raiz confiáveis armazenar para o computador Local.



