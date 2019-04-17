---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: "Apêndice I - criando contas de gerenciamento de contas protegidas e grupos no Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 666180adca691d6c9783a43063df76877115fc40
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Apêndice i: criação de gerenciamento de contas para protegido contas e grupos no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Apêndice i: criação de gerenciamento de contas para protegido contas e grupos no Active Directory  

Um dos desafios na implementação de um modelo do Active Directory que não dependem de associação permanente em grupos altamente privilegiados é que deve haver um mecanismo para preencher esses grupos quando a associação temporária nos grupos é necessária. Algumas soluções de gerenciamento de identidade privilegiados exigem que contas de serviço do software são concedidas permanente associação a grupos como DA ou administradores em cada domínio na floresta. No entanto, tecnicamente não é necessário para soluções de gerenciamento de identidade privilegiado (PIM) executar seus serviços em tais contextos altamente privilegiados.  
  
Este apêndice fornece informações que você pode usar soluções de PIM nativamente implementado ou de terceiros para criar contas que têm privilégios limitados e podem ser controladas severamente, mas podem ser usadas para popular grupos privilegiados no Active Directory quando temporária elevação é necessária. Se você estiver implementando PIM como uma solução nativa, essas contas podem ser usadas pela equipe administrativa para executar a população de grupo temporários e, se você estiver implementando PIM através do software de terceiros, você pode ser capaz de se adaptar a essas contas para funcionar como contas de serviço.  
  
> [!NOTE]  
> Os procedimentos descritos neste apêndice fornecem uma abordagem para o gerenciamento de grupos altamente privilegiados no Active Directory. Você pode adaptar esses procedimentos para atender às suas necessidades, adicionar restrições adicionais ou omitir dentre as restrições que estão descritas aqui.  
  
### <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Criando o gerenciamento de contas para protegido contas e grupos no Active Directory  
Criar contas que pode ser usado para gerenciar a associação dos grupos privilegiados sem exigir que as contas de gerenciamento para ser concedido excessivos direitos e permissões consiste em quatro atividades gerais que são descritas nas instruções passo a passo que seguem:  
  
1.  Primeiro, você deve criar um grupo que gerenciará as contas, porque essas contas devem ser gerenciadas por um conjunto limitado de usuários confiáveis. Se você não tiver uma estrutura de UO adequa segregam contas privilegiadas e protegidas e sistemas da população geral no domínio, você deve criar uma. Embora instruções específicas não forem fornecidas neste apêndice, capturas de tela mostram um exemplo de tal uma hierarquia de UO.  
  
2.  Crie as contas de gerenciamento. Essas contas devem ser criadas como contas de usuário "normal" e concedidas sem direitos de usuário além daqueles que já são concedidos aos usuários por padrão.  
  
3.  Implementar restrições sobre as contas de gerenciamento que torná-los utilizáveis apenas para a finalidade especializada para o qual eles foram criados, além de controlar quem pode habilitar e usar as contas (o grupo que você criou na primeira etapa).  
  
4.  Configure permissões no objeto AdminSDHolder em cada domínio para permitir que as contas de gerenciamento alterar a associação dos grupos privilegiados no domínio.  
  
Completamente, você deve testar todos esses procedimentos e modificá-las conforme necessário para seu ambiente antes de implementá-los em um ambiente de produção. Você também deve verificar se todas as configurações funcionam conforme o esperado (alguns procedimentos de teste são fornecidos neste apêndice), e você deve testar um cenário de recuperação de desastre nos quais as contas de gerenciamento não estão disponíveis para serem usados para preencher grupos protegidos para fins de recuperação. Para obter mais informações sobre como fazer backup e restaurar o Active Directory, consulte o [Backup do AD DS e o guia passo a passo de recuperação](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx).  
  
