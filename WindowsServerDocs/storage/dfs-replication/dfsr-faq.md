---
title: Replicação do DFS – Perguntas Frequentes
ms.date: 06/18/2014
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 1e11f6c596d7e5eb0bdf379adcf47d21e74e9f6b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80815619"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Replicação do DFS: Perguntas frequentes (FAQ)


Atualizado: 30 de abril de 2019

Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Estas perguntas frequentes respondem perguntas sobre a Replicação de DFS (Sistema de Arquivos Distribuído) (também conhecida como DFS-R ou DFSR) para o Windows Server.

Para obter mais informações sobre Namespaces DFS, consulte [Namespaces DFS: Perguntas frequentes](https://technet.microsoft.com/library/ee404780).

Para obter informações sobre as novidades da Replicação do DFS, confira os seguintes tópicos:

  - [Visão geral de Namespaces do DFS e Replicação do DFS](https://technet.microsoft.com/library/jj127250) (no Windows Server 2012)  
      
  - Tópico [Novidades no Sistema de Arquivos Distribuído](https://technet.microsoft.com/library/ee307957) em [Alterações na funcionalidade do Windows Server 2008 para o Windows Server 2008 R2](https://technet.microsoft.com/library/dd391932)  
      
  - Tópico [Sistema de Arquivos Distribuído](https://technet.microsoft.com/library/cc753479) em [Alterações na funcionalidade do Windows Server 2003 com SP1 para o Windows Server 2008](https://technet.microsoft.com/library/cc753208)  
      

Para obter uma lista das alterações recentes deste tópico, consulte a seção [Histórico de Alterações](#change-history) deste tópico.

      

## <a name="interoperability"></a>Interoperabilidade

### <a name="can-dfs-replication-communicate-with-frs"></a>A Replicação do DFS pode se comunicar com o FRS?

Não. A Replicação do DFS não se comunica com o FRS (Serviço de Replicação de Arquivo). A Replicação do DFS e o FRS podem ser executados no mesmo servidor ao mesmo tempo, mas nunca devem ser configurados para replicar as mesmas pastas ou subpastas, pois isso pode causar perda de dados.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>A Replicação do DFS pode substituir o FRS para replicação do SYSVOL?

Sim, a Replicação do DFS pode substituir o FRS para replicação SYSVOL em servidores que executam o Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2008 R2 ou o Windows Server 2008. Servidores que executam o Windows Server 2003 R2 não são compatíveis com o uso de Replicação do DFS para replicar a pasta SYSVOL.

Para obter mais informações sobre como replicar o SYSVOL usando a Replicação do DFS, confira o [Guia de migração de Replicação do SYSVOL: FRS para Replicação do DFS](https://technet.microsoft.com/library/dd640019).

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>Posso atualizar de FRS para Replicação do DFS sem perder as definições de configuração?

Sim. Para migrar de FRS para a Replicação do DFS, confira os seguintes documentos:

  - Para migrar a replicação de pastas que não seja a pasta SYSVOL, confira [Guia de operações do DFS: Migrar do FRS para a Replicação do DFS](https://go.microsoft.com/fwlink/?linkid=192776) e [FRS2DFSR – Um utilitário de migração de FRS para DFSR](https://go.microsoft.com/fwlink/?linkid=195437) (https://go.microsoft.com/fwlink/?LinkID=195437).  
      
  - Para migrar a replicação da pasta SYSVOL para Replicação do DFS, configura o [Guia de migração da Replicação do SYSVOL: FRS para Replicação do DFS](https://technet.microsoft.com/library/dd640019).  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>posso usar a Replicação do DFS em um ambiente misto Windows/UNIX?

Sim. Embora a Replicação do DFS seja compatível apenas com a replicação de conteúdo entre servidores que executam o Windows Server, os clientes UNIX podem acessar compartilhamentos de arquivos nos servidores Windows. Para fazer isso, instale os serviços para NFS (sistemas de arquivos de rede) no servidor de Replicação do DFS.

Você também pode usar a funcionalidade de cliente SMB/CIFS incluída em muitos clientes UNIX para acessar diretamente os compartilhamentos de arquivos do Windows, embora essa funcionalidade geralmente seja limitada ou exija modificações no ambiente do Windows (como desabilitar a assinatura SMB usando a Política de Grupo).

A Replicação do DFS interopera com NFS em um servidor que executa um sistema operacional Windows Server, mas você não pode replicar um ponto de montagem NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>Posso usar o Serviço de Cópias de Sombra de Volume com Replicação do DFS?

Sim. A Replicação do DFS é compatível com volumes VSS (Serviço de Cópias de Sombra de Volume) e os instantâneos anteriores podem ser restaurados com êxito com o Cliente de Versões Anteriores.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>Posso usar o backup do Windows (Ntbackup.exe) para fazer backup de uma pasta replicada remotamente?

Não, não há suporte para usar o backup do Windows (Ntbackup.exe) em um computador que executa o Windows Server 2003 ou anterior para fazer backup do conteúdo de uma pasta replicada em um computador que executa o Windows Server 2012, o Windows Server 2008 R2 ou o Windows Server 2008.

Para fazer backup de arquivos armazenados em uma pasta replicada, use o Backup do Windows Server ou o Microsoft&reg; System Center Data Protection Manager. Para obter informações sobre a funcionalidade de Backup e Recuperação no Windows Server 2008 R2 e no Windows Server 2008, confira [Backup e Recuperação](https://technet.microsoft.com/library/Cc754097). Para obter mais informações, confira [System Center Data Protection Manager](https://go.microsoft.com/fwlink/?linkid=182261) (https://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>As políticas do sistema de arquivos afetam a Replicação do DFS?

Sim. Não configure políticas do sistema de arquivos em pastas replicadas. A política do sistema de arquivos reaplica as permissões NTFS a cada intervalo de atualização de Política de Grupo. Isso pode resultar em violações de compartilhamento porque um arquivo aberto não é replicado até que seja fechado.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>A Replicação do DFS replica caixas de correio hospedadas no Microsoft Exchange Server?

Não. A Replicação do DFS não pode ser usada para replicar caixas de correio hospedadas no Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>A Replicação do DFS é compatível com telas de arquivo criadas pelo Gerenciador de Recursos de Servidor de Arquivos?

Sim. No entanto, as configurações de triagem de arquivo do FSRM (Gerenciador de Recursos de Servidor de Arquivos) devem corresponder em ambas as extremidades da replicação. Além disso, a Replicação do DFS um mecanismo de filtro próprio para arquivos e pastas que você pode usar para excluir determinados arquivos e tipos de arquivos da replicação.

Veja abaixo as práticas recomendadas para implementar triagens de arquivo ou cotas:

  - A pasta DfsrPrivate oculta não deve estar sujeita a cotas nem a triagens de arquivo.  
      
  - Os arquivos que passaram por triagem não devem existir em nenhuma pasta replicada antes de a triagem ser habilitada.  
      
  - Nenhuma pasta pode exceder a cota antes de a cota ser habilitada.  
      
  - Você deve usar cotas rígidas com cuidado. É possível que membros individuais de um grupo de replicação permaneçam dentro de uma cota antes da replicação, mas a excedam quando os arquivos forem replicados. Por exemplo, se um usuário copiar um arquivo de 10 megabytes (MB) para o servidor A (que está no limite rígido) e outro usuário copiar um arquivo de 5 MB no servidor B, quando a próxima replicação ocorrer, ambos os servidores excederão a cota em 5 megabytes. Isso pode fazer com que a Replicação do DFS repita continuamente a replicação dos arquivos, causando lacunas no vetor de versão e possíveis problemas de desempenho.  
      

### <a name="is-dfs-replication-cluster-aware"></a>A Replicação do DFS reconhece o cluster?

Sim, a Replicação do DFS no Windows Server 2012 R2, no Windows Server 2012 e no Windows Server 2008 R2 inclui a capacidade de adicionar um cluster de failover como um membro de um grupo de replicação. Para obter mais informações, confira [Adicionar um cluster de failover a um grupo de replicação](https://go.microsoft.com/fwlink/?linkid=155085) (https://go.microsoft.com/fwlink/?LinkId=155085). O serviço de Replicação do DFS nas versões do Windows anteriores ao Windows Server 2008 R2 não foi desenvolvido para coordenar com um cluster de failover. O serviço não realizará o failover para outro nó.


> [!NOTE]
> A Replicação do DFS não é compatível com a replicação de arquivos em Volumes Compartilhados Clusterizados. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>A Replicação do DFS é compatível com a Eliminação de Duplicação de Dados?

Sim, a Replicação do DFS pode replicar pastas em volumes que usam Eliminação de Duplicação de Dados no Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>A Replicação do DFS é compatível com o RIS e o WDS?

Sim. A Replicação do DFS replica volumes nos quais o SIS (Armazenamento de Instância Única) está habilitado. O SIS é usado pelo RIS (Serviços de Instalação Remota), pelo WDS (Serviços de Implantação do Windows) e pelo Windows Storage Server.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>É possível usar Replicação do DFS com Arquivos Offline?

Você pode usar a Replicação do DFS e Arquivos Offline com segurança em cenários em que há apenas um usuário por vez, que grava nos arquivos. Isso é útil para os usuários que viajam entre duas filiais e desejam poder acessar os arquivos em qualquer filial ou offline. Arquivos Offline armazenam em cache os arquivos localmente para uso offline e a Replicação do DFS replica os dados entre cada filial.

Não use Replicação do DFS com Arquivos Offline em um ambiente de vários usuários porque a Replicação do DFS não fornece nenhum mecanismo de bloqueio distribuído ou funcionalidade de check-out de arquivos. Se dois usuários modificarem o mesmo arquivo ao mesmo tempo em servidores diferentes, a Replicação do DFS moverá o arquivo mais antigo para a pasta DfsrPrivate\\ConflictandDeleted (localizada no caminho local da pasta replicada) durante a próxima replicação.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>Quais aplicativos antivírus são compatíveis com a Replicação do DFS?

Os aplicativos antivírus podem causar uma replicação excessiva se as atividades de verificação deles alterarem os arquivos em uma pasta replicada. Para obter mais informações, confira [Testar a interoperabilidade de aplicativo antivírus com a Replicação do DFS](https://go.microsoft.com/fwlink/?linkid=73990) (https://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>Quais são os benefícios de usar a Replicação do DFS, em vez do Windows SharePoint Services?

O Windows&reg; SharePoint&reg; Services fornece uma coerência rígida na forma de funcionalidade de check-out do arquivo que a Replicação do DFS não fornece. Se você estiver preocupado com várias pessoas editando o mesmo arquivo, use o Windows SharePoint Services. O Windows SharePoint Services 2.0 com Service Pack 2 está disponível como parte do Windows Server 2003 R2. O Windows SharePoint Services pode ser baixado do site da Microsoft. Ele não está incluído nas versões mais recentes do Windows Server. No entanto, se você está replicando dados em vários sites e os usuários não editam os mesmos arquivos ao mesmo tempo, a Replicação do DFS oferece maior largura de banda e gerenciamento mais simples.

## <a name="limitations-and-requirements"></a>Limitações e requisitos

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>A Replicação do DFS pode replicar entre filiais sem uma conexão VPN?

Sim, supondo que haja um link de WAN (rede de longa distância) privado (não a Internet) conectando as filiais. No entanto, você deve abrir as portas adequadas em firewalls externos. A Replicação do DFS usa o mapeador de ponto de extremidade RPC (porta 135) e uma porta efêmera atribuída aleatoriamente acima de 1024. Você pode usar a ferramenta de linha de comando **Dfsrdiag** para especificar uma porta estática em vez da porta efêmera. Para obter mais informações sobre como especificar o mapeador de ponto de extremidade RPC, confira o [artigo 154596](https://go.microsoft.com/fwlink/?linkid=73991) na base de dados de conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=73991).

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>A Replicação do DFS pode replicar arquivos criptografados com o Encrypting File System?

Não. A Replicação do DFS não replicará arquivos ou pastas criptografados usando o EFS (Encrypting File System). Se um usuário criptografar um arquivo replicado anteriormente, a Replicação do DFS excluirá o arquivo de todos os outros membros do grupo de replicação. Isso garante que a única cópia disponível do arquivo seja a versão criptografada no servidor.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>A Replicação do DFS pode replicar arquivos do banco de dados do Microsoft Office Access ou .pst do Outlook?

A Replicação do DFS poderá replicar com segurança arquivos de pasta pessoal (.pst) do Microsoft Outlook e arquivos do Microsoft Access somente se eles forem armazenados para fins de arquivamento e não forem acessados pela rede usando um cliente como o Outlook ou o Access (para abrir os arquivos .pst ou do Access, primeiro copie-os para um dispositivo de armazenamento local). Os motivos para isso são os seguintes:

  - Abrir arquivos .pst em conexões de rede pode levar a dados corrompidos nos arquivos .pst. Para obter mais informações sobre por que não é possível acessar os arquivos .pst com segurança em uma rede, confira o [artigo 297019](https://go.microsoft.com/fwlink/?linkid=125363) na Base de Dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=125363).  
      
  - Arquivos .pst e do Access tendem a permanecer abertos por longos períodos enquanto são acessados por um cliente, como o Outlook ou o Office Access. Isso impede a Replicação do DFS de replicar esses arquivos até que eles sejam fechados.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>Posso usar a Replicação do DFS em um grupo de trabalho?

Não. A Replicação do DFS baseia-se no Active Directory&reg; Domain Services para configuração. Ela só funcionará em um domínio.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>É possível replicar mais de uma pasta em um servidor?

Sim. A Replicação do DFS pode replicar várias pastas entre servidores. Verifique se cada uma das pastas replicadas tem um caminho raiz exclusivo e se elas não se sobrepõem. Por exemplo, D:\\Sales e D:\\Contabilidade podem ser os caminhos raiz para duas pastas replicadas, mas D:\\Sales e D:\\Sales\\Reports não podem ser os caminhos raiz para duas pastas replicadas.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>A Replicação do DFS requer namespaces DFS?

Não. Os namespaces da Replicação do DFS e do DFS podem ser usados separadamente ou em conjunto. Além disso, a Replicação do DFS pode ser usada para replicar namespaces DFS autônomos, o que não era possível com o FRS.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>A Replicação do DFS requer sincronização de horário entre os servidores?

Não. A Replicação do DFS não requer explicitamente a sincronização de horário entre os servidores. No entanto, a Replicação do DFS requer que os relógios do servidor correspondam de maneira mais rigorosa. Os relógios do servidor devem ser definidos dentro de cinco minutos uns dos outros (por padrão) para que a autenticação do Kerberos funcione corretamente. Por exemplo, a Replicação do DFS usa carimbos de data/hora para determinar qual arquivo tem precedência no caso de um conflito. Horários precisos também são importantes para coleta de lixo, agendas e outros recursos.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>A Replicação do DFS é compatível à replicação de um volume inteiro?

Sim. No entanto, você deve primeiro instalar o Windows Server 2003 Service Pack 2 ou o hotfix. Para obter mais informações, confira o [artigo 920335](https://go.microsoft.com/fwlink/?linkid=76776) na Base de Dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=76776). Além disso, a replicação de um volume inteiro pode causar os seguintes problemas:

  - Se o volume contiver um arquivo de paginação do Windows, a replicação falhará e registrará o evento 4312 do DFSR no log de eventos do sistema.  
      
  - A Replicação do DFS define o sistema e os atributos ocultos na pasta replicada nos servidores de destino. Isso ocorre porque o Windows aplica o sistema e os atributos ocultos à pasta raiz do volume por padrão. Se o caminho local da pasta replicada nos servidores de destino for também uma raiz do volume, nenhuma alteração adicional será feita aos atributos da pasta.  
      
  - Ao replicar um volume que contém a pasta do sistema Windows, a Replicação do DFS reconhece a pasta %WINDIR% e não a replica. No entanto, a Replicação do DFS replica pastas usadas por aplicativos que não são da Microsoft, o que pode fazer com que os aplicativos falhem nos servidores de destino se os aplicativos tiverem problemas de interoperabilidade com Replicação do DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>A Replicação do DFS dá suporte a RPC sobre HTTP?

Não.

### <a name="does-dfs-replication-work-across-wireless-networks"></a>A Replicação do DFS funciona em redes sem fio?

Sim. A Replicação do DFS é independente do tipo de conexão.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>A Replicação do DFS funciona em volumes ReFS ou FAT?

Não. A Replicação do DFS é compatível com volumes formatados somente com o sistema de arquivos NTFS; o Sistema de Arquivos Resiliente (ReFS) e o sistema de arquivos FAT não são compatíveis. A Replicação do DFS requer NTFS porque usa o diário de alterações do NTFS e outros recursos do sistema de arquivos NTFS.

### <a name="does-dfs-replication-work-with-sparse-files"></a>A Replicação do DFS funciona com arquivos esparsos?

Sim. Você pode replicar arquivos esparsos. O atributo **Esparso** é preservado no membro de recebimento.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>Preciso fazer logon como administrador para replicar arquivos?

Não. A Replicação do DFS é um serviço executado na conta sistema local, portanto, não é necessário fazer logon como administrador para replicar. No entanto, você deve ser um administrador de domínio ou administrador local dos servidores de arquivos afetados para fazer alterações na configuração de Replicação do DFS.

Para obter mais informações, confira "Delegação e requisitos de segurança da Replicação do DFS" em [Delegar a capacidade de gerenciar a Replicação do DFS](https://go.microsoft.com/fwlink/?linkid=182294) (https://go.microsoft.com/fwlink/?LinkId=182294).

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>Como posso atualizar ou substituir um membro da Replicação do DFS?

Para atualizar ou substituir um membro da Replicação do DFS, confira esta postagem no blog Ask the Directory Services Team: [Como substituir o hardware ou o SO do membro da DFSR](https://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>A Replicação do DFS é adequado para replicar perfis móveis?

Sim. Há suporte para determinados cenários ao replicar perfis de usuários móveis. Para obter informações sobre os cenários compatíveis, confira [Declaração de suporte da Microsoft sobre de dados de perfil do usuário replicados](https://go.microsoft.com/fwlink/?linkid=201282) (https://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>Há um limite de caracteres de arquivo ou um limite à profundidade da pasta?

O Windows e a Replicação do DFS são compatíveis com caminhos de pasta de até 32 mil caracteres. A Replicação do DFS não está limitada a caminhos de pasta de 260 caracteres.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>Os membros de um grupo de replicação devem residir no mesmo domínio?

Não. Os grupos de replicação podem se estender entre domínios dentro de uma floresta, mas não entre florestas diferentes.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>Quais são os limites compatíveis com a Replicação do DFS?

A lista a seguir apresenta um conjunto de diretrizes de escalabilidade que foram testadas pela Microsoft e aplicadas ao Windows Server 2012 R2, ao Windows Server 2016 e ao Windows Server 2019

  - Tamanho de todos os arquivos replicados em um servidor: 100 terabytes.  
      
  - Número de arquivos replicados em um volume: 70 milhões.  
      
  - Tamanho máximo do arquivo: 250 gigabytes.  
      


> [!IMPORTANT]
> Ao criar grupos de replicação com um grande número ou tamanho de arquivos, é recomendável exportar um clone de banco de dados e usar técnicas de pré-propagação para minimizar a duração da replicação inicial. Para obter mais informações, confira [Sincronização inicial da Replicação do DFS no Windows Server 2012 R2: o ataque dos clones](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/DFS-Replication-Initial-Sync-in-Windows-Server-2012-R2-Attack-of/ba-p/424877). 
<br>


A seguinte lista apresenta um conjunto de diretrizes de escalabilidade que foram testadas pela Microsoft no Windows Server 2012, no Windows Server 2008 R2 e no Windows Server 2008:

  - Tamanho de todos os arquivos replicados em um servidor: 10 terabytes.  
      
  - Número de arquivos replicados em um volume: 11 milhões.  
      
  - Tamanho máximo do arquivo: 64 gigabytes.  
      


> [!NOTE]
> Não há mais um limite ao número de grupos de replicação, pastas replicadas, conexões ou membros do grupo de replicação. 
<br>


Para obter uma lista das diretrizes de escalabilidade que foram testadas pela Microsoft para o Windows Server 2003 R2, confira [Diretrizes de escalabilidade da Replicação do DFS](https://go.microsoft.com/fwlink/?linkid=75043) (https://go.microsoft.com/fwlink/?LinkId=75043).

### <a name="when-should-i-not-use-dfs-replication"></a>Quando não devo usar a Replicação do DFS?

Não use a Replicação do DFS em um ambiente em que vários usuários atualizem ou modifiquem os mesmos arquivos simultaneamente em servidores diferentes. Isso pode fazer com que a Replicação do DFS mova cópias conflitantes dos arquivos para a pasta DfsrPrivate\\ConflictandDeleted oculta.

Quando vários usuários precisam modificar os mesmos arquivos ao mesmo tempo em servidores diferentes, use o recurso de check-out do arquivo do Windows SharePoint Services para garantir que apenas um usuário esteja trabalhando em um arquivo. O Windows SharePoint Services 2.0 com Service Pack 2 está disponível como parte do Windows Server 2003 R2. O Windows SharePoint Services pode ser baixado do site da Microsoft. Ele não está incluído nas versões mais recentes do Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>Por que uma atualização de esquema é necessária para Replicação do DFS?

A Replicação do DFS usa novos objetos no contexto de nomeação de domínio de Active Directory Domain Services para armazenar informações de configuração. Esses objetos são criados quando você atualiza o esquema do Active Directory Domain Services. Para obter mais informações, confira [Analisar os requisitos para Replicação do DFS](https://go.microsoft.com/fwlink/?linkid=182264) (https://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Ferramentas de monitoramento e gerenciamento

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>Posso automatizar o relatório de integridade para receber avisos?

Sim. Há três maneiras de automatizar os relatórios de integridade:

  - Usar o módulo DFSR do Windows PowerShell incluído no Windows Server 2012 R2 ou DfsrAdmin.exe em conjunto com as tarefas agendadas para gerar relatórios de integridade regularmente. Para obter mais informações, confira [Como automatizar os relatórios de integridade da Replicação do DFS](https://go.microsoft.com/fwlink/?linkid=74010) (https://go.microsoft.com/fwlink/?LinkId=74010).  
      
  - Usar o pacote de gerenciamento Replicação do DFS para System Center Operations Manager para criar alertas com base em condições especificadas.  
      
  - Use o provedor WMI da Replicação do DFS para o script de alertas.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>Posso usar o Microsoft System Center Operations Manager para monitorar Replicação do DFS?

Sim. Para obter mais informações, confira [Pacote de Gerenciamento da Replicação do DFS para System Center Operations Manager 2007](https://go.microsoft.com/fwlink/?linkid=182265) no Centro de Download da Microsoft (https://go.microsoft.com/fwlink/?LinkId=182265).

### <a name="does-dfs-replication-support-remote-management"></a>A Replicação do DFS é compatível com gerenciamento remoto?

Sim. A Replicação do DFS é compatível com gerenciamento remoto usando o console de Gerenciamento do DFS e o comando **Add Replication Group**. Por exemplo, no servidor A, você pode se conectar a um grupo de replicação definido na floresta com os servidores A e B como membros.

O Gerenciamento de DFS está incluído no Windows Server 2012 R2, no Windows Server 2012, no Windows Server 2008 R2, no Windows Server 2008 e no Windows Server 2003 R2. Para gerenciar a Replicação do DFS de outras versões do Windows, use a Área de Trabalho Remota ou as [Ferramentas de Administração de Servidor Remoto para o Windows 7](https://technet.microsoft.com/library/Ee449475).


> [!IMPORTANT]
> Para ver ou gerenciar grupos de replicação que contêm pastas replicadas somente leitura ou membros que são clusters de failover, use a versão do Gerenciamento do DFS incluída no Windows Server 2012 R2, no Windows Server 2012, no Windows Server 2008 R2, em <a href="https://go.microsoft.com/fwlink/p/?linkid=238560">Ferramentas de Administração de Servidor Remoto para Windows 8</a> ou em <a href="https://technet.microsoft.com/library/ee449475">Ferramentas de Administração de Servidor Remoto para Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>O Ultrasound e o Sonar funcionam com a Replicação do DFS?

Não. A Replicação do DFS tem o próprio conjunto de ferramentas de monitoramento e diagnóstico. O Ultrasound e o Sonar podem apenas monitorar o FRS.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>Como os arquivos podem ser recuperados das pastas ConflictAndDeleted ou PreExisting?

Para recuperar arquivos perdidos, restaure os arquivos da pasta do sistema de arquivos ou da pasta compartilhada usando o histórico de arquivos, o comando **Restore previous versions** no Explorador de Arquivos ou restaurando os arquivos do backup. Para recuperar arquivos diretamente das pastas ConflictAndDeleted ou PreExisting, use os cmdlets `Get-DfsrPreservedFiles` e `Restore-DfsrPreservedFiles` do Windows PowerShell (incluído com o módulo DFSR no Windows Server 2012 R2) ou o script de exemplo [RestoreDFSR](https://code.msdn.microsoft.com/restoredfsr) da Galeria de Códigos do MSDN. Esse script destina-se apenas à recuperação de desastres e é fornecido no estado em que se encontra, sem garantia.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>Há como saber o estado da replicação?

Sim. Há várias maneiras de monitorar a replicação:

  - A Replicação do DFS tem um pacote de gerenciamento para o System Center Operations Manager que fornece monitoramento proativo.  
      
  - O Gerenciamento do DFS tem um relatório de diagnóstico nativo para a lista de pendências de replicação, eficiência da replicação e o número de arquivos e pastas em um determinado grupo de replicação.  
      
  - O módulo DFSR do Windows PowerShell no Windows Server 2012 R2 contém cmdlets para iniciar testes de propagação e gravar relatórios de propagação e de integridade. Para obter mais informações, confira [Cmdlets de replicação do Sistema de Arquivos Distribuído no Windows PowerShell](https://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag.exe é uma ferramenta de linha de comando que pode gerar uma contagem da lista de pendências ou disparar um teste de propagação. Ambos mostram o estado da replicação. A propagação mostra se os arquivos estão sendo replicados em todos os nós. A lista de pendências mostra quantos arquivos ainda precisam ser replicados para que os dois computadores estejam em sincronia. A contagem da lista de pendências é o número de atualizações que um membro do grupo de replicação não processou. Em computadores que executam o Windows Server 2012 R2, o Windows Server 2012 ou o Windows Server 2008 R2, o Dfsrdiag.exe também pode exibir as atualizações que a Replicação do DFS está replicando no momento.  
      
  - Os scripts podem usar o WMI para coletar informações da lista de pendências, seja manualmente ou por meio do MOM.  
      

## <a name="performance"></a>Desempenho

### <a name="does-dfs-replication-support-dial-up-connections"></a>A Replicação do DFS oferece suporte a conexões discadas?

Embora a Replicação do DFS funcione em velocidades de conexão discada, ela poderá ser colocada na lista de pendências se houver grandes números de alterações a serem replicadas. Se forem feitas pequenas alterações aos arquivos existentes, a Replicação do DFS com a RDC (Compactação Diferencial Remota) fornecerá um desempenho muito maior do que copiar o arquivo diretamente.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>A Replicação do DFS executa a detecção de largura de banda?

Não. A Replicação do DFS não executa a detecção de largura de banda. Você pode configurar a Replicação do DFS para usar uma quantidade limitada de largura de banda conforme a conexão (limitação de largura de banda). No entanto, a Replicação do DFS não reduzirá ainda mais a utilização da largura de banda se o adaptador de rede ficar saturado e a Replicação do DFS poderá saturar o link por períodos curtos. A limitação da largura de banda com a Replicação do DFS não é completamente precisa porque a Replicação do DFS limita a largura de banda por meio da limitação de chamadas RPC. Como resultado, vários buffers em níveis inferiores da pilha de rede (incluindo RPC) podem interferir, causando intermitências de tráfego de rede.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>A Replicação do DFS limita a largura de banda por agendamento, por servidor ou por conexão?

Se você configurar a limitação da largura de banda ao especificar o agendamento, todas as conexões desse grupo de replicação usarão essa configuração para limitar a largura de banda. A limitação da largura de banda também pode ser definida como uma configuração no nível da conexão usando o Gerenciamento do DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>A Replicação do DFS usa o Active Directory Domain Services para calcular os links de site e os custos de conexão?

Não. A Replicação do DFS usa a topologia definida pelo administrador, que é independente do custo do site do Active Directory Domain Services.

### <a name="how-can-i-improve-replication-performance"></a>Como posso aprimorar o desempenho da replicação?

Para saber mais sobre os diferentes métodos de ajuste do desempenho de replicação, confira [Como ajustar o desempenho de replicação no DFSR](https://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) no [blog Ask the Directory Services Team](https://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>Como a Replicação do DFS evita saturar uma conexão?

Na Replicação do DFS, você define a largura de banda máxima que deseja usar em uma conexão e o serviço mantém esse nível de uso de rede. Isso é diferente do BITS (Serviço de Transferência Inteligente em Segundo Plano), e a Replicação do DFS não saturará a conexão se você a definir adequadamente.

No entanto, a limitação da largura de banda não é de 100% precisa e a Replicação do DFS pode saturar o link por curtos períodos de tempo. Isso ocorre porque a Replicação do DFS limita a largura de banda ao limitar as chamadas RPC. Como esse processo depende de vários buffers em níveis inferiores da pilha de rede, incluindo RPC, o tráfego de replicação tende a viajar em intermitências que podem, às vezes, saturar os links de rede.

A Replicação do DFS no Windows Server 2008 inclui vários aprimoramentos de desempenho, conforme discutido em [Sistema de Arquivos Distribuído](https://technet.microsoft.com/library/Cc753479), um tópico em [Alterações de funcionalidade do Windows Server 2003 com SP1 para o Windows Server 2008](https://technet.microsoft.com/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>Como a Replicação do DFS o desempenho se comparam ao FRS?

A Replicação do DFS é muito mais rápida do que o FRS, especialmente quando pequenas alterações são feitas em arquivos grandes e a RDC está habilitada. Por exemplo, com a Conexão de Área de Trabalho Remota, uma pequena alteração em uma apresentação do PowerPoint&reg; de 2 MB pode resultar no envio de apenas 60 KB (quilobytes) pela rede, uma economia de 97% em bytes transferidos.

A RDC não é usada em arquivos menores que 64 KB e pode não ser positiva para LANs de alta velocidade em que a largura de banda da rede não é problema. A RDC pode ser desabilitada por conexão usando o Gerenciamento do DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>Com que frequência a Replicação do DFS replica dados?

Os dados são replicados de acordo com o agendamento que você define. Por exemplo, você pode definir a agenda para intervalos de 15 minutos, sete dias por semana. Durante esses intervalos, a replicação é habilitada. A replicação inicia logo após a detecção de uma alteração de arquivo (geralmente em segundos).

O agendamento do grupo de replicação pode ser definido como UTC (horário local coordenado) enquanto a agenda de conexão é definida como a hora local do membro de recebimento. Leve isso em conta quando o grupo de replicação abranger vários fusos horários. Horário local significa o horário do membro que hospeda a conexão de entrada. O agendamento exibido da conexão de entrada e da conexão de saída correspondente reflete as diferenças de fuso horário quando a agenda é definida como horário local.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>Quanto dos recursos do sistema de meu servidor a Replicação do DFS consumirá?

O disco, a memória e os recursos de CPU usados pela Replicação do DFS dependem de vários fatores, incluindo o número e o tamanho dos arquivos, a taxa de alteração, o número de membros do grupo de replicação e o número de pastas replicadas. Além disso, alguns recursos são mais difíceis de estimar. Por exemplo, a tecnologia ESE (Mecanismo de Armazenamento Extensível) usada para o banco de dados da Replicação do DFS pode consumir um grande percentual de memória disponível, que é liberada sob demanda. Aplicativos que não sejam da Replicação do DFS podem ser hospedados no mesmo servidor, dependendo da configuração do servidor. No entanto, ao hospedar vários aplicativos ou funções de servidor em um servidor, é importante testar essa configuração antes de implementá-la em um ambiente de produção.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>O que acontecerá se um link de WAN falhar durante a replicação?

Se a conexão for desativada, a Replicação do DFS continuará tentando replicar enquanto o agendamento estiver aberto. Também serão indicados erros de conectividade no log de eventos da Replicação do DFS que podem ser coletados usando o MOM (proativamente por meio de alertas) e o relatório de integridade Replicação do DFS (reativamente, como quando um administrador o executa).

## <a name="remote-differential-compression-details"></a>Detalhas da Compactação Diferencial Remota

### <a name="are-changes-compressed-before-being-replicated"></a>As alterações são compactadas antes de serem replicadas?

Sim. Partes alteradas de arquivos são compactadas antes de serem enviadas para todos os tipos de arquivo, exceto os seguintes (que já estão compactados): .wma, .wmv, .zip, .jpg, .mpg, .mpeg, .m1v, .mp2, .mp3, .mpa, .cab, .wav, .snd, .au, .asf, .wm, .avi, .z, .gz, .tgz e .frx. As configurações de compactação para esses tipos de arquivo não são configuráveis no Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>Um administrador pode desligar a RDC ou alterar o limite?

Sim. Você pode desligar a RDC por meio da página de propriedades de uma determinada conexão. A desabilitação da RDC pode reduzir a utilização da CPU e a latência de replicação em links de LAN (rede local) rápidos que não têm nenhuma restrição de largura de banda ou para grupos de replicação que consistem principalmente em arquivos menores que 64 KB. Se você optar por desabilitar a RDC em uma conexão, teste a eficiência da replicação antes e depois da alteração para verificar se você aprimorou o desempenho da replicação.

Você pode alterar o limite de tamanho de RDC usando o comando **Dfsradmin Connection Set**, o provedor WMI Replicação do DFS ou editando manualmente o arquivo XML de configuração.

### <a name="does-rdc-work-on-all-file-types"></a>A RDC funciona em todos os tipos de arquivos?

Sim. A RDC calcula diferenças no nível de bloco, independentemente do tipo de dados de arquivo. No entanto, a RDC funciona com mais eficiência em determinados tipos de arquivo, como documentos do Word, arquivos PST e imagens VHD.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>Como funciona a RDC em um arquivo compactado?

A Replicação do DFS usa RDC, que computa os blocos no arquivo alterados e envia apenas esses blocos pela rede. A Replicação do DFS não precisa saber nada sobre o conteúdo do arquivo, apenas quais blocos mudaram.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>A RDC entre arquivos está habilitada ao atualizar para o Windows Server Enterprise Edition ou Datacenter Edition?

As edições Standard do Windows Server não são compatíveis com a RDC entre arquivos. No entanto, ela é habilitada automaticamente quando você atualiza para uma edição compatível com a RDC entre arquivos ou se um membro da conexão de replicação está executando uma edição compatível. Para obter uma lista de edições compatíveis com RDC entre arquivos, veja Quais edições do sistema operacional Windows são compatíveis com RDC entre arquivos?

### <a name="is-rdc-true-block-level-replication"></a>A RDC é uma replicação em nível de bloco verdadeira?

Não. A RDC é um protocolo de uso geral para compactação de transferência de arquivos. A Replicação do DFS usa RDC em blocos no nível de arquivo, não no nível de bloco de disco. A RDC divide um arquivo em blocos. Para cada bloco em um arquivo, ela calcula uma assinatura, que é um pequeno número de bytes que podem representar o bloco maior. O conjunto de assinaturas é transferido do servidor para o cliente. O cliente compara as assinaturas do servidor com as dele. Em seguida, o cliente solicita que o servidor envie somente os dados para assinaturas que ainda não estão no cliente.

### <a name="what-happens-if-i-rename-a-file"></a>O que acontecerá se eu renomear um arquivo?

A Replicação do DFS altera o nome do arquivo em todos os outros membros do grupo de replicação durante a próxima replicação. Os arquivos são rastreados usando uma ID exclusiva, portanto, alterar o nome de um arquivo e mover o arquivo dentro da réplica não tem nenhum efeito sobre a capacidade da Replicação do DFS de replicar um arquivo.

### <a name="what-is-cross-file-rdc"></a>O que é RDC entre arquivos?

A RDC entre arquivos permite que Replicação do DFS use a RDC mesmo quando não existir um arquivo com o mesmo nome na extremidade do cliente. A RDC entre arquivos usa uma heurística para determinar arquivos que são semelhantes ao arquivo que precisa ser replicado e usa blocos de arquivos semelhantes idênticos ao arquivo de replicação para minimizar a quantidade de dados transferidos pela WAN. A RDC entre arquivos pode usar blocos de até cinco arquivos semelhantes nesse processo.

Para usar a RDC entre arquivos, um membro da conexão de replicação deve estar executando uma edição do Windows compatível com a RDC entre arquivos. Para obter uma lista de edições compatíveis com RDC entre arquivos, veja Quais edições do sistema operacional Windows são compatíveis com RDC entre arquivos?

### <a name="what-is-rdc"></a>O que é RDC?

A RDC (Compactação Diferencial Remota) é um protocolo de cliente-servidor que pode ser usado para atualizar arquivos com eficiência em uma rede de largura de banda limitada. A RDC detecta inserções, remoções e reorganizações de dados em arquivos, permitindo que a Replicação do DFS replique apenas as alterações quando os arquivos forem atualizados. A RDC é usada somente para arquivos de 64 KB ou mais por padrão. A RDC pode usar uma versão mais antiga de um arquivo com o mesmo nome na pasta replicada ou na pasta DfsrPrivate\\ConflictandDeleted (localizada sob o caminho local da pasta replicada).

### <a name="when-is-rdc-used-for-replication"></a>Quando a RDC é usada para replicação?

A RDC é usada quando o arquivo excede um limite de tamanho mínimo. Esse limite de tamanho é de 64 KB por padrão. Depois que um arquivo que excede esse limite tiver sido replicado, as versões atualizadas do arquivo sempre usarão a RDC, a menos que uma parte grande do arquivo seja alterada ou a RDC esteja desabilitada.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>Quais edições do sistema operacional Windows são compatíveis com a RDC entre arquivos?

Para usar a RDC entre arquivos, um membro da conexão de replicação deve estar executando uma edição do sistema operacional Windows compatível com a RDC entre arquivos. A tabela a seguir mostra quais edições do sistema operacional Windows são compatíveis com a RDC entre arquivos.

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
<td><p>Windows Server 2003 R2</p></td>
<td><p>Não</p></td>
<td><p>Sim</p></td>
<td><p>Não</p></td>
</tr>
</tbody>
</table>

\* Opcionalmente, você pode desabilitar a RDC entre arquivos no Windows Server 2012 R2.

## <a name="replication-details"></a>Detalhes da replicação

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>Posso alterar o caminho de uma pasta replicada depois que ela é criada?

Não. Se você precisar alterar o caminho de uma pasta replicada, deverá excluí-la no Gerenciamento do DFS e adicioná-la novamente como uma nova pasta replicada. A Replicação do DFS então usa a RDC (Compactação Diferencial Remota) para executar uma sincronização que determina se os dados são os mesmos nos membros de envio e recebimento. Ela não replica todos os dados na pasta novamente.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>Posso configurar os atributos de arquivo que são replicados?

Não, você não pode configurar os atributos de arquivo que a Replicação do DFS replica.

Para obter uma lista de valores de atributo e suas descrições, confira [Atributos de arquivo](https://go.microsoft.com/fwlink/?linkid=182268) no MSDN (https://go.microsoft.com/fwlink/?LinkId=182268).

Os valores de atributo a seguir são definidos usando a função `SetFileAttributes dwFileAttributes` e replicados pela Replicação do DFS. As alterações nesses valores de atributo disparam a replicação dos atributos. O conteúdo do arquivo não é replicado, a menos que o conteúdo também seja alterado. Para obter mais informações, confira [Função SetFileAttributes](https://go.microsoft.com/fwlink/?linkid=182269) na biblioteca do MSDN (https://go.microsoft.com/fwlink/?LinkId=182269).

  - FILE\_ATTRIBUTE\_HIDDEN  
      
  - FILE\_ATTRIBUTE\_READONLY  
      
  - FILE\_ATTRIBUTE\_SYSTEM  
      
  - FILE\_ATTRIBUTE\_NOT\_CONTENT\_INDEXED  
      
  - FILE\_ATTRIBUTE\_OFFLINE  
      

Os valores de atributo a seguir são replicados pela Replicação do DFS, mas não disparam a replicação.

  - FILE\_ATTRIBUTE\_ARCHIVE  
      
  - FILE\_ATTRIBUTE\_NORMAL  
      

Os valores de atributo de arquivo a seguir também disparam a replicação, embora não possam ser definidos usando a função `SetFileAttributes` (use a função `GetFileAttributes` para ver os valores de atributo).

  - FILE\_ATTRIBUTE\_REPARSE\_POINT  
      

> [!NOTE]
> A Replicação do DFS não replica valores de atributo de ponto de nova análise, a menos que a marca de nova análise seja IO_REPARSE_TAG_SYMLINK. Arquivos com a marca de nova análise IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS ou IO_REPARSE_TAG_HSM são replicados como arquivos normais. No entanto, a tag de nova análise e os buffers de dados de nova análise não são replicados para outros servidores porque o ponto de nova análise funciona apenas no sistema local. 
<br>

  - FILE\_ATTRIBUTE\_COMPRESSED  
      
  - FILE\_ATTRIBUTE\_ENCRYPTED  
      

> [!NOTE]
> A Replicação do DFS não replica arquivos criptografados usando o EFS (Encrypting File System). A Replicação do DFS replica arquivos criptografados usando um software que não é da Microsoft, mas somente se ele não definir o valor do atributo FILE_ATTRIBUTE_ENCRYPTED no arquivo. 
<br>

  - FILE\_ATTRIBUTE\_SPARSE\_FILE  
      
  - FILE\_ATTRIBUTE\_DIRECTORY  
      

A Replicação do DFS não replica o valor FILE\_ATTRIBUTE\_TEMPORARY.

### <a name="can-i-control-which-member-is-replicated"></a>Posso controlar qual membro é replicado?

Sim. Você pode escolher uma topologia ao criar um grupo de replicação. Ou você pode selecionar **Nenhuma Topologia** e configurar manualmente as conexões depois que o grupo de replicação tiver sido criado.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>Posso propagar um membro do grupo de replicação com os dados antes da replicação inicial?

Sim. A Replicação do DFS é compatível com a cópia de arquivos para um membro do grupo de replicação antes da replicação inicial. Essa "preparação prévia" pode reduzir drasticamente a quantidade de dados replicados durante a replicação inicial.

A replicação inicial não precisa replicar o conteúdo quando os arquivos diferem somente por atributos reais ou carimbos de data/hora. Um atributo real é um atributo que pode ser definido pela função do Win32 `SetFileAttributes`. Para obter mais informações, confira [Função SetFileAttributes](https://go.microsoft.com/fwlink/?linkid=182269) na biblioteca do MSDN (https://go.microsoft.com/fwlink/?LinkId=182269). Se dois arquivos diferirem por outros atributos, como compactação, o conteúdo do arquivo será replicado.

Para pré-configurar um membro do grupo de replicação, copie os arquivos para a pasta apropriada nos servidores de destino, crie o grupo de replicação e, em seguida, escolha um membro primário. Escolha o membro que tem os arquivos mais atualizados que você deseja replicar, pois o conteúdo do membro primário é considerado "autoritativo". Isso significa que, durante a replicação inicial, os arquivos do membro primário sempre substituirão outras versões dos arquivos em outros membros do grupo de replicação.

Para obter informações sobre a pré-propagação e a clonagem do banco de dados DFSR, confira [Sincronização inicial da Replicação do DFS no Windows Server 2012 R2: o ataque dos clones](https://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Para obter mais informações sobre a replicação inicial, confira [Criar um grupo de replicação](https://technet.microsoft.com/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>A Replicação do DFS supera problemas comuns do Serviço de Replicação de Arquivos?

Sim. A Replicação do DFS supera três problemas comuns do FRS:

  - Encapsulamentos de diário: a Replicação do DFS recupera-se de encapsulamentos de diário em tempo real. Cada arquivo ou pasta existente será marcada como journalWrap e verificada no sistema de arquivos antes que a replicação seja habilitada novamente. Durante a recuperação, esse volume não está disponível para replicação em nenhuma das direções.  
      
  - Replicação excessiva: para evitar a replicação excessiva, a Replicação do DFS usa um sistema de créditos.  
      
  - Pastas transformadas: para evitar nomes de pastas transformadas, a Replicação do DFS armazena dados conflitantes em uma pasta DfsrPrivate\\ConflictandDeleted oculta (localizada sob o caminho local da pasta replicada). Por exemplo, criar várias pastas simultaneamente com nomes idênticos em servidores diferentes replicados usando o FRS faz com que o FRS altere o nome das pastas mais antigas. A Replicação do DFS, em vez disso, move as pastas mais antigas para a pasta Conflito e Excluída.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>A Replicação do DFS replica arquivos em ordem cronológica?

Não. Os arquivos podem ser replicados fora de ordem.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>A Replicação do DFS replica arquivos que estão sendo usados por outro aplicativo?

Se um aplicativo abrir um arquivo e criar um bloqueio de arquivo nele (impedindo que ele seja usado por outros aplicativos enquanto estiver aberto), a Replicação do DFS não replicará o arquivo até que ele seja fechado. Se o aplicativo abrir o arquivo com acesso de leitura/compartilhamento, o arquivo ainda poderá ser replicado.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>A Replicação do DFS replica permissões de arquivo NTFS, fluxos de dados alternativos, links físicos e pontos de nova análise?

  - A Replicação do DFS replica as permissões de arquivo NTFS e os fluxos de dados alternativos.  
      
  - A Microsoft não é compatível com a criação de links físicos de NTFS de ou para arquivos em uma pasta replicada. Fazer isso pode causar problemas de replicação com os arquivos afetados. Os arquivos de vínculo físico são ignorados pela Replicação do DFS e não são replicados. Os pontos de junção também não são replicados e a Replicação do DFS registra em log o evento 4406 para cada ponto de junção que encontra.  
      
  - Os únicos pontos de nova análise replicados pela Replicação do DFS são aqueles que usam a tag IO\_REPARSE\_TAG\_SYMLINK; no entanto, a Replicação do DFS não garante que o alvo de um symlink também seja replicado. Para obter mais informações, confira o [blog Ask the Directory Services Team](https://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - Arquivos com as tags de nova análise IO\_REPARSE\_TAG\_DEDUP, IO\_REPARSE\_TAG\_SIS ou IO\_REPARSE\_TAG\_HSM são replicados como arquivos normais. A tag de nova análise e os buffers de dados de nova análise não são replicados para outros servidores porque o ponto de nova análise funciona apenas no sistema local. Dessa forma, a Replicação do DFS pode replicar pastas em volumes que usam Eliminação de Duplicação de Dados no Windows Server 2012 ou SIS (armazenamento de instância única), no entanto, as informações de eliminação de duplicação de dados são mantidas separadamente por cada servidor no qual o serviço de função está habilitado.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>A Replicação do DFS replicará alterações de carimbo de data/hora se nenhuma outra alteração for feita no arquivo?

Não, a Replicação do DFS não replicará os arquivos para os quais a única alteração é uma alteração no carimbo de data/hora. Além disso, o carimbo de data/hora alterado não é replicado para outros membros do grupo de replicação, a menos que outras alterações sejam feitas no arquivo.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>A Replicação do DFS replica permissões atualizadas em um arquivo ou pasta?

Sim. A Replicação do DFS replica as alterações de permissão para arquivos e pastas. Somente a parte do arquivo associada à ACL (lista de controle de acesso) é replicada, embora a Replicação do DFS ainda deva ler o arquivo inteiro na área de preparo.


> [!NOTE]
> Alterar ACLs em um grande número de arquivos pode afetar o desempenho da replicação. No entanto, ao usar a RDC, a quantidade de dados transferidos é proporcional ao tamanho das ACLs, não ao tamanho do arquivo inteiro. A quantidade de tráfego de disco ainda é proporcional ao tamanho dos arquivos, pois os arquivos devem ser lidos de e para a pasta de preparo. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>A Replicação do DFS é compatível com a mesclagem de arquivos de texto em caso de conflito?

A Replicação do DFS não mescla arquivos quando há um conflito. No entanto, ela tenta preservar a versão mais antiga do arquivo na pasta DfsrPrivate\\ConflictandDeleted oculta no computador em que o conflito foi detectado.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>A Replicação do DFS usa criptografia ao transmitir dados?

Sim. A Replicação do DFS usa conexões de RPC (chamada de procedimento remoto) com criptografia.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>É possível desabilitar o uso de RPC criptografada?

Não. O serviço de Replicação do DFS usa RPC (chamadas de procedimento remoto) sobre TCP para replicar dados. Para proteger as transferências de dados pela Internet, o serviço de Replicação do DFS foi projetado para sempre usar a constante em nível de autenticação `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`. Isso garante que a comunicação RPC pela Internet seja sempre criptografada. Portanto, não é possível desabilitar o uso de RPC criptografado pelo serviço de Replicação do DFS.

Para obter mais informações, confira os seguintes sites da Microsoft:

  - [Referência Técnica de RPC](https://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [Sobre a Compactação Diferencial Remota](https://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Constantes em nível de autenticação](https://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>Como as replicações simultâneas são tratadas?

Há um gerenciador de atualização por pasta replicada. Os gerenciadores de atualização funcionam de maneira independente uns do outros.

Por padrão, um máximo de 16 (quatro no Windows Server 2003 R2) downloads simultâneos são compartilhados entre todas as conexões e grupos de replicação. Como as atualizações de conexões e grupo de replicação não são serializadas, não há nenhuma ordem específica para o recebimento das atualizações. Se dois agendamentos estão abertos, as atualizações geralmente são recebidas e instaladas de ambas as conexões ao mesmo tempo.

### <a name="how-do-i-force-replication-or-polling"></a>Como forçar a replicação ou a sondagem?

Você pode forçar a replicação imediatamente usando o gerenciamento de DFS, conforme descrito em [Editar agendas de replicação](https://technet.microsoft.com/library/Cc732278). Você também pode forçar a replicação usando o cmdlet `Sync-DfsReplicationGroup`, incluído no módulo DFSR do PowerShell introduzido com o Windows Server 2012 R2 ou o comando **Dfsrdiag SyncNow**. Você pode forçar a sondagem usando o cmdlet `Update-DfsrConfigurationFromAD` ou o comando **Dfsrdiag PollAD**.

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>É possível configurar um tempo de silêncio entre as replicações para arquivos que mudam com frequência?

Não. Se a agenda estiver aberta, a Replicação do DFS replicará as alterações conforme as detectar. Não é possível configurar um tempo de silêncio para arquivos.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>É possível configurar uma replicação unidirecional com a Replicação do DFS?

Sim. Se você estiver usando o Windows Server 2012 ou o Windows Server 2008 R2, poderá criar uma pasta replicada somente leitura que replica o conteúdo por meio de uma conexão unidirecional. Para obter mais informações, confira [Tornar uma pasta replicada somente leitura em um membro específico](https://go.microsoft.com/fwlink/?linkid=156740) (https://go.microsoft.com/fwlink/?LinkId=156740).

Não há suporte para criar uma conexão de replicação unidirecional com a Replicação do DFS no Windows Server 2008 ou no Windows Server 2003 R2. Isso pode causar vários problemas, incluindo erros de topologia de verificação de integridade, problemas de preparo e problemas com o banco de dados Replicação do DFS.

Se você estiver usando o Windows Server 2008 ou o Windows Server 2003 R2, poderá simular uma conexão unidirecional executando as seguintes ações:

  - Treine os administradores para fazer alterações apenas aos servidores que você deseja designar como primários. Em seguida, permita que as alterações sejam replicadas nos servidores de destino.  
      
  - Configure as permissões de compartilhamento nos servidores de destino para que os usuários finais não tenham permissões de Gravação. Se nenhuma alteração for permitida nos servidores de branch, não haverá nada para replicar de volta, simulando uma conexão unidirecional e mantendo a baixa utilização da WAN.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>Há uma maneira de forçar uma replicação completa de todos os arquivos, incluindo arquivos inalterados?

Não. Se a Replicação do DFS considerar os arquivos idênticos, eles não serão replicados. Se os arquivos alterados não tiverem sido replicados, a Replicação do DFS os replicará automaticamente quando configurada para fazer isso. Para substituir o agendamento configurado, use o método WMI **ForceReplicate()** . No entanto, essa é apenas uma substituição de agendamento e não força a replicação de arquivos idênticos ou inalterados.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>O que acontecerá se o membro primário sofrer uma perda de banco de dados durante a replicação inicial?

Durante a replicação inicial, os arquivos do membro primário sempre terão precedência na resolução de conflitos que ocorrerá se os membros de recebimento tiverem versões diferentes de arquivos no membro primário. A designação de membro primário é armazenada no Active Directory Domain Services e a designação é desmarcada depois que o membro primário está pronto para ser replicado, mas antes que todos os membros do grupo de replicação sejam replicados.

Se a replicação inicial falhar ou o serviço de Replicação do DFS for reiniciado durante a replicação, o membro primário verá a designação de membro primário no banco de dados de Replicação do DFS local e tentará novamente a replicação inicial. Se o banco de dados da Replicação do DFS do membro primário for perdido depois de limpar a designação primária no Active Directory Domain Services, mas antes que todos os membros do grupo de replicação concluam a replicação inicial, todos os membros do grupo de replicação falharão em replicar a pasta, pois nenhum servidor está designado como o membro primário. Se isso acontecer, use o comando **Dfsradmin membership /set /isprimary:true** no servidor membro primário para restaurar manualmente a designação de membro primário.

Para obter mais informações sobre a replicação inicial, confira [Criar um grupo de replicação](https://technet.microsoft.com/library/cc725893).


> [!WARNING]
> A designação de membro primário é usada somente durante o processo de replicação inicial. Se você usar o comando <STRONG>Dfsradmin</STRONG> para especificar um membro primário para uma pasta replicada após a conclusão da replicação, a Replicação do DFS não designará o servidor como um membro primário no Active Directory Domain Services. No entanto, se o banco de dados de Replicação do DFS no servidor depois sofrer danos irreversíveis ou perda, o servidor tentará executar uma replicação inicial como o membro primário, em vez de recuperar seus dados de outro membro do grupo de replicação. Essencialmente, o servidor passa a ser um servidor primário não autorizado, o que pode causar conflitos. Por esse motivo, especifique o membro primário manualmente somente se tiver certeza de que a replicação inicial sofreu uma falha irrecuperável. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>O que acontecerá se a agenda de replicação for fechada enquanto um arquivo estiver sendo replicado?

Se a RDC (Compactação Diferencial Remota) estiver habilitada na conexão, a replicação de entrada de um arquivo maior que 64 KB que começou a ser replicado imediatamente antes do fechamento da agenda (ou alteração para **Nenhuma largura de banda**) continuará quando a agenda for aberta (ou mudar para algo diferente de **Nenhuma largura de banda**). A replicação continua no estado em que estava quando a replicação foi interrompida.

Se a RDC estiver desativada, a Replicação do DFS reiniciará completamente a transferência de arquivo. Isso pode atrasar quando o arquivo está disponível no membro de recebimento.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>O que acontece quando dois usuários atualizam simultaneamente o mesmo arquivo em servidores diferentes?

Quando a Replicação do DFS detecta um conflito, ela usa a versão do arquivo que foi salva por último. Ela move o outro arquivo para a pasta DfsrPrivate\\ConflictandDeleted (no caminho local da pasta replicada no computador que resolveu o conflito). Ele permanece lá até a limpeza de pasta de Conflito e Excluída, que ocorre quando a pasta de conflito e excluída excede o tamanho configurado ou a Replicação do DFS encontra um erro de espaço em disco insuficiente. A pasta de Conflito e Excluída não é replicada. Esse método de resolução de conflitos evita o problema de diretórios transformados que eram possíveis no FRS.

Quando ocorre um conflito, a Replicação do DFS registra um evento informativo no log de eventos da Replicação do DFS. Esse evento não requer ação do usuário pelos seguintes motivos:

  - Ele não é visível para os usuários (ele fica visível somente para administradores de servidor).  
      
  - A Replicação do DFS trata o conflito e a pasta excluída como um cache. Quando um limite de cota é atingido, ele limpa alguns desses arquivos. Não há nenhuma garantia de que os arquivos conflitantes serão salvos.  
      
  - O conflito pode residir em um servidor diferente daquele da origem do conflito.  
      

## <a name="staging"></a>Preparando

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>A Replicação do DFS continua a preparação de arquivos quando a replicação é desabilitada por uma cota de limitação de largura de banda ou agendamento ou quando uma conexão é desabilitada manualmente?

Não. A Replicação do DFS não continuará a preparar arquivos fora dos tempos de replicação agendados, se a cota de limitação de largura de banda tiver sido excedida nem quando as conexões estiverem desabilitadas.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>A Replicação do DFS impede que outros aplicativos acessem um arquivo durante o preparo?

Não. A Replicação do DFS abre arquivos de uma maneira que não impede os usuários nem os aplicativos de abrir os arquivos na pasta de replicação. Esse método é conhecido como "bloqueio oportunista".

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>É possível alterar a localização da pasta de preparo com a ferramenta de Gerenciamento do DFS?

Sim. A localização da pasta de preparo é configurado na guia **Avançado** da caixa de diálogo **Propriedades** para cada membro de um grupo de replicação.

### <a name="when-are-files-staged"></a>Quando os arquivos são preparados?

Os arquivos são preparados no membro de envio quando o membro de recebimento solicita o arquivo (a menos que o arquivo tenha 64 KB ou menos), conforme mostrado na tabela a seguir. Se a RDC (Compactação Diferencial Remota) estiver desabilitada na conexão, o arquivo será preparado, a menos que tenha 256 KB ou menos. Os arquivos também serão preparados no membro receptor à medida que são transferidos se tiverem menos de 64 KB de tamanho, embora você possa definir essa configuração entre 16 KB e 1 MB. Se a agenda for fechada, os arquivos não serão preparados.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Os tamanhos mínimos de arquivo para arquivos de preparo

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>RDC habilitada</th>
<th>RDC desabilitada</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Membro remetente</p></td>
<td><p>64 KB</p></td>
<td><p>256 KB</p></td>
</tr>
<tr class="odd">
<td><p>Membro destinatário</p></td>
<td><p>64 KB por padrão</p></td>
<td><p>64 KB por padrão</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>O que acontece se um arquivo for alterado depois de ser preparado, mas antes de ser completamente transmitido para o site remoto?

Se alguma parte do arquivo já estiver sendo transmitida, a Replicação do DFS continuará a transmissão. Se o arquivo for alterado antes que a Replicação do DFS comece a transmitir o arquivo, a versão mais recente do arquivo será enviada.

## <a name="change-history"></a>Histórico de Alterações


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Data</th>
<th>Descrição</th>
<th>Motivo</th>
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
<td><p>Atualizada a seção "Quais são os limites compatíveis da Replicação do DFS?" com resultados de testes no Windows Server 2012 R2.</p></td>
<td><p>Atualizações para a versão mais recente do Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>30 de janeiro de 2013</p></td>
<td><p>Adicionada a entrada "A Replicação do DFS continua a preparação de arquivos quando a replicação é desabilitada por uma cota de limitação de largura de banda ou agendamento ou quando uma conexão é desabilitada manualmente?".</p></td>
<td><p>Perguntas do cliente</p></td>
</tr>
<tr class="even">
<td><p>31 de outubro de 2012</p></td>
<td><p>Editada a entrada "Quais são os limites compatíveis da Replicação do DFS?" para aumentar o número testado de arquivos replicados em um volume.</p></td>
<td><p>Comentários de clientes</p></td>
</tr>
<tr class="odd">
<td><p>15 de agosto de 2012</p></td>
<td><p>Editada a entrada "A Replicação do DFS replicar permissões de arquivo NTFS, fluxos de dados alternativos, links físicos e pontos de nova análise?" para esclarecer melhor como a Replicação do DFS lida com links físicos e reanalisar os pontos.</p></td>
<td><p>Comentários dos serviços de atendimento ao cliente</p></td>
</tr>
<tr class="even">
<td><p>13 de junho de 2012</p></td>
<td><p>Editada a entrada "A Replicação do DFS funciona em volumes ReFS ou FAT?" para adicionar a discussão de ReFS.</p></td>
<td><p>Comentários de clientes</p></td>
</tr>
<tr class="odd">
<td><p>25 de abril de 2012</p></td>
<td><p>Editada a entrada "A Replicação do DFS replicar permissões de arquivo NTFS, fluxos de dados alternativos, links físicos e pontos de nova análise?" para esclarecer como a Replicação do DFS lida com links físicos.</p></td>
<td><p>Reduzir a confusão em potencial</p></td>
</tr>
<tr class="even">
<td><p>30 de março de 2011</p></td>
<td><p>Editada a entrada "A Replicação do DFS pode replicar os arquivos de banco de dados .pst do Outlook ou do Microsoft Office Access?" para corrigir o impacto potencial do uso da Replicação do DFS com arquivos .pst e do Access.</p>
<p>Adicionado Como posso aprimorar o desempenho da replicação?</p></td>
<td><p>Perguntas do cliente sobre a entrada anterior, que indicaram incorretamente que a replicação de arquivos .pst ou do Access podia danificar o banco de dados Replicação do DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 de janeiro de 2011</p></td>
<td><p>Adicionado "Como os arquivos podem ser recuperados das pastas ConflictAndDeleted ou PreExisting?"</p></td>
<td><p>Comentários de clientes</p></td>
</tr>
<tr class="even">
<td><p>20 de outubro de 2010</p></td>
<td><p>Adicionado "Como posso atualizar ou substituir um membro da Replicação do DFS"?</p></td>
<td><p>Comentários de clientes</p></td>
</tr>
</tbody>
</table>

