---
title: Solucionando problemas do serviço guardião de host
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
description: Resoluções para problemas comuns encontrados ao implantar ou operar um servidor HGS (serviço guardião de host) em uma malha protegida.
ms.author: ryanpu
ms.date: 10/12/2020
ms.openlocfilehash: 398029ed048ac835d23ffebb954baad13970b538
ms.sourcegitcommit: c56e74743e5ad24b28ae81668668113d598047c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91987290"
---
# <a name="troubleshooting-the-host-guardian-service"></a>Solucionando problemas do serviço guardião de host

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este artigo descreve as resoluções para problemas comuns encontrados ao implantar ou operar um servidor HGS (serviço guardião de host) em uma malha protegida.
Se você não tiver certeza da natureza do seu problema, primeiro tente executar o [diagnóstico de malha protegida](guarded-fabric-troubleshoot-diagnostics.md) em seus servidores HgS e hosts do Hyper-V para restringir as possíveis causas.

## <a name="certificates"></a>Certificados

O HGS requer vários certificados para operar, incluindo a criptografia configurada pelo administrador e o certificado de autenticação, bem como um certificado de atestado gerenciado pelo HGS em si.
Se esses certificados estiverem configurados incorretamente, o HGS não poderá atender solicitações de hosts Hyper-V que desejam atestar ou desbloquear protetores de chave para VMs blindadas.
As seções a seguir abordam problemas comuns relacionados aos certificados configurados no HGS.

### <a name="certificate-permissions"></a>Permissões de certificado

O HGS deve ser capaz de acessar as chaves pública e privada da criptografia e dos certificados de assinatura adicionados ao HGS pela impressão digital do certificado.
Especificamente, a conta de serviço gerenciado de grupo (gMSA) que executa o serviço HGS precisa de acesso às chaves.
Para localizar o gMSA usado pelo HGS, execute o seguinte comando em um prompt do PowerShell com privilégios elevados em seu servidor HGS:

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

A forma como você concede o acesso à conta gMSA para usar a chave privada depende de onde a chave é armazenada: no computador como um arquivo de certificado local, em um HSM (módulo de segurança de hardware) ou usando um provedor de armazenamento de chaves de terceiros personalizado.

#### <a name="grant-access-to-software-backed-private-keys"></a>Conceder acesso a chaves privadas com suporte de software

Se você estiver usando um certificado autoassinado ou um certificado emitido por uma autoridade de certificação que **não** esteja armazenada em um módulo de segurança de hardware ou provedor de armazenamento de chaves personalizado, você poderá alterar as permissões de chave privada executando as seguintes etapas:

1. Abrir o Gerenciador de certificados local (certlm. msc)
2. Expanda **certificados de > pessoais** e localize o certificado de autenticação ou criptografia que você deseja atualizar.
3. Clique com o botão direito do mouse no certificado e selecione **todas as tarefas > gerenciar chaves privadas**.
4. Selecione **Adicionar** para conceder a um novo usuário acesso à chave privada do certificado.
5. No seletor de objetos, insira o nome da conta gMSA para HGS encontrado anteriormente e, em seguida, selecione **OK**.
6. Verifique se o gMSA tem acesso de **leitura** ao certificado.
7. Selecione **OK** para fechar a janela de permissão.

