---
title: Capturar informações sobre o modo de TPM exigido pelo HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 12/12/2018
ms.openlocfilehash: 82171eee10a06cad6bb3ac30e8f771086975c242
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841657"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>Autorizar hosts protegidos usando o atestado de TPM

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Modo TPM usa um identificador TPM (também chamado de uma chave de endosso ou de identificador de plataforma \[EKpub\]) para começar a determinar se um determinado host está autorizado como "protegido". Esse modo de Atestado usa medidas de integridade de inicialização segura e código para garantir que um determinado host do Hyper-V está em um estado íntegro e está em execução somente código confiável. Em ordem para o Atestado entender o que é e não está íntegro, você deve capturar os seguintes artefatos:

1.  Identificador do TPM (EKpub)

    -  Essas informações são exclusivas para cada host Hyper-V

2.  Linha de base do TPM (medições de inicialização)

    -  Isso é aplicável a todos os hosts do Hyper-V que são executados na mesma classe de hardware

3.  Política de integridade de código (uma lista de permissões de binários permitidos)

    -  Isso é aplicável a todos os hosts do Hyper-V que compartilham comuns de hardware e software

É recomendável que você capture a linha de base e a política de CI do "host de referência" que representa cada classe exclusiva da configuração de hardware do Hyper-V em seu data center. Começando com o Windows Server versão 1709, as políticas de CI de exemplo são fornecidas C:\Windows\schemas\CodeIntegrity\ExamplePolicies. 

## <a name="versioned-attestation-policies"></a>Políticas de Atestado com controle de versão

Windows Server 2019 apresenta um novo método de Atestado, chamado *Atestado v2*, em que um certificado TPM deve estar presente para adicionar o EKPub ao HGS. O método de atestado de v1 usado no Windows Server 2016 permitia que você substituir essa verificação de segurança especificando-sinalizador de força ao executar Add-HgsAttestationTpmHost ou outros cmdlets de atestado de TPM para capturar os artefatos. Começando com o Windows Server 2019, Atestado v2 é usado por padrão e você precisa especificar o sinalizador de v1 - Policyversion="1.0 quando você executar Add-HgsAttestationTpmHost se você precisa registrar um TPM sem um certificado. -Force sinalizador não funciona com o atestado de v2. 

Um host pode atestar somente se todos os artefatos (EKPub + TPM linha de base + política de CI) usam a mesma versão do Atestado. Atestado v2 é tentado primeiro e, se isso falhar, o Atestado v1 é usado. Isso significa que se você precisar registrar um identificador TPM usando Atestado v1, você precisará especificar também o sinalizador de v1 - Policyversion="1.0 para usar o atestado de v1 ao capturar a linha de base TPM e criar a política de CI. Se a linha de base TPM e a política de CI foram criados usando Atestado v2 e, em seguida, mais tarde você precisará adicionar um host protegido sem um certificado TPM, você precisa criar novamente cada artefato com o sinalizador de v1 - Policyversion="1.0. 

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>Capturar o identificador do TPM (identificador de plataforma ou EKpub) para cada host

1.  No domínio do fabric, verifique se o TPM em cada host está pronto para uso – ou seja, o TPM for inicializado e propriedade obtida. Você pode verificar o status do TPM abrindo o Console de gerenciamento do TPM (msc) ou executando **Get-Tpm** em uma janela elevada do Windows PowerShell. Se o TPM não estiver na **pronto** de estado, você precisará inicializá-lo e defina sua propriedade. Isso pode ser feito no Console de gerenciamento do TPM ou executando **Initialize Tpm**.

2.  Em cada host protegido, execute o seguinte comando em um console do Windows PowerShell com privilégios elevados para obter seu EKpub. Para `<HostName>`, substitua o nome de host exclusivo com algo adequado para identificar esse host – isso pode ser seu nome de host ou o nome usado por um serviço de inventário de malha (se disponível). Para sua conveniência, nomeie o arquivo de saída usando o nome do host.

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  Repita as etapas anteriores para cada host que se tornará um host protegido, certificando-se de fornecer um nome exclusivo de cada arquivo XML.

