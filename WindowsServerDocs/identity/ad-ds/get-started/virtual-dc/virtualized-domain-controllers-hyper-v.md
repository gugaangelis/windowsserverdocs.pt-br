---
Title: Virtualizando controladores de domínio usando o Hyper-V
description: Considerações a fazer quando a virtualização do Windows Server Active Directory controladores de domínio no Hyper-V
author: MicrosoftGuyJFlo
ms.author: joflore
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.openlocfilehash: 684f3418bf336af4959282e7a8c2088d22a8c8dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865127"
---
# <a name="virtualizing-domain-controllers-using-hyper-v"></a>Virtualizando controladores de domínio usando o Hyper-V

> Aplica-se a: Windows Server 2016

Este tópico será atualizado para tornar as diretrizes aplicáveis ao Windows Server 2016. Windows Server 2012 apresenta vários aprimoramentos para controladores de domínio virtualizado (DCs), incluindo garantias para impedir que a reversão do USN em controladores de domínio virtuais e a capacidade de clonar controladores de domínio virtuais. Para obter mais informações sobre esses aprimoramentos, consulte [Introdução à virtualização de serviços de domínio Active Directory (AD DS) (nível 100)](../../introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md).

Hyper-V consolida diferentes funções de servidor em um único computador físico. Este guia descreve os controladores de domínio em execução como sistemas operacionais convidados de 32 bits ou 64 bits.

## <a name="planning-to-virtualize-domain-controllers"></a>Planejando virtualizar controladores de domínio

Esta seção aborda os requisitos de hardware para o servidor do Hyper-v, como evitar pontos únicos de falha, selecionando o tipo apropriado de configuração de servidores Hyper-V e máquinas virtuais e as decisões de segurança e desempenho.

## <a name="hyper-v-requirements"></a>Requisitos do Hyper-V

Para instalar e usar a função Hyper-V, você deve ter o seguinte:

   - **X64 processador**
      - Hyper-V está disponível nas versões baseadas em x64 do Windows Server 2008 ou posterior.  
   - **Virtualização assistida por hardware**
      - Esse recurso está disponível em processadores que incluem uma opção de virtualização, especificamente, Tecnologia de Virtualização da Intel (Intel VT) ou Virtualização AMD (AMD-V).  
   - **Proteção de execução de dados de hardware (DEP)**
      - Hardware DEP deve estar disponível e habilitado. Especificamente, você deve habilitar o bit Intel XD (bit execute disable) ou o bit AMD NX (nenhum bit no execute).  

## <a name="avoid-creating-single-points-of-failure"></a>Evitar a criação de pontos de falha únicos

Procure evitar criar pontos únicos de falha em potencial quando planejar a implantação do controlador de domínio virtual. Para isso, implemente a redundância do sistema. Por exemplo, considere as seguintes recomendações e tenha em mente a possibilidade de aumentos no custo de administração:

1. Execute pelo menos dois controladores de domínio virtualizados por domínio em diferentes hosts de virtualização, o que diminui o risco de perder todos os controladores de domínio se um único host de virtualização falhar.  
2. Conforme recomendável para outras tecnologias, diversifique o hardware (usando diferentes CPUs, placas-mãe, adaptadores de rede ou outros elementos de hardware) no qual os controladores de domínio são executados. A diversificação do hardware limita o dano que pode ocorrer devido a um problema de funcionamento específico de uma configuração de fornecedor, de um driver ou de um único componente ou tipo de hardware.  
3. Se possível, os controladores de domínio devem ser executados em hardware localizado em diferentes regiões do mundo. Isso ajuda a reduzir o impacto de um desastre ou de uma falha que afeta um site no qual os controladores de domínio estão hospedados.  
4. Mantenha os controladores de domínio físicos em cada um dos domínios. Isso ameniza o risco de um problema no funcionamento da plataforma de virtualização que afeta todos os sistemas de host que usam a plataforma.  

## <a name="security-considerations"></a>Considerações sobre segurança

O computador host no qual os controladores de domínio virtuais são executados deve ser gerenciado tão cuidadosamente quanto um controlador de domínio gravável, mesmo se o computador for apenas associado a um domínio ou de grupo de trabalho. Essa é uma consideração de segurança importante. Um host mal gerenciado está vulnerável a um ataque de elevação de privilégio, que ocorre quando um usuário mal-intencionado ganha privilégios de acesso e sistema que não foram autorizados ou atribuídos de forma legítima. Um usuário mal-intencionado pode usar esse tipo de ataque para comprometer todas as máquinas virtuais, domínios e florestas que esse computador hospeda.

Certifique-se de manter as seguintes considerações de segurança em mente ao planejar virtualizar controladores de domínio:

   - O administrador local de um computador que hospeda controladores de domínio virtuais e graváveis deve ser considerado equivalente em credenciais ao administrador de domínio padrão de todos os domínios e florestas aos quais esses controladores de domínio pertencem.  
   - A configuração recomendada para evitar problemas de segurança e desempenho é um host que executa uma instalação Server Core do Windows Server 2008 ou posterior, sem aplicativos diferentes do Hyper-V. Essa configuração limita o número de aplicativos e serviços que estão instalados no servidor, que deve resultar em maior desempenho e menos aplicativos e serviços que podem ser explorados com más intenções para atacar o computador ou rede. O efeito desse tipo de configuração é conhecido como uma superfície de ataque reduzida. Em uma filial ou outros locais que não podem ser protegidos satisfatoriamente, é recomendado um controlador de domínio somente leitura (RODC). Se existir uma rede de gerenciamento separada, recomendamos que o host seja conectado apenas à rede de gerenciamento.  
   - Você pode usar o Bitlocker com seus controladores de domínio, desde o Windows Server 2016, você pode usar o recurso de TPM virtual também oferecem o material da chave de convidado para desbloquear o volume do sistema.
   - [Malha de protegidos e VMs blindadas](/it-server/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) pode fornecer controles adicionais para proteger os controladores de domínio.

Para obter informações sobre RODCs, consulte [guia de implantação e planejamento do controlador de domínio somente leitura](../../deploy/rodc/read-only-domain-controller-updates.md).

Para obter mais informações sobre como proteger controladores de domínio, consulte [Best Practice Guide for Securing Active Directory Installations](../../plan/security-best-practices/best-practices-for-securing-active-directory.md).

## <a name="security-boundaries-for-different-host-and-guest-configurations"></a>Limites de segurança para configurações de convidado e host diferente

O uso de máquinas virtuais torna possível ter muitas configurações diferentes de controladores de domínio. Considere cuidadosamente o modo que as máquinas virtuais afetam os limites e as confianças na sua topologia do Active Directory. As possíveis configurações para um controlador de domínio do Active Directory e computadores host (servidor Hyper-V) e seus computadores convidados (máquinas virtuais executadas no servidor Hyper-V) são descritas na tabela a seguir.

|Computador|Configuração 1|Configuração 2|
|-------|---------------|---------------|
|Host|Computador membro ou do grupo de trabalho|Computador membro ou do grupo de trabalho|
|Convidado|Controlador de domínio|Computador membro ou do grupo de trabalho|

