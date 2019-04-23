---
title: Solução de problemas de serviço guardião de Host
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 2dc9a612fa9760a6ca5f05efe1c287fd0872a1d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861247"
---
# <a name="troubleshooting-the-host-guardian-service"></a>Solução de problemas de serviço guardião de Host

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve as soluções de problemas comuns encontrados durante a implantação ou operação de um servidor do serviço de guardião de Host (HGS) em uma malha protegida.
Se você não tiver certeza da natureza do problema, primeiro tente executar o [protegidos do diagnóstico do fabric](guarded-fabric-troubleshoot-diagnostics.md) nos servidores HGS e hosts do Hyper-V para reduzir o potencial faz com que.

## <a name="certificates"></a>Certificados

HGS requer vários certificados para operar, incluindo a criptografia configurado pelo administrador e a assinatura de certificado, bem como um certificado de Atestado gerenciado pelo HGS em si.
Se esses certificados estão configurados incorretamente, HGS será impossível atender a solicitações de hosts do Hyper-V que queiram atestar ou desbloquear os protetores de chave para as VMs blindadas.
As seções a seguir abordam problemas comuns relacionados aos certificados configurados no HGS.

### <a name="certificate-permissions"></a>Permissões de certificado

HGS deve ser capaz de acessar as chaves públicas e privadas de criptografia e certificados adicionados ao HGS pela impressão digital do certificado de assinatura.
Especificamente, o grupo gerenciado conta de serviço (gMSA) que executa o serviço HGS precisa ter acesso às chaves.
Para localizar a gMSA usada pelo HGS, execute o seguinte comando em um prompt elevado do PowerShell em seu servidor HGS:

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

Como você pode conceder acesso à conta gMSA para usar a chave privada depende de onde a chave é armazenada: no computador como um arquivo de certificado local, em um módulo de segurança de hardware (HSM) ou usando um provedor de armazenamento de chaves personalizado de terceiros.

#### <a name="grant-access-to-software-backed-private-keys"></a>Conceder acesso a chaves privadas com suporte de software

Se você estiver usando um certificado autoassinado ou um certificado emitido por uma autoridade de certificação que está **não** armazenados em um módulo de segurança de hardware ou o provedor de armazenamento de chaves personalizado, você pode alterar as permissões de chave privadas, executando o seguintes etapas:

1. Gerenciador de certificados local aberto (certlm)
2. Expandir **pessoal > certificados** e localize o certificado de assinatura ou criptografia que você deseja atualizar.
3. Clique com botão direito no certificado e selecione **todas as tarefas > Gerenciar chaves privadas**.
4. Clique em **adicionar** para conceder acesso de usuário nova, a chave privada do escolhê.
5. No seletor de objetos, insira o nome da conta gMSA para HGS encontrado anteriormente e, em seguida, clique em **Okey**.
6. Verifique se tem a gMSA **leitura** acesso ao certificado.
7. Clique em **Okey** para fechar a janela de permissão.

