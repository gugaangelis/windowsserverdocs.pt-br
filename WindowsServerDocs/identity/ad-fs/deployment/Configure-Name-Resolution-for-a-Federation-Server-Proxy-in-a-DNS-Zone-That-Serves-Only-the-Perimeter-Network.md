---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: Configurar resolução de nomes para um servidor de proxy em uma zona DNS que atende apenas a rede de perímetro
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b8e3cc133ce95872881115608bb8cfb17b2427
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816007"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurar resolução de nomes para um servidor de proxy em uma zona DNS que atende apenas a rede de perímetro

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para que a resolução de nomes pode funcionar com êxito para um servidor de Federação em um Active Directory Federation Services \(do AD FS\) cenário no qual sistema de nomes de domínio de um ou mais \(DNS\) zonas servem apenas o perímetro rede, as seguintes tarefas devem ser concluídas:  
  
-   O arquivo de hosts no proxy de servidor de federação deve ser atualizado para adicionar o endereço IP de um servidor de Federação.  
  
-   DNS na rede de perímetro deve ser configurado para resolver o que nome para o proxy do servidor de Federação do host de todas as solicitações de cliente para o AD FS. Para fazer isso, você pode adicionar um host \(um\) registro de recurso DNS de perímetro para o proxy do servidor de Federação.  
  
> [!NOTE]  
> Estes procedimentos pressupõem que um host \(um\) registro de recurso para o servidor de Federação já foi criado no escritório corporativo de rede DNS. Se este registro ainda não existir, crie esse registro e, em seguida, executar esses procedimentos. Para obter mais informações sobre como criar o host \(um\) registro de recurso para o servidor de federação, consulte [adicionar um Host &#40;A&#41; registro de recurso ao DNS corporativo para um servidor de Federação](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Adicione o endereço IP de um servidor de federação para o arquivo de hosts  
Para que um proxy do servidor de Federação possa funcionar conforme o esperado na rede de perímetro do parceiro de conta, você deve adicionar uma entrada ao arquivo hosts em que proxy do servidor de federação que aponta para o nome do host DNS do servidor de Federação \(por exemplo, fs.fabrikam.com \) e o endereço IP \(por exemplo, 192.168.1.4\) na rede corporativa do parceiro de conta. A adição dessa entrada ao arquivo hosts impede que o proxy do servidor de federação entre em contato com a própria para resolver um cliente\-iniciou a chamada para um servidor de federação no parceiro de conta.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Para adicionar o endereço IP de um servidor de federação para o arquivo de hosts  
  
1.  Navegue até % systemroot %\\Winnt\\System32\\pasta de diretório de Drivers e localize o **hosts** arquivo.  
  
2.  Inicie o Bloco de Notas e abra o arquivo de **hosts**.  
  
3.  Adicione o endereço IP e o nome do host de um servidor de federação no parceiro de conta para o **hosts** de arquivos, conforme mostrado no exemplo a seguir:  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  Salve e feche o arquivo.  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Adicionar um host \(um\) registro DNS de perímetro para um proxy do servidor de federação de recurso  
Para que os clientes na Internet podem acessar um servidor de federação com êxito por meio de um proxy do servidor de Federação recém-implantados, você deve primeiro criar um host \(um\) registro de recurso DNS de perímetro. Este registro de recurso resolve o nome do host do servidor de federação de conta \(por exemplo, fs.fabrikam.com\) para o endereço IP do proxy de servidor de federação de conta \(por exemplo, 131.107.27.68\) no rede de perímetro.  
  
> [!NOTE]  
> Supõe-se que você está usando um servidor DNS, executando o Windows 2000 Server, Windows Server 2003 ou Windows Server 2008 com o serviço do servidor DNS, para controlar a zona DNS de perímetro.  
  
Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Para adicionar um host \(um\) registro DNS de perímetro para um proxy do servidor de federação de recurso  
  
1.  Em um servidor DNS para a rede de perímetro, abra o snap DNS\-no. Clique em **inicie**, aponte para **ferramentas administrativas**e, em seguida, clique em **DNS**.  
  
2.  Na árvore de console, com o botão direito\-clique na zona de pesquisa direta aplicável e, em seguida, clique em **novo Host \(A ou AAAA\)**.  
  
3.  Na **nome**, digite apenas o nome do computador do servidor de Federação. Por exemplo, para o nome de domínio totalmente qualificado \(FQDN\) fs.fabrikam.com, digite **fs**.  
  
4.  Na **endereço IP**, digite o endereço IP do novo proxy de servidor de federação, por exemplo, **131.107.27.68**.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um Proxy do servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolução de nome para Proxies de servidor de Federação](https://technet.microsoft.com/library/dd807055.aspx)  
  

