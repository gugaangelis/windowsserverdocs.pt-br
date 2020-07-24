---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: Exclusividade de SPN e UPN
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: bca2934b5b691f69fc70cd9d5230a2865b24ac94
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966418"
---
# <a name="spn-and-upn-uniqueness"></a>Exclusividade de SPN e UPN

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Justin Turner, engenheiro de escalonamento de suporte sênior com o grupo do Windows  
  
> [!NOTE]  
> Este documento foi criado por um engenheiro de atendimento ao cliente da Microsoft e é destinado a administradores e arquitetos de sistemas experientes que procuram explicações técnicas mais profundas para recursos e soluções no Windows Server 2012 R2 do que aquelas geralmente oferecidas em tópicos do TechNet. No entanto, ele não passou pelas mesmas etapas de edição que eles, por isso a linguagem pode parecer que menos refinada do que a geralmente encontrada no TechNet.  
  
## <a name="overview"></a>Visão geral  
Controladores de domínio que executam o Windows Server 2012 R2 bloqueiam a criação de SPN (nomes de entidade de serviço) duplicados e UPN (nomes de entidade de usuário). Isso inclui se a restauração ou reanimação de um objeto excluído ou a renomeação de um objeto resultaria em uma duplicata.  
  
### <a name="background"></a>Segundo plano  
Normalmente, os nomes de entidade de serviço (SPN) duplicados ocorrem e resultam em falhas de autenticação e podem levar à utilização excessiva da CPU do LSASs. Não há nenhum método na caixa para bloquear a adição de um SPN ou UPN duplicado. *  
  
Valores UPN duplicados interrompem a sincronização entre o AD local e o Office 365.  
  
* Setspn.exe normalmente é usado para criar novos SPNs e funcionalmente incorporado à versão lançada com o Windows Server 2008 que adiciona uma verificação de duplicatas.  
  
**Tabela SEQ tabela \\ \* árabe 1: exclusividade de UPN e SPN**  
  
|Recurso|Comentário|  
|-----------|-----------|  
|Exclusividade do UPN|Os UPNs duplicados violam a sincronização de contas do AD locais com serviços baseados no Windows Azure AD, como o Office 365.|  
|Exclusividade de SPN|O Kerberos requer SPNs para autenticação mútua.  Os SPNs duplicados resultam em falhas de autenticação.|  
  
Para obter mais informações sobre requisitos de exclusividade para UPNs e SPNs, consulte [restrições de exclusividade](/openspecs/windows_protocols/ms-adts/3c154285-454c-4353-9a99-fb586e806944).  
  
## <a name="symptoms"></a>Sintomas  
Os códigos de erro 8467 ou 8468 ou seus equivalentes hexadecimais, simbólicos ou de cadeia de caracteres são registrados em vários diálogos na tela e na ID de evento 2974 no log de eventos de serviços de diretório. A tentativa de criar um UPN ou SPN duplicado só é bloqueada nas seguintes circunstâncias:  
  
-   A gravação é processada por um controlador de domínio do Windows Server 2012 R2  
  
**Tabela SEQ tabela \\ \* árabe 2: códigos de erro de exclusividade de UPN e SPN**  
  
|Decimal|Hex|Simbólico|Cadeia de caracteres|  
|-----------|-------|------------|----------|  
|8467|21C7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|A operação falhou porque o valor de SPN fornecido para adição/modificação não é exclusivo em toda a floresta.|  
|8648|21C8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|A operação falhou porque o valor UPN fornecido para adição/modificação não é exclusivo em toda a floresta.|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>A nova criação de usuário falhará se o UPN não for exclusivo  
  
### <a name="dsamsc"></a>DSA. msc  
O nome de logon do usuário que você escolheu já está em uso nesta empresa. Escolha outro nome de logon e tente novamente.  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
Modificar uma conta existente:  
  
O nome de logon de usuário especificado já existe na empresa. Especifique um novo, seja alterando o prefixo ou selecionando um sufixo diferente na lista.  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Centro Administrativo do Active Directory (DSAC.exe)  
Uma tentativa de criar um novo usuário no Centro Administrativo do Active Directory com um UPN que já existe resultará no seguinte erro.  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**Figura SEQ figura \\ \* árabe 1 erro exibido no centro administrativo do AD quando uma nova criação de usuário falhar devido a um UPN duplicado**  
  
### <a name="event-2974-source-activedirectory_domainservice"></a>Origem do evento 2974: ActiveDirectory_DomainService  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**Figura SEQ figura \\ \* árabe 2 ID do evento 2974 com o erro 8648**  
  
