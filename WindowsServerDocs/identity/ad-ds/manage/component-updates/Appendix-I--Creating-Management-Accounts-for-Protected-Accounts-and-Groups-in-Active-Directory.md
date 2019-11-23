---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: Apêndice I-criando contas de gerenciamento para contas e grupos protegidos no Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8880f26acd8b32a4ab8a32ede067d158f2d6aed1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369208"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Apêndice I: Criar o gerenciamento de contas para contas e grupos protegidos no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um dos desafios de implementar um modelo de Active Directory que não depende da associação permanente em grupos altamente privilegiados é que deve haver um mecanismo para popular esses grupos quando a Associação temporária nos grupos é necessária. Algumas soluções com privilégios de gerenciamento de identidade exigem que as contas de serviço do software recebam associação permanente em grupos como DA ou administradores em cada domínio na floresta. No entanto, tecnicamente não é necessário para que as soluções de Privileged Identity Management (PIM) executem seus serviços em contextos altamente privilegiados.  
  
Este apêndice fornece informações que você pode usar para soluções de PIM implementadas nativamente ou de terceiros para criar contas com privilégios limitados e podem ser rigorosamente controladas, mas podem ser usadas para preencher grupos com privilégios no Active Directory quando a elevação temporária é necessária. Se você estiver implementando o PIM como uma solução nativa, essas contas poderão ser usadas pela equipe administrativa para executar a população temporária do grupo e, se você estiver implementando o PIM por meio de software de terceiros, poderá adaptar essas contas para funcionar como serviço as.  
  
> [!NOTE]  
> Os procedimentos descritos neste apêndice fornecem uma abordagem para o gerenciamento de grupos altamente privilegiados no Active Directory. Você pode adaptar esses procedimentos para atender às suas necessidades, adicionar restrições adicionais ou omitir algumas das restrições descritas aqui.  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Criando contas de gerenciamento para contas e grupos protegidos no Active Directory

A criação de contas que podem ser usadas para gerenciar a associação de grupos privilegiados sem exigir que as contas de gerenciamento recebam direitos e permissões excessivas consiste em quatro atividades gerais que são descritas nas instruções passo a passo que dar  
  
1.  Primeiro, você deve criar um grupo que gerenciará as contas, pois essas contas devem ser gerenciadas por um conjunto limitado de usuários confiáveis. Se você ainda não tiver uma estrutura de UO que acomode segregando contas e sistemas com privilégios e protegidos da população geral no domínio, deverá criar um. Embora não sejam fornecidas instruções específicas neste apêndice, as capturas de tela mostram um exemplo dessa hierarquia de UO.  
  
2.  Crie as contas de gerenciamento. Essas contas devem ser criadas como contas de usuário "regulares" e não recebem direitos de usuário além daqueles que já foram concedidos aos usuários por padrão.  
  
3.  Implemente restrições nas contas de gerenciamento que as tornem utilizáveis somente para a finalidade especializada para a qual foram criadas, além de controlar quem pode habilitar e usar as contas (o grupo que você criou na primeira etapa).  
  
4.  Configure permissões no objeto AdminSDHolder em cada domínio para permitir que as contas de gerenciamento alterem a Associação dos grupos privilegiados no domínio.  
  
Você deve testar exaustivamente todos esses procedimentos e modificá-los conforme necessário para seu ambiente antes de implementá-los em um ambiente de produção. Você também deve verificar se todas as configurações funcionam conforme o esperado (alguns procedimentos de teste são fornecidos neste apêndice) e você deve testar um cenário de recuperação de desastre no qual as contas de gerenciamento não estão disponíveis para serem usadas para preencher os grupos protegidos para recuperação metas. Para obter mais informações sobre como fazer backup e restaurar Active Directory, consulte o guia passo a passo de [backup e recuperação do AD DS](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx).  
  
