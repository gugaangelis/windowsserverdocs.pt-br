---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: "Arquitetura de controlador de domínio virtualizado"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac8b190df065547d82aa431761eb5c00c94a2ad6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-architecture"></a>Arquitetura de controlador de domínio virtualizado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico aborda a arquitetura de clonagem de controlador de domínio virtualizado e restauração segura. Ele mostra os processos de clonagem e restauração segura com fluxogramas e, em seguida, fornece uma explicação detalhada de cada etapa do processo.  
  
-   [Arquitetura de clonagem de controlador de domínio virtualizado](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)  
  
-   [Arquitetura de restauração segura do controlador de domínio virtualizado](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)  
  
## <a name="BKMK_CloneArch"></a>Arquitetura de clonagem de controlador de domínio virtualizado  
  
### <a name="overview"></a>Visão geral  
Clonagem de controlador de domínio virtualizado se baseia na plataforma do hipervisor para expor um identificador chamado **ID de geração de VM** para detectar a criação de uma máquina virtual. AD DS inicialmente armazena o valor desse identificador em seu banco de dados (NTDS. DIT) durante a promoção de controlador de domínio. Quando inicializa a máquina virtual, o valor atual da ID da VM geração da máquina virtual é comparado com o valor no banco de dados. Se os dois valores forem diferentes, o controlador de domínio redefine a ID de invocação e descarta o pool RID, desta forma evitando USN reutilizar ou a criação de possível de entidades de segurança duplicadas. O controlador de domínio e procura um arquivo DCCloneConfig.xml nos locais chamado na etapa 3 da [clonagem detalhadas processamento](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails). Se encontrar um arquivo DCCloneConfig.xml, ele conclui que ele está sendo implantado como um clone, portanto, ele inicia clonagem provisionar em si como um controlador de domínio adicional, promovendo novamente usando o NTDS existente. Conteúdo DIT e SYSVOL copiado da mídia de origem.  
  
Em um ambiente misto onde alguns hipervisores suportam GenerationID VM e outras não, é possível que uma mídia clone acidentalmente seja implantado em um hipervisor que não oferece suporte a VM GenerationID. A presença do arquivo DCCloneConfig.xml indica administrativa intenção de clonar um controlador de domínio. Portanto, se um arquivo DCCloneConfig.xml é encontrado durante a inicialização, mas uma VM-GenerationID não for fornecido do host, o clone DC é inicializado no diretório serviços restaurar modo (DSRM) para impedir que qualquer impacto para o restante do ambiente. A mídia clone subsequentemente pode ser movida para um hipervisor que dá suporte a VM GenerationID e, em seguida, clonagem pode ser repetido.  
  
