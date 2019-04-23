---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: Exclusividade de SPN e UPN
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c9a769fdd9fb7d13c47da465b25bc59e7f55237f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856737"
---
# <a name="spn-and-upn-uniqueness"></a>Exclusividade de SPN e UPN

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Engenheiro de escalonamento de suporte sênior Justin Turner com o grupo do Windows  
  
> [!NOTE]  
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.  
  
## <a name="overview"></a>Visão geral  
Controladores de domínio executando o Windows Server 2012 R2 bloquear a criação de duplicar os nomes da entidade de serviço (SPN) e nomes de usuário principal (UPN). Isso inclui se a restauração ou a reanimação de marcas de um objeto excluído ou a renomeação de um objeto resultaria em uma duplicata.  
  
### <a name="background"></a>Histórico  
Nomes de entidade de serviço duplicados (SPN) comumente ocorrer e resultar em falhas de autenticação e pode levar à utilização excessiva da CPU de LSASS. Não há nenhum método de caixa de entrada para bloquear a adição de um SPN duplicado ou UPN. *  
  
Valores duplicados de UPN quebrar a sincronização entre local AD e o Office 365.  
  
*Setspn.exe normalmente é usado para criar novo SPNs e funcionalmente foi criado para a versão lançada com o Windows Server 2008 que adiciona uma verificação de duplicatas.  
  
**Tabela tabela SEQ \\ \* árabe 1: Exclusividade SPN e UPN**  
  
|Recurso|Comentário|  
|-----------|-----------|  
|Exclusividade UPN|Duplicados UPNs quebra a sincronização de contas do AD com serviços baseado no AD do Windows Azure como o Office 365 no local.|  
|Exclusividade SPN|Kerberos requer SPNs para a autenticação mútua.  SPNs duplicados resultam em falhas de autenticação.|  
  
