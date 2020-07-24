---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: Resolução de nomes para clientes de perímetro e de Internet
author: billmath
manager: femila
ms.date: 04/13/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 83257c34b5070f5be637d13aff07f4389fa1f9a1
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964758"
---
# <a name="name-resolution-for-perimeter-and-internet-clients"></a>Resolução de nomes para clientes de perímetro e de Internet


Para que a resolução de nomes possa funcionar com êxito para um proxy de servidor de Federação em um Serviços de Federação do Active Directory (AD FS) \( AD FS \) cenário no qual uma ou mais zonas DNS do sistema de nomes \( de domínio \) atendem tanto à rede de perímetro quanto aos clientes da Internet, as seguintes tarefas devem ser concluídas:  
  
-   O DNS na zona da Internet que você controla deve ser configurado para resolver todas as solicitações de clientes da Internet para o nome de host AD FS para o proxy do servidor de Federação. Para fazer isso, adicione um host \( um \) registro de recurso à zona DNS da Internet para o proxy do servidor de Federação.  
  
-   O DNS na rede de perímetro deve ser configurado para resolver todas as solicitações de entrada do cliente para o nome de host AD FS para o servidor de Federação. Para fazer isso, adicione um host \( um \) registro de recurso à zona DNS de perímetro para o proxy do servidor de Federação.  
  
> [!NOTE]  
> Supõe-se que um host que \( um \) registro de recurso para o servidor de Federação já tenha sido criado no DNS da rede corporativa. Se esse registro ainda não existir, crie esse registro e execute esses procedimentos. Para obter mais informações sobre como criar um registro de recurso de um host \( \) para o servidor de Federação, consulte [adicionar um host &#40;um registro de recurso&#41; ao DNS corporativo para um servidor de Federação](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Adicionar um \( registro de \) recurso do host a uma zona DNS da Internet para um proxy de servidor de Federação  
Para que os computadores cliente na Internet possam acessar com êxito um servidor de Federação por meio de um proxy de servidor de Federação implantado recentemente, você deve primeiro criar um \( registro de recurso de um host \) na zona DNS da Internet que você controla. Esse registro de recurso resolve o nome do host do servidor de Federação da conta \( , por exemplo, FS.fabrikam.com \) para o endereço IP do proxy do servidor de Federação da conta \( , por exemplo, 131.107.27.68 \) na rede de perímetro.  
  
> [!NOTE]  
> Presume-se que você esteja usando um servidor DNS que executa o Windows 2000 Server, o Windows Server 2003 ou o Windows Server 2008 com o serviço do servidor DNS para controlar a zona DNS da Internet.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Para adicionar um host \( um \) registro de recurso à zona DNS da Internet para um proxy de servidor de Federação  
  
1.  Em um servidor DNS para a zona DNS da Internet, abra o snap- \- in DNS.  
  
2.  Na árvore de console, clique com o botão direito \- do mouse na zona de pesquisa direta aplicável e clique em **novo host \( A ou aaaa \) **.  
  
3.  Em **nome**, digite apenas o nome do computador do servidor de Federação. Por exemplo, para o nome de domínio totalmente qualificado \( FQDN \) FS.fabrikam.com, digite **FS**.  
  
4.  Em **endereço IP**, digite o endereço IP para o novo proxy de servidor de Federação, por exemplo, 131.107.27.68.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Adicionar um registro de recurso de host \( \) a uma zona DNS de perímetro para um proxy de servidor de Federação  
Para que as solicitações de cliente da Internet possam ser processadas com êxito pelo proxy do servidor de Federação e cheguem ao servidor de Federação após serem resolvidas pela zona DNS da Internet, você deve criar um \( registro de recurso de host a \) na zona DNS do perímetro. Esse registro de recurso resolve o nome do host do servidor de Federação da conta \( , por exemplo, FS. fabrikam.com \) para o endereço IP do servidor de Federação da conta \( , por exemplo, 192.168.1.4 \) na rede corporativa.  
  
> [!NOTE]  
> Presume-se que você esteja usando um servidor DNS que executa o Windows 2000 Server, o Windows Server 2003, o Windows Server 2008 ou o Windows Server &reg; 2012 com o serviço do servidor DNS para controlar a zona DNS do perímetro.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Para adicionar um host \( um \) registro de recurso à zona DNS de perímetro para um proxy de servidor de Federação  
  
1.  Em um servidor DNS para a rede de perímetro, abra o **snap- \- in DNS**.  
  
2.  Na árvore de console, clique com o botão direito \- do mouse na zona de pesquisa direta aplicável e clique em **novo host \( A ou aaaa \) **.  
  
3.  Em **nome**, digite apenas o nome do computador do servidor de Federação. Por exemplo, para o FQDN fs.fabrikam;com, digite **fs**.  
  
4.  Na caixa de texto **endereço IP** , digite o endereço IP do servidor de Federação na rede corporativa, por exemplo, 192.168.1.4.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolução de nome para proxies de servidor de federação](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807055(v=ws.11))  
  
