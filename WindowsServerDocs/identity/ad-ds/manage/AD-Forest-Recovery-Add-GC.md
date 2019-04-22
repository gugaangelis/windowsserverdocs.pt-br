---
title: Recuperação de floresta do AD - adicionando o GC
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 156a4a64d9c3bb8261bd603b72ae11b81ff1d152
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825627"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>Recuperação de floresta do AD - adicionando o GC

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para adicionar o catálogo global para um controlador de domínio.  
  
## <a name="to-add-the-global-catalog"></a>Para adicionar o catálogo global  
  
1. Clique em **inicie**, aponte para **todos os programas**, aponte para **ferramentas administrativas**e, em seguida, clique em **serviços e Sites do Active Directory**.  
2. Na árvore de console, expanda o **Sites** contêiner e, em seguida, selecione o site apropriado que contém o servidor de destino.  
3. Expanda o **servidores** contêiner e, em seguida, expanda o objeto de servidor para o controlador de domínio ao qual você deseja adicionar o catálogo global.  
4. Clique com botão direito **configurações de NTDS**e, em seguida, clique em **propriedades**.  
5. Selecione o **Catálogo Global** caixa de seleção.  
![Adicionar o GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)

## <a name="to-add-the-global-catalog-using-repadmin"></a>Para adicionar o catálogo global usando Repadmin  

- Abra um prompt de comando com privilégios elevados, digite o seguinte comando e pressione ENTER:  

   ```  
   repadmin.exe /options DC_NAME +IS_GC  
   ```  

A seguir é maneiras de acelerar o processo de adicionar o catálogo global para o controlador de domínio no domínio raiz:  

- O ideal é que o controlador de domínio no domínio raiz deve ser um parceiro de replicação dos controladores de domínio restaurados nos domínios não raiz. Nesse caso, confirme que o Knowledge Consistency Checker (KCC) criou correspondente **repsFrom** objeto para o DC de origem e a partição na raiz do controlador de domínio. Você pode confirmar isso executando o **repadmin /showreps /v** comando. 

- Se não houver nenhuma **repsFrom** objeto criado, criar esse objeto para a partição de configuração. Dessa forma, o controlador de domínio no domínio raiz pode determinar quais controladores de domínio no domínio raiz de não tem sido excluídos. Você pode fazer isso com os seguintes comandos:  

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

   Por exemplo, o repadmin / adicione o comando para a partição de configuração do domínio contoso.com pode ser:  

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
   ```

- Se o **repsFrom** objeto está presente, tente sincronizar o controlador de domínio no domínio raiz com o controlador de domínio no domínio não raiz da seguinte maneira:  

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
   ```

   Em que *DestinationDomainController* é o controlador de domínio no domínio raiz e *SourceDomainController* é o controlador de domínio restaurado no domínio não raiz. 

- O servidor DNS do domínio raiz deve ter os registros de recurso de alias (CNAME) para o DC de origem. Certifique-se de que a zona DNS pai contém registros de recursos de delegação (servidor de nomes (NS) e registros de recursos de host (A)) para os controladores de domínio corretos (os controladores de domínio que foram restaurados do backup) na zona filho. 
- Certifique-se de que o controlador de domínio no domínio raiz está se conectando a correta KDC Key Distribution Center () no domínio não raiz. Para testar isso, no prompt de comando, digite o seguinte comando e pressione ENTER:  

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force  
   ```

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
