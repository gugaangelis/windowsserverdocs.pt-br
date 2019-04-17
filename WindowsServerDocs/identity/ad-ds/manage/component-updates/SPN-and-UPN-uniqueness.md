---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: Exclusividade SPN e UPN
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 81d686c81082ad29384585d541c1304d654e1924
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="spn-and-upn-uniqueness"></a>Exclusividade SPN e UPN

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: sênior Justin Turner engenheiro de escalonamento de suporte com o grupo do Windows  
  
> [!NOTE]  
> Este conteúdo foi criado por um engenheiro de suporte de cliente da Microsoft e se destina a arquitetos de sistemas que estão procurando mais profundamente explicações técnicas de recursos e soluções no Windows Server 2012 R2 de tópicos no TechNet geralmente fornecem e administradores experientes. No entanto, ele não passou os mesmos edição passos, para que a linguagem podem parecer que menos refinado que o que geralmente é encontrado no TechNet.  
  
## <a name="overview"></a>Visão geral  
Controladores de domínio que executam o Windows Server 2012 R2 bloco a criação de duplicam nomes de entidade de serviço (SPN) e nomes de entidade de segurança do usuário (UPN). Isso inclui se a restauração ou reanimação de um objeto excluído ou a renomeação de um objeto resultaria em uma duplicação.  
  
### <a name="background"></a>Em segundo plano  
Nomes de entidade de serviço duplicada (SPN) normalmente ocorrer e resultar em falhas de autenticação e pode levar a utilização de CPU LSASS excessiva. Não há nenhum método nativos para bloquear a adição de um SPN duplicado ou UPN. *  
  
Valores de nome UPN duplicados quebrar a sincronização entre locais AD e o Office 365.  
  
*Setspn.exe costuma ser usado para criar novos SPNs e funcionalmente foi criado para a versão lançada com o Windows Server 2008, que adiciona uma verificação de duplicatas.  
  
**Tabela SEQ Table \ \ \ * árabe 1: exclusividade UPN e SPN**  
  
|Recurso|Comentário|  
|-----------|-----------|  
|Exclusividade UPN|Duplicar UPNs quebra sincronização de contas do AD com serviços do Windows Azure baseada no AD como o Office 365 no local.|  
|Exclusividade SPN|Kerberos requer SPNs para autenticação mútua.  SPNs duplicados resultam em falhas de autenticação.|  
  
