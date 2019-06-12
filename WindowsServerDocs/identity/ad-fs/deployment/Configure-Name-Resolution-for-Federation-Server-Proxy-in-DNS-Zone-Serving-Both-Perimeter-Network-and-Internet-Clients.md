---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: Configurar resolução de nomes para um proxy do servidor de federação em uma zona DNS que atende à rede de perímetro e aos clientes da Internet
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: cef87e725db7068ac4ed93524e09a25de95ec276
ms.sourcegitcommit: 9a4ab3a0d00b06ff16173aed616624c857589459
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828510"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>Configurar resolução de nomes para um proxy do servidor de federação em uma zona DNS que atende à rede de perímetro e aos clientes da Internet


Para que a resolução de nomes pode funcionar com êxito para um proxy do servidor de Federação em um Active Directory Federation Services \(do AD FS\) cenário no qual sistema de nomes de domínio de um ou mais \(DNS\) zonas servem tanto o rede de perímetro e os clientes da Internet, as seguintes tarefas devem ser concluídas:  
  
-   DNS na zona da Internet que você controle deve ser configurado para resolver o que nome para o proxy do servidor de Federação do host de todas as solicitações de cliente de Internet para o AD FS. Para fazer isso, você pode adicionar um host \(um\) registro de recurso para a zona de DNS da Internet para o proxy do servidor de Federação.  
  
-   DNS na rede de perímetro deve ser configurado para resolver o que nome para o servidor de Federação do host de todas as solicitações do cliente para o AD FS. Para fazer isso, você pode adicionar um host \(um\) registro de recurso para a zona DNS de perímetro para o proxy do servidor de Federação.  
  
> [!NOTE]  
> Supõe-se que um host \(um\) registro de recurso para o servidor de Federação já foi criado no escritório corporativo de rede DNS. Se este registro ainda não existir, crie esse registro e, em seguida, executar esses procedimentos. Para obter mais informações sobre como criar um host \(um\) registro de recurso para o servidor de federação, consulte [adicionar um Host &#40;A&#41; registro de recurso ao DNS corporativo para um servidor de Federação](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Adicionar um host \(um\) registro de recurso para a zona DNS da Internet para um proxy do servidor de Federação  
Para que os computadores cliente na Internet podem acessar um servidor de federação com êxito por meio de um proxy do servidor de Federação recém-implantados, você deve primeiro criar um host \(um\) registro de recurso na zona DNS da Internet que você controla. Este registro de recurso resolve o nome do host do servidor de federação de conta \(por exemplo, fs.fabrikam.com\) para o endereço IP do proxy de servidor de federação de conta \(por exemplo, 131.107.27.68\) no rede de perímetro.  
  
> [!NOTE]  
> Supõe-se que você está usando um servidor DNS executando o Windows 2000 Server, Windows Server 2003 ou Windows Server 2008 com o serviço servidor DNS para a zona DNS de Internet de controlar.  
  
Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Para adicionar um host \(um\) registro de recurso para a zona DNS da Internet para um proxy do servidor de Federação  
  
1.  Em um servidor DNS para a zona DNS da Internet, abra o snap DNS\-no.  
  
2.  Na árvore de console, com o botão direito\-clique na zona de pesquisa direta aplicável e, em seguida, clique em **novo Host \(A ou AAAA\)** .  
  
3.  Na **nome**, digite apenas o nome do computador do servidor de Federação. Por exemplo, para o nome de domínio totalmente qualificado \(FQDN\) fs.fabrikam.com, digite **fs**.  
  
4.  Na **endereço IP**, digite o endereço IP do novo proxy de servidor de federação, por exemplo, 131.107.27.68.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Adicionar um host \(um\) registro de recurso para a zona DNS de perímetro para um proxy do servidor de Federação  
Para que as solicitações de cliente da Internet podem ser processadas com êxito pelo proxy de servidor de Federação e acessar o servidor de Federação depois que elas são resolvidas pela zona de DNS da Internet, você deve criar um host \(um\) registro de recurso no perímetro Zona DNS. Este registro de recurso resolve o nome do host do servidor de federação de conta \(por exemplo, fs. Fabrikam.com\) para o endereço IP do servidor de federação de conta \(por exemplo, 192.168.1.4\) na rede corporativa.  
  
> [!NOTE]  
> Supõe-se que você está usando um servidor DNS executando o Windows 2000 Server, Windows Server 2003, Windows Server 2008 ou Windows Server® 2012 com o serviço servidor DNS para controlar a zona DNS de perímetro.  
  
Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Para adicionar um host \(um\) registro de recurso para a zona DNS de perímetro para um proxy do servidor de Federação  
  
1.  Em um servidor DNS para a rede de perímetro, abra o **DNS snap\-em**.  
  
2.  Na árvore de console, com o botão direito\-clique na zona de pesquisa direta aplicável e, em seguida, clique em **novo Host \(A ou AAAA\)** .  
  
3.  Na **nome**, digite apenas o nome do computador do servidor de Federação. Por exemplo, para o FQDN fs.fabrikam.com, digite **fs**.  
  
4.  No **endereço IP** caixa de texto, digite o IP de endereço para o servidor de federação na rede corporativa, por exemplo, 192.168.1.4.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolução de nome para proxies de servidor de federação](https://technet.microsoft.com/library/dd807055.aspx)  
  

