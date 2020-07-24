---
ms.assetid: aac117a7-aa7a-4322-96ae-e3cc22ada036
title: Gerenciar emissão de RIDs
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 065ef1af6d125b0c78669c11ffa28d7e4c9632d8
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966328"
---
# <a name="managing-rid-issuance"></a>Gerenciar emissão de RIDs

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica a alteração na função FSMO do mestre RID, incluindo a nova funcionalidade de emissão e monitoramento no mestre RID e como analisar e solucionar problemas na emissão de RID.  
  
-   [Como gerenciar a emissão de RIDs](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Manage)  
  
-   [Solucionando problemas de emissão de RIDs](../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_Tshoot)  
  
Mais informações estão disponíveis no [blog do AskDS](/archive/blogs/askds/managing-rid-issuance-in-windows-server-2012).  
  
## <a name="managing-rid-issuance"></a><a name="BKMK_Manage"></a>Gerenciando a emissão de RID  
Por padrão, um domínio possui a capacidade para aproximadamente um bilhão de entidades de segurança, como usuários, grupos e computadores. Naturalmente, não há domínios com tantos objetos ativamente usados. Entretanto, o Suporte ao Cliente da Microsoft descobriu casos em que:  
  
-   O provisionamento de software ou scripts administrativos acidentalmente criou usuários, grupos e computadores em massa.  
  
-   Muitos grupos de segurança e distribuição não usados foram criados por usuários delegados  
  
-   Muitos controladores de domínio foram rebaixados, restaurados ou tiveram os metadados limpos  
  
-   Recuperações de floresta foram realizadas  
  
-   A operação InvalidateRidPool foi executada com frequência  
  
-   O valor de registro Tamanho de Bloco RID foi aumentado de forma incorreta  
  
Todas essas situações usam RIDs desnecessariamente, muitas vezes por engano. Por muitos anos, alguns ambientes não tinham mais RIDs, o que fez com que eles migrassem para um novo domínio ou executassem recuperações de floresta.  
  
O Windows Server 2012 aborda problemas com a alocação de RID que só se tornaram problemáticos com o tempo e a ubiquidade do Active Directory. Isso inclui um melhor registro em log de eventos, os limites mais apropriados e a capacidade de entrar em uma emergência para dobrar o tamanho geral do espaço global do RID para um domínio.  
  
### <a name="periodic-consumption-warnings"></a>Avisos periódicos de consumo  
O Windows Server 2012 adiciona rastreamento de eventos de espaço de RID global, que fornece avisos antecipados quando os principais marcos são ultrapassados. O modelo calcula a marca de 10 (dez) por cento usado no pool global e registra em log um evento quando ela é atingida. Ele, então, calcula os próximos dez por cento usados do restante e o ciclo de eventos continua. Na medida em que o espaço de RID global vai se esgotando, eventos são acelerados, conforme atinge-se os dez por cento cada vez mais rápido em um pool decrescente (mas a redução do log de eventos impedirá mais de uma entrada por hora). O log de eventos do Sistema em cada controlador de domínio grava o evento de aviso 16658 Directory-Services-SAM.  
  
Presumindo um espaço de RID global de 30 bits, o primeiro evento é registrado em log durante a alocação do pool que contém o 107.374.182º<sup></sup> RID. A taxa de eventos é acelerada naturalmente até o último ponto de verificação de 100.000, com 110 eventos gerados no total. O comportamento é similar para um espaço de RID global de 31 bits desbloqueado: iniciando em 214.748.365 e concluindo em 117 eventos.  
  
> [!IMPORTANT]  
> Esse evento não é esperado; investigue os processos de criação de usuários, comutadores e grupos imediatamente no domínio. Criar mais de 100 milhões de objetos do AD DS é totalmente fora do comum.  
  
![Emissão de RID](media/Managing-RID-Issuance/ADDS_RID_TR_EventWaypoints2.png)  
  
### <a name="rid-pool-invalidation-events"></a>Eventos de invalidação do pool RID  
Há novos alertas de eventos que um pool DC RID local descartou. Esses são alertas informativos e podem ser esperados, especialmente devido à nova funcionalidade do VDC. Consulte a lista de eventos abaixo para obter detalhes sobre o evento.  
  
