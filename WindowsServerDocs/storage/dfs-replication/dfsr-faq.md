---
Title: 'Replicação do DFS: Perguntas frequentes (FAQ)'
ms.date: 06/18/2014
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 3782667e54f5e6b52c07645704b95fc9e7409a27
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476066"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Replicação do DFS: Perguntas frequentes (FAQ)


Atualizado: 30 de abril de 2019

Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Estas perguntas Frequentes responde a perguntas sobre a replicação de sistema de arquivos distribuído (DFS) (também conhecida como DFS-R ou DFSR) para o Windows Server.

Para obter mais informações sobre Namespaces DFS, consulte [Namespaces DFS: Perguntas frequentes](https://technet.microsoft.com/en-us/library/ee404780).

Para obter informações sobre o que há de novo na replicação do DFS, consulte os tópicos a seguir:

  - [Namespaces do DFS e visão geral da replicação do DFS](http://technet.microsoft.com/en-us/library/jj127250) (no Windows Server 2012)  
      
  - [O que há de novo no sistema de arquivos distribuído](https://technet.microsoft.com/en-us/library/ee307957) tópico em [alterações na funcionalidade do Windows Server 2008 para o Windows Server 2008 R2](https://technet.microsoft.com/en-us/library/dd391932)  
      
  - [Sistema de arquivos distribuído](https://technet.microsoft.com/en-us/library/cc753479) tópico em [alterações na funcionalidade do Windows Server 2003 com SP1 para Windows Server 2008](https://technet.microsoft.com/en-us/library/cc753208)  
      

Para obter uma lista das alterações recentes deste tópico, consulte a seção [Histórico de alterações](#change-history).

      

## <a name="interoperability"></a>Interoperabilidade

### <a name="can-dfs-replication-communicate-with-frs"></a>Replicação do DFS pode se comunicar com o FRS?

Não. Replicação do DFS não se comunica com a replicação FRS (serviço). Replicação do DFS e FRS podem ser executados no mesmo servidor ao mesmo tempo, mas eles nunca devem ser configurados para replicar as mesmas pastas ou subpastas, porque isso pode causar perda de dados.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>Replicação do DFS pode substituir o FRS para replicação do SYSVOL

Sim, a replicação DFS pode substituir o FRS para replicação do SYSVOL em servidores que executam o Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. Servidores que executam o Windows Server 2003 R2 não suportam o uso da replicação DFS para replicar a pasta SYSVOL.

Para obter mais informações sobre a replicação de SYSVOL por meio da replicação do DFS, consulte o [guia de migração de replicação SYSVOL: Replicação FRS para DFS](https://technet.microsoft.com/en-us/library/dd640019).

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>Posso atualizar de FRS para replicação do DFS sem perder as definições de configuração?

Sim. Para migrar uma replicação de FRS para replicação do DFS, consulte os seguintes documentos:

  - Para migrar uma replicação de pastas diferentes da pasta SYSVOL, consulte [guia de operações do DFS: Migrando de FRS para replicação do DFS](http://go.microsoft.com/fwlink/?linkid=192776) e [FRS2DFSR – um FRS, DFSR utilitário de migração](http://go.microsoft.com/fwlink/?linkid=195437) (http://go.microsoft.com/fwlink/?LinkID=195437).  
      
  - Para migrar a replicação da pasta SYSVOL para replicação do DFS, consulte [guia de migração de replicação SYSVOL: Replicação FRS para DFS](https://technet.microsoft.com/en-us/library/dd640019).  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>Pode usar a replicação do DFS em um ambiente misto do Windows/UNIX?

Sim. Embora a replicação do DFS dá suporte apenas a replicar o conteúdo entre servidores que executam o Windows Server, os clientes UNIX podem acessar compartilhamentos de arquivos em servidores do Windows. Para fazer isso, instale os serviços de sistemas de arquivos de rede (NFS) no servidor de replicação do DFS.

Você também pode usar a funcionalidade de cliente SMB/CIFS incluída em muitos clientes UNIX para acessar diretamente os compartilhamentos de arquivos do Windows, embora essa funcionalidade é geralmente limitada ou requer modificações para o ambiente do Windows (como desabilitar a assinatura SMB usando Diretiva de grupo).

Replicação do DFS interopera com NFS em um servidor executando um sistema operacional Windows Server, mas você não pode replicar um ponto de montagem do NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>Pode usar o serviço de cópias de sombra de Volume com a replicação DFS?

Sim. A replicação DFS tem suporte em volumes de Volume Shadow Copy Service (VSS) e instantâneos anteriores podem ser restaurados com êxito com o cliente para versões anteriores.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>Pode usar o Backup do Windows (Ntbackup.exe) para backup de uma pasta replicada remotamente?

Não, usando o Backup do Windows (Ntbackup.exe) em um computador executando o Windows Server 2003 ou anterior para fazer backup do conteúdo de uma pasta replicada em um computador executando o Windows Server 2012, Windows Server 2008 R2, ou Windows Server 2008 não é suportado.

Para fazer backup de arquivos que são armazenados em uma pasta replicada, use o Backup do Windows Server ou o Microsoft® System Center Data Protection Manager. Para obter informações sobre a funcionalidade de Backup e recuperação no Windows Server 2008 R2 e Windows Server 2008, consulte [Backup e recuperação](https://technet.microsoft.com/en-us/library/Cc754097). Para obter mais informações, consulte [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) (http://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>Políticas de sistema de arquivos afetam a replicação DFS?

Sim. Não configure diretivas do sistema de arquivos em pastas replicadas. A política do sistema de arquivos reaplica as permissões NTFS em cada intervalo de atualização de diretiva de grupo. Isso pode resultar em violações de compartilhamento, porque um arquivo aberto não é replicado até que o arquivo seja fechado.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>A replicação DFS replique caixas de correio hospedadas no Microsoft Exchange Server?

Não. Replicação do DFS não pode ser usada para replicar as caixas de correio hospedadas no Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>Replicação do DFS dá suporte a telas de arquivo criadas pelo Gerenciador de recursos de servidor de arquivos?

Sim. No entanto, as configurações de triagem de arquivo de arquivo servidor FSRM Gerenciador de recursos () deve corresponder em ambas as extremidades da replicação. Além disso, a replicação DFS tem seu próprio mecanismo de filtro para arquivos e pastas que você pode usar para excluir determinados arquivos e tipos de arquivo da replicação.

A seguir estão as práticas recomendadas para implementar as cotas ou telas de arquivo:

  - A pasta DfsrPrivate oculta não deve ser sujeitos a cotas ou telas de arquivo.  
      
  - Arquivos filtrados não devem existir em qualquer pasta replicada antes de triagem está habilitada.  
      
  - Nenhuma pasta pode exceder a cota antes que a cota é habilitada.  
      
  - Você deve usar cotas de disco rígidas com cuidado. É possível para membros individuais de um grupo de replicação para permanecer dentro de uma cota antes da replicação, mas excedê-la quando arquivos são replicados. Por exemplo, se um usuário copia um arquivo de 10 megabytes (MB) no servidor R (que é, em seguida, o limite de disco rígido) e outro usuário copia um arquivo de 5 MB no servidor B, quando a próxima replicação ocorre, ambos os servidores excederá a cota por 5 megabytes. Isso pode causar a replicação do DFS Repetir continuamente a replicação de arquivos, causando falhas no vetor da versão e possíveis problemas de desempenho.  
      

### <a name="is-dfs-replication-cluster-aware"></a>Cluster de replicação do DFS está ciente?

Sim, a replicação DFS no Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2 inclui a capacidade de adicionar um cluster de failover como um membro de um grupo de replicação. Para obter mais informações, consulte [adicionar um Cluster de Failover para um grupo de replicação](http://go.microsoft.com/fwlink/?linkid=155085) (http://go.microsoft.com/fwlink/?LinkId=155085). O serviço de replicação do DFS em versões do Windows antes do Windows Server 2008 R2 não foi projetado para coordenar com um cluster de failover e o serviço não realizará failover para outro nó.


> [!NOTE]
> Replicação do DFS não dá suporte a replicação de arquivos em Volumes Compartilhados do Cluster. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>A replicação DFS é compatível com eliminação de duplicação de dados?

Sim, a replicação DFS replique pastas em volumes que usam a eliminação de duplicação no Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>A replicação DFS é compatível com o RIS e WDS?

Sim. Replicação do DFS replica os volumes nos quais o armazenamento de instância única (SIS) está habilitado. SIS é usado por serviços de instalação remota (RIS), o Windows Deployment Services (WDS) e o Windows Storage Server.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>É possível usar a replicação do DFS com arquivos Offline?

Você pode usar com segurança arquivos Offline e a replicação do DFS juntos em cenários quando há apenas um usuário cada vez que grava os arquivos. Isso é útil para os usuários que viajam entre duas filiais e desejam ser capaz de acessar seus arquivos em qualquer branch ou enquanto estiver offline. Arquivos offline armazena em cache os arquivos localmente para uso offline e a replicação do DFS replica os dados entre cada filial.

Não use a replicação do DFS com arquivos Offline em um ambiente multiusuário porque a replicação do DFS não fornece qualquer mecanismo de bloqueio distribuído ou a capacidade de fazer check-out de arquivos. Se dois usuários alterarem o mesmo arquivo ao mesmo tempo em servidores diferentes, a replicação DFS move o arquivo mais antigo para o DfsrPrivate\\ConflictandDeleted pasta (localizada no caminho local da pasta replicada) durante a próxima replicação.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>Quais aplicativos antivírus são compatíveis com a replicação DFS?

Aplicativos antivírus podem causar replicação excessiva se suas atividades de verificação alteram os arquivos em uma pasta replicada. Para obter mais informações, [teste de interoperabilidade de aplicativos de antivírus com a replicação do DFS](http://go.microsoft.com/fwlink/?linkid=73990) (http://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>Quais são os benefícios de usar a replicação do DFS em vez do Windows SharePoint Services?

Windows® SharePoint® Services fornece coerência forte na forma de funcionalidade de check-out de arquivos que a replicação do DFS não. Se você estiver preocupado com o mesmo arquivo de edição de várias pessoas, é recomendável usar o Windows SharePoint Services. Windows SharePoint Services 2.0 com Service Pack 2 está disponível como parte do Windows Server 2003 R2. Windows SharePoint Services pode ser baixado no site da Microsoft; ele não está incluído nas versões mais recentes do Windows Server. No entanto, se você estiver replicando dados em vários sites e os usuários não serão editar os mesmos arquivos ao mesmo tempo, a replicação DFS fornece maior largura de banda e gerenciamento mais simples.

## <a name="limitations-and-requirements"></a>Requisitos e limitações

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>Replicação do DFS pode replicar entre filiais sem uma conexão de VPN?

Sim, supondo que há um link redes de longa distância (WANs) privada (não a Internet) conectam as filiais. No entanto, você deve abrir as portas adequadas nos firewalls externos. A replicação DFS usa o mapeador de ponto de extremidade RPC (porta 135) e uma porta efêmera atribuída aleatoriamente acima de 1024. Você pode usar o **Dfsrdiag** ferramenta de linha de comando para especificar uma porta estática em vez da porta efêmera. Para obter mais informações sobre como especificar o mapeador de ponto de extremidade RPC, consulte [artigo 154596](http://go.microsoft.com/fwlink/?linkid=73991) na Base de dados de Conhecimento Microsoft (http://go.microsoft.com/fwlink/?LinkId=73991).

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>A replicação DFS replique arquivos criptografados com o sistema de arquivos com criptografia?

Não. Replicação do DFS não serão replicadas arquivos ou pastas que são criptografadas usando o Encrypting File System (EFS). Se um usuário criptografa um arquivo que tenha sido replicado anteriormente, a replicação do DFS exclui o arquivo de todos os outros membros do grupo de replicação. Isso garante que a cópia disponível somente do arquivo é a versão criptografada no servidor.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>A replicação DFS replique. pst do Outlook ou arquivos de banco de dados do Access do Microsoft Office?

Replicação do DFS pode replicar com segurança arquivos de pasta particular do Microsoft Outlook (. pst) e arquivos do Microsoft Access somente se eles são armazenados para fins de arquivamento e não são acessados pela rede por meio de um cliente como o Outlook ou de acesso (para abrir o arquivo. pst ou acesso arquivos, primeiro copie os arquivos em um dispositivo de armazenamento local). As razões para isso são as seguintes:

  - Abrir arquivos. pst em conexões de rede pode levar à corrupção de dados nos arquivos. pst. Para obter mais informações sobre por que os arquivos. pst não podem ser acessados com segurança de em uma rede, consulte [297019 do artigo](http://go.microsoft.com/fwlink/?linkid=125363) na Base de dados de Conhecimento Microsoft (http://go.microsoft.com/fwlink/?LinkId=125363).  
      
  - arquivos. pst e acesso tendem a permanecer aberta por longos períodos de tempo em que está sendo acessado por um cliente como o Outlook ou o Office Access. Isso impede que a replicação do DFS replicar esses arquivos até que eles são fechados.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>Pode usar a replicação do DFS em um grupo de trabalho?

Não. Replicação do DFS se baseia em serviços de domínio do Active Directory® para a configuração. Ele só funcionará em um domínio.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>Mais de uma pasta pode ser replicada em um único servidor?

Sim. A replicação DFS replique várias pastas entre servidores. Certifique-se de que cada uma das pastas replicadas tem uma única raiz do caminho e o que eles não se sobrepõem. Por exemplo, d:\\vendas e d:\\estatísticas podem ser os caminhos de raiz para duas pastas replicadas, mas d:\\vendas e d:\\vendas\\relatórios não podem ser os caminhos de raiz para duas pastas replicadas.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>Replicação do DFS requer Namespaces DFS?

Não. Namespaces do DFS e replicação DFS podem ser usados separadamente ou em conjunto. Além disso, a replicação DFS pode ser usada para replicar os namespaces DFS autônomo, que não era possível com o FRS.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>Replicação do DFS requer sincronização de horário entre servidores?

Não. Replicação do DFS não requer sincronização de horário entre servidores explicitamente. No entanto, a replicação do DFS requer que os relógios dos servidores correspondam. Os relógios dos servidores devem ser definidos dentro de cinco minutos entre si (por padrão) para a autenticação Kerberos funcione corretamente. Por exemplo, a replicação DFS usa carimbos de data / hora para determinar qual arquivo tem precedência em caso de conflito. Tempos exatos também são importantes para coleta de lixo, agendas e outros recursos.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>A replicação DFS oferece suporte a replicação de um volume inteiro?

Sim. No entanto, você deve primeiro instalar Windows Server 2003 Service Pack 2 ou o hotfix. Para obter mais informações, consulte [920335 do artigo](http://go.microsoft.com/fwlink/?linkid=76776) na Base de dados de Conhecimento Microsoft (http://go.microsoft.com/fwlink/?LinkId=76776). Além disso, a replicação de um volume inteiro pode causar os seguintes problemas:

  - Se o volume contiver um arquivo de paginação do Windows, a replicação falhará e registrará o evento DFSR 4312 no log de eventos do sistema.  
      
  - Replicação do DFS define os atributos de sistema e ocultos na pasta replicada nos servidores de destino. Isso ocorre porque o Windows aplicam os atributos de sistema e ocultos para a pasta raiz de volume por padrão. Se o caminho local da pasta replicada nos servidores de destino também é uma raiz de volume, não há mais alterações são feitas para os atributos de pasta.  
      
  - Ao replicar um volume que contém a pasta de sistema do Windows, a replicação do DFS reconhece a pasta % WINDIR % e não é replicado. No entanto, a replicação do DFS replicar pastas usadas por aplicativos não-Microsoft, que podem causar falhas de aplicativos nos servidores de destino se os aplicativos têm problemas de interoperabilidade com a replicação do DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>A replicação DFS oferece suporte a RPC sobre HTTP?

Não.

### <a name="does-dfs-replication-work-across-wireless-networks"></a>Replicação do DFS funciona em redes sem fio?

Sim. A replicação DFS é independente do tipo de conexão.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>Replicação do DFS funciona em volumes FAT ou ReFS?

Não. A replicação DFS oferece suporte a volumes formatados com o sistema de arquivos NTFS xibições Não há suporte para o sistema de arquivos resiliente (ReFS) e o sistema de arquivos FAT. Replicação do DFS requer NTFS porque ele usa o diário de alterações do NTFS e sistema de arquivos de outros recursos do NTFS.

### <a name="does-dfs-replication-work-with-sparse-files"></a>Replicação do DFS funciona com arquivos esparsos?

Sim. Você pode replicar arquivos esparsos. O **Sparse** atributo é preservado no membro de recebimento.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>É necessário fazer logon como administrador para replicar arquivos?

Não. A replicação DFS é um serviço executado sob a conta sistema local, portanto, você não precisará fazer logon como administrador para replicar. No entanto, você deve ser um administrador de domínio ou um administrador local dos servidores de arquivos afetados para fazer alterações na configuração de replicação do DFS.

Para obter mais informações, consulte "requisitos de segurança de replicação do DFS e a delegação" na [delegar a capacidade de gerenciar a replicação do DFS](http://go.microsoft.com/fwlink/?linkid=182294) (http://go.microsoft.com/fwlink/?LinkId=182294).

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>Como atualizar ou substituir um membro de replicação DFS?

Para atualizar ou substituir um membro de replicação do DFS, consulte este blog postar no Ask o blog da equipe de serviços de diretório: [Substituindo o sistema operacional ou Hardware de membro DFSR](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>A replicação DFS é adequado para a replicação de perfis móveis?

Sim. Determinados cenários têm suporte durante a replicação de perfis de usuário móvel. Para obter informações sobre os cenários com suporte, consulte [suporte instrução ao redor replicados perfil de dados da Microsoft de usuário](http://go.microsoft.com/fwlink/?linkid=201282) (http://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>Há um limite de caracteres do arquivo ou o limite para a profundidade de pasta?

Windows e a replicação do DFS dá suporte a caminhos de pasta com até 32 mil caracteres. Replicação do DFS não está limitada a caminhos de pasta de 260 caracteres.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>Membros de um grupo de replicação devem residir no mesmo domínio?

Não. Grupos de replicação podem abranger entre domínios em uma única floresta, mas não em florestas diferentes.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>Quais são os limites com suporte de replicação do DFS?

A lista a seguir fornece um conjunto de diretrizes de escalabilidade que foram testados pela Microsoft no Windows Server 2012 R2:

  - Tamanho de todos os arquivos replicados em um servidor: 100 terabytes.  
      
  - Número de arquivos duplicados em um volume: 70 milhões.  
      
  - Tamanho máximo do arquivo: 250 gigabytes.  
      


> [!IMPORTANT]
> Durante a criação de grupos de replicação com um grande número ou o tamanho dos arquivos, é recomendável exportando um clone do banco de dados e usando técnicas de propagação previamente para minimizar a duração da replicação inicial. Para obter mais informações, consulte <A href="http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx">sincronização inicial de replicação de DFS no Windows Server 2012 R2: Ataque dos Clones</A>. 
<br>


A lista a seguir fornece um conjunto de diretrizes de escalabilidade que foram testados pela Microsoft no Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008:

  - Tamanho de todos os arquivos replicados em um servidor: 10 terabytes.  
      
  - Número de arquivos duplicados em um volume: milhões de 11.  
      
  - Tamanho máximo do arquivo: 64 gigabytes.  
      


> [!NOTE]
> Não há um limite para o número de grupos de replicação, as pastas replicadas, as conexões ou membros do grupo de replicação. 
<br>


Para obter uma lista de diretrizes de escalabilidade que foram testados pela Microsoft para Windows Server 2003 R2, consulte [diretrizes de escalabilidade de replicação do DFS](http://go.microsoft.com/fwlink/?linkid=75043) (http://go.microsoft.com/fwlink/?LinkId=75043).

### <a name="when-should-i-not-use-dfs-replication"></a>Quando devo usar a replicação do DFS não?

Não use a replicação do DFS em um ambiente onde vários usuários atualizarem ou modificar os mesmos arquivos ao mesmo tempo em servidores diferentes. Isso pode fazer com que a replicação do DFS mover cópias conflitantes dos arquivos para o DfsrPrivate oculto\\ConflictandDeleted pasta.

Quando vários usuários precisam modificar os mesmos arquivos ao mesmo tempo em servidores diferentes, use o recurso de check-out de arquivos do Windows SharePoint Services para garantir que apenas um usuário está trabalhando em um arquivo. Windows SharePoint Services 2.0 com Service Pack 2 está disponível como parte do Windows Server 2003 R2. Windows SharePoint Services pode ser baixado no site da Microsoft; ele não está incluído nas versões mais recentes do Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>Por que é uma atualização de esquema necessária para a replicação DFS?

A replicação do DFS usa novos objetos no contexto de nomeação de domínio do Active Directory Domain Services para armazenar informações de configuração. Esses objetos são criados quando você atualizar o esquema do Active Directory Domain Services. Para obter mais informações, consulte [revisar os requisitos para replicação DFS](http://go.microsoft.com/fwlink/?linkid=182264) (http://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Ferramentas de gerenciamento e monitoramento

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>É possível automatizar o relatório de integridade para receber avisos?

Sim. Há três maneiras de automatizar os relatórios de integridade:

  - Use o módulo de DFSR Windows PowerShell incluído no Windows Server 2012 R2 ou DfsrAdmin.exe em conjunto com tarefas agendadas regularmente, gerar relatórios de integridade. Para obter mais informações, consulte [automatizando relatórios de integridade da replicação do DFS](http://go.microsoft.com/fwlink/?linkid=74010) (http://go.microsoft.com/fwlink/?LinkId=74010).  
      
  - Use o pacote de gerenciamento de replicação do DFS para o System Center Operations Manager para criar alertas com base nas condições especificadas.  
      
  - Use o provedor de WMI de replicação do DFS para alertas de script.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>Pode usar o Microsoft System Center Operations Manager para monitorar a replicação DFS?

Sim. Para obter mais informações, consulte o [pacote de gerenciamento de replicação do DFS para o System Center Operations Manager 2007](http://go.microsoft.com/fwlink/?linkid=182265) no Microsoft Download Center (http://go.microsoft.com/fwlink/?LinkId=182265).

### <a name="does-dfs-replication-support-remote-management"></a>Replicação do DFS permitem o gerenciamento remoto?

Sim. Replicação do DFS dá suporte ao gerenciamento remoto usando o console de gerenciamento de DFS e a **adicionar grupo de replicação** comando. Por exemplo, no servidor A, você pode se conectar a um grupo de replicação definido na floresta com servidores A e B como membros.

Gerenciamento de DFS está incluído no Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 e Windows Server 2003 R2. Para gerenciar a replicação do DFS de outras versões do Windows, use a área de trabalho remota ou o [servidor remoto administração ferramentas para Windows 7](https://technet.microsoft.com/en-us/library/Ee449475).


> [!IMPORTANT]
> Para exibir ou gerenciar grupos de replicação que contêm as pastas replicadas somente leitura ou membros que são clusters de failover, você deve usar a versão do Gerenciamento DFS, que está incluído no Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, o <a href="http://go.microsoft.com/fwlink/p/?linkid=238560">Ferramentas de administração de servidor remoto para Windows 8</a>, ou o <a href="https://technet.microsoft.com/en-us/library/ee449475">ferramentas de administração de servidor remoto para Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>O Ultrasound e Sonar funcionam com a replicação DFS?

Não. A replicação DFS tem seu próprio conjunto de ferramentas de monitoramento e diagnóstico. O Ultrasound e Sonar só são capazes de monitoramento de FRS.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>Como os arquivos podem ser recuperados das pastas ConflictAndDeleted ou PreExisting?

Para recuperar arquivos perdidos, restaure os arquivos da pasta do sistema de arquivo ou pasta compartilhada usando o histórico de arquivos, o **restaurar versões anteriores** comando no Explorador de arquivos ou restaurar os arquivos de backup. Para recuperar arquivos diretamente na pasta ConflictAndDeleted ou PreExisting, use o `Get-DfsrPreservedFiles` e `Restore-DfsrPreservedFiles` cmdlets do Windows PowerShell (incluído com o módulo DFSR no Windows Server 2012 R2), ou o [RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr) script de exemplo na Galeria de código do MSDN. Esse script é destinado apenas para recuperação de desastres e é fornecido como-está, sem garantia.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>Há uma maneira de saber o estado de replicação?

Sim. Há várias maneiras de monitorar a replicação:

  - A replicação DFS tem um pacote de gerenciamento do System Center Operations Manager que fornece monitoramento proativo.  
      
  - O gerenciamento de DFS tem um relatório de diagnóstico de caixa de entrada para a lista de pendências de replicação, eficiência de replicação e o número de arquivos e pastas em um grupo de replicação.  
      
  - O módulo de DFSR Windows PowerShell no Windows Server 2012 R2 contém cmdlets para iniciar os testes de propagação e escrever relatórios de integridade e de propagação. Para obter mais informações, consulte [distribuídos Cmdlets de replicação do arquivo de sistema no Windows PowerShell](http://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag.exe é uma ferramenta de linha de comando que pode gerar uma contagem de lista de pendências ou gatilho de um teste de propagação. Ambos mostram o estado de replicação. Propagação mostra se os arquivos estão sendo replicados para todos os nós. Lista de pendências mostra quantos arquivos ainda precisará replicar antes de dois computadores estejam em sincronia. A contagem de lista de pendências é o número de atualizações que um membro do grupo de replicação não processada. Em computadores que executam o Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2, Dfsrdiag.exe também pode exibir as atualizações que a replicação do DFS está replicando atualmente.  
      
  - Scripts podem usar o WMI para coletar informações de lista de pendências — manualmente ou por meio do MOM.  
      

## <a name="performance"></a>Desempenho

### <a name="does-dfs-replication-support-dial-up-connections"></a>A replicação DFS oferece suporte a conexões dial-up?

Embora a replicação do DFS funcione em velocidades de conexão discada, pode obter acumulada se houver grandes números de alterações replicadas. Se a pequenas alterações são feitas em arquivos existentes, a replicação do DFS com compactação diferencial remota (RDC) fornece um desempenho muito maior do que copiar o arquivo diretamente.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>Replicação do DFS realiza a detecção de largura de banda?

Não. Replicação do DFS não realiza a detecção de largura de banda. Você pode configurar a replicação do DFS para usar uma quantidade limitada de largura de banda em uma base por conexão (limitação de largura de banda). No entanto, a replicação do DFS não reduzir ainda mais a utilização de largura de banda se o adaptador de rede se torna saturado, e a replicação do DFS pode esgotar o link por curtos períodos. Largura de banda com a replicação do DFS não está completamente precisa porque a replicação do DFS limita a largura de banda pela limitação de chamadas RPC. Como resultado, vários buffers em níveis inferiores da pilha de rede (incluindo RPC) podem interferir, fazendo com que os picos de tráfego de rede.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>Replicação do DFS Limitar largura de banda de acordo com a agenda, por servidor ou por conexão?

Se você configurar a limitação de largura de banda ao especificar o agendamento, todas as conexões para esse grupo de replicação usará essa configuração de limitação de largura de banda. Limitação de largura de banda pode ser definido também como uma configuração de nível de conexão usando o Gerenciamento DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>Replicação do DFS usar os serviços de domínio do Active Directory para calcular os custos de conexão e de links de site?

Não. A replicação DFS usa a topologia definida pelo administrador, que é independente do custo de site do Active Directory Domain Services.

### <a name="how-can-i-improve-replication-performance"></a>Como posso melhorar o desempenho da replicação?

Para saber mais sobre os diferentes métodos de ajuste de desempenho de replicação, consulte [ajustando o desempenho de replicação no DFSR](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) sobre o [pergunte o blog da equipe de serviços de diretório](http://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>Como a replicação do DFS evitar a saturação de uma conexão?

Na replicação do DFS, você define a largura de banda máxima que você deseja usar em uma conexão e o serviço mantém o nível de uso da rede. Isso é diferente do plano de fundo serviço BITS (transferência inteligente) e a replicação do DFS não saturar a conexão se você defini-lo adequadamente.

No entanto, a limitação de largura de banda não é 100% precisas e a replicação do DFS pode esgotar o link por curtos períodos de tempo. Isso ocorre porque a replicação do DFS limita a largura de banda pela limitação de chamadas RPC. Como esse processo depende de vários buffers em níveis inferiores da pilha de rede, incluindo RPC, o tráfego de replicação tende a picos que às vezes podem saturar os links de rede de viagem.

Replicação do DFS no Windows Server 2008 inclui vários aprimoramentos de desempenho, conforme discutido em [sistema de arquivos distribuído](https://technet.microsoft.com/en-us/library/Cc753479), um tópico em [alterações na funcionalidade do Windows Server 2003 com SP1 para Windows Server 2008](https://technet.microsoft.com/en-us/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>Como o desempenho da replicação do DFS compara com o FRS?

A replicação DFS é muito mais rápida que o FRS, particularmente quando pequenas alterações são feitas em arquivos grandes e RDC está habilitado. Por exemplo, com o RDC, uma pequena alteração para uma apresentação MB PowerPoint® 2 pode resultar em apenas 60 KB (Kilobytes que estão sendo enviados pela rede) — um 97% de economia em bytes transferidos.

RDC não é usado em arquivos menores do que 64 KB e pode não ser benéfico em LANs de alta velocidade em que a largura de banda de rede não é sustentada. RDC pode ser desabilitado em uma base por conexão usando o Gerenciamento DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>A frequência com que a replicação DFS replicar dados?

Replicam os dados de acordo com o agendamento que você definir. Por exemplo, você pode definir a agenda para intervalos de 15 minutos, sete dias por semana. Durante esses intervalos, a replicação é habilitada. Replicação começa logo depois que for detectada uma alteração de arquivo (geralmente dentro de segundos).

A agenda de grupo de replicação pode ser definida para Hora Universal coordenada (UTC) enquanto o agendamento de conexão é definido como a hora local do membro de recebimento. Leve isso em conta ao grupo de replicação se estender por vários fusos horários. Hora local significa que a hora do membro que está hospedando a conexão de entrada. O agendamento exibido da conexão de entrada e a conexão de saída correspondente refletem as diferenças de fuso horário quando a agenda é definida como hora local.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>A quantidade de recursos do sistema do meu servidor consumirá a replicação DFS?

O disco, memória e recursos de CPU usados pela replicação do DFS dependem de vários fatores, incluindo o número e tamanho dos arquivos, taxa de alteração, o número de membros do grupo de replicação e o número de pastas replicadas. Além disso, alguns recursos são mais difíceis de estimar. Por exemplo, a tecnologia de mecanismo de armazenamento extensível (ESE) usada para o banco de dados de replicação DFS pode consumir uma grande porcentagem de memória disponível, o que ele for lançado sob demanda. Aplicativos que não sejam a replicação DFS podem ser hospedados no mesmo servidor, dependendo da configuração do servidor. No entanto, ao hospedar vários aplicativos ou funções de servidor em um único servidor, é importante que você teste essa configuração antes de implementá-la em um ambiente de produção.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>O que acontece se um link WAN falhar durante a replicação?

Se a conexão falhar, a replicação do DFS continuará tentando replicar enquanto a agenda está aberta. Também haverá erros de conectividade observados no log de eventos de replicação DFS que podem ser coletado usando o MOM (proativamente por meio de alertas) e o relatório de integridade de replicação do DFS (de forma reativa, como quando um administrador executa a ele).

## <a name="remote-differential-compression-details"></a>Detalhes de compactação diferencial remota

### <a name="are-changes-compressed-before-being-replicated"></a>As alterações são compactadas antes de serem duplicados?

Sim. As partes alteradas de arquivos são compactadas antes de serem enviados para todos os tipos, exceto o seguinte (que já é compactado) de arquivo:. wma,. wmv,. zip,. jpg,. mpg,. MPEG,. m1v,. mp2, MP3,. MPA,. cab,. wav,. snd,. au,. ASF,. WM,. avi, z, gz, tgz e FRx. Configurações de compactação para esses tipos de arquivo não são configuráveis no Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>Um administrador desligar RDC e alterar o limite?

Sim. Você pode desativar o RDC por meio da página de propriedades de uma determinada conexão. Desabilitar a RDC pode reduzir a CPU latência de replicação e de utilização em links de rede local (LAN) rápido que não há restrições de largura de banda ou para grupos de replicação consistem principalmente em arquivos menores do que 64 KB. Se você optar por desabilitar a RDC em uma conexão, teste a eficiência de replicação antes e após a alteração para verificar que você ter melhor desempenho de replicação.

Você pode alterar o limite de tamanho RDC usando o **Dfsradmin definido de Conexão** de comando, o provedor de WMI de replicação do DFS, ou editando manualmente o XML de configuração de arquivo.

### <a name="does-rdc-work-on-all-file-types"></a>RDC funciona em todos os tipos de arquivo?

Sim. RDC calcula as diferenças no nível de bloco, independentemente do tipo de dados de arquivo. No entanto, o RDC funciona com mais eficiência em determinados tipos de arquivo, como documentos do Word, arquivos PST e imagens de VHD.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>Como funciona a RDC em um arquivo compactado?

A replicação DFS usa RDC, que calcula os blocos no arquivo que foram alterados e envia somente os blocos pela rede. Replicação do DFS não precisa saber nada sobre o conteúdo do arquivo — apenas quais blocos foram alterados.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>É a arquivos e RDC habilitado ao atualizar para o Windows Server Enterprise Edition ou Datacenter Edition?

As edições do Windows Server Standard não dão suporte a RDC entre arquivos. No entanto, ele é habilitado automaticamente quando você atualizar para uma edição que dá suporte a RDC entre arquivos, ou se um membro da conexão de replicação está executando uma edição com suporte. Para obter uma lista das edições que suporte a RDC entre arquivos, veja quais edições do suporte do sistema operacional Windows a RDC entre arquivos?

### <a name="is-rdc-true-block-level-replication"></a>É a replicação em nível de bloco RDC true?

Não. RDC é um protocolo de finalidade geral para a compactação de transferência de arquivo. A replicação DFS usa RDC em blocos no nível de arquivo, não no nível de bloco do disco. RDC divide um arquivo em blocos. Para cada bloco em um arquivo, ele calcula uma assinatura, que é um pequeno número de bytes que pode representar o bloco maior. O conjunto de assinaturas é transferido do servidor para cliente. O cliente compara as assinaturas de servidor para seu próprio. O cliente e solicitações para o servidor enviar somente os dados de assinaturas que ainda não estão no cliente.

### <a name="what-happens-if-i-rename-a-file"></a>O que acontece se eu renomear um arquivo?

Replicação do DFS renomeia o arquivo em todos os outros membros do grupo de replicação durante a próxima replicação. Arquivos são controladas usando uma ID exclusiva, então, renomear um arquivo e movendo que o arquivo dentro da réplica não tem nenhum efeito na capacidade de replicação do DFS para replicar um arquivo.

### <a name="what-is-cross-file-rdc"></a>O que é a RDC entre arquivos?

Entre arquivos RDC permite que a replicação DFS usa RDC, mesmo quando não existe um arquivo com o mesmo nome no final do cliente. Entre arquivos RDC usa uma heurística para determinar os arquivos que são semelhantes para o arquivo que precisa ser replicado e usa blocos dos arquivos semelhantes que são idênticos para o arquivo de replicação para minimizar a quantidade de dados transferidos por WAN. Entre arquivos RDC pode usar blocos de até cinco arquivos semelhantes nesse processo.

Para usar a RDC entre arquivos, um membro da conexão de replicação deve estar executando uma edição do Windows que dá suporte a RDC entre arquivos. Para obter uma lista das edições que suporte a RDC entre arquivos, veja quais edições do suporte do sistema operacional Windows a RDC entre arquivos?

### <a name="what-is-rdc"></a>O que é o RDC?

Compactação diferencial remota (RDC) é um protocolo de cliente-servidor que pode ser usado para atualizar com eficiência os arquivos em uma rede de largura de banda limitada. RDC detecta inserções, remoções e reorganizações de dados em arquivos, permitindo que a replicação DFS replique somente as alterações quando os arquivos são atualizados. Por padrão, o RDC é usada apenas para arquivos que são de 64 KB ou maior. RDC pode usar uma versão mais antiga de um arquivo com o mesmo nome na pasta replicada ou no DfsrPrivate\\pasta ConflictandDeleted (localizada no caminho local da pasta replicada).

### <a name="when-is-rdc-used-for-replication"></a>Quando o RDC é usada para replicação?

A RDC é usada quando o arquivo excede um limite de tamanho mínimo. Esse limite de tamanho é 64 KB por padrão. Depois que um arquivo que excede esse limite foi replicado, versões atualizadas do arquivo de sempre usam RDC, a menos que uma grande parte do arquivo é alterada ou RDC está desabilitado.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>Quais edições do suporte do sistema operacional Windows a RDC entre arquivos?

Para usar a RDC entre arquivos, um membro da conexão de replicação deve estar executando uma edição do sistema operacional Windows que dá suporte a RDC entre arquivos. A tabela a seguir mostra quais edições do sistema operacional Windows oferecem suporte a RDC entre arquivos.

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>A arquivos e a disponibilidade RDC nas edições do sistema operacional Windows

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Versão do sistema operacional</th>
<th>Standard Edition</th>
<th>Enterprise Edition</th>
<th>Datacenter Edition</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>Sim*</p></td>
<td><p>Não disponível</p></td>
<td><p>Sim*</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012</p></td>
<td><p>Sim</p></td>
<td><p>Não disponível</p></td>
<td><p>Sim</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2008 R2</p></td>
<td><p>Não</p></td>
<td><p>Sim</p></td>
<td><p>Sim</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2008</p></td>
<td><p>Não</p></td>
<td><p>Sim</p></td>
<td><p>Não</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2003 R2</p></td>
<td><p>Não</p></td>
<td><p>Sim</p></td>
<td><p>Não</p></td>
</tr>
</tbody>
</table>

\* Você também pode desabilitar a RDC no Windows Server 2012 R2 entre arquivos.

## <a name="replication-details"></a>Detalhes de replicação

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>Posso alterar o caminho para uma pasta replicada depois que ela é criada?

Não. Se você precisar alterar o caminho de uma pasta replicada, você deve excluí-lo no Gerenciamento DFS e adicioná-lo novamente como uma nova pasta replicada. A replicação DFS usa compactação diferencial remota (RDC) para executar uma sincronização que determina se os dados são os mesmos no envio e recebimento de membros. Ele não replica todos os dados na pasta novamente.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>Pode configurar quais atributos de arquivo são replicados?

Não, você não pode configurar quais atributos de arquivo que a replicação do DFS replica.

Para obter uma lista de valores de atributo e suas descrições, consulte [atributos de arquivo](http://go.microsoft.com/fwlink/?linkid=182268) no MSDN (http://go.microsoft.com/fwlink/?LinkId=182268).

Os seguintes valores de atributo são definidos usando o `SetFileAttributes dwFileAttributes` função e eles são replicados pela replicação DFS. As alterações a esses valores de atributo disparam a replicação dos atributos. O conteúdo do arquivo não é replicado, a menos que o conteúdo também é alterado. Para obter mais informações, consulte [função SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) na biblioteca MSDN (http://go.microsoft.com/fwlink/?LinkId=182269).

  - ARQUIVO\_ATRIBUTO\_HIDDEN  
      
  - FILE\_ATTRIBUTE\_READONLY  
      
  - ARQUIVO\_ATRIBUTO\_SISTEMA  
      
  - ARQUIVO\_ATRIBUTO\_NÃO\_CONTEÚDO\_INDEXADO  
      
  - ARQUIVO\_ATRIBUTO\_OFFLINE  
      

Os seguintes valores de atributo são replicados por replicação do DFS, mas eles não disparar a replicação.

  - ARQUIVO\_ATRIBUTO\_ARQUIVO MORTO  
      
  - ARQUIVO\_ATRIBUTO\_NORMAL  
      

Os seguintes valores de atributo de arquivo também disparam a replicação, embora eles não podem ser definidos usando o `SetFileAttributes` função (use o `GetFileAttributes` função para exibir os valores de atributo).

  - ARQUIVO\_ATRIBUTO\_DE NOVA ANÁLISE\_PONTO  
      

> [!NOTE]
> Replicação do DFS não replica valores de atributo de ponto de nova análise, a menos que a marca de nova análise é IO_REPARSE_TAG_SYMLINK. Arquivos com as marcas de nova análise IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS ou IO_REPARSE_TAG_HSM são replicados como arquivos normais. No entanto, os buffers de dados de nova análise e a marca de nova análise não são replicados para outros servidores porque o ponto de nova análise funciona apenas no sistema local. 
<br>

  - ARQUIVO\_ATRIBUTO\_COMPACTADA  
      
  - ARQUIVO\_ATRIBUTO\_CRIPTOGRAFADO  
      

> [!NOTE]
> Replicação do DFS replica arquivos que são criptografados usando o Encrypting File System (EFS). A replicação DFS replique arquivos que são criptografados por meio de software não Microsoft, mas somente se ele não define o valor do atributo FILE_ATTRIBUTE_ENCRYPTED no arquivo. 
<br>

  - ARQUIVO\_ATRIBUTO\_SPARSE\_ARQUIVO  
      
  - ARQUIVO\_ATRIBUTO\_DIRETÓRIO  
      

Replicação do DFS não replicar o arquivo\_atributo\_valor temporário.

### <a name="can-i-control-which-member-is-replicated"></a>É possível controlar qual membro será replicado?

Sim. Quando você cria um grupo de replicação, você pode escolher uma topologia. Ou você pode selecionar **nenhuma topologia** e configurar manualmente as conexões após o grupo de replicação ter sido criado.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>Eu posso propagar um membro do grupo de replicação com os dados antes da replicação inicial?

Sim. Replicação do DFS dá suporte a cópia de arquivos para um membro do grupo de replicação antes da replicação inicial. Essa "pré-configuração" pode reduzir drasticamente a quantidade de dados replicados durante a replicação inicial.

A replicação inicial não precisa replicar o conteúdo quando os arquivos diferem somente por atributos real ou carimbos de data / hora. Um real é um atributo que pode ser definido pela função Win32 `SetFileAttributes`. Para obter mais informações, consulte [função SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) na biblioteca MSDN (http://go.microsoft.com/fwlink/?LinkId=182269). Se dois arquivos diferem por outros atributos, como compactação, o conteúdo do arquivo é replicado.

Para pré-configurar um membro do grupo de replicação, copie os arquivos para a pasta apropriada nos servidores de destino, crie o grupo de replicação e, em seguida, escolher um membro primário. Escolha o membro que tem os arquivos mais atualizados que você deseja replicar, pois o conteúdo do membro primário é considerado "autoritativo". Isso significa que durante a replicação inicial, os arquivos do membro primário sempre substituirá outras versões dos arquivos em outros membros do grupo de replicação.

Para obter informações sobre a pré-propagação e clonagem de banco de dados DFSR, consulte [sincronização inicial de replicação de DFS no Windows Server 2012 R2: Ataque dos Clones](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Para obter mais informações sobre a replicação inicial, consulte [criar um grupo de replicação](https://technet.microsoft.com/en-us/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>Replicação do DFS superar problemas de serviço de replicação de arquivo comuns?

Sim. Replicação do DFS supera três problemas comuns de FRS:

  - Quebras de diário: Replicação do DFS recupera de ocultação em tempo real. Cada arquivo ou pasta existente será marcada como journalWrap e verificada no sistema de arquivos antes da replicação é habilitada novamente. Durante a recuperação, esse volume não está disponível para replicação em qualquer direção.  
      
  - Replicação excessiva: Para impedir a replicação excessiva, a replicação DFS usa um sistema de créditos.  
      
  - Transformada pastas: Para impedir que os nomes de pasta transformada, a replicação DFS armazena dados conflitantes nas DfsrPrivate oculto\\pasta ConflictandDeleted (localizada no caminho local da pasta replicada). Por exemplo, criação de várias pastas simultaneamente com nomes idênticos em diferentes servidores replicados usando FRS faz com que o FRS renomear a pasta mais antigos (s). Em vez disso, a replicação DFS move as pastas mais antigas para a pasta local em conflito e excluídos.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>Replicação do DFS replica arquivos em ordem cronológica?

Não. Os arquivos podem ser replicados fora de ordem.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>Replicação do DFS replica arquivos que estão sendo usados por outro aplicativo?

Se um aplicativo abre um arquivo e cria um bloqueio de arquivo nele (impedindo que ele está sendo usado por outros aplicativos enquanto ele está aberto), a replicação do DFS não replicar o arquivo até que ele seja fechado. Se o aplicativo abre o arquivo com acesso de compartilhamento de leitura, o arquivo ainda pode ser replicado.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>A replicação DFS replique as permissões de arquivo NTFS, fluxos de dados alternativos, links físicos e pontos de nova análise?

  - Replicação do DFS replica permissões de arquivos NTFS e fluxos de dados alternativos.  
      
  - A Microsoft não suporta criando links de físicos NTFS para ou de arquivos em uma pasta replicada – isso pode causar problemas de replicação com os arquivos afetados. Arquivos de link físico são ignorados pela replicação DFS e não são replicados. Pontos de junção também não são replicados, e a replicação do DFS registra o evento 4406 para cada ponto de junção que encontrar.  
      
  - Somente pontos de nova análise replicados pela replicação do DFS são aqueles que usam a e/s\_de nova análise\_marca\_marca SYMLINK; no entanto, a replicação do DFS não garante que o destino de um symlink também é replicado. Para obter mais informações, consulte o [pergunte o blog da equipe de serviços de diretório](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - Arquivos com a e/s\_de nova análise\_marca\_eliminação de DUPLICATAS, e/s\_de nova análise\_marca\_SIS ou e/s\_de nova análise\_marca\_marcas de nova análise do HSM são replicadas como arquivos normais. Os buffers de dados de nova análise e a marca de nova análise não são replicados para outros servidores porque o ponto de nova análise funciona apenas no sistema local. Como tal, a replicação DFS replique pastas em volumes que usam a eliminação de duplicação de dados no Windows Server 2012 ou o armazenamento de instância única (SIS), no entanto, as informações de eliminação de duplicação de dados são mantidas separadamente por cada servidor no qual o serviço de função está habilitado.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>Replicação do DFS replicar alterações de carimbo de hora se nenhuma outra alteração é feitas no arquivo?

Não, a replicação do DFS replica arquivos para o qual a única alteração é uma alteração para o carimbo de hora. Além disso, o carimbo de hora alterado não é replicado para outros membros do grupo de replicação, a menos que outras alterações são feitas no arquivo.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>Que permissões atualizadas replicadas de replicação do DFS em um arquivo ou pasta?

Sim. Replicação do DFS replica as alterações de permissão para arquivos e pastas. Somente a parte do arquivo associado com a lista de controle de acesso (ACL) é replicada, embora a replicação do DFS ainda deve ler o arquivo inteiro para a área de preparo.


> [!NOTE]
> Alterar ACLs em um grande número de arquivos pode ter um impacto no desempenho de replicação. No entanto, ao usar a RDC, a quantidade de dados transferidos é proporcional ao tamanho de ACLs, não o tamanho do arquivo inteiro. A quantidade de tráfego de disco é ainda é proporcional ao tamanho dos arquivos porque os arquivos devem ser lidos de e para a pasta de preparo. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>A replicação DFS oferece suporte a mesclar arquivos de texto em caso de conflito?

Replicação do DFS não mesclar arquivos quando há um conflito. No entanto, ele tenta preservar a versão mais antiga do arquivo nas DfsrPrivate oculta\\ConflictandDeleted pasta no computador em que o conflito foi detectado.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>Replicação do DFS usar criptografia durante a transmissão de dados?

Sim. A replicação DFS usa conexões de procedimento remoto (RPC) com criptografia.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>É possível desabilitar o uso de RPC criptografado?

Não. O serviço de replicação do DFS usa chamadas de procedimento remoto (RPC) sobre TCP para replicar os dados. Para proteger as transferências de dados pela Internet, o serviço de replicação do DFS foi projetado para sempre usar a constante de nível de autenticação, `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`. Isso garante que a comunicação de RPC com a Internet sempre é criptografada. Portanto, não é possível desabilitar o uso de RPC criptografado pelo serviço de replicação do DFS.

Para obter mais informações, consulte os seguintes sites:

  - [Referência técnica de RPC](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [Sobre a compactação diferencial remota](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Constantes de nível de autenticação](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>Como são manipuladas as replicações simultâneas?

Há um Gerenciador de atualização por pasta replicada. Atualize o trabalho de gerentes independentemente um do outro.

Por padrão, um máximo de 16 (quatro no Windows Server 2003 R2) downloads simultâneos são compartilhados entre todas as conexões e grupos de replicação. Porque as conexões e atualizações do grupo de replicação não são serializadas, não há nenhuma ordem específica na qual as atualizações serão recebidas. Se dois agendamentos são abertos, atualizações são geralmente recebidas e instaladas de ambas as conexões ao mesmo tempo.

### <a name="how-do-i-force-replication-or-polling"></a>Como forçar a replicação ou sondagem?

Você pode forçar a replicação imediatamente usando o Gerenciamento DFS, conforme descrito em [editar agendamentos de replicação](https://technet.microsoft.com/en-us/library/Cc732278). Você também pode forçar a replicação usando o `Sync-DfsReplicationGroup` cmdlet, incluído no módulo do PowerShell de DFSR introduzido com o Windows Server 2012 R2 ou o **Dfsrdiag SyncNow** comando. Você pode forçar sondagem usando o `Update-DfsrConfigurationFromAD` cmdlet, ou o **Dfsrdiag PollAD** comando.

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>É possível configurar o tempo de espera entre as replicações para arquivos que mudam com frequência?

Não. Se a agenda estiver aberta, a replicação do DFS replicará alterações conforme ele observa-los. Não é possível configurar o tempo de espera para arquivos.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>É possível configurar a replicação unidirecional com a replicação DFS?

Sim. Se você estiver usando o Windows Server 2012 ou Windows Server 2008 R2, você pode criar uma pasta replicada somente leitura que replica o conteúdo por meio de uma conexão unidirecional. Para obter mais informações, consulte [tornar uma pasta replicada somente leitura em um membro específico](http://go.microsoft.com/fwlink/?linkid=156740) (http://go.microsoft.com/fwlink/?LinkId=156740).

Damos suporte à criação de uma conexão de replicação unidirecional com replicação do DFS no Windows Server 2008 ou Windows Server 2003 R2. Isso pode causar vários problemas, incluindo erros de topologia de verificação de integridade, problemas de teste e problemas com o banco de dados de replicação do DFS.

Se você estiver usando o Windows Server 2008 ou Windows Server 2003 R2, você pode simular uma conexão unidirecional, executando as seguintes ações:

  - Treinar os administradores façam alterações apenas nos servidores que você deseja designar como servidores primários. Em seguida, permitem que as alterações são replicadas para os servidores de destino.  
      
  - Configure as permissões de compartilhamento no servidor de destino para que os usuários finais não tem permissões de gravação. Se nenhuma alteração é permitida nos servidores de filiais, em seguida, não há nada para replicar novamente, simulando uma conexão unidirecional e manter baixa a utilização de WAN.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>Há uma maneira de forçar uma replicação completa de todos os arquivos, incluindo arquivos não alterados?

Não. Se a replicação do DFS considera os arquivos idênticos, ele não serão replicadas. Se não tiverem sido replicados arquivos alterados, a replicação do DFS serão replicadas-las quando configurado para fazer isso automaticamente. Para substituir o agendamento configurado, use o método WMI **ForceReplicate()** . No entanto, isso só é substituir uma agenda e não força a replicação de arquivos inalterados ou idênticos.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>O que acontece se o membro primário sofre uma perda de banco de dados durante a replicação inicial?

Durante a replicação inicial, os arquivos do membro primário sempre terá precedência na resolução de conflito ocorre se os membros de recebimento possuem versões diferentes de arquivos no membro primário. A designação do membro primário é armazenada nos serviços de domínio do Active Directory, e a designação é limpo depois que o membro primário é pronto para serem replicados, mas antes de replicar todos os membros da replicação do grupo.

Se houver falha na replicação inicial ou o serviço de replicação DFS é reiniciado durante a replicação, o membro primário vê a designação do membro primário no banco de dados de replicação DFS local e tenta novamente a replicação inicial. Se o banco de dados de replicação do DFS do membro primário for perdido depois de limpar a designação primária nos serviços de domínio do Active Directory, mas antes da replicação inicial de conclusão de todos os membros do grupo de replicação, todos os membros do grupo de replicação não conseguir replica a pasta porque nenhum servidor é designado como o membro primário. Se isso acontecer, use o **Dfsradmin associação /Set /isprimary:true** comando no servidor membro primário para restaurar a designação do membro primário manualmente.

Para obter mais informações sobre a replicação inicial, consulte [criar um grupo de replicação](https://technet.microsoft.com/en-us/library/cc725893).


> [!WARNING]
> A designação do membro primário é usada somente durante o processo de replicação inicial. Se você usar o <STRONG>Dfsradmin</STRONG> comando para especificar um membro primário para uma pasta replicada após a replicação for concluído, a replicação do DFS não designar o servidor como um membro primário nos serviços de domínio do Active Directory. No entanto, se o banco de dados de replicação do DFS no servidor sofrer subsequentemente irreversível corrupção ou perda de dados, o servidor tenta realizar uma replicação inicial que o membro primário, em vez de recuperar seus dados de outro membro da replicação grupo. Essencialmente, o servidor se torna um servidor primário não autorizado, que pode causar conflitos. Por esse motivo, especifique o membro primário manualmente somente se você tiver certeza de que a replicação inicial irremediavelmente falhou. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>O que acontece se o agendamento da replicação é fechada enquanto um arquivo está sendo replicado?

Se a compactação diferencial remota (RDC) estiver habilitada na conexão, a replicação de um arquivo maior do que 64 KB que começou a replicar imediatamente antes do fechamento de agendamento de entrada (ou alterar para **há largura de banda**) continua quando o Agendar é aberta (ou alterações em algo diferente de **há largura de banda**). A replicação continua o estado em que estava quando a replicação é interrompida.

Se a RDC estiver desativada, a replicação do DFS reinicia completamente a transferência de arquivos. Isso pode atrasar quando o arquivo está disponível no membro de recebimento.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>O que acontece quando dois usuários atualizam simultaneamente o mesmo arquivo em servidores diferentes?

Quando a replicação DFS detecta um conflito, ele usa a versão do arquivo que foi salvo pela última vez. Ele move o outro arquivo para o DfsrPrivate\\ConflictandDeleted pasta (sob o caminho local da pasta replicada no computador que o conflito resolvido). Ela permanece lá até que a limpeza da pasta conflito e excluídos, que ocorre quando a pasta conflito e excluídos excede o tamanho configurado ou a replicação do DFS encontra um erro de espaço em disco insuficiente. A pasta conflito e excluído não é replicada, e esse método de resolução de conflito evita o problema de diretórios transformados que era possível no FRS.

Quando ocorre um conflito, a replicação do DFS registra um evento informativo para o log de eventos de replicação do DFS. Esse evento não exigir ação do usuário pelos seguintes motivos:

  - Não é visível para os usuários (é visível somente para administradores de servidor).  
      
  - Replicação do DFS trata a pasta conflito e excluído como um cache. Quando um limite de cota for atingido, ele limpa alguns desses arquivos. Não há nenhuma garantia de que os arquivos conflitantes serão salvos.  
      
  - O conflito pode residir em um servidor diferente da origem do conflito.  
      

## <a name="staging"></a>Preparando

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>Replicação do DFS continuar a arquivos de teste quando a replicação é desabilitada por uma agenda ou de largura de banda de cota, ou quando uma conexão é desativado manualmente?

Não. Replicação do DFS não continua a arquivos de estágio fora de horários de replicação agendada, se a cota de limitação de largura de banda foi excedida, ou quando as conexões estão desabilitadas.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>Replicação do DFS impedir que outros aplicativos acessando um arquivo durante o preparo?

Não. Replicação do DFS abre arquivos de forma que não bloqueia a usuários ou aplicativos de abrir arquivos na pasta de replicação. Esse método é conhecido como "bloqueio oportunista".

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>É possível alterar o local da pasta de teste com a ferramenta de gerenciamento de DFS?

Sim. O local da pasta de preparo é configurado na **Advanced** guia da **propriedades** caixa de diálogo para cada membro de um grupo de replicação.

### <a name="when-are-files-staged"></a>Quando arquivos são preparados?

Arquivos são preparados no membro de envio quando o membro de recebimento solicita o arquivo (a menos que o arquivo é de 64 KB ou menor) conforme mostrado na tabela a seguir. Se a compactação diferencial remota (RDC) está desabilitado na conexão, o arquivo é preparado, a menos que ele é de 256 KB ou menos. Arquivos também são testados em membro de recebimento como eles são transferidos se eles forem menos de 64 KB de tamanho, embora você possa configurar essa configuração entre 16 KB e 1 MB. Se a agenda estiver fechada, os arquivos não são testados.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Os tamanhos mínimo de arquivo para arquivos de teste

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>RDC habilitado</th>
<th>RDC desabilitado</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Membro de envio</p></td>
<td><p>64 KB</p></td>
<td><p>256 KB</p></td>
</tr>
<tr class="odd">
<td><p>Membro de recebimento</p></td>
<td><p>64 KB por padrão</p></td>
<td><p>64 KB por padrão</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>O que acontece se um arquivo for alterado depois que ela for preparada, mas antes de ser completamente transmitido para o site remoto?

Se qualquer parte do arquivo já está sendo transmitido, a replicação do DFS continua a transmissão. Se o arquivo for alterado antes da replicação do DFS começa a transmitir o arquivo, a versão mais recente do arquivo é enviada.

## <a name="change-history"></a>Histórico de Alterações


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Date</th>
<th>Descrição</th>
<th>Reason</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>15 de novembro de 2018</p></td>
<td><p>Atualizado para o Windows Server de 2019.</p></td>
<td><p>Novo sistema operacional.</p></td>
</tr>
<tr class="even">
<td><p>9 de outubro de 2013</p></td>
<td><p>Atualizadas as quais são os limites com suporte de replicação do DFS? seção com resultados de testes no Windows Server 2012 R2.</p></td>
<td><p>Atualizações para a versão mais recente do Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>Em 30 de janeiro de 2013</p></td>
<td><p>Adicionada a replicação do DFS faz continuar arquivos de teste quando a replicação é desabilitada por uma agenda ou de largura de banda de cota, ou quando uma conexão é desativado manualmente? entrada.</p></td>
<td><p>Perguntas do cliente</p></td>
</tr>
<tr class="even">
<td><p>31 de outubro de 2012</p></td>
<td><p>Editado o que são os limites com suporte de replicação do DFS? entrada para aumentar o número testado de arquivos duplicados em um volume.</p></td>
<td><p>Comentários do cliente</p></td>
</tr>
<tr class="odd">
<td><p>15 de agosto de 2012</p></td>
<td><p>Editado arquivo permissões, fluxos de dados alternativos, links físicos NTFS de replicar o faz a replicação do DFS e pontos de nova análise? pontos de entrada para esclarecer como a replicação do DFS lida com links físicos e de nova análise ainda mais.</p></td>
<td><p>Comentários dos serviços de suporte técnico</p></td>
</tr>
<tr class="even">
<td><p>13 de junho de 2012</p></td>
<td><p>Editado o trabalho faz a replicação do DFS no ReFS ou volumes FAT? entrada para adicionar a discussão de referências.</p></td>
<td><p>Comentários do cliente</p></td>
</tr>
<tr class="odd">
<td><p>25 de abril de 2012</p></td>
<td><p>Editado arquivo permissões, fluxos de dados alternativos, links físicos NTFS de replicar o faz a replicação do DFS e pontos de nova análise? entrada para esclarecer como a replicação do DFS lida com links físicos.</p></td>
<td><p>Reduzir a confusão potencial</p></td>
</tr>
<tr class="even">
<td><p>30 de março de 2011</p></td>
<td><p>Editado a replicação do DFS pode replicar Outlook PST ou arquivos de banco de dados do Access do Microsoft Office? entrada para corrigir o impacto potencial de uso da replicação DFS com arquivos. pst e acesso.</p>
<p>Adicionado como melhorar o desempenho da replicação?</p></td>
<td><p>Perguntas do cliente sobre a entrada anterior, que incorretamente indicado replicando. pst ou acessar arquivos pode corromper o banco de dados de replicação do DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 de janeiro de 2011</p></td>
<td><p>Adicionados como arquivos podem ser recuperados do ConflictAndDeleted ou preexistente pastas?</p></td>
<td><p>Comentários do cliente</p></td>
</tr>
<tr class="even">
<td><p>20 de outubro de 2010</p></td>
<td><p>Adicionados como posso atualizar ou substituir um membro de replicação DFS?</p></td>
<td><p>Comentários do cliente</p></td>
</tr>
</tbody>
</table>

