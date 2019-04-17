---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: "Introdução à virtualização de serviços (AD DS) de domínio do Active Directory (nível 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3c7fdc2e20bc4a27f2555af54f396a15f664d6c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>Introdução à virtualização de serviços (AD DS) de domínio do Active Directory (nível 100)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Virtualização de ambientes de serviços de domínio do Active Directory (AD DS) foi contínua para um número de anos. A partir do Windows Server 2012, o AD DS oferece mais suporte para controladores de domínio de virtualização, apresentando os recursos de segurança para virtualização e habilitando a implantação rápida de controladores de domínio virtual por meio de clonagem. Esses novos recursos de virtualização oferecem maior suporte para nuvens públicas e privadas, ambientes híbridos nos locais onde partes do AD DS existirem no local e na nuvem e infraestruturas do AD DS que residem completamente no local.

**Neste documento**

-   [Virtualização segura de controladores de domínio](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#safe_virt_dc)

-   [Clonagem de controlador de domínio virtualizado](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#virtualized_dc_cloning)

-   [Etapas para implantar um controlador de domínio virtualizado clone](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)

-   [Solução de problemas](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#troubleshooting)

## <a name="safe_virt_dc"></a>Virtualização segura de controladores de domínio
Ambientes virtuais apresentam desafios únicos para as cargas de trabalho distribuídos que dependem de um esquema de replicação lógica baseada em relógio. A replicação do AD DS, por exemplo, usa um valor seja aumentado continuamente à crescente (conhecido como um USN ou o número de sequência de atualização) atribuído a transações em cada controlador de domínio. Instância do banco de dados do cada controlador de domínio também recebe uma identidade, conhecida como um InvocationID. InvocationID de um controlador de domínio e seu USN funcionam juntos como um identificador exclusivo associado a todas as transações de gravação executadas em cada controlador de domínio e deve ser exclusivo dentro da floresta.

Replicação do AD DS usa InvocationID e USNs em cada controlador de domínio para determinar quais alterações precisam ser replicada para outros controladores de domínio. Se um controlador de domínio é revertido em tempo fora do reconhecimento do controlador de domínio e um USN será reutilizado em uma transação completamente diferente, replicação não convergirão porque outros controladores de domínio acreditem que já receberam as atualizações associadas com o USN reutilizado no contexto do que InvocationID.

Por exemplo, a ilustração a seguir mostra a sequência de eventos que ocorre no Windows Server 2008 R2 e sistemas operacionais anteriores quando a reversão do USN é detectada em VDC2, o controlador de domínio de destino que está sendo executado em uma máquina virtual. Nesta ilustração, a detecção da reversão do USN ocorre em VDC2 quando um parceiro de replicação detecta que VDC2 enviou um valor de USN UTDV que foi visto anteriormente, o parceiro de duplicação, que indica que banco de dados do VDC2 tem revertidas no tempo de forma inadequada.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Uma máquina virtual (VM) torna mais fácil para os administradores de hipervisor para reverter um domínio USNs do controlador (seu relógio lógico), por exemplo, aplicando um instantâneo fora de reconhecimento do controlador de domínio. Para obter mais informações sobre USN e USN reversão, incluindo outra ilustração para demonstrar detectadas instâncias de reversão do USN, consulte [USN e reversão do USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

A partir do Windows Server 2012, controladores de domínio virtual do AD DS hospedados em plataformas de hipervisor que expõem um identificador chamado VM-Generation ID podem detectar e empregar medidas de segurança necessárias para proteger o ambiente AD DS, se a máquina virtual é revertida em tempo pelo aplicativo de um instantâneo VM. O design VM-GenerationID usa um mecanismo de hipervisor fornecedor independente para expor esse identificador no espaço de endereço da máquina virtual convidado, portanto, a experiência de virtualização seguro é consistentemente disponível para qualquer hipervisor que dá suporte a VM-GenerationID. Esse identificador pode ser amostrado por serviços e aplicativos em execução na máquina virtual para detectar se uma máquina virtual foi revertida no momento.

### <a name="BKMK_HowSafeguardsWork"></a>Como essas proteções de virtualização funcionam?
Durante a instalação do controlador de domínio, o AD DS inicialmente armazena o identificador de VM GenerationID como parte do atributo msDS-GenerationID no objeto de computador do controlador de domínio em seu banco de dados (conhecido como a árvore de informações de diretório ou DIT). O GenerationID VM independentemente é controlado por um driver do Windows na máquina virtual.

Quando um administrador restaura a máquina virtual de um instantâneo anterior, o valor atual de GenerationID a VM do driver máquina virtual é comparado com um valor na DIT.

Se os dois valores forem diferentes, o invocationID é reinicializada e o pool RID descartadas desta forma evitando USN reutilizar. Se os valores forem iguais, a transação é confirmada como normal.

AD DS também compara o valor atual de GenerationID a VM da máquina virtual com relação ao valor na DIT sempre que o controlador de domínio é reiniciado e, se forem diferentes, que ele redefine o invocationID, descarta o pool RID e atualiza o DIT com o novo valor. Ele também sem autoridade sincroniza na pasta SYSVOL para concluir a restauração segura. Isso permite que as proteções estender-se para o aplicativo de instantâneos em VMs que estavam desligamento. Essas proteções introduzidas no Windows Server 2012 permitem que os administradores do AD DS para se beneficiar vantagens exclusivas de implantação e gerenciamento de controladores de domínio em um ambiente virtualizado.

A ilustração a seguir mostra como as proteções de virtualização são aplicadas quando é detectado a reversão do USN mesmo em um controlador de domínio virtualizado que executa o Windows Server 2012 em um hipervisor que dá suporte a VM-GenerationID.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

Nesse caso, quando o hipervisor detecta uma alteração de valor de VM-GenerationID, proteções de virtualização são acionadas, incluindo a redefinição do InvocationID para o controlador de domínio virtualizado (de À B no exemplo anterior) e atualizar o valor de VM-GenerationID salvas na VM de acordo com o novo valor (G2) armazenado pelo hipervisor. As proteções Certifique-se de que a replicação converge para ambos os controladores de domínio.

Com o Windows Server 2012, o AD DS emprega proteções em controladores de domínio virtuais hospedadas no VM-GenerationID cientes hipervisores e garante que o aplicativo acidental de instantâneos ou outros tais habilitados em hipervisor mecanismos que poderiam reversão estado de uma máquina virtual não interrompa o ambiente AD DS (, evitando problemas de replicação como um balão de USN ou remanescente objetos). No entanto, a restauração de um controlador de domínio aplicando um instantâneo da máquina virtual não é recomendado como um mecanismo alternativo para fazer backup de um controlador de domínio. É recomendável que você continue a usar o Backup do Windows Server ou outras soluções de backup com base em gravador VSS.

> [!CAUTION]
> Se um controlador de domínio em um ambiente de produção acidentalmente é revertido para um instantâneo, é recomendável que você consultar os fornecedores dos aplicativos e serviços hospedados nessa máquina virtual, para obter orientação sobre como verificar o estado desses programas após instantâneo restaurar.

Para obter mais informações, consulte [virtualizados arquitetura de restauração segura de controlador de domínio](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="virtualized_dc_cloning"></a>Clonagem de controlador de domínio virtualizado
A partir do Windows Server 2012, os administradores podem facilmente e com segurança implantar os controladores de domínio copiando um controlador de domínio virtual existente. Em um ambiente virtual, os administradores não é mais necessário implantar uma imagem do servidor preparada usando sysprep.exe repetidamente, promover o servidor para um controlador de domínio em conclua requisitos de configuração adicionais para a implantação de cada controlador de domínio duplicado.

> [!NOTE]
> Os administradores precisam seguir processos existentes para implantar o primeiro controlador de domínio em um domínio, como ao usar um sysprep.exe para preparar um disco de rígido servidor virtual (VHD), promover o servidor para um controlador de domínio e, em seguida, conclua quaisquer requisitos de configuração adicionais. Em um cenário de recuperação de desastres, use o último backup do servidor para restaurar o primeiro controlador de domínio em um domínio.

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>Cenários que se beneficiam de clonagem de controlador de domínio virtual

-   Implantação rápida de controladores de domínio adicionais em um novo domínio

-   Restaurar rapidamente continuidade de negócios durante a recuperação de desastres restaurando a capacidade do AD DS por meio da implantação rápida de controladores de domínio usando clonagem

-   Otimizar as implantações de nuvem privada, aproveitando o provisionamento Elásticos de controladores de domínio para acomodar os requisitos de escala maior

-   Rápido provisionamento de ambientes de teste, permitindo a implantação e testes dos novos recursos e funcionalidades antes de distribuição de produção

-   Necessidades de maior capacidade Conheça rapidamente em filiais por meio da clonagem controladores de domínio existentes em filiais

Ao implantar rapidamente um grande número de controladores de domínio, continue a seguir os procedimentos existentes para validar a integridade de cada controlador de domínio após a conclusão da instalação. Implante controladores de domínio em lotes tamanhos razoável para que você pode validar sua integridade após a conclusão da cada lote de instalações. O tamanho recomendado de lote é 10. Para obter mais informações, consulte [etapas para implantar um controlador de domínio virtualizado clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc).

### <a name="clear-separation-of-responsibilities"></a>Clara separação das responsabilidades
A autorização para clonar controladores de domínio virtualizado é sob o controle do administrador do AD DS. Em ordem para os administradores de hipervisor para implantar outros controladores de domínio copiando controladores de domínio virtual, o administrador do AD DS tem que selecionar e autoriza um controlador de domínio e, em seguida, executar etapas preparatórias para habilitá-lo como uma fonte para clonagem.

Com a máquina virtual de provisionamento normalmente sob o alcance do administrador hipervisor, os administradores de hipervisor podem provisionar réplica máquinas de virtuais de controlador de domínio copiando controladores de domínio virtualizado que estão autorizados e preparados para clonar pelo administrador do AD DS.

> [!WARNING]
> Qualquer pessoa permissão para administrar o hipervisor que hospeda um controlador de domínio virtual deve ser altamente confiáveis e auditada no ambiente.

### <a name="how-does-virtual-domain-controller-cloning-work"></a>Como funciona o controlador de domínio virtuais clonagem trabalho?
O processo de clonagem envolve fazer uma cópia do VHD do controlador de domínio virtual existente (ou, para configurações mais complexas, o controlador de domínio VM), autorizando-o para clonagem no AD DS e criar um arquivo de configuração clone. Isso reduz o número de etapas e o tempo envolvidos na implantação de um controlador de domínio virtual réplica, caso contrário, eliminando tarefas de implantação repetitivo.

O controlador de domínio clone usa os seguintes critérios para detectar que se trata de uma cópia do outro controlador de domínio:

1.  O valor do ID VM-Generation fornecido pela máquina virtual é diferente do que o valor do ID VM-Generation armazenado na DIT.

    > [!NOTE]
    > A plataforma de hipervisor deve dar suporte a VM-Generation ID (ID de VM-Generation compatível com Windows Server 2012 Hyper-V).

2.  Presença de um arquivo chamado DCCloneConfig.xml em um dos seguintes locais:

    -   O diretório em que reside DIT

    -   %windir%\ntds

    -   A raiz de uma unidade de mídia removível

Depois que os critérios forem atendidos, ele passa pelo processo de clonagem provisionar em si como um controlador de domínio da réplica.

O controlador de domínio clone usa o contexto de segurança de controlador de domínio de origem (o controlador de domínio cuja cópia representa) para entrar em contato com a ferramenta controlador de domínio primário (PDC) do Windows Server 2012 emulador mestre detentor da função operações (também conhecida como operações mestre únicas flexíveis ou FSMO). O emulador do PDC deve estar executando o Windows Server 2012, mas ele não precisa estar em execução em um hipervisor.

> [!NOTE]
> Se você tiver uma extensão de esquema com atributos que referenciam o controlador de domínio de origem e o atributo está em um dos objetos copiados (objeto de computador, o objeto de configurações NTDS) para criar o clone, esse atributo não será copiado ou atualizado para o controlador de domínio clone de referência.

Depois de verificar se o controlador de domínio solicitante está autorizado para clonar, o emulador do PDC irá criar uma nova identidade de máquina, incluindo a nova conta, SID, nome e senha que identifica esse computador como um controlador de domínio réplica e enviar essas informações para o clone. O controlador de domínio clone preparará os arquivos de banco de dados do AD DS para servir como uma réplica e também limpará o estado da máquina.

Para obter mais informações, consulte [arquitetura de clonagem de controlador de domínio virtualizado](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch).

### <a name="cloning-components"></a>Componentes de clonagem
Os componentes de clonagem incluem novos cmdlets no módulo do Active Directory para Windows PowerShell e arquivos XML associados:

-   **Novo ADDCCloneConfigFile** "Este cmdlet cria e coloca DCCloneConfig.xml no local correto para garantir que ele está disponível para disparar clonagem. Ele também executa verificações de pré-requisito para garantir a clonagem bem-sucedida. Ela está incluída no módulo do Active Directory para Windows PowerShell. Você pode executá-lo localmente em um controlador de domínio virtualizado que está sendo preparado para clonar, ou você pode executá-lo remotamente usando a - opção offline. Você pode especificar configurações para o controlador de domínio clone, como seu nome, o site e o endereço IP.

    As que ele executa verificações de pré-requisito são:

    > [!NOTE]
    > Verificam se os pré-requisitos que não são executadas quando o "opção off-line é usada. Para obter mais informações, consulte [execução New-ADDCCloneConfigFile em modo offline](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).

    -   O controlador de domínio que está sendo preparado está autorizada para clonagem (for membro do **Cloneable controladores de domínio** grupo)

    -   O emulador do PDC executa o Windows Server 2012.

    -   Todos os programas ou serviços listados executem **Get-ADDCCloningExcludedApplicationList** estão incluídos no CustomDCCloneAllowList.xml (explicado com mais detalhes no final desta lista de componentes de clonagem).

-   **DCCloneConfig.xml** "para clonar com êxito um controlador de domínio virtualizado, esse arquivo deve estar presente no diretório em que reside DIT, *%windir%\NTDS*, ou a raiz de uma unidade de mídia removível. Além de ser usado como um dos gatilhos para detectar e iniciar clonagem, ele também fornece um meio para especificar configurações para o controlador de domínio clone.

    O esquema e um arquivo de exemplo para o arquivo DCCloneConfig.xml são armazenados em todos os computadores Windows Server 2012 em:

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.XML

    É recomendável que você use o cmdlet New-ADDCCloneConfigFile para criar o arquivo DCCloneConfig.xml. Embora você também pode usar o arquivo de esquema com um editor de XML ciente para criar esse arquivo, manualmente, editando o arquivo aumenta a probabilidade de erros. Se você editar o arquivo, ele deve ser feito usando editores de reconhecimento de XML, como o Visual Studio, [XML Notepad](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973), ou aplicativos de terceiros (não use o bloco de notas).

-   **Get-ADDCCloningExcludedApplicationList** "Este cmdlet é executado no controlador de domínio de origem antes de iniciar o processo de clonagem para determinar quais serviços ou programas instalados são não esteja na lista padrão com suporte, DefaultDCCloneAllowList.xml, ou uma inclusão definidos pelo usuário listar arquivo CustomDCCloneAllowList.xml nomeado e, portanto, não tenham sido avaliadas para clonagem impacto.

    Este cmdlet procura o controlador de domínio de origem para os serviços no Gerenciador de controle de serviços e programas instalados listado em **HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall** que não são especificados no padrão lista (DefaultDCCloneAllowList.xml) ou, se for fornecido, a lista de inclusão definidos pelo usuário (CustomDCCloneAllowList.xml file). A lista de aplicativos e serviços que é retornada por executar o cmdlet é a diferença entre o que já foi fornecido no DefaultDCCloneAllowList.xml ou o arquivo CustomDCCloneAllowList.xml e a lista é construída em tempo de execução com base no que está instalado no controlador de domínio a origem. A saída de programas e serviços de Get-ADDCCloningExcludedApplicationList pode ser adicionada ao arquivo CustomDCCloneAllowList.xml se você determinar que os serviços e programas podem ser com segurança clonados. Para determinar se um programa de serviço ou instalado pode ser clonado com segurança, avalie as seguintes condições:

    -   É o serviço ou programa instalado afetados pela identidade do computador, como nome, SID, senha e assim por diante?

    -   Não o serviço ou instalado store programa qualquer estado localmente no computador que pode afetar sua funcionalidade no clone?

    Você deve trabalhar com o fornecedor do software do aplicativo para determinar se o serviço ou o programa pode ser com segurança clonado.

    > [!NOTE]
    > Antes de provisionar serviços adicionais ou programas no arquivo CustomDCCloneAllowList.xml, verificar se você tem a licença necessária para copiar esse software contido nessa máquina virtual.

    Se os aplicativos não forem cloneable, removê-los do controlador de domínio de origem antes de criar a mídia de clone. Se um aplicativo aparece na saída do cmdlet, mas não está incluído no arquivo CustomDCCloneAllowList.xml, a clonagem falhará. Para clonar para ter sucesso, a saída do cmdlet não deve listar todos os serviços ou programas. Em outras palavras, um aplicativo deve ser incluído no arquivo CustomDCCloneAllowList.xml ou removido do controlador de domínio de origem.

    A tabela a seguir explica as opções para executar Get-ADDCCloningExcludedApplicationList.

    |||
    |-|-|
    |Argumento|Explicação|
    |*<no argument specified>*|Exibe uma lista de serviços ou programas no console que não tenha sido contabilizada clonagem. Se já houver um CustomDCCloneAllowList.XML em qualquer um dos locais permitidos, ele usa esse arquivo exibe o restante de serviços e de programas (que podem ser nada se correspondem as listas).|
    |-GenerateXml|Cria o arquivo de CustomDCCloneAllowList.XML preenchido com os serviços e os programas listados no console.|
    |-Força|Substitui um arquivo CustomDCCloneAllowList.XML existente.|
    |-Path|Caminho da pasta para criar o CustomDCCloneAllowList.XML.|

-   **DefaultDCCloneAllowList.xml** "Este arquivo está presente por padrão em cada Windows Server 2012 domínio controlador in o *%windir%\system32*. Ela lista os serviços e os programas que podem ser clonados com segurança por padrão instalados. Você não deve alterar o local ou o conteúdo desse arquivo ou clonagem falhará.

-   **CustomDCCloneAllowList.xml** "Se você tiver serviços ou programas instalados que residem no controlador de domínio de origem que estiverem fora daqueles listados no arquivo DefaultDCCloneAllowList.xml, esses programas e serviços devem ser incluídos nesse arquivo. Para encontrar os serviços ou programas instalados que não estão listados no no arquivo DefaultDCCloneAllowList.xml, execute o **Get-ADDCCloningExcludedApplicationList** cmdlet. Você deve usar o **"GenerateXml** argumento para gerar o arquivo XML.

    O processo de clonagem verifica os seguintes locais para que esse arquivo e usa o primeiro arquivo XML encontrado, independentemente do outro conteúdo da pasta:

    1.  A seguinte chave do registro:

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  Diretório de trabalho DSA

    3.  %systemroot%\ntds.

    4.  Mídia de leitura/gravação removível em ordem de letra de unidade, na raiz da unidade

### <a name="deployment-scenarios"></a>Cenários de implantação
Os seguintes cenários de implantação têm suporte para clonagem de controlador de domínio virtual:

-   Implante um controlador de domínio clone fazendo uma cópia do arquivo de disco rígido virtual (vhd) do controlador de domínio uma fonte.

-   Implante um controlador de domínio clone copiando a máquina virtual de um controlador de domínio de origem usando a semântica de exportação e importação exposta pelo hipervisor.

> [!NOTE]
> As etapas na seção [etapas para implantar um controlador de domínio virtualizado clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc) demonstram copiar uma máquina virtual usando o recurso de exportação e importação do Windows Server 2012 Hyper-V.

## <a name="steps_deploy_vdc"></a>Etapas para implantar um controlador de domínio virtualizado clone

-   [Pré-requisitos](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [Etapa 1: Conceder o controlador de domínio virtualizado origem a permissão para ser clonados](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [Etapa 2: Executar Get-ADDCCloningExcludedApplicationList cmdlet](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [Etapa 3: Executar New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [Etapa 4: Exportar e, em seguida, importe a máquina virtual do controlador de domínio da fonte](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>Pré-requisitos

-   Para concluir as etapas nos procedimentos a seguir, você deve ser um membro do grupo Admins. do domínio ou tem as permissões equivalentes atribuída a ele.

-   Os comandos do Windows PowerShell usados neste guia devem ser executados em um prompt de comando com privilégios elevados. Para fazer isso, clique com botão direito do **do Windows PowerShell** ícone e clique em **executar como administrador**.

-   Um servidor Windows Server 2012 com a função de servidor do Hyper-V instalada (**HyperV1**).

-   Um segundo servidor Windows Server 2012 com a função de servidor do Hyper-V instalada (**HyperV2**).

    > [!NOTE]
    > -   Se você estiver usando o hipervisor do outro, você deve contate o fornecedor pelo hipervisor para verificar se o hipervisor dá suporte a ID VM-Generation. Se o hipervisor não oferece suporte a ID de VM-Generation e você tiver fornecido um DCCloneConfig.xml, a VM novo será inicializado no modo de restauração dos serviços de diretório (DSRM).
    > -   Para aumentar a disponibilidade do serviço AD DS, este guia recomenda e fornece instruções usando dois hosts Hyper-V diferentes, que ajuda a impedir que um potencialmente único ponto de falha. No entanto, você não precisa dois hosts Hyper-V para realizar clonagem de controlador de domínio virtual.
    > -   Você precisa ser um membro do grupo local Administradores em cada servidor Hyper-V (**HyperV1** e **HyperV2**).
    > -   Para importar e exportar um arquivo VHD usando o Hyper-V com êxito, os comutadores de rede virtual em ambos os hosts Hyper-V devem ter o mesmo nome. Por exemplo, se você tiver uma rede virtual alternar **HyperV1** denominado VNet, em seguida, deve haver uma rede virtual ligue **HyperV2** denominado VNet.
    > -   Se os dois hosts Hyper-V (**HyperV1** e **HyperV2**) têm processadores diferentes, desligue a máquina virtual (**VirtualDC1**) que você pretende exportar, clique com botão direito a VM, clique em **configurações**, clique em **processador**e, em **compatibilidade de processador** selecione **migrar para um computador físico com uma versão de processador diferente** e clique em **Okey**.

-   Um implantados Windows Server 2012 controlador de domínio (físico ou virtualizado) que hospeda a função de emulador do PDC (**DC1**). Para verificar se a função de emulador do PDC está hospedada em um controlador de domínio do Windows Server 2012, execute o seguinte comando do Windows PowerShell:

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    O valor de OperatingSystemVersion deve retornar como uma versão 6.2. Se necessário, você pode transferir a função de emulador do PDC para um controlador de domínio que executa o Windows Server 2012. Para obter mais informações, consulte [usando Ntdsutil.exe para transferir ou executar funções FSMO para um controlador de domínio](https://support.microsoft.com/kb/255504).

-   Um controlador de domínio virtualizado de convidado implantado do Windows Server 2012 (**VirtualDC1**) que esteja no mesmo domínio que o controlador de domínio do Windows Server 2012 hospeda a função de emulador do PDC (**DC1**). Esse será o controlador de domínio de origem usado para clonagem. O controlador de domínio virtual convidado será hospedado em um servidor Windows Server 2012 Hyper-V (**HyperV1**).

    > [!NOTE]
    > -   Para clonar para ter sucesso, o controlador de domínio de origem que é usado para criar o clone não pode ser de um controlador de domínio que tenha sido rebaixado desde que a origem da mídia VHD foi criada.
    > -   Desligar o controlador de domínio de origem antes de copiar a VM ou o VHD.
    > -   Você não deve clonar um VHD ou restaurar um instantâneo é mais antigo do que o valor de tempo de vida de modo tombstone (ou o valor de tempo de vida do objeto excluído se a Lixeira do Active Directory estiver habilitada). Se você estiver copiando um VHD de um controlador de domínio existente, certifique-se de que o arquivo VHD não está mais antigo que o tempo de desativação valor (por padrão, 60 dias). Você não deve copiar um VHD de um controlador de domínio em execução para criar a mídia de clone.

    Ejete qualquer unidade de disquete virtual (VFD) a origem pode ter DC. Isso pode causar um problema de compartilhamento ao tentar importar a VM novo.

    Somente Windows Server 2012 controladores de domínio hospedados em um hipervisor VM-GenerationID podem ser usados como uma fonte para clonagem. O controlador de domínio de origem Windows Server 2012 usado para clonar deve ser em um estado íntegro. Para determinar o estado do controlador de domínio executar [dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx). Para obter uma melhor compreensão da saída retornada por dcdiag, consulte [que faz DCDIAG realmente … fazer?](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    Se o controlador de domínio de origem for um servidor DNS, o controlador de domínio clonados também será um servidor DNS. Você deve escolher um servidor DNS que hospeda apenas integrada do Active Directory zonas.

    Configurações do cliente DNS não são clonadas, mas em vez disso, são especificadas no arquivo DCCloneConfig.xml. Se não forem especificados, o controlador de domínio clonados apontará a mesmo como servidor DNS preferencial por padrão. O controlador de domínio clonados não terão uma delegação de DNS. O administrador da zona DNS pai deve atualizar a delegação de DNS para o controlador de domínio clonados conforme necessário.

    > [!WARNING]
    > As proteções de virtualização não se estende à Active Directory Lightweight Directory Services (AD LDS). Portanto, você não deve tentar clonar um controlador de domínio do AD DS que hospeda uma instância do AD LDS adicionando esta instância do AD LDS para o CustomDCCloneAllowList.xml. Como o AD LDS não é VM-Generation ID ciente, clonagem um controlador de domínio com o AD LDS pode causar divergência induzido reversão de USN em que o AD LDS conjunto de configurações.

    As seguintes funções de servidor não são suportadas para clonagem:

    -   Protocolo DHCP (protocolo DHCP)

    -   Serviços de certificados do Active Directory (AD CS)

    -   No Active Directory Lightweight Directory Services (AD LDS)

### <a name="bkmk4_grant_source"></a>Etapa 1: Conceder o controlador de domínio virtualizado origem a permissão para ser clonados
Neste procedimento, você concede o controlador de domínio de origem a permissão para ser clonados usando **Centro Administrativo do Active Directory** para adicionar o controlador de domínio de origem para o **Cloneable controladores de domínio** grupo.

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>Para conceder o controlador de domínio virtualizado origem a permissão para ser clonados

1.  Em qualquer controlador de domínio no mesmo domínio que o controlador de domínio que está sendo preparado para clonar (**VirtualDC1**), abra **Centro Administrativo do Active Directory** (ADAC), localize o objeto de controlador de domínio virtualizado (controladores de domínio estão geralmente localizados no **controladores de domínio** contêiner na ADAC), clique com botão direito-lo, escolha **adicionar ao grupo** e, em **insira o nome do objeto para selecionar** tipo **Cloneable controladores de domínio** e, em seguida, clique em **Okey**.

    A atualização de associação de grupo realizada nesta etapa deve ser replicada para o emulador do PDC antes clonagem pode ser realizada. Se o **Cloneable controladores de domínio** grupo não for encontrado, a função de emulador do PDC não pode ser hospedada em um controlador de domínio que executa o Windows Server 2012.

    > [!NOTE]
    > Para abrir ADAC em um controlador de domínio do Windows Server 2012, abra o Windows PowerShell e digite **dsac.exe**.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

O seguinte cmdlet do Windows PowerShell executa a mesma função que o procedimento anterior:


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>Etapa 2: Executar Get-ADDCCloningExcludedApplicationList cmdlet
Neste procedimento, execute o `Get-ADDCCloningExcludedApplicationList`cmdlet no controlador de domínio virtualizado de origem para identificar todos os programas ou serviços que não são avaliados para clonagem. Você precisa executar o cmdlet Get-ADDCCloningExcludedApplicationList antes do cmdlet New-ADDCCloneConfigFile porque se o cmdlet New-ADDCCloneConfigFile detectar um aplicativo excluído, ele não criará um arquivo DCCloneConfig.xml.

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>Para identificar aplicativos ou serviços que são executados em um controlador de domínio de origem que não tenham sido avaliados para clonagem

1.  No controlador de domínio de origem (**VirtualDC1**), clique em **Gerenciador do servidor**, clique em **ferramentas**, clique em **módulo Active Directory para Windows PowerShell** e, em seguida, digite o seguinte comando:


    Get-ADDCCloningExcludedApplicationList


2.  Verificar a lista dos serviços retornados e programas instalados com o fornecedor de software para determinar se pode ser com segurança clonados. Se os aplicativos ou serviços na lista não podem ser clonados com segurança, você deve removê-los do controlador de domínio de origem ou clonagem falhará.

3.  Para o conjunto de serviços e programas instalados que foram determinados para ser clonados com segurança, execute o comando novamente com o **"GenerateXML** alternar para provisionar esses serviços e programas no **CustomDCCloneAllowList.xml** arquivo.


    Get-ADDCCloningExcludedApplicationList - GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>Etapa 3: Executar New-ADDCCloneConfigFile
Execute New-ADDCCloneConfigFile no controlador de domínio de origem e, opcionalmente, especificar configurações para o controlador de domínio clone, como o nome, o endereço IP e resolução de DNS.

Por exemplo, para criar um controlador de domínio clone denominado VirtualDC2 com um endereço IPv4 estático, digite:


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> O controlador de domínio clone estarão localizado no mesmo site como o controlador de domínio de origem, a menos que um site diferente é especificado no arquivo DCCloneConfig.xml. É recomendável que você especifica um site adequado no arquivo DCCloneConfig.xml clone controlador de domínio com base em seu endereço IP.

O nome do computador é opcional. Se você não especificar um, será gerado um nome exclusivo com base na seguinte algoritmo:

-   O prefixo é 8 primeiros caracteres do nome de computador do controlador de domínio de origem. Por exemplo, um nome de computador de origem de SourceComputer é truncado para uma cadeia de caracteres de prefixo de SourceCo.

-   Um sufixo de nomenclatura exclusivo do formato "" CL*nnnn*"é acrescentado à cadeia de caracteres de prefixo onde *nnnn*é o próximo valor disponível de 0001-9999 PDC determina não está atualmente em uso. Por exemplo, se 0047 é o próximo número disponível no intervalo permitido, usando o exemplo anterior do prefixo de nome de computador SourceCo, o nome derivado usar para o computador clone será definido como SourceCo CL0047.

> [!NOTE]
> Um servidor de catálogo global (GC) é necessário para o cmdlet New-ADDCCloneConfigFile trabalhar com êxito. A associação do controlador de domínio de origem do **Cloneable controladores de domínio** grupo deve ser refletido no GC. O GC não precisa ser o mesmo controlador de domínio que o emulador do PDC, mas preferencialmente deve ser no mesmo site. Se não houver um GC, o comando falhará com o erro "o servidor não é operacional." Para obter mais informações, consulte [virtualizados a solução do controlador de domínio](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).

Para criar um controlador de domínio clone denominado Clone1 com estático IPv4 configurações e especificar servidores WINS preferenciais e alternativos, digite:


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> Se você especificar servidores WINS, você deve especificar ambos **"PreferredWINSServer** e **" AlternateWINSServer**. Se você especificar somente desses argumentos, clonagem falha com erro código 0x80041005 aparecendo no dcpromo.log.

Para criar um controlador de domínio clone denominado Clone2 com as configurações de IPv4 dinâmicas, digite:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> Nesse caso, deve haver um servidor DHCP no ambiente do que o clone pode atingir e obter o endereço IP e outras configurações de rede relevantes.

Para criar um controlador de domínio clone denominado Clone2 com as configurações de IPv4 dinâmicas e especificar servidores WINS preferenciais e alternativos, digite:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


Para criar um controlador de domínio clone com as configurações de IPv6 dinâmicas, digite:


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


Para criar um controlador de domínio clone com as configurações de IPv6 estáticas, digite:


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> Ao especificar as configurações de IPv6, a única diferença entre as configurações estáticas e dinâmicas é a inclusão de **-estático** alternar. A inclusão do **-estático** switch torna obrigatório para especificar pelo menos um **IPv6DNSResolver**. O endereço IPv6 estático deve ser configurado via endereços sem estado de configuração automática (SLAAC) com prefixos de roteador atribuído. Com o IPv6 dinâmico, os resolvedores DNS são opcionais, mas espera-se que o clone pode acessar um servidor DHCP IPv6 habilitado na sub-rede para obter o endereço IPv6 e informações de configuração de DNS.

#### <a name="BKMK_OfflineMode"></a>Executando New-ADDCCloneConfigFile em modo offline
Se você tiver várias cópias de mídia de controlador de domínio de origem que foram preparadas para clonagem (ou seja, o controlador de domínio de origem está autorizado para clonagem, o cmdlet Get-ADDCCloningExcludedApplicationList foi executado e assim por diante) e você deseja especificar configurações diferentes para cada cópia da mídia, você pode executar New-ADDCCloneConfigFile em modo offline. Isso pode ser mais eficiente do que Preparando individualmente cada VM, por exemplo, importando cada cópia.

Nesse caso, os administradores de domínio podem montar o disco offline e usar ferramentas de administração de servidor remoto (RSAT) para executar o cmdlet New-ADDCCloneConfigFile com o - argumento offline para adicionar os arquivos XML, que permite a automação semelhantes a fábrica usando novas opções do Windows PowerShell, incluídas no Windows Server 2012. Para obter mais informações sobre como montar o disco offline para executar o cmdlet New-ADDCCloneConfigFile em modo offline, consulte [adicionando XML para o disco do sistema Offline](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline).

Primeiro você deve executar o cmdlet localmente na mídia de origem para garantir que pré-requisito verifica pass. As verificações de pré-requisito não são executadas em modo offline porque o cmdlet pode ser executado em uma máquina que pode não ser o mesmo domínio ou de um computador ingressado no domínio. Depois de executar o cmdlet localmente, ele criará um arquivo DCCloneConfig.xml. Você pode excluir DCCloneConfig.xml que é criado localmente, se você pretende usar o modo offline posteriormente.

Para criar um controlador de domínio clone denominado CloneDC1 em modo offline, em um site chamado REDMOND"com endereço IPv4 estático, tipo:


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


Para criar um controlador de domínio clone denominado Clone2 em modo offline com IPv4 estático e configurações de IPv6 estáticas, tipo:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


Para criar um controlador de domínio clone em modo offline com IPv4 estático e dinâmico IPv6 configurações e especificar vários servidores DNS para as configurações de resolução DNS, digite:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


Para criar um controlador de domínio clone denominado Clone1 em modo offline com IPv4 dinâmico e configurações de IPv6 estáticas, tipo:


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


Para criar um controlador de domínio clone em modo offline com dinâmicas configurações de IPv6 e IPv4 dinâmico, digite:


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>Etapa 4: Exportar e, em seguida, importe a máquina virtual do controlador de domínio da fonte
Neste procedimento, exporte a máquina virtual do controlador de domínio virtualizado da fonte e, em seguida, importe a máquina virtual. Essa ação cria um controlador de domínio virtualizado clone em seu domínio.

Você precisa ser um membro do grupo local Administradores em cada host do Hyper-V. Se você usar credenciais diferentes para cada servidor, execute os cmdlets do Windows PowerShell para exportar e importar a VM em diferentes sessões do Windows PowerShell.

Se houver instantâneos no controlador de domínio de origem, eles deverão ser excluídos antes do controlador de domínio de origem é exportado porque a VM não importará se um instantâneo tem configurações de processador que são incompatíveis com o host do hyper-v de destino. Se as configurações de processador são compatíveis entre os hosts de origem e destino hyper-v, você pode exportar e copie a fonte sem excluir instantâneos antecipadamente. Após a importação, no entanto, os instantâneos devem ser excluídos do clone VM antes que ele é iniciado.

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>Para copiar um controlador de domínio virtual por exportar e importar, em seguida, o controlador de domínio de origem virtualizada

1.  Em **HyperV1**, o controlador de domínio de origem do desligamento (**VirtualDC1**).

    ![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

    Stop-VM-nomear VirtualDC1 - ComputerName HyperV1


2.  Em **HyperV1**, excluir instantâneos e exportar o controlador de domínio de origem (VirtualDC1) para o diretório c:\CloneDCs.

> [!NOTE]
> Você deve excluir todos os instantâneos associados porque cada vez que um instantâneo é executado, um novo arquivo AVHD é criado que atua como imagem diferencial de disco. Isso cria um efeito de cadeia. Se você respondeu instantâneos e inseri-lo DCCLoneConfig.xml em VHD, você pode acabar criar um clone de uma versão mais antiga do DIT ou inserindo o arquivo de configuração para o arquivo VHD errado. Excluir o instantâneo mescla todas essas AVHDs o VHD base.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  Copie a pasta **virtualdc1** c:\Import no diretório da **HyperV2**.

4.  Em **HyperV2**, usando **Gerenciador do Hyper-V**, importe a máquina virtual (usando o **importar Virtual Machine** assistente no **Gerenciador do Hyper-V**) da pasta **c:\Import\virtualdc1** e excluir todos os associados **instantâneos**.

Use o **copiar a máquina virtual (criar novo ID exclusivo)** opção ao importar a máquina virtual.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


Para criar vários clone controladores de domínio do controlador de domínio de origem mesmo:

  -   Interface do usuário: o **importar Virtual Machine** assistente, especifique novos locais para **pasta de configuração de máquina Virtual**, **store instantâneo**, **paginação Smart pasta**e um outro **local** para os discos rígidos virtuais para a máquina virtual.

  -   Windows PowerShell: especificar novos locais para a máquina virtual usando os seguintes parâmetros para o `Import-VM` cmdlet:

        $path = get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual máquinas" Import-VM-caminho $path.fullname-cópia - GenerateNewId - ComputerName HyperV2 - VhdDestinationPath "" - SnapshotFilePath "caminho" - SmartPagingFilePath "" - VirtualMachinePath "caminho"


> [!NOTE]
> O tamanho do lote recomendada para a criação de clone vários controladores de domínio simultaneamente é 10. O número máximo é restrito pelo número máximo de conexões de duplicação de saída, que por padrão é 16 para distribuído arquivo sistema DFSR (replicação) e 10 para o serviço de replicação de arquivos (FRS). Não implante mais do que o número recomendado de controladores de domínio clone simultaneamente, a menos que você testar exaustivamente esse número para seu ambiente.

5.  Em **HyperV1**, reinicie o controlador de domínio de origem (**(VirtualDC1**) para ativá-la novamente.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  Em **HyperV2**, inicie a máquina virtual (**VirtualDC2**) para levá-lo online como um controlador de domínio clone no domínio.

![Introdução ao AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> O emulador do PDC deve estar executando para clonar para ter sucesso. Se ele foi desligado, verifique se ele foi iniciado e realizada sincronização inicial para que ele está ciente tem a função de emulador do PDC. Para obter mais informações, consulte Microsoft [artigo KB 305476](https://support.microsoft.com/kb/305476).

Após a conclusão de clonagem, verifique se o nome do computador clone para garantir que a operação de clonagem foi bem-sucedida. Verifique se que a VM não foi iniciado no modo de restauração dos serviços de diretório (DSRM). Se você tentar fazer logon e receber um erro indicando que nenhum servidor de logon está disponível, tente fazer logon no DSRM. Se o controlador de domínio não clone com êxito e ele é inicializado no DSRM, verifique os logs no Visualizador de eventos e dcpromo registra em log na pasta %systemroot%/debug.

O controlador de domínio clonados será um membro do **Cloneable controladores de domínio** agrupar porque ele copia a participação do controlador de domínio de origem. Como prática recomendada, você deve deixar o **Cloneable controladores de domínio** grupo vazio até que você está pronto para realizar operações de clonagem, e você deve remover membros depois de operações de clonagem são concluídas.

Se o controlador de domínio de origem armazena uma mídia de backup, o controlador de domínio clonados também armazenará a mídia de backup. Você pode executar `wbadmin get versions` para mostrar a mídia de backup no controlador de domínio clonados. Um membro do grupo Admins. do domínio deve excluir a mídia de backup no controlador de domínio clonados para impedir que ele está sendo restaurado acidentalmente. Para obter mais informações sobre como excluir um backup de estado do sistema usando wbadmin.exe, consulte [Wbadmin excluir systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx).

## <a name="troubleshooting"></a>Solução de problemas
Se o controlador de domínio clone (**VirtualDC2**) é iniciado no modo de restauração dos serviços de diretório (DSRM), ele não retorna para um modo normal na sua própria na próxima reinicialização. Para fazer logon um controlador de domínio é iniciado no DSRM, use **. \Administrator** e especifique a senha DSRM.

Corrija a causa de clonagem falha e verifique se que Dcpromo indicar que a clonagem não pode ser repetida. Se não puder ser repetida clonagem, descarte com segurança a mídia. Se clonagem poder tentar novamente, você deve remover o sinalizador de inicialização do modo de Restauração DS para tentar clonagem novamente.

1.  Abrir o Windows Server 2012 com um comando com privilégios elevados (direita clique em Windows Server 2012 e escolha Executar como administrador) e, em seguida, digite **msconfig**.

2.  Sobre o **inicialização** guia, em **opções de inicialização**, desmarque **inicialização segura** (ele já está marcado com a opção **reparo do Active Directory habilitado**).

3.  Clique em **Okey** e reinicie quando solicitado.

Para obter informações de solução de problemas sobre controladores de domínio virtualizado, consulte [virtualizados a solução do controlador de domínio](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).


