---
title: 'Replicação do DFS: Perguntas frequentes (FAQ)'
ms.date: 06/18/2014
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 12410d619245153f759b54e7a8aff257888f04dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386067"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Replicação do DFS: Perguntas frequentes (FAQ)


Atualizado: 30 de abril de 2019

Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Este FAQ responde perguntas sobre a replicação de Sistema de Arquivos Distribuído (DFS) (também conhecida como DFS-R ou DFSR) para o Windows Server.

Para obter mais informações sobre Namespaces DFS, consulte [Namespaces DFS: Perguntas](https://technet.microsoft.com/library/ee404780)frequentes.

Para obter informações sobre as novidades do Replicação do DFS, consulte os seguintes tópicos:

  - [Visão geral dos namespaces do DFS e do replicação do DFS](https://technet.microsoft.com/library/jj127250) (no Windows Server 2012)  
      
  - [O que há de novo no tópico sistema de arquivos distribuído](https://technet.microsoft.com/library/ee307957) em [alterações na funcionalidade do Windows Server 2008 para o Windows Server 2008 R2](https://technet.microsoft.com/library/dd391932)  
      
  - [Sistema de arquivos distribuído](https://technet.microsoft.com/library/cc753479) tópico em [alterações na funcionalidade do Windows Server 2003 com SP1 para o Windows Server 2008](https://technet.microsoft.com/library/cc753208)  
      

Para obter uma lista das alterações recentes deste tópico, consulte a seção [Histórico de alterações](#change-history).

      

## <a name="interoperability"></a>Interoperabilidade

### <a name="can-dfs-replication-communicate-with-frs"></a>Replicação do DFS pode se comunicar com o FRS?

Nº Replicação do DFS não se comunica com o FRS (serviço de replicação de arquivo). O Replicação do DFS e o FRS podem ser executados no mesmo servidor ao mesmo tempo, mas eles nunca devem ser configurados para replicar as mesmas pastas ou subpastas porque isso pode causar perda de dados.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>Replicação do DFS pode substituir o FRS para replicação do SYSVOL

Sim, Replicação do DFS pode substituir o FRS para replicação SYSVOL em servidores que executam o Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2008 R2 ou o Windows Server 2008. Servidores que executam o Windows Server 2003 R2 não dão suporte ao uso de Replicação do DFS para replicar a pasta SYSVOL.

Para obter mais informações sobre como replicar o SYSVOL usando replicação do DFS, [consulte o guia de migração de replicação do SYSVOL: FRS para Replicação do DFS](https://technet.microsoft.com/library/dd640019).

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>Posso atualizar do FRS para Replicação do DFS sem perder as definições de configuração?

Sim. Para migrar a replicação do FRS para o Replicação do DFS, consulte os seguintes documentos:

  - Para migrar a replicação de pastas que não sejam a pasta [SYSVOL, consulte Guia de operações do DFS: Migrando do FRS para](http://go.microsoft.com/fwlink/?linkid=192776) o replicação do DFS e [o FRS2DFSR – um utilitário de migração de FRS para DFSR](http://go.microsoft.com/fwlink/?linkid=195437) (http://go.microsoft.com/fwlink/?LinkID=195437).  
      
  - Para migrar a replicação da pasta SYSVOL para replicação do DFS, [consulte Guia de migração de replicação do SYSVOL: FRS para Replicação do DFS](https://technet.microsoft.com/library/dd640019).  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>Posso usar Replicação do DFS em um ambiente misto do Windows/UNIX?

Sim. Embora Replicação do DFS ofereça suporte apenas à replicação de conteúdo entre servidores que executam o Windows Server, os clientes UNIX podem acessar compartilhamentos de arquivos nos servidores Windows. Para fazer isso, instale os serviços para NFS (sistemas de arquivos de rede) no servidor Replicação do DFS.

Você também pode usar a funcionalidade de cliente SMB/CIFS incluída em muitos clientes UNIX para acessar diretamente os compartilhamentos de arquivos do Windows, embora essa funcionalidade geralmente seja limitada ou exija modificações no ambiente do Windows (como desabilitar a assinatura SMB usando Política de Grupo).

Replicação do DFS interopera com NFS em um servidor que executa um sistema operacional Windows Server, mas você não pode replicar um ponto de montagem NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>Posso usar o Serviço de Cópias de Sombra de Volume com Replicação do DFS?

Sim. Replicação do DFS tem suporte em volumes de Serviço de Cópias de Sombra de Volume (VSS) e os instantâneos anteriores podem ser restaurados com êxito com o cliente de versões anteriores.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>Posso usar o backup do Windows (Ntbackup. exe) para fazer backup de uma pasta replicada remotamente?

Não, o uso do backup do Windows (Ntbackup. exe) em um computador que executa o Windows Server 2003 ou anterior para fazer backup do conteúdo de uma pasta replicada em um computador que executa o Windows Server 2012, o Windows Server 2008 R2 ou o Windows Server 2008 não tem suporte.

Para fazer backup de arquivos armazenados em uma pasta replicada, use Backup do Windows Server ou o Microsoft® System Center Data Protection Manager. Para obter informações sobre a funcionalidade de backup e recuperação no Windows Server 2008 R2 e no Windows Server 2008, consulte [backup e recuperação](https://technet.microsoft.com/library/Cc754097). Para obter mais informações, consulte [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) (http://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>As políticas do sistema de arquivos afetam Replicação do DFS?

Sim. Não configure políticas do sistema de arquivos em pastas replicadas. A política do sistema de arquivos reaplica as permissões NTFS a cada Política de Grupo intervalo de atualização. Isso pode resultar em violações de compartilhamento porque um arquivo aberto não é replicado até que o arquivo seja fechado.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>Replicação do DFS replicar caixas de correio hospedadas no Microsoft Exchange Server?

Nº Replicação do DFS não pode ser usado para replicar caixas de correio hospedadas no Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>Replicação do DFS as telas de arquivo de suporte criadas pelo Gerenciador de recursos do servidor de arquivos?

Sim. No entanto, as configurações de triagem de arquivo do File Server Resource Manager (FSRM) devem corresponder em ambas as extremidades da replicação. Além disso, Replicação do DFS tem seu próprio mecanismo de filtro para arquivos e pastas que você pode usar para excluir determinados arquivos e tipos de arquivos da replicação.

A seguir estão as práticas recomendadas para implementar triagens de arquivo ou cotas:

  - A pasta DfsrPrivate oculta não deve estar sujeita a cotas ou a triagens de arquivo.  
      
  - Os arquivos em tela não devem existir em nenhuma pasta replicada antes que a triagem seja habilitada.  
      
  - Nenhuma pasta pode exceder a cota antes que a cota seja habilitada.  
      
  - Você deve usar cotas rígidas com cuidado. É possível que membros individuais de um grupo de replicação permaneçam dentro de uma cota antes da replicação, mas excedam isso quando os arquivos são replicados. Por exemplo, se um usuário copiar um arquivo de 10 megabytes (MB) para o servidor A (em seguida, no limite rígido) e outro usuário copiar um arquivo de 5 MB no servidor B, quando a próxima replicação ocorrer, ambos os servidores excederão a cota em 5 megabytes. Isso pode fazer com que Replicação do DFS repita continuamente a replicação dos arquivos, causando buracos no vetor de versão e possíveis problemas de desempenho.  
      

### <a name="is-dfs-replication-cluster-aware"></a>O Replicação do DFS reconhece o cluster?

Sim, Replicação do DFS no Windows Server 2012 R2, o Windows Server 2012 e o Windows Server 2008 R2 incluem a capacidade de adicionar um cluster de failover como um membro de um grupo de replicação. Para obter mais informações, consulte [Adicionar um cluster de failover a um grupo de replicação](http://go.microsoft.com/fwlink/?linkid=155085) (http://go.microsoft.com/fwlink/?LinkId=155085). O serviço de Replicação do DFS nas versões do Windows anteriores ao Windows Server 2008 R2 não foi projetado para coordenar com um cluster de failover e o serviço não fará failover para outro nó.


> [!NOTE]
> Replicação do DFS não dá suporte à replicação de arquivos em volumes compartilhados do cluster. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>Replicação do DFS é compatível com eliminação de duplicação de dados?

Sim, Replicação do DFS pode replicar pastas em volumes que usam eliminação de duplicação de dados no Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>O é Replicação do DFS compatível com o RIS e o WDS?

Sim. Replicação do DFS replica volumes nos quais o SIS (armazenamento de instância única) está habilitado. O SIS é usado pelo RIS (serviços de instalação remota), pelo WDS (serviços de implantação do Windows) e pelo Windows Storage Server.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>É possível usar Replicação do DFS com Arquivos Offline?

Você pode usar Replicação do DFS e Arquivos Offline com segurança em cenários quando há apenas um usuário por vez, que grava nos arquivos. Isso é útil para os usuários que viajam entre duas filiais e desejam poder acessar seus arquivos em qualquer Branch ou offline. O Arquivos Offline armazena em cache os arquivos localmente para uso offline e Replicação do DFS replica os dados entre cada filial.

Não use Replicação do DFS com Arquivos Offline em um ambiente de vários usuários porque Replicação do DFS não fornece nenhum mecanismo de bloqueio distribuído ou recurso de check-out de arquivos. Se dois usuários modificarem o mesmo arquivo ao mesmo tempo em servidores diferentes, replicação do DFS moverá o arquivo mais antigo para\\a pasta DfsrPrivate ConflictandDeleted (localizada sob o caminho local da pasta replicada) durante a próxima replicação.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>Quais aplicativos antivírus são compatíveis com Replicação do DFS?

Os aplicativos antivírus podem causar uma replicação excessiva se suas atividades de verificação alteram os arquivos em uma pasta replicada. Para obter mais informações, [teste a interoperabilidade de aplicativos antivírus com o replicação do DFS](http://go.microsoft.com/fwlink/?linkid=73990) (http://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>Quais são os benefícios de usar Replicação do DFS em vez do Windows SharePoint Services?

O Windows® SharePoint® Services fornece uma coerência rígida na forma de funcionalidade de check-out do arquivo que o Replicação do DFS não. Se você estiver preocupado com várias pessoas editando o mesmo arquivo, é recomendável usar o Windows SharePoint Services. O Windows SharePoint Services 2,0 com Service Pack 2 está disponível como parte do Windows Server 2003 R2. O Windows SharePoint Services pode ser baixado do site da Microsoft; Ele não está incluído nas versões mais recentes do Windows Server. No entanto, se você estiver replicando dados em vários sites e os usuários não editarem os mesmos arquivos ao mesmo tempo, Replicação do DFS fornecerá maior largura de banda e gerenciamento mais simples.

## <a name="limitations-and-requirements"></a>Limitações e requisitos

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>Replicação do DFS pode replicar entre filiais sem uma conexão VPN?

Sim — pressupondo que haja um link de WAN (rede de longa distância) privado (não a Internet) conectando as filiais. No entanto, você deve abrir as portas adequadas em firewalls externos. Replicação do DFS usa o mapeador de ponto de extremidade RPC (porta 135) e uma porta efêmera atribuída aleatoriamente acima de 1024. Você pode usar a ferramenta de linha de comando **Dfsrdiag** para especificar uma porta estática em vez da porta efêmera. Para obter mais informações sobre como especificar o mapeador de ponto de extremidade RPC, consulte o [artigo 154596](http://go.microsoft.com/fwlink/?linkid=73991) na http://go.microsoft.com/fwlink/?LinkId=73991) base de dados de conhecimento Microsoft (.

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>O Replicação do DFS pode replicar arquivos criptografados com o Encrypting File System?

Nº Replicação do DFS não replicará arquivos ou pastas que são criptografados usando o Encrypting File System (EFS). Se um usuário criptografar um arquivo que foi replicado anteriormente, Replicação do DFS exclui o arquivo de todos os outros membros do grupo de replicação. Isso garante que a única cópia disponível do arquivo seja a versão criptografada no servidor.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>Replicação do DFS pode replicar arquivos de banco de dados do Outlook. pst ou do Microsoft Office Access?

Replicação do DFS pode replicar com segurança arquivos de pasta pessoal do Microsoft Outlook (. pst) e arquivos do Microsoft Access somente se eles forem armazenados para fins de arquivamento e não forem acessados pela rede usando um cliente como o Outlook ou o Access (para abrir o. pst ou o Access arquivos, primeiro copie os arquivos para um dispositivo de armazenamento local). Os motivos para isso são os seguintes:

  - Abrir arquivos. pst em conexões de rede pode levar à corrupção de dados nos arquivos. pst. Para obter mais informações sobre por que os arquivos. pst não podem ser acessados com segurança por meio de uma rede, consulte o http://go.microsoft.com/fwlink/?LinkId=125363) [artigo 297019](http://go.microsoft.com/fwlink/?linkid=125363) na base de dados de conhecimento Microsoft (.  
      
  - arquivos. pst e Access tendem a permanecer abertos por longos períodos de tempo enquanto são acessados por um cliente, como o Outlook ou o Office Access. Isso impede Replicação do DFS de replicar esses arquivos até que eles sejam fechados.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>Posso usar Replicação do DFS em um grupo de trabalho?

Nº Replicação do DFS se baseia em Active Directory serviços de domínio® para configuração. Ele só funcionará em um domínio.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>É possível replicar mais de uma pasta em um único servidor?

Sim. Replicação do DFS pode replicar várias pastas entre servidores. Verifique se cada uma das pastas replicadas tem um caminho raiz exclusivo e se elas não se sobrepõem. Por exemplo, d:\\Sales e d\\: Accounting podem ser os caminhos raiz para duas pastas replicadas,\\mas d: Sales\\e d:\\Sales Reports não podem ser os caminhos raiz para duas pastas replicadas.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>Replicação do DFS requer namespaces DFS?

Nº Os namespaces do Replicação do DFS e do DFS podem ser usados separadamente ou em conjunto. Além disso, Replicação do DFS pode ser usado para replicar namespaces DFS autônomos, o que não era possível com o FRS.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>Replicação do DFS requer sincronização de tempo entre servidores?

Nº Replicação do DFS não requer explicitamente a sincronização de horário entre servidores. No entanto, Replicação do DFS requer que os relógios do servidor correspondam de forma mais rigorosa. Os relógios do servidor devem ser definidos em cinco minutos uns dos outros (por padrão) para que a autenticação Kerberos funcione corretamente. Por exemplo, Replicação do DFS usa carimbos de data/hora para determinar qual arquivo tem precedência no caso de um conflito. Os tempos precisos também são importantes para coleta de lixo, agendas e outros recursos.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>Replicação do DFS dá suporte à replicação de um volume inteiro?

Sim. No entanto, você deve primeiro instalar o Windows Server 2003 Service Pack 2 ou o hotfix. Para obter mais informações, consulte o [artigo 920335](http://go.microsoft.com/fwlink/?linkid=76776) na base de dados http://go.microsoft.com/fwlink/?LinkId=76776) de conhecimento Microsoft (. Além disso, a replicação de um volume inteiro pode causar os seguintes problemas:

  - Se o volume contiver um arquivo de paginação do Windows, a replicação falhará e registrará o evento 4312 do DFSR no log de eventos do sistema.  
      
  - Replicação do DFS define o sistema e os atributos ocultos na pasta replicada no (s) servidor (es) de destino. Isso ocorre porque o Windows aplica o sistema e os atributos ocultos à pasta raiz do volume por padrão. Se o caminho local da pasta replicada no (s) servidor (es) de destino for também uma raiz do volume, nenhuma alteração adicional será feita nos atributos da pasta.  
      
  - Ao replicar um volume que contém a pasta do sistema Windows, Replicação do DFS reconhece a pasta% WINDIR% e não a Replica. No entanto, o Replicação do DFS replica pastas usadas por aplicativos que não são da Microsoft, o que pode fazer com que os aplicativos falhem no (s) servidor (es) de destino se os aplicativos tiverem problemas de interoperabilidade com Replicação do DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>Replicação do DFS dá suporte a RPC sobre HTTP?

Nº

### <a name="does-dfs-replication-work-across-wireless-networks"></a>Replicação do DFS funciona em redes sem fio?

Sim. Replicação do DFS é independente do tipo de conexão.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>Replicação do DFS funciona em volumes ReFS ou FAT?

Nº O Replicação do DFS dá suporte a volumes formatados somente com o sistema de arquivos NTFS; Não há suporte para o sistema de arquivos resiliente (ReFS) e o sistema de arquivos FAT. Replicação do DFS requer NTFS porque ele usa o diário de alterações do NTFS e outros recursos do sistema de arquivos NTFS.

### <a name="does-dfs-replication-work-with-sparse-files"></a>Replicação do DFS funciona com arquivos esparsos?

Sim. Você pode replicar arquivos esparsos. O atributo **esparso** é preservado no membro de recebimento.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>Preciso fazer logon como administrador para replicar arquivos?

Nº Replicação do DFS é um serviço executado na conta sistema local, portanto, não é necessário fazer logon como administrador para replicar. No entanto, você deve ser um administrador de domínio ou administrador local dos servidores de arquivos afetados para fazer alterações na configuração de Replicação do DFS.

Para obter mais informações, consulte "requisitos e delegação de segurança do Replicação do DFS" no [delegar a capacidade de gerenciar replicação do DFS](http://go.microsoft.com/fwlink/?linkid=182294) (http://go.microsoft.com/fwlink/?LinkId=182294).

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>Como posso atualizar ou substituir um membro Replicação do DFS?

Para atualizar ou substituir um membro Replicação do DFS, consulte esta postagem de blog no blog pergunte ao diretório de serviços de diretórios: [Substituindo o hardware ou o sistema operacional do membro do DFSR](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>O é Replicação do DFS adequado para replicar perfis móveis?

Sim. Certos cenários têm suporte ao replicar perfis de usuário móvel. Para obter informações sobre os cenários com suporte, consulte a [declaração de suporte da Microsoft sobre os dados de perfil de usuário replicados](http://go.microsoft.com/fwlink/?linkid=201282) (http://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>Há um limite ou limite de caracteres de arquivo para a profundidade da pasta?

O Windows e Replicação do DFS caminhos de pasta de suporte com até 32000 caracteres. Replicação do DFS não está limitado a caminhos de pasta de 260 caracteres.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>Os membros de um grupo de replicação devem residir no mesmo domínio?

Nº Os grupos de replicação podem se estender entre domínios dentro de uma única floresta, mas não em florestas diferentes.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>Quais são os limites com suporte de Replicação do DFS?

A lista a seguir fornece um conjunto de diretrizes de escalabilidade que foram testadas pela Microsoft no Windows Server 2012 R2:

  - Tamanho de todos os arquivos replicados em um servidor: 100 terabytes.  
      
  - Número de arquivos replicados em um volume: 70 milhões.  
      
  - Tamanho máximo do arquivo: 250 gigabytes.  
      


> [!IMPORTANT]
> Ao criar grupos de replicação com um grande número ou tamanho de arquivos, é recomendável exportar um clone de banco de dados e usar técnicas de pré-propagação para minimizar a duração da replicação inicial. Para obter mais informações, <A href="http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx">consulte replicação do DFS sincronização inicial no Windows Server 2012 R2: Ataque dos clones</A>. 
<br>


A lista a seguir fornece um conjunto de diretrizes de escalabilidade que foram testadas pela Microsoft no Windows Server 2012, no Windows Server 2008 R2 e no Windows Server 2008:

  - Tamanho de todos os arquivos replicados em um servidor: 10 terabytes.  
      
  - Número de arquivos replicados em um volume: 11 milhões.  
      
  - Tamanho máximo do arquivo: 64 gigabytes.  
      


> [!NOTE]
> Não há mais um limite para o número de grupos de replicação, pastas replicadas, conexões ou membros do grupo de replicação. 
<br>


Para obter uma lista das diretrizes de escalabilidade que foram testadas pela Microsoft para o Windows Server 2003 R2, consulte http://go.microsoft.com/fwlink/?LinkId=75043) [diretrizes de escalabilidade replicação do DFS](http://go.microsoft.com/fwlink/?linkid=75043) (.

### <a name="when-should-i-not-use-dfs-replication"></a>Quando não devo usar Replicação do DFS?

Não use Replicação do DFS em um ambiente em que vários usuários atualizem ou modifiquem os mesmos arquivos simultaneamente em servidores diferentes. Isso pode fazer com que replicação do DFS mova cópias conflitantes dos arquivos para a pasta DfsrPrivate\\ConflictandDeleted oculta.

Quando vários usuários precisam modificar os mesmos arquivos ao mesmo tempo em servidores diferentes, use o recurso de check-out do arquivo do Windows SharePoint Services para garantir que apenas um usuário esteja trabalhando em um arquivo. O Windows SharePoint Services 2,0 com Service Pack 2 está disponível como parte do Windows Server 2003 R2. O Windows SharePoint Services pode ser baixado do site da Microsoft; Ele não está incluído nas versões mais recentes do Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>Por que uma atualização de esquema é necessária para Replicação do DFS?

Replicação do DFS usa novos objetos no contexto de nomeação de domínio de Active Directory Domain Services para armazenar informações de configuração. Esses objetos são criados quando você atualiza o esquema de Active Directory Domain Services. Para obter mais informações, consulte [revisar requisitos para replicação do DFS](http://go.microsoft.com/fwlink/?linkid=182264) (http://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Ferramentas de monitoramento e gerenciamento

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>Posso automatizar o relatório de integridade para receber avisos?

Sim. Há três maneiras de automatizar os relatórios de integridade:

  - Use o módulo DFSR do Windows PowerShell incluído no Windows Server 2012 R2 ou DfsrAdmin. exe em conjunto com as tarefas agendadas para gerar relatórios de integridade regularmente. Para obter mais informações, consulte [automatizando relatórios de integridade replicação do DFS](http://go.microsoft.com/fwlink/?linkid=74010) (http://go.microsoft.com/fwlink/?LinkId=74010).  
      
  - Use o pacote de gerenciamento Replicação do DFS para System Center Operations Manager para criar alertas com base em condições especificadas.  
      
  - Use o provedor WMI Replicação do DFS para alertas de script.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>Posso usar o Microsoft System Center Operations Manager para monitorar Replicação do DFS?

Sim. Para obter mais informações, consulte o [pacote de gerenciamento do replicação do DFS para System Center Operations Manager 2007](http://go.microsoft.com/fwlink/?linkid=182265) no centro de http://go.microsoft.com/fwlink/?LinkId=182265) download da Microsoft (.

### <a name="does-dfs-replication-support-remote-management"></a>Replicação do DFS dá suporte ao gerenciamento remoto?

Sim. O Replicação do DFS dá suporte ao gerenciamento remoto usando o console de Gerenciamento DFS e o comando **Adicionar grupo de replicação** . Por exemplo, no servidor A, você pode se conectar a um grupo de replicação definido na floresta com os servidores A e B como membros.

O gerenciamento de DFS está incluído no Windows Server 2012 R2, no Windows Server 2012, no Windows Server 2008 R2, no Windows Server 2008 e no Windows Server 2003 R2. Para gerenciar Replicação do DFS de outras versões do Windows, use Área de Trabalho Remota ou o [ferramentas de administração de servidor remoto para o Windows 7](https://technet.microsoft.com/library/Ee449475).


> [!IMPORTANT]
> Para exibir ou gerenciar grupos de replicação que contêm pastas replicadas somente leitura ou membros que são clusters de failover, você deve usar a versão do Gerenciamento DFS que está incluída no Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, <a href="http://go.microsoft.com/fwlink/p/?linkid=238560">remoto Ferramentas de administração de servidor para Windows 8</a>ou o <a href="https://technet.microsoft.com/library/ee449475">ferramentas de administração de servidor remoto para Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>O Ultrasound e o sonar funcionam com Replicação do DFS?

Nº Replicação do DFS tem seu próprio conjunto de ferramentas de monitoramento e diagnóstico. O Ultrasound e o sonar são capazes de monitorar somente o FRS.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>Como os arquivos podem ser recuperados das pastas ConflictAndDeleted ou preexistentes?

Para recuperar arquivos perdidos, restaure os arquivos da pasta do sistema de arquivos ou da pasta compartilhada usando o histórico de arquivos, o comando **restaurar versões anteriores** no explorador de arquivos ou restaurando os arquivos do backup. Para recuperar arquivos diretamente da pasta ConflictAndDeleted ou preexistente, use os `Get-DfsrPreservedFiles` cmdlets do e `Restore-DfsrPreservedFiles` do Windows PowerShell (incluídos no módulo DFSR no Windows Server 2012 R2) ou o script de exemplo [RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr) do MSDN Galeria de códigos. Esse script destina-se apenas à recuperação de desastres e é fornecido no estado em que se encontra, sem garantia.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>Há uma maneira de saber o estado da replicação?

Sim. Há várias maneiras de monitorar a replicação:

  - Replicação do DFS tem um pacote de gerenciamento para System Center Operations Manager que fornece monitoramento proativo.  
      
  - O Gerenciamento DFS tem um relatório de diagnóstico na caixa para o registro posterior da replicação, eficiência da replicação e o número de arquivos e pastas em um determinado grupo de replicação.  
      
  - O módulo DFSR Windows PowerShell no Windows Server 2012 R2 contém cmdlets para iniciar testes de propagação e gravar relatórios de propagação e de integridade. Para obter mais informações, consulte [sistema de arquivos distribuído cmdlets de replicação no Windows PowerShell](https://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag. exe é uma ferramenta de linha de comando que pode gerar uma contagem de pendências ou disparar um teste de propagação. Ambos mostram o estado da replicação. A propagação mostrará se os arquivos estão sendo replicados em todos os nós. A lista de pendências mostra quantos arquivos ainda precisam ser replicados antes que dois computadores estejam em sincronia. A contagem de pendências é o número de atualizações que um membro do grupo de replicação não processou. Em computadores que executam o Windows Server 2012 R2, o Windows Server 2012 ou o Windows Server 2008 R2, o Dfsrdiag. exe também pode exibir as atualizações que o Replicação do DFS está replicando no momento.  
      
  - Os scripts podem usar o WMI para coletar informações da acumulação — manualmente ou por meio do MOM.  
      

## <a name="performance"></a>Desempenho

### <a name="does-dfs-replication-support-dial-up-connections"></a>Replicação do DFS oferece suporte a conexões dial-up?

Embora Replicação do DFS funcionem em velocidades discadas, ele poderá ser registrado em log se houver um grande número de alterações a serem replicadas. Se alterações pequenas forem feitas nos arquivos existentes, Replicação do DFS com a compactação diferencial remota (RDC) fornecerá um desempenho muito maior do que copiar o arquivo diretamente.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>Replicação do DFS executar a detecção de largura de banda?

Nº Replicação do DFS não executa a detecção de largura de banda. Você pode configurar Replicação do DFS para usar uma quantidade limitada de largura de banda em uma base por conexão (limitação de largura de banda). No entanto, Replicação do DFS não reduzirá ainda mais a utilização da largura de banda se a interface de rede ficar saturada e Replicação do DFS poderá saturar o link por períodos curtos. A limitação da largura de banda com Replicação do DFS não é completamente precisa porque Replicação do DFS limita a largura de banda por meio da limitação de chamadas RPC. Como resultado, vários buffers em níveis inferiores da pilha de rede (incluindo RPC) podem interferir, causando intermitências de tráfego de rede.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>Replicação do DFS limitar a largura de banda por agendamento, por servidor ou por conexão?

Se você configurar a limitação da largura de banda ao especificar o agendamento, todas as conexões desse grupo de replicação usarão essa configuração para limitação da largura de banda. A limitação da largura de banda também pode ser definida como uma configuração de nível de conexão usando o Gerenciamento DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>Replicação do DFS usar Active Directory Domain Services para calcular os links de site e os custos de conexão?

Nº Replicação do DFS usa a topologia definida pelo administrador, que é independente de Active Directory Domain Services o custo do site.

### <a name="how-can-i-improve-replication-performance"></a>Como posso melhorar o desempenho da replicação?

Para saber mais sobre os diferentes métodos de ajuste do desempenho de replicação, consulte [ajustando o desempenho de replicação no DFSR](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) no [blog da equipe de serviços de diretório](http://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>Como Replicação do DFS evitar saturação uma conexão?

No Replicação do DFS você define a largura de banda máxima que deseja usar em uma conexão e o serviço mantém esse nível de uso de rede. Isso é diferente do Serviço de Transferência Inteligente em Segundo Plano (BITS) e Replicação do DFS não satura a conexão se você defini-la adequadamente.

No entanto, a limitação da largura de banda não é de 100% precisa e Replicação do DFS pode saturar o link por curtos períodos de tempo. Isso ocorre porque Replicação do DFS limita a largura de banda por meio da limitação de chamadas RPC. Como esse processo depende de vários buffers em níveis inferiores da pilha de rede, incluindo RPC, o tráfego de replicação tende a viajar em intermitências que podem, às vezes, saturar os links de rede.

Replicação do DFS no Windows Server 2008 inclui vários aprimoramentos de desempenho, conforme discutido em [sistema de arquivos distribuído](https://technet.microsoft.com/library/Cc753479), um tópico em [alterações na funcionalidade do Windows Server 2003 com SP1 para o Windows Server 2008](https://technet.microsoft.com/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>Como Replicação do DFS o desempenho é comparado com o FRS?

Replicação do DFS é muito mais rápido do que o FRS, especialmente quando pequenas alterações são feitas em arquivos grandes e a RDC está habilitada. Por exemplo, com a RDC, uma pequena alteração em uma apresentação de® do PowerPoint de 2 MB pode resultar em apenas 60 kilobytes (KB) sendo enviados pela rede — uma economia de 97% em bytes transferidos.

A RDC não é usada em arquivos menores que 64 KB e pode não ser benéfico em LANs de alta velocidade em que a largura de banda da rede não é contendeda. A RDC pode ser desabilitada em uma base por conexão usando o Gerenciamento DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>Com que frequência Replicação do DFS replicar dados?

Os dados são replicados de acordo com o agendamento que você definir. Por exemplo, você pode definir a agenda para intervalos de 15 minutos, sete dias por semana. Durante esses intervalos, a replicação é habilitada. A replicação inicia logo após a detecção de uma alteração de arquivo (geralmente em segundos).

O agendamento do grupo de replicação pode ser definido como coordenada de tempo universal (UTC) enquanto a agenda de conexão é definida como a hora local do membro de recebimento. Leve isso em conta quando o grupo de replicação abranger vários fusos horários. Hora local significa a hora do membro que hospeda a conexão de entrada. O agendamento exibido da conexão de entrada e a conexão de saída correspondente refletem as diferenças de fuso horário quando a agenda é definida como hora local.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>Qual será a quantidade de recursos do sistema do meu servidor Replicação do DFS consumir?

O disco, a memória e os recursos de CPU usados pelo Replicação do DFS dependem de vários fatores, incluindo o número e o tamanho dos arquivos, a taxa de alteração, o número de membros do grupo de replicação e o número de pastas replicadas. Além disso, alguns recursos são mais difíceis de estimar. Por exemplo, a tecnologia ESE (mecanismo de armazenamento extensível) usada para o Replicação do DFS banco de dados pode consumir um grande percentual de memória disponível, que é liberada sob demanda. Aplicativos que não sejam Replicação do DFS podem ser hospedados no mesmo servidor, dependendo da configuração do servidor. No entanto, ao hospedar vários aplicativos ou funções de servidor em um único servidor, é importante que você teste essa configuração antes de implementá-la em um ambiente de produção.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>O que acontecerá se um link de WAN falhar durante a replicação?

Se a conexão for desativada, Replicação do DFS continuará tentando replicar enquanto o agendamento estiver aberto. Também haverá erros de conectividade indicados no log de eventos Replicação do DFS que podem ser coletados usando o MOM (proativamente por meio de alertas) e o relatório de integridade Replicação do DFS (reativamente, como quando um administrador o executa).

## <a name="remote-differential-compression-details"></a>Detalhes da compactação diferencial remota

### <a name="are-changes-compressed-before-being-replicated"></a>As alterações são compactadas antes de serem replicadas?

Sim. Partes alteradas de arquivos são compactadas antes de serem enviadas para todos os tipos de arquivo, exceto os seguintes (que já estão compactados):. WMA,. wmv,. zip,. jpg,. mpg,. MPEG,. M1V,. MP2,. mp3,. MPa,. cab,. wav,. SND,. au,. ASF,. WM,. avi,. z,. gz,. tgz e. FRX. As configurações de compactação para esses tipos de arquivo não são configuráveis no Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>Um administrador pode desativar a RDC ou alterar o limite?

Sim. Você pode desativar o RDC por meio da página de propriedades de uma determinada conexão. A desabilitação da RDC pode reduzir a utilização da CPU e a latência de replicação em links de LAN (rede local) rápidos que não têm nenhuma restrição de largura de banda ou para grupos de replicação que consistem principalmente em arquivos menores que 64 KB. Se você optar por desabilitar o RDC em uma conexão, teste a eficiência da replicação antes e depois da alteração para verificar se você melhorou o desempenho da replicação.

Você pode alterar o limite de tamanho de RDC usando o comando **Dfsradmin Connection Set** , o provedor WMI replicação do DFS ou editando manualmente o arquivo XML de configuração.

### <a name="does-rdc-work-on-all-file-types"></a>A RDC funciona em todos os tipos de arquivos?

Sim. A RDC computa diferenças no nível de bloco, independentemente do tipo de dados File. No entanto, a RDC funciona com mais eficiência em determinados tipos de arquivo, como documentos do Word, arquivos PST e imagens VHD.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>Como funciona a RDC em um arquivo compactado?

Replicação do DFS usa RDC, que computa os blocos no arquivo que foram alterados e envia apenas esses blocos pela rede. Replicação do DFS não precisa saber nada sobre o conteúdo do arquivo — somente os blocos foram alterados.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>A RDC entre arquivos está habilitada ao atualizar para o Windows Server Enterprise Edition ou Datacenter Edition?

As edições Standard do Windows Server não oferecem suporte a RDC entre arquivos. No entanto, ele é habilitado automaticamente quando você atualiza para uma edição que dá suporte a RDC entre arquivos ou se um membro da conexão de replicação estiver executando uma edição com suporte. Para obter uma lista de edições que dão suporte a RDC entre arquivos, consulte Quais edições do sistema operacional Windows dão suporte a RDC entre arquivos?

### <a name="is-rdc-true-block-level-replication"></a>A replicação em nível de bloco do RDC é verdadeira?

Nº A RDC é um protocolo de uso geral para compactação de transferência de arquivos. Replicação do DFS usa RDC em blocos no nível de arquivo, não no nível de bloco de disco. A RDC divide um arquivo em blocos. Para cada bloco em um arquivo, ele calcula uma assinatura, que é um pequeno número de bytes que podem representar o bloco maior. O conjunto de assinaturas é transferido do servidor para o cliente. O cliente compara as assinaturas do servidor com as suas próprias. Em seguida, o cliente solicita que o servidor envie somente os dados para assinaturas que ainda não estão no cliente.

### <a name="what-happens-if-i-rename-a-file"></a>O que acontece se eu renomear um arquivo?

Replicação do DFS renomeia o arquivo em todos os outros membros do grupo de replicação durante a próxima replicação. Os arquivos são rastreados usando uma ID exclusiva, portanto, renomear um arquivo e mover o arquivo dentro da réplica não tem nenhum efeito sobre a capacidade de Replicação do DFS replicar um arquivo.

### <a name="what-is-cross-file-rdc"></a>O que é RDC entre arquivos?

A RDC entre arquivos permite que Replicação do DFS use a RDC mesmo quando um arquivo com o mesmo nome não existir na extremidade do cliente. A RDC entre arquivos usa uma heurística para determinar arquivos que são semelhantes ao arquivo que precisa ser replicado e usa blocos de arquivos semelhantes que são idênticos ao arquivo de replicação para minimizar a quantidade de dados transferidos pela WAN. A RDC entre arquivos pode usar blocos de até cinco arquivos semelhantes nesse processo.

Para usar a RDC entre arquivos, um membro da conexão de replicação deve estar executando uma edição do Windows que ofereça suporte a RDC entre arquivos. Para obter uma lista de edições que dão suporte a RDC entre arquivos, consulte Quais edições do sistema operacional Windows dão suporte a RDC entre arquivos?

### <a name="what-is-rdc"></a>O que é RDC?

A RDC (Compactação Diferencial Remota) é um protocolo de cliente-servidor que pode ser usado para atualizar arquivos com eficiência em uma rede de largura de banda limitada. A RDC detecta inserções, remoções e reorganizaçãos de dados em arquivos, permitindo que Replicação do DFS replicar apenas as alterações quando os arquivos forem atualizados. A RDC é usada somente para arquivos que são 64 KB ou maiores por padrão. A RDC pode usar uma versão mais antiga de um arquivo com o mesmo nome na pasta replicada ou na pasta\\DfsrPrivate ConflictandDeleted (localizada sob o caminho local da pasta replicada).

### <a name="when-is-rdc-used-for-replication"></a>Quando a RDC é usada para replicação?

A RDC é usada quando o arquivo excede um limite de tamanho mínimo. Esse limite de tamanho é de 64 KB por padrão. Depois que um arquivo que excede esse limite tiver sido replicado, as versões atualizadas do arquivo sempre usarão a RDC, a menos que uma parte grande do arquivo seja alterada ou a RDC esteja desabilitada.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>Quais edições do sistema operacional Windows dão suporte a RDC entre arquivos?

Para usar a RDC entre arquivos, um membro da conexão de replicação deve estar executando uma edição do sistema operacional Windows que dá suporte a RDC entre arquivos. A tabela a seguir mostra quais edições do sistema operacional Windows dão suporte a RDC entre arquivos.

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>Disponibilidade de RDC entre arquivos em edições do sistema operacional Windows

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
<td><p>Sim<em></p></td>
<td><p>Não disponível</p></td>
<td><p>Sim</em></p></td>
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

\*Opcionalmente, você pode desabilitar a RDC entre arquivos no Windows Server 2012 R2.

## <a name="replication-details"></a>Detalhes da replicação

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>Posso alterar o caminho de uma pasta replicada depois que ela é criada?

Nº Se você precisar alterar o caminho de uma pasta replicada, deverá excluí-la no Gerenciamento DFS e adicioná-la novamente como uma nova pasta replicada. Replicação do DFS, em seguida, usa a RDC (Compactação Diferencial Remota) para executar uma sincronização que determina se os dados são os mesmos nos membros de envio e recebimento. Ele não replica todos os dados na pasta novamente.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>Posso configurar quais atributos de arquivo são replicados?

Não, você não pode configurar quais atributos de arquivo que Replicação do DFS replica.

Para obter uma lista de valores de atributo e suas descrições, consulte [atributos](http://go.microsoft.com/fwlink/?linkid=182268) de arquivo http://go.microsoft.com/fwlink/?LinkId=182268) no MSDN (.

Os valores de atributo a seguir são definidos usando `SetFileAttributes dwFileAttributes` a função e são replicados por replicação do DFS. As alterações nesses valores de atributo disparam a replicação dos atributos. O conteúdo do arquivo não é replicado, a menos que o conteúdo também seja alterado. Para obter mais informações, consulte a [função SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) na biblioteca MSDN http://go.microsoft.com/fwlink/?LinkId=182269) (.

  - \_ATRIBUTO\_DE ARQUIVO OCULTO  
      
  - \_ATRIBUTO\_DE ARQUIVO READONLY  
      
  - SISTEMA\_DE\_ATRIBUTOS DE ARQUIVO  
      
  - ATRIBUTO\_DE\_ARQUIVOSEM\_CONTEÚDO\_INDEXADO  
      
  - \_ATRIBUTO\_DE ARQUIVO OFFLINE  
      

Os valores de atributo a seguir são replicados por Replicação do DFS, mas não disparam a replicação.

  - ARQUIVO\_MORTO\_DE ATRIBUTO  
      
  - \_ATRIBUTO\_DE ARQUIVO NORMAL  
      

Os seguintes valores de atributo de arquivo também disparam a replicação, embora não possam `SetFileAttributes` ser definidos usando a `GetFileAttributes` função (use a função para exibir os valores de atributo).

  - PONTO DE\_NOVA ANÁLISE\_DE ATRIBUTO DE ARQUIVO\_  
      

> [!NOTE]
> Replicação do DFS não replica valores de atributo de ponto de nova análise, a menos que a marca de nova análise seja IO_REPARSE_TAG_SYMLINK. Os arquivos com as marcas de nova análise IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS ou IO_REPARSE_TAG_HSM são replicados como arquivos normais. No entanto, a marca de nova análise e os buffers de dados de nova análise não são replicados para outros servidores porque o ponto de nova análise funciona apenas no sistema local. 
<br>

  - \_ATRIBUTO\_DE ARQUIVO COMPACTADO  
      
  - \_ATRIBUTO\_DE ARQUIVO CRIPTOGRAFADO  
      

> [!NOTE]
> Replicação do DFS não replica os arquivos que são criptografados usando o Encrypting File System (EFS). Replicação do DFS replica arquivos que são criptografados usando software não Microsoft, mas somente se ele não definir o valor do atributo FILE_ATTRIBUTE_ENCRYPTED no arquivo. 
<br>

  - ARQUIVO ESPARSO\_\_DEATRIBUTODE\_ARQUIVO  
      
  - DIRETÓRIO\_DE\_ATRIBUTOS DE ARQUIVO  
      

Replicação do DFS não replica o valor temporário do\_atributo\_de arquivo.

### <a name="can-i-control-which-member-is-replicated"></a>Posso controlar qual membro é replicado?

Sim. Você pode escolher uma topologia ao criar um grupo de replicação. Ou você pode selecionar **nenhuma topologia** e configurar manualmente as conexões depois que o grupo de replicação tiver sido criado.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>Posso propagar um membro do grupo de replicação com dados antes da replicação inicial?

Sim. O Replicação do DFS dá suporte à cópia de arquivos para um membro do grupo de replicação antes da replicação inicial. Esse "pré-teste" pode reduzir drasticamente a quantidade de dados replicados durante a replicação inicial.

A replicação inicial não precisa replicar o conteúdo quando os arquivos diferem somente por atributos reais ou carimbos de data/hora. Um atributo real é um atributo que pode ser definido pela função `SetFileAttributes`do Win32. Para obter mais informações, consulte a [função SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) na biblioteca MSDN http://go.microsoft.com/fwlink/?LinkId=182269) (. Se dois arquivos diferirem por outros atributos, como compactação, o conteúdo do arquivo será replicado.

Para pré-configurar um membro do grupo de replicação, copie os arquivos para a pasta apropriada no (s) servidor (es) de destino, crie o grupo de replicação e, em seguida, escolha um membro primário. Escolha o membro que tem os arquivos mais atualizados que você deseja replicar, pois o conteúdo do membro primário é considerado "autoritativo". Isso significa que durante a replicação inicial, os arquivos do membro primário sempre substituirão outras versões dos arquivos em outros membros do grupo de replicação.

Para obter informações sobre a pré-propagação e a clonagem do banco [de dados DFSR, consulte replicação do DFS sincronização inicial no Windows Server 2012 R2: Ataque dos clones](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Para obter mais informações sobre a replicação inicial, consulte [criar um grupo de replicação](https://technet.microsoft.com/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>Replicação do DFS superar problemas comuns do serviço de replicação de arquivos?

Sim. Replicação do DFS supera três problemas comuns do FRS:

  - Quebras de diário: Replicação do DFS recupera de quebras de diário em tempo real. Cada arquivo ou pasta existente será marcada como journalWrap e verificada no sistema de arquivos antes que a replicação seja habilitada novamente. Durante a recuperação, esse volume não está disponível para replicação em nenhuma das direções.  
      
  - Replicação excessiva: Para evitar a replicação excessiva, Replicação do DFS usa um sistema de créditos.  
      
  - Pastas metamorfoseas: Para evitar nomes de pastas metamorfoseada, o replicação do DFS armazena dados conflitantes em uma\\pasta DfsrPrivate ConflictandDeleted oculta (localizada sob o caminho local da pasta replicada). Por exemplo, criar várias pastas simultaneamente com nomes idênticos em servidores diferentes replicados usando o FRS faz com que o FRS renomeie as pastas mais antigas. Replicação do DFS, em vez disso, move as pastas mais antigas para o conflito local e a pasta excluída.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>O Replicação do DFS replica arquivos em ordem cronológica?

Nº Os arquivos podem ser replicados fora de ordem.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>O Replicação do DFS replica arquivos que estão sendo usados por outro aplicativo?

Se um aplicativo abrir um arquivo e criar um bloqueio de arquivo nele (impedindo que ele seja usado por outros aplicativos enquanto estiver aberto), Replicação do DFS não replicará o arquivo até que ele seja fechado. Se o aplicativo abrir o arquivo com acesso de leitura/compartilhamento, o arquivo ainda poderá ser replicado.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>Replicação do DFS replicar permissões de arquivo NTFS, fluxos de dados alternativos, links físicos e pontos de nova análise?

  - Replicação do DFS replica as permissões de arquivo NTFS e os fluxos de dados alternativos.  
      
  - A Microsoft não dá suporte à criação de links físicos de NTFS de ou para arquivos em uma pasta replicada. isso pode causar problemas de replicação com os arquivos afetados. Os arquivos de vínculo físico são ignorados por Replicação do DFS e não são replicados. Os pontos de junção também não são replicados e Replicação do DFS registra o evento 4406 para cada ponto de junção que encontrar.  
      
  - Os únicos pontos de nova análise replicados por replicação do DFS são aqueles que usam a marca\_SYMLINK da\_marca\_de nova análise de e/s; no entanto, o replicação do DFS não garante que o destino de um SYMLINK também seja replicado. Para obter mais informações, consulte o [blog Pergunte à equipe de serviços de diretório](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - Arquivos com a eliminação\_de duplicação\_da\_marca de e\_/s,\_marca\_de reanálise de e/s\_com\_marca de nova análise do HSM ou marcas de nova análise de es\_são replicadas como arquivos normais. A marca de nova análise e os buffers de dados de nova análise não são replicados para outros servidores porque o ponto de nova análise funciona apenas no sistema local. Dessa forma, Replicação do DFS pode replicar pastas em volumes que usam eliminação de duplicação de dados no Windows Server 2012, ou SIS (armazenamento de instância única), no entanto, as informações de eliminação de duplicação de dados são mantidas separadamente por cada servidor no qual o serviço de função está habilitado.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>Replicação do DFS replicar alterações de carimbo de data/hora se nenhuma outra alteração for feita no arquivo?

Não, Replicação do DFS não replica os arquivos para os quais a única alteração é uma alteração no carimbo de data/hora. Além disso, o carimbo de data/hora alterado não é replicado para outros membros do grupo de replicação, a menos que outras alterações sejam feitas no arquivo.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>Replicação do DFS replicar permissões atualizadas em um arquivo ou pasta?

Sim. Replicação do DFS replica as alterações de permissão para arquivos e pastas. Somente a parte do arquivo associada à ACL (lista de controle de acesso) é replicada, embora Replicação do DFS ainda deve ler o arquivo inteiro na área de preparo.


> [!NOTE]
> Alterar ACLs em um grande número de arquivos pode afetar o desempenho da replicação. No entanto, ao usar a RDC, a quantidade de dados transferidos é proporcionalmente ao tamanho das ACLs, não ao tamanho do arquivo inteiro. A quantidade de tráfego de disco ainda é proporcional ao tamanho dos arquivos, pois os arquivos devem ser lidos de e para a pasta de preparo. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>O Replicação do DFS dá suporte à mesclagem de arquivos de texto em caso de conflito?

Replicação do DFS não mescla arquivos quando há um conflito. No entanto, ele tenta preservar a versão mais antiga do arquivo na pasta DfsrPrivate\\ConflictandDeleted oculta no computador em que o conflito foi detectado.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>Replicação do DFS usar criptografia ao transmitir dados?

Sim. Replicação do DFS usa conexões de RPC (chamada de procedimento remoto) com criptografia.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>É possível desabilitar o uso de RPC criptografado?

Nº O serviço de Replicação do DFS usa RPC (chamadas de procedimento remoto) sobre TCP para replicar dados. Para proteger transferências de dados pela Internet, o serviço de Replicação do DFS foi projetado para sempre usar a constante de nível de `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`autenticação,. Isso garante que a comunicação RPC pela Internet seja sempre criptografada. Portanto, não é possível desabilitar o uso de RPC criptografado pelo serviço de Replicação do DFS.

Para obter mais informações, consulte os seguintes sites da Microsoft:

  - [Referência técnica de RPC](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [Sobre a compactação diferencial remota](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Constantes de nível de autenticação](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>Como as replicações simultâneas são tratadas?

Há um Gerenciador de atualizações por pasta replicada. Os gerenciadores de atualização funcionam de forma independente um do outro.

Por padrão, um máximo de 16 (quatro no Windows Server 2003 R2) downloads simultâneos são compartilhados entre todas as conexões e grupos de replicação. Como as atualizações de conexões e grupos de replicação não são serializadas, não há nenhuma ordem específica na qual as atualizações são recebidas. Se dois agendamentos forem abertos, as atualizações serão geralmente recebidas e instaladas de ambas as conexões ao mesmo tempo.

### <a name="how-do-i-force-replication-or-polling"></a>Como fazer forçar a replicação ou sondagem?

Você pode forçar a replicação imediatamente usando o gerenciamento de DFS, conforme descrito em [Editar agendas de replicação](https://technet.microsoft.com/library/Cc732278). Você também pode forçar a replicação usando o `Sync-DfsReplicationGroup` cmdlet, incluído no módulo do PowerShell do DFSR, introduzido com o Windows Server 2012 R2, ou o comando **Dfsrdiag SyncNow** . Você pode forçar a sondagem usando `Update-DfsrConfigurationFromAD` o cmdlet ou o comando **Dfsrdiag PollAD** .

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>É possível configurar um tempo de silêncio entre as replicações para arquivos que mudam com frequência?

Nº Se a agenda estiver aberta, Replicação do DFS replicará as alterações conforme elas as observar. Não é possível configurar um tempo de silêncio para arquivos.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>É possível configurar uma replicação unidirecional com o Replicação do DFS?

Sim. Se você estiver usando o Windows Server 2012 ou o Windows Server 2008 R2, poderá criar uma pasta replicada somente leitura que Replica o conteúdo por meio de uma conexão unidirecional. Para obter mais informações, consulte [tornar uma pasta replicada somente leitura em um membro específico](http://go.microsoft.com/fwlink/?linkid=156740) (http://go.microsoft.com/fwlink/?LinkId=156740).

Não há suporte para a criação de uma conexão de replicação unidirecional com Replicação do DFS no Windows Server 2008 ou no Windows Server 2003 R2. Isso pode causar vários problemas, incluindo erros de topologia de verificação de integridade, problemas de preparo e problemas com o banco de dados Replicação do DFS.

Se você estiver usando o Windows Server 2008 ou o Windows Server 2003 R2, poderá simular uma conexão unidirecional executando as seguintes ações:

  - Treine os administradores para fazer alterações apenas no (s) servidor (es) que você deseja designar como servidores primários. Em seguida, permita que as alterações sejam replicadas nos servidores de destino.  
      
  - Configure as permissões de compartilhamento nos servidores de destino para que os usuários finais não tenham permissões de gravação. Se nenhuma alteração for permitida nos servidores de ramificação, não haverá nada para replicar de volta, simulando uma conexão unidirecional e mantendo a baixa utilização da WAN.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>Há uma maneira de forçar uma replicação completa de todos os arquivos, incluindo arquivos inalterados?

Nº Se Replicação do DFS considerar os arquivos idênticos, eles não serão replicados. Se os arquivos alterados não tiverem sido replicados, Replicação do DFS os replicará automaticamente quando configurados para fazer isso. Para substituir o agendamento configurado, use o método WMI **ForceReplicate ()** . No entanto, essa é apenas uma substituição de agendamento e não força a replicação de arquivos idênticos ou inalterados.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>O que acontece se o membro primário sofre uma perda de banco de dados durante a replicação inicial?

Durante a replicação inicial, os arquivos do membro primário sempre terão precedência na resolução de conflitos que ocorrerá se os membros de recebimento tiverem versões diferentes de arquivos no membro primário. A designação de membro primário é armazenada em Active Directory Domain Services e a designação é desmarcada depois que o membro primário está pronto para ser replicado, mas antes que todos os membros do grupo de replicação sejam replicados.

Se a replicação inicial falhar ou o serviço de Replicação do DFS for reiniciado durante a replicação, o membro primário verá a designação de membro primário no banco de dados de Replicação do DFS local e tentará novamente a replicação inicial. Se o banco de dados de Replicação do DFS do membro primário for perdido depois de limpar a designação primária no Active Directory Domain Services, mas antes que todos os membros do grupo de replicação concluam a replicação inicial, todos os membros do grupo de replicação falharão em replique a pasta porque nenhum servidor foi designado como o membro primário. Se isso acontecer, use o comando **Dfsradmin Membership/Set/IsPrimary: true** no servidor membro primário para restaurar manualmente a designação de membro primário.

Para obter mais informações sobre a replicação inicial, consulte [criar um grupo de replicação](https://technet.microsoft.com/library/cc725893).


> [!WARNING]
> A designação de membro primário é usada somente durante o processo de replicação inicial. Se você usar o comando <STRONG>Dfsradmin</STRONG> para especificar um membro primário para uma pasta replicada após a conclusão da replicação, replicação do DFS não designará o servidor como um membro primário no Active Directory Domain Services. No entanto, se o banco de dados de Replicação do DFS no servidor sofrer subseqüentemente danos irreversíveis ou perda, o servidor tentará executar uma replicação inicial como o membro primário em vez de recuperar seus dados de outro membro da replicação Group. Essencialmente, o servidor se torna um servidor primário não autorizado, o que pode causar conflitos. Por esse motivo, especifique o membro primário manualmente somente se você tiver certeza de que a replicação inicial tem Falha irrecuperável. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>O que acontecerá se a agenda de replicação for fechada enquanto um arquivo estiver sendo replicado?

Se a RDC (Compactação Diferencial Remota) estiver habilitada na conexão, a replicação de entrada de um arquivo maior que 64 KB que começou a ser replicado imediatamente antes do fechamento da agenda (ou alteração para **nenhuma largura de banda**) continuará quando a agenda for aberta (ou alterações em algo diferente de **nenhuma largura de banda**). A replicação continua no estado em que estava quando a replicação foi interrompida.

Se a RDC estiver desativada, Replicação do DFS reinicia completamente a transferência de arquivo. Isso pode atrasar quando o arquivo está disponível no membro de recebimento.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>O que acontece quando dois usuários atualizam simultaneamente o mesmo arquivo em servidores diferentes?

Quando Replicação do DFS detecta um conflito, ele usa a versão do arquivo que foi salvo por último. Ele move o outro arquivo para a pasta\\DfsrPrivate ConflictandDeleted (sob o caminho local da pasta replicada no computador que resolveu o conflito). Ele permanece lá até a limpeza de pasta de conflito e excluída, que ocorre quando a pasta de conflito e excluída excede o tamanho configurado ou Replicação do DFS encontra um erro de espaço em disco insuficiente. A pasta de conflito e excluída não é replicada, e esse método de resolução de conflitos evita o problema de diretórios metamorfoseos que eram possíveis no FRS.

Quando ocorre um conflito, o Replicação do DFS registra um evento informativo no log de eventos do Replicação do DFS. Esse evento não requer ação do usuário pelos seguintes motivos:

  - Ele não é visível para os usuários (ele fica visível somente para administradores de servidor).  
      
  - Replicação do DFS trata o conflito e a pasta excluída como um cache. Quando um limite de cota é atingido, ele limpa alguns desses arquivos. Não há nenhuma garantia de que os arquivos conflitantes serão salvos.  
      
  - O conflito pode residir em um servidor diferente da origem do conflito.  
      

## <a name="staging"></a>Preparo

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>Replicação do DFS continuar a preparação de arquivos quando a replicação for desabilitada por uma cota de limitação de largura de banda ou agendamento ou quando uma conexão for desabilitada manualmente?

Nº Replicação do DFS não continuará a preparar arquivos fora dos tempos de replicação agendados, se a cota de limitação de largura de banda tiver sido excedida ou quando as conexões estiverem desabilitadas.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>O Replicação do DFS impede que outros aplicativos acessem um arquivo durante o preparo?

Nº Replicação do DFS abre arquivos de uma maneira que não impede que os usuários ou aplicativos abram arquivos na pasta de replicação. Esse método é conhecido como "bloqueio oportunista".

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>É possível alterar o local da pasta de preparo com a ferramenta de Gerenciamento DFS?

Sim. O local da pasta de preparo é configurado na guia **avançado** da caixa de diálogo **Propriedades** para cada membro de um grupo de replicação.

### <a name="when-are-files-staged"></a>Quando os arquivos são preparados?

Os arquivos são preparados no membro de envio quando o membro de recebimento solicita o arquivo (a menos que o arquivo seja 64 KB ou menor), conforme mostrado na tabela a seguir. Se a RDC (Compactação Diferencial Remota) estiver desabilitada na conexão, o arquivo será preparado, a menos que seja 256 KB ou menor. Os arquivos também são preparados no membro receptor à medida que são transferidos se tiverem menos de 64 KB de tamanho, embora você possa definir essa configuração entre 16 KB e 1 MB. Se a agenda for fechada, os arquivos não serão preparados.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Os tamanhos mínimos de arquivo para arquivos de preparação

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

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>O que acontece se um arquivo for alterado depois de ser preparado, mas antes de ser completamente transmitido para o site remoto?

Se alguma parte do arquivo já estiver sendo transmitida, Replicação do DFS continuará a transmissão. Se o arquivo for alterado antes que Replicação do DFS comece a transmitir o arquivo, a versão mais recente do arquivo será enviada.

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
<td><p>Atualizado para o Windows Server 2019.</p></td>
<td><p>Novo sistema operacional.</p></td>
</tr>
<tr class="even">
<td><p>9 de outubro de 2013</p></td>
<td><p>Atualizado o que são os limites com suporte de Replicação do DFS? seção com resultados de testes no Windows Server 2012 R2.</p></td>
<td><p>Atualizações para a versão mais recente do Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>30 de janeiro de 2013</p></td>
<td><p>Adicionada a Replicação do DFS continuar a preparação de arquivos quando a replicação for desabilitada por uma cota de limitação de largura de banda ou agendamento ou quando uma conexão for desabilitada manualmente? inicial.</p></td>
<td><p>Perguntas do cliente</p></td>
</tr>
<tr class="even">
<td><p>31 de outubro de 2012</p></td>
<td><p>Editou quais são os limites com suporte de Replicação do DFS? entrada para aumentar o número testado de arquivos replicados em um volume.</p></td>
<td><p>Comentários do cliente</p></td>
</tr>
<tr class="odd">
<td><p>15 de agosto de 2012</p></td>
<td><p>Editou o Replicação do DFS replicar permissões de arquivo NTFS, fluxos de dados alternativos, links físicos e pontos de nova análise? a entrada para esclarecer ainda mais como o Replicação do DFS lida com links físicos e pontos de nova análise.</p></td>
<td><p>Comentários dos serviços de atendimento ao cliente</p></td>
</tr>
<tr class="even">
<td><p>13 de junho de 2012</p></td>
<td><p>Editou o Replicação do DFS funciona em volumes ReFS ou FAT? entrada para adicionar a discussão de ReFS.</p></td>
<td><p>Comentários do cliente</p></td>
</tr>
<tr class="odd">
<td><p>25 de abril de 2012</p></td>
<td><p>Editou o Replicação do DFS replicar permissões de arquivo NTFS, fluxos de dados alternativos, links físicos e pontos de nova análise? entrada para esclarecer como Replicação do DFS lida com links físicos.</p></td>
<td><p>Reduzir a confusão em potencial</p></td>
</tr>
<tr class="even">
<td><p>30 de março de 2011</p></td>
<td><p>Editado o Replicação do DFS pode replicar os arquivos de banco de dados Outlook. pst ou Microsoft Office Access? a entrada para corrigir o impacto potencial do uso de Replicação do DFS com arquivos. pst e Access.</p>
<p>Adicionado como posso melhorar o desempenho da replicação?</p></td>
<td><p>Perguntas do cliente sobre a entrada anterior, que indicaram incorretamente que a replicação de arquivos. pst ou do Access pode corromper o banco de dados Replicação do DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 de janeiro de 2011</p></td>
<td><p>Adicionado como os arquivos podem ser recuperados das pastas ConflictAndDeleted ou preexistentes?</p></td>
<td><p>Comentários do cliente</p></td>
</tr>
<tr class="even">
<td><p>20 de outubro de 2010</p></td>
<td><p>Adicionado como posso atualizar ou substituir um membro Replicação do DFS?</p></td>
<td><p>Comentários do cliente</p></td>
</tr>
</tbody>
</table>