![](media/virtualized-domain-controller-architecture/Dd363553.f44706fd-317e-4f0b-9578-4243f4db225f(WS.10).gif)

   - O administrador no computador host tem o mesmo acesso que um administrador de domínio nos convidados do controlador de domínio gravável e deve ser tratado dessa forma. No caso de um convidado RODC, o administrador do computador host tem o mesmo acesso que um administrador local no RODC convidado.   
   - Um controlador de domínio em uma máquina virtual terá os direitos administrativos no host se o host estiver vinculado ao mesmo domínio. Há uma oportunidade para um usuário mal-intencionado comprometer todas as máquinas virtuais se pela primeira vez, o usuário mal-intencionado obtiver acesso à máquina Virtual 1. Isso é conhecido como um vetor de ataque. Se houver controladores de domínio em vários domínios ou florestas, esses domínios deverão ter uma administração centralizada na qual o administrador de um domínio seja confiável em todos os domínios.  
   - A chance para atacar da Máquina virtual 1 existirá mesmo de ela estiver instalar como um RODC. Embora um administrador de um RODC não tenha explicitamente direitos do administrador de domínio, o RODC pode ser usado para enviar diretivas para o computador host. Essas diretivas podem incluir scripts de inicialização. Se essa operação for bem sucedida, o computador host poderá estar comprometido, e poderá ser usado para comprometer as outras máquinas virtuais no computador host.  

## <a name="security-of-vhd-files"></a>Segurança dos arquivos VHD

Um arquivo VHD de um controlador de domínio virtual é equivalente ao disco rígido físico de um controlador de domínio físico. Como tal, ele deve ser protegido com o mesmo cuidado que se tem ao proteger o disco rígido de um controlador de domínio físico. Certifique-se de que apenas administradores confiáveis tenham acesso permitido aos arquivos VHD do controlador de domínio.

## <a name="rodcs"></a>RODCs

