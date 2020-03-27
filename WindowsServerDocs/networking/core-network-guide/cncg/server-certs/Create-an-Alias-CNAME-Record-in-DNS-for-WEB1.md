---
title: Criar um registro de alias (CNAME) em DNS para WEB1
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0ac6ef7da3c9e604d9fa3c029f1afa2861a56741
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318334"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Criar um alias \(registro de\) CNAME no DNS para WEB1

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para adicionar um nome canônico de alias \(registro de recurso de\) CNAME para o servidor Web a uma zona no DNS em seu controlador de domínio. Com os registros CNAME, você pode usar mais de um nome para apontar para um único host, facilitando a realização de coisas como hospedar um protocolo FTP \(servidor FTP\) e um servidor Web no mesmo computador.   
  
Por isso, você é livre para usar o servidor Web para hospedar a lista de certificados revogados \(\) da CRL para sua autoridade de certificação \(\) da AC, bem como para executar serviços adicionais, como FTP ou servidor Web.  
  
Ao executar esse procedimento, substitua o **nome do alias** e outras variáveis por valores que são apropriados para sua implantação.  
  
Para executar esse procedimento, você deve ser membro de **Admins**. do domínio.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Para adicionar um alias \(registro de recurso CNAME\) a uma zona  
  
>[!NOTE]  
>Para executar esse procedimento usando o Windows PowerShell, consulte [Add-DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  No DC1, no Gerenciador do Servidor, clique em **ferramentas** e, em seguida, clique em **DNS**. O MMC (console de gerenciamento Microsoft) do Gerenciador de DNS é aberto.  
  
2.  Na árvore de console, clique duas vezes em **zonas de pesquisa direta**, clique com o botão direito do mouse na zona de pesquisa direta na qual você deseja adicionar o registro de recurso de alias e clique em **Novo Alias \(CNAME\)** . A caixa de diálogo **novo registro de recurso** é aberta.  
  
3.  Em **nome do alias**, digite o nome do alias **PKI**.  
  
4.  Quando você digita um valor para **nome de alias**, o **nome de domínio totalmente qualificado \(FQDN\)** preenchimento automático na caixa de diálogo. Por exemplo, se o nome do alias for "PKI" e seu domínio for corp.contoso.com, o valor **PKI.Corp.contoso.com** será preenchido automaticamente para você.  
  
5.  Em **nome de domínio totalmente qualificado \(\) FQDN para host de destino**, digite o FQDN do seu servidor Web. Por exemplo, se o seu servidor Web for denominado WEB1 e seu domínio for corp.contoso.com, digite **WEB1.Corp.contoso.com**.  
  
6.  Clique em **OK** para adicionar o novo registro à zona.  
  

