---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: "Configurar a resolução de nome de um Proxy de servidor de Federação em uma zona de DNS que serve apenas a rede do perímetro"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b8e3cc133ce95872881115608bb8cfb17b2427
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurar a resolução de nome de um Proxy de servidor de Federação em uma zona de DNS que serve apenas a rede do perímetro

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para que a resolução de nomes pode funcionar com êxito para um servidor de Federação em um cenário de \(AD FS\) de serviços de Federação do Active Directory nos quais uma ou mais zonas Domain Name System \(DNS\) servem somente a rede do perímetro, devem ser concluídas as seguintes tarefas:  
  
-   O arquivo de hosts do proxy do servidor de federação deve ser atualizado para adicionar o endereço IP de um servidor de Federação.  
  
-   DNS na rede de perímetro deve ser configurado para resolver o que nome do proxy do servidor de federação de host de todas as solicitações de cliente do AD FS. Para fazer isso, você adiciona um registro de recurso do host \(A\) para perímetro DNS do proxy do servidor de Federação.  
  
> [!NOTE]  
> Estes procedimentos pressupõem que já tenha sido criado um registro de recurso \(A\) de host para o servidor de federação na rede corporativa DNS. Se esse registro ainda não existir, crie esse registro e, em seguida, executar esses procedimentos. Para obter mais informações sobre como criar o registro de recurso \(A\) de host para o servidor de federação, consulte [adicionar um Host & #40; A & #41; Registro de recurso DNS corporativo para um servidor de Federação](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Adicione o endereço IP de um servidor de federação para o arquivo de hosts  
Para que um proxy de servidor de Federação pode funcionar como esperado na rede de perímetro de um parceiro de conta, você deve adicionar uma entrada ao arquivo hosts nesse proxy do servidor de federação que aponta para nome de host DNS de um servidor de Federação \ (por exemplo, fs.fabrikam.com\) e o endereço IP \ (por exemplo, 192.168.1.4\) na rede da empresa do parceiro de conta. Adicionar esta entrada para o arquivo de hosts impede que o proxy do servidor de Federação contatando em si para resolver uma chamada iniciada client\ para um servidor de federação no parceiro de conta.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Para adicionar o endereço IP de um servidor de federação para o arquivo de hosts  
  
1.  Navegue até a pasta de diretório %systemroot%\\Winnt\\System32\\Drivers e localize o **hosts** arquivo.  
  
2.  Inicie o Notepad e, em seguida, abra o **hosts** arquivo.  
  
3.  Adicione o endereço IP e o nome do host de um servidor de federação no parceiro de conta para a **hosts** do arquivo, conforme mostrado no exemplo a seguir:  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  Salve e feche o arquivo.  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Adicionar um registro de recurso do host \(A\) ao perímetro DNS para um proxy de servidor de Federação  
Para que os clientes na Internet com êxito podem acessar um servidor de federação por meio de um proxy de servidor de Federação recém-implantados, crie primeiro um registro de recurso do host \(A\) no perímetro DNS. Esse registro de recurso converte o nome do host do servidor de federação de conta \ (por exemplo, fs.fabrikam.com\) para o endereço IP do proxy do servidor de federação de conta \ (por exemplo, 131.107.27.68\) na rede de perímetro.  
  
> [!NOTE]  
> Presume-se que você estiver usando um servidor DNS, executando o Windows Server 2008, Windows Server 2003 ou Windows 2000 Server com o serviço de servidor DNS, para controlar a zona DNS do perímetro.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Para adicionar um registro de recurso do host \(A\) a perímetro DNS para um proxy de servidor de Federação  
  
1.  Em um servidor DNS para a rede do perímetro, abra o DNS snap\-in. Clique em **iniciar**, aponte para **ferramentas administrativas**e clique em **DNS**.  
  
2.  No console de árvore, clique right\ a zona de pesquisa direta aplicável e clique em **novo Host \(A or AAAA\)**.  
  
3.  Em **nome**, digite apenas o nome do computador do servidor de Federação. Por exemplo, para o domínio totalmente qualificado, nome \(FQDN\) fs.fabrikam.com, tipo **fs**.  
  
4.  Em **endereço IP**, digite o endereço IP para o novo proxy de servidor de federação, por exemplo, **131.107.27.68**.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um Proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolução de nome de Proxies de servidor de Federação](https://technet.microsoft.com/library/dd807055.aspx)  
  

