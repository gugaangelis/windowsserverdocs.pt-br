---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: "Configurar a resolução de nome de um Proxy de servidor de Federação em um DNS zona que serve as duas o perímetro da rede e clientes da Internet"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3b154459163b2142ff1d3aba424a86305d093de4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>Configurar a resolução de nome de um Proxy de servidor de Federação em um DNS zona que serve as duas o perímetro da rede e clientes da Internet

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para que a resolução de nomes possa funcionar bem para um proxy de servidor de Federação em um cenário de \(AD FS\) de serviços de Federação do Active Directory nos quais uma ou mais zonas Domain Name System \(DNS\) servem clientes de Internet e a rede do perímetro, devem ser concluídas as seguintes tarefas:  
  
-   DNS na zona da Internet que você controla deve ser configurado para resolver o que nome do proxy do servidor de federação de host de todas as solicitações de cliente de Internet do AD FS. Para fazer isso, você adiciona um registro de recurso do host \(A\) na zona DNS de Internet do proxy do servidor de Federação.  
  
-   DNS na rede de perímetro deve ser configurado para resolver o que nome do servidor de federação de host de todas as solicitações de cliente do AD FS. Para fazer isso, você adiciona um registro de recurso do host \(A\) na zona de DNS do perímetro do proxy do servidor de Federação.  
  
> [!NOTE]  
> Presume-se que já tenha sido criado um registro de recurso \(A\) de host para o servidor de federação na rede corporativa DNS. Se esse registro ainda não existir, crie esse registro e, em seguida, executar esses procedimentos. Para obter mais informações sobre como criar um registro de recurso \(A\) de host para o servidor de federação, consulte [adicionar um Host & #40; A & #41; Registro de recurso DNS corporativo para um servidor de Federação](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Adicionar um registro de recurso do host \(A\) a zona da Internet DNS para um proxy de servidor de Federação  
Para que os computadores cliente na Internet com êxito podem acessar um servidor de federação por meio de um proxy de servidor de Federação recém-implantados, crie primeiro um registro de recurso do host \(A\) na zona da Internet DNS que você controla. Esse registro de recurso converte o nome do host do servidor de federação de conta \ (por exemplo, fs.fabrikam.com\) para o endereço IP do proxy do servidor de federação de conta \ (por exemplo, 131.107.27.68\) na rede de perímetro.  
  
> [!NOTE]  
> Presume-se que você estiver usando um servidor DNS executando o Windows Server 2008, Windows Server 2003 ou Windows 2000 Server com o serviço de servidor DNS para controlar a zona DNS de Internet.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](http://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Para adicionar um registro de recurso do host \(A\) a zona da Internet DNS para um proxy de servidor de Federação  
  
1.  Em um servidor DNS para a zona da Internet DNS, abra o DNS snap\-in.  
  
2.  No console de árvore, clique right\ a zona de pesquisa direta aplicável e clique em **novo Host \(A or AAAA\)**.  
  
3.  Em **nome**, digite apenas o nome do computador do servidor de Federação. Por exemplo, para o domínio totalmente qualificado, nome \(FQDN\) fs.fabrikam.com, tipo **fs**.  
  
4.  Em **endereço IP**, digite o endereço IP para o novo proxy de servidor de federação, por exemplo, 131.107.27.68.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Adicionar um registro de recurso do host \(A\) na zona de DNS do perímetro para um proxy de servidor de Federação  
Para que as solicitações de cliente de Internet podem ser processadas com êxito pelo proxy do servidor de Federação e acessar o servidor de Federação depois que eles são resolvidos por zona da Internet DNS, você deve criar um registro de recurso do host \(A\) na zona perímetro DNS. Esse registro de recurso converte o nome do host do servidor de federação de conta \ (por exemplo, fs. fabrikam.com\) para o endereço IP do servidor de federação de conta \ (por exemplo, 192.168.1.4\) na rede corporativa.  
  
> [!NOTE]  
> Presume-se que você estiver usando um servidor DNS executando o Windows 2000 Server, Windows Server 2003, Windows Server 2008 ou Windows Server® 2012 com o serviço de servidor DNS para controlar a zona DNS do perímetro.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](http://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Para adicionar um registro de recurso do host \(A\) na zona de DNS do perímetro para um proxy de servidor de Federação  
  
1.  Em um servidor DNS para a rede do perímetro, abra o **DNS snap\-in**.  
  
2.  No console de árvore, clique right\ a zona de pesquisa direta aplicável e clique em **novo Host \(A or AAAA\)**.  
  
3.  Em **nome**, digite apenas o nome do computador do servidor de Federação. Por exemplo, para o FQDN fs.fabrikam.com, digite **fs**.  
  
4.  No **endereço IP** caixa de texto, digite o IP endereço para o servidor de federação na rede corporativa, por exemplo, 192.168.1.4.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um Proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolução de nome de Proxies de servidor de Federação](https://technet.microsoft.com/library/dd807055.aspx)  
  

