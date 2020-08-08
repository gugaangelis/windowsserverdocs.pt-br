---
title: Capturar informações do modo TPM exigidas pelo HGS
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 04/01/2019
ms.openlocfilehash: dedd7a3629b4381fd5f78f70a39f6906cab0573d
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995384"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>Autorizar hosts protegidos usando atestado baseado em TPM

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

O modo TPM usa um identificador TPM (também chamado de identificador de plataforma ou chave de endosso \[ EKpub \] ) para começar a determinar se um host específico está autorizado como "protegido". Esse modo de atestado usa medidas de inicialização segura e integridade de código para garantir que um determinado host Hyper-V esteja em um estado íntegro e esteja executando apenas código confiável. Para que o atestado entenda o que é e não está íntegro, você deve capturar os seguintes artefatos:

1.  Identificador TPM (EKpub)

    -  Essas informações são exclusivas para cada host do Hyper-V

2.  Linha de base do TPM (medidas de inicialização)

    -  Isso é aplicável a todos os hosts Hyper-V executados na mesma classe de hardware

3.  Política de integridade de código (uma lista de permissões de binários permitidos)

    -  Isso é aplicável a todos os hosts Hyper-V que compartilham hardware e software comuns

Recomendamos que você capture a política de linha de base e CI de um "host de referência" que seja representativo de cada classe exclusiva de configuração de hardware do Hyper-V em seu datacenter. A partir do Windows Server versão 1709, as políticas de CI de exemplo estão incluídas em C:\Windows\schemas\CodeIntegrity\ExamplePolicies.

## <a name="versioned-attestation-policies"></a>Políticas de atestado com controle de versão

O Windows Server 2019 apresenta um novo método de atestado, chamado de *atestado v2*, em que um certificado TPM deve estar presente para adicionar o EKPUB ao HgS. O método de atestado v1 usado no Windows Server 2016 permitia que você substituisse essa verificação de segurança, especificando o sinalizador-Force ao executar Add-HgsAttestationTpmHost ou outros cmdlets de atestado de TPM para capturar os artefatos. A partir do Windows Server 2019, o atestado V2 é usado por padrão e você precisa especificar o sinalizador-PolicyVersion v1 ao executar Add-HgsAttestationTpmHost se precisar registrar um TPM sem um certificado. O sinalizador-Force não funciona com o atestado v2.

Um host só poderá atestar se todos os artefatos (EKPub + TPM Baseline + IC Policy) usarem a mesma versão do atestado. O atestado V2 é tentado primeiro e, se isso falhar, o atestado v1 será usado. Isso significa que se você precisar registrar um identificador TPM usando o atestado v1, também precisará especificar o sinalizador-PolicyVersion v1 para usar o atestado v1 ao capturar a linha de base do TPM e criar a política CI. Se a política de linha de base e CI do TPM tiver sido criada usando o atestado V2 e, posteriormente, você precisar adicionar um host protegido sem um certificado TPM, será necessário recriar cada artefato com o sinalizador-PolicyVersion v1.

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>Capturar o identificador do TPM (identificador de plataforma ou EKpub) para cada host

1.  No domínio de malha, verifique se o TPM em cada host está pronto para uso, ou seja, se o TPM foi inicializado e a propriedade obtida. Você pode verificar o status do TPM abrindo o console de gerenciamento do TPM (TPM. msc) ou executando **Get-TPM** em uma janela do Windows PowerShell com privilégios elevados. Se o TPM não estiver no estado **pronto** , será necessário inicializá-lo e definir sua propriedade. Isso pode ser feito no console de gerenciamento do TPM ou executando **Initialize-TPM**.

2.  Em cada host protegido, execute o seguinte comando em um console do Windows PowerShell com privilégios elevados para obter seu EKpub. Para `<HostName>` o, substitua o nome de host exclusivo por algo adequado para identificar esse host, que pode ser seu nome de host ou o Name usado por um serviço de inventário de malha (se disponível). Para sua conveniência, nomeie o arquivo de saída usando o nome do host.

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  Repita as etapas anteriores para cada host que se tornará um host protegido, certificando-se de fornecer a cada arquivo XML um nome exclusivo.