O evento 2974 lista o valor que foi bloqueado e uma lista de um ou mais objetos (até 10) que já contêm esse valor.  Na figura a seguir, você pode ver que o valor do atributo UPN **<em>dhunt@blue.contoso.com</em>** já existe em quatro outros objetos.  Como esse é um novo recurso do Windows Server 2012 R2, a criação acidental de UPN e SPNs duplicados em um ambiente misto ainda ocorrerá quando os DCs de nível inferior processarem a tentativa de gravação.  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**Figura SEQ figura \\ \* árabe 3 evento 2974 mostrando todos os objetos que contêm o UPN duplicado**  
  
> [!TIP]  
> Revise a ID do evento 2974s regularmente para:  
>   
> -   identificar tentativas de criação de UPN ou SPNs duplicados  
> -   identificar objetos que já contêm duplicatas  
  
8648 = "a operação falhou porque o valor UPN fornecido para adição/modificação não é exclusivo em toda a floresta."  
  
### <a name="setspn"></a>SetSPN  
Setspn.exe teve uma detecção de SPN duplicada interna a ela desde a versão 2008 do Windows Server ao usar a opção **"-S"** .  Você pode ignorar a detecção de SPN duplicado usando a opção **"-a"** no entanto.  A criação de um SPN duplicado é bloqueada ao direcionar um controlador de domínio do Windows Server 2012 R2 usando SetSPN com a opção-A.  A mensagem de erro exibida é a mesma exibida ao usar a opção-S: "SPN duplicado encontrado, anulando a operação!"  
  
### <a name="adsiedit"></a>ADSIEdit  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**Figura SEQ figura \\ \* Arabic 4 mensagem de erro exibida em ADSIEdit quando a adição de UPN duplicado está bloqueada**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2:  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
PS em execução no servidor 2012 destinado a um controlador de domínio do Windows Server 2012 R2:  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC.exe em execução no Windows Server 2012 visando um controlador de domínio do Windows Server 2012 R2:  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**Figura SEQ figura \\ \* árabe 5 DSAC erro de criação de usuário em não windows Server 2012 R2 ao direcionar o windows Server 2012 R2 DC**  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**Figura SEQ figura \\ \* árabe 6 DSAC erro de modificação do usuário em não Windows Server 2012 R2 ao direcionar o windows Server 2012 R2 DC**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>A restauração de um objeto que resultaria em falha em um UPN duplicado:  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
Nenhum evento é registrado quando um objeto não consegue ser restaurado devido a um UPN/SPN duplicado.  
  
O UPN do objeto deve ser exclusivo para que seja restaurado.  
  
1.  Identificar o UPN existente no objeto na lixeira  
  
2.  Identificar todos os objetos que têm o mesmo valor  
  
3.  Remover os UPN (s) duplicados  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>Identificar o UPN conflitante na repadmin.exe de Objectusando excluído  
  
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
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>Para identificar todos os objetos com o mesmo UPN: usando Repadmin.exe  
  
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
> O parâmetro **/delete** não documentado anteriormente no repadmin.exe é usado para incluir objetos excluídos no conjunto de resultados  
  
### <a name="using-global-search"></a>Usando a pesquisa global  
  
-   Abrir Centro Administrativo do Active Directory e navegar até a **pesquisa global**  
  
-   Selecione o botão de opção **converter para LDAP**  
  
-   Tipo **(userPrincipalName =*ConflictingUPN*)**  
  
    -   Substitua ***ConflictingUPN*** pelo UPN real que está em conflito  
  
-   Selecione **aplicar**  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>Usando o Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
Se o objeto precisar ser restaurado, será necessário remover os UPNs duplicados dos outros objetos.  Para apenas um objeto, é simples o suficiente para usar ADSIEdit para remover a duplicata.  Se houver vários objetos com duplicatas, o Windows PowerShell poderá ser a melhor ferramenta para usar.  
  
Para anular o atributo UserPrincipalName usando o Windows PowerShell:  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> O atributo userPrincipalName é um atributo de valor único, portanto, esse procedimento só removerá o UPN duplicado.  
  
### <a name="duplicate-spn"></a>SPN duplicado  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**Figura SEQ Figure a \\ \* mensagem de erro Arabic 8 exibida no ADSIEdit quando a adição de SPN duplicado é bloqueada**  
  
