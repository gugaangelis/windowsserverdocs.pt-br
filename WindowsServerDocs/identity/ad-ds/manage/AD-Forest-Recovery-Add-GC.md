---
title: "Recuperação de floresta do AD - adicionando o GC"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 3749fd99737f61c55871f613b9feaa21090d3bae
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Recuperação de floresta do AD - adicionando o GC 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Use o procedimento a seguir para adicionar o catálogo global para um controlador de domínio.  
  
## <a name="to-add-the-global-catalog"></a>Para adicionar o catálogo global  
  
1.  Clique em **iniciar**, aponte para **todos os programas**, aponte para **ferramentas administrativas**e clique em **serviços e Sites do Active Directory**.  
2.  Na árvore de console, expanda o **Sites** contêiner e selecione o site apropriado que contém o servidor de destino.  
3.  Expanda o **servidores** contêiner e, em seguida, expanda o objeto de servidor para o controlador de domínio ao qual você deseja adicionar o catálogo global.  
4.  Clique com botão direito **configurações NTDS**e clique em **propriedades**.  
5.  Selecione o **Catálogo Global** caixa de seleção.  
![Adicionar GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)
  
## <a name="to-add-the-global-catalog-using-repadmin"></a>Para adicionar o catálogo global usando Repadmin  
  
1.  Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  
  
    ```  
    repadmin.exe /options DC_NAME +IS_GC  
    ```  
  
 A seguir é maneiras para acelerar o processo de adicionar o catálogo global para o controlador de domínio no domínio raiz:  
  
-   Idealmente, o controlador de domínio no domínio raiz deve ser um parceiro de replicação de controladores de domínio restaurados nos domínios não raiz. Em caso afirmativo, confirme que o verificador de consistência de Conhecimento (KCC) criou as correspondentes **repsFrom** objeto para a origem DC e partição no controlador de domínio raiz. Você pode confirmar isso executando o **repadmin /showreps /v** comando.  
  
-   Se não houver nenhum **repsFrom** objeto criado, criar esse objeto para a partição de configuração. Dessa forma, o controlador de domínio no domínio raiz pode determinar quais controladores de domínio no domínio raiz não foram excluídas. Você pode fazer isso com os seguintes comandos:  
  
    ```  
    repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
    ```  
  
    ```  
    repadmin /options DSA -Disable_NTDSCONN_XLATE  
    ```  
  
     O formato para o *SourceDomainControllerCNAME* é:  
  
    ```  
  
    sourceDCGuid._msdcs.root domain  
    ```  
  
     Por exemplo, o comando de /add repadmin para a partição de configuração do domínio contoso.com poderia ser:  
  
    ```  
    repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
    ```  
  
-   Se o **repsFrom** objeto está presente, tente sincronizar o controlador de domínio no domínio raiz com o controlador de domínio no domínio raiz não da seguinte maneira:  
  
    ```  
    Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
    ```  
  
     Onde *DestinationDomainController* é o controlador de domínio no domínio raiz e *SourceDomainController* é restaurado controlador de domínio no domínio raiz não.  
  
-   O servidor DNS do domínio raiz deve ter os registros de recurso de alias (CNAME) para o controlador de domínio de origem. Certifique-se de que a zona DNS pai contém registros de recurso de delegação (servidor de nomes (NS) e registros de recurso de host (A)) para os controladores de domínio corretos (controladores de domínio que foram restaurados a partir do backup) na zona da criança.  
  
-   Certifique-se de que o controlador de domínio no domínio raiz está se conectando a correta distribuição centro chaves (KDC) no domínio raiz não. Para testar isso, no prompt de comando, digite o seguinte comando e pressione ENTER:  
  
    ```  
    nltest /dsgetdc:nonroot domain name /KDC /Force  
    ```  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