4.  Forneça os arquivos XML resultantes para o administrador HGS.

5.  No domínio HGS, abra um console do Windows PowerShell com privilégios elevados em um servidor HGS e execute o comando a seguir. Repita o comando para cada um dos arquivos XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Se você encontrar um erro ao adicionar um identificador TPM sobre um certificado de chave de endosso não confiável (EKCert), certifique-se de que o [confiáveis foram adicionados certificados de raiz TPM](guarded-fabric-install-trusted-tpm-root-certificates.md) para o nó HGS.
    > Além disso, alguns fornecedores TPM não usam EKCerts.
    > Você pode verificar se um EKCert está ausente, abrindo o arquivo XML em um editor como o bloco de notas e verificando uma mensagem de erro que indica nenhuma EKCert foi encontrado.
    > Se esse for o caso e você confia que o TPM no seu computador é autêntico, você pode usar o `-Force` parâmetro para adicionar o identificador do host ao HGS. No Windows Server de 2019, você precisa usar também o `-PolicyVersion v1` parâmetro ao usar `-Force`. Isso cria uma política consistente com o comportamento do Windows Server 2016 e exigirá que você use `-PolicyVersion v1` ao registrar a política de CI e a linha de base TPM.

## <a name="create-and-apply-a-code-integrity-policy"></a>Criar e aplicar uma política de integridade de código

Uma política de integridade de código ajuda a garantir que somente os executáveis em que execução em um host confiável podem ser executados. Malware e outros executáveis fora os executáveis confiáveis não podem ser executados.

Cada host protegido deve ter uma política de integridade de código aplicada para executar VMs blindadas em modo TPM. Você especifique as políticas de integridade de código exata que confiar adicionando-as ao HGS. Políticas de integridade de código podem ser configuradas para aplicar a política, qualquer software que não estão em conformidade com a política, ou simplesmente de auditoria de bloqueio (log de um evento quando não definido na política de software seja executado). 

Começando com o Windows Server versão 1709, as políticas de integridade de código de exemplo estão incluídas no Windows em C:\Windows\schemas\CodeIntegrity\ExamplePolicies. Duas políticas são recomendadas para o Windows Server:

- **AllowMicrosoft**: Permite que todos os arquivos assinados pela Microsoft. Essa política é recomendada para aplicativos de servidor, como SQL ou o Exchange, ou se o servidor for monitorado por agentes publicados pela Microsoft.
- **DefaultWindows_Enforced**: Permite que somente os arquivos que são fornecidas no Windows e não permitem que outros aplicativos lançados pela Microsoft, como o Office. Essa política é recomendada para servidores que executam apenas as funções de servidor interno e os recursos como o Hyper-V. 

É recomendável que você primeiro cria a política de CI no modo de auditoria (log) para ver se ele não tem nada e impor a política para hospedar cargas de trabalho de produção. 