Se a mídia clone é implantada em um hipervisor que dá suporte a VM GenerationID, mas um arquivo DCCloneConfig.xml não for fornecido, como o controlador de domínio detecta uma alteração de VM GenerationID entre seu DIT e aquele do novo VM, ele irá disparar proteções para impedir a reutilização de USN e evitar SIDs duplicados. No entanto, clonagem não será iniciada, portanto, o controlador de domínio secundário continuará a ser executado com a mesma identidade como o controlador de domínio de origem. Este DC secundário deve ser removido da rede no horário mais cedo possível para evitar qualquer inconsistências no ambiente. Para obter mais informações sobre como recuperar esse DC secundário, garantindo que as atualizações replicados saídas, consulte Microsoft KB artigo [2742970](https://support.microsoft.com/kb/2742970).  
  
### <a name="BKMK_CloneProcessDetails"></a>Clonagem processamento detalhado  
O diagrama a seguir mostra a arquitetura de uma operação de clonagem inicial e de uma operação de repetição clonagem. Esses processos são explicados mais detalhadamente neste tópico.  
  
**Operação inicial de clonagem**  
  
![Arquitetura de DC virtualizada](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)  
  
**Operação de repetição clonagem**  
  
![Arquitetura de DC virtualizada](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)  
  
As etapas a seguir explicam o processo mais detalhadamente:  
  
1.  Um controlador de domínio de máquina virtual existente é inicializado em um hipervisor que dá suporte a ID de geração de VM.  
  
    1.  Essa VM não tem nenhum conjunto de valor de ID de geração de VM existente em seu objeto de computador do AD DS após a promoção.  
  
    2.  Mesmo se ele for nulo, a criação de computador próxima significa que ele ainda clones, como uma nova geração de ID de VM não corresponderão.  
  
    3.  A ID de geração de VM é definido após a próxima reinicialização do controlador de domínio e não replicar.  
  
2.  A máquina virtual, em seguida, lê a ID de geração de VM fornecidos pelo driver de VMGenerationCounter. Ele compara os dois IDs de geração de VM.  
  
    1.  Se as IDs corresponderem, isso não é uma nova máquina virtual e clonagem não continua. Se um arquivo DCCloneConfig.xml existir, o controlador de domínio renomeia o arquivo com um carimbo de data / hora para evitar clonagem. O servidor continuará a inicializar normalmente. Este é o funcionamento de cada inicialização de qualquer controlador de domínio virtuais no Windows Server 2012.  
  
    2.  Se as IDs de dois não corresponderem, isso é uma nova máquina virtual que contém um NTDS. DIT de um controlador de domínio anterior (ou é um instantâneo restaurado). Se um arquivo DCCloneConfig.xml existir, o controlador de domínio prossegue com as operações de clonagem. Caso contrário, ele continua com as operações de restauração do instantâneo. Consulte [virtualizados arquitetura de restauração segura de controlador de domínio](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).  
  
    3.  Se o hipervisor não fornecer uma ID de geração de VM para comparação, mas há um arquivo DCCloneConfig.xml, o convidado renomeia o arquivo e, em seguida, é inicializado no DSRM para proteger a rede de um controlador de domínio duplicado. Se não houver nenhum arquivo dccloneconfig.xml, o convidado é inicializado normalmente (com o potencial para um controlador de domínio duplicado na rede). Para obter mais informações sobre como recuperar esse controlador de domínio duplicados, consulte Microsoft KB artigo [2742970](https://support.microsoft.com/kb/2742970).  
  
3.  O serviço NTDS verifica o valor do nome do valor de registro VDCisCloning DWORD (sob HKEY_Local_Machine\System\CurrentControlSet\Services\Ntds\Parameters).  
  
    1.  Se não existir, isso é uma primeira tentativa de clonagem para essa máquina virtual. O convidado implementa as proteções de duplicação de objeto v CC de invalidar o pool RID local e definindo uma nova ID de invocação de replicação do controlador de domínio  
  
    2.  Se ele já está definido como 0x1, isso é uma "repetição" clonagem tentativa, em que uma operação de clonagem anterior falhou. As medidas de proteção para a variação v objeto duplicação não são tiradas tinha que já tenha executado uma vez antes e desnecessariamente preciso alterar o convidado várias vezes.  
  
4.  O nome do valor do registro IsClone DWORD escrito (sob Hkey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters)  
  
5.  O serviço NTDS muda o sinalizador de inicialização de convidado para iniciar no modo de reparo DS quaisquer outras reinicializações.  
  
6.  O serviço NTDS tenta ler DcCloneConfig.xml em um dos três locais aceitos (diretório de trabalho DSA, windir%\NTDS ou mídia de leitura/gravação removível em ordem de letra de unidade, na raiz da unidade).  
  
    1.  Se o arquivo não existe em qualquer local válido, o convidado verifica o endereço IP de duplicação. Se o endereço IP não é duplicado, o servidor inicialize normalmente. Se houver um endereço IP duplicado, o computador é inicializado no DSRM para proteger a rede de um controlador de domínio duplicado.  
  
    2.  Se o arquivo existir em um local válido, o serviço NTDS valida suas configurações. Se o arquivo está em branco (ou quaisquer configurações específicas são em branco) NTDS configura valores automáticos para essas configurações.  
  
    3.  Se o DcCloneConfig.xml existe, mas contém quaisquer entradas inválidas ou esteja ilegível, clonagem falhará e o convidado é inicializado no modo de restauração dos serviços de diretório (DSRM).  
  
7.  O convidado desabilita todos os DNS registro automático para evitar o sequestro acidental do nome do computador de origem e endereços IP.  
  
8.  O convidado interrompe o serviço de logon de rede para impedir que qualquer publicidade ou responder AD DS solicitações de rede de clientes.  
  
9. NTDS valida que não há nenhum serviço ou programas instalados que não fazem parte do DefaultDCCloneAllowList.xml ou CustomDCCloneAllowList.xml  
  
    1.  Se há serviços ou lista de permissões de programas instalados que não estejam na exclusão padrão ou a exclusão personalizada de lista de permissões, clonagem falhará e o convidado é inicializado no DSRM para proteger a rede de um controlador de domínio duplicado.  
  
    2.  Se não houver nenhum incompatibilidades, clonagem continua.  
  
10. Se o endereçamento IP automático será usado por causa de configurações de rede DCCloneConfig.xml em branco, o convidado permite DHCP nos adaptadores de rede para obter um IP concessão de endereço, o roteamento de rede e informações de resolução de nome.  
  
11. O convidado localiza e entra em contato com o controlador de domínio que executa o emulador do PDC FSMO função. Isso usa o protocolo de DCLocator e DNS. Ele faz com que uma conexão RPC e chama o método IDL_DRSAddCloneDC para clonar o objeto de computador do controlador de domínio.  
  
    1.  Se o objeto de computador de origem do convidado mantém permissão head estendida de domínio do "' permitem que um controlador de domínio criar um clone de si", em seguida, clonagem receita.  
  
    2.  Se o objeto de computador de origem do convidado não tem permissão estendida, clonagem falhará e o convidado é inicializado no DSRM para proteger a rede de um controlador de domínio duplicado.  
  
12. O nome de objeto de computador do AD DS é definido para corresponder ao nome especificada em DCCloneConfig.xml, se houver, senão gerado automaticamente no PDCE. NTDS cria o objeto de configuração NTDS correto para o site de lógica do Active Directory apropriado.  
  
    1.  Se esse for um PDC clonagem, em seguida, o convidado renomeia o computador local e é reinicializado. Após a reinicialização, ele passa por etapa 1 a 10 novamente depois passa para a etapa 13.  
  
    2.  Se esta for uma réplica DC não clonagem, há nenhuma reinicialização neste estágio.  
  
13. O convidado fornece as configurações de promoção para o serviço de servidor de função DS, que começa a promoção.  
  
14. O serviço de servidor de função DS interrompe todos os AD DS serviços relacionados (NTDS, NTFRS/DFSR, KDC, DNS).  
  
15. O convidado força NT5DS sincronização de tempo (Windows NTP) com outro controlador de domínio (em uma hierarquia de serviço de tempo do Windows padrão, isso significa que usando o PDCE). O convidado entra em contato com o PDCE. Liberam todos os tíquetes Kerberos existentes.  
  
16. O convidado configura os serviços DFSR ou NTFRS para ser executado automaticamente. O convidado exclui todos os arquivos de banco de dados DFSR e NTFRS existentes (padrão: c:\windows\ntfrs e c:\system information\dfsr\\ volume*< database_GUID >*), para forçar a sincronização não autorizada de SYSVOL quando o serviço é iniciado em seguida. O convidado não exclui o conteúdo do arquivo de SYSVOL para propagar previamente o SYSVOL quando a sincronização é iniciado mais tarde.  
  
17. O convidado é renomeado. O serviço de servidor de função DS no convidado começa a configuração do AD DS (promoção), usando o NTDS existente. Arquivo de banco de dados DIT como uma fonte, em vez do banco de dados de modelo incluído no c:\windows\system32 como normalmente faz uma promoção.  
  
18. O convidado entra em contato com o proprietário da função de RID Master FSMO para obter uma nova alocação de pool RID.  
  
19. O processo de promoção cria uma nova ID de invocação e recria o objeto de configurações NTDS para o controlador de domínio clonados (independentemente da clonagem, isso é parte da promoção de domínio ao usar um NTDS existente. Banco de dados DIT).  
  
20. NTDS replica em objetos que estão ausentes, mais recente, ou tem uma versão mais recente de um controlador de domínio do parceiro. NTDS. DIT já contém objetos desde o momento em que o controlador de domínio de origem deu offline e aqueles são usados para minimizar o tráfego de replicação possível entrada. As partições de catálogo global são preenchidas.  
  
21. O serviço DFSR ou FRS começa e porque não há nenhum banco de dados, SYSVOL sem autoridade sincroniza entrado de um parceiro de replicação. Esse processo novamente usa dados já existentes na pasta SYSVOL, para minimizar o tráfego de replicação de rede.  
  
22. O convidado reativa registro de cliente DNS agora que o computador é chamado de forma exclusiva e em rede.  
  
23. O convidado executa os módulos SYSPREP especificados pelo DefaultDCCloneAllowList.xml <SysprepInformation> elemento para eliminar referências para o SID e o nome do computador anterior.  
  
24. Promoção de clonagem é concluída.  
  
    1.  O convidado remove o sinalizador de inicialização DSRM para a próxima reinicialização será normal.  
  
    2.  O convidado renomeia o DCCloneConfig.xml com um carimbo de data e hora acrescentado, para que ele não é lido novamente na próxima inicialização para cima.  
  
    3.  O convidado remove o nome do valor do registro VdcIsCloning DWORD sob HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters.  
  
    4.  O convidado define DWORD "VdcCloningDone" nome do valor do registro na HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters para 0x1. Windows não usa esse valor, mas em vez disso, fornece-lo como um marcador de terceiros.  
  
25. O convidado atualiza o atributo msDS-GenerationID em seu próprio objeto de controlador de domínio clonados para corresponder o convidado atual ID de geração de VM.  
  
26. O convidado é reiniciado. Agora é um normal, controlador de domínio de publicidade.  
  
## <a name="BKMK_SafeRestoreArch"></a>Arquitetura de restauração segura do controlador de domínio virtualizado  
  
### <a name="overview"></a>Visão geral  
AD DS se baseia na plataforma do hipervisor para expor um identificador chamado **ID de geração de VM** para detectar a restauração do instantâneo de uma máquina virtual. AD DS inicialmente armazena o valor desse identificador em seu banco de dados (NTDS. DIT) durante a promoção de controlador de domínio. Quando um administrador restaura a máquina virtual de um instantâneo anterior, o valor atual da ID da VM geração da máquina virtual é comparado com o valor no banco de dados. Se os dois valores forem diferentes, o controlador de domínio redefine a ID de invocação e descarta o pool RID, desta forma evitando USN reutilizar ou a criação de possível de entidades de segurança duplicadas. Existem dois cenários onde restauração segura pode ocorrer:  
  
-   Quando um controlador de domínio virtual é iniciado após um instantâneo foi restaurado enquanto ele foi desligado  
  
-   Quando um instantâneo é restaurado em um controlador de domínio virtuais em execução  
  
    Se o controlador de domínio virtualizado no instantâneo está em um estado suspenso, em vez de desligamento, você precisa reiniciar o serviço do AD DS para disparar uma nova solicitação de pool RID. Você pode reiniciar o serviço do AD DS usando o snap-in Serviços ou usando o Windows PowerShell (serviço de reinicialização NTDS-forçar).  
  
As seções a seguir explicam restauração segura em detalhes para cada cenário.  
  
### <a name="safe-restore-detailed-processing"></a>Processamento de detalhadas de restauração segura  
O fluxograma a seguir mostra como segura restauração ocorre quando um controlador de domínio virtual é iniciado após um instantâneo foi restaurado enquanto ele foi desligado.  
  
![Arquitetura de DC virtualizada](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)  
  
1.  Quando a máquina virtual é inicializado após uma restauração do instantâneo, ele terá uma nova ID de geração de VM fornecido pelo host hipervisor devido a restauração do instantâneo.  
  
2.  O novo ID de geração de VM da máquina virtual é comparado com a ID de geração de VM no banco de dados. Porque as IDs de dois não corresponderem, ele utiliza proteções de virtualização (veja a etapa 3 na seção anterior). Depois que a restauração termina a aplicação, a VM-GenerationID definido em seu objeto de computador do AD DS é atualizado para corresponder a nova ID fornecer, o host de hipervisor.  
  
3.  O convidado emprega proteções de virtualização por:  
  
    1.  Invalidar o pool RID local.  
  
    2.  Definindo uma nova ID de invocação do banco de dados do controlador de domínio.  
  
> [!NOTE]  
> Essa parte da restauração segura sobrepõe-se o processo de clonagem. Embora esse processo seja sobre segurança restauração de um controlador de domínio virtual após reiniciar após uma restauração do instantâneo, as mesmas etapas que ocorrem durante o processo de clonagem.  
  
O diagrama a seguir mostra como proteções de virtualização evitar divergência induzida pela reversão do USN quando um instantâneo é restaurado em um controlador de domínio virtuais em execução.  
  
![Arquitetura de DC virtualizada](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)  
  
> [!NOTE]  
> A ilustração anterior é simplificada para explicar os conceitos.  
  
1.  No tempo T1, o administrador de hipervisor leva um instantâneo do DC1 virtual. DC1 neste momento tem um valor de USN (**highestCommittedUsn** na prática) de 100, valor de InvocationId (representado por ID no diagrama anterior) de um (na prática, isso deve ser GUID). O valor de savedVMGID é a VM-GenerationID no arquivo DIT do controlador de domínio (armazenadas contra o objeto de computador do controlador de domínio em um atributo denominado **msDS-GenerationId**). O VMGID é o valor atual de VM-GenerationId disponível do driver máquina virtual. Esse valor é fornecido pelo hipervisor.  
  
2.  Em um momento posterior T2, 100 usuários são adicionados neste controlador de domínio (considere a possibilidade dos usuários como um exemplo de atualizações que poderia ter sido realizado neste controlador de domínio entre tempo T1 e T2; essas atualizações podem na verdade ser uma combinação de criações de usuário, criações de grupo, atualizações de senha, atualizações de atributo e assim por diante). Neste exemplo, cada atualização consome uma USN exclusivo (embora na prática a criação de um usuário pode consumir mais de um USN). Antes de confirmar essas atualizações, DC1 verifica se o valor de GenerationID VM em seu banco de dados (savedVMGID) é igual ao valor atual disponível do driver (VMGID). Eles são iguais, como nenhum reversão aconteceu ainda, para que as atualizações estão comprometidas e USN move até 200, indicando que a próxima atualização pode usar USN 201. Não há nenhuma alteração nos InvocationId, savedVMGID ou VMGID. Essas atualizações replicam as DC2 no próximo ciclo de replicação. DC2 atualiza marca d'água alta (e **UptoDatenessVector**) representado aqui simplesmente como DC1(A) @USN = 200. Ou seja, DC2 está ciente de todas as atualizações de DC1 no contexto de InvocationId A até 200 USN.  
  
3.  No tempo T3, o instantâneo tirado tempo T1 é aplicado à DC1. DC1 foi revertida, portanto, seu USN reverte a 100, indicando que ele poderia usar USNs de 101 associar as atualizações posteriores. No entanto, nesse ponto, o valor de VMGID seria diferente no hipervisores que dão suporte a VM GenerationID.  
  
4.  Posteriormente, quando o DC1 faz qualquer atualização, ele verifica se o valor da VM-GenerationId que possui em seu banco de dados (savedVMGID) é igual ao valor do driver de máquina virtual (VMGID). Nesse caso, ele não é o mesmo, portanto, DC1 infere isso como indicativo de uma reversão e ele dispara proteções de virtualização; em outras palavras, ela redefine sua InvocationId (ID = B) e descarta o pool RID (não mostrado no diagrama anterior). Em seguida, salva o novo valor de VMGID em seu banco de dados e confirma essas atualizações (USN 101 - 250) no contexto do novo B. InvocationId No próximo ciclo de replicação, DC2 sabe nada com DC1 no contexto de InvocationId B, portanto, ele solicita tudo DC1 associado InvocationID B. Como resultado, as atualizações realizadas em DC1 logo após a aplicação de instantâneo com segurança convergirão. Além disso, o conjunto de atualizações que foram executadas em DC1 em T2 (que foram perdidas em DC1 após a restauração do instantâneo) seria replicar novamente em DC1 na próxima duplicação agendada porque eles tinham replicada para DC2 (conforme indicado pela linha pontilhada para DC1).  
  
Depois que o convidado emprega proteções de virtualização, NTDS replica entrada diferenças de objeto do Active Directory sem autoridade de um controlador de domínio do parceiro. O vetor UTDV do serviço de diretório de destino é atualizado de acordo. Em seguida, o convidado sincroniza SYSVOL:  
  
-   Se usar FRS, o convidado interrompe o serviço NTFRS e define o valor do registro BURFLAGS D2. Ele, em seguida, inicia o serviço NTFRS, que replica sem autoridade entrados, novamente usando inalterada SYSVOL dados existentes quando possível.  
  
-   Se usar DFSR, o convidado interrompe o serviço DFSR e exclui os arquivos de banco de dados DFSR (local padrão: %systemroot%\System. volume information\dfsr\\*<database GUID>*). Ele, em seguida, inicia o serviço DFSR, que replica sem autoridade entrados, novamente usando inalterada SYSVOL dados existentes quando possível.  
  
> [!NOTE]  
> -   Se o hipervisor não fornecer uma ID de geração de VM para comparação, o hipervisor não oferece suporte a proteções de virtualização e o convidado funcionará como um controlador de domínio virtualizado que executa o Windows Server 2008 R2 ou anterior. O convidado implementa a proteção de quarentena reversão USN se houver uma tentativa de iniciar a replicação com USNs não avançaram após o último USN mais alto visto pelo parceiro DC. Para obter mais informações sobre a proteção de quarentena reversão USN, consulte [USN e reversão do USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx)  
  


