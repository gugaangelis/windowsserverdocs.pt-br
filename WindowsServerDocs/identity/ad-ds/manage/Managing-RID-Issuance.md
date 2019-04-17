---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: "Gerenciando RID emissão"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f84bcc1aa32e9993903e094fc43feffcbe16a05b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="managing-rid-issuance"></a>Gerenciando RID emissão

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica a mudança para a função mestre RID FSMO, incluindo a emissão de novo e funcionalidade no mestre RID e como analisar e solucionar problemas de emissão RID de monitoramento.  
  
-   [Gerenciando RID emissão](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)  
  
-   [Solução de problemas de emissão RID](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)  
  
Mais informações estão disponíveis na [AskDS Blog](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx).  
  
## <a name="BKMK_Manage"></a>Gerenciando RID emissão  
Por padrão, um domínio tem capacidade para quase um bilhão entidades de segurança, como os usuários, grupos e computadores. Naturalmente, não há nenhuma domínios com que muitos objetos usados ativamente. No entanto, o suporte ao cliente Microsoft encontrou casos em que:  
  
-   O software ou scripts administrativos acidentalmente em massa de provisionamento criado usuários, grupos e computadores.  
  
-   Vários grupos de distribuição e segurança não utilizada foram criados por usuários delegados  
  
-   Muitos controladores de domínio foram rebaixados, restaurado, ou metadados removidos  
  
-   Foram realizadas recuperações de floresta  
  
-   A operação de InvalidateRidPool foi executada com frequência  
  
-   O valor de registro do tamanho do bloco LIVRAR aumentou incorretamente  
  
Todas essas situações consomem RIDs desnecessariamente, geralmente por engano. Ao longo de muitos anos, algumas ambientes ficou sem RIDs e isso forçado-los para migrar para um novo domínio ou executar recuperações de floresta.  
  
Windows Server 2012 corrige problemas com alocação RID somente se tornaram problemáticos com a idade e o onipresença do Active Directory. Isso inclui melhor registro em log de eventos, limites mais apropriados e a capacidade para - em uma emergência - para dobrar o tamanho geral do espaço RID global para um domínio.  
  
### <a name="periodic-consumption-warnings"></a>Avisos de consumo periódicas  
Windows Server 2012 adiciona o evento de espaço RID global de rastreamento que fornece aviso antecipado quando principais metas são excedidas. O modelo calcula a porcentagem dez (10) usados marca no pool global e registra um evento quando atingido. Em seguida, calcula o próximo dez por cento usado entre o restante e o ciclo de evento continua. Conforme o espaço RID global é esgotado, eventos acelerará como sucessos de 10% mais rápido em um pool de redução (mas retardamento do log de eventos impedirá que mais de uma entrada por hora). O log de eventos do sistema em todos os controladores de domínio grava eventos de aviso do diretório de serviços de SAM 16658.  
  
Pressupondo que um espaço RID global do padrão 30 bits, os logs de eventos primeiro ao alocar o pool que contém o 107,374,182<sup>e</sup> RID. A taxa de evento acelera até o último ponto de verificação de 100.000, naturalmente com 110 eventos gerados no total. O comportamento é semelhante para um desbloqueado 31 bits global RID espaço: começando em 214,748,365 e concluir em 117 eventos.  
  
> [!IMPORTANT]  
> Esse evento não é esperado; Investigue o usuário, computador e processos de criação de grupo imediatamente no domínio. Criar objetos do AD DS mais de 100 milhões é bastante fora do comum.  
  
![Emissão RID](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)  
  
### <a name="rid-pool-invalidation-events"></a>Eventos de invalidação Pool RID  
Há novas evento avisa que um pool de DC LIVRAR local foi descartado. Esses são informativo e poderiam ser esperados, principalmente devido a nova funcionalidade v CC. Veja a lista de eventos abaixo para obter detalhes sobre o evento.  
  