Registrado no log de eventos de serviços de diretório é uma **ACTIVEDIRECTORY_DOMAINSERVICE** ID de evento **2974**.  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![Exclusividade de SPN e UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**Figura SEQ figura \\ \* Arabic 9 erro registrado quando a criação de SPN duplicado está bloqueada**  
  
### <a name="workflow"></a>Fluxo de trabalho  
  
-   **Se DC = = GC**  
  
    -   Nenhuma chamada offbox necessária, a consulta pode ser satisfeita localmente  
  
    -   ***Caso UPN***  
  
        -   Consultar o índice UPN de toda a floresta local para o UPN fornecido (*userPrincipalName; um índice global*)  
  
            -   Se as entradas retornadas = = 0-> gravação continuar  
  
            -   Se as entradas retornarem! = 0-falha na gravação de >  
  
                -   Evento registrado  
  
                -   Também retorna o erro estendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   Consultar o índice SPN de toda a floresta local para obter o SPN fornecido (*um índice global*)  
  
            -   Se as entradas retornadas = = 0-> gravação continuar  
  
            -   Se as entradas retornarem! = 0-falha na gravação de >  
  
                -   Evento registrado  
  
                -   Também retorna o erro estendido:  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **Se DC! = GC**  
  
    -   Offbox chamar de forma **desejável** , mas não crítico, ou seja, essa é uma verificação de exclusividade de melhor esforço  
  
        -   A verificação continuará em relação à DIT local somente se o GC não puder ser localizado  
  
        -   Evento registrado para indicar tal  
  
    -   ***Caso UPN***  
  
        -   Enviar consulta LDAP contra GC mais próximo? consultar o índice UPN de toda a floresta do GC para o UPN fornecido (*userPrincipalName; um índice global*)  
  
            -   Se as entradas retornadas = = 0-> gravação continuar  
  
            -   Se as entradas retornarem! = 0-falha na gravação de >  
  
                -   Evento registrado  
  
                -   Também retorna o erro estendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   Enviar consulta LDAP contra GC mais próximo? consultar índice SPN de toda a floresta do GC para obter o SPN fornecido (*um índice global*)  
  
            -   Se as entradas retornadas = = 0-> gravação continuar  
  
            -   Se as entradas retornarem! = 0-falha na gravação de >  
  
                -   Evento registrado  
  
                -   Também retorna o erro estendido:  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
Quando objetos excluídos são reanimados, os valores SPN ou UPN presentes são verificados quanto à exclusividade. Se uma duplicata for encontrada, a solicitação falhará.  
  
-   Para determinadas alterações de atributo, como nome do host DNS, nome da conta SAM, etc., quando a modificação é feita, os SPNs são atualizados de acordo. No processo, os SPNs obsoletos são excluídos e novos SPNs são construídos e adicionados ao banco de dados. As modificações de atributo necessárias nas quais esse caminho é disparado são:  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
Se qualquer um dos novos valores de SPN for uma duplicata, a modificação falhará. Da lista acima, os atributos importantes são ATT_DNS_HOST_NAME (nome da máquina) e ATT_SAM_ACCOUNT_NAME (nome da conta SAM).  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>Experimente: explorando a exclusividade do SPN e do UPN  
Esta é a primeira de várias atividades "**Experimente isso**" no módulo.  Não há um guia de laboratório separado para este módulo.  As atividades de **teste** são essencialmente atividades de forma livre que permitem explorar o material da lição no ambiente de laboratório.  Você tem a opção de seguir o prompt ou sair do script e surgir com sua própria atividade.  
  
> [!NOTE]  
> -   Esta é a primeira de várias atividades "**Experimente isso**".  
> -   Não há um guia de laboratório separado para este módulo.  
> -   As atividades de **teste** são essencialmente atividades de forma livre que permitem explorar o material da lição no ambiente de laboratório.  
> -   Você tem a opção de seguir o prompt ou sair do script e surgir com sua própria atividade.  
> -   Embora nem todas as seções **experimentem esse** prompt, você ainda é incentivado a explorar o conteúdo da lição no laboratório, quando apropriado.  
  
Experimente a exclusividade de SPN e UPN.  Siga estes prompts ou conclua o seu próprio.  
  
1.  Criar novos usuários com o UPN  
  
2.  Criar contas com SPNs  
  
3.  Crie um novo usuário com um UPN já definido anteriormente ou altere o UPN de uma conta existente.  Faça o mesmo para um SPN em outra conta  
  
    1.  Popular uma conta de usuário existente com um UPN já em uso  
  
        1.  Usando o PowerShell, o ADSIEDIT ou o Centro Administrativo do Active Directory (DSAC.exe)  
  
    2.  Popular uma conta existente com um SPN já em uso  
  
        1.  Usando o Windows PowerShell, o ADSIEDIT ou o SetSPN  
  
4.  Observe os erros  
  
**Opcionalmente**  
  
1.  Verifique com o instrutor da sala de aula que está OK para habilitar a *[Lixeira do AD](../../get-started/adac/advanced-ad-ds-management-using-active-directory-administrative-center--level-200-.md#BKMK_EnableRecycleBin)* em centro administrativo do Active Directory.  Nesse caso, vá para a próxima etapa.  
  
2.  Popular o UPN em uma conta de usuário  
  
3.  Excluir a conta  
  
4.  Preencher uma conta diferente com o mesmo UPN que a conta excluída  
  
5.  Tentar usar a GUI da lixeira para restaurar a conta  
  
6.  Imagine que você acabou de receber o erro exibido na etapa anterior.  (e não têm um histórico das etapas que você acabou de executar) Seu objetivo é concluir a restauração da conta.  Consulte a pasta de trabalho para obter as etapas de exemplo.  
  
