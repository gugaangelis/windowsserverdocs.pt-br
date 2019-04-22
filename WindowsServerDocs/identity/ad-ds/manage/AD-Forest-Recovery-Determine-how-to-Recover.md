---
title: Recuperação de floresta do AD - determinar como recuperar a floresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 30d23d977b4d7cd320d78ff340120df7013dee4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817727"
---
# <a name="determine-how-to-recover-the-forest"></a>Determinar como recuperar a floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Recuperar toda a floresta do Active Directory envolve a restauração de backup ou reinstalar os serviços de domínio do Active Directory (AD DS) em cada controlador de domínio (DC) na floresta. Recuperando a floresta restaura cada domínio na floresta para seu estado no momento do último backup confiável. Consequentemente, a operação de restauração resultará na perda de pelo menos os seguintes dados do Active Directory:

- Todos os objetos (como usuários e computadores) que foram adicionados após o último backup confiável
- Todas as atualizações que foram feitas em objetos existentes, pois confiável para o último backup
- Todas as alterações que foram feitas para a partição de configuração ou a partição de esquema no AD DS (como alterações de esquema) desde o último backup confiável

Para cada domínio na floresta, a senha de uma conta de administrador de domínio deve ser conhecida. Preferencialmente, esta é a senha da conta de administrador interna. Você também deve saber a senha do DSRM para executar uma restauração de estado do sistema de um controlador de domínio. Em geral, é uma boa prática para arquivar a conta de administrador e o histórico de senha do DSRM em um local seguro para desde que os backups forem válidos, ou seja, dentro do período de tempo de vida da marca para exclusão ou o tempo de vida do objeto excluído dentro do período, se o Active Directory reciclar Bin está habilitado. Você também pode sincronizar a senha do DSRM com uma conta de usuário de domínio para torná-lo mais fácil de lembrar. Para obter mais informações, consulte o artigo KB [961320](https://support.microsoft.com/kb/961320). Sincronizar a conta DSRM deve ser feita com antecedência sobre a recuperação de floresta, como parte da preparação.

> [!NOTE]
> A conta de administrador é um membro do grupo Administradores internos, por padrão, assim como os grupos de Admins. do domínio e administradores de empresa. Esse grupo tem controle total de todos os controladores de domínio no domínio.

## <a name="determining-which-backups-to-use"></a>Determinando quais backups usar

Fazer backup de pelo menos dois controladores de domínio graváveis para cada domínio regularmente para que você tenha vários backups a serem escolhidos. Observe que você não pode usar o backup de um controlador de domínio somente leitura (RODC) para restaurar um controlador de domínio gravável. É recomendável que você restaure os controladores de domínio por meio de backups que foram feitos alguns dias antes da ocorrência da falha. Em geral, você deve determinar um equilíbrio entre o quanto são recentes e o safeness dos dados restaurados. Escolhendo um backup mais recente recupera os dados mais úteis, mas ela pode aumentar o risco de reintroduzir dados perigosos na floresta restaurada.

Restaurar backups de estado do sistema depende do sistema operacional e o servidor do backup original. Por exemplo, você não deve restaurar um backup de estado do sistema para um servidor diferente. Nesse caso, você poderá ver o seguinte aviso:

"O backup especificado é de um servidor diferente do atual. Não é recomendável executar uma recuperação de estado do sistema com o backup em um servidor alternativo, pois o servidor pode se tornar inutilizável. Tem certeza de que você deseja usar esse backup para recuperar o servidor atual?"

Se você precisar restaurar o Active Directory para um hardware diferente, crie backups de servidor completo e o plano para executar uma recuperação de servidor completo.

> [!IMPORTANT]
> Começando com o Windows Server 2008, não há suporte para restaurar o backup do estado do sistema para uma nova instalação do Windows Server no novo hardware ou o mesmo hardware. Se o Windows Server é reinstalado no mesmo hardware, conforme recomendado neste guia, você pode restaurar o controlador de domínio nesta ordem:
>
> 1. Execute uma restauração completa do servidor para restaurar o sistema operacional e todos os arquivos e aplicativos.
> 2. Execute uma restauração de estado do sistema usando wbadmin.exe para marcar o SYSVOL como autoritativo.
>
> Para obter mais informações, consulte Microsoft KB artigo [249694](https://support.microsoft.com/kb/249694).

Se a hora da ocorrência da falha é desconhecida, investigar mais para identificar os backups que contêm o último estado de seguro da floresta. Essa abordagem é menos recomendada. Portanto, é altamente recomendável que você mantenha logs detalhados sobre o estado de integridade do AD DS em uma base diária, para que, se houver uma falha de toda a floresta, o tempo aproximado de falha pode ser identificado. Você também deve manter uma cópia local dos backups para permitir uma recuperação mais rápida.

Se a Lixeira do Active Directory estiver habilitada, o tempo de vida de backup é igual do **deletedObjectLifetime** valor ou o **tombstoneLifetime** de valor, o que for menor. Para obter mais informações, consulte [Active Directory Recycle Bin guia passo a passo](https://go.microsoft.com/fwlink/?LinkId=178657) (https://go.microsoft.com/fwlink/?LinkId=178657).

Como alternativa, você também pode usar a ferramenta de montagem do banco de dados do Active Directory (Dsamain.exe) e uma ferramenta de Lightweight Directory Access Protocol (LDAP), como Ldp.exe ou usuários do Active Directory e computadores, para identificar qual backup tem o último estado de seguro das floresta. A ferramenta de montagem do banco de dados do Active Directory, que está incluída no Windows Server 2008 e sistemas de operacionais posteriores do Windows Server, expõe os dados do Active Directory que são armazenados em instantâneos ou backups como um servidor LDAP. Em seguida, você pode usar uma ferramenta LDAP para procurar os dados. Essa abordagem tem a vantagem de não exigir a reinicialização qualquer controlador de domínio no Directory Services Restore DSRM (modo) para examinar o conteúdo do backup do AD DS.

Para obter mais informações sobre como usar o banco de dados do Active Directory ferramenta de montagem, consulte o [guia de passo a passo de ferramenta de montagem do banco de dados Active Directory](https://technet.microsoft.com/library/cc753609\(WS.10\).aspx).

Você também pode usar o **ntdsutil instantâneo** comando para criar instantâneos do banco de dados do Active Directory. Ao agendar uma tarefa para criar instantâneos, periodicamente, você pode obter cópias adicionais do banco de dados do Active Directory ao longo do tempo. Você pode usar essas cópias para identificar melhor quando ocorreu a falha de toda a floresta e, em seguida, escolha o melhor backup para restaurar. Para criar instantâneos, use a versão do **ntdsutil** que acompanha o Windows Server 2008 ou o servidor administração RSAT (ferramentas remoto) para Windows Vista ou posterior. O controlador de domínio de destino pode executar qualquer versão do Windows Server. Para obter mais informações sobre como usar o **instantâneo ntdsutil** de comando, consulte [instantâneo](https://technet.microsoft.com/library/cc731620\(WS.10\).aspx).

## <a name="determining-which-domain-controllers-to-restore"></a>Determinando quais controladores de domínio para restaurar

Facilitar o processo de restauração é um fator importante ao decidir qual controlador de domínio para restaurar. É recomendável ter um controlador de domínio dedicado para cada domínio que é o controlador de domínio preferencial para uma restauração. Uma controlador de domínio torna mais fácil de planejar de forma confiável e executar a recuperação de floresta, porque você usa a mesma configuração de origem que foi usada para executar a restauração dedicada restaurar testes. Você pode gerar script de recuperação e não competem com configurações diferentes, como se o controlador de domínio contém operações de funções de mestre ou não, ou se ele é um servidor GC ou DNS ou não.

> [!NOTE]
> Enquanto não é recomendável restaurar um detentor da função de mestre de operações com o intuito de manter a simplicidade, algumas organizações podem optar por restaurar um para outras vantagens. Por exemplo, restaurar o mestre de RID pode ajudar a evitar problemas com o gerenciamento de RIDs durante a recuperação.  

Escolha um controlador de domínio que melhor atende aos seguintes critérios:

- Um controlador de domínio que pode ser gravada. Isso é obrigatório.

- Um controlador de domínio executando o Windows Server 2012 como uma máquina virtual em um hipervisor que dá suporte a VM-GenerationID. Esse controlador de domínio pode ser usado como uma fonte para clonagem.
- Um controlador de domínio que é acessível, fisicamente ou em uma rede virtual e preferivelmente localizado em um datacenter. Dessa forma, você pode facilmente isolá-lo da rede durante a recuperação de floresta.
- Um controlador de domínio que tenha um backup completo do servidor boa. Um bom backup é um backup que pode ser restaurado com êxito, foi feito alguns dias antes da falha e contém como possível de dados muito útil.
- Um controlador de domínio que era um servidor de sistema de nome de domínio (DNS) antes da falha. Isso economiza o tempo necessário para reinstalar o DNS.
- Se você também pode usar os serviços de implantação do Windows, escolha um controlador de domínio que não está configurado para usar o desbloqueio de rede do BitLocker. Nesse caso, o desbloqueio de rede do BitLocker não tem suporte para ser usado para o primeiro controlador de domínio para restaurar do backup durante a recuperação de floresta.

   Desbloqueio de rede do BitLocker como o *só* protetor de chave *não é possível* ser usado em controladores de domínio em que você tiver implantado o Windows Deployment Services (WDS) porque isso pode resultar em um cenário em que o primeiro controlador de domínio requer O Active Directory e o WDS para ser funcionando a fim de desbloquear. Mas, antes de restaurar o primeiro controlador de domínio, Active Directory ainda não está disponível para o WDS, portanto, ele não é possível desbloquear.

   Para determinar se um controlador de domínio for configurado para usar o desbloqueio de rede do BitLocker, verifique se um certificado de desbloqueio de rede é identificado na seguinte chave do registro:

   HKEY_LOCAL_MACHINESoftwarePoliciesMicrosoftSystemCertificatesFVE_NKP

Manter os procedimentos de segurança ao tratamento ou restaurar arquivos de backup que incluam o Active Directory. A urgência que acompanha a recuperação de floresta, inadvertidamente, pode levar a esquecendo de práticas recomendadas de segurança. Para obter mais informações, consulte a seção intitulada "Estabelecer domínio controlador Backup e restaurar estratégias" no [guia de práticas recomendadas para proteger instalações de diretório Active Directory e as operações diárias: Parte II](https://technet.microsoft.com/library/bb727066.aspx).

## <a name="identify-the-current-forest-structure-and-dc-functions"></a>Identificar as funções de controlador de domínio e a estrutura atual da floresta

Determine a estrutura atual da floresta, identificando todos os domínios na floresta. Faça uma lista de todos os controladores de domínio em cada domínio, especialmente os controladores de domínio que têm backups e controladores de domínio virtualizados que podem ser uma fonte para clonagem. Uma lista de controladores de domínio para o domínio raiz da floresta será o mais importante porque você recuperará esse domínio pela primeira vez. Depois de restaurar o domínio raiz da floresta, você pode obter uma lista de outros domínios, os controladores de domínio e os sites na floresta, usando o snap-ins do Active Directory.

Prepare uma tabela que mostra as funções de cada controlador de domínio no domínio, conforme mostrado no exemplo a seguir. Isso ajudará você a reverter para a configuração da floresta em pré-falha após a recuperação.

|Nome do controlador de domínio|Sistema operacional|FSMO|GC|RODC|Backup|DNS|Server Core|VM|VM-GenID|  
|-------------|----------------------|----------|--------|----------|------------|---------|-----------------|--------|---------------|  
|DC_1|Windows Server 2012|Mestre de esquema, mestre de nomeação de domínio|Sim|Não|Sim|Não|Não|Sim|Sim|  
|DC_2|Windows Server 2012|Nenhuma|Sim|Não|Sim|Sim|Não|Sim|Sim|  
|DC_3|Windows Server 2012|Mestre de Infra-Estrutura|Não|Não|Não|Sim|Sim|Sim|Sim|  
|DC_4|Windows Server 2012|Emulador PDC, mestre de RID|Sim|Não|Não|Não|Não|Sim|Não|  
|DC_5|Windows Server 2012|Nenhuma|Não|Não|Sim|Sim|Não|Sim|Sim|  
|RODC_1|Windows Server 2008 R2|Nenhuma|Sim|Sim|Sim|Sim|Sim|Sim|Não|  
|RODC_2|Windows Server 2008|Nenhuma|Sim|Sim|Não|Sim|Sim|Sim|Não|  

Para cada domínio na floresta, identifique um único DC gravável que tem um backup confiável do banco de dados do Active Directory para esse domínio. Tenha cuidado ao escolher um backup para restaurar um controlador de domínio. Se o dia e a causa da falha de aproximadamente forem conhecidos, a recomendação geral é usar um backup que foi feito a alguns dias antes da data.
  
Neste exemplo, há quatro candidatos de backup: DC_1, DC_2, DC_4 e DC_5. Desses candidatos de backup, restaurar apenas um. O controlador de domínio recomendado é DC_5 pelos seguintes motivos:  

- Ele cumpre requisitos para usá-lo como uma fonte para clonagem de controlador de domínio virtualizado, que é, ele executa o Windows Server 2012 como um controlador de domínio virtual em um hipervisor que dá suporte a VM-GenerationID, executa o software que pode ser clonado (ou que pode ser removido se não conseguir ser clone d). Após a restauração, a função de emulador PDC será ser executada para que o servidor e ele podem ser adicionados ao grupo de controladores de domínio Clonáveis para o domínio.  
- Ele é executado uma instalação completa do Windows Server 2012. Um controlador de domínio que executa uma instalação Server Core pode ser menos conveniente como um destino para recuperação.  
- Ele é um servidor DNS. Portanto, DNS não precisa ser reinstalado.  

> [!NOTE]
> Como DC_5 não é um servidor de catálogo global, ele também tem uma vantagem que o catálogo global não precisa ser removido após a restauração. Mas se o controlador de domínio também é que um servidor de catálogo global não é um fator decisivo porque começando com o Windows Server 2012, todos os controladores de domínio são servidores de catálogo global padrão, removendo e adicionando o catálogo global, após a restauração é recomendada como parte da floresta processo de recuperação de qualquer forma.  

## <a name="recover-the-forest-in-isolation"></a>Recuperar a floresta em isolamento

O cenário preferencial é desligar todos os controladores de domínio graváveis antes do primeiro controlador de domínio restaurado é levado de volta em produção. Isso garante que todos os dados perigosos não replicam volta à floresta recuperada. É particularmente importante desligar todos os proprietários da função de mestre de operações.  

> [!NOTE]
> Pode haver casos em que você move o primeiro controlador de domínio que você planeja recuperar para cada domínio a uma rede isolada, permitindo que os demais DCs permaneçam on-line para minimizar o tempo de inatividade do sistema. Por exemplo, se você estiver recuperando de uma atualização de esquema com falha, você poderá manter os controladores de domínio em execução na rede de produção durante a execução de etapas de recuperação em isolamento.

Se você estiver executando controladores de domínio virtualizados, você pode movê-los a uma rede virtual que é isolada da rede de produção em que você executar a recuperação. Movendo os controladores de domínio virtualizados para uma rede separada oferece dois benefícios:  

- Controladores de domínio recuperados serão impedidos de nova ocorrência do problema que causou a recuperação de floresta como eles são isolados.  
- Clonagem de controlador de domínio virtualizado pode ser executada na rede separada, de forma que um número crítico de controladores de domínio pode estar em execução e testado antes que sejam colocados de volta para a rede de produção.

Se você estiver executando controladores de domínio no hardware físico, desconecte o cabo de rede do primeiro controlador de domínio que você planeja restaurar no domínio raiz da floresta. Se possível, também desconecte os cabos de rede de todos os outros controladores de domínio. Isso impede que os controladores de domínio replicando, se eles forem iniciados acidentalmente durante o processo de recuperação de floresta.  

Em uma floresta grande que é distribuída entre vários locais, pode ser difícil garantir que todos os controladores de domínio graváveis são desligados. Por esse motivo, as etapas de recuperação — como redefinir a conta de computador e conta krbtgt, além para limpeza de metadados — são projetados para garantir que os controladores de domínio graváveis recuperados replica com controladores de domínio graváveis perigosos (no caso de alguns ainda estão online no floresta).  
  
No entanto, apenas colocando os controladores de domínio graváveis offline pode você garantir que a replicação não ocorrerá. Portanto, sempre que possível, você deve implantar a tecnologia de gerenciamento remoto que pode ajudá-lo para desligar e isolar fisicamente os controladores de domínio graváveis durante a recuperação de floresta.  
  
Os RODCs podem continuar a operar enquanto os controladores de domínio graváveis estiverem offline. Nenhum outro controlador de domínio diretamente replicará as alterações de qualquer RODC — especialmente, nenhuma alteração de contêiner de esquema ou configuração — para que eles não apresentam o mesmo risco como controladores de domínio graváveis durante a recuperação. Depois que todos os controladores de domínio graváveis estiverem online e recuperados, você deve recompilar todos os RODCs.  
  
Os RODCs continuará permitir o acesso aos recursos locais são armazenados em cache em seus respectivos locais enquanto as operações de recuperação estão acontecendo em paralelo. Recursos locais que não são armazenadas em cache no RODC terá as solicitações de autenticação encaminhadas para um controlador de domínio gravável. Essas solicitações falhará porque os controladores de domínio graváveis estiverem offline. Algumas operações, como alterações de senha também não funcionará até que você recuperar os controladores de domínio graváveis.  
  
Se você estiver usando uma arquitetura de rede de hub e spoke, você pode concentrar-se primeiro recuperando os controladores de domínio graváveis em sites de hub. Posteriormente, você pode recriar os RODCs em sites remotos.  
  
## <a name="next-steps"></a>Próximas etapas

- [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperação de floresta do AD - elaborar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD - executar a recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
- [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperação de floresta do AD - recuperação de um único domínio dentro de uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperação de floresta do AD - recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