> [!NOTE]  
> Ao implementar as etapas descritas neste apêndice, você criará contas que poderão gerenciar a associação de todos os grupos protegidos em cada domínio, não apenas os grupos Active Directory de maior privilégio, como EAs, DAs e BAs. Para obter mais informações sobre grupos protegidos no Active Directory, consulte o [Apêndice C: contas e grupos protegidos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Instruções passo a passo para criar contas de gerenciamento para grupos protegidos  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Criando um grupo para habilitar e desabilitar contas de gerenciamento

As contas de gerenciamento devem ter suas senhas redefinidas a cada uso e devem ser desabilitadas quando as atividades exigidas forem concluídas. Embora você também possa considerar a implementação de requisitos de logon de cartão inteligente para essas contas, é uma configuração opcional e essas instruções pressupõem que as contas de gerenciamento serão configuradas com um nome de usuário e uma senha longa e complexa como mínima controles. Nesta etapa, você criará um grupo que tem permissões para redefinir a senha nas contas de gerenciamento e para habilitar e desabilitar as contas.  
  
Para criar um grupo para habilitar e desabilitar contas de gerenciamento, execute as seguintes etapas:  
  
1.  Na estrutura da UO em que você estará hospedando as contas de gerenciamento, clique com o botão direito do mouse na UO em que você deseja criar o grupo, clique em **novo** e em **grupo**.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  Na caixa de diálogo **novo grupo de objetos** , insira um nome para o grupo. Se você planeja usar esse grupo para "ativar" todas as contas de gerenciamento em sua floresta, transforme-a em um grupo de segurança universal. Se você tiver uma floresta de domínio único ou se planejar criar um grupo em cada domínio, poderá criar um grupo de segurança global. Clique em **OK** para criar o grupo.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  Clique com o botão direito do mouse no grupo que você acabou de criar, clique em **Propriedades**e clique na guia **objeto** . Na caixa de diálogo **propriedade de objeto** do grupo, selecione **proteger objeto contra exclusão acidental**, o que não apenas impedirá que usuários autorizados de outra forma excluam o grupo, mas também de movê-lo para outra UO, a menos que o atributo seja primeiro desmarcado.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > Se você já tiver configurado permissões nas UOs pai do grupo para restringir a administração a um conjunto limitado de usuários, talvez não seja necessário executar as etapas a seguir. Eles são fornecidos aqui para que, mesmo que você ainda não tenha implementado controle administrativo limitado sobre a estrutura da UO na qual você criou esse grupo, você pode proteger o grupo contra modificações por usuários não autorizados.  
  
4.  Clique na guia **Membros** e adicione as contas para membros da sua equipe que serão responsáveis por habilitar contas de gerenciamento ou preencher grupos protegidos quando necessário.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Se você ainda não tiver feito isso, no console **Active Directory usuários e computadores** , clique em **Exibir** e selecione **recursos avançados**. Clique com o botão direito do mouse no grupo que você acabou de criar, clique em **Propriedades**e clique na guia **segurança** . Na guia **segurança** , clique em **avançado**.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  Na caixa de diálogo **configurações de segurança avançadas para [grupo]** , clique em **desabilitar herança**. Quando solicitado, clique em **converter permissões herdadas em permissões explícitas neste objeto**e clique em **OK** para retornar à caixa de diálogo **segurança** do grupo.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  Na guia **segurança** , remova os grupos que não devem ter permissão para acessar esse grupo. Por exemplo, se você não quiser que os usuários autenticados possam ler o nome do grupo e as propriedades gerais, poderá remover essa ACE. Você também pode remover ACEs, como aquelas para operadores de conta e acesso compatível com o servidor anterior ao Windows 2000. No entanto, você deve deixar um conjunto mínimo de permissões de objeto em vigor. Deixe as seguintes ACEs intactas:  
  
    -   AUTO-restauração  
  
    -   SISTEMA  
  
    -   Administradores do domínio  
  
    -   Administrador corporativo  
  
    -   Administradores  
  
    -   Grupo de acesso de autorização do Windows (se aplicável)  
  
    -   CONTROLADORES DE DOMÍNIO DA EMPRESA  
  
    Embora possa parecer preintuitivo permitir que os grupos com maior privilégio no Active Directory gerenciem esse grupo, seu objetivo na implementação dessas configurações não é impedir que os membros desses grupos façam alterações autorizadas. Em vez disso, o objetivo é garantir que, quando você tiver ocasiões de exigir níveis muito altos de privilégio, as alterações autorizadas terão sucesso. É por esse motivo que a alteração de aninhamento de grupo privilegiado padrão, direitos e permissões não é recomendável em todo este documento. Deixando as estruturas padrão intactas e esvaziando a Associação dos grupos de privilégios mais altos no diretório, você poderá criar um ambiente mais seguro que ainda funcione conforme o esperado.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Se você ainda não tiver configurado políticas de auditoria para os objetos na estrutura da UO em que você criou esse grupo, configure a auditoria para registrar as alterações desse grupo.  
  
8.  Você concluiu a configuração do grupo que será usado para "fazer check-out" das contas de gerenciamento quando elas forem necessárias e "fazer check-in" das contas quando suas atividades forem concluídas.  
  
#### <a name="creating-the-management-accounts"></a>Criando as contas de gerenciamento

Você deve criar pelo menos uma conta que será usada para gerenciar a associação de grupos com privilégios em sua instalação do Active Directory e, preferencialmente, uma segunda conta para servir como um backup. Se você optar por criar as contas de gerenciamento em um único domínio na floresta e conceder a eles recursos de gerenciamento para todos os grupos protegidos de domínios, ou se optar por implementar contas de gerenciamento em cada domínio na floresta, os procedimentos serão efetivamente o mesmo.  
  
> [!NOTE]  
> As etapas neste documento pressupõem que você ainda não implementou controles de acesso baseado em função e o Privileged Identity Management para Active Directory. Portanto, alguns procedimentos devem ser executados por um usuário cuja conta seja membro do grupo Admins. do domínio para o domínio em questão.  
>   
> Ao usar uma conta com privilégios DA, você pode fazer logon em um controlador de domínio para executar as atividades de configuração. As etapas que não exigem privilégios DA a podem ser executadas por contas com menos privilégios que estão conectadas a estações de trabalho administrativas. Capturas de tela que mostram caixas de diálogo com borda na cor azul mais clara representam atividades que podem ser executadas em um controlador de domínio. Capturas de tela que mostram caixas de diálogo na cor azul mais escura representam atividades que podem ser executadas em estações de trabalho administrativas com contas que têm privilégios limitados.  
  
Para criar as contas de gerenciamento, execute as seguintes etapas:  
  
1. Faça logon em um controlador de domínio com uma conta que seja membro do grupo DA dos do domínio.  

2. Inicie **Active Directory usuários e computadores** e navegue até a UO em que você criará a conta de gerenciamento.  

3. Clique com o botão direito do mouse na UO e clique em **novo** e em **usuário**.  

4. Na caixa de diálogo **novo objeto – usuário** , insira as informações de nomenclatura desejadas para a conta e clique em **Avançar**.  

   ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. Forneça uma senha inicial para a conta de usuário, desmarque o **usuário deve alterar a senha no próximo logon**, selecione o **usuário não pode alterar a senha** e a **conta está desabilitada**e clique em **Avançar**.  

   ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. Verifique se os detalhes da conta estão corretos e clique em **concluir**.  

7. Clique com o botão direito do mouse no objeto de usuário que você acabou de criar e clique em **Propriedades**.  

8. Clique na guia **conta** .  

9. No campo **Opções de conta** , selecione o sinalizador a **conta é confidencial e não pode ser delegado** , selecione **essa conta dá suporte à criptografia Kerberos AES 128 bits** e/ou a **seguinte conta dá suporte ao sinalizador de criptografia Kerberos AES 256** e clique em **OK**.  

   ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > Como essa conta, como outras contas, terá uma função limitada, mas poderosa, a conta só deve ser usada em hosts administrativos seguros. Para todos os hosts administrativos seguros em seu ambiente, você deve considerar a implementação de Política de Grupo configuração de **segurança de rede: configurar tipos de criptografia permitidos para Kerberos** para permitir apenas os tipos de criptografia mais seguros que você pode implementar para hosts seguros.  
   >
   > Embora a implementação de tipos de criptografia mais seguras para os hosts não atenue ataques de roubo de credenciais, o uso apropriado e a configuração dos hosts seguros têm. A configuração de tipos de criptografia mais fortes para hosts que são usados apenas por contas com privilégios simplesmente reduz a superfície de ataque geral dos computadores.  
   >
   > Para obter mais informações sobre como configurar tipos de criptografia em sistemas e contas, consulte [configurações do Windows para tipo de criptografia com suporte para Kerberos](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
   >
   > Essas configurações têm suporte apenas em computadores que executam o Windows Server 2012, Windows Server 2008 R2, Windows 8 ou Windows 7.  
  
10. Na guia **objeto** , selecione **proteger objeto contra exclusão acidental**. Isso não apenas impedirá que o objeto seja excluído (mesmo por usuários autorizados), mas impedirá que ele seja movido para uma UO diferente em sua hierarquia de AD DS, a menos que a caixa de seleção seja desmarcada primeiro por um usuário com permissão para alterar o atributo.  

    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. Clique na guia **controle remoto** .  

12. Desmarque o sinalizador **habilitar controle remoto** . Ele nunca deve ser necessário para que a equipe de suporte se conecte às sessões desta conta para implementar correções.  

    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

    > [!NOTE]  
    > Cada objeto no Active Directory deve ter um proprietário de ti designado e um proprietário de negócios designado, conforme descrito em [planejando o comprometimento](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Se você estiver controlando a propriedade de objetos de AD DS no Active Directory (em oposição a um banco de dados externo), deverá inserir informações de propriedade apropriadas nas propriedades desse objeto.  
    >
    > Nesse caso, o proprietário da empresa é provavelmente uma divisão de ti, andthere não é proibição dos proprietários de negócios também ser proprietários de ti. O ponto de estabelecimento da propriedade de objetos é permitir que você identifique os contatos quando as alterações precisam ser feitas aos objetos, talvez anos a partir de sua criação inicial.  

13. Clique na guia **organização** .  

14. Insira todas as informações necessárias em seus padrões de objeto AD DS.  

    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. Clique na guia **discar** .  

16. No campo **permissão de acesso à rede** , selecione **negar acesso**. Essa conta nunca deve precisar se conectar por meio de uma conexão remota.  

    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

    > [!NOTE]  
    > É improvável que essa conta seja usada para fazer logon em controladores de domínio somente leitura (RODCs) em seu ambiente. No entanto, caso a circunstância exija a conta para fazer logon em um RODC, você deve adicionar essa conta ao grupo de replicação de senha do RODC negado para que sua senha não seja armazenada em cache no RODC.  
    >
    > Embora a senha da conta deva ser redefinida após cada uso e a conta seja desabilitada, a implementação dessa configuração não tem um efeito deletério na conta e pode ajudar em situações em que um administrador se esqueça de redefinir a conta senha e desabilite-a.  

17. Clique na guia **Membro de**.  

18. Clique em **Adicionar**.  

19. Digite **grupo de replicação de senha do RODC negado** na caixa de diálogo **Selecionar usuários, contatos, computadores** e clique em **verificar nomes**. Quando o nome do grupo for sublinhado no seletor de objetos, clique em **OK** e verifique se a conta agora é membro dos dois grupos exibidos na captura de tela a seguir. Não adicione a conta a nenhum grupo protegido.  

20. Clique em **OK**.  

    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. Clique na guia **segurança** e clique em **avançado**.  

22. Na caixa de diálogo **configurações de segurança avançadas** , clique em **desabilitar herança** e copie as permissões herdadas como permissões explícitas e clique em **Adicionar**.  

    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. Na **entrada permissão para a caixa de diálogo [conta]** , clique em **selecionar uma entidade de segurança** e adicione o grupo criado no procedimento anterior. Role até a parte inferior da caixa de diálogo e clique em **limpar tudo** para remover todas as permissões padrão.  

    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. Role até a parte superior da caixa de diálogo **entrada de permissão** . Verifique se a lista suspensa **tipo** está definida como **permitir**e, na lista suspensa **aplica-se a** , selecione **este objeto somente**.  

25. No campo **permissões** , selecione **ler todas as propriedades**, **permissões de leitura**e **Redefinir senha**.  

    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. No campo **Propriedades** , selecione **ler userAccountControl** e **gravar userAccountControl**.  

27. Clique em **OK**, **OK** novamente na caixa de diálogo **configurações de segurança avançadas** .  

    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

    > [!NOTE]  
    > O atributo **userAccountControl** controla várias opções de configuração de conta. Não é possível conceder permissão para alterar apenas algumas das opções de configuração quando você concede permissão de gravação ao atributo.  

28. No campo **nomes de grupo ou de usuário** da guia **segurança** , remova todos os grupos que não devem ter permissão para acessar ou gerenciar a conta. Não remova todos os grupos que foram configurados com ACEs Deny, como o grupo Everyone e a conta autocalculada (essa ACE foi definida quando o sinalizador **usuário não pode alterar a senha** foi habilitado durante a criação da conta. Além disso, não remova o grupo que você acabou de adicionar, a conta do sistema ou grupos como EA, DA, BA ou o grupo de acesso de autorização do Windows.  

    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. Clique em **avançado** e verifique se a caixa de diálogo Configurações de segurança avançadas é semelhante à captura de tela a seguir.  

30. Clique em **OK**e em **OK** novamente para fechar a caixa de diálogo de propriedade da conta.  

    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. A instalação da primeira conta de gerenciamento agora está concluída. Você testará a conta em um procedimento posterior.  

##### <a name="creating-additional-management-accounts"></a>Criando contas de gerenciamento adicionais

Você pode criar contas de gerenciamento adicionais repetindo as etapas anteriores, copiando a conta que você acabou de criar ou criando um script para criar contas com as definições de configuração desejadas. No entanto, observe que, se você copiar a conta que acabou de criar, muitas das configurações e ACLs personalizadas não serão copiadas para a nova conta e você precisará repetir a maioria das etapas de configuração.  
  
Em vez disso, você pode criar um grupo ao qual você delega direitos para preencher e não preencher grupos protegidos, mas você precisará proteger o grupo e as contas que você coloca nele. Como deve haver muito poucas contas em seu diretório que recebam a capacidade de gerenciar a associação de grupos protegidos, a criação de contas individuais pode ser a abordagem mais simples.  
  
Independentemente de como você opta por criar um grupo no qual você coloca as contas de gerenciamento, deve garantir que cada conta seja protegida conforme descrito anteriormente. Você também deve considerar a implementação de restrições de GPO semelhantes àquelas descritas no [Apêndice D: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
##### <a name="auditing-management-accounts"></a>Contas de gerenciamento de auditoria

Você deve configurar a auditoria na conta para registrar, no mínimo, todas as gravações na conta. Isso permitirá que você não só identifique a habilitação bem-sucedida da conta e a redefinição de sua senha durante os usos autorizados, mas também para identificar tentativas por usuários não autorizados a manipular a conta. As gravações com falha na conta devem ser capturadas em seu sistema SIEM (monitoramento de eventos e informações de segurança) (se aplicável) e devem disparar alertas que fornecem notificação à equipe responsável por investigar possíveis comprometimentos.  
  
As soluções SIEM tomam informações de eventos de fontes de segurança envolvidas (por exemplo, logs de eventos, dados de aplicativos, fluxos de rede, produtos antimalware e fontes de detecção de intrusão), agrupam os dados e tentam fazer exibições inteligentes e ações proativas . Há muitas soluções de SIEM comercial, e muitas empresas criam implementações privadas. Um SIEM bem projetado e implementado adequadamente pode aprimorar significativamente os recursos de monitoramento de segurança e de resposta a incidentes. No entanto, os recursos e a precisão variam enormemente entre as soluções. Os SIEM estão além do escopo deste documento, mas as recomendações de eventos específicas contidas devem ser consideradas por qualquer implementador de SIEM.  
  
Para obter mais informações sobre as definições de configuração de auditoria recomendadas para controladores de domínio, consulte [monitoramento Active Directory para sinais de comprometimento](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). As definições de configuração específicas do controlador de domínio são fornecidas no [monitoramento Active Directory para sinais de comprometimento](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>Habilitando contas de gerenciamento para modificar a associação de grupos protegidos

Neste procedimento, você configurará permissões no objeto AdminSDHolder do domínio para permitir que as contas de gerenciamento recém-criadas modifiquem a associação de grupos protegidos no domínio. Este procedimento não pode ser executado por meio de uma GUI (interface gráfica do usuário).  
  
Conforme discutido no [Apêndice C: contas e grupos protegidos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), a ACL no objeto AdminSDHolder de um domínio é efetivamente "copiada" para objetos protegidos quando a tarefa SDProp é executada. Grupos e contas protegidos não herdam suas permissões do objeto AdminSDHolder; suas permissões são definidas explicitamente para corresponder às do objeto AdminSDHolder. Portanto, quando você modifica permissões no objeto AdminSDHolder, você deve modificá-las para atributos que são apropriados para o tipo do objeto protegido que você está direcionando.  
  
Nesse caso, você concederá as contas de gerenciamento recém-criadas para permitir que elas leiam e gravem o atributo Members em objetos de grupo. No entanto, o objeto AdminSDHolder não é um objeto de grupo e os atributos de grupo não são expostos no editor de ACL gráfica. Por esse motivo, você implementará as alterações de permissões por meio do utilitário de linha de comando Dsacls. Para conceder as permissões de contas de gerenciamento (desabilitadas) para modificar a associação de grupos protegidos, execute as seguintes etapas:  
  
1. Faça logon em um controlador de domínio, preferivelmente o controlador de domínio que contém a função do emulador de PDC (PDCE), com as credenciais de uma conta de usuário que se tornou membro do grupo DA no domínio.  
  
   ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2. Abra um prompt de comando com privilégios elevados clicando com o botão direito do mouse em **prompt de comando** e clique em **Executar como administrador**.  
  
   ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3. Quando for solicitado a aprovar a elevação, clique em **Sim**.  
  
   ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
   > [!NOTE]  
   > Para obter mais informações sobre elevação e controle de conta de usuário (UAC) no Windows, consulte [processos e interações do UAC](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) no site do TechNet.  
  
4. No prompt de comando, digite (substituindo suas informações específicas de domínio) **Dsacls [nome diferenciado do objeto AdminSDHolder em seu domínio]/g [UPN da conta de gerenciamento]: RPWP; membro**.  
  
   ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
   O comando anterior (que não diferencia maiúsculas de minúsculas) funciona da seguinte maneira:  
  
   - Dsacls define ou exibe ACEs em objetos de diretório  
  
   - CN = AdminSDHolder, CN = System, DC = TailSpinToys, DC = MSFT identifica o objeto a ser modificado  
  
   - /G indica que uma ACE de concessão está sendo configurada  
  
   - PIM001@tailspintoys.msft é o UPN (nome principal do usuário) da entidade de segurança à qual as ACEs serão concedidas  
  
   - RPWP concede as permissões Read Property e Write Property  
  
   - Member é o nome da propriedade (atributo) na qual as permissões serão definidas  
  
   Para obter mais informações sobre o uso de **Dsacls**, digite Dsacls sem nenhum parâmetro em um prompt de comando.  
  
   Se você tiver criado várias contas de gerenciamento para o domínio, deverá executar o comando Dsacls para cada conta. Depois de concluir a configuração de ACL no objeto AdminSDHolder, você deve forçar a execução de SDProp ou aguardar até que sua execução agendada seja concluída. Para obter informações sobre como forçar a execução de SDProp, consulte "executando SDProp manualmente" no [Apêndice C: contas e grupos protegidos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
   Quando SDProp tiver sido executado, você poderá verificar se as alterações feitas no objeto AdminSDHolder foram aplicadas a grupos protegidos no domínio. Não é possível verificar isso exibindo a ACL no objeto AdminSDHolder pelos motivos descritos anteriormente, mas você pode verificar se as permissões foram aplicadas exibindo as ACLs em grupos protegidos.  
  
5. Em **Active Directory usuários e computadores**, verifique se você habilitou os **recursos avançados**. Para fazer isso, clique em **Exibir**, localize o grupo **Admins** . do domínio, clique com o botão direito do mouse no grupo e clique em **Propriedades**.  
  
6. Clique na guia **segurança** e clique em **avançado** para abrir a caixa de diálogo **configurações de segurança avançadas para admins** . do domínio.  
  
   ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7. Selecione **permitir Ace para a conta de gerenciamento** e clique em **Editar**. Verifique se a conta recebeu apenas as permissões **ler Membros** e **gravar Membros** no grupo da e clique em **OK**.  
  
8. Clique em **OK** na caixa de diálogo **configurações de segurança avançadas** e clique em **OK** novamente para fechar a caixa de diálogo de Propriedade do grupo da.  
  
   ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. Você pode repetir as etapas anteriores para outros grupos protegidos no domínio; as permissões devem ser as mesmas para todos os grupos protegidos. Agora você concluiu a criação e a configuração das contas de gerenciamento para os grupos protegidos neste domínio.  
  
    > [!NOTE]  
    > Qualquer conta que tenha permissão para gravar a associação de um grupo no Active Directory também pode se adicionar ao grupo. Esse comportamento é por design e não pode ser desabilitado. Por esse motivo, você sempre deve manter as contas de gerenciamento desabilitadas quando não estiver em uso e deve monitorar de forma precisa as contas quando elas estiverem desabilitadas e quando estiverem em uso.  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>Verificando as definições de configuração de grupo e conta

Agora que você criou e configurou contas de gerenciamento que podem modificar a associação de grupos protegidos no domínio (que inclui os grupos EA, DA e BA mais altamente privilegiados), você deve verificar se as contas e seu grupo de gerenciamento foram criado corretamente. A verificação consiste nessas tarefas gerais:  
  
1.  Teste o grupo que pode habilitar e desabilitar contas de gerenciamento para verificar se os membros do grupo podem habilitar e desabilitar as contas e redefinir suas senhas, mas não podem executar outras atividades administrativas nas contas de gerenciamento.  
  
2.  Teste as contas de gerenciamento para verificar se elas podem adicionar e remover membros de grupos protegidos no domínio, mas não podem alterar outras propriedades de contas e grupos protegidos.  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>Testar o grupo que habilitará e desabilitará as contas de gerenciamento
  
1.  Para testar a habilitação de uma conta de gerenciamento e redefinir sua senha, faça logon em uma estação de trabalho administrativa segura com uma conta que seja membro do grupo que você criou no [Apêndice I: Criando contas de gerenciamento para contas e grupos protegidos no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Abra **Active Directory usuários e computadores**, clique com o botão direito do mouse na conta de gerenciamento e clique em **habilitar conta**.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Uma caixa de diálogo deve ser exibida, confirmando se a conta foi habilitada.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  Em seguida, redefina a senha na conta de gerenciamento. Para fazer isso, clique com o botão direito do mouse na conta novamente e clique em **Redefinir senha**.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Digite uma nova senha para a conta nos campos **nova senha** e **Confirmar senha** e clique em **OK**.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Será exibida uma caixa de diálogo confirmando que a senha da conta foi redefinida.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Agora, tente modificar as propriedades adicionais da conta de gerenciamento. Clique com o botão direito do mouse na conta e clique em **Propriedades**e clique na guia **controle remoto** .  
  
8.  Selecione **habilitar controle remoto** e clique em **aplicar**. A operação deve falhar e uma mensagem de erro de **acesso negado** deve ser exibida.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Clique na guia **conta** da conta e tente alterar o nome da conta, o horário de logon ou as estações de trabalho de logon. Todos devem falhar e as opções de conta que não são controladas pelo atributo **userAccountControl** devem estar esmaecidas e indisponíveis para modificação.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Tente adicionar o grupo de gerenciamento a um grupo protegido, como o grupo DA. Quando você clica em **OK**, uma mensagem deve ser exibida, informando que você não tem permissões para modificar o grupo.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Execute testes adicionais conforme necessário para verificar se você não pode configurar nada na conta de gerenciamento, exceto configurações de **userAccountControl** e redefinições de senha.  
  
    > [!NOTE]  
    > O atributo **userAccountControl** controla várias opções de configuração de conta. Não é possível conceder permissão para alterar apenas algumas das opções de configuração quando você concede permissão de gravação ao atributo.  
  
##### <a name="test-the-management-accounts"></a>Testar as contas de gerenciamento

Agora que você habilitou uma ou mais contas que podem alterar a associação de grupos protegidos, você pode testar as contas para garantir que elas possam modificar a associação de grupo protegida, mas não podem executar outras modificações em contas e grupos protegidos.  
  
1.  Faça logon em um host administrativo seguro como a primeira conta de gerenciamento.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Inicie **Active Directory usuários e computadores** e localize o **grupo Admins**. do domínio.  
  
3.  Clique com o botão direito do mouse no grupo **Admins** . do domínio e clique em **Propriedades**.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  Nas **Propriedades admins**. do domínio, clique na guia **Membros** e **clique em** adicionar. Insira o nome de uma conta que receberá privilégios de administradores de domínio temporários e clique em **verificar nomes**. Quando o nome da conta for sublinhado, clique em **OK** para retornar à guia **Membros** .  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  Na guia **Membros** da caixa de diálogo **Propriedades de admins** . do domínio, clique em **aplicar**. Depois de clicar em **aplicar**, a conta deverá permanecer como um membro do grupo da e você não deverá receber mensagens de erro.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Clique na guia **gerenciado por** na caixa de diálogo **Propriedades de admins** . do domínio e verifique se você não pode inserir texto em nenhum campo e se todos os botões estão esmaecidos.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  Clique na guia **geral** na caixa de diálogo **Propriedades de admins** . do domínio e verifique se você não pode modificar nenhuma das informações sobre essa guia.  
  
    ![Criando contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Repita essas etapas para grupos protegidos adicionais, conforme necessário. Quando tiver terminado, faça logon em um host administrativo seguro com uma conta que seja membro do grupo que você criou para habilitar e desabilitar as contas de gerenciamento. Em seguida, redefina a senha na conta de gerenciamento que você acabou de testar e desabilite a conta. Você concluiu a instalação das contas de gerenciamento e do grupo que será responsável por habilitar e desabilitar as contas.  
