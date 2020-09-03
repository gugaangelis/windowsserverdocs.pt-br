---
title: Obter certificados para HGS
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: 0f9499402a5788cd3dc9ad9cd262d65636f9284c
ms.sourcegitcommit: 076504a92cddbd4b84bfcd89da1bf1c8c9e79495
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89427488"
---
# <a name="obtain-certificates-for-hgs"></a>Obter certificados para HGS

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Ao implantar o HGS, você será solicitado a fornecer certificados de assinatura e criptografia que são usados para proteger as informações confidenciais necessárias para iniciar uma VM blindada.
Esses certificados nunca deixam HGS e são usados apenas para descriptografar chaves de VM blindadas quando o host no qual estão sendo executados tem comprovado que está íntegro.
Os locatários (proprietários de VM) usam a metade pública dos certificados para autorizar seu datacenter a executar suas VMs blindadas.
Esta seção aborda as etapas necessárias para obter certificados de criptografia e assinatura compatíveis para o HGS.

## <a name="request-certificates-from-your-certificate-authority"></a>Solicitar certificados de sua autoridade de certificação

Embora não seja necessário, é altamente recomendável que você obtenha os certificados de uma autoridade de certificação confiável.
Isso ajuda os proprietários da VM a verificar se eles estão autorizando o servidor HGS correto (ou seja, o provedor de serviços ou Datacenter) para executar suas VMs blindadas.
Em um cenário empresarial, você pode optar por usar sua própria AC corporativa para emitir esses certificados.
Hosters e provedores de serviço devem considerar o uso de uma autoridade de certificação pública e conhecida em vez disso.

Os certificados de assinatura e de criptografia devem ser emitidos com as seguintes propriedades de Certificiate (a menos que marcado como "recomendado"):

Propriedade do modelo de certificado | Valor obrigatório
------------------------------|----------------
Provedor de criptografia               | Qualquer provedor de armazenamento de chaves (KSP). **Não** há suporte para CSPs (provedores de serviços de criptografia) herdados.
Algoritmo de chave                 | RSA
Tamanho mínimo da chave              | 2048 bits
Algoritmo de assinatura           | Recomendado: SHA256
Uso de chave                     | Assinatura digital *e codificação de* dados
Uso avançado de chave            | Autenticação do servidor
Política de renovação de chave            | Renove com a mesma chave. A renovação de certificados HGS com chaves diferentes impedirá a inicialização de VMs blindadas.
Nome da entidade                  | Recomendado: o nome ou endereço da Web da sua empresa. Essas informações serão mostradas aos proprietários da VM no assistente de arquivo de dados de blindagem.

Esses requisitos se aplicam se você estiver usando certificados com suporte de hardware ou software.
Por motivos de segurança, é recomendável que você crie suas chaves HGS em um módulo de segurança de hardware (HSM) para impedir que as chaves privadas sejam copiadas do sistema.
Siga as orientações do fornecedor do HSM para solicitar certificados com os atributos acima e certifique-se de instalar e autorizar o KSP do HSM em cada nó do HGS.

Cada nó HGS exigirá acesso aos mesmos certificados de autenticação e criptografia.
Se você estiver usando certificados com suporte de software, poderá exportar seus certificados para um arquivo PFX com uma senha e permitir que o HGS gerencie os certificados para você.
Você também pode optar por instalar os certificados no repositório de certificados do computador local em cada nó HGS e fornecer a impressão digital para o HGS.
Ambas as opções são explicadas no tópico [inicializar o cluster HgS](guarded-fabric-initialize-hgs.md) .

## <a name="create-self-signed-certificates-for-test-scenarios"></a>Criar certificados autoassinados para cenários de teste

Se você estiver criando um ambiente de laboratório HGS e não tiver ou desejar usar uma autoridade de certificação, poderá criar certificados autoassinados.
Você receberá um aviso ao importar as informações de certificado no assistente de arquivo de dados de blindagem, mas todas as funcionalidades permanecerão as mesmas.

Para criar certificados autoassinados e exportá-los para um arquivo PFX, execute os seguintes comandos no PowerShell:

```powershell
$certificatePassword = Read-Host -AsSecureString -Prompt "Enter a password for the PFX file"

$signCert = New-SelfSignedCertificate -Subject "CN=HGS Signing Certificate" -KeyUsage DataEncipherment, DigitalSignature
Export-PfxCertificate -FilePath .\signCert.pfx -Password $certificatePassword -Cert $signCert
Remove-Item $signCert.PSPath

$encCert = New-SelfSignedCertificate -Subject "CN=HGS Encryption Certificate" -KeyUsage DataEncipherment, DigitalSignature
Export-PfxCertificate -FilePath .\encCert.pfx -Password $certificatePassword -Cert $encCert
Remove-Item $encCert.PSPath
```

## <a name="request-an-ssl-certificate"></a>Solicitar um certificado SSL

Todas as chaves e informações confidenciais transmitidas entre hosts Hyper-V e HGS são criptografadas no nível de mensagem, ou seja, as informações são criptografadas com chaves conhecidas para HGS ou Hyper-V, impedindo que alguém farejasse seu tráfego de rede e roube chaves para suas VMs.
No entanto, se você tiver reqiurements de conformidade ou simplesmente preferir criptografar todas as comunicações entre o Hyper-V e o HGS, poderá configurar o HGS com um certificado SSL que criptografará todos os dados no nível de transporte.

Os hosts Hyper-V e os nós HGS precisarão confiar no certificado SSL fornecido, portanto, é recomendável que você solicite o certificado SSL de sua autoridade de certificação corporativa. Ao solicitar o certificado, certifique-se de especificar o seguinte:

Propriedade de certificado SSL | Valor obrigatório
-------------------------|---------------
Nome da entidade             | Nome do seu cluster HGS (conhecido como o nome da rede distribuída ou FQDN do objeto de computador virtual). Essa será a concatenação do nome do serviço HGS fornecido ao `Initialize-HgsServer` e seu nome de domínio do HgS.
Nome alternativo da entidade | Se você estiver usando um nome DNS diferente para acessar seu cluster HGS (por exemplo, se estiver atrás de um balanceador de carga), certifique-se de incluir esses nomes DNS no campo SAN de sua solicitação de certificado.

As opções para especificar esse certificado ao inicializar o servidor HGS são cobertas em [Configurar o primeiro nó HgS](guarded-fabric-initialize-hgs.md).
Você também pode adicionar ou alterar o certificado SSL em um momento posterior usando o cmdlet [set-HgsServer](/powershell/module/hgsserver/set-hgsserver?view=win10-ps) .

## <a name="next-step"></a>Próxima etapa

> [!div class="nextstepaction"]
> [Instalar o HGS](guarded-fabric-choose-where-to-install-hgs.md)
