---
title: Recuperação de floresta do AD - executar a recuperação inicial
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 9883d337520c3920f8638ddfe5f6bd393e31fd2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442861"
---
# <a name="perform-initial-recovery"></a>Executar recuperação inicial  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Esta seção inclui as seguintes etapas:  

- [Restaurar o primeiro controlador de domínio gravável em cada domínio](#restore-the-first-writeable-domain-controller-in-each-domain)  
- [Reconectar-se cada controlador de domínio gravável restaurado à rede](#reconnect-each-restored-writeable-domain-controller-to-a-common-network)  
- [Adicionar o catálogo global para um controlador de domínio no domínio raiz da floresta](#add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain)  

## <a name="restore-the-first-writeable-domain-controller-in-each-domain"></a>Restaurar o primeiro controlador de domínio gravável em cada domínio  

Começando com um controlador de domínio gravável no domínio raiz da floresta, conclua as etapas nesta seção para restaurar o primeiro controlador de domínio. Domínio raiz da floresta é importante porque ele armazena os grupos de administradores de esquema e administradores de empresa. Ele também ajuda a manter a hierarquia de confiança da floresta. Além disso, o domínio raiz da floresta normalmente contém o servidor DNS raiz de namespace do DNS da floresta. Consequentemente, a zona DNS integrada ao Active Directory para esse domínio contém os registros de recurso de alias (CNAME) para todos os outros controladores de domínio da floresta (que são necessários para replicação) e os registros de recursos DNS do catálogo global. 
  
Depois de recuperar o domínio raiz da floresta, repita as mesmas etapas para recuperar os domínios restantes na floresta. Você pode recuperar mais de um domínio ao mesmo tempo; No entanto, sempre recupera um domínio pai antes de recuperar um filho para evitar qualquer interrupção na hierarquia de confiança ou resolução de nomes DNS. 
  
Para cada domínio que você recupere, restaure somente um controlador de domínio gravável do backup. Essa é a parte mais importante da recuperação porque o controlador de domínio deve ter um banco de dados que não tem sido influenciado por tudo o que causou a floresta falhe. É importante ter um backup confiável que é totalmente testado antes de ser inserido no ambiente de produção. 
  
Em seguida, execute as seguintes etapas. Procedimentos para executar determinadas etapas estão na [recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md). 
  
1. Se você planeja restaurar um servidor físico, certifique-se de que o cabo de rede de destino do controlador de domínio não está anexado e, portanto, não está conectado à rede de produção. Para uma máquina virtual, você pode remover o adaptador de rede ou usar um adaptador de rede que está anexado a outra rede no qual você pode testar o processo de recuperação enquanto isolada da rede de produção. 
  
2. Como esse é o primeiro controlador de domínio gravável no domínio, você deve executar uma restauração não autoritativa de AD DS e uma restauração autoritativa do SYSVOL. A operação de restauração devem ser concluídas usando o Active Directory com suporte a backup e restaurar o aplicativo, como o Backup do Windows Server (ou seja, você não deve restaurar o controlador de domínio por meio de métodos sem suporte, como restaurar um instantâneo VM). 
   - Uma restauração autoritativa do SYSVOL é necessária porque a replicação do SYSVOL replicado pasta deve ser iniciada após a recuperação de desastres. Todos os DCs subsequentes que são adicionados no domínio devem sincronizar novamente a sua pasta SYSVOL com uma cópia da pasta que foi selecionada para ser autorizada antes que a pasta pode ser anunciada. 

   > [!CAUTION]
   > Execute uma operação de restauração autoritativa (ou primária) do SYSVOL somente para o primeiro controlador de domínio a ser restaurado no domínio raiz da floresta. Incorretamente executando operações de restauração primária do SYSVOL em outros controladores de domínio leva a conflitos de replicação de dados SYSVOL. 

   - Há duas opções de realizar uma restauração não autoritativa de AD DS e uma restauração autoritativa do SYSVOL:  
   - Executar uma recuperação de servidor completo e, em seguida, forçar uma sincronização autoritativa do SYSVOL. Para obter procedimentos detalhados, consulte [realiza uma recuperação de servidor completo](AD-Forest-Recovery-Perform-a-Full-Recovery.md) e [executar uma sincronização autoritativa de SYSVOL replicado por DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md). 
   - Execute uma recuperação de servidor completo seguida por uma restauração de estado do sistema. Essa opção exige que você crie os dois tipos de backups de antemão: um backup completo do servidor e um backup de estado do sistema. Para obter procedimentos detalhados, consulte [realiza uma recuperação de servidor completo](AD-Forest-Recovery-Perform-a-Full-Recovery.md) e [executar uma restauração não autoritativa de serviços de domínio do Active Directory](AD-Forest-Recovery-Nonauthoritative-Restore.md). 
  
3. Depois de restaurar e reiniciar o controlador de domínio gravável, verifique se a falha não afetou os dados no controlador de domínio. Se os dados do controlador de domínio estiver danificados, em seguida, repita a etapa 2, com um backup diferente. 
   - Se o controlador de domínio restaurado hospeda uma função de mestre de operações, você precisa adicionar a seguinte entrada do registro para evitar o AD DS que está sendo indisponível até que ele tenha concluído a replicação de uma partição de diretório gravável:  

      **HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Repl executar sincronizações iniciais**  
  
      Crie a entrada com o tipo de dados **REG_DWORD** e um valor de **0**. Depois que a floresta está totalmente recuperada, você pode redefinir o valor desta entrada para **1**, que requer um controlador de domínio é reiniciado e de entrada contém funções de mestre de operações para ter êxito AD DS e replicação de saída com seu conhecido parceiros da réplica antes que ele anuncia a mesmo como controlador de domínio e começará a fornecer serviços aos clientes. Para obter mais informações sobre os requisitos de sincronização inicial, consulte o artigo KB [305476](https://support.microsoft.com/kb/305476). 
  
      Continue para as próximas etapas somente depois que você restaure e verifique se os dados e antes de adicionar este computador na rede de produção. 
  
4. Se você suspeitar que a falha de toda a floresta estava relacionada à rede intrusão ou ataque mal-intencionado, redefina as senhas de conta para todas as contas administrativas, incluindo membros de administradores de empresa, Admins. do domínio, administradores de esquema, operadores de servidor, conta Grupos de operadores e assim por diante. A redefinição de senhas de contas administrativas deve ser concluída antes de controladores são instalados durante a próxima fase da recuperação de floresta de domínio adicional. 
5. No primeiro controlador de domínio restaurado no domínio raiz da floresta, execute todas as funções de mestre de operações de domínio e floresta. As credenciais de administradores de empresa e administradores de esquema são necessárias para executar funções de mestre de operações de toda a floresta. 
  
     Em cada domínio filho, execute funções de mestre de operações de todo o domínio. Embora você pode manter as funções mestre de operações para o controlador de domínio restaurado apenas temporariamente, como executar essas funções garante a você em relação à qual DC hospeda-los no momento no processo de recuperação de floresta. Como parte de seu processo após a recuperação, você pode redistribuir as funções de mestre de operações, conforme necessário. Para obter mais informações sobre como executar funções de mestre de operações, consulte [captura de uma função de mestre de operações](AD-forest-recovery-seizing-operations-master-role.md). Para obter recomendações sobre onde colocar as funções de mestre de operações, consulte [quais são os mestres de operações?](https://technet.microsoft.com/library/cc779716.aspx). 
  
6. Limpe os metadados de todos os outros controladores de domínio graváveis no domínio raiz da floresta que não estão sendo restaurados do backup (todos os DCs graváveis no domínio, exceto o primeiro controlador de domínio). Se você usar a versão de usuários do Active Directory e computadores ou Sites do Active Directory e serviços que está incluído no Windows Server 2008 ou posterior ou RSAT para o Windows Vista ou versões posteriores, a limpeza de metadados é executada automaticamente quando você exclui um objeto de controlador de domínio. Além disso, o objeto de servidor e o objeto de computador para o controlador de domínio excluído também são excluídos automaticamente. Para obter mais informações, consulte [limpeza de metadados de removido controladores de domínio graváveis](AD-Forest-Recovery-Cleaning-Metadata.md). 
  
     Limpeza de metadados de possíveis eliminação de duplicação de objetos de configurações NTDS impede que se o AD DS é instalado em um controlador de domínio em um site diferente. Potencialmente, isso também pode salvar o Knowledge Consistency Checker (KCC) o processo de criação de links de replicação quando os controladores de domínio em si podem não estar presentes. Além disso, como parte da limpeza de metadados, registros DNS do localizador de controlador de domínio para todos os outros controladores de domínio no domínio serão excluídos do DNS. 
  
     Até que os metadados de todos os outros controladores de domínio no domínio é removido, esse controlador de domínio, se ele fosse um mestre de RID antes da recuperação, não assumirá a função de mestre de RID e, portanto, não poderá emitir novos RIDs. Você pode ver a ID do evento 16650 no log do sistema no Visualizador de eventos que indica essa falha, mas você deve ver a ID de evento 16648 indicando sucesso um pouco após a limpeza de metadados. 
  
7. Se você tiver zonas DNS que são armazenadas no AD DS, certifique-se de que o serviço servidor DNS local está instalado e em execução no controlador de domínio que você tiver restaurado. Se esse controlador de domínio não tiver um servidor DNS antes da falha de floresta, você deve instalar e configurar o servidor DNS. 
  
    > [!NOTE]
    > Se o controlador de domínio restaurado executa o Windows Server 2008, você precisa instalar o hotfix no artigo KB [975654](https://support.microsoft.com/kb/975654) ou conecte-se o servidor a uma rede isolada temporariamente para instalar o servidor DNS. O hotfix não é necessário para todas as outras versões do Windows Server. 
  
     No domínio raiz da floresta, configure o controlador de domínio restaurado com seu próprio endereço IP (ou um endereço de loopback, como 127.0.0.1) como seu servidor DNS preferencial. Você pode definir essa configuração nas propriedades de TCP/IP do adaptador de rede local (LAN). Isso é o primeiro servidor DNS da floresta. Para obter mais informações, consulte [configurar o TCP/IP para usar DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     Em cada domínio filho, configure o controlador de domínio restaurado com o endereço IP do primeiro servidor DNS no domínio raiz da floresta como seu servidor DNS preferencial. Você pode definir essa configuração nas propriedades de TCP/IP do adaptador de rede local. Para obter mais informações, consulte [configurar o TCP/IP para usar DNS](https://technet.microsoft.com/library/cc779282\(WS.10\).aspx). 
  
     Nas zonas DNS msdcs e domínio, exclua registros de NS de controladores de domínio que não existem mais após a limpeza de metadados. Verifique se os registros SRV dos DCs limpos foram removidos. Para ajudar a acelerar a remoção de registros SRV do DNS, execute:  
  
    ```  
    nltest.exe /dsderegdns:server.domain.tld  
    ```  
  
8. Aumentar o valor do pool RID disponível 100.000. Para obter mais informações, consulte [aumentando o valor de pools RID disponíveis](AD-Forest-Recovery-Raise-RID-Pool.md). Se você tiver motivos para acreditar que a elevar o Pool RID 100.000 é insuficiente para sua situação específica, você deve determinar o aumento de menor que é seguro usar ainda. Os RIDs são um recurso finito que não deve ser usado para cima desnecessariamente. 
  
     Se novas entidades de segurança foram criadas no domínio após a hora do backup que você pode usar para a restauração, essas entidades de segurança podem ter direitos de acesso em determinados objetos. Essas entidades de segurança não existem mais após a recuperação porque a recuperação foi revertido para o backup; No entanto, seus direitos de acesso ainda podem existir. Se o pool RID disponível não é disparado após uma restauração, o novo usuário objetos que são criados após a recuperação de floresta poderá obter IDs idênticas de segurança (SIDs) e pode ter acesso a esses objetos, o que não foi originalmente projetado. 
  
     Para ilustrar, considere o exemplo do novo funcionário chamado Amy que foi mencionada na introdução. O objeto de usuário para Amy não existe mais após a operação de restauração porque ele foi criado após o backup que foi usado para restaurar o domínio. No entanto, quaisquer direitos de acesso que foram atribuídos a esse objeto de usuário podem persistir após a operação de restauração. Se o SID para esse objeto de usuário é reatribuído a um novo objeto após a operação de restauração, o novo objeto obteria os direitos de acesso. 
  
9. Invalide o pool RID atual. O pool RID atual será invalidado após a restauração de estado do sistema. Mas se uma restauração de estado do sistema não foi executada, o pool RID atual precisa ser invalidados a fim de impedir que o controlador de domínio restaurado emitir novamente os RIDs do pool RID que foi atribuído no momento em que o backup foi criado. Para obter mais informações, consulte [invalidar o pool RID atual](AD-Forest-Recovery-Invaildate-RID-Pool.md). 
  
    > [!NOTE]
    > Na primeira vez que você tentar criar um objeto com um SID depois que você invalidar o pool RID, você receberá um erro. A tentativa de criar um objeto dispara uma solicitação para um novo pool RID. Repetição da operação é bem-sucedida pois o novo pool RID será alocado. 
  
10. Redefina a senha da conta de computador desse controlador de domínio duas vezes. Para obter mais informações, consulte [redefinindo a senha da conta de computador do controlador de domínio](AD-Forest-Recovery-Reset-Computer-Account-DC.md). 
  
11. Redefina a senha de krbtgt duas vezes. Para obter mais informações, consulte [redefinição de senha de krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md). 
  
     Como o histórico de senha krbtgt duas senhas, redefina senhas duas vezes para remover a senha (prefailure) original de histórico de senha. 
  
    > [!NOTE]
    > Se a recuperação de floresta em resposta a uma violação de segurança, você também poderá redefinir as senhas de relação de confiança. Para obter mais informações, consulte [redefinição de senha de relação de confiança no um lado da relação de confiança](AD-Forest-Recovery-Reset-Trust.md). 
  
12. Se a floresta tiver vários domínios e o controlador de domínio restaurado era um servidor de catálogo global antes da falha, desmarque a **Catálogo Global** caixa de seleção nas propriedades de configurações NTDS para remover o catálogo global do controlador de domínio. A exceção a essa regra é o caso comum de uma floresta com apenas um domínio. Nesse caso, não é necessário remover o catálogo global. Para obter mais informações, consulte [remover o catálogo global](AD-Forest-Recovery-Remove-GC.md). 
  
     Restaurando um catálogo global de um backup que é mais recente do que outros backups que são usados para restaurar controladores de domínio em outros domínios, você poderá introduzir objetos remanescentes. Considere o exemplo a seguir. No domínio A, o DC1 é restaurado de um backup que foi feito no horário T1. No domínio B, o DC2 é restaurado de um backup de catálogo global que foi feito no tempo T2. Suponha que T2 é mais recente que T1 e alguns objetos foram criados entre T1 e T2. Depois que esses controladores de domínio forem restaurados, o DC2, que é um catálogo global, contém dados mais recentes para réplica parcial de domínio do que o domínio A mantém em si. DC2, nesse caso, mantém os objetos remanescentes, porque esses objetos não estão presentes no DC1. 
  
     A presença de objetos remanescentes pode levar a problemas. Por exemplo, as mensagens de email não podem ser entregue a um usuário cujo objeto de usuário foi movido entre domínios. Depois de colocar o controlador de domínio desatualizado ou on-line novamente o servidor de catálogo global, ambas as instâncias do objeto de usuário são exibidos no catálogo global. Os dois objetos tiverem o mesmo endereço de email; Portanto, as mensagens de email não podem ser entregue. 
  
     Um segundo problema é que uma conta de usuário que não existe mais ainda poderá aparecer na lista de endereços global. Um terceiro problema é que um grupo universal que não existe mais ainda pode aparecer no token de acesso do usuário. 
  
     Se você restaurar um controlador de domínio era um catálogo global — tanto inadvertidamente ou porque foi o backup solitário confiáveis, recomendamos que você impeça a ocorrência de objetos remanescentes, desabilitando o catálogo global, logo após a operação de restauração é Conclua. Desabilitar o sinalizador de catálogo global resultará em computador perder todas as suas réplicas parciais (partições) e manter em si ao status do controlador de domínio regulares. 
  
13. Configure o serviço de tempo do Windows. No domínio raiz da floresta, configure o emulador PDC para sincronizar a hora de uma fonte de tempo externa. Para obter mais informações, consulte [configurar o serviço de tempo do Windows no emulador PDC no domínio raiz da floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29). 
  
## <a name="reconnect-each-restored-writeable-domain-controller-to-a-common-network"></a>Reconectar-se cada controlador de domínio gravável restaurado para uma rede comum

Neste estágio você deve ter um controlador de domínio restaurado (e as etapas de recuperação executadas) no domínio raiz da floresta e, em cada um dos domínios restantes. Unir esses controladores de domínio a uma rede comum que é isolada do restante do ambiente e completar as etapas a seguir para validar a integridade de floresta e de replicação.

> [!NOTE]
> Quando você ingressa os controladores de domínio físicos para uma rede isolada, você talvez precise alterar seus endereços IP. Como resultado, os endereços IP dos registros DNS estará incorreto. Como um servidor de catálogo global não está disponível, as atualizações dinâmicas de DNS falhará. Controladores de domínio virtuais são mais vantajosos nesse caso, porque podem ser unidas a uma rede virtual nova sem alterar seus endereços IP. Esse é um motivo por que os controladores de domínio virtuais são recomendadas como os controladores de domínio de primeiro a serem restaurados durante a recuperação de floresta. 
  
Após a validação, adicionar os controladores de domínio na rede de produção e conclua as etapas para verificar a integridade de replicação da floresta.

- Para corrigir a resolução de nomes, crie registros de delegação de DNS e configurar DNS dicas de raiz e encaminhamento conforme necessário. Execute **repadmin /replsum** para verificar a replicação entre controladores de domínio. 
- Se o controlador de domínio restaurado não é parceiros de replicação direta, recuperação da replicação será muito mais rápida com a criação de objetos de conexão temporária entre eles. 
- Para validar a limpeza de metadados, execute **Repadmin /viewlist \\** * para obter uma lista de todos os controladores de domínio na floresta. Execute **Nltest /DCList:** *< domínio\>*  para obter uma lista de todos os controladores de domínio no domínio. 
- Para verificar a integridade do controlador de domínio e DNS, execute DCDiag /v para relatar erros em todos os controladores de domínio na floresta. 

## <a name="add-the-global-catalog-to-a-domain-controller-in-the-forest-root-domain"></a>Adicionar o catálogo global para um controlador de domínio no domínio raiz da floresta

Um catálogo global é necessário para esses e outros motivos:  
  
- Para habilitar logons para usuários. 
- Para habilitar o serviço de Logon de rede em execução nos controladores de domínio em cada domínio filho para se registrar e remover registros no servidor DNS no domínio raiz. 
  
Embora essa opção é preferível que o controlador de domínio raiz da floresta se tornar um catálogo global, é possível escolher qualquer um dos controladores de domínio restaurados para se tornar um catálogo global. 
  
> [!NOTE]
> Um controlador de domínio só será anunciado como um servidor de catálogo global até que ele concluiu uma sincronização completa de todas as partições de diretório na floresta. Portanto, o controlador de domínio deve ser forçado a replicar com cada um dos DCs restaurados na floresta. 
>
> Monitorar o serviço de diretório log de eventos no Visualizador de eventos para o evento ID 1119, que indica que esse controlador de domínio é um servidor de catálogo global, ou verifique se que a seguinte chave do registro tem um valor de 1:  
>
> **Promoção de catálogo HKLM\System\CurrentControlSet\Services\NTDS\Parameters\Global concluída**  
  
Para obter mais informações, consulte [adicionando o catálogo global](AD-Forest-Recovery-Add-GC.md). 
  
Neste estágio, você deve ter uma floresta estável, com um controlador de domínio para cada domínio e um catálogo global na floresta. Você deve fazer um novo backup de cada um dos controladores de domínio que você acabou de restaurar. Agora, você pode começar a reimplantar os outros controladores de domínio na floresta por meio da instalação do AD DS. 

## <a name="next-steps"></a>Próximas etapas

- [Recuperação de floresta do AD – Pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperação de floresta do AD - elaborar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD - executar a recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)  
- [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperação de floresta do AD - recuperação de um único domínio dentro de uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperação de floresta do AD - recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
