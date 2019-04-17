---
title: "Recuperação de floresta do AD - determinar como recuperar da floresta"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: cc2525068225644b0964a2726bd9c0dcf2e0cba0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="determine-how-to-recover-the-forest"></a>Determinar como recuperar da floresta  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Recuperar toda a floresta do Active Directory envolve a restauração de backup ou reinstalando os serviços de domínio do Active Directory (AD DS) em todos os controladores de domínio (DC) na floresta. Recuperar floresta restaura cada domínio na floresta para seu estado no momento do último backup confiável. Consequentemente, a operação de restauração resultará na perda de pelo menos os seguintes dados do Active Directory:  
  
-   Todos os objetos que foram adicionados depois do último backup confiável (por exemplo, os usuários e computadores)  
  
-   Todas as atualizações que foram feitas aos objetos existentes como confiável o último backup  
  
-   Todas as alterações que foram feitas para a partição de configuração ou a partição do esquema no AD DS (como alterações no esquema) desde o último backup confiável  
  
 Para cada domínio na floresta, a senha de uma conta de administrador do domínio deve ser conhecida. Preferencialmente, esta é a senha da conta do administrador interna não deve ser desabilitada. Você também deve saber a senha DSRM para executar uma restauração do estado do sistema de um controlador de domínio. Em geral, é uma boa prática para arquivar a conta de administrador e o histórico de senhas DSRM em um lugar seguro para, desde os backups são válidos, ou seja, dentro da período de tempo de vida de desativação ou dentro do objeto excluído período de tempo de vida se a Lixeira do Active Directory está habilitada. Você também pode sincronizar a senha DSRM com uma conta de usuário de domínio para tornar mais fácil de lembrar. Para obter mais informações, consulte o artigo KB [961320](https://support.microsoft.com/kb/961320). Sincronizar a conta DSRM deve ser feito antes da recuperação floresta, como parte da preparação.  
  
> [!NOTE]
>  A conta de administrador é um membro do grupo Administradores interno por padrão, assim como os grupos de administradores de domínio e administradores corporativos. Esse grupo tem controle total de todos os controladores de domínio no domínio.  
  
## <a name="determining-which-backups-to-use"></a>Determinando quais backups usar  
 Faça backup pelo menos dois controladores de domínio graváveis para cada domínio regularmente para que você tenha vários backups para sua escolha. Observe que você não pode usar o backup de um controlador de domínio somente leitura (RODC) para restaurar um controlador de domínio gravável. Recomendamos que você restaure os controladores de domínio usando backups feitos alguns dias antes da ocorrência da falha. Em geral, você deve determinar um equilíbrio entre o quanto são recentes e safeness dos dados restaurados. Escolhendo um backup mais recente recupera os dados mais útil, mas pode aumentar o risco de reintrodução dados perigosos na floresta restaurada.  
  
 Restauração de backups de estado do sistema depende do sistema operacional original e o servidor do backup. Por exemplo, você não deve restaurar um backup do estado do sistema para um servidor diferente. Nesse caso, você pode ver o seguinte aviso:  
  
 "O backup especificado é de um servidor diferente daquele atual. Não recomendamos a realizar uma recuperação de estado do sistema com o backup para um servidor alternativo porque o servidor poderá ficar inutilizável. Tem certeza de que deseja usar esse backup para recuperar o servidor atual?"  
  
 Se você precisar restaurar o Active Directory para um hardware diferente, crie backups de servidor completo e planeja realizar uma recuperação do servidor completo.  
  
> [!IMPORTANT]
>  A partir do Windows Server 2008, ele não há suporte para restaurar o backup do estado do sistema para uma nova instalação do Windows Server no novo hardware ou no mesmo hardware. Se o Windows Server for reinstalado no mesmo hardware, como recomendados posteriormente neste guia, você pode restaurar o controlador de domínio nesta ordem:  
>   
>  1.  Execute uma restauração do servidor completo para restaurar o sistema operacional e todos os arquivos e aplicativos.  
> 2.  Execute uma restauração de estado do sistema usando wbadmin.exe para marcar SYSVOL como autoritativo.  
>   
>  Para obter mais informações, consulte Microsoft KB artigo [249694](https://support.microsoft.com/kb/249694).  
  
 Se o tempo da ocorrência da falha for desconhecido, investigue mais para identificar backups que contêm o último estado de seguro da floresta. Essa abordagem é menos desejável. Portanto, é altamente recomendável que você mantenha logs detalhados sobre o estado de integridade do AD DS diariamente para que, se houver uma falha de toda a floresta, o tempo aproximado de falha pode ser identificado. Você também deve manter uma cópia local dos backups para permitir que uma recuperação mais rápida.  
  
 Se a Lixeira do Active Directory for habilitada, o tempo de vida de backup é igual do **deletedObjectLifetime** valor ou o **vida útil para desativação** valor, o que for menor. Para obter mais informações, consulte [a Lixeira do Active Directory Step-by-Step guia](https://go.microsoft.com/fwlink/?LinkId=178657) (https://go.microsoft.com/fwlink/?LinkId=178657).  
  
 Como alternativa, você também pode usar o Active Directory banco de dados montagem ferramenta (Dsamain.exe) e uma ferramenta de Lightweight Directory Access Protocol (LDAP), como Ldp.exe ou Active Directory usuários e computadores, para identificar quais backup tem o último estado de seguro da floresta. A ferramenta de montagem do banco de dados do Active Directory, que está incluída no Windows Server 2008 e nos sistemas operacionais Windows Server posteriores, expõe os dados do Active Directory que são armazenados em backups ou instantâneos como um servidor LDAP. Em seguida, você pode usar uma ferramenta LDAP para procurar os dados. Essa abordagem tem a vantagem de não exigir que você reinicie qualquer controlador de domínio no diretório serviços de modo de restauração (DSRM) para examinar o conteúdo do backup do AD DS.  
  
 Para obter mais informações sobre como usar o banco de dados do Active Directory montagem ferramenta, consulte o [ferramenta de montagem de banco de dados do Active Directory Step-by-Step guia](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).  
  
 Você também pode usar o **ntdsutil instantâneo** comando para criar instantâneos do banco de dados do Active Directory. Agendando uma tarefa para criar periodicamente instantâneos, você pode obter cópias adicionais do banco de dados do Active Directory ao longo do tempo. Você pode usar essas cópias para identificar melhor quando ocorreu a falha de toda a floresta e, em seguida, escolha o melhor backup para restaurar. Para criar instantâneos, use a versão do **ntdsutil** que acompanha o Windows Server 2008 ou o servidor remoto administração ferramentas (RSAT) para o Windows Vista ou posterior. O controlador de domínio de destino pode executar qualquer versão do Windows Server. Para obter mais informações sobre como usar o **ntdsutil instantâneo** de comando, consulte [instantâneo](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).  
  
  
## <a name="determining-which-domain-controllers-to-restore"></a>Determinando quais controladores de domínio para restaurar  
 Facilidade do processo de restauração é um fator importante ao decidir qual controlador de domínio para restaurar. É recomendável ter um controlador de domínio dedicado para cada domínio que é o controlador de domínio preferencial para uma restauração. Uma restauração dedicada DC facilita a planejar e executar a recuperação de floresta porque você usa a mesma configuração de fonte que foi usada para executar de forma confiável restaurar testes. Você pode criar scripts de recuperação do e não entrem em conflito com configurações diferentes, como se o controlador de domínio mantém operações funções mestre ou não, ou se ele é um servidor DNS ou GC ou não.  
  
> [!NOTE]
>  Embora não seja recomendável para restaurar um detentor da função mestre de operações para fins de simplicidade, algumas organizações podem optar por restaurar um para outras vantagens. Por exemplo, restaurar o mestre RID pode ajudar a evitar problemas com o gerenciamento de RIDs durante a recuperação.  
  
 Escolha um controlador de domínio que melhor atenda aos seguintes critérios:  
  
-   Um controlador de domínio gravável. Isso é obrigatório.  
  
-   Um controlador de domínio que executa o Windows Server 2012 como uma máquina virtual em um hipervisor que dá suporte a VM-GenerationID. Neste controlador de domínio pode ser usado como uma fonte para clonagem.  
  
-   Um controlador de domínio que seja acessível, fisicamente ou em uma rede virtual e preferencialmente localizados em um datacenter. Dessa forma, você pode facilmente isole-lo da rede durante a recuperação de floresta.  
  
-   Um controlador de domínio que tenha um backup do servidor completo boa. Um bom backup é um backup que possa ser restaurado com êxito, foi tirado alguns dias antes da falha e contém como dados muito úteis possível.  
  
-   Um controlador de domínio que foi um servidor de sistema de nome de domínio (DNS) antes da falha. Isso economiza o tempo necessário para reinstalar o DNS.  
  
-   Se você também usar serviços de implantação do Windows, escolha um controlador de domínio que não está configurada para usar desbloqueio pela rede do BitLocker. Nesse caso, o desbloqueio pela rede do BitLocker não é permitido para ser usado para o primeiro controlador de domínio que restaurar o backup durante a recuperação de uma floresta.  
  
     Desbloqueio de rede do BitLocker como o *somente* protetor de chave *não é possível* ser usado em controladores de domínio em que você tenha implantado o Windows Deployment Services (WDS) porque isso resulta em um cenário onde exige que o primeiro controlador de domínio do Active Directory e o WDS estar funcionando para desbloquear. Mas antes de restaurar o primeiro controlador de domínio, Active Directory ainda não está disponível para o WDS, portanto, não é possível desbloquear.  
  
     Para determinar se um controlador de domínio está configurado para usar desbloqueio pela rede do BitLocker, verifique se um certificado de desbloqueio pela rede é identificado na seguinte chave do registro:  
  
     HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP  
  
 Manter procedimentos de segurança quando a manipulação ou restaurar arquivos de backup que incluem o Active Directory. A necessidade que acompanha a recuperação de floresta inadvertidamente pode levar a contemplando práticas recomendadas de segurança. Para obter mais informações, consulte a seção "Estabelecimento domínio controlador de Backup e restauração estratégias" [guia de práticas recomendadas para proteger Active Directory instalações e operações diárias: Parte II](https://technet.microsoft.com/library/bb727066.aspx).  
  
## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identificar a estrutura de floresta atual e funções de DC  
 Determine a estrutura da floresta atual, identificando todos os domínios na floresta. Faça uma lista de todos os controladores de domínio em cada domínio, particularmente os controladores de domínio que possuem backups e virtualizados controladores de domínio que podem ser uma origem para clonagem. Uma lista de controladores de domínio para domínio raiz da floresta será o mais importante porque você recuperará neste domínio pela primeira vez. Depois de restaurar o domínio raiz da floresta, você pode obter uma lista de outros domínios, controladores de domínio e os sites na floresta usando o snap-ins do Active Directory.  
  
 Prepare uma tabela que mostra as funções de cada controlador de domínio no domínio, conforme mostrado no exemplo a seguir. Isso ajudará você a reverter para a configuração de pré-falha da floresta após a recuperação.  
  
|Nome do controlador de domínio|Sistema Operacional|FSMO|GC|RODC|Backup|DNS|Núcleo do servidor|VM|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|Mestre de esquema, mestre de nomeação de domínio|Sim|Não|Sim|Não|Não|Sim|Sim|  
|DC_2|Windows Server 2012|None|Sim|Não|Sim|Sim|Não|Sim|Sim|  
|DC_3|Windows Server 2012|Infraestrutura mestre|Não|Não|Não|Sim|Sim|Sim|Sim|  
|DC_4|Windows Server 2012|Emulador do PDC, mestre RID|Sim|Não|Não|Não|Não|Sim|Não|  
|DC_5|Windows Server 2012|None|Não|Não|Sim|Sim|Não|Sim|Sim|  
|RODC_1|Windows Server 2008 R2|None|Sim|Sim|Sim|Sim|Sim|Sim|Não|  
|RODC_2|Windows Server 2008|None|Sim|Sim|Não|Sim|Sim|Sim|Não|  
  
 Para cada domínio na floresta, identifique um controlador de domínio gravável único que tenha um backup do banco de dados do Active Directory para esse domínio confiável. Tenha cuidado ao escolher um backup para restaurar um controlador de domínio. Se o dia e a causa da falha aproximadamente são conhecidas, a recomendação geral é usar um backup que foi feito a alguns dias antes dessa data.  
  
 Neste exemplo, há quatro candidatos backup: DC_1, DC_2, DC_4 e DC_5. Esses candidatos de backup, restaure apenas um. O controlador de domínio recomendado é DC_5 pelos seguintes motivos:  
  
-   Ele atende aos requisitos para usá-la como uma fonte para clonagem DC virtualizado, que é, ele executa o Windows Server 2012 como um controlador de domínio virtual em um hipervisor que suporta VM-GenerationID, executa o software que tem permissão para ser clonados (ou que podem ser removidos se não conseguir ser clonados). Após a restauração, a função de emulador do PDC será ser executada em servidor e elas podem ser adicionados ao grupo Cloneable controladores de domínio para o domínio.  
  
-   Ele executa uma instalação completa do Windows Server 2012. Um controlador de domínio que executa uma instalação Server Core pode ser menos conveniente como um destino para a recuperação.  
  
-   É um servidor DNS. Portanto, DNS não precisam ser reinstalados.  
  
> [!NOTE]
>  Como DC_5 não é um servidor de catálogo global, ele também tem uma vantagem que o catálogo global não precisa ser removida após a restauração. Mas se ou não o controlador de domínio também é que um servidor de catálogo global não é um fator decisivo porque a partir do Windows Server 2012, todos os controladores de domínio são servidores de catálogo global por padrão e removendo e adicionando o catálogo global após a restauração é recomendada como parte do processo de recuperação de floresta em qualquer caso.  
  
## <a name="recover-the-forest-in-isolation"></a>Recuperar da floresta isoladamente  
 O cenário preferencial é desligar todos os controladores de domínio graváveis antes do primeiro controlador de domínio restaurado será colocado novamente em produção. Isso garante que todos os dados perigosos não replicam volta à floresta recuperada. É especialmente importante desligar todos os titulares de função mestre de operações.  
  
> [!NOTE]
>  Pode haver casos em que você move o primeiro controlador de domínio que você pretende recuperar para cada domínio a uma rede isolada permitindo outros controladores de domínio permaneçam on-line para minimizar o tempo ocioso do sistema. Por exemplo, se você estiver recuperando de uma atualização de esquema com falhas, você poderá manter controladores de domínio em execução na rede de produção enquanto você executar as etapas de recuperação em isolamento.  
  
 Se você estiver executando virtualizados controladores de domínio, você pode movê-los para uma rede virtual que é isolada da rede de produção em que você executar a recuperação. Mover virtualizados controladores de domínio a uma rede separada oferece dois benefícios:  
  
-   Controladores de domínio recuperados são impedidos de recorrência do problema que causou a recuperação de floresta porque eles estão isolados.  
  
-   Virtualizado DC clonagem pode ser executada na rede separada para que um número crítico de controladores de domínio pode ser executados e testado antes que sejam colocados de volta para a rede de produção.  
  
 Se você estiver executando controladores de domínio em um hardware físico, desconecte o cabo de rede do primeiro controlador de domínio que você pretende restaurar no domínio raiz da floresta. Se possível, também desconecte os cabos de rede de todos os outros controladores de domínio. Isso impede que controladores de domínio replicar, se elas são iniciadas durante o processo de recuperação de floresta acidentalmente.  
  
 Em uma floresta grande que é distribuída por vários locais, pode ser difícil garantir que todos os controladores de domínio graváveis são encerrados. Por esse motivo, as etapas de recuperação — como redefinir as contas de computador e krbtgt, além de limpeza de metadados — são projetados para garantir que os controladores de domínio graváveis recuperados não replicar com controladores de domínio graváveis perigosos (no caso de algumas ainda online na floresta).  
  
 No entanto, apenas colocando controladores de domínio graváveis offline pode você garante que a duplicação não ocorre? Portanto, sempre que possível, você deve implantar tecnologia de gerenciamento remoto que pode ajudá-lo para desligar e isolar fisicamente os controladores de domínio graváveis durante a recuperação de floresta.  
  
 RODCs podem continuar a operar enquanto controladores de domínio graváveis estiverem offline. Nenhum outro DC replicará diretamente todas as alterações de qualquer RODC — especialmente, nenhuma alteração de contêiner de esquema ou configuração — portanto, eles não apresentam o mesmo risco como controladores de domínio graváveis durante a recuperação. Depois que todos os controladores de domínio graváveis são recuperados e online, você deve recriar todos os RODCs.  
  
 RODCs continuará permitir o acesso a recursos locais que são armazenadas em cache em seus respectivos sites enquanto as operações de recuperação estão acontecendo em paralelo. Recursos locais que não são armazenadas em cache no RODC terá solicitações de autenticação encaminhadas para um controlador de domínio gravável. Essas solicitações falhará porque controladores de domínio graváveis estão offline. Algumas operações, como alterações de senha também não funcionarão até que você recuperar controladores de domínio graváveis.  
  
 Se você estiver usando uma arquitetura de rede de hub e spoke, você pode se concentrar primeiro sobre como recuperar os controladores de domínio graváveis os sites de hub. Mais tarde, você pode recriar os RODCs em locais remotos.  
  
## <a name="next-steps"></a>Próximas etapas
-   [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperação de floresta do AD - descobrindo um plano de recuperação personalizada floresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperação de floresta do AD - realizar uma recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperação de floresta do AD - recuperando um único domínio em uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [AD floresta recuperação - floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
