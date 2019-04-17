---
title: "Recuperação de floresta do AD - realizar uma recuperação inicial"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: fc2ec09c5b96b76229d532adc6a254c8108d8940
ms.sourcegitcommit: ac73f0f0dca04be731b928183bfdffc97d82c277
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="perform-initial-recovery"></a>Executar a recuperação inicial  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Esta seção inclui as seguintes etapas:  
  
-   [Restaurar o primeiro controlador de domínio gravável em cada domínio](#Restore-the-first-writeable-domain-controller-in-each-domain)  

-   [Reconecte cada controlador de domínio gravável restaurado à rede](#Reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
  
-   [Adicionar o catálogo global para um controlador de domínio no domínio raiz da floresta](#Add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  
  
  
## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Restaurar o primeiro controlador de domínio gravável em cada domínio  
 Começando com um controlador de domínio gravável no domínio raiz da floresta, conclua as etapas nesta seção para restaurar o primeiro controlador de domínio. Domínio raiz da floresta é importante porque ele armazena os grupos Administradores de esquema e administradores corporativos. Ele também ajuda a manter a hierarquia de confiança da floresta. Além disso, o domínio raiz da floresta normalmente contém o servidor DNS raiz para o namespace DNS da floresta. Consequentemente, a zona DNS integrada ao Active Directory para esse domínio contém os registros de recurso de alias (CNAME) para todos os outros controladores de domínio na floresta (que são necessárias para replicação) e os registros de recurso DNS do catálogo global.  
  
 Depois que você recupera domínio raiz da floresta, repita as mesmas etapas para recuperar os domínios na floresta restantes. Você pode recuperar mais de um domínio simultaneamente; No entanto, sempre recupere um domínio pai antes de recuperar um filho para impedir qualquer interrupção na hierarquia de confiança ou resolução de nomes DNS.  
  
 Para cada domínio que você recuperar, restaure somente um controlador de domínio gravável de backup. Isso é a parte mais importante da recuperação porque o controlador de domínio deve ter um banco de dados que não foram influenciado pela tudo o que causou a floresta falhar. É importante ter um backup confiável que é completamente testado antes que ele é apresentado no ambiente de produção.  
  
 Em seguida, execute as seguintes etapas. Procedimentos para executar algumas etapas estão em [recuperação de floresta do AD - procedimentos ](AD-Forest-Recovery-Procedures.md).  
  
1.  Se você pretende restaurar um servidor físico, certifique-se de que o cabo de rede do destino DC não está conectado e, portanto, não estiver conectado à rede de produção. Para uma máquina virtual, você pode remover o adaptador de rede ou usar um adaptador de rede está conectado a outra rede onde você pode testar o processo de recuperação enquanto isolado da rede de produção.  
  
2.  Como esse é o primeiro controlador de domínio gravável no domínio, você deve executar uma restauração não autorizada do AD DS e uma restauração autorizada do SYSVOL. A operação de restauração deve ser concluído usando um backup compatível com o Active Directory e restaurar o aplicativo, como o Backup do Windows Server (ou seja, você não deve restaurar o controlador de domínio usando métodos sem suporte, como restaurar um instantâneo VM).  
  
     Tipo de restauração do SYSVOL é necessário porque replicação do SYSVOL replicados pasta deve ser iniciada após a recuperação de desastres. Todos os controladores de domínio subsequentes são adicionados no domínio devem sincronizar sua pasta SYSVOL com uma cópia da pasta que foi selecionada para ser autoritativo antes que a pasta pode ser anunciada.  
  
    > [!CAUTION]
    >  Execute uma operação de restauração autoritativo (ou primário) SYSVOL somente para o primeiro controlador de domínio a serem restauradas no domínio raiz da floresta. Incorretamente realizando operações de restauração primária do SYSVOL em outros controladores de domínio leva a conflitos de replicação de dados SYSVOL.  
  
     Há duas opções de executam uma restauração não autorizada do AD DS e uma restauração autorizada do SYSVOL:  
  
    -   Realizar uma recuperação do servidor completo e, em seguida, em seguida, forçar uma sincronização de SYSVOL autoritativa. Para obter os procedimentos detalhados, consulte [realizar uma recuperação de servidor completo](AD-Forest-Recovery-Perform-a-Full-Recovery.md) e [execute uma sincronização autoritativa das SYSVOL replicados DFSR ](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  
  
    -   Realizar uma recuperação do servidor completo seguida por uma restauração de estado do sistema. Essa opção requer que você crie os dois tipos de backups antecipadamente: backup completo do servidor e um backup do estado do sistema. Para obter os procedimentos detalhados, consulte [realizar uma recuperação de servidor completo](AD-Forest-Recovery-Perform-a-Full-Recovery.md) e [executar uma restauração sem autorização dos serviços de domínio do Active Directory ](AD-Forest-Recovery-Nonauthoritative-Restore.md).  
  
3.  Depois de restaurar e reiniciar o controlador de domínio gravável, verifique se que a falha não afeta os dados no controlador de domínio. Se os dados do controlador de domínio estão danificados, em seguida, repita a etapa 2, com um backup diferente.  
  
     Se o controlador de domínio restaurado hospeda uma função mestre de operações, talvez seja necessário adicionar a seguinte entrada do registro para evitar o AD DS não estando disponíveis até o término de replicação de uma partição de diretório gravável:  
  
     **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl executar sincronizações iniciais**  
  
     Crie a entrada com o tipo de dados **REG_DWORD** e um valor de **0**. Depois de floresta é totalmente recuperada, você pode redefinir o valor dessa entrada para **1**, que requer um controlador de domínio que será reiniciado e mantém operações funções mestre de ter êxito entrada do AD DS e serviços de duplicação de saída com seus parceiros réplica conhecidos antes de ele notifique a mesmo como controlador de domínio e começa a fornecer aos clientes. Para obter mais informações sobre os requisitos de sincronização inicial, consulte o artigo KB [305476](https://support.microsoft.com/kb/305476).  
  
     Continue para as próximas etapas somente depois que você restaure e verifique se os dados e antes de adicionar este computador na rede de produção.  
  
4.  Se você suspeitar que a falha de toda a floresta foi relacionada a invasões de rede ou ataques mal-intencionados, redefina as senhas de conta de todas as contas administrativas, incluindo membros a administradores corporativos, Admins. do domínio, administradores de esquema, operadores de servidor, grupos de operadores de conta e assim por diante. A redefinição de senhas de conta administrativa deverão ser concluída antes instalados durante a próxima fase da recuperação floresta controladores de domínio adicional.  
  
5.  No primeiro restaurado controlador de domínio no domínio raiz da floresta, execute todas as funções mestre de operações de todo o domínio e toda a floresta. Administradores de empresa e administradores de esquema de credenciais são necessárias para executar funções mestre de operações de toda a floresta.  
  
     Em cada domínio filho, capture funções mestre de operações de todo o domínio. Embora você pode manter as funções mestre de operações no DC restaurado apenas temporariamente, capturando essas funções garante a você sobre quais DC hospeda-los neste momento no processo de recuperação de floresta. Como parte do seu processo de pós-recuperação, você pode redistribuir funções mestre de operações conforme necessário. Para saber mais sobre funções mestre de operações de captura, consulte [capturar uma função mestre de operações](AD-forest-recovery-seizing-operations-master-role.md). Para obter recomendações sobre onde colocar funções mestre de operações, consulte [quais são mestres de operações?](https://technet.microsoft.com/en-us/library/cc779716.aspx).  
  
6.  Limpe os metadados de todos os outros controladores de domínio graváveis no domínio raiz da floresta que não são restaurados de backup (todos os graváveis controladores de domínio no domínio, exceto essa primeira DC). Se você usa a versão do Active Directory usuários e computadores ou Sites do Active Directory e serviços que está incluído no Windows Server 2008 ou posterior ou RSAT para o Windows Vista ou posterior, a limpeza de metadados é executada automaticamente quando você exclui um objeto DC. Além disso, o objeto de servidor e o objeto de computador para o controlador de domínio excluído também serão excluídos automaticamente. Para obter mais informações, consulte [limpeza metadados de controladores de domínio graváveis removido](AD-Forest-Recovery-Cleaning-Metadata.md).  
  
     Limpeza dos metadados evita a duplicação possível de objetos de configurações NTDS se AD DS estiver instalado em um controlador de domínio em um site diferente. Potencialmente, isso também salvasse o verificador de consistência de Conhecimento (KCC) o processo de criação de links de replicação quando os controladores de domínio propriamente ditas não podem estar presentes. Além disso, como parte da limpeza de metadados, registros DNS do localizador de controlador de domínio para todos os outros controladores de domínio no domínio serão excluídos do DNS.  
  
     Até que os metadados de todos os outros controladores de domínio no domínio seja removido, esse controlador de domínio, se ele fosse um mestre RID antes da recuperação, não assumirá a função mestre RID e, portanto, não será capaz de emitir RIDs novo. Você pode ver a ID de evento 16650 no log do sistema no Visualizador de eventos, indicando que essa falha, mas você deve ver a ID de evento 16648 indicando sucesso um pouco depois que tiver limpado os metadados.  
  
7.  Se você tiver zonas DNS que são armazenadas no AD DS, certifique-se de que o serviço de servidor DNS local está instalado e em execução no controlador de domínio que você restaurou. Se esse controlador de domínio não era um servidor DNS antes da falha de floresta, você deve instalar e configurar o servidor DNS.  
  
    > [!NOTE]
    >  Se o controlador de domínio restaurado executa o Windows Server 2008, você precisa instalar o hotfix no artigo KB [975654](https://support.microsoft.com/kb/975654) ou vincular o servidor a uma rede isolada temporariamente para instalar o servidor DNS. O hotfix não é necessário para outras versões do Windows Server.  
  
     No domínio raiz da floresta, configure o controlador de domínio restaurado com seu próprio endereço IP (ou um endereço de loopback, como 127.0.0.1) como seu servidor DNS preferencial. Você pode definir essa configuração nas propriedades de TCP/IP do adaptador de rede local (LAN). Este é o primeiro servidor DNS na floresta. Para obter mais informações, consulte [configurar o TCP/IP para usar o DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx).  
  
     Em cada domínio filho, configure o controlador de domínio restaurado com o endereço IP do servidor DNS primeiro no domínio raiz da floresta como seu servidor DNS preferencial. Você pode definir esta configuração das propriedades de TCP/IP de adaptador de rede local. Para obter mais informações, consulte [configurar o TCP/IP para usar o DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx).  
  
     Nas zonas msdcs e domínio DNS, exclua registros NS de controladores de domínio que não existem mais após a limpeza de metadados. Verifique se os registros SRV de controladores de domínio limpas foram removidos. Para ajudar a acelerar a remoção de registros SRV do DNS, execute:  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8.  Aumente o valor do pool RID disponível pelo 100.000. Para obter mais informações, consulte [aumentando o valor de pools RID disponíveis](AD-Forest-Recovery-Raise-RID-Pool.md). Se você tiver motivos para acreditar que elevar o Pool RID 100.000 seja insuficiente para sua situação específica, você deve determinar o aumento de menor que seja seguro ainda usar. RIDs são um recurso finito não deve ser usado para cima desnecessariamente.  
  
     Se novos objetos de segurança foram criados no domínio após o tempo do backup que você usa para a restauração, essas entidades de segurança podem ter direitos de acesso em determinados objetos. Essas entidades de segurança não existem mais após a recuperação porque a recuperação foi revertido para o backup; No entanto, seus direitos de acesso ainda podem existir. Se o pool RID disponível não é gerado após uma restauração, o novo usuário objetos que são criados após a recuperação de floresta pode obter IDs de idênticos de segurança (SIDs) e pode ter acesso a esses objetos, que não foi originalmente destinado.  
  
     Para ilustrar, considere o exemplo do funcionário novo denominado Amy foi mencionada na introdução. O objeto de usuário para Amy não existe mais após a operação de restauração porque ele foi criado após o backup que foi usado para restaurar o domínio. No entanto, quaisquer direitos de acesso que foram atribuídos a esse objeto de usuário podem persistir após a operação de restauração. Se o SID para esse objeto de usuário é reatribuído a um novo objeto após a operação de restauração, o novo objeto obteria os direitos de acesso.  
  
9. Invalide o pool RID atual. O pool RID atual é invalidado depois de restaurar o estado do sistema. Mas se uma restauração de estado do sistema não foi executada, o pool RID atual precisa ser invalidados para impedir que o controlador de domínio restaurado reenviar RIDs do pool RID que foi atribuído ao tempo que o backup foi criado. Para obter mais informações, consulte [invalidar o pool RID atual](AD-Forest-Recovery-Invaildate-RID-Pool.md).  
  
    > [!NOTE]
    >  Na primeira vez que você tentar criar um objeto com um SID depois invalidar o pool RID, você receberá um erro. A tentativa de criar um objeto dispara uma solicitação para um novo pool RID. Repetição da operação de êxito porque o novo pool RID será alocado.  
  
10. Redefina a senha de conta de computador desse DC duas vezes. Para obter mais informações, consulte [redefinir a senha da conta de computador do controlador de domínio](AD-Forest-Recovery-Reset-Computer-Account-DC.md).  
  
11. Redefina a senha krbtgt duas vezes. Para obter mais informações, consulte [redefinir a senha krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md).  
  
     Como o histórico de senhas krbtgt é duas senhas, redefina senhas duas vezes para remover a senha (prefailure) original do histórico de senhas.  
  
    > [!NOTE]
    >  Se a recuperação de floresta seja em resposta a uma violação de segurança, você também pode redefinir as senhas de confiança. Para obter mais informações, consulte [redefinir uma senha de confiança em um lado da relação de confiança](AD-Forest-Recovery-Reset-Trust.md).  
  
12. Se a floresta possui vários domínios e o controlador de domínio restaurado era um servidor de catálogo global antes da falha, limpe o **Catálogo Global** caixa de seleção nas propriedades configurações NTDS para remover o catálogo global do controlador de domínio. A exceção a essa regra é o caso comum de uma floresta com apenas um domínio. Nesse caso, não é necessário remover o catálogo global. Para obter mais informações, consulte [remover o catálogo global](AD-Forest-Recovery-Remove-GC.md).  
  
     Restaurando um catálogo global de um backup que é mais recentes do que outros backups são usados para restaurar controladores de domínio em outros domínios, você pode introduzir objetos remanescentes. Considere o exemplo a seguir. No domínio A, DC1 é restaurado de um backup que foi tirado tempo T1. No domínio B, DC2 é restaurado de um backup de catálogo global que foi tirado tempo T2. Suponha que T2 é mais recente que T1 e alguns objetos foram criados entre T1 e T2. Depois que esses controladores de domínio são restaurados, o DC2, que é um catálogo global, retém dados mais recentes de réplica parcial do domínio que mantém um domínio em si. DC2, nesse caso, contém objetos remanescentes porque esses objetos não estão presentes em DC1.  
  
     A presença de objetos remanescentes pode causar problemas. Por exemplo, mensagens de email não podem ser entregue a um usuário cujo objeto de usuário foi movido entre domínios. Depois que você reduza o DC desatualizado ou servidor de catálogo global online, ambas as instâncias do objeto de usuário aparecem no catálogo global. Ambos os objetos têm o mesmo endereço de email; Portanto, mensagens de email não podem ser entregues.  
  
     Um segundo problema é que uma conta de usuário que não existe mais ainda pode aparecer na lista de endereços global. Um terceiro problema é que um grupo universal que não existe mais ainda pode aparecer no token de acesso do usuário.  
  
     Se você restaurar um controlador de domínio que foi um catálogo global — seja inadvertidamente ou porque foi o backup solitária que você confia — nós recomendamos que você impedir a ocorrência de remanescente objetos desabilitando o catálogo global logo após a operação de restauração for concluída. Desabilitar o sinalizador de catálogo global resultará no computador perder todas as suas réplicas parciais (partições) e manter-se ao status de DC normal.  
  
13. Configure o serviço de tempo do Windows. No domínio raiz da floresta, configure o emulador do PDC para sincronizar o tempo de uma fonte de horário externo. Para obter mais informações, consulte [configurar o serviço de tempo do Windows no emulador do PDC no domínio raiz da floresta](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
 

## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Reconecte cada controlador de domínio gravável restaurado a uma rede comum  
 Nesse estágio você deve ter um controlador de domínio restaurado (e as etapas de recuperação realizadas) no domínio raiz da floresta e em cada um dos domínios restantes. Participe esses controladores de domínio a uma rede comum que é isolada do resto do ambiente e conclua as seguintes etapas para validar a integridade de floresta e replicação.  
  
> [!NOTE]
>  Quando você ingressar físicos controladores de domínio a uma rede isolada, talvez seja necessário alterar seus endereços IP. Como resultado, os endereços IP dos registros DNS será errados. Como um servidor de catálogo global não está disponível, as atualizações dinâmicas do DNS falhará. Controladores de domínio virtuais são mais vantajosas neste caso porque pode ser adicionados a uma nova rede virtual sem alterar seus endereços IP. Esse é um motivo por que controladores de domínio virtuais são recomendados como controladores de domínio primeiro a serem restauradas durante a recuperação de floresta.  
  
 Após a validação, ingresse controladores de domínio da rede de produção e conclua as etapas para verificar a integridade da replicação de floresta.  
  
-   Para corrigir a resolução de nomes, crie registros de delegação DNS e configurar DNS dicas de encaminhamento e raiz conforme necessário. Executar **repadmin /replsum** verificar replicação entre controladores de domínio.  
  
-   Se o controlador de domínio restaurado não é parceiros de replicação direta, recuperação de replicação será muito mais rápida através da criação de objetos temporários conexão entre eles.  
  
-   Para validar a limpeza de metadados, execute **Repadmin /viewlist \ *** para obter uma lista de todos os controladores de domínio na floresta. Executar **Nltest /DCList:***< domain\ >* para obter uma lista de todos os controladores de domínio no domínio.  
  
-   Para verificar a integridade do controlador de domínio e DNS, execute DCDiag /v para relatar erros em todos os controladores de domínio na floresta.  
  
  

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Adicionar o catálogo global para um controlador de domínio no domínio raiz da floresta  
 Um catálogo global é necessário para esses e outros motivos:  
  
-   Para habilitar logons para os usuários.  
  
-   Para habilitar o serviço de Logon de rede em execução em controladores de domínio em cada domínio filho para registrar e remover registros no servidor DNS no domínio raiz.  
  
 Embora seja preferencial que a raiz da floresta DC se tornar um catálogo global, é possível optar por qualquer um dos controladores de domínio restaurados para se tornar um catálogo global.  
  
> [!NOTE]
>  Um controlador de domínio não será divulgado como um servidor de catálogo global até concluir uma sincronização completa de todas as partições de diretório na floresta. Portanto, o controlador de domínio deve ser forçado a replicar com cada uma das restaurados controladores de domínio na floresta.  
>   
>  Monitorar o serviço de diretório log de eventos no Visualizador de eventos para o evento 1119 ID, que indica que essa DC é um servidor de catálogo global, ou verifique se que a seguinte chave do registro tem um valor de 1:  
>   
>  **Promoção de catálogo HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global completa**  
  
 Para obter mais informações, consulte [adicionando o catálogo global](AD-Forest-Recovery-Add-GC.md).  
  
 Nesse estágio, você deve ter uma floresta estável, com um controlador de domínio para cada domínio e um catálogo global na floresta. Você deve fazer um novo backup de cada um dos controladores de domínio que você restaurou. Agora você pode começar a reimplantá outros controladores de domínio na floresta instalando o AD DS.  

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
  
