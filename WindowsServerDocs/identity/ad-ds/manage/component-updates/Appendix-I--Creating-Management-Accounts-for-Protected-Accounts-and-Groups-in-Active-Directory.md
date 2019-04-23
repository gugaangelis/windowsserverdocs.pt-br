---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: Apêndice I - criação de contas de gerenciamento de contas protegidas e grupos no Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c71b96f6c44cfc2b14b4c5d203f876e55cc728ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855627"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Apêndice i: Criando o gerenciamento de contas para contas protegidas e grupos no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um dos desafios na implementação de um modelo do Active Directory que não depende da associação permanente em grupos altamente privilegiados é que deve haver um mecanismo para preencher esses grupos quando a associação temporária em grupos é necessária. Algumas soluções de gerenciamento de identidades com privilégios exigem que as contas de serviço do software são concedidas a associação permanente em grupos como administradores em cada domínio na floresta ou o Acelerador de desenvolvimento. No entanto, tecnicamente não é necessário para soluções de Privileged Identity Management (PIM) executar seus serviços em tais contextos altamente privilegiados.  
  
Este apêndice fornece informações que você pode usar para soluções PIM nativamente implementadas ou de terceiros para criar as contas que têm privilégios limitados e podem ser controladas de forma rigorosa, mas podem ser usadas para preencher grupos privilegiados no Active Directory quando elevação temporária é necessária. Se você estiver implementando PIM como uma solução nativa, essas contas podem ser usadas pela equipe administrativa para realizar a população de grupo temporário, e se você estiver implementando PIM por meio do software de terceiros, você poderá adaptar essas contas para funcionar como serviço contas.  
  
> [!NOTE]  
> Os procedimentos descritos neste apêndice fornecem uma abordagem para o gerenciamento de grupos altamente privilegiados no Active Directory. Você pode adaptar esses procedimentos para atender às suas necessidades, adicione restrições adicionais ou omitir algumas das restrições que são descritas aqui.  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Criando o gerenciamento de contas para contas protegidas e grupos no Active Directory

Criação de contas que pode ser usado para gerenciar a associação de grupos privilegiados sem exigir que as contas de gerenciamento para receber permissões e direitos em excesso consiste em quatro atividades gerais descritas as instruções passo a passo que execute:  
  
1.  Primeiro, você deve criar um grupo que gerenciará as contas, porque essas contas devem ser gerenciadas por um conjunto limitado de usuários confiáveis. Se você ainda não tiver uma estrutura de UO que acomoda Segregando contas privilegiadas e protegidas e os sistemas da população geral no domínio, você deve criar um. Embora as instruções específicas não são fornecidas neste apêndice, capturas de tela mostram um exemplo de tal uma hierarquia de UO.  
  
2.  Crie as contas de gerenciamento. Essas contas devem ser criadas como contas de usuário "normal" e concedidas sem direitos de usuário além daquelas que já são concedidas aos usuários por padrão.  
  
3.  Implementar restrições nas contas de gerenciamento que os tornam úteis apenas para fins de especializado para a qual eles foram criados, além de controlar quem pode habilitar e usar as contas (o grupo que você criou na primeira etapa).  
  
4.  Configure permissões no objeto AdminSDHolder em cada domínio para permitir que as contas de gerenciamento alterar a associação de grupos privilegiados no domínio.  
  
Cuidadosamente, você deve testar todos esses procedimentos e modificá-los conforme necessário para seu ambiente antes de implementá-los em um ambiente de produção. Você também deve verificar que todas as configurações funcionam como esperado (alguns procedimentos de teste são fornecidos neste apêndice), e você deve testar um cenário de recuperação de desastre no qual as contas de gerenciamento não estão disponíveis a ser usada para popular grupos protegidos para a recuperação finalidades. Para obter mais informações sobre backup e restauração do Active Directory, consulte o [Backup do AD DS e o guia passo a passo de recuperação](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx).  
  