4.  Forneça os arquivos XML resultantes para o administrador HGS.

5.  No domínio HGS, abra um console do Windows PowerShell elevado em um servidor HGS e execute o comando a seguir. Repita o comando para cada um dos arquivos XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Se você encontrar um erro ao adicionar um identificador de TPM em relação a um certificado de chave de endosso não confiável (EKCert), verifique se os [certificados raiz do TPM confiável foram adicionados](guarded-fabric-install-trusted-tpm-root-certificates.md) ao nó HgS.
    > Além disso, alguns fornecedores de TPM não usam EKCerts.
    > Você pode verificar se um EKCert está ausente abrindo o arquivo XML em um editor como o bloco de notas e verificando se há uma mensagem de erro indicando que nenhum EKCert foi encontrado.
    > Se esse for o caso e você confiar que o TPM em seu computador é autêntico, você poderá usar o `-Force` parâmetro para adicionar o identificador de host ao HgS. No Windows Server 2019, você também precisa usar o `-PolicyVersion v1` parâmetro ao usar o `-Force` . Isso cria uma política consistente com o comportamento do Windows Server 2016 e exigirá que você use `-PolicyVersion v1` ao registrar a política CI e a linha de base do TPM também.

## <a name="create-and-apply-a-code-integrity-policy"></a>Criar e aplicar uma política de integridade de código

Uma política de integridade de código ajuda a garantir que apenas os executáveis confiáveis para serem executados em um host tenham permissão para serem executados.
Malware e outros executáveis fora dos executáveis confiáveis são impedidos de serem executados.

Cada host protegido deve ter uma política de integridade de código aplicada para executar VMs blindadas no modo TPM.
Você especifica as políticas de integridade de código exatas que confia adicionando-as ao HGS.
As políticas de integridade de código podem ser configuradas para impor a política, bloqueando qualquer software que não esteja em conformidade com a política ou simplesmente auditar (registrar um evento quando o software não definido na política for executado).

A partir do Windows Server versão 1709, as políticas de integridade de código de exemplo estão incluídas no Windows em C:\Windows\schemas\CodeIntegrity\ExamplePolicies. Duas políticas são recomendadas para o Windows Server:

- **AllowMicrosoft**: permite todos os arquivos assinados pela Microsoft. Essa política é recomendada para aplicativos de servidor, como SQL ou Exchange, ou se o servidor for monitorado por agentes publicados pela Microsoft.
- **DefaultWindows_Enforced**: permite apenas os arquivos fornecidos no Windows e não permite que outros aplicativos sejam lançados pela Microsoft, como o Office. Essa política é recomendada para servidores que executam apenas funções de servidor e recursos internos, como o Hyper-V.

É recomendável que você primeiro crie a política CI no modo de auditoria (log) para ver se está faltando alguma coisa e, em seguida, impor a política para cargas de trabalho de produção do host.

Se você usar o cmdlet [New-CIPolicy](/powershell/module/configci/new-cipolicy?view=win10-ps) para gerar sua própria política de integridade de código, será necessário decidir os níveis de regra a serem usados.
Recomendamos um nível primário do **Publicador** com fallback para **hash**, que permite que a maioria dos softwares assinados digitalmente sejam atualizados sem alterar a política de CI.
Novos softwares escritos pelo mesmo editor também podem ser instalados no servidor sem alterar a política de CI.
Os executáveis que não forem assinados digitalmente serão codificados – as atualizações nesses arquivos exigirão que você crie uma nova política de CI.
Para obter mais informações sobre os níveis de regra de política de CI disponíveis, consulte [implantar políticas de integridade de código: regras de política e regras de arquivo](/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules) e ajuda de cmdlet.

1.  No host de referência, gere uma nova política de integridade de código. Os comandos a seguir criam uma política no nível do **Publicador** com fallback para **hash**. Em seguida, ele converte o arquivo XML para o formato de arquivo binário que o Windows e o HGS precisam aplicar e medir a política de CI, respectivamente.

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >O comando acima cria uma política CI somente no modo de auditoria. Ele não impedirá que binários não autorizados sejam executados no host. Você só deve usar políticas impostas em produção.