Para obter mais informações sobre os requisitos de exclusividade para UPNs e SPNs, consulte [restrições de exclusividade](https://msdn.microsoft.com/library/dn392337.aspx).  
  
## <a name="symptoms"></a>Sintomas  
Códigos de erro 8467 ou 8468 ou seus hex, simbólico ou equivalentes de cadeia de caracteres são registrados em várias na tela diálogos e no evento 2974 ID no log de eventos de serviços de diretório. A tentativa de criar um UPN ou a SPN duplicado é bloqueada somente nas seguintes circunstâncias:  
  
-   A gravação é processada por um controlador de domínio do Windows Server 2012 R2  
  
**Tabela tabela SEQ \\ \* árabe 2: Códigos de erro de exclusividade SPN e UPN**  
  
|Decimal|Hex|Simbólico|String|  
|-----------|-------|------------|----------|  
|8467|21C7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|A operação falhou porque o valor SPN fornecido para adição/modificação não é exclusivo em toda a floresta.|  
|8648|21C8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|A operação falhou porque o valor UPN fornecido para adição/modificação não é exclusivo em toda a floresta.|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>Criação do novo usuário falhará se o UPN não é exclusivo  
  
### <a name="dsamsc"></a>DSA.msc  
O nome de logon de usuário que você escolheu já está em uso nesta empresa. Escolha outro nome de logon e, em seguida, tente novamente.  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
Modificar uma conta existente:  
  
O nome de logon do usuário especificado já existe na empresa. Especifique um novo, alterando o prefixo ou selecionando um sufixo diferente na lista.  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Centro Administrativo do Active Directory (DSAC.exe)  
Uma tentativa de criar um novo usuário no Centro Administrativo do Active Directory com o UPN que já existe gerará o erro a seguir.  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**Figura Figura SEQ \\ \* erro árabe 1 exibido na Central Administrativa do AD quando a criação do novo usuário falha devido ao UPN duplicado**  
  
### <a name="event-2974-source-activedirectorydomainservice"></a>Origem do evento 2974: ActiveDirectory_DomainService  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**Figura Figura SEQ \\ \* 2974 árabe da ID de evento 2 com erro 8648**  
  
O evento 2974 lista o valor que foi bloqueado e uma lista de objetos de um ou mais (até 10) que já contêm esse valor.  Na figura a seguir, você pode ver esse valor de atributo UPN ***dhunt@blue.contoso.com*** já existe em quatro outros objetos.  Como esse é um novo recurso no Windows Server 2012 R2, criação acidental de UPN e o SPNs duplicados em um ambiente misto ainda ocorrerá quando a tentativa de gravação de processar os controladores de domínio de nível inferior.  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**Figura Figura SEQ \\ \* 2974 de evento árabe 3 mostrando todos os objetos que contém o UPN duplicado**  
  
> [!TIP]  
> Revise o evento ID 2974s regularmente a:  
>   
> -   identificar tentativas de criar SPNs ou UPN duplicado  
> -   identificar os objetos que já contêm duplicatas  
  
8648 = "A operação falhou porque o valor UPN fornecido para adição/modificação não é exclusivo em toda a floresta".  
  
### <a name="setspn"></a>SetSPN:  
Setspn.exe teve a detecção de duplicidades SPN incorporada a ele desde o lançamento do Windows Server 2008 ao usar o **"-S"** opção.  Você pode ignorar a detecção de duplicidades SPN usando o **"-um"** opção no entanto.  Criação de um SPN duplicado é bloqueada durante o direcionamento para um Windows Server 2012 R2 controlador de domínio usando o SetSPN com a opção - A.  A mensagem de erro exibida é o mesmo que o exibido ao usar a opção -S: "SPN duplicado encontrado, anulando operação!"  
  
### <a name="adsiedit"></a>ADSIEDIT:  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**Figura Figura SEQ \\ \* mensagem de erro de 4 árabe exibida em ADSIEdit quando a adição de UPN duplicado é bloqueada**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2:  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
PS em execução do Server 2012, visando um controlador de domínio do Windows Server 2012 R2:  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC.exe em execução no Windows Server 2012, visando um controlador de domínio do Windows Server 2012 R2:  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**Figura Figura SEQ \\ \* erro de criação de usuário árabe DSAC de 5 em não - Windows Server 2012 R2 ao direcionar o controlador de domínio do Windows Server 2012 R2**  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**Figura Figura SEQ \\ \* erro de modificação de usuário árabe DSAC de 6 em não - Windows Server 2012 R2 ao direcionar o controlador de domínio do Windows Server 2012 R2**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>Falha na restauração de um objeto que pode resultar em um UPN duplicado:  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
Nenhum evento é registrado quando um objeto há falha na restauração devido a um UPN duplicado / SPN.  
  
O UPN do objeto deve ser exclusivo para que ele seja restaurado.  
  
1.  Identificar o UPN que existe no objeto na Lixeira  
  
2.  Identificar todos os objetos que têm o mesmo valor  
  
3.  Remover o UPN(s) duplicados  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>Identificar o UPN conflitante no repadmin.exe objectUsing excluído  
  
```  
Repadmin /showattr DCName "DN of deleted objects container" /subtree /filter:"(msDS-LastKnownRDN=<NAME>)" /deleted /atts:userprincipalname  
```  
  
```  
repadmin /showattr DCName "CN=Deleted Objects,DC=blue,DC=contoso,DC=com" /subtree /filter:"(msDS-LastKnownRDN=Dianne Hunt2)" /deleted /atts:userprincipalname  
  
C:\>repadmin /showattr winbluedc1 "cn=deleted objects,dc=blue,dc=contoso,dc=com" /subtree /filter:"(msds-lastknownrdn=Dianne Hunt2)" /deleted /atts:userprincipalname  
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Object  
s,DC=blue,DC=contoso,DC=com  
    1> userPrincipalName: dhunt@blue.contoso.com  
```  
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>Para identificar todos os objetos com o mesmo UPN: Repadmin.exe usando  
  
```  
repadmin /showattr WinBlueDC1 "DC=blue,DC=contoso,DC=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN  
  
C:\>repadmin /showattr winbluedc1 "dc=blue,dc=contoso,dc=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN  
DN: CN=Administrator,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser1,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser10,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser100,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=Dianne Hunt,OU=Marketing,DC=blue,DC=contoso,DC=com  
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Objects,DC=blue,DC=contoso,DC=com  
```  
  
> [!TIP]  
> Anteriormente não documentados **/ excluídas** parâmetro em repadmin.exe é usado para incluir objetos excluídos no conjunto de resultados  
  
### <a name="using-global-search"></a>Usando a pesquisa Global  
  
-   Abrir Centro Administrativo e navegue até **Pesquisa Global**  
  
-   Selecione o **converter em LDAP** botão de opção  
  
-   Type **(userPrincipalName=*ConflictingUPN*)**  
  
    -   Substitua ***ConflictingUPN*** com o UPN real que está em conflito  
  
-   Selecione **aplicar**  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>Usando o Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
Se o objeto precisa ser restaurado, você precisa removerá os UPNs duplicados de outros objetos.  Para apenas um objeto, é simple o suficiente para usar ADSIEdit para remover a duplicata.  Se houver vários objetos com as duplicatas, o Windows PowerShell pode ser a melhor ferramenta para usar.  
  
Para anular o atributo UserPrincipalName usando o Windows PowerShell:  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> O atributo userPrincipalName é o atributo de valor único, portanto, esse procedimento só removerá o UPN duplicado.  
  
### <a name="duplicate-spn"></a>SPN duplicado  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**Figura Figura SEQ \\ \* mensagem de erro de 8 árabe exibida em ADSIEdit quando a adição de SPN duplicado é bloqueada**  
  
Registrado nos serviços de diretório de log de eventos é um **Domainservice** ID do evento **2974**.  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**Figura Figura SEQ \\ \* árabe erro que 9 é registrado quando a criação de SPN duplicado está bloqueada**  
  
### <a name="workflow"></a>Fluxo de Trabalho  
  
-   **If DC == GC**  
  
    -   Nenhuma chamada offbox necessária, a consulta pode ser atendida localmente  
  
    -   ***Caso UPN***  
  
        -   Índice UPN toda a floresta de consulta local para o UPN fornecido (*userPrincipalName; um índice global*)  
  
            -   Se as entradas retornadas = = 0 -> prossegue de gravação  
  
            -   Se as entradas retornadas! = 0 -> Falha de gravação  
  
                -   Evento registrado  
  
                -   Também retorna erro estendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   Índice SPN toda a floresta de consulta local para o SPN fornecido (*servicePrincipalName; um índice global*)  
  
            -   Se as entradas retornadas = = 0 -> prossegue de gravação  
  
            -   Se as entradas retornadas! = 0 -> Falha de gravação  
  
                -   Evento registrado  
  
                -   Também retorna erro estendido:  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **If DC != GC**  
  
    -   Chamada de Offbox **desejável** , mas não críticos, ou seja, isso é uma verificação de exclusividade de melhor esforço  
  
        -   Verificação continua em relação a DIT local somente se o GC não pode ser localizado  
  
        -   Evento registrado para indicar como  
  
    -   ***Caso UPN***  
  
        -   Enviar a consulta LDAP no GC mais próximo? índice UPN de toda a floresta de consulta do GC para UPN fornecido (*userPrincipalName; um índice global*)  
  
            -   Se as entradas retornadas = = 0 -> prossegue de gravação  
  
            -   Se as entradas retornadas! = 0 -> Falha de gravação  
  
                -   Evento registrado  
  
                -   Também retorna erro estendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   Enviar a consulta LDAP no GC mais próximo? índice SPN de toda a floresta de consulta do GC para SPN fornecido (*servicePrincipalName; um índice global*)  
  
            -   Se as entradas retornadas = = 0 -> prossegue de gravação  
  
            -   Se as entradas retornadas! = 0 -> Falha de gravação  
  
                -   Evento registrado  
  
                -   Também retorna erro estendido:  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
Quando os objetos excluídos são animados novamente, valores SPN ou UPN presentes são verificados quanto à exclusividade. Se uma duplicata for encontrada, a solicitação falhará.  
  
-   Para determinadas alterações de atributo, como o nome de Host de DNS, etc. nome de conta SAM, quando a modificação é feita, os SPNs são atualizados adequadamente. No processo, os SPNs obsoletos são excluídos e novos SPNs são construídos e adicionados ao banco de dados. As modificações de atributo necessárias em relação ao qual esse caminho é disparado são:  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
Se qualquer um do novo valor SPN é uma duplicata, podemos falhar a modificação. Da lista acima, os atributos importantes estão ATT_DNS_HOST_NAME (nome do computador) e ATT_SAM_ACCOUNT_NAME (nome da conta SAM).  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>Tente o seguinte: Explorando a exclusividade de SPN e UPN  
Este é o primeiro de vários "**tente isso**" atividades no módulo.  Não é um guia de laboratório separado para este módulo.  O **tente isso** atividades são essencialmente as atividades de forma livre que permitem que você explore o material da lição no ambiente de laboratório.  Você tem a opção de seguir o prompt ou fora de script e surgir com sua própria atividade.  
  
> [!NOTE]  
> -   Este é o primeiro de vários "**tente isso**" atividades.  
> -   Não é um guia de laboratório separado para este módulo.  
> -   O **tente isso** atividades são essencialmente as atividades de forma livre que permitem que você explore o material da lição no ambiente de laboratório.  
> -   Você tem a opção de seguir o prompt ou fora de script e surgir com sua própria atividade.  
> -   Embora nem todas as seções a seguir têm uma **tente isso** prompt, você ainda é incentivados a explorar o conteúdo de lição no laboratório onde for apropriado.  
  
Experimente com exclusividade de SPN e UPN.  Siga esses prompts ou conclua seu próprio.  
  
1.  Criar novos usuários com o UPN  
  
2.  Criar contas com SPNs  
  
3.  Criar um novo usuário com um UPN definido já anteriormente ou alterar o UPN da conta existente.  Faça o mesmo para um SPN na outra conta  
  
    1.  Preencher uma conta de usuário com um UPN já está em uso  
  
        1.  Usando o Centro Administrativo do Active Directory, ADSIEDIT ou do PowerShell (DSAC.exe)  
  
    2.  Preencher uma conta existente com um SPN já está em uso  
  
        1.  Usando o Windows PowerShell, ADSIEDIT ou SetSPN  
  
4.  Observe os erros  
  
**Opcionalmente**  
  
1.  Verificar com o instrutor em sala de aula é okey para habilitar o *[Lixeira do AD](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin)* no Centro Administrativo do Active Directory.  Nesse caso, vá para a próxima etapa.  
  
2.  Preencher o UPN em uma conta de usuário  
  
3.  Excluir a conta  
  
4.  Preencher uma conta diferente com o mesmo UPN como a conta excluída  
  
5.  Tentativa de usar a GUI de Bin Lixeira para restaurar a conta  
  
6.  Imagine que você apenas foi apresentado com o erro que você vê na etapa anterior.  (e não têm um histórico das etapas que você acabou de ser executada) Sua meta é concluir a restauração da conta.  Consulte que a pasta de trabalho por exemplo etapas.  
  