Se você usar o [New-CIPolicy](https://docs.microsoft.com/powershell/module/configci/new-cipolicy?view=win10-ps) cmdlet para gerar sua própria política de integridade de código, você precisará decidir os níveis de regra usar. Recomendamos um nível primário do **Publisher** com o fallback para **Hash**, que permite que mais digitalmente assinados software a ser atualizado sem alterar a política de CI.
Novo software criptográfico escrito por mesmo editor também pode ser instalado no servidor sem alterar a política de CI.
Arquivos executáveis que não são assinados digitalmente serão transformada em hash – atualizações para esses arquivos exigirá que você crie uma nova política de CI.
Para obter mais informações sobre os níveis de regra de política de CI disponíveis, consulte [implantar políticas de integridade de código: regras de política e as regras de arquivo](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules) e a Ajuda do cmdlet.

1.  No host de referência, gere uma nova política de integridade de código. Os seguintes comandos criam uma política na **Publisher** nível com fallback para **Hash**. Ela então converte o arquivo XML para o formato de arquivo binário no Windows e HGS precisam aplicar e medir a política de CI, respectivamente.

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >O comando acima cria uma política de CI no modo de auditoria apenas. Ele não bloqueará binários não autorizados seja executado no host. Você só deve usar políticas impostas em produção.

2.  Manter o arquivo de política de integridade de código (arquivo XML) em que você pode localizar facilmente. Você precisará editar esse arquivo mais tarde para impor a política de CI ou mesclagem em alterações de futuras atualizações feitas no sistema.

3.  Aplica a política de CI para seu host de referência:

    1.  Copie o arquivo de política de CI binário (HW1CodeIntegrity.p7b) no seguinte local no seu host de referência (o nome do arquivo deve corresponder exatamente ao):<br>
        **C:\\Windows\\System32\\CodeIntegrity\\SIPolicy.p7b**

    2.  Reinicie o host para aplicar a política.

3.  Teste a política de integridade de código, executando uma carga de trabalho típica. Isso pode incluir VMs em execução, quaisquer agentes de gerenciamento de malha, agentes de backup, ou Solucionando problemas de ferramentas no computador. Verifique se há quaisquer violações de integridade de código e atualize sua política de CI, se necessário.

4.  Altere sua política de CI para o modo de imposto, executando os seguintes comandos em relação a seu arquivo XML de política de CI atualizado.

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  Aplica a política de CI para todos os seus hosts (com configuração de hardware e software idêntica) usando os seguintes comandos:

    ```powershell
    Copy-Item -Path '<Path to HW1CodeIntegrity\_enforced.p7b>' -Destination 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'

    Restart-Computer
    ```

    >[!NOTE]
    >Tenha cuidado ao aplicar as políticas de CI para hosts e quando a atualização de qualquer software nessas máquinas. Quaisquer drivers de modo kernel que são incompatíveis com a política de CI podem impedir que a máquina sendo inicializado. 

6.  Forneça o arquivo binário (neste exemplo, HW1CodeIntegrity\_enforced.p7b) para o administrador HGS.

7.  No domínio HGS, copie a política de integridade de código para um servidor HGS e execute o comando a seguir.

    Para `<PolicyName>`, especifique um nome para a política de CI que descreve o tipo de host que ele se aplica ao. Uma prática recomendada é nomeá-lo após o marca/modelo do seu computador e qualquer configuração especial de software em execução nele.<br>Para `<Path>`, especifique o caminho e nome de arquivo de política de integridade de código.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>Capturar a linha de base do TPM para cada classe exclusivo de hardware

Uma linha de base do TPM é necessária para cada classe exclusiva de hardware em sua malha de datacenter. Use um "host de referência" novamente. 

1. No host de referência, certifique-se de que a função Hyper-V e o recurso de suporte de Hyper-V de guardião de Host são instalados.

    >[!WARNING]
    >O recurso de suporte de Hyper-V de guardião de Host permite que a proteção baseada em virtualização de integridade de código que pode ser incompatível com alguns dispositivos. É altamente recomendável testar essa configuração em seu laboratório antes de habilitar esse recurso. Deixar de fazer isso pode resultar em falhas inesperadas, inclusive perda de dados ou um erro de tela azul (também chamado de erro de parada).

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```
        
2. Para capturar a política de linha de base, execute o seguinte comando em um console do Windows PowerShell com privilégios elevados.
        
    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >Você precisará usar o **SkipValidation -** sinalizador que indica se o host de referência não tem inicialização segura habilitada, a segurança baseada em virtualização habilitada, um IOMMU presente e em execução ou uma política de integridade de código aplicadas. Essas validações são projetadas para que você fique ciente dos requisitos mínimos da execução de uma VM blindada no host. Usando o sinalizador - SkipValidation não altera a saída do cmdlet; ele simplesmente silencia os erros.

3.  Fornece a linha de base do TPM (TCGlog arquivo) para o administrador HGS.

4.  No domínio HGS, copie o arquivo de TCGlog para um servidor HGS e execute o comando a seguir. Normalmente, será nomear a política após a classe de hardware, que ele representa (por exemplo, "Revisão de modelo do fabricante").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Confirmar Atestado](guarded-fabric-confirm-hosts-can-attest-successfully.md)