### <a name="BKMK_RIDBlockMaxSize"></a>Limite de tamanho de bloco marca d'  
Normalmente, um controlador de domínio solicita RID alocações nos blocos de 500 RIDs ao mesmo tempo. Você pode substituir esse padrão usando o seguinte registro REG_DWORD valor em um controlador de domínio:  
  
```  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values  
RID Block Size  
  
```  
  
Antes do Windows Server 2012, não houve nenhum valor máximo imposto na chave do registro, exceto o máximo DWORD implícito (que tem um valor 0xffffffff ou 4294967295). Esse valor é consideravelmente maior do que o espaço total de RID global. Os administradores, às vezes, inadequadamente ou acidentalmente configuraram LIVRAR tamanho de bloco com valores esgotado o RID global em um ritmo massive.  
  
No Windows Server 2012, você não pode definir esse valor de registro superior a 15.000 decimal (0x3A98 hexadecimal). Isso impede que massive alocação RID não intencional.  
  
Se você definir o valor *superior* que 15.000, o valor é tratado como 15.000 e o controlador de domínio registra o evento 16653 no log de eventos de serviços de diretório em cada inicialização até que o valor seja corrigido.  
  
### <a name="BKMK_GlobalRidSpaceUnlock"></a>Tamanho de espaço RID global desbloquear  
Antes do Windows Server 2012, o espaço RID global foi limitado a 2<sup>30</sup> (ou 1.073.741.823) total RIDs. Depois que atingido, apenas um domínio migração ou floresta recuperação para um período de tempo mais antigo permitidos criação de novos SIDs - recuperação de desastres, por qualquer medida. A partir do Windows Server 2012, o 2<sup>31</sup> bits podem ser desbloqueado para aumentar o pool global para 2.147.483.648 RIDs.  
  
AD DS armazena essa configuração em um atributo oculto especial chamado **SidCompatibilityVersion** no contexto do RootDSE de todos os controladores de domínio. Esse atributo não é legível usando ADSIEdit, LDP ou outras ferramentas. Para ver um aumento no espaço de RID global, examine o log de eventos do sistema para o evento de aviso 16655 do diretório de serviços de SAM ou use o seguinte comando Dcdiag:  
  
```  
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"  
  
```  
  
Se você aumentar o pool RID global, o pool disponível será alterado para 2.147.483.647 em vez do padrão 1.073.741.823. Por exemplo:  
  
![Emissão RID](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)  
  
> [!WARNING]  
> Destina-se essa desbloqueio *somente* para evitar ficar sem RIDS e deve ser usado *somente* em conjunto com a marca d' teto imposição (veja a próxima seção). Não "preventivamente" defina isso em ambientes que tenham milhões de restante RIDs e aumento de baixo, como potencialmente houver problemas de compatibilidade com SIDs gerado do pool RID desbloqueado.  
>   
> Isso desbloquear operação não pode ser revertido ou removidos, exceto por uma recuperação de floresta completa para backups anteriores.  
  
