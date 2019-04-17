---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: "Apêndice C - protegido contas e grupos no Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c1d29b61844428115f8b2d1f5d935f225a249659
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Apêndice c: protegidas contas e grupos no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Apêndice c: protegidas contas e grupos no Active Directory  
No Active Directory, um conjunto de altamente privilegiadas contas e grupos padrão são considerados protegidas contas e grupos. Com a maioria dos objetos no Active Directory, os administradores delegados (usuários que já tenham sido recebido permissões para gerenciar objetos do Active Directory) podem alterar as permissões em objetos, incluindo a alteração de permissões para que possam alterar os membros dos grupos, por exemplo.  

No entanto, com protegido contas e grupos, as permissões dos objetos são definidas e impostas por meio de um processo automático que garante que as permissões na permanece objetos consistente, mesmo se os objetos são movidos o diretório. Mesmo se alguém muda manualmente as permissões do objeto um protegido, esse processo garante que as permissões são retornadas rapidamente os padrões.  

### <a name="protected-groups"></a>Grupos protegidos  
A tabela a seguir contém os grupos no Active Directory listados pelo sistema de operacional de controlador de domínio protegidos.  

**Protegido contas e grupos no Active Directory pelo sistema operacional**  

|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4 - Windows Server 2003 RTM**|**Windows Server 2003 SP1 +**|**Windows Server 2012, Windows Server 2008 R2, Windows Server 2008**|  
|Administradores|Operadores de conta|Operadores de conta|Operadores de conta|  
||Administrador|Administrador|Administrador|  
||Administradores|Administradores|Administradores|  
||Operadores de backup|Operadores de backup|Operadores de backup|  
||Editores de certificados|||  
|Administradores de domínio|Administradores de domínio|Administradores de domínio|Administradores de domínio|  
||Controladores de domínio|Controladores de domínio|Controladores de domínio|  
|Administradores corporativos|Administradores corporativos|Administradores corporativos|Administradores corporativos|  
||Krbtgt|Krbtgt|Krbtgt|  
||Operadores de impressão|Operadores de impressão|Operadores de impressão|  
||||Controladores de domínio somente leitura|  
||Replicator|Replicator|Replicator|  
|Administradores de esquema|Administradores de esquema|Administradores de esquema|Administradores de esquema|  
||Operadores de servidor|Operadores de servidor|Operadores de servidor|  

#### <a name="adminsdholder"></a>AdminSDHolder  
A finalidade do objeto AdminSDHolder é fornecer permissões "modelo" para as contas protegidas e grupos no domínio. AdminSDHolder automaticamente é criada como um objeto no contêiner do sistema de cada domínio do Active Directory. O caminho é: **CN = AdminSDHolder, CN = System, DC = < domain_component >, DC = < domain_component >?.**  

Ao contrário da maioria dos objetos no domínio do Active Directory, que pertencem ao grupo Administradores, AdminSDHolder é de propriedade do grupo Admins. do domínio. Por padrão, o EAs podem fazer alterações ao objeto de AdminSDHolder qualquer do domínio, assim como os grupos de administradores de domínio e administradores do domínio. Além disso, embora o proprietário padrão de AdminSDHolder é grupo de administradores de domínio do domínio, os membros dos administradores ou administradores de empresa podem assumir a propriedade do objeto.  

#### <a name="sdprop"></a>SDProp  
SDProp é um processo que é executado a cada 60 minutos (por padrão) no controlador de domínio que mantém o emulador de PDC (PDCE do domínio). SDProp compara as permissões AdminSDHolder objeto do domínio com as permissões no protegido contas e grupos no domínio. Se as permissões em qualquer um dos grupos e contas protegidas não coincidem as permissões no objeto AdminSDHolder, as permissões em protegido contas e grupos são redefinidas para que correspondam do objeto de AdminSDHolder do domínio.  

Além disso, a herança de permissões está desabilitada em grupos protegidos e contas, que significa que mesmo que as contas e grupos são movidos para locais diferentes no diretório, elas não herdam as permissões dos novos objetos pai. Herança é desabilitada no objeto AdminSDHolder para que as alterações de permissão aos objetos pai não alteram as permissões de AdminSDHolder.  

##### <a name="changing-sdprop-interval"></a>Alterando o intervalo de SDProp  
Normalmente, você não deve precisar alterar o intervalo em que é executada SDProp, exceto para fins de teste. Se você precisar alterar o intervalo de SDProp, em PDCE para o domínio, use regedit para adicionar ou modificar o valor de AdminSDProtectFrequency DWORD em HKLM\System\CurrentControlSet\Services\NTDS\Parameters..  

O intervalo de valores é em segundos de 60 para 7200 (um minuto para duas horas). Para reverter as alterações, exclua a chave AdminSDProtectFrequency, o que fará com que SDProp reverter para o intervalo de 60 minutos. Você geralmente deve não reduz esse intervalo em domínios de produção como ele pode aumentar LSASS sobrecarga de processamento no controlador de domínio. O impacto desse aumento depende do número de objetos protegidos no domínio.  