2.  Mantenha seu arquivo de política de integridade de código (arquivo XML), onde você pode encontrá-lo facilmente. Você precisará editar esse arquivo posteriormente para impor a política de CI ou mesclar alterações de atualizações futuras feitas no sistema.

3.  Aplique a política CI ao seu host de referência:

    1.  Execute o comando a seguir para configurar o computador para usar sua política de CI. Você também pode implantar a política CI com [política de grupo](/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy) ou [System Center Virtual Machine Manager](/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019#manage-and-deploy-code-integrity-policies-with-vmm).

        ```powershell
        Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
        ```

    2.  Reinicie o host para aplicar a política.

3.  Teste a política de integridade de código executando uma carga de trabalho típica. Isso pode incluir VMs em execução, agentes de gerenciamento de malha, agentes de backup ou ferramentas de solução de problemas no computador. Verifique se há alguma violação de integridade de código e atualize sua política de CI, se necessário.

4.  Altere a política de CI para o modo imposto executando os comandos a seguir em seu arquivo XML da política CI atualizado.

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  Aplique a política CI a todos os seus hosts (com configuração de hardware e software idênticas) usando os seguintes comandos:

    ```powershell
    Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }

    Restart-Computer
    ```

    >[!NOTE]
    >Tenha cuidado ao aplicar as políticas de CI aos hosts e ao atualizar qualquer software nesses computadores. Qualquer driver de modo kernel que não esteja em conformidade com a política de CI pode impedir que o computador seja inicializado.

6.  Forneça o arquivo binário (neste exemplo, HW1CodeIntegrity \_ imforced. p7b) para o administrador do HgS.

7.  No domínio HGS, copie a política de integridade de código para um servidor HGS e execute o comando a seguir.

    Para `<PolicyName>` , especifique um nome para a política CI que descreve o tipo de host ao qual se aplica. Uma prática recomendada é nomeá-la após a marca/modelo de seu computador e qualquer configuração de software especial em execução nela.<br>Para `<Path>` , especifique o caminho e o nome do arquivo da política de integridade de código.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>Capturar a linha de base do TPM para cada classe exclusiva de hardware

Uma linha de base do TPM é necessária para cada classe exclusiva de hardware em sua malha de datacenter. Use um "host de referência" novamente.

1. No host de referência, verifique se a função Hyper-V e o recurso de suporte do Hyper-V do guardião de host estão instalados.

    >[!WARNING]
    >O recurso de suporte do Hyper-V do guardião de host permite a proteção baseada em virtualização de integridade de código que pode ser incompatível com alguns dispositivos. É altamente recomendável testar essa configuração em seu laboratório antes de habilitar esse recurso. Deixar de fazer isso pode resultar em falhas inesperadas, inclusive perda de dados ou um erro de tela azul (também chamado de erro de parada).

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2. Para capturar a política de linha de base, execute o seguinte comando em um console do Windows PowerShell elevado.

    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >Você precisará usar o sinalizador **-SkipValidation** se o host de referência não tiver a inicialização segura habilitada, uma IOMMU presente, segurança baseada em virtualização habilitada e em execução, ou uma política de integridade de código aplicada. Essas validações foram projetadas para que você saiba os requisitos mínimos de execução de uma VM blindada no host. O uso do sinalizador-SkipValidation não altera a saída do cmdlet; Ele simplesmente silencia os erros.

3.  Forneça a linha de base do TPM (arquivo TCGlog) ao administrador do HGS.

4.  No domínio HGS, copie o arquivo TCGlog para um servidor HGS e execute o comando a seguir. Normalmente, você nomeará a política após a classe de hardware que ela representa (por exemplo, "revisão do modelo do fabricante").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>Próxima etapa

> [!div class="nextstepaction"]
> [Confirmar atestado](guarded-fabric-confirm-hosts-can-attest-successfully.md)