Um benefício dos RODCs é a capacidade de colocá-los em locais onde a segurança física não pode ser garantida, como em filiais. Você pode usar a criptografia de unidade de disco BitLocker do Windows para proteger os próprios arquivos VHD (não os sistemas de arquivos contido nele) contra o comprometimento no host pelo roubo do disco físico. 
<!-- Removed link to Windows Server 2008 Hyper-V and BitLocker Drive Encryption (http://go.microsoft.com/fwlink/?linkid=123534). Link is dead. -->

## <a name="performance"></a>Desempenho

Com a nova arquitetura de 64 bits microkernel, há aumentos significativos no desempenho do Hyper-V de plataformas de virtualização anteriores. Para melhor desempenho de host, o host deve ser uma instalação Server Core do Windows Server 2008 ou posterior, e ele não deve ter funções de servidor que não sejam o Hyper-V instalado.

O desempenho das máquinas virtuais depende especificamente da carga de trabalho. Para garantir um desempenho satisfatório do Active Directory, teste topologias específicas. Avaliar a carga de trabalho atual durante um período de tempo com uma ferramenta como a confiabilidade e o Monitor de desempenho (Perfmon. msc) ou o [Microsoft Assessment and Planning (MAP) toolkit](https://go.microsoft.com/fwlink/?linkid=137077). A ferramenta MAP também poderá ser valiosa se você quiser fazer um inventário de todos os servidores e funções de servidor que existem na sua rede atualmente.

Para obter uma ideia geral do desempenho de controladores de domínio virtualizados, os testes de desempenho a seguir foram realizados com o [desempenho Testando a ferramenta Active Directory (ADTest.exe)](https://go.microsoft.com/fwlink/?linkid=137088).

Os testes do LDAP foram realizados em um controlador de domínio físico com ADTest.exe e, em seguida, realizados em uma máquina virtual que estava hospedada em um servidor idêntico ao controlador de domínio físico. Somente um processador lógico foi usado no computador físico e somente um processador virtual foi usado na máquina virtual para atingir facilmente 100 por cento de utilização da CPU. Na tabela a seguir, a letra e número em parêntesis após cada teste indicam o teste específico em ADTest.exe. Como mostram esses dados, desempenho do controlador de domínio virtualizado foi de 88 a 98 por cento do desempenho do controlador de domínio físico.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Medida</th>
<th>Testar</th>
<th>Físico</th>
<th>Virtual</th>
<th>Delta</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Pesquisas/s</p></td>
<td><p>Procura nome comum no escopo base (L1)</p></td>
<td><p>11508</p></td>
<td><p>10276</p></td>
<td><p>-10.71%</p></td>
</tr>
<tr class="even">
<td><p>Pesquisas/s</p></td>
<td><p>Procura um conjunto de atributos no escopo base (L2)</p></td>
<td><p>10123</p></td>
<td><p>9005</p></td>
<td><p>-11.04%</p></td>
</tr>
<tr class="odd">
<td><p>Pesquisas/s</p></td>
<td><p>Procura todos os atributos no escopo base (L3)</p></td>
<td><p>1284</p></td>
<td><p>1242</p></td>
<td><p>-3.27%</p></td>
</tr>
<tr class="even">
<td><p>Pesquisas/s</p></td>
<td><p>Procura nome comum no escopo de subárvore (L6)</p></td>
<td><p>8613</p></td>
<td><p>7904</p></td>
<td><p>-8.23%</p></td>
</tr>
<tr class="odd">
<td><p>Associações bem-sucedidas/s</p></td>
<td><p>Realizar ligações rápidas (B1)</p></td>
<td><p>1438</p></td>
<td><p>1374</p></td>
<td><p>-4.45%</p></td>
</tr>
<tr class="even">
<td><p>Associações bem-sucedidas/s</p></td>
<td><p>Realizar ligações simples (B2)</p></td>
<td><p>611</p></td>
<td><p>550</p></td>
<td><p>-9.98%</p></td>
</tr>
<tr class="odd">
<td><p>Associações bem-sucedidas/s</p></td>
<td><p>Usar NTLM para realizar ligações (B5)</p></td>
<td><p>1068</p></td>
<td><p>1056</p></td>
<td><p>-1.12%</p></td>
</tr>
<tr class="even">
<td><p>Grava/seg.</p></td>
<td><p>Gravar vários atributos (W2)</p></td>
<td><p>6467</p></td>
<td><p>5885</p></td>
<td><p>-9.00%</p></td>
</tr>
</tbody>
</table>

Para garantir um desempenho satisfatório, os componentes de integração (IC) foram instalados para permitir que o sistema operacional convidado use unidades de disco sintéticas de conhecimento de hypervisor ou de "esclarecimentos". Durante o processo de instalação, pode ser necessário usar drivers IDE ou de adaptador de rede emulados. Em ambientes de produção, você deve substituir essas unidades de disco emuladas por unidades de disco sintéticas para aumentar o desempenho.

Ao monitorar o desempenho de máquinas virtuais com o Monitor de Desempenho e Confiança (Perfmon.msc), dentro da máquina virtual as informações da CPU não serão totalmente precisas como resultado do modo como a CPU virtual é programada no processador físico. Quando você quiser obter informações da CPU de uma máquina virtual executada em um servidor Hyper-V, use os contadores do Processador Lógico Hypervisor do Hyper-V na partição host.

Para obter mais informações sobre o desempenho de ajuste do AD DS e do Hyper-V, consulte [desempenho ajustando as diretrizes para o Windows Server 2016](../../../../administration/performance-tuning/index.md).
<!-- Updated to 2016 perf guidance -->

Além disso, não planeje usar um VHD de disco diferente em uma máquina virtual configurada como um controlador de domínio porque ele pode reduzir o desempenho. Para saber mais sobre os tipos de disco do Hyper-V, incluindo discos diferentes, consulte [Assistente novo disco rígido Virtual](http://go.microsoft.com/fwlink/?linkid=137279).
<!-- Couldn't find an equivalent WS 2016 Hyper-V article. -->

Para obter informações adicionais sobre o AD DS em ambientes de hospedagem virtual, consulte [coisas a considerar quando você hospeda controladores de domínio do Active Directory em ambientes de hospedagem virtuais](https://go.microsoft.com/fwlink/?linkid=141292) na Base de dados de Conhecimento Microsoft.

## <a name="deployment-considerations-for-virtualized-domain-controllers"></a>Considerações sobre implantação para controladores de domínio virtualizados

Há várias práticas comuns de máquina virtual que você deve evitar ao implantar controladores de domínio e considerações especiais para sincronização de hora e o armazenamento.

## <a name="virtualization-deployment-practices-to-avoid"></a>Práticas de implantação de virtualização a serem evitadas

As plataformas de virtualização, como o Hyper-V, oferecem uma série de recursos convenientes que tornam o gerenciamento, a manutenção, o backup e a migração de computadores mais fáceis. No entanto, as seguintes práticas comuns de implantação e os recursos não devem ser usados para controladores de domínio virtual:

   - Para garantir a durabilidade de gravações do Active Directory, não implante arquivos de banco de dados do controlador de domínio virtual (Active Directory banco de dados (NTDS. DIT), logs e SYSVOL) em discos virtuais do IDE. Em vez disso, crie um segundo VHD anexado a um controlador SCSI virtual e certifique-se de que o banco de dados, logs e SYSVOL são colocados no disco de SCSI da máquina virtual durante a instalação do controlador de domínio.  
   - Não implemente VHDs (discos rígidos virtuais) diferenciais em uma máquina virtual que você esteja configurando como um controlador de domínio. Isso torna a reversão para uma versão anterior muito fácil, e também reduz o desempenho. Para obter mais informações sobre tipos de VHD, consulte [Assistente novo disco rígido Virtual](https://go.microsoft.com/fwlink/?linkid=137279).  
   - Não implante novos domínios do Active Directory e florestas em uma cópia de um sistema operacional Windows Server que não foi primeiro preparado usando a ferramenta System Preparation (Sysprep). Para obter mais informações sobre como executar o Sysprep, consulte [visão geral do Sysprep (preparação do sistema)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)

      > [!WARNING]
      > Não há suporte para a execução de Sysprep em um controlador de domínio.

   - Para ajudar a evitar uma possível situação de reversão do USN (número de sequência de atualização), não use cópias de um arquivo VHD que represente um controlador de domínio já implantado para implantar controladores de domínio adicionais. Para obter mais informações sobre a reversão do USN, consulte [USN e reversão de USN](#usn-and-usn-rollback).
      - Windows Server 2012 e mais recente permite aos administradores clonar imagens de controlador de domínio se preparado adequadamente quando desejam implantar controladores de domínio adicionais
   - Não use o recurso Exportar do Hyper-V para exportar uma máquina virtual que esteja executando um controlador de domínio.
      - Com o Windows Server 2012 e mais recente, uma exportação e importação de um convidado virtual do controlador de domínio é tratada como uma restauração não autoritativa conforme ele detecta uma alteração da ID de geração e ele não está configurado para clonagem.
      - Verifique se que você não estiver usando o convidado que você exportou mais.
  - Você pode usar a replicação do Hyper-V para manter uma segunda cópia inativa de um controlador de domínio. Se você iniciar a imagem replicada, você também precisará executar a limpeza adequada, pelo mesmo motivo que não usar a fonte depois de exportar uma imagem de convidado do controlador de domínio.

## <a name="physical-to-virtual-migration"></a>Migração física para virtual

O System Center Virtual Machine Manager (VMM) 2008 fornece gerenciamento unificado de máquinas físicas e virtuais. Ele também fornece o recurso para migrar uma máquina física para uma máquina virtual. Esse processo é conhecido como conversão de máquina física em virtual (conversão P2V). Durante o processo de conversão P2V, a nova máquina virtual e o controlador de domínio físico que está sendo migrado não devem estar executando ao mesmo tempo, para evitar uma situação de reversão de USN, conforme descrito em [USN e reversão de USN](#usn-and-usn-rollback).

Você deve executar a conversão P2V usando o modo offline para que os dados do diretório estejam consistentes quando o controlador de domínio for ligado novamente. A opção de modo offline é oferecida e recomendada no Assistente para Converter Servidor Físico. Para obter uma descrição da diferença entre o modo online e o modo offline, consulte [P2V: Convertendo computadores físicos em máquinas virtuais no VMM](https://go.microsoft.com/fwlink/?linkid=155072). Durante a conversão P2V, a máquina virtual não deve estar conectada à rede. O adaptador de rede da máquina virtual deve ser habilitado somente depois que o processo de conversão P2V é concluída e verificada. Neste ponto, a máquina de origem física estará desligada. Não coloque a máquina de origem física de volta na rede antes de reformatar o disco rígido.

> [!NOTE]
> Há opções mais seguras para criar novos controladores de domínio virtuais que não executam os riscos de criação de uma reversão de USN. Você pode configurar um novo controlador de domínio virtual por promoção regular, a promoção da instalação da mídia (IfM) e também usando a clonagem do controlador de domínio, se você já tiver pelo menos um controlador de domínio virtual.
Isso também ajuda a evitar problemas com hardware ou problemas relacionados à plataforma convertido de P2V convidados virtuais podem ser encontrados.

> [!WARNING]
> Para evitar problemas com replicação do Active Directory, certifique-se de que apenas uma instância (física ou virtual) de um determinado domínio controlador existe em uma determinada rede em qualquer ponto no tempo.
> Você pode reduzir a probabilidade do clone antigo, sendo um problema:
> 
> - Quando o novo controlador de domínio virtual está em execução, altere a senha da conta de computador usando duas vezes: resetpwd netdom. exe /Server: < controlador de domínio >...
> - Exportar e importar o novo convidado virtual para forçá-lo, tornando-se uma nova ID de geração e, portanto, ID de invocação de um banco de dados.
> 

## <a name="using-p2v-migration-to-create-test-environments"></a>Usando a migração P2V para criar ambientes de teste

Você pode usar a migração P2V no VMM para criar ambientes de teste. Você pode migrar controladores de domínio de produção de máquinas físicas para máquinas virtuais para criar um ambiente de teste sem reduzir permanentemente os controladores de domínio de produção. No entanto, o ambiente de teste deverá estar em uma rede diferente do ambiente de produção se existirem duas instâncias do mesmo controlador de domínio. Tome muito cuidado ao criar ambientes de teste com a migração P2V para evitar reversões de USN que podem afetar seus ambientes de teste e produção. Este é um método que você pode usar para criar ambientes de teste com P2V.

Um controlador de domínio em produção de cada domínio é migrado para uma máquina virtual de teste usando P2V de acordo com as diretrizes mencionadas na seção de migração física para virtual. As máquinas de produção físicas e as máquinas virtuais de teste deverão estar em redes diferentes quando ficarem online novamente. Para evitar reversões de USN no ambiente de teste, todos os controladores de domínio a serem migrados de máquinas físicas para virtuais devem ficar offline. (Você pode fazer isso interrompendo o serviço ntds ou reiniciando o computador no DSRM (Modo de Restauração dos Serviços de Diretório).) Depois que os controladores de domínio estiverem offline, nenhuma nova atualização deverá ser introduzida ao ambiente. Os computadores devem permanecer offline durante a migração P2V; nenhum dos computadores deve ficar online até que todos os computadores tenham sido migrados totalmente. Para saber mais sobre a reversão do USN, consulte o USN e reversão de USN.

Os controladores de domínio de teste subsequentes devem ser promovidos como réplicas no ambiente de teste.

## <a name="time-service"></a>Serviço de data/hora

Para máquinas virtuais que são configuradas como controladores de domínio, é recomendável que você desabilite a sincronização de horário entre o sistema e o convidado sistema operacional do host que atua como um controlador de domínio. Isso permite que seu controlador de domínio convidado sincronizar a hora da hierarquia de domínio.

Para desabilitar o provedor de sincronização de tempo do Hyper-V, desligue a VM e desmarque a caixa de seleção de sincronização de tempo em Integration Services.

> [!NOTE]
> Este guia foi atualizado recentemente para refletir a recomendação atual para sincronizar a hora para o controlador de domínio da hierarquia de domínio apenas convidados, em vez da recomendação anterior para desabilitar a sincronização de horário entre o host parcialmente controlador de domínio do sistema e convidado.

## <a name="storage"></a>Armazenamento

Para otimizar o desempenho da máquina virtual do controlador de domínio e garantir a durabilidade de gravações do Active Directory, use as seguintes recomendações para armazenar arquivos VHD de sistema operacional e do Active Directory:

   - **Armazenamento convidado**. Armazene o arquivo de banco de dados do Active Directory (Ntds.dit), os arquivos de log e os arquivos SYSVOL em um disco virtual separado dos arquivos do sistema operacional. Crie um segundo VHD anexado a um controlador SCSI virtual e armazene o banco de dados, logs e SYSVOL no disco de SCSI virtual da máquina virtual. Discos SCSI virtuais fornecem melhor desempenho em comparação ao IDE virtual e eles dão suporte ao acesso de unidade forçado (FUA). FUA garante que o sistema operacional grava e lê os dados diretamente da mídia de ignorar todos os mecanismos de cache.

   > [!NOTE]
   > Se você estiver planejando usar o Bitlocker para o convidado virtual do controlador de domínio, você precisa certificar-se de que os volumes adicionais são configurados para "o desbloqueio automático".
   > Para obter mais informações sobre a configuração automática de desbloqueio pode ser encontrado no [BitLockerAutoUnlock Enable](https://docs.microsoft.com/powershell/module/bitlocker/enable-bitlockerautounlock)

   - **Armazenamento do host de arquivos VHD**. Recomendações: As recomendações do armazenamento do host abordam o armazenamento de arquivos VHD. Para obter um máximo desempenho, não armazene arquivos VHD em um disco que seja usado frequentemente por outros serviços ou aplicativos, como o disco do sistema no qual o sistema operacional Windows host está instalado. Armazene cada arquivo VHD em uma partição separada do sistema operacional host e de quaisquer outros arquivos VHD. A configuração ideal é armazenar cada arquivo VHD em uma unidade de disco física separada.  

    O sistema de disco físico do host também deve satisfazer **pelo menos um** dos critérios a seguir para atender aos requisitos de carga de trabalho virtualizada de integridade de dados:  

      - O sistema usa discos de classe de servidor (SCSI, Fibre Channel).  
      - O sistema torna-se de que os discos estão conectados a um bateria cache adaptador de barramento host (HBA).  
      - O sistema usa um controlador de armazenamento (por exemplo, um sistema RAID) como o dispositivo de armazenamento.  
      - O sistema torna-se de que a energia para o disco está protegida por uma fonte de alimentação ininterrupta (UPS).  
      - O sistema torna-se de que o recurso de cache de gravação do disco está desabilitado.  

   - **VHD fixo versus discos de passagem**. Há muitas maneiras de configurar o armazenamento para máquinas virtuais. Quando os arquivos VHD são usados, os VHDs de tamanho fixo são mais eficientes que os VHDs dinâmicos, porque a memória dos VHDs de tamanho fixo é alocada quando eles são criados. Os discos de passagem, que as máquinas virtuais podem usar para acessar a mídia de armazenamento físico, estão ainda mais otimizados para desempenho. Os discos de passagem são essencialmente discos físicos ou LUNs (números de unidade lógica) conectados a uma máquina virtual. Os discos de passagem não oferecem suporte ao recurso de instantâneo. Por isso, eles são a configuração de disco rígido preferencial, pois o uso de instantâneos com controladores de domínio não é recomendável.  

Para reduzir a chance de corrupção de dados do Active Directory, use controladores SCSI virtuais:

   - Use unidades físicas SCSI (em oposição às unidades IDE/ATA) nos servidores Hyper-V que hospedam controladores de domínio virtuais. Se você não puder usar unidades SCSI, verifique se o armazenamento de gravação em cache está desabilitado nas unidades ATA/IDE que hospedam controladores de domínio virtuais. Para obter mais informações, consulte [ID do evento 1539 – integridade do banco de dados](https://go.microsoft.com/fwlink/?linkid=162419).
   - Para garantir a durabilidade do Active Directory grava, o banco de dados do Active Directory, logs e SYSVOL deve ser colocado em um disco virtual de SCSI. Discos SCSI virtuais dão suporte a acesso de unidade forçado (FUA). FUA garante que o sistema operacional grava e lê os dados diretamente da mídia de ignorar todos os mecanismos de cache.  

## <a name="operational-considerations-for-virtualized-domain-controllers"></a>Considerações operacionais para controladores de domínio virtualizados

Os controladores de domínio executados em máquinas virtuais têm restrições operacionais que não se aplicam aos controladores de domínio executados em máquinas físicas. Ao usar um controlador de domínio virtualizado, há alguns recursos e práticas do software de virtualização que você não deve usar:

   - Não pause, interrompa nem armazene o estado salvo de um controlador de domínio em uma máquina virtual por períodos de tempo maiores que o tempo de vida da marca de exclusão da floresta e depois continue a partir do estado pausado ou salvo. Fazer isso pode interferir na replicação. Para saber como determinar o tempo de vida da marca para exclusão da floresta, consulte [determinar o tempo de vida da marca para exclusão da floresta](https://go.microsoft.com/fwlink/?linkid=137177).  
   - Não copie nem clone VHDs (discos rígidos virtuais). Mesmo com as proteções em vigor para a VM convidada, VHDs individuais ainda podem ser copiados e fazer com que a reversão do USN.
   - Não pegue nem use um instantâneo de um controlador de domínio virtual. Tecnicamente, ele é compatível com Windows Server 2012 e mais recente, não é uma substituição para uma boa estratégia de backup. Há alguns motivos para tirar instantâneos do controlador de domínio ou restaurando os instantâneos.
   - Não use um VHD diferencial em uma máquina virtual configurada como um controlador de domínio. Isso torna a reversão para uma versão anterior muito fácil e também reduz o desempenho.  
   - Não use o recurso Exportar em uma máquina virtual que esteja executando um controlador de domínio.  
   - Não restaure um controlador de domínio nem tente reverter o conteúdo de um banco de dados do Active Directory de outra forma que não seja usando um backup com suporte. Para obter mais informações, consulte [Backup e restaurar as considerações para controladores de domínio virtualizados](#backup-and-restore-practices-to-avoid).  

Todas essas recomendações são feitas para ajudar a evitar a possibilidade de uma reversão do USN (número de sequência de atualização). Para obter mais informações sobre a reversão do USN, consulte USN e reversão de USN.

## <a name="backup-and-restore-considerations-for-virtualized-domain-controllers"></a>Considerações sobre backup e restauração dos controladores de domínio virtualizados

O backup de controladores de domínio é um requisito crítico para qualquer ambiente. Os backups protegem contra perda de dados no caso de uma falha ou erro administrativo do controlador de domínio. Se esse evento ocorrer, será necessário reverter o estado do sistema do controlador de domínio para um ponto no tempo antes da falha ou erro. O método com suporte de restaurar um controlador de domínio para um estado íntegro é usar um aplicativo de backup compatível com Active Directory, como o Backup do Windows Server, para restaurar um backup de estado do sistema originado da instalação atual do controlador de domínio. Para obter mais informações sobre como usar o Backup do Windows Server com os serviços de domínio do Active Directory (AD DS), consulte o [Backup do AD DS e o guia passo a passo de recuperação](https://go.microsoft.com/fwlink/?LinkId=138501).

Com a tecnologia de máquina virtual, determinados requisitos das operações de restauração do Active Directory assumem significância extra. Por exemplo, se você restaurar um controlador de domínio usando uma cópia do arquivo VHD, ignore a etapa crítica de atualizar a versão do banco de dados de um controlador de domínio depois de ser restaurado. A replicação continuará com números de rastreamento inadequados, resultando em um banco de dados inconsistente entre réplicas do controlador de domínio. Na maioria dos casos, esse problema passa desapercebido pelo sistema de replicação e nenhum erro é reportado, apesar das inconsistências entre os controladores de domínio.

Há uma maneira com suporte para realizar o backup e restauração de um controlador de domínio virtualizado:

1. Executar o Backup do Windows Server no sistema operacional convidado.  

Com o Windows Server 2012 e hosts do Hyper-V mais recentes e convidados, você poderá fazer backups com suporte de controladores de domínio usando instantâneos, convidado VM exportação e importação e também a replicação do Hyper-V. No entanto todos esses não são uma boa opção para criar um histórico de backup apropriado, com a pequena exceção de exportação VM de convidado.

Com o Windows Server 2016 Hyper-V há suporte para "instantâneos de produção" em que o servidor Hyper-V dispara um backup baseado em VSS do convidado e quando o convidado é feito com o instantâneo, o host busca os VHDs e armazena-os no local de backup.

Embora isso funcione com o Windows Server 2012 e mais recente, há uma incompatibilidade com o Bitlocker:

- Ao fazer uma captura de ajuste do VSS, AD deseja executar uma tarefa de pós-instantâneo para marcar o banco de dados como provenientes de um backup ou, no caso de uma fonte IFM Preparando-se para o RODC, remova as credenciais do banco de dados.
- Quando o Hyper-V monta o volume de instantâneo para essa tarefa, não há nenhum recurso que seria desbloquear o Volume para acesso sem criptografia. Portanto, o mecanismo de banco de dados do AD Falha ao acessar o banco de dados e, eventualmente, falhará o instantâneo.

> [!NOTE]
> O projeto VM blindado mencionado anteriormente tem um host Hyper-V controlado por backup como uma não-meta máximo para proteção de dados da VM convidada.

## <a name="backup-and-restore-practices-to-avoid"></a>Práticas de backup e restauração a serem evitadas

Como mencionado, os controladores de domínio executados em máquinas virtuais têm restrições que não se aplicam aos controladores de domínio executados em máquinas físicas. Ao fazer backup ou restaurar um controlador de domínio virtual, há determinados recursos e práticas do software de virtualização que você não deve usar:

   - Não copie nem clone arquivos VHD de controladores de domínio em vez de executar backups regulares. Se o arquivo VHD for copiado ou clonado, ele se tornará obsoleto. Em seguida, se o VHD for iniciado no modo normal, você encontrará uma reversão de USN. Você deve realizar as operações de backup adequadas que têm suporte dos Serviços de Domínio Active Directory (AD DS), como usar o recurso Backup do Windows Server.  
   - Não use o recurso Instantâneo como backup para restaurar uma máquina virtual que foi configurada como um controlador de domínio. Problemas ocorrerão com replicação quando você reverter a máquina virtual para um estado anterior com o Windows Server 2008 R2 e anteriores. Para obter mais informações, consulte [USN e reversão de USN](#usn-and-usn-rollback). Embora o uso de um instantâneo para restaurar um controlador de domínio somente leitura (RODC) não cause problemas de replicação, esse método de restauração ainda não é recomendado.  

## <a name="restoring-a-virtual-domain-controller"></a>Restaurando um controlador de domínio virtual

Para restaurar um controlador de domínio quando ele falha, você deve fazer backup de estado do sistema regularmente. O estado do sistema inclui dados e arquivos de log do Active Directory, o Registro, o volume do sistema (pasta SYSVOL) e vários elementos do sistema operacional. Esse requisito é o mesmo para controladores de domínio físicos e virtuais. Os procedimentos de restauração do estado do sistema que os aplicativos de backup compatíveis com Active Directory realizam foram desenvolvidos para garantir a consistência dos bancos de dados locais e replicados do Active Directory após um processo de restauração, incluindo a notificação de redefinições da ID de invocação para parceiros de replicação. Entretanto, o uso de ambientes de hospedagem virtual ou aplicativos de imagem do sistema operacional ou disco torna possível que os administradores ignorem as verificações e as validações que normalmente ocorrem quando o estado de sistema do controlador de domínio é restaurado.

Quando uma máquina virtual do controlador de domínio falha e não ocorre uma reversão de USN, há duas situações com suporte para restaurar a máquina virtual:

   - Se um backup de dados do estado do sistema válido que pré-data a falha existir, você poderá restaurar o estado do sistema usando a opção de restauração do utilitário de backup usado para criar o backup. O backup de dados do estado do sistema deve ter sido criado usando um utilitário de backup compatível com Active Directory dentro do tempo de vida da marca de exclusão, que é por padrão, não mais que 180 dias. Você deve fazer backup dos seus controladores de domínio pelo menos a cada meio tempo de vida da marca de exclusão. Para obter instruções sobre como determinar o tempo de vida da marca para exclusão específico para sua floresta, consulte [determinar o tempo de vida da marca para exclusão da floresta](https://go.microsoft.com/fwlink/?linkid=137177).  
   - Se uma cópia funcional do arquivo VHD estiver disponível, mas o backup de estado do sistema estiver indisponível, você poderá remover a máquina virtual existente. Restaure a máquina virtual existente usando uma cópia anterior do VHD, mas certifique-se de iniciá-la no Modo de Restauração dos Serviços de Diretório (DSRM) e configurar o Registro corretamente, conforme descrito na próxima seção. Em seguida, reinicie o controlador de domínio no modo normal.

Use o processo na ilustração a seguir para determinar a melhor maneira de restaurar seu controlador de domínio virtualizado.

![](media\virtualized-domain-controller-architecture\Dd363553.85c97481-7b95-4705-92a7-006e48bc29d0(WS.10).gif)

Nos RODCs, o processo de restauração e as decisões são mais simples.

![](media\virtualized-domain-controller-architecture\Dd363553.4c5c5eda-df95-4c6b-84e0-d84661434e5d(WS.10).gif)

## <a name="restoring-the-system-state-backup-of-a-virtual-domain-controller"></a>Restaurando o backup de estado do sistema de um controlador de domínio virtual

Se um backup de estado do sistema válido existir na máquina virtual do controlador de domínio, você poderá restaurar de forma segura o backup seguindo o procedimento de restauração descrito pela ferramenta de backup usada para fazer backup do arquivo VHD.

> [!IMPORTANT]
> Para restaurar corretamente o controlador de domínio, você deve reiniciá-lo no DSRM. Você não deve permitir que o controlador de domínio inicie no modo normal. Se você perder a oportunidade de acessar o DSRM durante a inicialização do sistema, desligue a máquina virtual do controlador de domínio antes que ela possa iniciar completamente no modo normal. É importante iniciar o controlador de domínio no DSRM porque iniciá-lo no modo normal incrementará seus USNs, mesmo se o controlador de domínio estiver desconectado da rede. Para obter mais informações sobre a reversão do USN, consulte USN e reversão de USN. 

## <a name="to-restore-the-system-state-backup-of-a-virtual-domain-controller"></a>Para restaurar o backup de estado do sistema de um controlador de domínio virtual

1. Inicie a máquina virtual do controlador de domínio e pressione F5 para acessar a tela do Gerenciador de Inicialização do Windows. Se você for solicitado a inserir as credenciais de conexão, imediatamente clique no botão **Pausar** na máquina virtual para que ela não continue a inicializar. Em seguida, insira suas credenciais de conexão e clique no botão **Reproduzir** na máquina virtual. Clique dentro da janela da máquina virtual e, em seguida, pressione F5.

   Se você não vir a tela do Gerenciador de Inicialização do Windows e o controlador de domínio começar a inicializar no modo normal, desligue a máquina virtual para evitar que ela complete a inicialização. Repita essa etapa quantas vezes for necessário até que você possa acessar a tela do Gerenciador de Inicialização do Windows. Você não pode acessar DSRM no menu do Windows Error Recovery. Por isso, desative a máquina virtual e tente novamente se o menu do Windows Error Recovery aparecer.

2. Na tela do Gerenciador de Inicialização do Windows, pressione F8 para acessar opções de inicialização avançada.
3. Na tela **Opções de Inicialização Avançadas**, selecione **Modo de Restauração dos Serviços de Diretório** e pressione ENTER. Isso inicia o controlador de domínio no DSRM.
4. Use o método de restauração adequado para a ferramenta usada para criada o backup de estado do sistema. Se você usou o Backup do Windows Server, consulte [executar uma restauração não autoritativa de AD DS](https://go.microsoft.com/fwlink/?linkid=132637).

## <a name="restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available"></a>Restaurando um controlador de domínio virtual quando um backup de dados de estado do sistema adequado não está disponível

Se você não tiver um backup de dados de estado do sistema que pré-date a falha da máquina virtual, será possível usar um arquivo VHD anterior para restaurar o controlador de domínio executado em uma máquina virtual. Se possível, faça uma cópia do VHD para que, se encontrar um problema durante o procedimento ou perder uma etapa, você possa fazer uma nova tentativa usando VHD copiado.

> [!IMPORTANT]
> - Não considere usar o procedimento a seguir como substituto de backups agendados e planejados regularmente.
> - **As restaurações realizadas com o procedimento a seguir não têm suporte pela Microsoft e devem ser usadas somente quando não há nenhuma outra alternativa.**
> - Não use este procedimento se a cópia do VHD que você está prestes a restaurar foi inicializada no modo normal por qualquer máquina virtual.

## <a name="to-restore-a-previous-version-of-a-virtual-domain-controller-vhd-without-system-state-data-backup"></a>Para restaurar uma versão anterior de um VHD de controlador de domínio virtual sem o backup de dados de estado do sistema

1. Usando o VHD anterior, inicie o controlador de domínio virtual no DSRM, conforme descrito na seção anterior. Não permita que o controlador de domínio inicie no modo normal. Se você não vir a tela do Gerenciador de Inicialização do Windows e o controlador de domínio começar a inicializar no modo normal, desligue a máquina virtual para evitar que ela complete a inicialização. Consulte a seção anterior para obter instruções detalhadas para acessar o DSRM.
2. Abra o Editor do Registro. Para abrir o Editor do Registro, clique em **Iniciar**, **Executar**, digite **regedit** e clique em OK. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**. No Editor do Registro, expanda o seguinte caminho: **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\NTDS\\Parameters**. Procure um valor chamado **Contagem da restauração anterior DSA**. Se existir um valor, anote a configuração. Se não houver o valor, a configuração será igual ao padrão, que é zero. Não adicione um valor se você não vir um lá.
3. Clique com o botão direito do mouse em **Parâmetros**, clique em **Novo** e, em seguida, clique em **Valor DWORD (32 bits)**.
4. Digite o novo nome **Banco de dados restaurado do backup** e, em seguida, pressione ENTER.
5. Clique duas vezes no valor que você acabou de criar para abrir a caixa de diálogo **Editar valor DWORD (32 bits)** e, em seguida, digite **1** na caixa **Dados do valor**. O **banco de dados restaurado da entrada de backup** opção está disponível nos controladores de domínio que estejam executando o Windows 2000 Server com Service Pack 4 (SP4), Windows Server 2003 com as atualizações que estão incluídas no [como detectar e se recuperar de uma reversão de USN no Windows Server 2003, Windows Server 2008 e Windows Server 2008 R2](https://go.microsoft.com/fwlink/?linkid=137182) na Base de dados de Conhecimento da Microsoft instalado e Windows Server 2008.
6. Reinicie o controlador de domínio no modo normal.
7. Quando o controlador de domínio reiniciar, abra o Visualizador de Eventos. Para abrir o Visualizador de Eventos, clique em **Iniciar**, em **Painel de Controle**, clique duas vezes em **Ferramentas Administrativas** e, em seguida, clique duas vezes em **Visualizador de Eventos**.
8. Expanda **Logs de Aplicativos e Serviços** e, em seguida, clique no log **Serviços de Diretório**. Verifique se esses eventos aparecem no painel de detalhes.
9. Clique com o botão direito do mouse no log **Serviços de Diretório** e, em seguida, clique em **Localizar**. Em **Localizar**, digite **1109** e clique em **Localizar Próximo**.
10. Você deve ver pelo menos uma entrada da ID de evento 1109. Se você não vir essa entrada, vá para a próxima etapa. Caso contrário, clique duas vezes na entrada e analise o texto que confirma que a atualização foi feita no InvocationID:

   ```
   Active Directory has been restored from backup media, or has been configured to host an application partition. 
   The invocationID attribute for this directory server has been changed. 
   The highest update sequence number at the time the backup was created is <time>

   InvocationID attribute (old value):<Previous InvocationID value>
   InvocationID attribute (new value):<New InvocationID value>
   Update sequence number:<USN>

   The InvocationID is changed when a directory server is restored from backup media or is configured to host a writeable application directory partition.
   ```

11. Feche o Visualizador de Eventos.
12. Use o Editor do Registro para verificar se o valor em **Contagem da restauração anterior DSA** é igual ao valor anterior mais um. Se este não for o valor correto e você não puder encontrar uma entrada para a ID de Evento 1109 no Visualizador de Eventos, verifique se os service packs do controlador de domínio estão atualizados. Não é possível repetir este procedimento no mesmo VHD. Você pode tentar novamente em uma cópia do VHD ou em outro VHD que não tenha sido iniciado no modo normal repetindo a etapa 1.
13. Feche o Editor do Registro.

## <a name="usn-and-usn-rollback"></a>USN e reversão de USN

Esta seção descreve problemas de replicação que podem ocorrer como resultado de uma restauração incorreta do banco de dados do Active Directory com uma versão mais antiga de uma máquina virtual. Para obter mais detalhes sobre o processo de replicação do Active Directory, consulte [conceitos de replicação do Active Directory](../replication/active-directory-replication-concepts.md)

<!-- Replaced this link with 2016 article: [How the Active Directory Replication Model Works](http://go.microsoft.com/fwlink/?linkid=27636) (http://go.microsoft.com/fwlink/?LinkID=27636). -->

## <a name="usns"></a>USNs

Os Serviços de Domínio Active Directory (AD DS) usam números de sequência de atualização (USNs) para acompanhar a replicação de dados entre os controladores de domínio. Cada vez que uma alteração é feita aos dados no diretório, o USN é incrementado para indicar que uma alteração foi feita.

Para cada partição de diretório que um controlador de domínio de destino armazena, USNs são usados para controlar a atualização de origem mais recente que um controlador de domínio introduzido no seu banco de dados, bem como o status de cada controlador de domínio que armazena uma réplica das partição de diretório. Quando controladores de domínio replicam as alterações entre si, eles consultam seus parceiros de replicação para que as alterações com USNs maiores que o USN da última alteração que o controlador de domínio recebeu de cada parceiro.

As seguintes duas tabelas de metadados de replicação contêm USNs. Os controladores de domínio de origem e de destino os usam para filtrar alterações que o controlador de domínio de destino exige.

1. **Vetor atual**: Uma tabela que o controlador de domínio de destino mantém para acompanhar as atualizações de origem que são recebidas de todos os controladores de domínio de origem. Quando um controlador de domínio de destino solicita alterações para uma partição de diretório, ela fornece seu vetor atual para o controlador de domínio de origem. O controlador de domínio de origem, em seguida, usa esse valor para filtrar as atualizações que ele envia para o controlador de domínio de destino. O controlador de domínio de origem envia seu vetor atual para o destino após a conclusão de um ciclo de replicação com êxito para garantir que o controlador de domínio de destino, sabe que ele sincronizou com todos os controladores de domínio atualizações de origem e as atualizações estão no mesmo nível como a origem.  
2. **Marca d'água alta**: Um valor que o controlador de domínio de destino mantém para acompanhar as alterações mais recentes que recebeu de um controlador de domínio de origem específico para uma partição específica. A marca d'água alta impede que o controlador de domínio de origem envie as alterações que já recebeu pelo controlador de domínio de destino.  

## <a name="directory-database-identity"></a>Identidade do banco de dados do diretório

Além dos USNs, os controladores de domínio acompanham o banco de dados do diretório dos parceiros de replicação de origem. A identidade do banco de dados do diretório executado no servidor é mantida separadamente da identidade do próprio objeto do servidor. A identidade do banco de dados de diretório em cada controlador de domínio é armazenada na **invocationID** atributo do objeto Configurações NTDS, que está localizado no seguinte caminho Lightweight Directory Access Protocol (LDAP): cn = NTDS As configurações, cn = ServerName, cn = Servers, cn =*SiteName*, cn = Sites, cn = Configuration, dc =*ForestRootDomain*. A identidade do objeto do servidor é armazenada no atributo **objectGUID** do objeto Configurações de NTDS. A identidade do objeto do servidor nunca muda. No entanto, a identidade das alterações do banco de dados de diretório quando um procedimento de restauração de estado do sistema ocorre no servidor ou quando uma partição de diretório de aplicativo é adicionada, removido, em seguida e posteriormente adicionado novamente do servidor. (outro cenário: quando uma instância do Hyper-v dispara seus gravadores VSS em uma partição que contém VHD de um controlador de domínio virtual, o convidado por sua vez dispara seus próprios gravadores VSS (o mesmo mecanismo usado pelo backup/restauração acima) resulta em outro meio pelo qual o invocationID é Redefinir)

Consequentemente, **invocationID** relaciona efetivamente um conjunto de atualizações de origem em um controlador específico a uma versão específica do banco de dados do diretório. O vetor de atualização e o HIGHWATER marcam o uso de tabelas do **invocationID** e a GUID do controlador de domínio, respectivamente, para que as informações de replicação de banco de dados nesse domínio controladores saber de qual cópia do Active Directory está chegando.

O **invocationID** é um valor de GUID que pode ser visto próximo à parte superior da saída depois que você executa o comando **repadmin /showrepl**. O seguinte texto representa uma saída de exemplo do comando:

   ```
   Repadmin: running command /showrepl against full DC local host
   Default-First-Site-Name\VDC1
   DSA Options: IS_GC
   DSA object GUID: 966651f3-a544-461f-9f2c-c30c91d17818
   DSA invocationID: b0d9208b-8eb6-4205-863d-d50801b325a9
   ```

Quando o AD DS é restaurado corretamente em um controlador de domínio, o **invocationID** é redefinido. Como resultado dessa alteração, você terá um aumento no tráfego de replicação – cuja duração é relativo ao tamanho da partição que está sendo replicado

Por exemplo, suponha que VDC1 e DC2 são dois controladores de domínio no mesmo domínio. A figura a seguir mostra a percepção do DC2 sobre o VDC1 quando o valor invocationID é redefinido em uma situação de restauração adequada.

![](media\virtualized-domain-controller-architecture\Dd363553.ca71fc12-b484-47fb-991c-5a0b7f516366(WS.10).gif)

## <a name="usn-rollback"></a>Reversão de USN

A reversão de USN ocorre quando as atualizações normais dos USNs são descartadas e um controlador de domínio tenta usar um USN mais baixo que sua atualização mais recente. A reversão de USN será detectada e a replicação será interrompida antes que a divergência na floresta é criada, na maioria dos casos. 

A reversão de USN pode ser causada de várias maneiras, por exemplo, quando arquivos antigos do disco rígido virtual (VHD) são usados ou uma conversão físico para virtual (conversão P2V) é realizada sem garantir que a máquina física fique offline permanentemente após a conversão. Tome as seguintes precauções para garantir que a reversão de USN não ocorra:

   - Quando não executando o Windows Server 2012 ou mais recente, não pegue nem use um instantâneo de uma máquina de virtual do controlador de domínio.
   - Não copie o arquivo VHD do controlador de domínio.  
   - Quando não executando o Windows Server 2012 ou mais recente, não exporte a máquina virtual que está executando um controlador de domínio.  
   - Não restaure um controlador de domínio nem tente reverter o conteúdo de um banco de dados do Active Directory de outra forma que não seja uma solução de backup com suporte, como o Backup do Windows Server.  

Em alguns casos, a reversão de USN pode não ser detectada. Em outros casos, ela pode causar outros erros de replicação. Nesses casos, é necessário identificar a extensão do problema e resolvê-lo em tempo hábil. Para obter informações sobre como remover objetos remanescentes podem ocorrer como resultado da reversão do USN, consulte [objetos do Active Directory desatualizados geram a identificação de evento 1988 no Windows Server 2003](https://go.microsoft.com/fwlink/?linkid=137185) na Base de dados de Conhecimento Microsoft.

## <a name="usn-rollback-detection"></a>Detecção de reversão de USN

Na maioria dos casos, as reversões de USN sem uma redefinição correspondente do **invocationID** causada por procedimentos de restauração inadequados são detectadas. O Windows Server 2008 fornece proteções contra replicação inapropriada após uma operação de restauração inadequada do controlador de domínio. Essas proteções são acionadas porque uma operação de restauração inadequada resulta em USNs mais baixos que representam alterações de origem que os parceiros de replicação já receberam.

No Windows Server 2008 e no Windows Server 2003 SP1, quando um controlador de domínio de destino solicita alterações com um USN usado anteriormente, a resposta de seu parceiro de replicação de origem é interpretada pelo controlador de domínio de destino para significar que seus metadados de replicação estão desatualizados. Isso indica que o banco de dados do Active Directory no controlador de domínio de origem foi revertido para um estado anterior. Por exemplo, o arquivo VHD de uma máquina virtual foi revertido para uma versão anterior. Neste caso, o controlador de domínio de destino inicia as seguintes medidas de quarentena no controlador de domínio que enfrentou uma restauração inadequada:

   - O AD DS pausa o serviço de logon de rede, o que evita a mudança de senhas nas contas do usuário e do computador. Essa ação evitará a perda de tais alterações se elas ocorrerem após uma restauração inadequada.  
   - O AD DS desabilita a replicação de entrada e de saída do Active Directory.  
   - O AD DS gera a ID do evento 2095 no log de eventos do Serviço de Diretório para indicar a condição.  

A ilustração a seguir mostra a sequência de eventos que ocorre quando a reversão de USN é detectada no VDC2, o controlador de domínio de destino executado em uma máquina virtual. Nesta ilustração, a detecção da reversão de USN ocorre no VDC2 quando um parceiro de replicação detecta que o VDC2 enviou um valor USN atual visto anteriormente pelo controlador de domínio de destino, que indica que o banco de dados do VDC2 foi revertido inadequadamente.

![](media\virtualized-domain-controller-architecture\\Dd363553.373b0504-43fc-40d0-9908-13fdeb7b3f14(WS.10).gif)

Se o log de eventos do Serviço de Diretório reportar uma ID do evento 2095, complete o procedimento a seguir imediatamente.

## <a name="to-resolve-event-id2095"></a>Para resolver a ID do evento 2095

1. Isole a máquina virtual que registrou o erro da rede.
2. Tente determinar se alguma alteração foi originada desse controlador de domínio e propagou-se para outros controladores de domínio. Se o evento foi um resultado de um instantâneo ou cópia de uma máquina virtual sendo inicializada, tente determinar o horário em que a reversão de USN ocorreu. Você pode verificar os parceiros de replicação daquele controlador de domínio para determinar se a replicação ocorreu desde então.

   Você pode usar a ferramenta Repadmin para criar essa determinação. Para obter informações sobre como usar Repadmin, consulte [monitoramento e solução de problemas do Active Directory Replication usando Repadmin](https://go.microsoft.com/fwlink/?linkid=122830). Se você não puder determinar isso sozinho, entre em contato com [Microsoft Support](https://support.microsoft.com) para obter assistência.

3. Force o rebaixamento do controlador de domínio. Isso envolve limpar os metadados do controlador de domínio e executar as funções de mestre de operações (também conhecidas como FSMO (flexible single master operations)). Para obter mais informações, consulte a seção "Recuperando da reversão do USN" [como detectar e recuperar de uma reversão de USN no Windows Server 2003, Windows Server 2008 e Windows Server 2008 R2](https://go.microsoft.com/fwlink/?linkid=137182) na Base de dados de Conhecimento Microsoft.
4. Exclua todos os arquivos VHD antigos no controlador de domínio.

## <a name="undetected-usn-rollback"></a>Reversão de USN não detectada

A reversão de USN pode não ser detectada em uma de duas circunstâncias:

1. O arquivo VHD é anexado a máquinas virtuais diferentes executadas em vários locais simultaneamente.  
2. O USN no controlador de domínio restaurado aumentou o último USN que o outro controlador de domínio recebeu.  

Na primeira circunstância, outros controladores de domínio podem ser replicados com uma das máquinas virtuais e alterações podem ocorrer nas máquinas virtuais sem serem replicadas na outra. Essa divergência da floresta é difícil de detectar e causará respostas imprevisíveis do diretório. Essa situação poderá ocorrer após uma migração P2V se a máquina física e a virtual forem executadas na mesma rede. Isso poderá acontecer se vários controladores de domínio virtuais forem criados a partir do mesmo controlador de domínio físico e, em seguida, executados na mesma rede.

Na segunda circunstância, uma variedade de USNs aplica-se a dois conjuntos diferentes de alterações. Isso pode continuar em períodos estendidos sem ser detectado. Sempre que um objeto criado durante esse período for modificado, um objeto persistente será detectado e reportado como ID do evento 1988 no Visualizador de Eventos. A ilustração a seguir mostra como a reversão de USN pode não ser detectada em tal circunstância.

![](media\virtualized-domain-controller-architecture\Dd363553.63565fe0-d970-4b4e-b5f3-9c76bc77e2d4(WS.10).gif)

## <a name="read-only-domain-controllers"></a>Controladores de domínio somente leitura

Os RODCs são controladores de domínio que hospedam cópias somente leitura das partições em um banco de dados do Active Directory. Os RODCs evitam a maioria dos problemas de reversão de USN porque eles não replicam nenhuma alteração nos outros controladores de domínio. Porém, se um RODC for replicado de um controlador de domínio gravável afetado pela reversão de USN, o RODC também será afetado.

Não é recomendável restaurar um RODC que usa um instantâneo. Restaure um RODC usando um aplicativo de backup compatível com o Active Directory. Além disso, como acontece com os controladores de domínio graváveis, deve-se ter cuidado para não permitir que um RODC fique offline além do tempo de vida da marca de exclusão Essa condição pode resultar em objetos persistentes no RODC.

Para obter mais informações sobre RODCs, consulte o [guia de implantação e planejamento do controlador de domínio somente leitura](../../deploy/rodc/read-only-domain-controller-updates.md).