##### <a name="running-sdprop-manually"></a>Executando SDProp manualmente  
Uma melhor abordagem para testar alterações AdminSDHolder é executar SDProp manualmente, que faz com que a tarefa seja executada imediatamente, mas não afeta a execução agendada. Executar SDProp manualmente é realizado ligeiramente diferente em controladores de domínio que executam o Windows Server 2008 e versões anteriores do que em controladores de domínio que executam o Windows Server 2012 ou Windows Server 2008 R2.  

Procedimentos para execução SDProp manualmente em sistemas operacionais mais antigos são fornecidos em [artigo Microsoft Support 251343](https://support.microsoft.com/kb/251343), e a seguir estão instruções passo a passo para sistemas operacionais mais antigos e mais recentes. Em ambos os casos, você deve se conectar ao objeto rootDSE no Active Directory e executar uma operação de modificação com um DN nulo para o objeto rootDSE, especificando o nome da operação como o atributo para modificar. Para obter mais informações sobre operações podem ser modificadas no objeto rootDSE, consulte [rootDSE modificar operações](https://msdn.microsoft.com/library/cc223297.aspx) no site do MSDN.  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>Executando SDProp manualmente no Windows Server 2008 ou anterior  
Você pode forçar SDProp para ser executado usando Ldp.exe ou executar um script de modificação LDAP. Para executar SDProp usando Ldp.exe, execute as etapas a seguir depois que você fez alterações ao objeto AdminSDHolder em um domínio:  

1.  Iniciar **Ldp.exe**.  

2.  Clique em **Conexão** na caixa de diálogo Ldp e clique em **conectar**.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3.  No **conectar** caixa de diálogo caixa, digite o nome do controlador de domínio para o domínio que mantém a função de emulador do PDC (PDCE) e clique em **Okey**.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4.  Verifique se você tiver conectado com êxito, conforme indicado pela **Dn: (RootDSE)** na seguinte captura de tela, clique em **Conexão** e clique em **associar**.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5.  No **associar** caixa de diálogo, digite as credenciais de uma conta de usuário que tenha permissão para modificar o objeto rootDSE. (Se você fizer logon como esse usuário, você pode selecionar **associar como** usuário conectado no momento.) Clique em **Okey**.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6.  Depois de concluir a operação de vinculação, clique em **procurar**e clique em **modificar**.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7.  No **modificar** caixa de diálogo, deixe o **DN** campo espaço em branco. No **atributo de entrada editar** , digite **FixUpInheritance**e no **valores** , digite **Sim**. Clique em **Enter** para popular o **entrada lista** conforme mostrado na captura de tela a seguir.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8.  Na caixa de diálogo Modificar preenchida, clique em executar e verifique se que as alterações feitas ao objeto AdminSDHolder surgiram nesse objeto.  

> [!NOTE]  
> Para obter informações sobre como modificar AdminSDHolder para permitir que contas sem privilégios designadas modificar os membros dos grupos protegidos, consulte [apêndice i: criação de gerenciamento de contas para protegido contas e grupos no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Se você preferir executar SDProp manualmente por meio de LDIFDE ou um script, você pode criar uma entrada de modificar, conforme mostrado aqui:  

![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>Executando SDProp manualmente no Windows Server 2012 ou Windows Server 2008 R2  
Você também pode forçar SDProp para ser executado usando Ldp.exe ou executar um script de modificação LDAP. Para executar SDProp usando Ldp.exe, execute as etapas a seguir depois que você fez alterações ao objeto AdminSDHolder em um domínio:  

1.  Iniciar **Ldp.exe**.  

2.  No **Ldp** caixa de diálogo, clique em **Conexão**e clique em **conectar**.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3.  No **conectar** caixa de diálogo caixa, digite o nome do controlador de domínio para o domínio que mantém a função de emulador do PDC (PDCE) e clique em **Okey**.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4.  Verifique se você tiver conectado com êxito, conforme indicado pela **Dn: (RootDSE)** na seguinte captura de tela, clique em **Conexão** e clique em **associar**.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5.  No **associar** caixa de diálogo, digite as credenciais de uma conta de usuário que tenha permissão para modificar o objeto rootDSE. (Se você fizer logon como esse usuário, você pode selecionar **Bind como feito logon usuário**.) Clique em **Okey**.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6.  Depois de concluir a operação de vinculação, clique em **procurar**e clique em **modificar**.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7.  No **modificar** caixa de diálogo, deixe o **DN** campo espaço em branco. No **atributo de entrada editar** , digite **RunProtectAdminGroupsTask**e no **valores** , digite **1**. Clique em **Enter** para preencher a lista de entrada, conforme mostrado aqui.  

    ![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8.  No preenchida **modificar** caixa de diálogo, clique em **executar**e verifique se que as alterações feitas ao objeto AdminSDHolder surgiram nesse objeto.  

Se você preferir executar SDProp manualmente por meio de LDIFDE ou um script, você pode criar uma entrada de modificar, conforme mostrado aqui:  

![protegido contas e grupos](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