### <a name="rid-block-size-limit"></a><a name="BKMK_RIDBlockMaxSize"></a>Limite de tamanho do bloco RID  
Normalmente, um controlador de domínio solicita alocações de RID em blocos de 500 RIDs de cada vez. Você pode substituir esse padrão usando o seguinte valor REG_DWORD de registro em um controlador de domínio:  
  
```  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\RID Values  
RID Block Size  
  
```  
  
Antes do Windows Server 2012, não havia nenhum valor máximo imposto nessa chave do Registro, exceto o máximo DWORD implícito (que apresenta um valor de 0xffffffff ou 4294967295). Esse valor é consideravelmente maior do que o espaço de RID global total. Às vezes, os administradores configuravam inapropriada ou acidentalmente o Tamanho de Bloco RID com valores que esgotavam o RID global a uma alta taxa.  
  
No Windows Server 2012, não é possível definir esse valor de registro como acima de 15.000 decimal (0x3A98 hexadecimal). Isso impede uma alta alocação de RID não intencional.  
  
Se você definir o valor como *superior* a 15.000, ele será tratado como 15.000 e o controlador de domínio registrará o evento 16653 no log de eventos dos Serviços de Diretório a cada reinicialização, até que o valor seja corrigido.  
  
### <a name="global-rid-space-size-unlock"></a><a name="BKMK_GlobalRidSpaceUnlock"></a>Desbloqueio de tamanho de espaço de RID global  
Antes do Windows Server 2012, o espaço de RID global era limitado a um total de 2<sup>30</sup> (ou 1.073.741.823) RIDs. Uma vez atingido, somente uma migração de domínio ou recuperação de floresta em um período de tempo mais antigo permitia uma nova criação de SIDs — de qualquer forma, recuperação de desastre. A partir do Windows Server 2012, 2<sup>31</sup> bits podem ser desbloqueados para aumentar o pool global para 2.147.483.648 RIDs.  
  
O AD DS armazena essa configuração em um atributo especial oculto denominado **SidCompatibilityVersion** no contexto RootDSE de todos os controladores de domínio. Esse atributo não é legível usando ADSIEdit, LDP ou outras ferramentas. Para ver um aumento no espaço de RID global, examine se há, no log de eventos do Sistema, um evento de aviso 16655 de Directory-Services-SAM, ou use o seguinte comando Dcdiag:  
  
```  
Dcdiag.exe /TEST:RidManager /v | find /i "Available RID Pool for the Domain"  
  
```  
  
Se você aumentar o pool RID global, o pool disponível mudará para 2.147.483.647 em vez do padrão 1.073.741.823. Por exemplo:  
  
![Emissão de RID](media/Managing-RID-Issuance/ADDS_RID_TR_Dcdiag.png)  
  
> [!WARNING]  
> Esse desbloqueio tem *apenas* a finalidade de evitar que os RIDs se esgotem e deve ser usado *somente* em conjunto com a Imposição de Limite Máximo de RID (veja a próxima seção). Não configure isso "com preempção" em ambientes que possuem milhões de RIDs restantes e com baixo crescimento, já que existem possíveis problemas de compatibilidade do aplicativo com SIDs gerados a partir do pool RID desbloqueado.  
>   
> Essa operação de desbloqueio não pode ser revertida ou removida, exceto por uma completa recuperação da floresta para backups mais antigos.  
  
