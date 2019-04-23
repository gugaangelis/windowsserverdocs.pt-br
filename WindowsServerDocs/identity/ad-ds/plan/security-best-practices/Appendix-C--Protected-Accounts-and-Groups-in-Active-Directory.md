---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: Apêndice C - protegida contas e grupos no Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70e29ad42b57cf315c7179d6eea8220ad2bfab48
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834697"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Apêndice C: Contas protegidas e grupos no Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Apêndice C: Contas protegidas e grupos no Active Directory

Dentro do Active Directory, um conjunto padrão de contas altamente privilegiadas e grupos são consideradas contas e grupos protegidos. Com a maioria dos objetos no Active Directory, os administradores delegados (usuários que tenham sido delegadas permissões para gerenciar objetos do Active Directory) podem alterar permissões em objetos, incluindo alterar permissões para que possam alterar as associações de os grupos, por exemplo.  

No entanto, com contas protegidas e grupos, permissões de objetos são definidas e aplicadas por meio de um processo automático que garante que as permissões no permanece objetos, consistente, mesmo se os objetos forem movidos o diretório. Mesmo se alguém altera manualmente as permissões de um objeto protegido, esse processo garante que as permissões são retornadas aos seus padrões rapidamente.  

### <a name="protected-groups"></a>Grupos protegidos

A tabela a seguir contém os grupos protegidos no Active Directory listados pelo sistema de operacional do controlador de domínio.  

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>Contas protegidas e grupos no Active Directory pelo sistema operacional

| Windows Server 2003 RTM | Windows Server 2003 SP1+ | Windows Server 2012, <br> Windows Server 2008 R2, <br> Windows Server 2008 | Windows Server 2016 |
| --- | --- | --- | --- |
|Opers. de contas|Opers. de contas|Opers. de contas|Opers. de contas|
|Administrador|Administrador|Administrador|Administrador|
|Administradores|Administradores|Administradores|Administradores|
|Operadores de cópia|Operadores de cópia|Operadores de cópia|Operadores de cópia|
|Editores de certificados|||
|Administradores do domínio|Administradores do domínio|Administradores do domínio|Administradores do domínio|
|Controladores de Domínio|Controladores de Domínio|Controladores de Domínio|Controladores de Domínio|
|Administrador corporativo|Administrador corporativo|Administrador corporativo|Administrador corporativo|
||||Administradores de chave da empresa|
||||Administradores de chave|
|Krbtgt|Krbtgt|Krbtgt|Krbtgt|
|Operadores de Impressão|Operadores de Impressão|Operadores de Impressão|Operadores de Impressão|
|||Controladores de Domínio somente leitura|Controladores de Domínio somente leitura|
|Duplicador|Duplicador|Duplicador|Duplicador|
|Administradores de esquemas|Administradores de esquemas|Administradores de esquemas|Administradores de esquemas|
|Opers. de servidores|Opers. de servidores|Opers. de servidores|Opers. de servidores|

#### <a name="adminsdholder"></a>AdminSDHolder

A finalidade do objeto AdminSDHolder é fornecer permissões de "modelo" para as contas e grupos protegidos no domínio. AdminSDHolder é criado automaticamente como um objeto no contêiner do sistema de cada domínio do Active Directory. O caminho dele é: **CN = AdminSDHolder, CN = System, DC = < domain_component >, DC = < domain_component >?.**  

Ao contrário da maioria dos objetos no domínio do Active Directory, que são propriedade do grupo Administradores, AdminSDHolder é propriedade do grupo Admins. do domínio. Por padrão, EAs pode fazer alterações AdminSDHolder objeto de qualquer domínio, como grupos de Admins. do domínio e administradores do domínio. Além disso, embora o proprietário padrão AdminSDHolder é o grupo de administradores de domínio do domínio, membros de administradores ou administradores de empresa podem assumir a propriedade do objeto.  

#### <a name="sdprop"></a>SDProp

SDProp é um processo que é executado a cada 60 minutos (por padrão) no controlador de domínio que mantém o emulador de PDC (PDCE do domínio). SDProp compara as permissões no objeto de AdminSDHolder do domínio com as permissões a contas e grupos protegidos no domínio. Se as permissões em qualquer um dos grupos e contas protegidas não coincidem com as permissões no objeto AdminSDHolder, as permissões a contas e grupos protegidos serão redefinidas para coincidir com aqueles do objeto de AdminSDHolder do domínio.  

Além disso, a herança de permissões é desabilitada em grupos protegidos e contas, que significa que mesmo se as contas e grupos são movidos para locais diferentes no diretório, eles não herdam permissões seus novos objetos pai. Herança está desabilitada no objeto AdminSDHolder, para que as alterações de permissão aos objetos pai não alteram as permissões de AdminSDHolder.  

##### <a name="changing-sdprop-interval"></a>Alterando o intervalo de SDProp

Normalmente, não será preciso alterar o intervalo no qual é executado SDProp, exceto para fins de teste. Se você precisar alterar o intervalo de SDProp no PDCE para o domínio, use regedit para adicionar ou modificar o valor de DWORD AdminSDProtectFrequency em HKLM\System\CurrentControlSet\Services\NTDS\Parameters.  