Se você estiver executando o HGS no Server Core ou estiver gerenciando o servidor remotamente, você não poderá gerenciar chaves privadas, usando o Gerenciador de certificados local.
Em vez disso, você precisará baixar o [módulo do PowerShell de ferramentas de malha protegida](https://www.powershellgallery.com/packages/GuardedFabricTools) que permitirá que você gerencie as permissões no PowerShell.

1. Abra um console do PowerShell com privilégios elevados no computador Server Core ou usar a comunicação remota do PowerShell com uma conta que tenha permissões de administrador local no HGS.
2. Execute os seguintes comandos para instalar o módulo do PowerShell de ferramentas de malha protegida e conceder acesso à conta gMSA para a chave privada.

```powershell
$certificateThumbprint = '<ENTER CERTIFICATE THUMBPRINT HERE>'

# Install the Guarded Fabric Tools module, if necessary
Install-Module -Name GuardedFabricTools -Repository PSGallery

# Import the module into the current session
Import-Module -Name GuardedFabricTools

# Get the certificate object
$cert = Get-Item "Cert:\LocalMachine\My\$certificateThumbprint"

# Get the gMSA account name
$gMSA = (Get-IISAppPool -Name KeyProtection).ProcessModel.UserName

# Grant the gMSA read access to the certificate
$cert.Acl = $cert.Acl | Add-AccessRule $gMSA Read Allow
```

#### <a name="grant-access-to-hsm-or-custom-provider-backed-private-keys"></a>Conceder acesso ao HSM ou chaves privadas com suporte de provedor personalizadas

Se as chaves privadas do certificado são apoiadas por um módulo de segurança de hardware (HSM) ou um provedor personalizado de armazenamento de chaves (KSP), o modelo de permissão dependerá seu fornecedor de software específico.
Para obter melhores resultados, consulte a documentação do fornecedor ou site de suporte para obter informações sobre a chave privada como as permissões são tratadas para seu dispositivo/software específico.

Alguns módulos de segurança de hardware não dão suporte para a concessão de acesso de contas de usuário específico a uma chave privada; em vez disso, eles permitem que o acesso à conta de computador para todas as chaves em um conjunto específico de chave.
Para esses dispositivos, ela normalmente é suficiente para conceder o acesso ao computador para suas chaves e HGS serão capaz de aproveitar essa conexão.

**Dicas para HSMs**

Abaixo estão as opções de configuração sugerido para ajudarão você a usar chaves protegidas por HSM com o HGS com base na Microsoft e experiências de seus parceiros.
Essas dicas são fornecidas para sua conveniência e não têm garantia de estar correto no momento da leitura, nem são endossadas pelos fabricantes do HSM.
Para obter informações precisas que pertencem ao dispositivo específico, se você tiver mais dúvidas, entre em contato com o fabricante do HSM.

Série/marca do HSM      | Sugestão
----------------------|-------------
Gemalto SafeNet       | Verifique se que a propriedade de uso de chave no arquivo de solicitação de certificado é definida para 0xa0, permitindo que o certificado a ser usado para assinatura e criptografia. Além disso, você deve conceder à conta de gMSA *ler* acesso à chave privada usando a ferramenta de Gerenciador de certificados local (consulte as etapas acima).
Thales nShield        | Certifique-se de que cada nó HGS tem acesso ao mundo de segurança que contém as chaves de criptografia e assinatura. Você não precisa configurar permissões específicas de gMSA.
Utimaco CryptoServers | Verifique se que a propriedade de uso de chave no arquivo de solicitação de certificado é definida para 0x13, permitindo que o certificado a ser usado para criptografia, descriptografia e assinatura.

### <a name="certificate-requests"></a>Solicitações de certificado

Se você estiver usando uma autoridade de certificação para emitir os certificados em um ambiente de infraestrutura de chave pública (PKI), você precisará garantir que a solicitação de certificado inclui os requisitos mínimos para o uso HGS dessas chaves.

**Certificados de assinatura**

Propriedade CSR | Valor obrigatório
-------------|---------------
Algoritmo    | RSA
Tamanho da chave     | Pelo menos 2048 bits
Uso de chave    | Assinatura/logon/DigitalSignature

**Certificados de criptografia**

Propriedade CSR | Valor obrigatório
-------------|---------------
Algoritmo    | RSA
Tamanho da chave     | Pelo menos 2048 bits
Uso de chave    | Encryption/Encrypt/DataEncipherment

**Modelos de serviços de certificados do Active Directory**

Se você estiver usando modelos de certificado de serviços de certificados do Active Directory (ADCS) para criar os certificados, é recomendável que você usam um modelo com as seguintes configurações:

Propriedade de modelo de AD CS | Valor obrigatório
-----------------------|---------------
Categoria de provedor      | Provedor de Armazenamento de Chaves
Nome do algoritmo         | RSA
Tamanho mínimo da chave       | 2048
Finalidade                | Assinatura e criptografia
Extensão do uso da chave    | Codificação de dados de assinatura, codificação de chave digital ("permitir a criptografia de dados do usuário")


### <a name="time-drift"></a>Descompasso de tempo

Se a hora do seu servidor descompasso significativamente com a de outros nós HGS ou hosts do Hyper-V em sua malha protegida, você poderá encontrar problemas com a validade do certificado de signatário Atestado.
O certificado do signatário de Atestado é criado e renovado em segundo plano em HGS e é usado para assinar certificados de integridade emitidos para hosts protegidos pelo serviço de Atestado.

Para atualizar o certificado do signatário Atestado, execute o seguinte comando em um prompt elevado do PowerShell.

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName 
AttestationSignerCertRenewalTask
```

Como alternativa, você pode executar manualmente a tarefa agendada, abrindo **Agendador de tarefas** (taskschd), navegando para **biblioteca do Agendador de tarefas > Microsoft > Windows > HGSServer** e executando o tarefa denominada **AttestationSignerCertRenewalTask**.

## <a name="switching-attestation-modes"></a>Alternar os modos de Atestado

Se você alternar HGS do modo TPM para o modo do Active Directory ou vice-versa usando o [HgsServer conjunto](https://technet.microsoft.com/library/mt652180.aspx) cmdlet, ele pode levar até 10 minutos para cada nó no cluster para aplicar o novo modo de Atestado HGS.
Esse comportamento é normal.
É recomendável que você não remover todas as políticas de permitir que os hosts do modo de Atestado anterior até que você tiver verificado que todos os hosts são Atestado com êxito usando o novo modo de Atestado.

**Problema conhecido ao mudar do TPM para o modo do AD**

Se você inicializados seu cluster HGS no modo TPM e mais tarde alternar para modo do Active Directory, há um problema conhecido que impedirá que outros nós no cluster HGS de alternar para o novo modo de Atestado.
Para garantir que todos os servidores HGS estiver impondo o modo de Atestado correto, execute `Set-HgsServer -TrustActiveDirectory` **em cada nó** do seu cluster do HGS.
Esse problema não se aplica se você estiver alternando do modo TPM para o modo de AD *e* o cluster foi originalmente configurado no modo de AD.

Você pode verificar o modo de Atestado do seu servidor HGS executando [Get-HgsServer](https://technet.microsoft.com/library/mt652162.aspx).

## <a name="memory-dump-encryption-policies"></a>Políticas de criptografia de despejo de memória

Se você está tentando configurar políticas de criptografia de despejo de memória e não vir o padrão HGS despejar políticas (Hgs\_NoDumps, Hgs\_DumpEncryption e Hgs\_DumpEncryptionKey) ou o (de cmdlet de política de despejo Add-HgsAttestationDumpPolicy), é provável que você não tem a atualização cumulativa mais recente instalada.
Para corrigir isso, [atualizar seu servidor HGS](guarded-fabric-manage-hgs.md#patching-hgs) para a atualização cumulativa mais recente Windows e [ativar as novas políticas de Atestado](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation).
Verifique se que você atualizar seus hosts do Hyper-V para a mesma atualização cumulativa antes de ativar as novas políticas de Atestado, como hosts que não têm os novos recursos de criptografia de despejo instalados provavelmente falhará Atestado depois que a política HGS está ativada.

## <a name="endorsement-key-certificate-error-messages"></a>Mensagens de erro de certificado de chave de endosso

Ao registrar um host usando o [HgsAttestationTpmHost adicionar](https://docs.microsoft.com/powershell/module/hgsattestation/add-hgsattestationtpmhost) cmdlet, dois identificadores TPM são extraídos do arquivo de identificador de plataforma fornecido: o certificado de chave de endosso (EKcert) e a chave de endosso público (EKpub).
O EKcert identifica o fabricante do TPM, fornecer garantias de que o TPM é autêntico e fabricados pela cadeia de fornecimento normal.
O EKpub identifica exclusivamente esse TPM específico e é uma das medidas de que HGS usa para conceder a um host de VMs blindadas de acesso para executar.

Você receberá um erro ao registrar um host do TPM, se qualquer uma das duas condições forem verdadeira:
1. O arquivo de identificador de plataforma **não** contêm um certificado de chave de endosso
2. O arquivo de identificador de plataforma contém um certificado de chave de endosso, mas esse certificado é **não confiável** em seu sistema

Determinados fabricantes TPM não incluem EKcerts seus TPMs.
Se você suspeitar que esse é o caso com o TPM, confirme com seu OEM seu TPMs não devem ter um EKcert e usar o `-Force` sinalizador para registrar manualmente o host com o HGS.
Se o TPM deve ter um EKcert, mas nenhum foi encontrado no arquivo de identificador de plataforma, verifique se você estiver usando um console do PowerShell de administrador (elevado) quando em execução [Get-PlatformIdentifier](https://docs.microsoft.com/powershell/module/platformidentifier/get-platformidentifier) no host.

Se você recebeu o erro que seu EKcert é não confiável, certifique-se de que você tenha [instalou o pacote de certificados de raiz confiável do TPM](guarded-fabric-install-trusted-tpm-root-certificates.md) em cada servidor do HGS e que o certificado raiz para o fornecedor do TPM está presente no local da máquina  **TrustedTPM\_RootCA** armazenar. Todos os certificados intermediários aplicáveis também precisam ser instalados na **TrustedTPM\_IntermediateCA** armazenar no computador local.
Depois de instalar os certificados intermediários e raiz, você deve ser capaz de executar `Add-HgsAttestationTpmHost` com êxito.