Se você estiver executando o HGS no Server Core ou estiver gerenciando o servidor remotamente, não poderá gerenciar chaves privadas usando o Gerenciador de certificados local.
Em vez disso, você precisará baixar o [módulo do PowerShell das ferramentas de malha protegida](https://www.powershellgallery.com/packages/GuardedFabricTools) que permitirá que você gerencie as permissões no PowerShell.

1. Abra um console do PowerShell com privilégios elevados no computador Server Core ou use a comunicação remota do PowerShell com uma conta que tenha permissões de administrador local no HGS.
2. Execute os comandos a seguir para instalar o módulo do PowerShell de ferramentas de malha protegida e conceder o acesso à conta gMSA à chave privada.

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

#### <a name="grant-access-to-hsm-or-custom-provider-backed-private-keys"></a>Conceder acesso ao HSM ou a chaves privadas com suporte do provedor personalizado

Se as chaves privadas do seu certificado forem apoiadas por um HSM (módulo de segurança de hardware) ou um KSP (provedor de armazenamento de chaves) personalizado, o modelo de permissão dependerá de seu fornecedor de software específico.
Para obter os melhores resultados, consulte a documentação do fornecedor ou o site de suporte para obter informações sobre como as permissões de chave privada são tratadas para seu dispositivo/software específico.
Em todos os casos, o gMSA usado pelo HGS requer permissões de *leitura* nas chaves privadas do certificado de criptografia, assinatura e comunicações para que ele possa executar operações de assinatura e criptografia.

Alguns módulos de segurança de hardware não dão suporte à concessão de contas de usuário específicas acesso a uma chave privada; em vez disso, eles permitem que a conta de computador tenha acesso a todas as chaves em um conjunto de chaves específico.
Para esses dispositivos, geralmente é suficiente para fornecer ao computador acesso às suas chaves e o HGS poderá aproveitar essa conexão.

**Dicas para HSMs**

Veja abaixo as opções de configuração sugeridas para ajudá-lo a usar com êxito as chaves com suporte do HSM com o HGS com base nas experiências da Microsoft e de seus parceiros.
Essas dicas são fornecidas para sua conveniência e não têm garantia de estarem corretas no momento da leitura, nem são endossadas pelos fabricantes HSM.
Entre em contato com seu fabricante HSM para obter informações precisas sobre o dispositivo específico se você tiver mais dúvidas.

Marca/série HSM      | Sugestão
----------------------|-------------
Gemalto SafeNet       | Verifique se a propriedade uso de chave no arquivo de solicitação de certificado está definida como 0xa0, permitindo que o certificado seja usado para assinatura e criptografia. Além disso, você deve conceder à conta gMSA acesso de *leitura* à chave privada usando a ferramenta Gerenciador de certificados local (veja as etapas acima).
nCipher nShield        | Verifique se cada nó HGS tem acesso ao mundo de segurança que contém as chaves de assinatura e criptografia. Você também pode precisar conceder o acesso de *leitura* do gMSA à chave privada usando o Gerenciador de certificados local (consulte as etapas acima).
Utimaco CryptoServers | Verifique se a propriedade uso de chave no arquivo de solicitação de certificado está definida como 0x13, permitindo que o certificado seja usado para criptografia, descriptografia e assinatura.

### <a name="certificate-requests"></a>Solicitações de certificado

Se você estiver usando uma autoridade de certificação para emitir seus certificados em um ambiente de infraestrutura de chave pública (PKI), você precisará garantir que a solicitação de certificado incluirá os requisitos mínimos para o uso das chaves do HGS.

**Certificados de autenticação**

Propriedade CSR | Valor obrigatório
-------------|---------------
Algoritmo    | RSA
Tamanho da chave     | Pelo menos 2048 bits
Uso de chave    | Assinatura/sinal/DigitalSignature

**Certificados de criptografia**

Propriedade CSR | Valor obrigatório
-------------|---------------
Algoritmo    | RSA
Tamanho da chave     | Pelo menos 2048 bits
Uso de chave    | Criptografia/criptografar/DataEncipherment

**Modelos de serviços de certificados Active Directory**

Se você estiver usando modelos de certificado Active Directory ADCS (serviços de certificados) para criar os certificados, recomendamos usar um modelo com as seguintes configurações:

Propriedade de modelo ADCS | Valor obrigatório
-----------------------|---------------
Categoria do provedor      | Provedor de armazenamento de chaves
Nome do algoritmo         | RSA
Tamanho mínimo da chave       | 2.048
Finalidade                | Assinatura e criptografia
Extensão de uso de chave    | Assinatura digital, codificação de chave, codificação de dados ("permitir criptografia de dados do usuário")


### <a name="time-drift"></a>Descompasso de tempo

Se o tempo do servidor tiver se dessincronizado significativamente do de outros nós HGS ou hosts do Hyper-V em sua malha protegida, você poderá encontrar problemas com a validade do certificado do signatário do atestado.
O certificado de signatário de atestado é criado e renovado em segundo plano no HGS e é usado para assinar certificados de integridade emitidos para hosts protegidos pelo serviço de atestado.

Para atualizar o certificado de signatário de atestado, execute o seguinte comando em um prompt do PowerShell com privilégios elevados.

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName
AttestationSignerCertRenewalTask
```

Como alternativa, você pode executar manualmente a tarefa agendada abrindo **Agendador de tarefas** (Taskschd. msc), navegando para **Agendador de tarefas biblioteca > Microsoft > Windows > HGSServer** e executando a tarefa chamada **AttestationSignerCertRenewalTask**.

## <a name="switching-attestation-modes"></a>Alternando modos de atestado

Se você alternar HGS do modo TPM para Active Directory modo ou vice-versa usando o cmdlet [set-HgsServer](https://technet.microsoft.com/library/mt652180.aspx) , pode levar até 10 minutos para cada nó em seu cluster HgS iniciar a imposição do novo modo de atestado.
Isso é um comportamento normal.
É recomendável que você não remova nenhuma política que permita hosts do modo de atestado anterior até ter verificado que todos os hosts estão atestados com êxito usando o novo modo de atestado.

**Problema conhecido ao alternar do TPM para o modo do AD**

Se você tiver inicializado o cluster HGS no modo TPM e depois mudar para o modo de Active Directory, há um problema conhecido que impede que outros nós em seu cluster HGS alternem para o novo modo de atestado.
Para garantir que todos os servidores HGS estejam impondo o modo de atestado correto, execute `Set-HgsServer -TrustActiveDirectory` **em cada nó** do cluster HgS.
Esse problema não se aplicará se você estiver alternando do modo TPM para o modo AD *e* o cluster tiver sido originalmente configurado no modo ad.

Você pode verificar o modo de atestado do seu servidor HGS executando [Get-HgsServer](https://technet.microsoft.com/library/mt652162.aspx).

## <a name="memory-dump-encryption-policies"></a>Políticas de criptografia de despejo de memória

Se você estiver tentando configurar as políticas de criptografia de despejo de memória e não vir as políticas de despejo HGS padrão (HgS \_ Nodumps, HgS \_ DumpEncryption e HgS \_ DumpEncryptionKey) ou o cmdlet de política de despejo (Add-HgsAttestationDumpPolicy), é provável que você não tenha a atualização cumulativa mais recente instalada.
Para corrigir isso, [atualize seu servidor HgS](guarded-fabric-manage-hgs.md#patching-hgs) para o Windows Update cumulativo mais recente e [ative as novas políticas de atestado](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation).
Certifique-se de atualizar seus hosts Hyper-V para a mesma atualização cumulativa antes de ativar as novas políticas de atestado, já que os hosts que não têm os novos recursos de criptografia de despejo instalados provavelmente falharão com atestado quando a política HGS for ativada.

## <a name="endorsement-key-certificate-error-messages"></a>Mensagens de erro de certificado de chave de endosso

Ao registrar um host usando o cmdlet [Add-HgsAttestationTpmHost](/powershell/module/hgsattestation/add-hgsattestationtpmhost) , dois identificadores de TPM são extraídos do arquivo de identificador de plataforma fornecido: o EKcert (certificado de chave de endosso) e a EKpub (chave de endosso pública).
O EKcert identifica o fabricante do TPM, fornecendo garantias de que o TPM é autêntico e fabricado por meio da cadeia de suprimentos normal.
O EKpub identifica exclusivamente esse TPM específico e é uma das medidas que o HGS usa para conceder a um host acesso para executar VMs blindadas.

Você receberá um erro ao registrar um host TPM se uma das duas condições for verdadeira:
1. O arquivo de identificador de plataforma **não** contém um certificado de chave de endosso
2. O arquivo de identificador de plataforma contém um certificado de chave de endosso, mas esse certificado **não é confiável** no seu sistema

Determinados fabricantes de TPM não incluem EKcerts em seu TPMs.
Se você suspeitar de que esse é o caso do seu TPM, confirme com seu OEM que seu TPMs não deve ter um EKcert e use o `-Force` sinalizador para registrar manualmente o host com HgS.
Se o TPM tiver um EKcert, mas um não foi encontrado no arquivo de identificador de plataforma, verifique se você está usando um console do PowerShell de administrador (elevado) ao executar [Get-PlatformIdentifier](/powershell/module/platformidentifier/get-platformidentifier) no host.

Se você recebeu o erro de que seu EKcert não é confiável, verifique se você [instalou o pacote de certificados raiz do TPM confiável](guarded-fabric-install-trusted-tpm-root-certificates.md) em cada servidor HgS e se o certificado raiz do seu fornecedor de TPM está presente no repositório ** \_ RootCA do TrustedTPM** do computador local. Todos os certificados intermediários aplicáveis também precisam ser instalados no repositório **TrustedTPM \_ IntermediateCA** no computador local.
Depois de instalar os certificados raiz e intermediário, você poderá executar `Add-HgsAttestationTpmHost` com êxito.

## <a name="group-managed-service-account-gmsa-privileges"></a>Privilégios de conta de serviço gerenciado (gMSA) de grupo

A conta de serviço HGS (gMSA usada para o pool de aplicativos do serviço de proteção de chave no IIS) precisa receber o privilégio [gerar auditorias de segurança](/windows/security/threat-protection/security-policy-settings/generate-security-audits) , também conhecido como `SeAuditPrivilege` . Se esse privilégio estiver ausente, a configuração inicial do HGS será realizada com sucesso e o IIS será iniciado, mas o serviço de proteção de chave não funcionará e retornará o erro HTTP 500 _("erro de servidor no aplicativo/KeyProtection")._ Você também pode observar as seguintes mensagens de aviso no log de eventos do aplicativo.
```
System.ComponentModel.Win32Exception (0x80004005): A required privilege is not held by the client
at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.NativeUtility.RegisterAuditSource(String pszSourceName, SafeAuditProviderHandle& phAuditProvider)
at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.SecurityLog.RegisterAuditSource(String sourceName)
```
ou
```
Failed to register the security event source.
   at System.Web.HttpApplicationFactory.EnsureAppStartCalledForIntegratedMode(HttpContext context, HttpApplication app)
   at System.Web.HttpApplication.RegisterEventSubscriptionsWithIIS(IntPtr appContext, HttpContext context, MethodInfo[] handlers)
   at System.Web.HttpApplication.InitSpecial(HttpApplicationState state, MethodInfo[] handlers, IntPtr appContext, HttpContext context)
   at System.Web.HttpApplicationFactory.GetSpecialApplicationInstance(IntPtr appContext, HttpContext context)
   at System.Web.Hosting.PipelineRuntime.InitializeApplication(IntPtr appContext)

Failed to register the security event source.
   at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.SecurityLog.RegisterAuditSource(String sourceName)
   at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.SecurityLog.ReportAudit(EventLogEntryType eventType, UInt32 eventId, Object[] os)
   at Microsoft.Windows.KpsServer.KpsServerHttpApplication.Application_Start()

A required privilege is not held by the client
   at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.NativeUtility.RegisterAuditSource(String pszSourceName, SafeAuditProviderHandle& phAuditProvider)
   at Microsoft.Windows.KpsServer.Common.Diagnostics.Auditing.SecurityLog.RegisterAuditSource(String sourceName)
```
Além disso, você pode notar que nenhum dos cmdlets do serviço de proteção de chave (por exemplo, [Get-HgsKeyProtectionCertificate](/powershell/module/hgskeyprotection/get-hgskeyprotectioncertificate)) funcionam e, em vez disso, retorna erros.

Para resolver esse problema, você precisa conceder gMSA "gerar auditorias de segurança" (SeAuditPrivilege). Para fazer isso, você pode usar a política de segurança local `SecPol.msc` em cada nó do cluster HgS ou política de grupo. Como alternativa, você pode usar a ferramenta [SecEdit.exe](/windows-server/administration/windows-commands/secedit) para exportar a política de segurança atual, fazer as edições necessárias no arquivo de configuração (que é um texto sem formatação) e, em seguida, importá-las de volta.

> [!NOTE]
> Ao definir essa configuração, a lista de princípios de segurança definidos para um privilégio substitui totalmente os padrões (ele não concatena). Portanto, ao definir essa configuração de política, não se esqueça de incluir os contentores padrão desse privilégio (serviço de rede e serviço local), além do gMSA que você está adicionando.
