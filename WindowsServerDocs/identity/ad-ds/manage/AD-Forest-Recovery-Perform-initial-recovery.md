---
title: Recuperação de floresta do AD-executar recuperação inicial
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 7d592198187d44927f643b45e7a8bb4c2eec2a69
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823899"
---
# <a name="perform-initial-recovery"></a>Executar recuperação inicial  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Esta seção inclui as seguintes etapas:  

- [Restaurar o primeiro controlador de domínio gravável em cada domínio](#restore-the-first-writeable-domain-controller-in-each-domain)  
- [Reconecte cada controlador de domínio gravável restaurado à rede](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
- [Adicionar o catálogo global a um controlador de domínio no domínio raiz da floresta](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Restaurar o primeiro controlador de domínio gravável em cada domínio  

A partir de um DC gravável no domínio raiz da floresta, conclua as etapas nesta seção para restaurar o primeiro DC. O domínio raiz da floresta é importante porque armazena os administradores de esquema e os grupos de administradores de empresa. Ele também ajuda a manter a hierarquia de confiança na floresta. Além disso, o domínio raiz da floresta geralmente contém o servidor raiz DNS para o namespace DNS da floresta. Consequentemente, a zona DNS integrada ao Active Directory para esse domínio contém os registros de recurso de alias (CNAME) para todos os outros DCs na floresta (que são necessários para replicação) e os registros de recurso DNS do catálogo global. 
  
Depois de recuperar o domínio raiz da floresta, repita as mesmas etapas para recuperar os domínios restantes na floresta. Você pode recuperar mais de um domínio simultaneamente; no entanto, sempre recupere um domínio pai antes de recuperar um filho para evitar qualquer interrupção na hierarquia de confiança ou resolução de nome DNS. 
  
Para cada domínio que você recuperar, restaure apenas um DC gravável do backup. Essa é a parte mais importante da recuperação porque o DC deve ter um banco de dados que não tenha sido influenciado por qualquer que tenha causado falha na floresta. É importante ter um backup confiável que é completamente testado antes de ser introduzido no ambiente de produção. 
  
Em seguida, execute as etapas a seguir. Os procedimentos para executar determinadas etapas estão em [procedimentos de recuperação de floresta do AD](AD-Forest-Recovery-Procedures.md). 
  
1. Se você planeja restaurar um servidor físico, verifique se o cabo de rede do controlador de domínio de destino não está anexado e, portanto, não está conectado à rede de produção. Para uma máquina virtual, você pode remover o adaptador de rede ou usar um adaptador de rede que está conectado a outra rede em que você pode testar o processo de recuperação enquanto estiver isolado da rede de produção. 
  
2. Como esse é o primeiro DC gravável no domínio, você deve executar uma restauração não autoritativa de AD DS e uma restauração autoritativa do SYSVOL. A operação de restauração deve ser concluída usando um aplicativo de backup e restauração com reconhecimento de Active Directory, como Backup do Windows Server (ou seja, você não deve restaurar o controlador de domínio usando métodos sem suporte, como restaurar um instantâneo de VM). 
   - Uma restauração autoritativa do SYSVOL é necessária porque a replicação da pasta replicada SYSVOL deve ser iniciada após a recuperação de um desastre. Todos os DCs subsequentes adicionados ao domínio devem sincronizar novamente sua pasta SYSVOL com uma cópia da pasta que foi selecionada para ser autoritativa antes que a pasta possa ser anunciada. 

   > [!CAUTION]
   > Execute uma operação de restauração autoritativa (ou primária) do SYSVOL somente para que o primeiro DC seja restaurado no domínio raiz da floresta. A execução incorreta de operações de restauração primária do SYSVOL em outros DCs leva a conflitos de replicação de dados do SYSVOL. 

   - Há duas opções para executar uma restauração não autoritativa de AD DS e uma restauração autoritativa do SYSVOL:  
   - Execute uma recuperação completa do servidor e Force uma sincronização autoritativa do SYSVOL. Para obter procedimentos detalhados, consulte [executando uma recuperação de servidor completo](AD-Forest-Recovery-Perform-a-Full-Recovery.md) e [executar uma sincronização autoritativa de SYSVOL replicado pelo DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md). 
   - Execute uma recuperação de servidor completa seguida por uma restauração de estado do sistema. Essa opção requer que você crie os dois tipos de backups antecipadamente: um backup de servidor completo e um backup de estado do sistema. Para obter procedimentos detalhados, consulte [executando uma recuperação de servidor completo](AD-Forest-Recovery-Perform-a-Full-Recovery.md) e [executando uma restauração não autoritativa de Active Directory Domain Services](AD-Forest-Recovery-Nonauthoritative-Restore.md). 
  
3. Depois de restaurar e reiniciar o controlador de domínio gravável, verifique se a falha não afetou os dados no controlador de domínio. Se os dados do controlador de domínio estiverem danificados, repita a etapa 2 com um backup diferente. 
   - Se o controlador de domínio restaurado hospedar uma função de mestre de operações, talvez seja necessário adicionar a seguinte entrada de registro para evitar que AD DS não estejam disponíveis até que ela tenha concluído a replicação de uma partição de diretório gravável:  

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl executar sincronizações iniciais**  
  
      Crie a entrada com o tipo de dados **REG_DWORD** e um valor de **0**. Depois que a floresta é recuperada completamente, você pode redefinir o valor dessa entrada como **1**, que requer um controlador de domínio que reinicie e mantenha as funções de mestre de operações para ter êxito AD DS a replicação de entrada e saída com seus parceiros de réplica conhecidos antes que ele se anuncie como controlador de domínio e comece a fornecer serviços aos clientes. Para obter mais informações sobre os requisitos de sincronização inicial, consulte o artigo [305476](https://support.microsoft.com/kb/305476)do KB. 
  
      Continue para as próximas etapas somente depois de restaurar e verificar os dados e antes de ingressar este computador na rede de produção. 
  
4. Se você suspeitar que a falha em toda a floresta estava relacionada à invasão de rede ou a um ataque mal-intencionado, redefina as senhas da conta para todas as contas administrativas, incluindo membros dos administradores corporativos, administradores de domínio, administradores de esquema, operadores de servidor, grupos de operadores de contas e assim por diante. A redefinição de senhas de conta administrativa deve ser concluída antes que os controladores de domínio adicionais sejam instalados durante a próxima fase da recuperação de floresta. 
5. No primeiro DC restaurado no domínio raiz da floresta, capture todas as funções de mestre de operações de todo o domínio e de toda a floresta. As credenciais de administradores de empresa e administradores de esquema são necessárias para executar funções de mestre de operações em toda a floresta. 
  
     Em cada domínio filho, execute funções de mestre de operações em todo o domínio. Embora você possa reter as funções de mestre de operações no controlador de domínio restaurado apenas temporariamente, a captura dessas funções garante que o DC as hospede nesse ponto no processo de recuperação de floresta. Como parte do processo de pós-recuperação, você pode redistribuir as funções de mestre de operações conforme necessário. Para obter mais informações sobre como capturar funções de mestre de operações, consulte [capturando uma função de mestre de operações](AD-forest-recovery-seizing-operations-master-role.md). Para obter recomendações sobre onde posicionar as funções de mestre de operações, consulte [o que são mestres de operações?](https://technet.microsoft.com/library/cc779716.aspx). 
  
6. Limpe os metadados de todos os outros DCs graváveis no domínio raiz da floresta que você não está restaurando do backup (todos os DCs graváveis no domínio, exceto para esse primeiro DC). Se você usar a versão do Active Directory usuários e computadores ou Active Directory sites e serviços incluídos no Windows Server 2008 ou posterior ou no RSAT para Windows Vista ou posterior, a limpeza de metadados será executada automaticamente quando você excluir um objeto DC. Além disso, o objeto de servidor e o objeto de computador para o controlador de domínio excluído também são excluídos automaticamente. Para obter mais informações, consulte [limpando metadados de controladores de dados graváveis removidos](AD-Forest-Recovery-Cleaning-Metadata.md). 
  
     Limpar os metadados impede a possível duplicação de objetos de configurações NTDS se AD DS estiver instalado em um controlador de domínio em um site diferente. Potencialmente, isso também poderia salvar o Knowledge Consistency Checker (KCC) o processo de criação de links de replicação quando os próprios DCs não estiverem presentes. Além disso, como parte da limpeza de metadados, os registros de recurso DNS do localizador de DC para todos os outros DCs no domínio serão excluídos do DNS. 
  
     Até que os metadados de todos os outros DCs no domínio sejam removidos, esse DC, se fosse um mestre RID antes da recuperação, não assumirá a função mestre RID e, portanto, não poderá emitir novos RIDs. Você pode ver a ID do evento 16650 no log do sistema em Visualizador de Eventos indicando essa falha, mas você deve ver a ID do evento 16648 indicando êxito um pouco depois de ter limpado os metadados. 
  
7. Se você tiver zonas DNS armazenadas no AD DS, verifique se o serviço do servidor DNS local está instalado e em execução no controlador de domínio que você restaurou. Se esse controlador de domínio não era um servidor DNS antes da falha da floresta, você deve instalar e configurar o servidor DNS. 
  
    > [!NOTE]
    > Se o DC restaurado executar o Windows Server 2008, você precisará instalar o hotfix no artigo [975654](https://support.microsoft.com/kb/975654) do KB ou conectar o servidor a uma rede isolada temporariamente para instalar o servidor DNS. O hotfix não é necessário para outras versões do Windows Server. 
  
     No domínio raiz da floresta, configure o DC restaurado com seu próprio endereço IP (ou um endereço de loopback, como 127.0.0.1) como seu servidor DNS preferencial. Você pode definir essa configuração nas propriedades de TCP/IP do adaptador de rede local (LAN). Esse é o primeiro servidor DNS na floresta. Para obter mais informações, consulte [Configurar TCP/IP para usar o DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     Em cada domínio filho, configure o DC restaurado com o endereço IP do primeiro servidor DNS no domínio raiz da floresta como seu servidor DNS preferencial. Você pode definir essa configuração nas propriedades de TCP/IP do adaptador de LAN. Para obter mais informações, consulte [Configurar TCP/IP para usar o DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     Nas zonas DNS de _msdcs e domínio, exclua os registros NS de DCs que não existem mais após a limpeza de metadados. Verifique se os registros SRV dos DCs limpos foram removidos. Para ajudar a acelerar a remoção do registro SRV do DNS, execute:  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8. Aumente o valor do pool RID disponível por 100.000. Para obter mais informações, consulte [elevando o valor dos pools RID disponíveis](AD-Forest-Recovery-Raise-RID-Pool.md). Se você tiver um motivo para acreditar que a geração do pool de RIDs por 100.000 é insuficiente para sua situação específica, você deve determinar o menor aumento que ainda é seguro de usar. RIDs são um recurso finito que não deve ser usado sem necessidade. 
  
     Se novas entidades de segurança foram criadas no domínio após o horário do backup que você usa para a restauração, essas entidades de segurança podem ter direitos de acesso em determinados objetos. Essas entidades de segurança não existem mais após a recuperação porque a recuperação foi revertida para o backup; no entanto, seus direitos de acesso ainda podem existir. Se o pool RID disponível não for gerado após uma restauração, novos objetos de usuário criados após a recuperação de floresta poderão obter SIDs (IDs de segurança) idênticas e poderão ter acesso a esses objetos, o que não foi originalmente planejado. 
  
     Para ilustrar, considere o exemplo do novo funcionário chamado Amy que foi mencionado na introdução. O objeto de usuário para Amy não existe mais após a operação de restauração porque foi criado após o backup que foi usado para restaurar o domínio. No entanto, todos os direitos de acesso que foram atribuídos a esse objeto de usuário podem persistir após a operação de restauração. Se o SID desse objeto de usuário for reatribuído a um novo objeto após a operação de restauração, o novo objeto obterá esses direitos de acesso. 
  
9. Invalidar o pool RID atual. O pool RID atual é invalidado após uma restauração do estado do sistema. Mas se uma restauração de estado do sistema não for executada, o pool RID atual precisará ser invalidado para impedir que o controlador de domínio restaurado emita novamente os RIDs do pool RID que foi atribuído no momento em que o backup foi criado. Para obter mais informações, consulte [invalidando o pool RID atual](AD-Forest-Recovery-Invaildate-RID-Pool.md). 
  
    > [!NOTE]
    > Na primeira vez que tentar criar um objeto com um SID depois de invalidar o pool RID, você receberá um erro. A tentativa de criar um objeto dispara uma solicitação para um novo pool de RIDs. A repetição da operação é realizada com sucesso porque o novo pool de RIDs será alocado. 
  
10. Redefina a senha da conta do computador desse DC duas vezes. Para obter mais informações, consulte [redefinindo a senha da conta de computador do controlador de domínio](AD-Forest-Recovery-Reset-Computer-Account-DC.md). 
  
11. Redefina a senha krbtgt duas vezes. Para obter mais informações, consulte [redefinindo a senha krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md). 
  
     Como o histórico de senha krbtgt é de duas senhas, Redefina senhas duas vezes para remover a senha original (de falha) do histórico de senha. 
  
    > [!NOTE]
    > Se a recuperação de floresta estiver em resposta a uma violação de segurança, você também poderá redefinir as senhas de confiança. Para obter mais informações, consulte [redefinindo uma senha de confiança em um lado da relação de confiança](AD-Forest-Recovery-Reset-Trust.md). 
  
12. Se a floresta tiver vários domínios e o controlador de domínio restaurado for um servidor de catálogo global antes da falha, desmarque a caixa de seleção **catálogo global** nas propriedades de configurações NTDS para remover o catálogo global do controlador de domínio. A exceção a essa regra é o caso comum de uma floresta com apenas um domínio. Nesse caso, não é necessário remover o catálogo global. Para obter mais informações, consulte [removendo o catálogo global](AD-Forest-Recovery-Remove-GC.md). 
  
     Ao restaurar um catálogo global de um backup mais recente do que outros backups usados para restaurar controladores de domínio em outros domínios, você pode introduzir objetos remanescentes. Considere o exemplo a seguir. No domínio A, o DC1 é restaurado de um backup que foi realizado no tempo T1. No domínio B, o DC2 é restaurado de um backup de catálogo global que foi realizado no momento T2. Suponha que T2 é mais recente do que T1 e alguns objetos foram criados entre T1 e T2. Depois que esses DCs são restaurados, o DC2, que é um catálogo global, mantém os dados mais recentes da réplica parcial do domínio A que o domínio A se mantém. O DC2, nesse caso, mantém objetos remanescentes porque esses objetos não estão presentes no DC1. 
  
     A presença de objetos remanescentes pode levar a problemas. Por exemplo, mensagens de email podem não ser entregues a um usuário cujo objeto de usuário foi movido entre domínios. Depois que você colocar o controlador de domínio ou o servidor de catálogo global desatualizado novamente, as duas instâncias do objeto de usuário aparecerão no catálogo global. Ambos os objetos têm o mesmo endereço de email; Portanto, as mensagens de email não podem ser entregues. 
  
     Um segundo problema é que uma conta de usuário que não existe mais pode ainda aparecer na lista de endereços global. Um terceiro problema é que um grupo universal que não existe mais ainda pode aparecer no token de acesso de um usuário. 
  
     Se você tiver restaurado um controlador de domínio que era um catálogo global, seja inadvertidamente ou porque esse era o backup solitários confiável, recomendamos que você impeça a ocorrência de objetos remanescentes desabilitando o catálogo global logo após a conclusão da operação de restauração. Desabilitar o sinalizador de catálogo global fará com que o computador perca todas as suas réplicas parciais (partições) e se reabilite para o status normal do DC. 
  
13. Configure o serviço de tempo do Windows. No domínio raiz da floresta, configure o emulador do PDC para sincronizar a hora de uma fonte de tempo externa. Para obter mais informações, consulte [Configurar o serviço de tempo do Windows no emulador do PDC no domínio raiz da floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29). 
  
## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Reconecte cada controlador de domínio gravável restaurado a uma rede comum

Nesse estágio, você deve ter um DC restaurado (e as etapas de recuperação executadas) no domínio raiz da floresta e em cada um dos domínios restantes. Junte esses controladores de domínio a uma rede comum isolada do restante do ambiente e conclua as etapas a seguir para validar a integridade e a replicação da floresta.

> [!NOTE]
> Ao unir os DCs físicos a uma rede isolada, talvez seja necessário alterar seus endereços IP. Como resultado, os endereços IP dos registros DNS estarão errados. Como um servidor de catálogo global não está disponível, as atualizações dinâmicas seguras para o DNS falharão. Os DCs virtuais são mais vantajosos nesse caso porque podem ser Unidos a uma nova rede virtual sem alterar seus endereços IP. Essa é uma das razões pelas quais os DCs virtuais são recomendados como os primeiros controladores de domínio a serem restaurados durante a recuperação da floresta. 
  
Após a validação, ingresse os controladores de domínio na rede de produção e conclua as etapas para verificar a integridade da replicação da floresta.

- Para corrigir a resolução de nomes, crie registros de delegação de DNS e configure as dicas de encaminhamento e raiz de DNS conforme necessário. Execute **repadmin/replsum** para verificar a replicação entre os DCS. 
- Se os controladores de domínio restaurados não forem parceiros de replicação direta, a recuperação de replicação será muito mais rápida criando objetos de conexão temporários entre eles. 
- Para validar a limpeza de metadados, execute **repadmin/viewlist \\** * para obter uma lista de todos os DCS na floresta. Execute **nltest/DCList:** *<\>de domínio* para obter uma lista de todos os DCS no domínio. 
- Para verificar a integridade do DC e do DNS, execute DCDiag/v para relatar erros em todos os DCs na floresta. 

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Adicionar o catálogo global a um controlador de domínio no domínio raiz da floresta

Um catálogo global é necessário por esses e outros motivos:  
  
- Para habilitar logons para usuários. 
- Para habilitar o serviço de logon de rede em execução nos DCs em cada domínio filho para registrar e remover registros no servidor DNS no domínio raiz. 
  
Embora seja preferível que o controlador de domínio raiz da floresta se torne um catálogo global, é possível escolher qualquer um dos DCs restaurados para se tornar um catálogo global. 
  
> [!NOTE]
> Um controlador de domínio não será anunciado como um servidor de catálogo global até que tenha concluído uma sincronização completa de todas as partições de diretório na floresta. Portanto, o DC deve ser forçado a replicar com cada um dos controladores de domínio restaurados na floresta. 
>
> Monitore o log de eventos do serviço de diretório em Visualizador de Eventos para a ID de evento 1119, que indica que esse DC é um servidor de catálogo global ou verifique se a seguinte chave do registro tem um valor de 1:  
>
> **Promoção do catálogo do HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global concluída**  
  
Para obter mais informações, consulte [adicionando o catálogo global](AD-Forest-Recovery-Add-GC.md). 
  
Nesse estágio, você deve ter uma floresta estável, com um DC para cada domínio e um catálogo global na floresta. Você deve fazer um novo backup de cada um dos DCs que acabou de restaurar. Agora você pode começar a reimplantar outros controladores de domínio na floresta instalando AD DS. 

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

- [Recuperação de floresta do AD – Pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperação de floresta do AD-planejar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD – identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD-determine como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD-executar recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)  
- [Recuperação de floresta do AD-perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperação de floresta do AD-recuperando um único domínio em uma floresta de multidomínio](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperação de floresta do AD-recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
