---
title: Recuperação de floresta do AD-determine como recuperar a floresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: d604efded5b6a2ff3911a92f52817498f43c9933
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369171"
---
# <a name="determine-how-to-recover-the-forest"></a>Determinar como recuperar a floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

A recuperação de uma floresta Active Directory inteira envolve a restauração do backup ou a reinstalação de Active Directory Domain Services (AD DS) em cada DC (controlador de domínio) na floresta. A recuperação da floresta restaura cada domínio na floresta para seu estado no momento do último backup confiável. Consequentemente, a operação de restauração resultará na perda de pelo menos os seguintes dados de Active Directory:

- Todos os objetos (como usuários e computadores) que foram adicionados após o último backup confiável
- Todas as atualizações que foram feitas em objetos existentes desde o último backup confiável
- Todas as alterações feitas na partição de configuração ou na partição de esquema em AD DS (como alterações de esquema) desde o último backup confiável

Para cada domínio na floresta, a senha de uma conta de administrador de domínio deve ser conhecida. Preferivelmente, essa é a senha da conta de administrador interna. Você também deve saber a senha do DSRM para executar uma restauração do estado do sistema de um controlador de domínio. Em geral, é uma prática recomendada arquivar a conta de administrador e o histórico de senha do DSRM em um local seguro, desde que os backups sejam válidos, ou seja, dentro do período de vida útil da marca de exclusão ou dentro do período de tempo de vida do objeto excluído se Active Directory reciclagem O compartimento está habilitado. Você também pode sincronizar a senha do DSRM com uma conta de usuário de domínio para facilitar a sua memorização. Para obter mais informações, consulte o artigo [961320](https://support.microsoft.com/kb/961320)da base de dados de conhecimento. A sincronização da conta do DSRM deve ser feita antes da recuperação da floresta, como parte da preparação.

> [!NOTE]
> A conta de administrador é um membro do grupo de administradores internos por padrão, assim como os grupos admins. do domínio e administradores de empresa. Esse grupo tem controle total de todos os DCs no domínio.

## <a name="determining-which-backups-to-use"></a>Determinando quais backups usar

Faça backup de pelo menos dois DCs graváveis para cada domínio regularmente para que você tenha vários backups para escolher. Observe que você não pode usar o backup de um RODC (controlador de domínio somente leitura) para restaurar um DC gravável. Recomendamos que você restaure os DCs usando backups que foram feitos alguns dias antes da ocorrência da falha. Em geral, você deve determinar uma compensação entre a recente e a segurança dos dados restaurados. A escolha de um backup mais recente recupera dados mais úteis, mas pode aumentar o risco de reintroduzir dados perigosos na floresta restaurada.

A restauração de backups de estado do sistema depende do sistema operacional original e do servidor do backup. Por exemplo, você não deve restaurar um backup de estado do sistema para um servidor diferente. Nesse caso, você pode ver o seguinte aviso:

"O backup especificado é de um servidor diferente do atual. Não recomendamos a execução de uma recuperação de estado do sistema com o backup em um servidor alternativo, pois o servidor pode se tornar inutilizável. Tem certeza de que deseja usar este backup para recuperar o servidor atual? "

Se você precisar restaurar Active Directory para um hardware diferente, crie backups completos do servidor e planeje a execução de uma recuperação completa do servidor.

> [!IMPORTANT]
> A partir do Windows Server 2008, não há suporte para restaurar o backup do estado do sistema para uma nova instalação do Windows Server em um novo hardware ou o mesmo hardware. Se o Windows Server for reinstalado no mesmo hardware, conforme recomendado posteriormente neste guia, você poderá restaurar o controlador de domínio nesta ordem:
>
> 1. Execute uma restauração completa do servidor para restaurar o sistema operacional e todos os arquivos e aplicativos.
> 2. Execute uma restauração de estado do sistema usando o Wbadmin. exe para marcar o SYSVOL como autoritativo.
>
> Para obter mais informações, consulte o artigo [249694](https://support.microsoft.com/kb/249694)do Microsoft KB.

Se a hora da ocorrência da falha for desconhecida, investigue mais para identificar os backups que mantêm o último estado de segurança da floresta. Essa abordagem é menos desejável. Portanto, é altamente recomendável que você mantenha logs detalhados sobre o estado de integridade de AD DS diariamente para que, se houver uma falha em toda a floresta, o tempo aproximado de falha possa ser identificado. Você também deve manter uma cópia local dos backups para permitir uma recuperação mais rápida.

Se Active Directory Lixeira estiver habilitada, o tempo de vida do backup será igual ao valor **deletedObjectLifetime** ou ao valor **tombstoneLifetime** , o que for menor. Para obter mais informações, consulte [Active Directory guia](https://go.microsoft.com/fwlink/?LinkId=178657) passo a passo da lixeira (https://go.microsoft.com/fwlink/?LinkId=178657).

Como alternativa, você também pode usar a ferramenta de montagem de banco de dados Active Directory (Dsamain. exe) e uma ferramenta LDAP (Lightweight Directory Access Protocol), como Ldp. exe ou Active Directory usuários e computadores, para identificar qual backup tem o último estado de segurança do floresta. A ferramenta de montagem de banco de dados Active Directory, que está incluída nos sistemas operacionais Windows Server 2008 e posteriores do Windows Server, expõe Active Directory dados armazenados em backups ou instantâneos como um servidor LDAP. Em seguida, você pode usar uma ferramenta LDAP para procurar os dados. Essa abordagem tem a vantagem de não exigir que você reinicie qualquer DC no Modo de Restauração dos Serviços de Diretório (DSRM) para examinar o conteúdo do backup de AD DS.

Para obter mais informações sobre como usar a ferramenta de montagem de banco de dados Active Directory, consulte o [guia passo a passo da ferramenta de montagem de banco de dados do Active Directory](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).

Você também pode usar o comando **Ntdsutil snapshot** para criar instantâneos do banco de dados Active Directory. Ao agendar uma tarefa para criar instantâneos periodicamente, você pode obter cópias adicionais do banco de dados Active Directory ao longo do tempo. Você pode usar essas cópias para identificar melhor quando a falha em toda a floresta ocorreu e escolher o melhor backup a ser restaurado. Para criar instantâneos, use a versão do **Ntdsutil** fornecida com o windows Server 2008 ou o ferramentas de administração de servidor remoto (RSAT) para Windows Vista ou posterior. O DC de destino pode executar qualquer versão do Windows Server. Para obter mais informações sobre como usar o comando de **instantâneo do Ntdsutil** , consulte [instantâneo](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).

## <a name="determining-which-domain-controllers-to-restore"></a>Determinando quais controladores de domínio restaurar

A facilidade do processo de restauração é um fator importante ao decidir qual controlador de domínio deve ser restaurado. É recomendável ter um DC dedicado para cada domínio que seja o DC preferencial para uma restauração. Um DC de restauração dedicado torna mais fácil planejar e executar a recuperação de floresta de forma confiável, pois você usa a mesma configuração de origem que foi usada para executar testes de restauração. Você pode criar o script da recuperação e não lutar com configurações diferentes, como se o controlador de domínio contém funções de mestre de operações ou não, ou se é um servidor GC ou DNS ou não.

> [!NOTE]
> Embora não seja recomendável restaurar um detentor da função mestre de operações no interesse da simplicidade, algumas organizações podem optar por restaurar uma para outras vantagens. Por exemplo, restaurar o mestre RID pode ajudar a evitar problemas com o gerenciamento de RIDs durante a recuperação.  

Escolha um controlador de domínio que melhor atenda aos seguintes critérios:

- Um DC que é gravável. Isso é obrigatório.

- Um DC que executa o Windows Server 2012 como uma máquina virtual em um hipervisor que dá suporte a VM-Generationid. Esse controlador de domínio pode ser usado como uma fonte para clonagem.
- Um DC que é acessível, fisicamente ou em uma rede virtual, e, preferencialmente, localizado em um datacenter. Dessa forma, você pode isolá-lo facilmente da rede durante a recuperação da floresta.
- Um DC que tem um bom backup completo do servidor. Um bom backup é um backup que pode ser restaurado com êxito, que foi feito alguns dias antes da falha e contém o máximo possível de dados úteis.
- Um DC que era um servidor DNS (sistema de nomes de domínio) antes da falha. Isso poupa o tempo necessário para reinstalar o DNS.
- Se você também usar os serviços de implantação do Windows, escolha um DC que não esteja configurado para usar o desbloqueio de rede do BitLocker. Nesse caso, o desbloqueio de rede do BitLocker não tem suporte a ser usado para o primeiro controlador de domínio que você restaura do backup durante uma recuperação de floresta.

   O desbloqueio de rede do BitLocker como o *único* protetor de chave *não pode* ser usado em DCS nos quais você implantou o WDS (serviços de implantação do Windows), pois isso resulta em um cenário em que o primeiro DC requer Active Directory e o WDS funcione para automático. Mas antes de restaurar o primeiro controlador de domínio, Active Directory ainda não está disponível para o WDS, portanto, ele não pode desbloquear.

   Para determinar se um controlador de domínio está configurado para usar o desbloqueio de rede do BitLocker, verifique se um certificado de desbloqueio de rede está identificado na seguinte chave do registro:

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

Mantenha os procedimentos de segurança ao manipular ou restaurar arquivos de backup que incluem Active Directory. A urgência que acompanha a recuperação da floresta pode levar intencionalmente à desprocura de práticas recomendadas de segurança. Para obter mais informações, consulte a seção "estabelecendo estratégias de backup e restauração do controlador de domínio" no guia de prática do [Best para proteger as instalações de Active Directory e as operações cotidianas: Parte II @ no__t-0.

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identificar a estrutura de floresta e as funções de controlador de domínio atuais

Determine a estrutura da floresta atual identificando todos os domínios na floresta. Faça uma lista de todos os DCs em cada domínio, especialmente os DCs que têm backups e os DCs virtualizados que podem ser uma fonte de clonagem. Uma lista de DCs para o domínio raiz da floresta será a mais importante, pois você primeiro recuperará esse domínio. Depois de restaurar o domínio raiz da floresta, você pode obter uma lista de outros domínios, DCs e sites na floresta usando Active Directory snap-ins.

Prepare uma tabela que mostra as funções de cada DC no domínio, conforme mostrado no exemplo a seguir. Isso irá ajudá-lo a reverter para a configuração de pré-falha da floresta após a recuperação.

|Nome do DC|Sistema operacional|FSMO|GCS|RODC|Backup|DNS|Server Core|VM|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|Mestre de esquema, mestre de nomeação de domínio|Sim|Não|Sim|Não|Não|Sim|Sim|  
|DC_2|Windows Server 2012|Nenhuma|Sim|Não|Sim|Sim|Não|Sim|Sim|  
|DC_3|Windows Server 2012|Mestre de Infra-Estrutura|Não|Não|Não|Sim|Sim|Sim|Sim|  
|DC_4|Windows Server 2012|Emulador de PDC, mestre de RID|Sim|Não|Não|Não|Não|Sim|Não|  
|DC_5|Windows Server 2012|Nenhuma|Não|Não|Sim|Sim|Não|Sim|Sim|  
|RODC_1|Windows Server 2008 R2|Nenhuma|Sim|Sim|Sim|Sim|Sim|Sim|Não|  
|RODC_2|Windows Server 2008|Nenhuma|Sim|Sim|Não|Sim|Sim|Sim|Não|  

Para cada domínio na floresta, identifique um único DC gravável que tenha um backup confiável do banco de dados Active Directory para esse domínio. Tenha cuidado ao escolher um backup para restaurar um DC. Se o dia e a causa da falha forem aproximadamente conhecidos, a recomendação geral é usar um backup feito alguns dias antes dessa data.
  
Neste exemplo, há quatro candidatos de backup: DC_1, DC_2, DC_4 e DC_5. Desses candidatos de backup, você restaura apenas um. O DC recomendado é DC_5 pelos seguintes motivos:  

- Ele atende aos requisitos para usá-lo como uma fonte para clonagem virtualizada de DC, ou seja, ele executa o Windows Server 2012 como um controlador de domínio virtual em um hipervisor que dá suporte a VM-Generationid, executa software que pode ser clonado (ou que poderá ser removido se não puder ser clonado d). Após a restauração, a função de emulador de PDC será executada nesse servidor e poderá ser adicionada ao grupo de controladores de domínio clonáveis para o domínio.  
- Ele executa uma instalação completa do Windows Server 2012. Um DC que executa uma instalação do Server Core pode ser menos conveniente como um destino para recuperação.  
- É um servidor DNS. Portanto, o DNS não precisa ser reinstalado.  

> [!NOTE]
> Como o DC_5 não é um servidor de catálogo global, ele também tem uma vantagem em que o catálogo global não precisa ser removido após a restauração. Mas se o DC também é um servidor de catálogo global não é um fator decisivo porque, a partir do Windows Server 2012, todos os DCs são servidores de catálogo global por padrão e a remoção e a adição do catálogo global após a restauração é recomendada como parte da floresta processo de recuperação em qualquer caso.  

## <a name="recover-the-forest-in-isolation"></a>Recuperar a floresta isoladamente

O cenário preferencial é desligar todos os DCs graváveis antes que o primeiro controlador de domínio restaurado seja colocado de volta para a produção. Isso garante que qualquer dado perigoso não seja replicado de volta para a floresta recuperada. É particularmente importante desligar todos os detentores de função do mestre de operações.  

> [!NOTE]
> Pode haver casos em que você move o primeiro DC que pretende recuperar para cada domínio em uma rede isolada, enquanto permite que outros controladores permaneçam online a fim de minimizar o tempo de inatividade do sistema. Por exemplo, se você estiver recuperando de uma atualização de esquema com falha, poderá optar por manter os controladores de domínio em execução na rede de produção enquanto executa as etapas de recuperação isoladamente.

Se você estiver executando DCs virtualizados, poderá movê-los para uma rede virtual isolada da rede de produção em que você executará a recuperação. Mover DCs virtualizados para uma rede separada fornece dois benefícios:  

- Os DCs recuperados são impedidos de recorrência do problema que causou a recuperação da floresta porque eles estão isolados.  
- A clonagem de DC virtualizado pode ser executada na rede separada para que um número crítico de controladores de domínio possa ser executado e testado antes de ser levado de volta à rede de produção.

Se você estiver executando DCs em hardware físico, desconecte o cabo de rede do primeiro DC que você planeja restaurar no domínio raiz da floresta. Se possível, desconecte também os cabos de rede de todos os outros DCs. Isso impede que os DCs sejam replicados, se forem iniciados acidentalmente durante o processo de recuperação da floresta.  

Em uma floresta grande que é distribuída em vários locais, pode ser difícil garantir que todos os DCs graváveis sejam desligados. Por esse motivo, as etapas de recuperação — como redefinir a conta de computador e a conta krbtgt, além da limpeza de metadados — foram projetadas para garantir que os DCs graváveis recuperados não sejam replicados com DCs graváveis perigosos (caso alguns ainda estejam online no floresta).  
  
No entanto, apenas com o uso de DCs graváveis offline é possível garantir que a replicação não ocorra. Portanto, sempre que possível, você deve implantar a tecnologia de gerenciamento remoto que pode ajudá-lo a desligar e isolar fisicamente os DCs graváveis durante a recuperação da floresta.  
  
Os RODCs podem continuar a operar enquanto os DCs graváveis estiverem offline. Nenhum outro controlador de domínio replicará diretamente quaisquer alterações de qualquer RODC — especialmente, sem alterações de esquema ou de contêiner de configuração – para que não apresentem o mesmo risco que os DCs graváveis durante a recuperação. Depois que todos os DCs graváveis forem recuperados e online, você deverá recompilar todos os RODCs.  
  
Os RODCs continuarão a permitir o acesso a recursos locais que são armazenados em cache em seus respectivos sites, enquanto as operações de recuperação entram em paralelo. Os recursos locais que não são armazenados em cache no RODC terão solicitações de autenticação encaminhadas para um controlador de domínio gravável. Essas solicitações falharão porque os DCs graváveis estão offline. Algumas operações, como alterações de senha, também não funcionarão até que você recupere os DCs graváveis.  
  
Se você estiver usando uma arquitetura de rede hub e spoke, poderá se concentrar primeiro na recuperação dos DCs graváveis nos sites do Hub. Posteriormente, você pode recriar os RODCs em sites remotos.  
  
## <a name="next-steps"></a>Próximas etapas

- [Recuperação de floresta do AD – Pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperação de floresta do AD-planejar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD – identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD-determine como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD-executar recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)  
- [Recuperação de floresta do AD-perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperação de floresta do AD-recuperando um único domínio em uma floresta de multidomínio](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperação de floresta do AD-recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
