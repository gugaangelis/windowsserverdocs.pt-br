---
title: Criar um registro de Alias (CNAME) no DNS para WEB1
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 65f10efe1cfd2866fb99406bf031197f6268b05b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Criar um registro de Alias \(CNAME\) no DNS para WEB1

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para adicionar um registro de recurso Alias nome canônico \(CNAME\) para o servidor Web a uma zona de DNS no controlador de domínio. Com registros CNAME, você pode usar mais de um nome para apontar para um único host, tornando mais fácil fazer coisas como host tanto um servidor FTP \(FTP\) como um servidor Web no mesmo computador.   
  
Por isso, você é livre para usar seu servidor Web para hospedar a revogação de certificados listar \(CRL\) para sua autoridade de certificação \(CA\), bem como para executar serviços adicionais, como o servidor de Web ou FTP.  
  
Quando você executa este procedimento, substitua **apelido** e outras variáveis com valores que são apropriados para a implantação.  
  
Para executar este procedimento, você deve ser um membro do **Admins. do domínio**.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Para adicionar um registro de recurso \(CNAME\) alias para uma zona  
  
>[!NOTE]  
>Para executar este procedimento usando o Windows PowerShell, consulte [adicionar DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  Em DC1, no Gerenciador do servidor, clique em **ferramentas** e, em seguida, clique em **DNS**. O Console de gerenciamento do DNS Manager Microsoft (MMC) é aberta.  
  
2.  Na árvore de console, clique duas vezes em **zonas de pesquisa direta**, clique com botão direito a zona de pesquisa direta onde deseja adicionar o registro de Alias de recurso e, em seguida, clique em **novo Alias \(CNAME\)**. O **novo registro de recurso** caixa de diálogo é aberta.  
  
3.  Em **apelido**, digite o nome do alias **pki**.  
  
4.  Quando você digitar um valor para **apelido**, o **\(FQDN\) de nome de domínio totalmente qualificado** autopreenche a caixa de diálogo. Por exemplo, se o nome do seu alias é "pki" e seu domínio é corp.contoso.com, o valor **pki.corp.contoso.com** é preenchida automaticamente para você.  
  
5.  Em **\(FQDN\) de nome de domínio totalmente qualificado para host de destino**, digite o FQDN do seu servidor Web. Por exemplo, se o seu servidor Web é denominado WEB1 e seu domínio é corp.contoso.com, digite **WEB1.corp.contoso.com**.  
  
6.  Clique em **Okey** para adicionar o novo registro à zona.  
  