#### <a name="important-caveats"></a>Avisos importantes  
Os controladores do domínio do Windows Server 2003 e do Windows Server 2008 não podem emitir RIDs quando o 31º<sup></sup> bit do pool RID global está bloqueado. Os controladores de domínio do Windows Server 2008 R2 *podem* usar 31 de bits de<sup>St</sup> *, mas somente se* tiverem o hotfix [KB 2642658](https://support.microsoft.com/kb/2642658) instalado. Controladores de domínio sem suporte e sem patch tratam o pool RID global como esgotado quando desbloqueado.  
  
Esse recurso não é imposto por nenhum nível funcional de domínio; tenha muito cuidado para que somente controladores de domínio do Windows Server 2012 ou Windows Server 2008 R2 atualizado existam no domínio.  
  
#### <a name="implementing-unlocked-global-rid-space"></a>Implementando o espaço de RID global desbloqueado  
Para desbloquear o pool RID para o 31º<sup></sup> bit depois de receber o alerta de limite máximo de RID (veja abaixo), execute as seguintes etapas:  
  
1.  Verifique se a função Mestre RID está em execução em um controlador de domínio do Windows Server 2012. Se não estiver, transfira-a para um controlador de domínio do Windows Server 2012.  
  
2.  Execute LDP.exe  
  
3.  Clique no menu **Conexão**, em **Conectar** no Mestre RID do Windows Server 2012 na porta 389, e em **Associar** como um administrador de domínio.  
  
4.  Clique no menu **Procurar** e em **Modificar**.  
  
5.  Verifique se **DN** está em branco.  
  
6.  Em **Editar atributo de entrada**, digite:  
  
    ```  
    SidCompatibilityVersion  
    ```  
  
7.  Em **Valores**, digite:  
  
    ```  
    1  
    ```  
  
8.  Verifique se **Adicionar** está selecionado em **Operação** e clique em **Enter**. Isso atualiza a **Lista de Entrada**.  
  
9. Selecione as opções **Síncrono** e **Estendido**, e clique em **Executar**.  
  
    ![Emissão de RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModify.png)  
  
10. Se isso for bem-sucedido, a janela de saída LDP mostrará:  
  
    ```  
    ***Call Modify...  
     ldap_modify_ext_s(Id, '(null)',[1] attrs, SvrCtrls, ClntCtrls);  
    modified "".  
  
    ```  
  
    ![Emissão de RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPModifySuccess.png)  
  
11. Confirme o pool RID global aumentado examinando se há, no Log de Eventos do Sistema nesse controlador de domínio, o evento informativo 16655 Directory-Services-SAM.  
  
### <a name="rid-ceiling-enforcement"></a>Imposição de limite máximo de RID  
Para conseguir uma medida de proteção e um reconhecimento administrativo elevado, o Windows Server 2012 introduz um limite máximo artificial na variação de RID global a 10 (dez) por cento de RIDs restantes no espaço global. Quando em 1 (um) por cento do limite máximo artificial, os controladores de domínio que solicitam pools RID gravam o evento de aviso 16656 Directory-Services-SAM no log de eventos do sistema. Ao atingir o limite máximo de dez por cento no FSMO do Mestre RID, ele grava o evento 16657 Directory-Services-SAM no log de eventos do sistema e não alocará nenhum pool RID adicional até substituir o limite máximo. Isso o força a avaliar o estado do mestre RID no domínio e a abordar a potencial alocação de RID sem controle; isso também protege os domínios de esgotar todo o espaço de RID.  
  
Esse limite máximo é embutido em códigos a dez por cento restante do espaço de RID disponível. Ou seja, o limite máximo é ativado quando o mestre RID aloca um pool que inclui o RID correspondente de 90 (noventa) por cento do espaço de RID global.  
  
-   Para domínios padrão, o primeiro ponto de gatilho é 2<sup>30</sup>-1 * 0,90 = 966.367.640 (ou 107.374.183 RIDs restantes).  
  
-   Para domínios com um espaço de RID de 31 bits desbloqueado, o ponto de gatilho é 2<sup>31</sup>-1 * 0,90 = 1.932.735.282 RIDs (ou 214.748.365 RIDs restantes).  
  
Quando disparado, o mestre RID define o atributo do Active Directory **msDS-RIDPoolAllocationEnabled** (nome comum **ms-DS-RID-Pool-Allocation-Enabled**) como FALSE no objeto:  
  
CN = Gerenciador de RID $, CN = sistema, DC =*<domain>*  
  
Isso grava o evento 16657 e impede a emissão de bloco RID em todos os controladores de domínios. Os controladores de domínio continuam consumindo os pools RID pendentes, já emitidos a eles.  
  
Para remover o bloqueio e permitir que a alocação do pool RID continue, defina esse valor como TRUE. Na próxima alocação de RID executada pelo mestre RID, o atributo retornará ao seu valor padrão NOT SET. Depois disso, não haverá mais limites máximos e, finalmente, não haverá mais espaço de RID global, exigindo recuperação de floresta ou migração de domínio.  
  
#### <a name="removing-the-ceiling-block"></a>Removendo o bloqueio de limite máximo  
Para remover o bloqueio depois de atingir o limite máximo artificial, execute as seguintes etapas:  
  
1.  Verifique se a função Mestre RID está em execução em um controlador de domínio do Windows Server 2012. Se não estiver, transfira-a para um controlador de domínio do Windows Server 2012.  
  
2.  Execute LDP.exe.  
  
3.  Clique no menu **Conexão**, em *Conectar* no Mestre RID do Windows Server 2012 na porta 389, e em **Associar** como um administrador de domínio.  
  
4.  Clique no menu **Exibir**, em **Árvore**, e, em **DN de Base**, selecione o próprio contexto de nomeação de domínios do Mestre RID. Clique em **OK**.  
  
5.  No painel de navegação, faça drill down no contêiner **CN=System** e clique no objeto **CN=RID Manager$**. Clique com o botão direito do mouse nisso e clique em **Modificar**.  
  
6.  Em Editar Atributo de Entrada, digite:  
  
    ```  
    MsDS-RidPoolAllocationEnabled  
    ```  
  
7.  Em **Valores**, digite (em maiúsculas):  
  
    ```  
    TRUE  
    ```  
  
8.  Selecione **Substituir** em **Operação** e clique em **Enter**. Isso atualiza a **Lista de Entrada**.  
  
9. Habilite as opções **Síncrono** e **Estendido**, e clique em **Executar**:  
  
    ![Emissão de RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeiling.png)  
  
10. Se isso for bem-sucedido, a janela de saída LDP mostrará:  
  
    ```  
    ***Call Modify...  
    ldap_modify_ext_s(ld, 'CN=RID Manager$,CN=System,DC=<domain>',[1] attrs, SvrCtrls, ClntCtrls);  
    Modified "CN=RID Manager$,CN=System,DC=<domain>".  
  
    ```  
  
    ![Emissão de RID](media/Managing-RID-Issuance/ADDS_RID_TR_LDPRaiseCeilingSuccess.png)  
  
### <a name="other-rid-fixes"></a>Outras correções de RID  
Sistemas operacionais Windows Server anteriores tinham uma perda de pool RID quando o atributo rIDSetReferences estava ausente. Para resolver esse problema em controladores de domínio que executam o Windows Server 2008 R2, instale o hotfix do [KB 2618669](https://support.microsoft.com/kb/2618669).  
  
### <a name="unfixed-rid-issues"></a>Problemas de RID não corrigidos  
Historicamente, tem havido uma perda de RID na falha na criação da conta; ao criar uma conta, a falha ainda consume um RID. O exemplo comum é criar um usuário com uma senha que não atende à complexidade.  
  
### <a name="rid-fixes-for-earlier-versions-of-windows-server"></a>Correções de RID para versões anteriores do Windows Server  
Todas as correções e alterações acima possuem hotfixes do Windows Server 2008 R2 liberados. No momento, não há hotfixes do Windows Server 2008 planejados ou em andamento.  
  
## <a name="troubleshooting-rid-issuance"></a><a name="BKMK_Tshoot"></a>Solucionando problemas de emissão de RIDs  
  
### <a name="introduction-to-troubleshooting"></a>Introdução à solução de problemas  
A solução de problemas de emissão de RIDs requer um método lógico e linear. A menos que você esteja monitorando seus logs de eventos cuidadosamente quanto a avisos e erros disparados pelo RID, suas primeiras indicações de um problema provavelmente serão criações de contas com falha. A chave para solucionar problemas de emissão de RID é compreender quando o sintoma é esperado ou não; muitos problemas de emissão de RID podem afetar somente um controlador de domínio e não estão relacionados com melhorias de componente. Este diagrama simples ajuda a tornar as decisões mais claras:  
  
![Emissão de RID](media/Managing-RID-Issuance/adds_rid_issuance_troubleshooting.png)  
  
### <a name="troubleshooting-options"></a>Opções para solução de problemas  
  
#### <a name="logging-options"></a>Opções de log  
Todo registro em log na emissão de RID ocorre no log de Eventos do Sistema, sob Directory-Services-SAM de origem. Por padrão, o registro em log é habilitado e configurado para máximo de detalhamento. Se nenhuma entrada for registrada em log para as novas alterações de componente no Windows Server 2012, trate o problema como um problema de emissão de RID clássico (também conhecido como anterior ao Windows Server 2012 e herdado) visto em sistemas operacionais Windows 2008 R2 ou mais antigos.  
  
#### <a name="utilities-and-commands-for-troubleshooting"></a>Utilitários e comandos para solução de problemas  
Para solucionar problemas não explicados por logs mencionados anteriormente — especialmente problemas de emissão de RID mais antigos — use a seguinte lista de ferramentas como um ponto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   Monitor de Rede 3.4  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Metodologia geral para solução de problemas de configuração do controlador de domínio  
  
1.  O erro é causado por um simples problema de permissões ou de disponibilidade do controlador de domínio?  
  
    1.  Você está tentando criar uma entidade de segurança sem as permissões necessárias? Examine se há erros de acesso negado na saída.  
  
    2.  Um controlador de domínio está disponível? Examine o erro retornado ou as mensagens de disponibilidade do controlador de domínio ou do LDAP.  
  
2.  O erro retornado menciona especificamente RIDs, e é específico o suficiente para ser usado como orientação? Se for, siga a orientação.  
  
3.  O erro retornado menciona especificamente RIDs mas, por outro lado, não é específico? Por exemplo, "O Windows não pode criar o objeto porque o Serviço de Diretório não pôde alocar um identificador relativo."  
  
    1.  Examine o log de eventos do sistema no controlador de domínio para eventos de RID "herdados" (anteriores ao Windows Server 2012) detalhados na [solicitação do pool RID](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee406152(v=ws.10)) (16642, 16643, 16644, 16645, 16656).  
  
    2.  Examine se há, no Evento do Sistema no controlador de domínio e no Mestre RID, novos eventos que indiquem bloqueio, detalhados abaixo neste tópico (16655, 16656, 16657).  
  
    3.  Valide a integridade da replicação do Active Directory com Repadmin.exe e a disponibilidade do Mestre RID com **Dcdiag.exe /test:ridmanager /v**. Habilite capturas de rede de dupla face entre o controlador de domínio e o Mestre RID, se esses testes forem inconclusivos.  
  
### <a name="troubleshooting-specific-problems"></a>Solucionando problemas específicos  
As novas mensagens a seguir são registradas em log no log de eventos do sistema nos controladores de domínio do Windows Server 2012. Sistemas de acompanhamento de integridade do AD automatizados, como o System Center Operations Manager, deveriam monitorar esses eventos; todos são importantes e alguns indicam problemas críticos de domínio.  
  
|||  
|-|-|  
|ID do evento|16653|  
|Fonte|Directory-Services-SAM|  
|Severidade|Aviso|  
|Mensagem|Um tamanho de pool para identificadores de conta (RIDs) que foi configurado por um Administrador é maior do que o máximo com suporte. O valor máximo de %1 será usado quando o controlador de domínio for o mestre RID.<p>Para obter mais informações, consulte [Limite de tamanho de bloco RID](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_RIDBlockMaxSize).|  
|Notas e resolução|O valor máximo para o Tamanho de Bloco RID agora é 15.000 decimal (3A98 hexadecimal). Um controlador de domínio não pode solicitar mais de 15.000 RIDs. Esse evento efetua registros em log a cada inicialização, até que o valor seja definido como um valor nesse máximo ou abaixo dele.|  
  
|||  
|-|-|  
|ID do evento|16654|  
|Fonte|Directory-Services-SAM|  
|Severidade|Informativo|  
|Mensagem|Um pool de RIDs (identificadores de conta) foi invalidado. Isso pode ocorrer nos seguintes casos previstos:<p>1. um controlador de domínio é restaurado do backup.<p>2. um controlador de domínio em execução em uma máquina virtual é restaurado do instantâneo.<p>3. um administrador invalidou manualmente o pool.<p>Consulte https://go.microsoft.com/fwlink/?LinkId=226247 para obter mais informações.|  
|Notas e resolução|Se esse evento for inesperado, contate todos os administradores do domínio e determine quais deles executou a ação. O log de eventos dos Serviços de Diretório também contém mais informações sobre quando uma dessas etapas foi executada.|  
  
|||  
|-|-|  
|ID do evento|16655|  
|Fonte|Directory-Services-SAM|  
|Severidade|Informativo|  
|Mensagem|O máximo global para identificadores de conta (RIDs) foi aumentado para %1.|  
|Notas e resolução|Se esse evento for inesperado, contate todos os administradores do domínio e determine quais deles executou a ação. Esse evento registra o aumento do tamanho do pool RID geral além do padrão de 2<sup>30</sup>e não ocorrerá automaticamente; somente por ação administrativa.|  
  
|||  
|-|-|  
|ID do evento|16656|  
|Fonte|Directory-Services-SAM|  
|Severidade|Aviso|  
|Mensagem|O máximo global para identificadores de conta (RIDs) foi aumentado para %1.|  
|Notas e resolução|Ação necessária! Um pool de identificador de conta (RID) foi alocado a esse controlador de domínio. O valor do pool indica que esse domínio consumiu uma parte considerável do total de identificadores de conta disponíveis.<p>Um mecanismo de proteção será ativado quando o domínio atingir o seguinte limite da conta total disponível-identificadores restantes: %1.  O mecanismo de proteção impedirá a criação de conta até que você reabilite manualmente a alocação de identificador de conta no controlador de domínio de mestre RID.<p>Consulte https://go.microsoft.com/fwlink/?LinkId=228610 para obter mais informações.|  
  
|||  
|-|-|  
|ID do evento|16657|  
|Fonte|Directory-Services-SAM|  
|Severidade|Erro|  
|Mensagem|Ação necessária! Esse domínio consumiu uma parte considerável do total de identificadores de conta (RIDs) disponíveis. Um mecanismo de proteção foi ativado porque a conta total disponível-os identificadores restantes é menor que: X% [argumento de teto artificial].<p>O mecanismo de proteção impede a criação de conta até que você reabilite manualmente a alocação de identificador de conta no controlador de domínio de mestre RID.<p>É extremamente importante que certos diagnósticos sejam executados antes da criação de conta ser reabilitada, para assegurar que esse domínio não esteja consumindo identificadores de conta a uma taxa anormalmente alta. Todo problema identificado deve ser resolvido antes da criação da conta ser reabilitada.<p>Falha em diagnosticar e corrigir qualquer problema subjacente que resulte em uma taxa anormalmente alta de consumo do identificador de conta pode levar ao esgotamento de identificadores de conta no domínio. Depois disso, a criação da conta ficará permanentemente desabilitada nesse domínio.<p>Consulte https://go.microsoft.com/fwlink/?LinkId=228610 para obter mais informações.|  
|Notas e resolução|Contate todos os administradores de domínio e informe-os de que mais nenhuma entidade de segurança poderá ser criada nesse domínio até que essa proteção seja substituída. Para obter mais informações sobre como substituir a proteção e provavelmente aumentar o pool RID geral, consulte [Desbloqueio de tamanho de espaço de RID global](../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/../../ad-ds/manage/Managing-RID-Issuance.md#BKMK_GlobalRidSpaceUnlock).|  
  
|||  
|-|-|  
|ID do evento|16658|  
|Fonte|Directory-Services-SAM|  
|Severidade|Aviso|  
|Mensagem|Esse evento é uma atualização periódica na quantidade total restante de identificadores de conta (RIDs) disponíveis. O número de identificadores de conta restantes é aproximadamente: %1.<p>Os identificadores de conta são usados conforme as contas são criadas, quando eles estão esgotados nenhuma conta nova pode ser criada no domínio.<p>Consulte https://go.microsoft.com/fwlink/?LinkId=228745 para obter mais informações.|  
|Notas e resolução|Contate todos os administradores de domínio e informe-os que o consumo de RID ultrapassou o marco principal; determine se esse comportamento é esperado ou não examinando os padrões de criação de objeto de confiança de segurança. Presenciar esse evento seria muito incomum, pois isso significaria que pelo menos ~100 milhões de RIDS foram alocados.|  
  
## <a name="see-also"></a>Consulte Também  
[Gerenciando a emissão de RID no Windows Server 2012](/archive/blogs/askds/managing-rid-issuance-in-windows-server-2012)  
  
