---
title: Gerenciar certificados para rede definida pelo software
description: Você pode usar este tópico para aprender a gerenciar certificados para as comunicações Northbound e Southbound do controlador de rede ao implantar SDN (rede definida pelo software) no Windows Server 2016 datacenter.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: b1cff080630c68ee8c4b7f0904f8fd0978330edc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405991"
---
# <a name="manage-certificates-for-software-defined-networking"></a>Gerenciar certificados para rede definida pelo software

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para aprender a gerenciar certificados para as comunicações Northbound e Southbound do controlador de rede ao implantar o Sdn \(\) de rede definido pelo software no Windows Server 2016 datacenter e estiver usando o sistema Central Virtual Machine Manager \(SCVMM\) como seu cliente de gerenciamento de Sdn.

>[!NOTE]
>Para obter informações gerais sobre o controlador de rede, consulte [controlador de rede](../technologies/network-controller/Network-Controller.md).

Se você não estiver usando o Kerberos para proteger a comunicação do controlador de rede, poderá usar certificados X. 509 para autenticação, autorização e criptografia.

O\-Sdn no Windows Server 2016 datacenter dá suporte a certificados autoassinados\)e de autoridade \(de certificação X. 509 assinados por AC. Este tópico fornece instruções passo a passo para criar esses certificados e aplicá-los para proteger os canais de comunicação Northbound do controlador de rede com clientes de gerenciamento e comunicações Southbound com dispositivos de rede, como o software Load Balancer \(SLB\).
.
Ao usar a autenticação baseada\-em certificado, você deve registrar um certificado em nós do controlador de rede que é usado das seguintes maneiras.

1. Criptografar a comunicação Northbound com \(o\) SSL protocolo SSL entre os nós do controlador de rede e os clientes de gerenciamento, como System Center Virtual Machine Manager.
2. Autenticação entre nós do controlador de rede e dispositivos e serviços Southbound, como hosts Hyper-V e balanceadores \(de\)carga de software SLBs.

## <a name="creating-and-enrolling-an-x509-certificate"></a>Criando e registrando um certificado X. 509

Você pode criar e registrar um\-certificado autoassinado ou um certificado emitido por uma autoridade de certificação.

>[!NOTE]
>Ao usar o SCVMM para implantar o controlador de rede, você deve especificar o certificado X. 509 que é usado para criptografar as comunicações do Northbound durante a configuração do modelo de serviço do controlador de rede.

A configuração do certificado deve incluir os valores a seguir.

- O valor da caixa de texto **RestEndPoint** deve ser o nome \(\) de domínio totalmente qualificado ou o endereço IP do controlador de rede. 
- O valor **RestEndPoint** deve corresponder ao nome da \(entidade nome comum,\) CN do certificado X. 509.

### <a name="creating-a-self-signed-x509-certificate"></a>Criando um\-certificado X. 509 autoassinado

Você pode criar um certificado X. 509 autoassinado e exportá-lo com a \(chave privada protegida por\) uma senha seguindo estas etapas para\-implantações\-de nó único e de vários nós do controlador de rede .

Ao criar\-certificados autoassinados, você pode usar as diretrizes a seguir.

- Você pode usar o endereço IP do ponto de extremidade REST do controlador de rede para o parâmetro DnsName, mas isso não é recomendável porque ele requer que os nós do controlador de rede estejam todos \(localizados em uma única sub-rede de gerenciamento, por exemplo, em um único rack\)
- Para implantações de NC de vários nós, o nome DNS que você especificar se tornará o FQDN do \(host do controlador de rede de cluster de DNS que os registros são criados automaticamente.\) 
- Para implantações de controlador de rede de nó único, o nome DNS pode ser o nome de host do controlador de rede seguido pelo nome de domínio completo.

#### <a name="multiple-node"></a>Vários nós

Você pode usar o comando [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) do Windows PowerShell para criar um\-certificado autoassinado.

**Sintaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Exemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Nó único

Você pode usar o comando [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) do Windows PowerShell para criar um\-certificado autoassinado.

**Sintaxe**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Exemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Criando um certificado\-X. 509 assinado por CA

Para criar um certificado usando uma AC, você \(já deve ter implantado uma PKI\) de infraestrutura de chave pública com \(Active Directory serviços\)de certificados AD CS. 

>[!NOTE]
>Você pode usar CAs ou ferramentas de terceiros, como OpenSSL, para criar um certificado para uso com o controlador de rede, no entanto, as instruções neste tópico são específicas para o AD CS. Para saber como usar uma CA ou ferramenta de terceiros, consulte a documentação do software que você está usando.

A criação de um certificado com uma AC inclui as etapas a seguir.

1. Você ou o domínio ou administrador de segurança de sua organização configura o modelo de certificado
2. Você ou o administrador do controlador de rede da sua organização ou o administrador do SCVMM solicita um novo certificado da autoridade de certificação.

#### <a name="certificate-configuration-requirements"></a>Requisitos de configuração de certificado

Enquanto você estiver configurando um modelo de certificado na próxima etapa, certifique-se de que o modelo configurado inclui os seguintes elementos necessários.

1. O nome da entidade do certificado deve ser o FQDN do host do Hyper-V
2. O certificado deve ser colocado no repositório pessoal do computador local (meu – CERT: \ localmachine\my)
3. O certificado deve ter a autenticação do servidor (EKU: 1.3.6.1.5.5.7.3.1) e autenticação de cliente (EKU: 1.3.6.1.5.5.7.3.2) políticas de aplicativo.

>[!NOTE]
>Se o repositório \(de certificados pessoal My-CERT\) : \ localmachine\my no host\-Hyper-V tiver mais de um certificado X. 509 com o nome da entidade (CN) como o FQDN\)do \(nome de domínio totalmente qualificado do host, Verifique se o certificado que será usado pelo SDN tem uma propriedade de uso avançado de chave personalizada adicional com o OID 1.3.6.1.4.1.311.95.1.1.1. Caso contrário, a comunicação entre o controlador de rede e o host pode não funcionar.

#### <a name="to-configure-the-certificate-template"></a>Para configurar o modelo de certificado
  
>[!NOTE]
>Antes de executar esse procedimento, você deve examinar os requisitos de certificado e os modelos de certificado disponíveis no console de modelos de certificado. Você pode modificar um modelo existente ou criar uma duplicata de um modelo existente e, em seguida, modificar sua cópia do modelo. É recomendável criar uma cópia de um modelo existente.

1. No servidor em que o AD CS está instalado, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **autoridade de certificação**. O MMC \(\) do console de gerenciamento Microsoft Management da autoridade de certificação é aberto. 
2. No MMC, clique duas vezes no nome da autoridade de certificação, clique com o botão direito do mouse em **modelos de certificado**e clique em **gerenciar**.
3. O console modelos de certificado é aberto. Todos os modelos de certificado são exibidos no painel de detalhes.
4. No painel de detalhes, clique no modelo que você deseja duplicar.
5.  Clique no menu **ação** e, em seguida, clique em **duplicar modelo**. A caixa de diálogo **Propriedades** do modelo é aberta.
6.  Na caixa de diálogo **Propriedades** do modelo, na guia **nome da entidade** , clique em **fornecer na solicitação**. \(Essa configuração é necessária para certificados SSL do controlador de rede.\)
7.  Na caixa de diálogo **Propriedades** do modelo, na guia **tratamento de solicitação** , certifique-se de que permitir que **a chave privada seja exportada** esteja selecionada. Verifique também se a **assinatura e** a finalidade da criptografia estão selecionadas.
8.  Na caixa de diálogo **Propriedades** do modelo, na guia **extensões** , selecione **uso da chave**e clique em **Editar**.
9.  Em **assinatura**, verifique se a **assinatura digital** está selecionada.
10.  Na caixa de diálogo **Propriedades** do modelo, na guia **extensões** , selecione **políticas de aplicativo**e clique em **Editar**.
11.  Em **políticas de aplicativo**, verifique se a **autenticação de cliente** e a **autenticação de servidor** estão listadas.
12.  Salve a cópia do modelo de certificado com um nome exclusivo, como o **modelo de controlador de rede**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Para solicitar um certificado da autoridade de certificação

Você pode usar o snap-in de certificados para solicitar certificados. Você pode solicitar qualquer tipo de certificado que tenha sido pré-configurado e disponibilizado por um administrador da autoridade de certificação que processa a solicitação de certificado.

**Usuários** ou **Administradores** locais é a associação de grupo mínima necessária para concluir este procedimento.

1. Abra o snap-in de certificados para um computador.
2. Na árvore de console, clique **em \(certificados computador\)local**. Selecione o repositório de certificados **pessoal** .
3. No menu **ação** , aponte para * * todas as tarefas<strong>e, em seguida, clique em * * solicitar novo certificado</strong> para iniciar o assistente de registro de certificado. Clique em **Avançar**.
4. Selecione o **configurado pela** política de registro de certificado de administrador e clique em **Avançar**.
5. Selecione a **política** \(de registro de Active Directory com base no modelo de autoridade de certificação que você\)configurou na seção anterior.
6. Expanda a seção **detalhes** e configure os itens a seguir.
   1. Verifique se o **uso da chave** inclui <strong>assinatura digital * * e * * codificação de chave</strong>.
   2. Verifique se **as políticas de aplicativo** incluem 1.3.6.1.5.5.7.3.1\) de **autenticação** \(de servidor e\)1.3.6.1.5.5.7.3.2 de **autenticação** \(de cliente.
7. Clique em **Propriedades**.
8. Na guia **assunto** , em **nome da entidade**, em **tipo**, selecione **nome comum**. Em valor, especifique **ponto de extremidade REST do controlador de rede**.
9. Clique em **Aplicar**e clique em **OK**.
10. Clique em **Registrar**.

No MMC certificados, clique no repositório pessoal para exibir o certificado que você registrou da autoridade de certificação.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Exportando e copiando o certificado para a biblioteca do SCVMM

Depois de criar um certificado\-autoassinado ou\-assinado por uma autoridade de certificação, você deve exportar o certificado \(com a chave privada\) no formato. pfx e \(sem a chave privada no formatobase-64.cer\) do snap-in de certificados. 

Em seguida, você deve copiar os dois arquivos exportados para as pastas **serverCertificate.CR** e **NCCertificate.CR** que você especificou no momento em que você importou o modelo de serviço NC.

1. Abra o snap-in de certificados (certlm. msc) e localize o certificado no repositório de certificados pessoal do computador local.
2. Clique\-com o botão direito do mouse no certificado, clique em **todas as tarefas**e clique em **Exportar**. O Assistente para Exportação de Certificados é aberto. Clique em **Avançar**.
3. Selecione **Sim**, exportar a opção chave privada e clique em **Avançar**.
4. Escolha **troca de informações pessoais-#12 PKCS (. PFX)** e aceite o padrão para **incluir todos os certificados no caminho de certificação** , se possível.
5. Atribua os usuários/grupos e uma senha para o certificado que você está exportando, clique em **Avançar**.
6. Na página arquivo a ser exportado, procure o local onde você deseja colocar o arquivo exportado e dê um nome a ele.
7. Da mesma forma, exporte o certificado no. Formato CER. Observação: Para exportar para o. Formato CER, desmarque a opção Sim, exportar a chave privada.
8. Copie o. PFX para a pasta ServerCertificate.cr.
9. Copie o. Arquivo CER para a pasta NCCertificate.cr

Quando terminar, atualize essas pastas na biblioteca do SCVMM e verifique se você tem esses certificados copiados. Continue com a configuração e a implantação do modelo de serviço do controlador de rede.

## <a name="authenticating-southbound-devices-and-services"></a>Autenticando dispositivos e serviços Southbound 

A comunicação do controlador de rede com hosts e dispositivos SLB MUX usa certificados para autenticação. A comunicação com os hosts está acima do protocolo OVSDB enquanto a comunicação com os dispositivos SLB MUX está sobre o protocolo WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Comunicação de host do Hyper-V com o controlador de rede

Para a comunicação com os hosts Hyper-V em OVSDB, o controlador de rede precisa apresentar um certificado para os computadores host. Por padrão, o SCVMM escolhe o certificado SSL configurado no controlador de rede e o utiliza para comunicação Southbound com os hosts.

Esse é o motivo pelo qual o certificado SSL deve ter a EKU de autenticação de cliente configurada. Esse certificado é configurado nos hosts do Hyper-V \(recursos REST de "servidores" são representados no controlador de rede\)como um recurso de servidor e podem ser exibidos executando o comando **Get-NetworkControllerServer do Windows PowerShell** .

Veja a seguir um exemplo parcial do recurso REST do servidor.

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

Para a autenticação mútua, o host Hyper-V também deve ter um certificado para se comunicar com o controlador de rede. 

Você pode registrar o certificado de uma AC \(\)de autoridade de certificação. Se um certificado baseado em AC não for encontrado no computador host, o SCVMM criará um certificado autoassinado e o provisionará no computador host.

O controlador de rede e os certificados de host do Hyper-V devem ser confiáveis entre si. O certificado raiz do certificado de host Hyper-V deve estar presente no repositório de autoridades de certificação raiz confiáveis do controlador de rede para o computador local e vice-versa. 

Quando você estiver usando\-certificados autoassinados, o SCVMM garantirá que os certificados necessários estejam presentes no repositório de autoridades de certificação raiz confiáveis para o computador local. 

Se você estiver usando certificados baseados em AC para os hosts do Hyper-V, precisará garantir que o certificado raiz da autoridade de certificação esteja presente no repositório de autoridades de certificação raiz confiáveis do controlador de rede para o computador local.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Software Load Balancer a comunicação MUX com o controlador de rede

O software Load Balancer multiplexador \(MUX\) e o controlador de rede se comunicam pelo protocolo WCF, usando certificados para autenticação.

Por padrão, o SCVMM escolhe o certificado SSL configurado no controlador de rede e o utiliza para a comunicação Southbound com os dispositivos Mux. Esse certificado é configurado no recurso REST "NetworkControllerLoadBalancerMux" e pode ser exibido executando o cmdlet **Get-NetworkControllerLoadBalancerMux**do PowerShell.

Exemplo de recurso \(do MUX REST\)parcial:

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

Para autenticação mútua, você também deve ter um certificado nos dispositivos SLB MUX. Esse certificado é configurado automaticamente pelo SCVMM quando você implanta o balanceador de carga de software usando o SCVMM.

>[!IMPORTANT]
>Nos nós host e SLB, é essencial que o repositório de certificados das autoridades de certificação raiz confiáveis não inclua nenhum certificado no qual "emitido para" não seja o mesmo que "emitido por". Se isso ocorrer, a comunicação entre o controlador de rede e o dispositivo Southbound falhará.

O controlador de rede e os certificados SLB MUX devem ser confiáveis entre \(si o certificado raiz do certificado MUX SLB deve estar presente no repositório de autoridades de certificação raiz confiáveis do computador do controlador de\)rede e vice-versa. Quando você estiver usando\-certificados autoassinados, o SCVMM garantirá que os certificados necessários estejam presentes no repositório de autoridades de certificação raiz confiáveis do computador local.



