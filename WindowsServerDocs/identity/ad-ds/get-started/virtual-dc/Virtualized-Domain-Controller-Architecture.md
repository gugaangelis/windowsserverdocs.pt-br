---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: Arquitetura do controlador de domínio virtualizado
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 04d50961aeb6190c15ba4c772afd8767d9d24f9b
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939006"
---
# <a name="virtualized-domain-controller-architecture"></a>Arquitetura do controlador de domínio virtualizado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico aborda a arquitetura da clonagem e da restauração segura de controlador de domínio virtualizado. Ele mostra a clonagem e a restauração segura de processos com fluxogramas. Em seguida, apresenta uma explicação detalhada de cada etapa do processo.

-   [Arquitetura da clonagem de controlador de domínio virtualizado](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)

-   [Arquitetura da restauração segura de controlador de domínio virtualizado](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)

## <a name="virtualized-domain-controller-cloning-architecture"></a><a name="BKMK_CloneArch"></a>Arquitetura da clonagem de controlador de domínio virtualizado

### <a name="overview"></a>Visão geral
A clonagem de controlador de domínio virtualizado conta com a plataforma de hipervisor para expor um identificador chamado **ID de geração de VM** para detectar a criação de uma máquina virtual. O AD DS armazena inicialmente o valor desse identificador em seu banco de dados (NTDS.DIT) durante a promoção do controlador de domínio. Quando a máquina virtual é inicializada, o valor atual da ID de geração de VM da máquina virtual é comparado com o valor no banco de dados. Se os dois valores forem diferentes, o controlador de domínio redefinirá a ID de invocação e descartará o pool RID, evitando, assim, a reutilização de USN ou a possível criação de entidades de segurança duplicadas. O controlador de domínio pesquisa o arquivo DCCloneConfig.xml nos locais destacados na Etapa 3, em [Processamento detalhado de clonagem](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails). Se ele localizar o arquivo DCCloneConfig.xml, concluirá que ele está sendo implantado como um clone e iniciará a clonagem para provisionar a si mesmo como um controlador de domínio adicional ao refazer a promoção usando o conteúdo existente em NTDS.DIT e SYSVOL, copiado da mídia de origem.

Em um ambiente misto no qual alguns hipervisores dão suporte de VM-GenerationID, mas outros não, é possível que uma mídia de clone seja implantada acidentalmente em um hipervisor que não dá suporte a VM-GenerationID. A presença do arquivo DCCloneConfig.xml indica a intenção administrativa de clonar um DC (controlador de domínio). Portanto, se o arquivo DCCloneConfig.xml for localizado durante a inicialização, mas um VM-GenerationID não tiver sido fornecido pelo host, o DC de clone será inicializado em DSRM (Modo de Restauração dos Serviços de Diretório) para impedir qualquer impacto no restante do ambiente. A mídia de clone poderá ser movida subsequentemente para um hipervisor que dê suporte à VM-GenerationID e, depois, a tentativa de clonagem poderá ser repetida.