> [!NOTE]  
> Ao implementar as etapas descritas neste apêndice, você criará contas que poderão ser capazes de gerenciar a participação de todos os grupos protegidos em cada domínio, não apenas os grupos de Active Directory de privilégio mais alto, como EAs, DAs e BAs. Para obter mais informações sobre grupos protegidos no Active Directory, consulte [apêndice c: Grupos no Active Directory e contas protegidas](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Instruções passo a passo para a criação de contas de gerenciamento para grupos protegidos  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Criação de um grupo para habilitar e desabilitar contas de gerenciamento

Contas de gerenciamento devem ter suas senhas redefinir cada uso e devem ser desabilitadas quando atividades exigir que eles estiverem concluídas. Embora você também pode considerar a implementação de requisitos de logon de cartão inteligente para essas contas, é uma configuração opcional, e essas instruções supõem que as contas de gerenciamento serão configuradas com um nome de usuário e uma senha longa e complexa, como mínimo controles. Nesta etapa, você criará um grupo que tenha permissões para redefinir a senha nas contas de gerenciamento e para habilitar e desabilitar as contas.  
  
Para criar um grupo para habilitar e desabilitar contas de gerenciamento, execute as seguintes etapas:  
  
1.  Na estrutura de UO em que você irá hospedar as contas de gerenciamento, clique com botão direito a UO em que você deseja criar o grupo, clique em **New** e clique em **grupo**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  No **novo objeto - grupo** caixa de diálogo, digite um nome para o grupo. Se você planeja usar esse grupo "Ativar" todas as contas de gerenciamento em sua floresta, verifique um grupo de segurança universal. Se você tiver uma floresta de domínio único ou se você planeja criar um grupo em cada domínio, você pode criar um grupo de segurança global. Clique em **OK** para criar o grupo.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  Clique com o botão direito do mouse no grupo recém-criado, clique em **Propriedades** e na guia **Objeto**. No grupo de **propriedade de objeto** caixa de diálogo, selecione **proteger objeto contra exclusão acidental**, que não somente impedirá que usuários autorizados de outra forma excluam o grupo, mas também de movê-lo para outra UO, a menos que o atributo é o primeiro desmarcada.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > Se você já tiver configurado as permissões no pai do grupo UOs para restringir a administração para um conjunto limitado de usuários, você talvez não precise executar as etapas a seguir. Elas são fornecidas aqui, de modo que, mesmo se você ainda não implementou o controle administrativo limitado sobre a estrutura de UO em que você criou esse grupo, você pode proteger o grupo contra modificação por usuários não autorizados.  
  
4.  Clique o **membros** guia e adicionar as contas para membros da equipe que serão responsáveis por habilitar contas de gerenciamento ou preenchendo protegido grupos quando necessário.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Se você ainda não fez isso, além de **Active Directory Users and Computers** console, clique em **exibição** e selecione **recursos avançados**. Clique no grupo que você acabou de criar, clique em **propriedades**e clique no **segurança** guia. Na guia **Segurança** , clique em **Avançado**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  No **configurações de segurança avançadas para [Group]** caixa de diálogo, clique em **desabilitar herança**. Quando solicitado, clique em **converter permissões de herança em permissões explícitas nesse objeto**e clique em **Okey** para retornar para o grupo **segurança** caixa de diálogo.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  Sobre o **segurança** guia, remova grupos que não devem ter permissão para acessar esse grupo. Por exemplo, se você não quiser que os usuários autenticados possam ler o nome e as propriedades gerais do grupo, você pode remover essa ACE. Você também pode remover as ACEs, como aquelas para operadores e versões anteriores ao Windows 2000 Server compatível com acesso à conta. No entanto, você deve, deixe um conjunto mínimo de permissões de objeto no lugar. As seguintes ACEs deixe intacto:  
  
    -   SELF  
  
    -   SISTEMA  
  
    -   Administradores do domínio  
  
    -   Administrador corporativo  
  
    -   Administradores  
  
    -   Grupo de acesso de autorização Windows (se aplicável)  
  
    -   CONTROLADORES DE DOMÍNIO DA EMPRESA  
  
    Embora possa parecer contraintuitivo permitir os mais altos grupos privilegiados no Active Directory para gerenciar esse grupo, seu objetivo na implementação dessas configurações é não impedir que os membros desses grupos autorizados de alterações. Em vez disso, a meta é garantir que, quando você tem a não precisam de muito altos níveis de privilégio, alterações autorizadas tenham êxito. É por esse motivo que a alteração padrão com privilégios de aninhamento de grupos e direitos, e permissões não são aconselhadas ao longo deste documento. Ao deixar as estruturas padrão intactos e esvaziando a associação dos grupos de privilégio mais altos no diretório, você pode criar um ambiente mais seguro que ainda funciona conforme o esperado.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Se já não tiver configurado políticas de auditoria para os objetos na estrutura de UO em que você criou esse grupo, você deve configurar a auditoria para registrar as alterações desse grupo.  
  
8.  Você concluiu a configuração do grupo que será usado para "check-out" gerenciamento de contas quando eles forem necessários e "check-in" as contas quando suas atividades tiverem sido concluídas.  
  
#### <a name="creating-the-management-accounts"></a>Criação de contas de gerenciamento

Você deve criar pelo menos uma conta que será usada para gerenciar a associação de grupos privilegiados na instalação do Active Directory e, preferencialmente uma segunda conta para servir como um backup. Se você optar por criar as contas de gerenciamento em um único domínio na floresta e conceder a eles recursos de gerenciamento para todos os domínios a grupos protegidos, ou se você optar por implementar contas de gerenciamento em cada domínio na floresta, os procedimentos são efetivamente o mesmo.  
  
> [!NOTE]  
> As etapas neste documento pressupõem que você ainda não implementou os controles de acesso baseado em função e privileged identity management para Active Directory. Portanto, alguns procedimentos devem ser executados por um usuário cuja conta é membro do grupo Admins. do domínio para o domínio em questão.  
>   
> Quando você estiver usando uma conta com privilégios de acelerador de desenvolvimento, você pode fazer logon um controlador de domínio para executar as atividades de configuração. As etapas que não exigem privilégios de acelerador de desenvolvimento podem ser executadas pelas contas com menos privilégios que são registradas estações de trabalho administrativas. Capturas de tela que mostram caixas de diálogo com a cor azul clara representam atividades que podem ser executadas em um controlador de domínio. Capturas de tela que mostram caixas de diálogo na cor azul mais escura representam atividades que podem ser executadas em estações de trabalho administrativas com as contas que têm privilégios limitados.  
  
Para criar as contas de gerenciamento, execute as seguintes etapas:  
  
1. Faça logon no controlador de domínio com uma conta que seja membro do grupo de acelerador de desenvolvimento do domínio.  

2. Inicie **Active Directory Users and Computers** e navegue até a UO em que você estará criando a conta de gerenciamento.  

3. Clique com botão direito na UO e clique em **New** e clique em **usuário**.  

4. No **novo objeto - usuário** diálogo caixa, digite suas informações de nomenclatura desejadas para a conta e clique em **próxima**.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. Forneça uma senha inicial para a conta de usuário, desmarque **usuário deve alterar a senha no próximo logon**, selecione **usuário não pode alterar a senha** e **conta está desabilitada**, e Clique em **próxima**.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. Verifique se os detalhes da conta estão corretos e clique em **concluir**.  

7. Clique no objeto do usuário que você acabou de criar e clique em **propriedades**.  

8. Clique o **conta** guia.  

9. No **opções de conta** campo, selecione o **conta é sigilosa e não pode ser delegada** sinalizador, selecione o **esta conta dá suporte à criptografia Kerberos AES de 128 bits** e/ou o **essa conta oferece suporte à criptografia Kerberos AES 256** sinalizador e, em seguida, clique em **Okey**.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > Como essa conta, como outras contas, terá uma função limitada, mas poderosa, a conta só deve ser usada nos hosts administrativos seguros. Para todos os hosts administrativos de seguros no seu ambiente, você deve considerar implementar a configuração de diretiva de grupo **segurança de rede: Configurar tipos de criptografia permitidos para Kerberos** para permitir que somente os tipos de criptografia mais seguros, você pode implementar para hosts seguros.  
   >
   > Embora a implementação de tipos de criptografia mais seguros para os hosts não atenua ataques de roubo de credenciais, o uso adequado e a configuração dos hosts seguros faz. Simplesmente definindo tipos de criptografia mais fortes para hosts que são usados somente por contas privilegiadas reduz a superfície de ataque geral dos computadores.  
   >
   > Para obter mais informações sobre como configurar tipos de criptografia em sistemas e contas, consulte [configurações do Windows para o suporte para o tipo de criptografia Kerberos](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
   >
   > Essas configurações são suportadas apenas em computadores que executam o Windows Server 2012, Windows Server 2008 R2, Windows 8 ou Windows 7.  
  
10. Sobre o **objeto** guia, selecione **proteger objeto contra exclusão acidental**. Isso não somente impedirá que o objeto seja excluído (até mesmo por usuários autorizados), mas impedirá que ele seja movido para outra OU em sua hierarquia do AD DS, a menos que a caixa de seleção está desmarcada pela primeira vez por um usuário com permissão para alterar o atributo.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. Clique o **controle remoto** guia.  

12. Desmarque a **habilitar o controle remoto** sinalizador. Nunca deve ser necessária para a equipe de suporte para se conectar a sessões dessa conta para implementar correções.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

   > [!NOTE]  
   > Cada objeto no Active Directory deve ter um proprietário IT designado e um proprietário de negócios designado, conforme descrito em [planejar para comprometimento](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Se você estiver rastreando a posse de objetos do AD DS no Active Directory (em vez de um banco de dados externo), você deve inserir informações de propriedade apropriados nas propriedades do objeto.  
   >
   > Nesse caso, o proprietário da empresa é provavelmente uma divisão de TI, andthere não é nenhum proibição de proprietários de negócios, além de ser proprietários IT. O ponto de estabelecer a propriedade de objetos é permitir que você identifique os contatos quando alterações precisam ser feitas a objetos, talvez anos desde sua criação inicial.  

13. Clique no **organização** guia.  

14. Insira todas as informações necessárias em seus padrões de objeto do AD DS.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. Clique no **Dial-in** guia.  

16. No **permissão de acesso de rede** campo, selecione **negar acesso**. Essa conta deve nunca precisar se conectar usando uma conexão remota.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

   > [!NOTE]  
   > É improvável que essa conta será usada para fazer logon controladores de domínio somente leitura (RODCs) em seu ambiente. No entanto, circunstância nunca deve exigir a conta para fazer logon em um RODC, você deve adicionar essa conta ao grupo de replicação de Senha RODC negado para que sua senha não é armazenado em cache no RODC.  
   >
   > Embora a senha da conta deve ser redefinida após cada uso e a conta deve ser desabilitada, implementar essa configuração não tem um efeito deletério na conta, e pode ser útil em situações em que um administrador esquecer redefinir a conta senha e desativá-lo.  

17. Clique na guia **Membro de**.  

18. Clique em **Adicionar**.  

19. Tipo de **negado grupo de replicação de Senha RODC** na **selecionar usuários, contatos, computadores** caixa de diálogo e clique em **verificar nomes**. Quando o nome do grupo é sublinhado no seletor de objetos, clique em **Okey** e verificar se a conta agora é um membro dos dois grupos exibidos na seguinte captura de tela. Não adicione a conta para todos os grupos protegidos.  

20. Clique em **OK**.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. Clique o **segurança** guia e clique em **avançado**.  

22. No **configurações de segurança avançadas** caixa de diálogo, clique em **desabilitar herança** e copiar as permissões herdadas, como as permissões explícitas e clique em **adicionar**.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. No **entrada de permissão para [conta]** caixa de diálogo, clique em **selecionar uma entidade** e adicione o grupo que você criou no procedimento anterior. Role até a parte inferior da caixa de diálogo e clique em **Limpar tudo** para remover todas as permissões padrão.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. Role até a parte superior dos **entrada de permissão** caixa de diálogo. Certifique-se de que o **tipo** lista suspensa é definida como **permitir**e, nas **aplica-se a** lista suspensa, selecione **apenas este objeto**.  

25. No **permissões** campo, selecione **ler todas as propriedades**, **permissões de leitura**, e **redefinição de senha**.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. No **propriedades** campo, selecione **ler userAccountControl** e **gravar userAccountControl**.  

27. Clique em **Okey**, **Okey** novamente na **configurações de segurança avançadas** caixa de diálogo.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

   > [!NOTE]  
   > O **userAccountControl** atributo controla várias opções de configuração de conta. Você não pode conceder permissão para alterar apenas algumas das opções de configuração quando você conceder permissão de gravação para o atributo.  

28. No **nomes de usuário ou grupo** campo da **segurança** guia, remova todos os grupos que não devem ter permissão para acessar ou gerenciar a conta. Não remova todos os grupos que foram configurados com Deny ACEs, como o grupo Todos e o SELF computados conta (essa ACE foi definida quando o **usuário não pode alterar a senha** sinalizador foi habilitado durante a criação da conta. Também não remova o grupo que você acabou de adicionar, a conta do sistema ou grupos como EA, acelerador de desenvolvimento, BA ou o grupo de acesso de autorização do Windows.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. Clique em **avançado** e verifique se que a caixa de diálogo Configurações avançadas de segurança seja semelhante à seguinte captura de tela.  

30. Clique em **Okey**, e **Okey** novamente para fechar a caixa de diálogo de propriedade da conta.  

   ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. A instalação da primeira conta de gerenciamento está concluída. Você testará a conta em um procedimento posterior.  

##### <a name="creating-additional-management-accounts"></a>Criação de contas de gerenciamento adicional

Você pode criar contas de gerenciamento adicionais, repetindo as etapas anteriores, copiando-se a conta que você acabou de criar ou criando um script para criar contas com as configurações desejadas. No entanto, observe que, se você copiar a conta que você acabou de criar, muitas das configurações personalizadas e as ACLs não serão copiadas para a nova conta e você precisará repetir a maioria das etapas de configuração.  
  
Em vez disso, você pode criar um grupo ao qual você delegar direitos para popular e unpopulate grupos protegidos, mas você deverá proteger o grupo e as contas que você coloque nele. Porque deve haver pouquíssimas contas em seu diretório que são concedidas a capacidade de gerenciar a associação de grupos protegidos, a criação de contas individuais pode ser a abordagem mais simples.  
  
Independentemente de como você optar por criar um grupo no qual você coloca as contas de gerenciamento, você deve garantir que cada conta é protegida conforme descrito anteriormente. Você também deve considerar implementar restrições de GPO semelhantes aos descritos em [apêndice d: Protegendo contas de administrador internas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
##### <a name="auditing-management-accounts"></a>Auditoria de gerenciamento de contas

Você deve configurar a auditoria na conta para fazer logon, no mínimo, todas as gravações para a conta. Isso permitirá que você não apenas para identifica bem-sucedidas habilitação da conta e redefinição de senha de sua durante usos autorizados, mas também identificar tentativas por usuários não autorizados para manipular a conta. Gravações com falha na conta devem ser capturadas em seu sistema de monitoramento de eventos (SIEM) e informações de segurança (se aplicável) e devem acionar alertas que fornecem notificação para a equipe responsável para investigar possíveis comprometimentos.  
  
Soluções SIEM recebem informações de evento de fontes de segurança envolvido (por exemplo, logs de eventos, dados de aplicativos, fluxos de rede, produtos antimalware e fontes de detecção de intrusão), os dados de agrupamento e tentam tornar exibições inteligentes e ações proativas . Há muitas soluções comerciais de SIEM e muitas empresas criar implementações particulares. Uma solução de SIEM bem projetada e implementada adequadamente pode melhorar significativamente o monitoramento de segurança e recursos de resposta a incidentes. No entanto, recursos e a precisão variam bastante entre soluções. SIEMs estão além do escopo deste artigo, mas as recomendações de evento específico contidas devem ser consideradas por qualquer implementador do SIEM.  
  
Para obter mais informações sobre as configurações de auditoria recomendadas para controladores de domínio, consulte [monitoramento do Active Directory para sinais de comprometimento](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Definições de configuração específicas de controlador de domínio são fornecidas nos [monitoramento do Active Directory para sinais de comprometimento](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>Habilitar contas de gerenciamento modificar a associação de grupos protegidos

Neste procedimento, você irá configurar permissões no objeto de AdminSDHolder do domínio para permitir que as contas de gerenciamento recém-criado modificar a associação de grupos protegidos no domínio. Esse procedimento não pode ser executado por meio de uma interface gráfica do usuário (GUI).  
  
Conforme discutido em [apêndice c: Grupos no Active Directory e contas protegidas](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), a ACL no AdminSDHolder de um domínio objeto é efetivamente "copiado" para objetos protegidos quando a tarefa SDProp é executado. Contas e grupos protegidos não herdam suas permissões de objeto AdminSDHolder; suas permissões são definidas explicitamente para corresponder no objeto AdminSDHolder. Portanto, quando você modifica as permissões no objeto AdminSDHolder, você deve modificá-los para atributos que são apropriados para o tipo do objeto protegido destinados.  
  
Nesse caso, você será concedendo as contas de gerenciamento recém-criado para permitir que eles leiam e os membros de atributo de gravação em objetos de grupo. No entanto, o objeto AdminSDHolder não é um objeto de grupo e atributos de grupo não são expostos no editor gráfico da ACL. É por esse motivo que você implementará as alterações de permissões por meio do utilitário de linha de comando Dsacls. Para conceder permissões de contas para modificar a associação de grupos protegidos de gerenciamento (desabilitado), execute as seguintes etapas:  
  
1.  Faça logon no controlador de domínio, preferencialmente o controlador de domínio que detém a função de emulador de PDC (PDCE), com as credenciais de uma conta de usuário que se tornou um membro do grupo no domínio.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2.  Abra um prompt de comando com privilégios elevados clicando **Prompt de comando** e clique em **executar como administrador**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3.  Quando solicitado a aprovar a elevação, clique em **Sim**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
    > [!NOTE]  
    > Para obter mais informações sobre a elevação e conta controle de usuário (UAC) no Windows, consulte [UAC processos e interações](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) no site do TechNet.  
  
4.  No Prompt de comando, digite (substituindo suas informações específicas de domínio) **Dsacls [nome diferenciado do objeto AdminSDHolder no seu domínio] /G [conta de gerenciamento UPN]: RPWP; membro**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
    O comando anterior (que não diferencia maiusculas de minúsculas) funciona da seguinte maneira:  
  
    -   DSACLS define ou exibe ACEs nos objetos de diretório  
  
    -   CN = AdminSDHolder, CN = System, DC = TailSpinToys, DC = msft identifica o objeto a ser modificado  
  
    -   /G indica que uma concessão de ACE está sendo configurada  
  
    -   PIM001@tailspintoys.msft é o nome Principal de usuário (UPN) da entidade de segurança para o qual as ACEs serão concedidas  
  
    -   Concessões RPWP propriedade permissões leitura e gravação propriedade  
  
    -   Membro é o nome da propriedade (atributo) no qual as permissões serão definidas  
  
    Para obter mais informações sobre o uso de **Dsacls**, digite Dsacls sem parâmetros em um prompt de comando.  
  
    Se você tiver criado várias contas de gerenciamento para o domínio, você deve executar o comando Dsacls para cada conta. Quando você tiver concluído a configuração de ACL no objeto AdminSDHolder, você deve forçar SDProp para executar ou aguarde até que sua execução agendada é concluída. Para obter informações sobre como forçar a SDProp a execução, consulte "Executando SDProp manualmente" em [apêndice c: Grupos no Active Directory e contas protegidas](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    Quando SDProp tiver sido executado, você pode verificar as alterações feitas no objeto AdminSDHolder foram aplicadas a grupos protegidos no domínio. Não é possível verificar isso exibindo a ACL no objeto AdminSDHolder pelos motivos descritos anteriormente, mas você pode verificar se as permissões foram aplicadas ao exibir as ACLs em grupos protegidos.  
  
5.  Na **Active Directory Users and Computers**, verifique se que você habilitou **recursos avançados**. Para fazer isso, clique em **modo de exibição**, localize o **Admins. do domínio** agrupar, clique no grupo e clique em **propriedades**.  
  
6.  Clique o **segurança** guia e clique em **avançado** para abrir o **Advanced Security Settings for Admins. do domínio** caixa de diálogo.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7.  Selecione **ACE de permissão para a conta de gerenciamento** e clique em **editar**. Verifique se que a conta tenha recebida só **ler membros** e **membros de gravação** permissões no grupo DA e clique **Okey**.  
  
8.  Clique em **Okey** na **configurações de segurança avançadas** caixa de diálogo e clique em **Okey** novamente para fechar a caixa de diálogo de propriedade para o grupo de acelerador de desenvolvimento.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. Você pode repetir as etapas anteriores para outros grupos protegidos no domínio; as permissões devem ser as mesmas para todos os grupos protegidos. Agora você concluiu criação e configuração das contas de gerenciamento para os grupos protegidos nesse domínio.  
  
    > [!NOTE]  
    > Qualquer conta que tenha permissão para gravar a associação de um grupo no Active Directory também pode adicionar em si ao grupo. Esse comportamento ocorre por design e não pode ser desabilitado. Por esse motivo, você deve sempre manter desabilitadas quando não estiver em uso de contas de gerenciamento e deve monitorar de perto as contas quando eles permanecerão desabilitados e quando eles estão sendo usados.  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>Verificando o grupo e as definições de configuração de conta

Agora que você tiver criado e configurado a contas de gerenciamento que podem modificar a associação de grupos protegidos no domínio (que inclui os grupos EA, acelerador de desenvolvimento e BA mais altamente privilegiados), você deve verificar se as contas e seu grupo de gerenciamento foram criado corretamente. Verificação consiste em uma dessas tarefas gerais:  
  
1.  O grupo que pode habilitar e desabilitar contas de gerenciamento para verificar se os membros do grupo podem habilitam e desabilitar as contas e redefiniram suas senhas, mas não é possível executar outras atividades administrativas em contas de gerenciamento de teste.  
  
2.  As contas de gerenciamento para verificar que eles podem adicionar e remover membros para grupos no domínio protegidos, mas não é possível alterar qualquer outra propriedade de contas protegidas e grupos de teste.  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>O grupo que será habilitar e desabilitar contas de gerenciamento de teste
  
1.  Para testar a habilitar uma conta de gerenciamento e redefinir sua senha, faça logon para uma estação de trabalho administrativa segura com uma conta que seja membro do grupo criado na [i Apêndice: Criação de contas de gerenciamento para protegidos em grupos e contas no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Abra **Active Directory Users and Computers**, a conta de gerenciamento com o botão direito e clique em **habilitar conta**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Deve exibir uma caixa de diálogo, confirmando que a conta tiver sido habilitada.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  Em seguida, redefina a senha da conta de gerenciamento. Para fazer isso, clique com botão direito na conta novamente e clique em **Redefinir senha**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Digite uma nova senha para a conta de **nova senha** e **Confirmar senha** campos e clique em **Okey**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Deve aparecer uma caixa de diálogo, confirmando que a senha para a conta foi redefinida.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Agora tente modificar as propriedades adicionais da conta de gerenciamento. A conta com o botão direito e clique em **propriedades**e clique no **controle remoto** guia.  
  
8.  Selecione **habilitar o controle remoto** e clique em **aplicar**. A operação falhará e um **acesso negado** deve exibir a mensagem de erro.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Clique o **conta** guia para a conta e tente alterar o nome da conta, horário de logon ou estações de trabalho de logon. Tudo deve falhar e opções que não são controladas da conta do **userAccountControl** atributo deve ser desabilitado e indisponível para modificação.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Tentativa de adicionar o grupo de gerenciamento para um grupo protegido, como o grupo de acelerador de desenvolvimento. Quando você clica em **Okey**, deve aparecer uma mensagem informando que você não tem permissões para modificar o grupo.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Executar testes adicionais conforme necessário para verificar que você não pode configurar qualquer coisa na conta de gerenciamento, exceto **userAccountControl** configurações e as redefinições de senha.  
  
    > [!NOTE]  
    > O **userAccountControl** atributo controla várias opções de configuração de conta. Você não pode conceder permissão para alterar apenas algumas das opções de configuração quando você conceder permissão de gravação para o atributo.  
  
##### <a name="test-the-management-accounts"></a>As contas de gerenciamento de teste

Agora que você habilitou a uma ou mais contas que podem alterar a associação de grupos protegidos, você pode testar as contas para garantir que eles podem modificar a associação de grupo protegido, mas não é possível executar outras modificações em grupos e contas protegidas.  
  
1.  Faça logon em um host administrativo seguro como a primeira conta de gerenciamento.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Inicie **Active Directory Users and Computers** e localize o **grupo Admins. do domínio**.  
  
3.  Clique com botão direito do **Admins. do domínio** agrupar e clique em **propriedades**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  No **propriedades de Admins**, clique no **membros** guia e **clique** adicionar. Insira o nome de uma conta que receberá privilégios de Admins. do domínio temporários e clique em **verificar nomes**. Quando o nome da conta está sublinhado, clique em **Okey** para retornar para o **membros** guia.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  Sobre o **membros** guia para o **propriedades de Admins. do domínio** caixa de diálogo, clique em **aplicar**. Depois de clicar em **aplicar**, a conta deve permanecer um membro do grupo de acelerador de desenvolvimento e você não deve receber nenhuma mensagem de erro.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Clique o **gerenciado por** guia o **propriedades de Admins. do domínio** caixa de diálogo caixa e verifique se você não pode inserir texto em todos os campos e todos os botões ficarão esmaecidos.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  Clique o **gerais** guia o **propriedades de Admins. do domínio** caixa de diálogo caixa e verifique se que você não pode modificar qualquer uma das informações sobre essa guia.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Repita essas etapas para grupos protegidos adicionais conforme necessário. Quando você terminar, faça logon em um host protegido administrativo com uma conta que seja membro do grupo que você criou para habilitar e desabilitar as contas de gerenciamento. Em seguida, redefina a senha da conta de gerenciamento apenas testado e desabilitar a conta. Você concluiu a configuração das contas de gerenciamento e o grupo que será responsável por habilitar e desabilitar as contas.  