O intervalo de valores é em segundos, de 60 a 7200 (um minuto para duas horas). Para reverter as alterações, exclua a chave de AdminSDProtectFrequency, o que fará com que SDProp reverter para o intervalo de 60 minutos. Você geralmente deve não reduzir esse intervalo em domínios de produção como ele pode aumentar a sobrecarga de processamento no controlador de domínio do LSASS. O impacto desse aumento é dependente do número de objetos protegidos no domínio.  

##### <a name="running-sdprop-manually"></a>Execução manual de SDProp

Uma abordagem melhor para testar as alterações de AdminSDHolder é executar SDProp manualmente, que faz com que a tarefa seja executada imediatamente, mas não afeta a execução agendada. Execução manual de SDProp é executada de forma ligeiramente diferente em controladores de domínio executando o Windows Server 2008 e versões anteriores do que em controladores de domínio que executam o Windows Server 2012 ou Windows Server 2008 R2.  

Procedimentos para execução manual de SDProp em sistemas operacionais mais antigos são fornecidos no [artigo do Microsoft Support 251343](https://support.microsoft.com/kb/251343), e a seguir estão as instruções passo a passo para sistemas operacionais mais antigos e mais recentes. Em ambos os casos, você deve conectar-se ao objeto rootDSE no Active Directory e executar uma operação de modificação com um DN nulo para o objeto rootDSE, especificando o nome da operação como o atributo para modificar. Para obter mais informações sobre as operações modificáveis do objeto rootDSE, consulte [rootDSE modificar operações](https://msdn.microsoft.com/library/cc223297.aspx) no site do MSDN.  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>Executando SDProp manualmente no Windows Server 2008 ou anterior

Você pode forçar SDProp para ser executado usando Ldp.exe ou executando um script de modificação de LDAP. Para executar SDProp usando Ldp.exe, execute as etapas a seguir após você ter feito alterações ao objeto AdminSDHolder em um domínio:  

1. Inicie **Ldp.exe**.  
2. Clique em **Conexão** na caixa de diálogo Ldp e clique **Connect**.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3. No **Connect** diálogo caixa, digite o nome do controlador de domínio para o domínio que detém a função de emulador de PDC (PDCE) e clique em **Okey**.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4. Verifique se que você se conectou com êxito, conforme indicado pelo **Dn: (RootDSE)**  na seguinte captura de tela, clique em **Conexão** e clique em **associar**.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5. No **associar** caixa de diálogo, digite as credenciais de uma conta de usuário que tenha permissão para modificar o objeto rootDSE. (Se você estiver conectado como esse usuário, você pode selecionar **associar como** usuário atualmente conectado.) Clique em **OK**.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6. Depois de concluir a operação de associação, clique em **navegue**e clique em **modificar**.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7. No **Modify** caixa de diálogo, deixe o **DN** espaço em branco o campo. No **Editar atributo de entrada** , digite **FixUpInheritance**e, nas **valores** , digite **Sim**. Clique em **Enter** para popular o **lista de entrada** conforme mostrado na seguinte captura de tela.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8. Na caixa de diálogo Modificar preenchida, clique em executar e verificar que as alterações feitas no objeto AdminSDHolder apareceu naquele objeto.  

> [!NOTE]  
> Para obter informações sobre como modificar o AdminSDHolder para permitir que contas sem privilégios designadas modificar a associação de grupos protegidos, consulte [i Apêndice: Criação de contas de gerenciamento para protegidos em grupos e contas no Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Se você preferir executar SDProp manualmente por meio da LDIFDE ou um script, você pode criar uma entrada de modificar, como mostrado aqui:  

![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>Execução manual de SDProp no Windows Server 2012 ou Windows Server 2008 R2

Você também pode forçar SDProp para ser executado usando Ldp.exe ou executando um script de modificação de LDAP. Para executar SDProp usando Ldp.exe, execute as etapas a seguir após você ter feito alterações ao objeto AdminSDHolder em um domínio:  

1. Inicie **Ldp.exe**.  

2. No **Ldp** caixa de diálogo, clique em **Conexão**e clique em **Connect**.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3. No **Connect** diálogo caixa, digite o nome do controlador de domínio para o domínio que detém a função de emulador de PDC (PDCE) e clique em **Okey**.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4. Verifique se que você se conectou com êxito, conforme indicado pelo **Dn: (RootDSE)**  na seguinte captura de tela, clique em **Conexão** e clique em **associar**.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5. No **associar** caixa de diálogo, digite as credenciais de uma conta de usuário que tenha permissão para modificar o objeto rootDSE. (Se você estiver conectado como esse usuário, você pode selecionar **Bind usuário conectado atualmente como**.) Clique em **OK**.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6. Depois de concluir a operação de associação, clique em **navegue**e clique em **modificar**.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7. No **Modify** caixa de diálogo, deixe o **DN** espaço em branco o campo. No **Editar atributo de entrada** , digite **RunProtectAdminGroupsTask**e, nas **valores** , digite **1**. Clique em **Enter** para preencher a lista de entrada, conforme mostrado aqui.  

   ![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8. No populada **Modify** caixa de diálogo, clique em **executar**e verifique se que as alterações feitas no objeto AdminSDHolder apareceu naquele objeto.  

Se você preferir executar SDProp manualmente por meio da LDIFDE ou um script, você pode criar uma entrada de modificar, como mostrado aqui:  

![grupos e contas protegidas](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
