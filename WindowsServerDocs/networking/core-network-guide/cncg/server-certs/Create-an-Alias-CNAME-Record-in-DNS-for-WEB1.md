---
title: Criar um registro de alias (CNAME) em DNS para WEB1
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9a966ab2883e22173ecf3e64e87d2a4b7a9c57d2
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995588"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Criar um \( registro CNAME \) de alias no DNS para WEB1

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para adicionar um \( registro de recurso CNAME \) de nome canônico de alias para seu servidor Web a uma zona no DNS em seu controlador de domínio. Com os registros CNAME, você pode usar mais de um nome para apontar para um único host, facilitando a realização de coisas como hospedar um \( servidor FTP protocolo FTP \) e um servidor Web no mesmo computador.

Por isso, você é livre para usar o servidor Web para hospedar a CRL da lista de certificados revogados \( \) para sua autoridade de certificação \( CA \) , bem como para executar serviços adicionais, como FTP ou servidor Web.

Ao executar esse procedimento, substitua o **nome do alias** e outras variáveis por valores que são apropriados para sua implantação.

Para executar esse procedimento, você deve ser membro de **Admins**. do domínio.

## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Para adicionar um \( registro de \) recurso CNAME de alias a uma zona

>[!NOTE]
>Para executar esse procedimento usando o Windows PowerShell, consulte [Add-DnsServerResourceRecordCName](/powershell/module/dnsserver/add-dnsserverresourcerecordcname?view=winserver2012r2-ps).

1.  No DC1, no Gerenciador do Servidor, clique em **ferramentas** e, em seguida, clique em **DNS**. O MMC (console de gerenciamento Microsoft) do Gerenciador de DNS é aberto.

2.  Na árvore de console, clique duas vezes em **zonas de pesquisa direta**, clique com o botão direito do mouse na zona de pesquisa direta na qual você deseja adicionar o registro de recurso de alias e clique em **novo alias \( CNAME \) **. A caixa de diálogo **novo registro de recurso** é aberta.

3.  Em **nome do alias**, digite o nome do alias **PKI**.

4.  Quando você digita um valor para **nome de alias**, o ** \( FQDN \) automático do nome de domínio totalmente qualificado** é preenchido automaticamente na caixa de diálogo. Por exemplo, se o nome do alias for "PKI" e seu domínio for corp.contoso.com, o valor **PKI.Corp.contoso.com** será preenchido automaticamente para você.

5.  Em **FQDN do nome de domínio totalmente qualificado \( para o host de \) destino**, digite o FQDN do seu servidor Web. Por exemplo, se o seu servidor Web for denominado WEB1 e seu domínio for corp.contoso.com, digite **WEB1.Corp.contoso.com**.

6.  Clique em **OK** para adicionar o novo registro à zona.