#### <a name="important-caveats"></a>Limitações importantes  
Windows Server 2003 e controladores de domínio do Windows Server 2008 não podem emitir RIDs quando o RID global pool 31<sup>st</sup> bit for desbloqueado. Controladores de domínio do Windows Server 2008 R2 *pode* usar 31<sup>st</sup> bit RIDs *, mas somente se* tiverem hotfix [KB 2642658](https://support.microsoft.com/kb/2642658) instalado. Controladores de domínio sem suporte e sem patch tratam o pool RID global como esgotado quando desbloqueado.  
  
Esse recurso não é imposto pelo nível funcional de domínio; Tome muito cuidado que existem somente atualizados controladores de domínio do Windows Server 2008 R2 ou Windows Server 2012 no domínio.  
  
#### <a name="implementing-unlocked-global-rid-space"></a>Implementando a marca d' Global desbloqueado espaço  
Para desbloquear o pool RID para o 31<sup>st</sup> bit depois de receber o alerta de teto RID (veja abaixo) execute as seguintes etapas:  
  
1.  Certifique-se de que o mestre de RID função está sendo executado em um controlador de domínio do Windows Server 2012. Caso contrário, transferi-lo para um controlador de domínio do Windows Server 2012.  
  
2.  Executar LDP.exe  
  
3.  Clique no **Conexão** menu e clique em **conectar** para Windows Server 2012 mestre RID na porta 389 e clique em **associar** como um administrador de domínio.  
  
4.  Clique no **procurar** menu e clique em **modificar**.  
  
5.  Certifique-se de que **DN** está em branco.  
  
6.  Em **atributo de entrada editar**, tipo:  
  
    ```  
    SidCompatibilityVersion  
    ```  
  
7.  Em **valores**, tipo:  
  
    ```  
    1  
    ```  
  
8.  Certifique-se de que **adicionar** está selecionado no **operação** e clique em **Enter**. Isso atualiza o **entrada lista**.  
  
9. Selecione o **síncronas** e **estendido** clique em Opções, **executar**.  
  
    ![Emissão RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)  
  
10. Se for bem-sucedida, o LDP saída janela mostra:  
  
    ```  
    ***Call Modify...  
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);  
    modified "".  
  
    ```  
  
    ![Emissão RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)  
  
11. Confirme o pool RID global aumentado examinando o Log de eventos do sistema no controlador de domínio para o evento informativo do diretório de serviços de SAM 16655.  
  
### <a name="rid-ceiling-enforcement"></a>LIVRAR teto imposição  
Para lidar com uma medida de proteção e elevar reconhecimento administrativo, Windows Server 2012 introduz um teto artificial no intervalo RID global dez (10) por cento restante RIDs no namespace global. Quando dentro de um (1) % do teto artificial, controladores de domínio solicitando pools RID escrever o evento de aviso do diretório de serviços de SAM 16656 o log de eventos do sistema. Quando atingir o máximo de dez por cento em RID Master FSMO, grava eventos do diretório de serviços de SAM 16657 o log de eventos do sistema e não alocará mais pools RID até substituindo o teto. Isso força a avaliar o estado do mestre RID no domínio e resolver possível alocação RID fuga; Isso também impede domínios esgotando todo o espaço RID.  
  
Este teto é codificado no restante de 10% do espaço disponível RID. Ou seja, o teto ativa quando o mestre de RID aloca um pool que inclua o RID correspondente a 90 (90) % do espaço RID global.  
  
-   Para os domínios de padrão, o primeiro ponto de gatilho é 2<sup>30</sup>-1 * 0,90 = 966,367,640 (ou 107,374,183 RIDs restante).  
  
-   Para domínios com um espaço RID 31 bits desbloqueado, o ponto de gatilho é 2<sup>31</sup>-1 * 0,90 = 1,932,735,282 RIDs (ou 214,748,365 RIDs restante).  
  
Quando acionado, o mestre de RID define o atributo do Active Directory **msDS-RIDPoolAllocationEnabled** (nome comum **ms-DS Pool - RID--alocação-habilitado**) como FALSE no objeto:  
  
CN = RID Manager$, CN = sistema, DC =*<domain>*  
  
Isso grava o evento 16657 e impede a emissão de bloco RID para todos os controladores de domínio. Controladores de domínio continuam a consumir qualquer pools RID pendentes já emitidos para eles.  
  
Para remover o bloco e permitir que a alocação de pools RID continuar, defina esse valor como TRUE. Na próxima alocação de RID realizada pelo mestre RID, o atributo retornará padrão não valor definido. Depois disso, não há nenhuma tetos adicionais e eventualmente, o espaço RID global acabar, exigir que a migração de recuperação ou de domínio da floresta.  
  
#### <a name="removing-the-ceiling-block"></a>Remover o bloqueio de teto  
Para remover o bloco alcançando uma vez o teto artificial, execute as seguintes etapas:  
  
1.  Certifique-se de que o mestre de RID função está sendo executado em um controlador de domínio do Windows Server 2012. Caso contrário, transferi-lo para um controlador de domínio do Windows Server 2012.  
  
2.  Execute LDP.exe.  
  
3.  Clique no **Conexão** menu e clique em *conectar* para Windows Server 2012 mestre RID na porta 389 e clique em **associar** como um administrador de domínio.  
  
4.  Clique no **exibição** menu e clique em **árvore**, em seguida, para o **Base DN** selecione o contexto de nomenclatura do mestre RID próprio domínio. Clique em **Okey**.  
  
5.  No painel de navegação, aprofundar o **CN = System** contêiner e clique no **CN = RID Manager$** objeto. Clique com botão direito-la e clique em **modificar**.  
  
6.  No atributo de entrada editar, digite:  
  
    ```  
    MsDS-RidPoolAllocationEnabled  
    ```  
  
7.  Em **valores**, tipo (em maiusculo):  
  
    ```  
    TRUE  
    ```  
  
8.  Selecione **substituir** em **operação** e clique em **Enter**. Isso atualiza o **entrada lista**.  
  
9. Habilitar o **síncronas** e **estendido** clique em Opções, **executar**:  
  
    ![Emissão RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)  
  
10. Se for bem-sucedida, o LDP saída janela mostra:  
  
    ```  
    ***Call Modify...  
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);  
    Modified "CN=RID Manager$,CN=System,DC=<domain>".  
  
    ```  
  
    ![Emissão RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)  
  
### <a name="other-rid-fixes"></a>Outras correções RID  
Sistemas de operacionais anteriores do Windows Server tinha um pool RID vazem quando atributo rIDSetReferences ausente. Para resolver esse problema em controladores de domínio que executam o Windows Server 2008 R2, instale o hotfix por [KB 2618669](https://support.microsoft.com/kb/2618669).  
  
### <a name="unfixed-rid-issues"></a>Problemas RID unfixed  
Historicamente houve um vazamento RID em caso de falha de criação de conta; ao criar uma conta, falha ainda usa o RID. O exemplo comum é criar um usuário com uma senha que não atendem a complexidade.  
  
### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>Marca d' correções para versões anteriores do Windows Server  
Todas as correções e alterações acima têm o Windows Server 2008 R2 hotfixes lançados. Não há atualmente nenhum hotfixes do Windows Server 2008 planejada ou em andamento.  
  
## <a name="BKMK_Tshoot"></a>Solução de problemas de emissão RID  
  
### <a name="introduction-to-troubleshooting"></a>Introdução à solução de problemas  
Solução de problemas de emissão RID requer um método lógico e linear. A menos que você estiver monitorando os logs de eventos com cuidado para erros e avisos disparadas RID, suas primeira indicações de um problema têm probabilidade de ser criações de conta falha. A chave para solução de problemas de emissão RID é entender quando o sintoma deve ou não; muitos problemas de emissão RID podem afetam apenas um controlador de domínio e não tem nada a fazer melhorias de componente. Este diagrama simple abaixo ajuda a tomar essas decisões mais clara:  
  
![Emissão RID](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)  
  
### <a name="troubleshooting-options"></a>Opções de solução de problemas  
  
#### <a name="logging-options"></a>Opções de registro em log  
Registro em log todas as na emissão RID ocorre no log de eventos do sistema, em fonte diretório de serviços de SAM. Registro em log está habilitado e configurado para máximo detalhamento, por padrão. Se nenhuma entrada estiver conectada para que as alterações de componente novo no Windows Server 2012, trate o problema como um clássico (também conhecido como herdados, anteriores ao Windows Server 2012) problema de emissão RID visto no Windows 2008 R2 ou sistemas operacionais mais antigos.  
  
#### <a name="utilities-and-commands-for-troubleshooting"></a>Utilitários e os comandos de solução de problemas  
Para solucionar problemas não explicados pelos logs mencionados anteriormente - problemas de emissão RID especialmente mais antigos – use a seguinte lista de ferramentas como ponto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   3.4 Do Monitor de rede  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Metodologia geral para solução de problemas de configuração do controlador de domínio  
  
1.  É o erro causado por um simples problema de disponibilidade de controlador de domínio ou de permissões?  
  
    1.  Você está tentando criar uma segurança principal sem as permissões necessárias? Examine a saída de erros de acesso negado.  
  
    2.  Um controlador de domínio está disponível? Examine as mensagens de disponibilidade de controlador erro ou LDAP ou domínio retornadas.  
  
2.  O erro retornado especificamente menciona RIDs e é específico para usar como orientação? Em caso afirmativo, siga as orientações.  
  
3.  O erro retornado especificamente menciona RIDs, mas é outra forma não específicos? Por exemplo, "Windows não é possível criar o objeto porque o serviço de diretório não pôde alocar um identificador relativo."  
  
    1.  Examine o log de eventos do sistema no controlador de domínio para "herdado" (Pre-Windows Server 2012) RID eventos detalhados em [d' Pool solicitar](https://technet.microsoft.com/en-us/library/ee406152(WS.10).aspx) (16642, 16643, 16644, 16645, 16656).  
  
    2.  Examine o evento do sistema no controlador de domínio e o mestre RID novos eventos indicando que o bloco detalhados abaixo neste tópico (16655, 16656, 16657).  
  
    3.  Validar a integridade da replicação do Active Directory com a disponibilidade de Repadmin.exe e mestre RID com **Dcdiag.exe//test: ridmanager /v**. Habilite capturas de rede dupla face entre o controlador de domínio e o mestre RID se esses testes são inconclusive.  
  
### <a name="troubleshooting-specific-problems"></a>Solução de problemas específicos  
Faça as seguintes novas mensagens no log de eventos do sistema em controladores de domínio do Windows Server 2012. Automatizada integridade AD rastreamento sistemas, como o System Center Operations Manager deve monitorar esses eventos; todos são importantes, e alguns são indicadores de problemas cruciais do domínio.  
  
|||  
|-|-|  
|ID de evento|16653|  
|Fonte|Diretório de serviços de SAM|  
|Severity|Aviso|  
|Mensagem|Um tamanho de pool para identificadores de conta (RIDs) que foi configurado por um administrador é maior que o máximo com suporte. O valor máximo de %1 será usado quando o controlador de domínio é o mestre de RID.<br /><br />Para obter mais informações, consulte [limite de tamanho de bloco d'](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize).|  
|Notas e resolução|O valor máximo para o tamanho do bloco LIVRAR agora está 15000 decimal (3A98 hexadecimal). Um controlador de domínio não pode solicitar mais de 15.000 RIDs. Este logs de eventos em cada inicialização até que o valor for definido como um valor em ou abaixo desse máximo.|  
  
|||  
|-|-|  
|ID de evento|16654|  
|Fonte|Diretório de serviços de SAM|  
|Severity|Informativa|  
|Mensagem|Um conjunto de identificadores de conta (RIDs) tiver sido invalidado. Isso pode ocorrer nos seguintes casos esperados:<br /><br />1. Um controlador de domínio é restaurado de backup.<br /><br />2. Um controlador de domínio em execução em uma máquina virtual é restaurado do instantâneo.<br /><br />3. Um administrador tem invalidados manualmente o pool.<br /><br />Consulte https://go.microsoft.com/fwlink/?LinkId=226247 para obter mais informações.|  
|Notas e resolução|Se esse evento inesperado, entre em contato com todos os administradores de domínio e determinar qual deles executou a ação. O log de eventos de serviços de diretório também contém mais informações sobre quando uma destas etapas foi realizada.|  
  
|||  
|-|-|  
|ID de evento|16655|  
|Fonte|Diretório de serviços de SAM|  
|Severity|Informativa|  
|Mensagem|O máximo global para identificadores de conta (RIDs) foi aumentado para %1.|  
|Notas e resolução|Se esse evento inesperado, entre em contato com todos os administradores de domínio e determinar qual deles executou a ação. Esse evento notas o aumento do RID geral pool tamanho além do padrão de 2<sup>30</sup>não será realizada automaticamente; apenas por ação administrativa.|  
  
|||  
|-|-|  
|ID de evento|16656|  
|Fonte|Diretório de serviços de SAM|  
|Severity|Aviso|  
|Mensagem|O máximo global para identificadores de conta (RIDs) foi aumentado para %1.|  
|Notas e resolução|Ação necessária! Um pool de identificadores de conta (RID) foi alocado para esse controlador de domínio. O valor de pool indica que neste domínio tem consumido uma parte considerável dos totais disponíveis-identificadores de conta.<br /><br />Um mecanismo de proteção será ativado quando o domínio atinge o limite de seguir do totais disponíveis-identificadores de conta restante: %1.  O mecanismo de proteção impedirá a criação de conta até que você reabilite alocação de identificadores de conta no controlador de domínio mestre RID.<br /><br />Consulte https://go.microsoft.com/fwlink/?LinkId=228610 para obter mais informações.|  
  
|||  
|-|-|  
|ID de evento|16657|  
|Fonte|Diretório de serviços de SAM|  
|Severity|Erro|  
|Mensagem|Ação necessária! Neste domínio tem consumido uma parte considerável dos totais disponíveis-identificadores de conta (RIDs). Um mecanismo de proteção foi ativado porque os totais disponíveis-identificadores de conta restante é menor que: X % [argumento teto artificial].<br /><br />O mecanismo de proteção impede que a criação de conta até que você reabilite alocação de identificadores de conta no controlador de domínio mestre RID.<br /><br />É extremamente importante que determinados diagnóstico sejam executado antes de ativar novamente a conta de criação para garantir que esse domínio não está consumindo identificadores de conta com uma taxa de forma anormal alto. Quaisquer problemas identificados devem ser resolvidos antes de ativar novamente a criação da conta.<br /><br />Falha para diagnosticar e corrigir qualquer problema subjacente causando uma forma anormal alta taxa de consumo de identificadores de conta pode causar esgotamento de identificadores de conta no domínio após o qual criação da conta será desabilitada permanentemente neste domínio.<br /><br />Consulte https://go.microsoft.com/fwlink/?LinkId=228610 para obter mais informações.|  
|Notas e resolução|Entre em contato com todos os administradores de domínio e informá-los de que não há outras entidades de segurança podem ser criadas neste domínio até que essa proteção é substituída. Para obter mais informações sobre como substituir a proteção e possivelmente aumentar o RID geral pool, consulte [Global LIVRAR espaço tamanho desbloquear](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock).|  
  
|||  
|-|-|  
|ID de evento|16658|  
|Fonte|Diretório de serviços de SAM|  
|Severity|Aviso|  
|Mensagem|Esse evento é uma atualização periódica na quantidade total restante da conta-identificadores (RIDs) disponíveis. O número de identificadores de conta restantes é aproximadamente: %1.<br /><br />Account-identifiers são usados como contas são criadas quando estiverem no fim sem novas contas podem ser criadas no domínio.<br /><br />Consulte https://go.microsoft.com/fwlink/?LinkId=228745 para obter mais informações.|  
|Notas e resolução|Entre em contato com todos os administradores de domínio e informá-los que RID consumo tenha excedido marco; Determine se esse for o comportamento esperado ou não, analisando padrões de criação de confiança de segurança. Nunca ver esse evento seria altamente incomuns, porque significa que pelo menos ~ 100 milhões RIDS alocou.|  
  
## <a name="see-also"></a>Consulte também  
[Gerenciando a emissão de RID no Windows Server 2012](http://blogs.technet.com/b/askds/archive/2012/08/10/managing-rid-issuance-in-windows-server-2012.aspx)  
  


