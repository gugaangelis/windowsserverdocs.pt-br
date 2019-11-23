---
title: Recuperação de floresta do AD-adicionando o GC
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: f82033dd042847c7c735423c25756b936b137230
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369345"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Recuperação de floresta do AD-adicionando o GC

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para adicionar o catálogo global a um controlador de domínio.  
  
## <a name="to-add-the-global-catalog"></a>Para adicionar o catálogo global  
  
1. Clique em **Iniciar**, aponte para **todos os programas**, aponte para **ferramentas administrativas**e clique em **Active Directory sites e serviços**.  
2. Na árvore de console, expanda o contêiner **sites** e selecione o site apropriado que contém o servidor de destino.  
3. Expanda o contêiner **servidores** e expanda o objeto de servidor para o DC ao qual você deseja adicionar o catálogo global.  
4. Clique com o botão direito do mouse em **Configurações NTDS**e clique em **Propriedades**.  
5. Marque a caixa de seleção **catálogo global** .  
![adicionar GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)

## <a name="to-add-the-global-catalog-using-repadmin"></a>Para adicionar o catálogo global usando repadmin  

- Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  

   ```  
   repadmin.exe /options DC_NAME +IS_GC  
   ```  

Veja a seguir maneiras de acelerar o processo de adicionar o catálogo global ao DC no domínio raiz:  

- O ideal é que o DC no domínio raiz seja um parceiro de replicação dos DCs restaurados nos domínios não raiz. Nesse caso, confirme se o Knowledge Consistency Checker (KCC) criou o objeto **repsFrom** correspondente para o DC de origem e a partição no controlador de domínio raiz. Você pode confirmar isso executando o comando **repadmin/showreps/v** . 

- Se não houver nenhum objeto **repsFrom** criado, crie esse objeto para a partição de configuração. Dessa forma, o DC no domínio raiz pode determinar quais DCs no domínio não raiz foram excluídos. Você pode fazer isso com os seguintes comandos:  

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE  
   ```

   O formato para o *SourceDomainControllerCNAME* é:  

   ```
  
   sourceDCGuid._msdcs.root domain  
   ```

   Por exemplo, o comando repadmin/Add para a partição de configuração do domínio contoso.com pode ser:  

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
   ```

- Se o objeto **repsFrom** estiver presente, tente sincronizar o DC no domínio raiz com o DC no domínio não raiz da seguinte maneira:  

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
   ```

   Em que *DestinationDomainController* é o DC no domínio raiz e *SOURCEDOMAINCONTROLLER* é o DC restaurado no domínio não raiz. 

- O servidor DNS do domínio raiz deve ter os registros de recurso de alias (CNAME) para o DC de origem. Verifique se a zona DNS pai contém registros de recurso de delegação (servidor de nomes (NS) e registros de recurso de host (A)) para os DCs corretos (os DCs que foram restaurados do backup) na zona filho. 
- Verifique se o DC no domínio raiz está entrando em contato com o KDC (centro de distribuição de chaves correto) no domínio não raiz. Para testar isso, no prompt de comando, digite o seguinte comando e pressione ENTER:  

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force  
   ```

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)  
