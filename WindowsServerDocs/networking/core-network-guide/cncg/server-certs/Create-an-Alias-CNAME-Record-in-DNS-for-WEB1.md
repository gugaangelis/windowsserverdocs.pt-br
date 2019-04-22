---
title: Criar um registro de alias (CNAME) em DNS para WEB1
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 442ee8b70311eaad3f0b3f263003786b6beab8bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813157"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Criar um Alias \(CNAME\) registros no DNS para o WEB1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para adicionar um nome de Alias canônico \(CNAME\) registro de recurso para o servidor Web para uma zona no DNS em seu controlador de domínio. Com registros CNAME, você pode usar mais de um nome para apontar para um único host, tornando mais fácil fazer coisas como ambos os hosts File Transfer Protocol \(FTP\) server e um servidor Web no mesmo computador.   
  
Por isso, você é livre para usar seu servidor Web para hospedar a lista de certificados revogados \(CRL\) autoridade de certificação \(autoridade de certificação\) , bem como para executar serviços adicionais, como o servidor FTP ou Web.  
  
Quando você executar esse procedimento, substitua **nome do Alias** e outras variáveis com valores apropriados para sua implantação.  
  
Para executar este procedimento, você deve ser um membro da **Admins. do domínio**.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Para adicionar um alias \(CNAME\) registro de recurso a uma zona  
  
>[!NOTE]  
>Para executar esse procedimento usando o Windows PowerShell, consulte [DnsServerResourceRecordCName adicionar](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  No DC1, no Gerenciador do servidor, clique em **ferramentas** e, em seguida, clique em **DNS**. O Microsoft Management Console (MMC) do DNS Manager é aberto.  
  
2.  Na árvore de console, clique duas vezes **zonas de pesquisa direta**, a zona de pesquisa direta onde deseja adicionar o registro de recurso de Alias e, em seguida, clique com o botão direito **novo Alias \(CNAME\)** . O **novo registro de recurso** caixa de diálogo é aberta.  
  
3.  Na **nome do Alias**, digite o nome do alias **pki**.  
  
4.  Quando você digita um valor para **nome do Alias**, o **nome de domínio totalmente qualificado \(FQDN\)**  preenche automaticamente na caixa de diálogo. Por exemplo, se o nome do alias é "pki" e seu domínio for corp.contoso.com, o valor **pki.corp.contoso.com** é preenchida automaticamente para você.  
  
5.  Na **nome de domínio totalmente qualificado \(FQDN\) para o host de destino**, digite o FQDN do seu servidor Web. Por exemplo, se seu servidor Web for denominado WEB1 e seu domínio for corp.contoso.com, digite **WEB1.corp.contoso.com**.  
  
6.  Clique em **Okey** para adicionar o novo registro à zona.  
  