Se a mídia de clone for implantada em um hipervisor que dê suporte a VM-GenerationID, mas o arquivo DCCloneConfig.xml não for fornecido, conforme o DC detectar uma alteração de VM-GenerationID entre seu DIT e o da nova VM (máquina virtual), ele acionará garantias para impedir a reutilização de USN e evitar SIDs duplicados. Porém, a clonagem não será iniciada e, portanto, o DC secundário continuará em execução sob a mesma identidade como o DC de origem. Esse DC secundário deve ser removido da rede o quanto antes para evitar quaisquer inconsistências no ambiente. Para obter mais informações sobre como recuperar esse controlador de domínio secundário, garantindo que as atualizações sejam replicadas de saída, consulte o artigo [2742970](https://support.microsoft.com/kb/2742970)do Microsoft KB.

### <a name="cloning-detailed-processing"></a><a name="BKMK_CloneProcessDetails"></a>Processamento detalhado de clonagem
O diagrama a seguir mostra a arquitetura de uma operação de clonagem inicial e de uma operação de repetição de clonagem. Esses processos serão explicados em mais detalhes posteriormente neste tópico.

**Operação de clonagem inicial**

![Arquitetura de DC virtualizado](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)

**Operação de repetição de clonagem**

![Arquitetura de DC virtualizado](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)

As seguintes etapas explicam o processo em mais detalhes:

1.  Um controlador de domínio da máquina virtual existente é inicializado em um hipervisor que dá suporte à ID de geração de VM.

    1.  Essa VM não tem um valor de ID de geração de VM existente definido em seu objeto de computador do AD DS depois da promoção.

    2.  Mesmo que seja nula, a próxima criação de computador significará que a clonagem ainda é feita, pois uma nova ID de geração de VM não terá correspondência.

    3.  A ID de geração de VM é definida depois da próxima reinicialização do DC e não é replicada.

2.  A máquina virtual lê a ID de geração de VM fornecida pelo driver VMGenerationCounter. Ela compara as duas IDs de geração de VM.

    1.  Se as IDs corresponderem, esta máquina virtual não é nova e a clonagem não prosseguirá. Se o arquivo DCCloneConfig.xml existir, o controlador de domínio renomeará o arquivo com um registro de data e hora para impedir a clonagem. O servidor continua com a inicialização normalmente. É dessa forma que funcionam todas as reinicializações de qualquer controlador de domínio virtual no Windows Server 2012.

    2.  Se as duas IDs não corresponderem, essa será uma máquina virtual nova que contém um NTDS.DIT de um controlador de domínio anterior (ou será um instantâneo restaurado). Se o arquivo DCCloneConfig.xml existir, o controlador de domínio prosseguirá com as operações de clonagem. Caso contrário, ele continuará com as operações de restauração de instantâneo. Consulte [Arquitetura da restauração segura de controlador de domínio virtualizado](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

    3.  Se o hipervisor não fornecer uma ID de geração de VM para a comparação, mas houver um arquivo DCCloneConfig.xml, o convidado renomeará o arquivo e será inicializado em DSRM para proteger a rede contra um controlador de domínio duplicado. Se não houver um arquivo dccloneconfig.xml, o convidado será inicializado normalmente (com a possibilidade de um controlador de domínio duplicado na rede). Para obter mais informações sobre como recuperar esse controlador de domínio duplicado, consulte o artigo [2742970](https://support.microsoft.com/kb/2742970) da Microsoft KB.

3.  O serviço NTDS verifica o valor do nome do valor de registro VDCisCloning DWORD (em HKEY_Local_Machine\System\CurrentControlSet\Services\Ntds\Parameters).

    1.  Se ele não existir, esta será a primeira tentativa de clonar essa máquina virtual. O convidado implementa as garantias de duplicação de objeto VDC relacionadas à invalidação do pool RID local e à definição de uma nova ID de inovação de replicação para o controlador de domínio.

    2.  Se ela já estiver definida como 0x1, está será uma tentativa de clonagem de "repetição", na qual uma operação anterior de clonagem falhou. As medidas de segurança de duplicação do objeto VDC não são adotadas, pois já deveriam ter sido executadas uma vez antes e alterariam desnecessariamente o convidado várias vezes.

4.  O nome do valor de registro IsClone DWORD é gravado (em Hkey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters)

5.  O serviço NTDS altera o sinalizador de inicialização do convidado para ser iniciado em modo de reparação DS em todas as reinicializações futuras.

6.  O serviço NTDS tenta ler o DcCloneConfig.xml em um dos três locais aceitos (Diretório de Trabalho DSA, %windir%\NTDS ou mídia removível de leitura/gravação, na ordem da letra da unidade, na raiz da unidade).

    1.  Se o arquivo não existir em nenhum local válido, o convidado verificará o endereço IP quanto a duplicação. Se o endereço IP não estiver duplicado, o servidor será inicializado normalmente. Se houver um endereço IP duplicado, o computador será inicializado em DSRM para proteger a rede contra um controlador de domínio duplicado.

    2.  Se o arquivo existir em um local válido, o serviço NTDS validará suas configurações. Se o arquivo estiver em branco (ou qualquer configuração específica estiver em branco), o NTDS definirá valores automáticos para essas configurações.

    3.  Se o DcCloneConfig.xml existir, mas contiver qualquer entrada inválida ou ilegível, a clonagem falhará e o convidado será inicializado em DSRM (Modo de Restauração dos Serviços de Diretório).

7.  O convidado desabilita todos os registros automáticos de DNS (Sistema de Nomes de Domínio) para impedir o roubo acidental do nome e dos endereços IP do computador de origem.

8.  O convidado interrompe o serviço de logon de rede para impedir qualquer anúncio ou resposta das solicitações de rede do AD DS de clientes.

9. O NTDS valida que não existem serviços ou programas instalados que não façam parte do DefaultDCCloneAllowList.xml ou do CustomDCCloneAllowList.xml

    1.  Se existirem serviços ou programas instalados que não estejam na lista de permissão de exclusão padrão ou personalizada, a clonagem falhará e o convidado será inicializado em DSRM para proteger a rede contra um controlador de domínio duplicado.

    2.  Se não existirem incompatibilidades, a clonagem continuará.

10. Se o endereçamento IP automático for usado devido a configurações de rede de DCCloneConfig.xml em branco, o convidado habilitará DHCP nos adaptadores de rede para obter informações de concessão de endereço IP, roteamento de rede e resolução de nomes.

11. O convidado localiza e contata o controlador de domínio que executa a função FSMO do emulador PDC (controlador de domínio primário). Isso usa DNS e o protocolo DCLocator. Ele faz uma conexão RPC (chamada de procedimento remoto) e chama o método IDL_DRSAddCloneDC para clonar o objeto de computador do controlador de domínio.

    1.  Se o objeto de computador de origem do convidado tiver uma permissão principal estendida do domínio para "'Permitir que um controlador de domínio crie um clone dele mesmo", a clonagem continuará.

    2.  Se o objeto de computador de origem do convidado não tiver essa permissão estendida, a clonagem falhará e o convidado será inicializado em DSRM para proteger a rede contra um controlador de domínio duplicado.

12. O nome do objeto de computador do AD DS é definido para corresponder ao nome especificado no DCCloneConfig.xml, caso exista, ou gerado automaticamente no PDCE. O NTDS cria o objeto de configuração NTDS correto para o site lógico do Active Directory adequado.

    1.  Se esta for uma clonagem PDC, o convidado renomeará o computador local e será reinicializado. Após a reinicialização, passará pela etapa 1-10 novamente e, em seguida, vai para a etapa 13.

    2.  Se esta for uma clonagem de DC de réplica, não haverá reinicialização neste estágio.

13. O convidado fornece as configurações de promoção ao serviço Servidor de Função de SD, que inicia a promoção.

14. O serviço Servidor de Função de SD interrompe todos os serviços relacionados ao AD DS (NTDS, NTFRS/DFSR, KDC, DNS).

15. O convidado força a sincronização de tempo NT5DS (Windows NTP) com outro controlador de domínio (em uma hierarquia padrão do Serviço de Horário do Windows, isso significa usar o PDCE). O convidado contata o PDCE. Todos os tíquetes Kerberos existentes são liberados.

16. O convidado configura os serviços DFSR ou NTFRS para que sejam executados automaticamente. O convidado exclui todos os arquivos de banco de dados DFSR e NTFRS existentes (padrão: c:\windows\ntfrs e c:\System Volume Information\DFSR \\ *<database_GUID>*), a fim de forçar a sincronização não autoritativa do SYSVOL quando o serviço é iniciado pela próxima vez. O convidado não exclui o conteúdo de arquivo de SYSVOL para propagar previamente o SYSVOL quando a sincronização for iniciada depois.

17. O convidado é renomeado. O serviço Servidor de Função de SD no convidado inicia a configuração do AD DS (promoção) usando o arquivo de banco de dados NTDS.DIT existente como uma origem, em vez do banco de dados modelo incluído em c:\windows\system32, como uma promoção normalmente faz.

18. O convidado contata o detentor da função FSMO do mestre RID para obter uma nova alocação de pool RID.

19. O processo de promoção cria uma nova ID de invocação e recria o objeto de configurações NTDS para o controlador de domínio clonado (independentemente da clonagem, isso faz parte da promoção do domínio ao usar um banco de dados NTDS.DIT existente).

20. O NTDS é replicado em objetos que estão ausentes, são mais novos ou têm uma versão posterior de um controlador de domínio parceiro. O NTDS.DIT já contém objetos do momento em que o controlador de domínio de origem ficou offline, e esses objetos são usados onde possível para minimizar a entrada do tráfego de replicação. As partições do catálogo global são preenchidas.

21. O serviço DFSR ou FRS é iniciado e, como não há um banco de dados, o SYSVOL sincroniza a entrada de modo não autoritativo com base em um parceiro de replicação. Esse processo reutiliza os dados pré-existentes na pasta SYSVOL para minimizar o tráfego de replicação de rede.

22. O convidado habilita novamente o registro de cliente DNS agora que o computador está nomeado e conectado à rede de modo exclusivo.

23. O convidado executa os módulos SYSPREP especificados pelo elemento DefaultDCCloneAllowList.xml <SysprepInformation> para remover referências ao nome e SID prévios do computador.

24. A promoção da clonagem é concluída.

    1.  O convidado remove o sinalizador de inicialização de DSRM para que a próxima reinicialização seja normal.

    2.  O convidado renomeia o DCCloneConfig.xml com um registro de data e hora acrescido para que ele não seja lido novamente na próxima inicialização.

    3.  O convidado remove o nome do valor de registro VdcIsCloning DWORD em HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters.

    4.  O convidado define o nome do valor de registro "VdcCloningDone" DWORD em HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters como 0x1. O Windows não usa esse valor, mas o fornece como um marcador para terceiros.

25. O convidado atualiza o atributo msDS-GenerationID em seu próprio objeto de controlador de domínio clonado para corresponder à ID de geração de VM do convidado atual.

26. O convidado é reiniciado. Trata-se agora de um controlador de domínio normal e anunciador.

## <a name="virtualized-domain-controller-safe-restore-architecture"></a><a name="BKMK_SafeRestoreArch"></a>Arquitetura da restauração segura de controlador de domínio virtualizado

### <a name="overview"></a>Visão geral
O AD DS conta com a plataforma de hipervisor para expor um identificador chamado **ID de geração de VM** para detectar a restauração de instantâneo de uma máquina virtual. O AD DS armazena inicialmente o valor desse identificador em seu banco de dados (NTDS.DIT) durante a promoção do controlador de domínio. Quando o administrador restaura a máquina virtual de um instantâneo anterior, o valor atual da ID de Geração de VM da máquina virtual é comparado ao valor no banco de dados. Se os dois valores forem diferentes, o controlador de domínio redefinirá a ID de invocação e descartará o pool RID, evitando, assim, a reutilização de USN ou a possível criação de entidades de segurança duplicadas. Existem dois cenários nos quais a restauração segura pode ocorrer:

-   Quando um controlador de domínio virtual é iniciado depois que um instantâneo é restaurado quando o controlador estava desligado

-   Quando um instantâneo é restaurado em um controlador de domínio virtual em execução

    Se o controlador de domínio virtualizado no instantâneo estiver em estado suspenso, e não desligado, você deverá reiniciar o serviço do AD DS para acionar uma nova solicitação de pool RID. Você pode reiniciar o serviço do AD DS usando o snap-in Serviços ou usando o Windows PowerShell (Restart-Service NTDS -force).

As seções a seguir explicam a restauração segura em detalhes para cada cenário.

### <a name="safe-restore-detailed-processing"></a>Processamento detalhado da restauração segura
O fluxograma a seguir mostra como ocorre a restauração segura quando um controlador de domínio virtual é iniciado depois que um instantâneo foi restaurado enquanto o controlador estava desligado.

![Arquitetura de DC virtualizado](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)

1.  Quando a máquina virtual é inicializada depois da restauração do instantâneo, ela tem uma nova ID de geração de VM fornecida pelo host do hipervisor em virtude da restauração do instantâneo.

2.  A nova ID de geração de VM da máquina virtual é comparada com a ID de geração de VM no banco de dados. Como as duas IDs não correspondem, ela emprega garantias de virtualização (consulte a etapa 3 da seção anterior). Depois que a aplicação da restauração é concluída, a VM-GenerationID definida em seu objeto de computador do AD DS é atualizado para corresponder à nova ID fornecida pelo host do hipervisor.

3.  O convidado emprega garantias de virtualização ao:

    1.  Invalidar o pool RID local.

    2.  Definir uma nova ID de invocação para o banco de dados do controlador de domínio.

> [!NOTE]
> Essa parte da restauração segura se sobrepõe ao processo de clonagem. Embora este processo esteja relacionado à restauração segura de um controlador de domínio virtual quando ele é inicializado depois de uma restauração de instantâneo, as mesmas etapas ocorrem durante o processo de clonagem.

O diagrama a seguir mostra como as garantias de virtualização impedem a divergência induzida pela reversão de USN quando um instantâneo é restaurado em um controlador de domínio virtual em execução.

![Arquitetura de DC virtualizado](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)

> [!NOTE]
> A ilustração anterior foi simplificada para explicar os conceitos.

1.  No horário T1, o administrador do hipervisor faz um instantâneo do DC1 virtual. Nesse horário, o DC1 tem um valor de USN (**highestCommittedUsn**, na prática) definido como 100 e valor de InvocationId (representado como ID no diagrama anterior) definido como A (na prática, isso deveria ser GUID). O valor de savedVMGID é o VM-GenerationID no arquivo DIT do DC (armazenado no objeto de computador do DC, em um atributo denominado **msDS-GenerationId**). O VMGID é o valor atual da VM-GenerationId disponível no driver da máquina virtual. Esse valor é fornecido pelo hipervisor.

2.  No horário T2, 100 usuários são adicionados a esse DC (considere usuários como um exemplo de atualizações que poderiam ter sido realizadas nesse DC entre T1 e T2; essas atualizações poderim, na verdade, ser uma combinação de criações de usuários, criações de grupos, atualizações de senhas, atualizações de atributos e assim por diante). Neste exemplo, cada atualização consome um USN exclusivo (embora, na prática, uma criação de usuário possa consumir mais que um USN). Antes de confirmar essas atualizações, o DC1 verifica se o valor de VM-GenerationID em seu banco de dados (savedVMGID) é o mesmo que o valor atual disponível no driver (VMGID). Eles são os mesmos, já que nenhuma reversão ocorreu ainda. Portanto, as atualizações são confirmadas e o USN muda para 200, indicando que a próxima atualização pode usar o USN 201. Não há alteração em InvocationId, savedVMGID nem VMGID. Essas atualizações são replicadas no DC2 no próximo ciclo de replicação. DC2 atualiza a marca d' água alta de ti (e **UptoDatenessVector**) representada aqui simplesmente como DC1 (A) @USN = 200. Ou seja, o DC2 reconhece todas as atualizações do DC1 no contexto do InvocationId A por meio do USN 200.

3.  No horário T3, o instantâneo feito no horário T1 é aplicado ao DC1. O DC1 sofreu reversão. Assim, seu USN é revertido para 100, indicando que ele poderia usar USNs a partir de 101 para se associar às atualizações subsequentes. No entanto, nesse ponto, o valor de VMGID seria diferente nos hipervisores que dão suporte a VM-GenerationID.

4.  Em seguida, quando o DC1 realiza qualquer atualização, ele verifica se o valor de VM-GenerationId que ele tem em seu banco de dados (savedVMGID) é o mesmo valor do driver da máquina virtual (VMGID). Nesse caso, se o valor não for o mesmo, o DC1 deduzirá isso como indicativo de uma reversão e acionará garantias de virtualização. Em outras palavras, ele redefinirá seu InvocationId (ID = B) e descartará o pool RID (não mostrado no diagrama anterior). Em seguida, ele salva o novo valor de VMGID em seu banco de dados e confirma essas atualizações (USN 101-250) no contexto da nova invocação B. No próximo ciclo de replicação, DC2 não sabe nada de DC1 no contexto de invocaid B, então ele solicita tudo, do DC1 associado à invocação B. Como resultado, as atualizações executadas no DC1 após a aplicação do instantâneo serão convergidas com segurança. Além disso, o conjunto de atualizações realizadas no DC1 no horário T2 (que foram perdidas no DC1 depois da restauração do instantâneo) seria replicado de volta no DC1 na próxima replicação agendada, pois tais atualizações foram replicadas no DC2 (como indicado pela linha pontilhada que retorna ao DC1).

Depois que o convidado emprega garantias de virtualização, o NTDS replica internamente as diferenças de objeto do Active Directory de modo não autoritativo em um controlador de domínio parceiro. O vetor de atualidade do serviço de diretório de destino é atualizado adequadamente. Em seguida, o convidado sincroniza SYSVOL:

-   Se estiver usando FRS, o convidado interromperá o serviço NTFRS e definirá o valor de registro D2 BURFLAGS. Então, ele iniciará o serviço NTFRS, que é replicado internamente e de modo não autoritativo, reutilizando, onde possível, os dados de SYSVOL existentes e não alterados.

-   Se estiver usando o DFSR, o convidado interromperá o serviço DFSR e excluirá os arquivos de banco de dados DFSR (local padrão:%SystemRoot%\System Volume Information\DFSR \\ *<database GUID>* ). Então, ele iniciará o serviço DFSR, que é replicado internamente e de modo não autoritativo, reutilizando, onde possível, os dados de SYSVOL existentes e não alterados.

> [!NOTE]
> -   Se o hipervisor não fornecer uma ID de geração de VM para comparação, o hipervisor não suportará as garantias de virtualização e o convidado funcionará como um controlador de domínio virtualizado que executa o Windows Server 2008 R2 ou anterior. O convidado implementa a proteção de quarentena da reversão de USN caso haja uma tentativa de iniciar a replicação com USNs que não ultrapassaram o último USN observado pelo controlador de domínio parceiro. Para obter mais informações sobre proteção de quarentena da reversão de USN, consulte [USN e reversão do USN](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10))