Para obter mais informações sobre os requisitos de exclusividade para UPNs e SPNs, consulte [restrições de exclusividade](https://msdn.microsoft.com/library/dn392337.aspx).  
  
## <a name="symptoms"></a>Sintomas  
Códigos de erro 8467 ou 8468 ou seus hex, simbólico ou equivalentes de cadeia de caracteres são registradas em log vários na tela diálogos e no evento ID 2974 no log de eventos dos serviços de diretório. A tentativa de criar um nome UPN ou o SPN duplicado é bloqueada somente nas seguintes circunstâncias:  
  
-   A gravação é processada por um controlador de domínio do Windows Server 2012 R2  
  
**Tabela SEQ Table \ \ \ * árabe 2: códigos de erro de exclusividade UPN e SPN**  
  
|Decimal|Hex|Simbólicos|Cadeia de caracteres|  
|-----------|-------|------------|----------|  
|8467|21C 7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|A operação falhou porque o valor SPN fornecido para adição/modificação não é exclusiva de toda a floresta.|  
|8648|8 21C|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|A operação falhou porque o valor de nome UPN fornecido para adição/modificação não é exclusiva de toda a floresta.|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>Criação de usuário novo falhará se UPN não é exclusiva  
  
### <a name="dsamsc"></a>DSA  
O nome de logon de usuário que você tenha escolhido já está em uso nesta empresa. Escolha outro nome de logon e tente novamente.  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
Modificar uma conta existente:  
  
O nome de logon do usuário especificado já existe na empresa. Especifique um novo nome, alterando o prefixo ou selecionando um sufixo diferente da lista.  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Centro Administrativo do Active Directory (DSAC.exe)  
Uma tentativa de criar um novo usuário no Centro Administrativo do Active Directory com um nome UPN que já existe produzirá o seguinte erro.  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**Figura Figura SEQ \ \ \ * 1 árabe erro exibido no Centro Administrativo do AD quando a criação do novo usuário falhe UPN duplicado**  
  
### <a name="event-2974-source-activedirectorydomainservice"></a>Origem de evento 2974: ActiveDirectory_DomainService  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**Figura Figura SEQ \ \ \ * 2974 árabe da ID de evento 2 com erro 8648**  
  
O evento 2974 contém o valor que foi bloqueado e uma lista de um ou mais objetos (até 10) que contêm já esse valor.  Na figura a seguir, você pode ver esse valor de atributo UPN *** dhunt@blue.contoso.com *** já existe em quatro outros objetos.  Como este é um novo recurso no Windows Server 2012 R2, criação acidental de duplicados UPN e SPNs em um ambiente misto ainda ocorrerá quando controladores de domínio de nível inferior processam a tentativa de gravação.  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**Figura Figura SEQ \ \ \ * árabe 2974 de evento 3 mostrando todos os objetos que contêm o UPN duplicado**  
  
> [!TIP]  
> Examine o evento ID 2974s regularmente para:  
>   
> -   identificar tentativas de criar UPN duplicado ou SPNs  
> -   identificar os objetos que contêm já duplicatas  
  
8648 = "A operação falhou porque o valor de nome UPN fornecido para adição/modificação não é exclusiva de toda a floresta."  
  
### <a name="setspn"></a>SetSPN:  
Setspn.exe teve duplicada detecção SPN interna a ele desde o lançamento do Windows Server 2008 ao usar o **"-S"** opção.  Você pode ignorar a detecção de SPN duplicada usando o **"-A"** opção porém.  Criação de um SPN duplicado é bloqueada direcionando um Windows Server 2012 R2 controlador de domínio usando SetSPN com a opção - A.  A mensagem de erro exibida é a mesma que foi exibido ao usar a opção -S: "Duplicar SPN encontrado, anulando operação!"  
  
### <a name="adsiedit"></a>ADSIEDIT:  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**Figura Figura SEQ \ \ \ * mensagem de erro de 4 árabe exibida em ADSIEdit quando adição de UPN duplicado estiver bloqueada**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2:  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
PS executando de Server 2012 direcionando um controlador de domínio do Windows Server 2012 R2:  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC.exe em execução no Windows Server 2012 direcionando um controlador de domínio do Windows Server 2012 R2:  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**Figura Figura SEQ \ \ \ * erro de criação de usuário árabe DSAC 5 em não - Windows Server 2012 R2 enquanto direcionando o controlador de domínio do Windows Server 2012 R2**  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**Figura Figura SEQ \ \ \ * erro de modificação de usuário árabe DSAC 6 em não - Windows Server 2012 R2 enquanto direcionando o controlador de domínio do Windows Server 2012 R2**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>Falha na restauração de um objeto que resultaria em um UPN duplicado:  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
Nenhum evento é registrado quando um objeto Falha ao restaurar devido a um UPN duplicado / SPN.  
  
O UPN do objeto deve ser exclusivo para que eles sejam restaurados.  
  
1.  Identificar o UPN que existe no objeto da Lixeira  
  
2.  Identificar todos os objetos que têm o mesmo valor  
  
3.  Remover o UPN(s) duplicados  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>Identificar o UPN conflitante no repadmin.exe objectUsing excluídos  
  
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
> Anteriormente não documentadas **/ excluído** parâmetro em repadmin.exe é usado para incluir objetos excluídos no conjunto de resultados  
  
### <a name="using-global-search"></a>Usando a pesquisa Global  
  
-   Centro Administrativo do Open Active Directory e navegue até **Pesquisa Global**  
  
-   Selecione o **converter em LDAP** botão de opção  
  
-   Tipo * *(userPrincipalName =*ConflictingUPN*) * *  
  
    -   Substitua ***ConflictingUPN*** com o UPN real que está em conflito  
  
-   Selecione **aplicar**  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>Usando o Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
Se o objeto precisa ser restaurado, você precisa removerá os UPNs duplicadas de outros objetos.  Para um único objeto, é simple usar ADSIEdit para remover a duplicar.  Se houver vários objetos com duplicatas, o Windows PowerShell pode ser a melhor ferramenta.  
  
Como null, o atributo UserPrincipalName usando o Windows PowerShell:  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> O atributo userPrincipalName é único valor de atributo, portanto, este procedimento somente removerá o UPN duplicado.  
  
### <a name="duplicate-spn"></a>Duplicar SPN  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**Figura Figura SEQ \ \ \ * mensagem de erro de 8 árabe exibida em ADSIEdit quando adição de SPN duplicado estiver bloqueada**  
  
Logon em serviços de diretório do log de eventos é um **ActiveDirectory_DomainService** ID de evento **2974**.  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![Exclusividade SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**Figura Figura SEQ \ \ \ * árabe erro 9 registrados em log quando a criação de SPN duplicado é bloqueada**  
  
### <a name="workflow"></a>Fluxo de trabalho  
  
-   **Se DC = = GC**  
  
    -   Nenhuma chamada offbox necessária, consulta pode ser atendida localmente  
  
    -   ***Caso UPN***  
  
        -   Índice UPN toda a floresta de consulta local para UPN fornecido (*userPrincipalName; um índice global*)  
  
            -   Se as entradas retornadas = = 0 -> receita de gravação  
  
            -   Se as entradas retornadas! = 0 -> Falha de gravação  
  
                -   Evento de logon  
  
                -   Também retorna o erro estendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   Índice SPN toda a floresta de consulta local para o SPN fornecido (*servicePrincipalName; um índice global*)  
  
            -   Se as entradas retornadas = = 0 -> receita de gravação  
  
            -   Se as entradas retornadas! = 0 -> Falha de gravação  
  
                -   Evento de logon  
  
                -   Também retorna o erro estendido:  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **Se DC! = GC**  
  
    -   Chamada de Offbox **desejável** mas não essencial, ou seja, essa é uma verificação de exclusividade de melhor esforço  
  
        -   Verificação continua contra DIT local somente se o GC não pode ser localizado  
  
        -   Evento registrado para indicar como  
  
    -   ***Caso UPN***  
  
        -   Enviar consulta LDAP GC mais próxima? índice UPN de toda a floresta de consulta do GC para UPN fornecido (*userPrincipalName; um índice global*)  
  
            -   Se as entradas retornadas = = 0 -> receita de gravação  
  
            -   Se as entradas retornadas! = 0 -> Falha de gravação  
  
                -   Evento de logon  
  
                -   Também retorna o erro estendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   Enviar consulta LDAP GC mais próxima? índice SPN de toda a floresta de consulta do GC de SPN fornecido (*servicePrincipalName; um índice global*)  
  
            -   Se as entradas retornadas = = 0 -> receita de gravação  
  
            -   Se as entradas retornadas! = 0 -> Falha de gravação  
  
                -   Evento de logon  
  
                -   Também retorna o erro estendido:  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
Quando os objetos excluídos são animados novamente, SPN ou UPN valores presentes são verificadas para confirmar a exclusividade. Se uma duplicata for encontrada, a solicitação falhará.  
  
-   Para determinadas alterações de atributo como o nome de Host DNS, etc. nome da conta SAM, quando é feita a modificação, SPNs são atualizados apropriadamente. O processo, os SPNs obsoletos são excluídos e SPNs novos são construídos e adicionados ao banco de dados. As modificações de atributo requisito em relação ao qual esse caminho é disparado são:  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
Se qualquer um do novo valor SPN é uma duplicata, nós falhar a modificação. Da lista acima, os atributos importantes são ATT_DNS_HOST_NAME (nome do computador) e ATT_SAM_ACCOUNT_NAME (nome da conta SAM).  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>Tente o seguinte: Explorando a exclusividade SPN e UPN  
Este é o primeiro de vários "**tentar fazer isso**" atividades no módulo.  Não há um guia separada de laboratório para esse módulo.  O **tentar fazer isso** atividades são essencialmente as atividades de forma livre que permitem que você explore o material lição no ambiente de laboratório.  Você tem a opção de visitar o prompt ou disparar script e inventar sua própria atividade.  
  
> [!NOTE]  
> -   Este é o primeiro de vários "**tentar fazer isso**" atividades.  
> -   Não há um guia separada de laboratório para esse módulo.  
> -   O **tentar fazer isso** atividades são essencialmente as atividades de forma livre que permitem que você explore o material lição no ambiente de laboratório.  
> -   Você tem a opção de visitar o prompt ou disparar script e inventar sua própria atividade.  
> -   Embora nem todas as seções têm uma **tentar fazer isso** prompt, é recomendável ainda para explorar o conteúdo lição no laboratório onde apropriado.  
  
Faça testes com exclusividade SPN e UPN.  Siga estas instruções ou concluir sua própria.  
  
1.  Criar novos usuários com UPN  
  
2.  Criar contas com SPNs  
  
3.  Crie um novo usuário com um nome UPN já anteriormente definido ou alterar o nome UPN da conta existente.  Faça o mesmo para um SPN em outra conta  
  
    1.  Popule uma conta de usuário existente com um nome UPN já está em uso  
  
        1.  Usando o Centro Administrativo do PowerShell, ADSIEDIT ou no Active Directory (DSAC.exe)  
  
    2.  Popule uma conta existente com um SPN já está em uso  
  
        1.  Usando o Windows PowerShell, ADSIEDIT ou SetSPN  
  
4.  Observe os erros  
  
**Opcionalmente**  
  
1.  Verifique com o instrutor de sala de aula que é okey para habilitar o * [Lixeira AD](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin) * no Centro Administrativo do Active Directory.  Em caso afirmativo, vá para a próxima etapa.  
  
2.  Popule o UPN em uma conta de usuário  
  
3.  Excluir a conta  
  
4.  Popule uma conta diferente com o mesmo UPN como a conta excluída  
  
5.  Tentativa de usar a interface gráfica reciclar Bin para restaurar a conta  
  
6.  Imagine que ter sido apresentado apenas com o erro que você vê na etapa anterior.  (e não tem um histórico das etapas que você acabou de ser executada) Seu objetivo é concluir a restauração da conta.  Veja como as etapas na pasta de trabalho.  
  