> [!NOTE]  
> Implementando as etapas descritas neste apêndice, você irá criar contas que serão capazes de gerenciar a participação de todos os grupos protegidos em cada domínio, não apenas privilégio mais alto grupos do Active Directory como EAs, DAs e BAs. Para saber mais sobre grupos protegidos no Active Directory, consulte [apêndice c: protegido contas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
#### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Instruções passo a passo para a criação de contas de gerenciamento de grupos protegidos  
  
##### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Criar um grupo para habilitar e desabilitar contas de gerenciamento  
Contas de gerenciamento devem ter suas senhas redefinir em cada uso e devem ser desabilitadas quando atividades que eles precisem são concluídas. Embora você também pode considerar a implementação de requisitos de logon de cartão inteligente para essas contas, é uma configuração opcional e essas instruções presumem que as contas de gerenciamento serão configuradas com um nome de usuário e senha longa e complexa como controles mínimos. Nesta etapa, você criará um grupo que tenha permissões para redefinir senha nas contas de gerenciamento para habilitar e desabilitar contas.  
  
Para criar um grupo para habilitar e desabilitar contas de gerenciamento, execute as seguintes etapas:  
  
1.  Na estrutura de UO onde você vai acomodar as contas de gerenciamento, clique com botão direito na UO onde você deseja criar o grupo, clique em **nova** e clique em **grupo**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  No **novo objeto - grupo** caixa de diálogo, digite um nome para o grupo. Se você pretende usar esse grupo "Ativar" todas as contas de gerenciamento na floresta, torne um grupo de segurança universal. Se você tiver uma floresta de domínio único ou se você pretende criar um grupo em cada domínio, você pode criar um grupo de segurança global. Clique em **Okey** para criar o grupo.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  Clique com botão direito do grupo que você acabou de criar, clique em **propriedades**e clique no **objeto** guia. Do grupo **propriedade do objeto** caixa de diálogo, selecione **objeto proteger contra exclusão acidental**, que não só impedirá que os usuários autorizados excluam do grupo, mas também de movê-lo para outra unidade Organizacional, a menos que o atributo primeiro fica desmarcado.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  

    > Se você já tiver configurado as permissões em pai do grupo UOs restringir administração para um conjunto limitado de usuários, não convém executar as etapas a seguir. Eles são fornecidos aqui para que mesmo se você ainda não tiver implementado limitado controle administrativo sobre a estrutura de UO em que você criou esse grupo, você pode proteger o grupo contra modificação por usuários não autorizados.  
  
4.  Clique no **membros** guia e adicionar as contas para membros da equipe quem serão responsáveis por habilitando contas de gerenciamento ou preenchendo protegido grupos quando necessário.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Se você ainda não tiver feito isso, além do **usuários e Active Directory computadores** do console, clique em **exibição** e selecione **recursos avançados**. Clique com botão direito do grupo que você acabou de criar, clique em **propriedades**e clique no **segurança** guia. Sobre o **segurança**, clique em **avançado**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  No **configurações de segurança avançadas para [grupo]** caixa de diálogo, clique em **desabilitar a herança**. Quando solicitado, clique em **Convert herdado permissões em permissões explícitas nesse objeto**e clique em **Okey** para retornar para o grupo **segurança** caixa de diálogo.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  Sobre o **segurança** guia, remover grupos que não devem ter permissão para acessar esse grupo. Por exemplo, se não quiser que os usuários autenticados a ser capaz de ler o nome do grupo e as propriedades gerais, você pode remover essa ACE. Você também pode remover ACEs, como aqueles para conta operadores e versões anteriores ao Windows 2000 Server acesso compatível. No entanto, você deve, deixar um conjunto mínimo de permissões de objeto no lugar. Deixe as ACEs seguir intacto:  
  
    -   SELF  
  
    -   SISTEMA  
  
    -   Administradores de domínio  
  
    -   Administradores corporativos  
  
    -   Administradores  
  
    -   Grupo de acesso de autorização Windows (se aplicável)  
  
    -   CONTROLADORES DE DOMÍNIO DA EMPRESA  
  
    Embora possa parecer contra-intuitivo para permitir que os grupos privilegiados mais altos no Active Directory para gerenciar esse grupo, seu objetivo na implementação dessas configurações não é impedir que membros desses grupos façam alterações autorizadas. Em vez disso, o objetivo é garantir que, quando você tiver ocasião para exigir muito altos níveis de privilégio, alterações autorizadas serão bem-sucedidas. É por esse motivo que alterando padrão privilegiados aninhamento de grupos, direitos, e as permissões são desencorajadas ao longo deste documento. Deixando estruturas padrão intacto e esvaziar a associação dos grupos de privilégio mais altos na pasta, você pode criar um ambiente mais seguro que ainda funciona conforme o esperado.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Se já não tiver configurado as políticas de auditoria para os objetos na estrutura de UO onde você criou esse grupo, você deve configurar a auditoria para registrar as alterações desse grupo.  
  
8.  Você concluiu a configuração do grupo que será usado para "check-out" gerenciamento de contas quando elas são necessárias e "check-in" as contas quando suas atividades foram concluídas.  
  
##### <a name="creating-the-management-accounts"></a>Criar as contas de gerenciamento  
Você deve criar pelo menos uma conta que será usada para gerenciar a associação dos grupos privilegiados na sua instalação do Active Directory e preferencialmente uma segunda conta para servir como um backup. Se você optar por criar as contas de gerenciamento em um único domínio na floresta e conceder recursos de gerenciamento para todos os domínios protegido grupos, ou se você optar por implementar contas de gerenciamento em cada domínio na floresta, os procedimentos são efetivamente o mesmo.  
  
> [!NOTE]  
> As etapas neste documento pressupõem que você ainda não implementaram os controles de acesso baseado em função e o gerenciamento de identidade privilegiado para o Active Directory. Portanto, alguns dos procedimentos devem ser realizadas por um usuário cuja conta é um membro do grupo Admins. do domínio para o domínio em questão.  
>   
> Quando você estiver usando uma conta com privilégios DA, você pode fazer logon um controlador de domínio para executar as atividades de configuração. Etapas que não exigem privilégios DA podem ser realizadas por contas com menos privilégios que fazem logon em estações de trabalho administrativas. Capturas de tela que mostram as caixas de diálogo na cor azul clara representam as atividades que podem ser executadas em um controlador de domínio. Capturas de tela que mostram as caixas de diálogo na cor azul mais escuro representam as atividades que podem ser executadas em estações de trabalho administrativas com contas que têm privilégios limitados.  
  
Para criar as contas de gerenciamento, execute as seguintes etapas:  
  
1.  Faça logon um controlador de domínio com uma conta que é um membro do grupo do domínio.  
  
2.  Iniciar **usuários e Active Directory computadores** e navegue até a UO onde você criará a conta de gerenciamento.  
  
3.  Clique com botão direito na UO e clique em **nova** e clique em **usuário**.  
  
4.  No **New Object - User** caixa de diálogo caixa, insira suas informações de nomenclatura desejadas para a conta e clique em **próxima**.  
  
5.  Faça logon um controlador de domínio com uma conta que é um membro do grupo do domínio.  
  
6.  Iniciar **usuários e Active Directory computadores** e navegue até a UO onde você criará a conta de gerenciamento.  
  
7.  Clique com botão direito na UO e clique em **nova** e clique em **usuário**.  
  
8.  No **New Object - User** caixa de diálogo caixa, insira suas informações de nomenclatura desejadas para a conta e clique em **próxima**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
9. Fornecer uma senha inicial da conta de usuário, desmarque **usuário deve alterar a senha no próximo logon**, selecione **usuário não pode alterar a senha** e **conta está desabilitada**e clique em **próxima**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  
  
10. Verifique se os detalhes da conta estão corretos e clique em **concluir**.  
  
11. Clique com botão direito do objeto de usuário que você acabou de criar e clique em **propriedades**.  
  
12. Clique no **conta** guia.  
  
13. No **opções de conta** campo, selecione o **conta é confidencial e não pode ser delegada** sinalizador, selecione o **essa conta dá suporte à criptografia de Kerberos AES 128 bits** e/ou o **essa conta dá suporte à criptografia Kerberos AES 256** sinalizar e clique em **Okey**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  
  
    > [!NOTE]  
    > Porque essa conta, como outras contas, terá uma função limitada, mas poderosa, a conta deve ser usada somente em hosts administrativos seguros. Para todos os hosts administrativos de seguros no seu ambiente, você deve considerar Implementando a configuração de política de grupo **segurança de rede: tipos de configurar criptografia permitidos para Kerberos** para permitir que apenas os tipos de criptografia mais seguros, você pode implementar para hosts seguros.  
    >   
    > Embora a implementação de tipos de criptografia mais seguros para os hosts não atenua ataques de roubo de credenciais, o uso apropriado e configuração dos hosts seguras faz. Basta definir tipos de criptografia mais fortes para hosts que são usados somente pelo contas privilegiadas reduz a superfície de ataque geral dos computadores.  
    >   
    > Para obter mais informações sobre como configurar tipos de criptografia em sistemas e contas, consulte [configurações do Windows para o tipo de criptografia de suporte Kerberos](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
    >   
    > **Observação** essas configurações têm suporte somente em computadores que executam o Windows Server 2012, Windows Server 2008 R2, Windows 8 ou Windows 7.  
  
14. Sobre o **objeto** , selecione **objeto proteger contra exclusão acidental**. Isso não apenas impedirá que o objeto está sendo excluído (até mesmo por usuários autorizados), mas impedirá que ele está sendo movido para uma UO diferente na sua hierarquia do AD DS, a menos que a caixa de seleção estiver desmarcada primeiro por um usuário com permissão para alterar o atributo.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  
  
15. Clique no **controle remoto** guia.  
  
16. Limpar o **habilitar o controle remoto** sinalizador. Ele não será necessário para a equipe de suporte para se conectar a sessões dessa conta implementar correções.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  
  
    > [!NOTE]  
    > Cada objeto no Active Directory deve ter um proprietário IT designado e um proprietário de negócios designado, conforme descrito em [planejamento de comprometimento](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Se você está controlando a propriedade de objetos do AD DS no Active Directory (em oposição a um banco de dados externo), você deve inserir informações de propriedade apropriadas nas propriedades do objeto.  
    >   
    > Nesse caso, o proprietário da empresa é provavelmente uma divisão de TI, andthere não é nenhuma proibição de empresários também está sendo proprietários IT. O ponto do estabelecimento de propriedade de objetos é permitir que você identifique contatos quando alterações precisam ser feitas para os objetos, talvez anos desde sua criação inicial.  
  
17. Clique no **organização** guia.  
  
18. Digite qualquer informação que é necessária em seus padrões de objeto do AD DS.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  
  
19. Clique no **discagem** guia.  
  
20. No **permissão de acesso de rede** campo, selecione **negar acesso**. Essa conta nunca deve precisar se conectar por uma conexão remota.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  
  
    > [!NOTE]  
    > É improvável que essa conta será usada para fazer logon controladores de domínio somente leitura (RODCs) em seu ambiente. No entanto, circunstância nunca deve exigir a conta para fazer logon em um RODC, você deve adicionar essa conta para o grupo de replicação de Senha RODC negado para que sua senha não é armazenado em cache no RODC.  
    >   
    > Embora a senha da conta deve ser redefinida após cada uso e a conta deve ser desabilitada, implementar essa configuração não tem um efeito deleterious na conta, e talvez seja útil em situações em que um administrador se esquecer de redefinir a senha da conta e desativá-lo.  
  
21. Clique no **membro de** guia.  
  
22. Clique em **adicionar**.  
  
23. Tipo **negado grupo de replicação de Senha RODC** no **selecionar usuários, contatos, computadores** caixa de diálogo e clique em **verificar nomes**. Quando o nome do grupo é sublinhado no seletor de objeto, clique em **Okey** e verifique se a conta agora é um membro dos dois grupos exibido na seguinte captura de tela. Não adicione a conta a qualquer grupos protegidos.  
  
24. Clique em **Okey**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  
  
25. Clique no **segurança** guia e clique em **avançado**.  
  
26. No **configurações de segurança avançadas** caixa de diálogo, clique em **desabilitar a herança** e copie as permissões herdadas como permissões explícitas e clique em **adicionar**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  
  
27. No **entrada de permissão para [conta]** caixa de diálogo, clique em **selecionar uma entidade de segurança** e adicione o grupo que você criou no procedimento anterior. Role até a parte inferior da caixa de diálogo e clique em **Limpar tudo** para remover todas as permissões padrão.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  
  
28. Role a tela para a parte superior do **entrada de permissão** caixa de diálogo. Certifique-se de que o **tipo** lista suspensa é definida como **permitir**e no **se aplica a** lista suspensa, selecione **somente esse objeto**.  
  
29. No **permissões** campo, selecione **ler todas as propriedades**, **permissões de leitura**, e **Redefinir senha**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  
  
30. No **propriedades** campo, selecione **ler userAccountControl** e **escrever userAccountControl**.  
  
31. Clique em **Okey**, **Okey** novamente no **configurações de segurança avançadas** caixa de diálogo.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  
  
    > [!NOTE]  
    > O **userAccountControl** atributo controla várias opções de configuração de conta. Você não pode conceder permissão para alterar somente de algumas das opções de configuração quando você concede permissão de gravação para o atributo.  
  
32. No **nomes de usuário ou grupo** campo a **segurança** guia, remova todos os grupos que não devem ter permissão para acessar ou gerenciar a conta. Não remova todos os grupos que foram configurados com ACEs negar, como o grupo Todos e o SELF calculado conta (essa ACE foi definida quando o **usuário não pode alterar a senha** sinalizador foi habilitado durante a criação da conta. Também não remova o grupo que você acabou de adicionar, a conta do sistema ou grupos como EA, DA, BA ou o grupo de acesso de autorização do Windows.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  
  
33. Clique em **avançado** e verifique se que a caixa de diálogo Configurações de segurança avançadas seja semelhante à seguinte captura de tela.  
  
34. Clique em **Okey**, e **Okey** novamente para fechar a caixa de diálogo de propriedades da conta.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  
  
35. Instalação da primeira conta gerenciamento agora está concluída. Você irá testar a conta em um procedimento posterior.  
  
###### <a name="creating-additional-management-accounts"></a>Criação de contas de gerenciamento adicionais  
Você pode criar contas de gerenciamento adicionais repetindo as etapas anteriores, copiando a conta que você acabou de criar ou através da criação de um script para criar contas com as configurações desejadas. Observe, porém, se você copiar a conta que você acabou de criar, muitas das configurações personalizadas e ACLs não serão copiadas para a nova conta e você terá que repetir a maioria das etapas de configuração.  
  
Em vez disso, você pode criar um grupo ao qual você delegar direitos para popular e unpopulate grupos protegidos, mas você precisará proteger o grupo e as contas que você coloque nele. Como deve haver muito poucos contas em seu diretório que são concedidas a capacidade de gerenciar a associação dos grupos protegidos, criar contas individuais pode ser a abordagem mais simples.  
  
Independentemente de como você optar por criar um grupo nos quais você colocar as contas de gerenciamento, você deve garantir que cada conta é protegida, conforme descrito anteriormente. Você também deve considerar a implementação de restrições de GPO semelhantes àquelas descritas [apêndice d: proteção de administrador interno contas no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
###### <a name="auditing-management-accounts"></a>Auditoria de gerenciamento de contas  
Você deve configurar a auditoria da conta para fazer logon, no mínimo, todas as gravações para a conta. Isso permitirá que você não apenas identifica bem-sucedida habilitação da conta e redefinir sua senha durante os usos autorizados, mas também identificar tentativas por usuários não autorizados para manipular a conta. Gravações com falha na conta devem ser capturadas em seu sistema de informações de segurança e monitoramento de evento (SIEM) (se aplicável) e devem disparar alertas que fornecem notificação para a equipe responsável por investigando comprometimentos potenciais.  
  
Soluções SIEM tirar informações do evento de fontes de segurança envolvidos (por exemplo, logs de eventos, dados de aplicativo, fluxos de rede, produtos antimalware e fontes de detecção de invasões), agrupar os dados e tentam fazer inteligentes modos de exibição e ações proativas. Existem muitas soluções SIEM comerciais, e muitas empresas criar implementações particulares. Um SIEM bem projetado e implementado adequadamente pode aumentar consideravelmente segurança capacidades de monitoramento e resposta a incidentes. No entanto, recursos e precisão variar extremamente entre soluções. SIEMs estão além do escopo deste documento, mas as recomendações de eventos específicos contidas devem ser consideradas por qualquer implementador SIEM.  
  
Para obter mais informações sobre configurações de auditoria recomendadas para controladores de domínio, consulte [do Active Directory monitoramento de sinais de comprometimento](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Configurações de configuração específicos de controlador de domínio são fornecidas em [do Active Directory monitoramento de sinais de comprometimento](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
##### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>Habilitando o gerenciamento de contas modificar os membros dos grupos protegidos  
Neste procedimento, você irá configurar permissões no objeto de AdminSDHolder do domínio para permitir que as contas de gerenciamento recém-criado modificar os membros dos grupos protegidos no domínio. Este procedimento não pode ser realizado por meio de uma interface gráfica do usuário (GUI).  
  
Conforme discutido em [apêndice c: protegido contas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), a ACL em AdminSDHolder um domínio objeto é efetivamente "copiado" para objetos protegidos quando a tarefa SDProp é executado. Contas e grupos protegidos não herdam suas permissões do objeto AdminSDHolder; as permissões são definidas explicitamente para corresponder no objeto AdminSDHolder. Portanto, quando você modifica as permissões no objeto AdminSDHolder, você deve modificá-los para atributos que são apropriados para o tipo do objeto protegido, que você está direcionando.  
  
Nesse caso, você estarão concedendo as contas de gerenciamento recém-criado para permitir que o para leitura e gravação os membros de atributo em objetos de grupo. No entanto, o objeto AdminSDHolder não é um objeto de grupo e atributos de grupo não são expostos no editor de ACL gráfico. É por este motivo que você implementará as alterações de permissões por meio do utilitário de linha de comando Dsacls. Para conceder permissões de contas para modificar os membros dos grupos protegidos de gerenciamento (desativado), execute as seguintes etapas:  
  
1.  Faça logon um controlador de domínio, preferencialmente o controlador de domínio que contém a função de emulador do PDC (PDCE), com as credenciais de uma conta de usuário que se tornou um membro do grupo no domínio.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2.  Abra um prompt de comando elevado clicando **Prompt de comando** e clique em **executar como administrador**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3.  Quando solicitado para aprovar a elevação, clique em **Sim**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
    > [!NOTE]  
    > Para saber mais sobre elevação e conta controle de usuário (UAC) no Windows, consulte [UAC processos e interações](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) no site do TechNet.  
  
4.  No Prompt de comando, digite (substituindo suas informações específicas do domínio) **Dsacls [nome diferenciado do objeto AdminSDHolder em seu domínio] /G [conta de gerenciamento UPN]: RPWP; membro**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
    O comando anterior (que não diferencia maiusculas de minúsculas) funciona da seguinte maneira:  
  
    -   DSACLS define ou exibe ACEs em objetos de diretório  
  
    -   CN = AdminSDHolder, CN = System, DC = TailSpinToys, DC = msft identifica o objeto a ser modificado  
  
    -   /G indica que está sendo configurada uma concessão ACE  
  
    -   PIM001@tailspintoys.msfté o nome Principal de usuário (UPN) do objeto de segurança para o qual as ACEs serão concedidas  
  
    -   Concessão de RPWP propriedade permissões leitura e gravação propriedade  
  
    -   Membro é o nome da propriedade (atributo) em que as permissões serão definidas  
  
    Para obter mais informações sobre o uso de **Dsacls**, digite Dsacls sem qualquer parâmetro em um prompt de comando.  
  
    Se você tiver criado várias contas de gerenciamento de domínio, você deve executar o comando Dsacls para cada conta. Quando você tiver concluído a configuração de ACL no objeto AdminSDHolder, você deve forçar SDProp executar ou aguarde até que sua execução agendada seja concluída. Para obter informações sobre como forçar SDProp para executar, consulte "Executando manualmente SDProp" em [apêndice c: protegido contas e grupos no Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    Quando SDProp foi executado, você pode verificar se as alterações feitas ao objeto AdminSDHolder foram aplicadas a grupos protegidos no domínio. Não é possível verificar isso exibindo a ACL no objeto AdminSDHolder pelos motivos descritos anteriormente, mas você pode verificar se as permissões foram aplicadas, exibindo as ACLs grupos protegidos.  
  
5.  Em **usuários e Active Directory computadores**, verifique se que você tenha habilitado **recursos avançados**. Para fazer isso, clique em **exibição**, localize o **Admins. do domínio** de grupo, clique com botão direito do grupo e clique em **propriedades**.  
  
6.  Clique no **segurança** guia e clique em **avançado** para abrir o **configurações de segurança avançadas para administradores do domínio** caixa de diálogo.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7.  Selecione **ACE de permissão da conta de gerenciamento de** e clique em **editar**. Verifique se que a conta tenha sido concedida apenas **membros de leitura** e **membros escrever** permissões no grupo DA e clique em **Okey**.  
  
8.  Clique em **Okey** no **configurações de segurança avançadas** caixa de diálogo e clique em **Okey** novamente para fechar a caixa de diálogo de propriedade para o grupo DA.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. Você pode repetir as etapas anteriores para outros grupos protegidos no domínio; as permissões devem ser as mesmas para todos os grupos protegidos. Você concluiu criação e a configuração das contas de gerenciamento para os grupos protegidos neste domínio.  
  
    > [!NOTE]  
    > Qualquer conta que tenha permissão para gravar membros de um grupo no Active Directory também pode adicionar próprio ao grupo. Esse comportamento é o esperado e não pode ser desabilitado. Por esse motivo, você deve sempre manter contas de gerenciamento desabilitadas quando não estiver em uso e estreitamente deve monitorar as contas quando ele estão desabilitados e quando eles estiverem em uso.  
  
##### <a name="verifying-group-and-account-configuration-settings"></a>Verificando o grupo e as configurações de conta  
Agora que você criou e configurou contas de gerenciamento que podem modificar a associação dos grupos protegidos no domínio (que inclui os grupos EA, DA e BA mais altamente privilegiados), você deve verificar se as contas e seu grupo de gerenciamento foram criadas corretamente. Verificação consiste nessas tarefas gerais:  
  
1.  Teste o grupo que pode habilitar e desabilitar contas de gerenciamento para verificar que membros do grupo podem habilitam e desabilitar contas e redefinam as senhas, mas não é possível realizar outras atividades administrativas nas contas de gerenciamento.  
  
2.  Teste as contas de gerenciamento para verificar se eles podem adicionar e remover membros protegidos grupos no domínio, mas não podem alterar as outras propriedades de protegido contas e grupos.  
  
###### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>Testar o grupo que serão habilitar e desabilitar contas de gerenciamento  
  
1.  Para testar habilitando uma conta de gerenciamento e redefinir sua senha, fazer logon uma estação de trabalho administrativa segura com uma conta que é um membro do grupo criado na [apêndice i: criação de gerenciamento de contas para protegido contas e grupos no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Abrir **usuários e Active Directory computadores**, clique com botão direito a conta de gerenciamento e clique em **Ativar conta**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Uma caixa de diálogo deve exibir, confirmando que a conta tiver sido habilitada.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  Em seguida, redefina a senha da conta de gerenciamento. Para fazer isso, clique com botão direito a conta novamente e clique em **Redefinir senha**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Digite uma nova senha para a conta no **nova senha** e **Confirmar senha** campos e clique em **Okey**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Uma caixa de diálogo deve aparecer, confirmando que a senha da conta foi redefinida.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Agora tente modificar propriedades adicionais da conta de gerenciamento. Clique com botão direito na conta e clique em **propriedades**e clique no **controle remoto** guia.  
  
8.  Selecione **habilitar o controle remoto** e clique em **aplicar**. Deverá haver falha na operação e um **acesso negado** deve exibir a mensagem de erro.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Clique no **conta** guia para a conta e tente alterar o nome da conta, horário de logon ou estações de trabalho de logon. Tudo deve falhar e opções que não são controladas da conta a **userAccountControl** atributo deve ser cinza-out e não está disponível para modificação.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Tente adicionar o grupo de gerenciamento para um grupo protegido, como o grupo DA. Quando você clica em **Okey**, deve aparecer uma mensagem informando que você não tem permissões para modificar o grupo.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Executar testes adicionais conforme necessário para confirmar que você não pode configurar nada na conta do gerenciamento exceto **userAccountControl** configurações e redefinições de senha.  
  
    > [!NOTE]  
    > O **userAccountControl** atributo controla várias opções de configuração de conta. Você não pode conceder permissão para alterar somente de algumas das opções de configuração quando você concede permissão de gravação para o atributo.  
  
###### <a name="test-the-management-accounts"></a>Testar as contas de gerenciamento  
Agora que você ativou uma ou mais contas que podem alterar a associação dos grupos protegidos, você pode testar as contas para garantir que eles podem modificar a associação ao grupo protegido, mas não é possível executar outras modificações no protegido contas e grupos.  
  
1.  Faça logon a um host administrativo seguro como a primeira conta de gerenciamento.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Iniciar **usuários e Active Directory computadores** e localize o **grupo Admins. do domínio**.  
  
3.  Clique com botão direito do **Admins. do domínio** de grupo e clique em **propriedades**.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  No **propriedades de Admins**, clique no **membros** guia e **clique** adicionar. Insira o nome de uma conta que receberão temporários privilégios Admins. do domínio e clique em **verificar nomes**. Quando o nome da conta é sublinhado, clique em **Okey** para retornar para o **membros** guia.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  Sobre o **membros** guia para o **propriedades de administradores de domínio** caixa de diálogo, clique em **aplicar**. Depois de clicar em **aplicar**, a conta deve permanecer um membro do grupo DA e você não deve receber nenhuma mensagem de erro.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Clique no **gerenciado por** guia a **propriedades de Admins** caixa de diálogo caixa e verifique se você não pode inserir texto em qualquer campo e todos os botões são esmaecidos.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  Clique no **geral** guia a **propriedades de Admins** caixa de diálogo caixa e verifique se que você não pode modificar as informações sobre essa guia.  
  
    ![criação de contas de gerenciamento](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Repita essas etapas para grupos de protegido adicionais conforme necessário. Quando terminar, faça logon em um host administrativo seguro com uma conta que é um membro do grupo que você criou para habilitar e desabilitar as contas de gerenciamento. Redefina a senha da conta de gerenciamento você testado e desabilitar a conta. Você concluiu a instalação das contas de gerenciamento e o grupo que será responsável por habilitando e desabilitando as contas.  
  


