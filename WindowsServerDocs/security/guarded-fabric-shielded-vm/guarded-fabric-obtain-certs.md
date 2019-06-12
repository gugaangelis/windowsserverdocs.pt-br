---
title: Obter certificados para HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2dc232eb7aeb8b0807a8e9989ae3dc893f925f66
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447359"
---
# <a name="obtain-certificates-for-hgs"></a>Obter certificados para HGS

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Quando você implanta o HGS, você precisará fornecer certificados de assinatura e criptografia são usados para proteger as informações confidenciais necessárias para iniciar o backup de uma VM blindada.
Esses certificados nunca deixe HGS e são usadas somente para chaves VM blindado de descriptografar quando o host no qual eles estão em execução provou está íntegro.
Locatários (os proprietários da VM) usam o público metade dos certificados para autorizar seu datacenter para executar suas VMs blindadas.
Esta seção aborda as etapas necessárias para obter certificados de assinatura e criptografia compatíveis para HGS.

## <a name="request-certificates-from-your-certificate-authority"></a>Solicitar certificados da autoridade de certificação

Embora não seja necessário, é altamente recomendável que você obtenha seus certificados de autoridade de certificação confiável.
Isso ajuda os proprietários das VM verificar que eles são autorizando o HGS servidor correto (ou seja, datacenter ou provedor de serviço) para executar suas VMs blindadas.
Em um cenário empresarial, você pode optar por usar sua própria autoridade de certificação corporativa para emitir esses certificados.
Provedores de serviços e hosters devem considerar o uso uma autoridade de certificação pública, bem conhecida, em vez disso.

Certificados de assinatura e a criptografia devem ser emitidos com as seguintes propriedades certificiate (a menos que marcado como "recomendada"):

Propriedade do modelo de certificado | Valor obrigatório 
------------------------------|----------------
Provedor de criptografia               | Qualquer provedor de armazenamento de chaves (KSP). Provedores de serviços de criptografia herdados (CSPs) são **não** com suporte.
Algoritmo de chave                 | RSA
Tamanho mínimo da chave              | 2048 bits
Algoritmo de assinatura           | Recomendado: SHA256
Uso de chave                     | Assinatura digital *e* codificação de dados
Uso avançado de chave            | Autenticação do servidor
Política de renovação de chave            | Renove com a mesma chave. Renovando certificados HGS com chaves diferentes impedirá que as VMs blindadas sendo inicializado.
Nome da entidade                  | Recomendado: da sua empresa nome ou endereço web. Essas informações serão mostradas para os proprietários da VM no Assistente de arquivo de dados de blindagem.

Esses requisitos se aplicam se você estiver usando certificados apoiados por hardware ou software.
Por motivos de segurança, é recomendável que você crie suas chaves HGS em um módulo HSM (Hardware Security) para impedir que as chaves privadas que está sendo copiado para o sistema.
Siga as orientações do seu fornecedor HSM para solicitar certificados com os atributos acima e não se esqueça de instalar e autorizar o KSP HSM em cada nó HGS.

Cada nó HGS exigirão acesso à mesma assinatura e certificados de criptografia.
Se você estiver usando certificados com apoio de software, você pode exportar seus certificados para um arquivo PFX com uma senha e permitir que o HGS gerenciar os certificados para você.
Você também pode optar por instalar os certificados no repositório de certificados do computador local em cada nó HGS e forneça a impressão digital para HGS.
Ambas as opções são explicadas as [inicializar o Cluster de HGS](guarded-fabric-initialize-hgs.md) tópico.

## <a name="create-self-signed-certificates-for-test-scenarios"></a>Criar certificados auto-assinados para cenários de teste

Se você estiver criando um ambiente de laboratório HGS e não tiver ou quiser usar uma autoridade de certificação, você pode criar certificados autoassinados.
Você receberá um aviso ao importar as informações do certificado no Assistente de arquivo de dados de blindagem, mas toda a funcionalidade permanece o mesmo.

Para criar certificados autoassinados e exportá-los para um arquivo PFX, execute os seguintes comandos do PowerShell:

```powershell
$certificatePassword = Read-Host -AsSecureString -Prompt "Enter a password for the PFX file"

$signCert = New-SelfSignedCertificate -Subject "CN=HGS Signing Certificate"
Export-PfxCertificate -FilePath .\signCert.pfx -Password $certificatePassword -Cert $signCert
Remove-Item $signCert.PSPath

$encCert = New-SelfSignedCertificate -Subject "CN=HGS Encryption Certificate"
Export-PfxCertificate -FilePath .\encCert.pfx -Password $certificatePassword -Cert $encCert
Remove-Item $encCert.PSPath
```

## <a name="request-an-ssl-certificate"></a>Solicitar um certificado SSL

Todas as chaves e informações confidenciais são transmitidos entre hosts Hyper-V e HGS é criptografado no nível da mensagem – ou seja, as informações são criptografadas com chaves conhecidas para HGS ou Hyper-V, impedindo que alguém espionando seu tráfego de rede e roube chaves em suas VMs.
No entanto, se você tiver reqiurements de conformidade ou simplesmente prefere criptografar todas as comunicações entre o Hyper-V e o HGS, você pode configurar o HGS com um certificado SSL que criptografará todos os dados no nível do transporte.

Os hosts Hyper-V e nós HGS precisará confiar no certificado SSL que você fornecer, portanto, é recomendável que você solicitar o certificado SSL da autoridade de certificação empresarial. Ao solicitar o certificado, certifique-se de especificar o seguinte:

Propriedade do certificado SSL | Valor obrigatório
-------------------------|---------------
Nome da entidade             | Nome do cluster do HGS (nome de rede distribuída). Essa será a concatenação do nome do seu serviço HGS fornecida para `Initialize-HgsServer` e seu nome de domínio do HGS.
Nome alternativo da entidade | Se você usando um nome DNS diferente para acessar o cluster HGS (por exemplo, se ele estiver atrás de um balanceador de carga), certifique-se de incluir os nomes DNS no campo de SAN de sua solicitação de certificado.

As opções para especificar esse certificado ao inicializar o servidor HGS são abordadas [configurar o primeiro nó HGS](guarded-fabric-initialize-hgs.md).
Você também pode adicionar ou alterar o certificado SSL em um momento posterior usando o [HgsServer conjunto](https://docs.microsoft.com/powershell/module/hgsserver/set-hgsserver?view=win10-ps) cmdlet.

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Instalar o HGS](guarded-fabric-choose-where-to-install-hgs.md)