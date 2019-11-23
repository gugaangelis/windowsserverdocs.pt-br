---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: Configurar resolução de nomes para um servidor de proxy em uma zona DNS que atende apenas a rede de perímetro
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: de4627f2e03e6432f4e678cd9ca932819cb483d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408431"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurar resolução de nomes para um servidor de proxy em uma zona DNS que atende apenas a rede de perímetro


Para que a resolução de nomes possa funcionar com êxito para um servidor de Federação em um Serviços de Federação do Active Directory (AD FS) \(AD FS cenário de\) em que um ou mais sistemas de nome de domínio \(zonas de\) de DNS servem apenas para a rede de perímetro, as seguintes tarefas devem ser concluídas:  
  
-   O arquivo de hosts no proxy do servidor de Federação deve ser atualizado para adicionar o endereço IP de um servidor de Federação.  
  
-   O DNS na rede de perímetro deve ser configurado para resolver todas as solicitações de cliente para o nome de host AD FS para o proxy do servidor de Federação. Para fazer isso, você adiciona um host \(um registro de recurso\) ao DNS de perímetro para o proxy do servidor de Federação.  
  
> [!NOTE]  
> Esses procedimentos pressupõem que um host \(um registro de recurso\) para o servidor de Federação já tenha sido criado no DNS da rede corporativa. Se esse registro ainda não existir, crie esse registro e execute esses procedimentos. Para obter mais informações sobre como criar o host \(um registro de recurso\) para o servidor de Federação, consulte [Adicionar &#40;um&#41; host um registro de recurso ao DNS corporativo para um servidor de Federação](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Adicionar o endereço IP de um servidor de Federação ao arquivo de hosts  
Para que um proxy de servidor de federação possa funcionar conforme o esperado na rede de perímetro de um parceiro de conta, você deve adicionar uma entrada ao arquivo de hosts nesse proxy de servidor de Federação que aponte para o nome de host DNS de um servidor de Federação \(por exemplo, fs.fabrikam.com\) e o endereço IP \(por exemplo, 192.168.1.4\) na rede corporativa do parceiro de conta. Adicionar essa entrada ao arquivo de hosts impede que o proxy do servidor de federação entre em contato para resolver um cliente\-chamada iniciada para um servidor de Federação no parceiro de conta.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Para adicionar o endereço IP de um servidor de Federação ao arquivo de hosts  
  
1.  Navegue até a pasta de diretórios% SystemRoot%\\WinNT\\system32\\drivers e localize o arquivo de **hosts** .  
  
2.  Inicie o Bloco de Notas e abra o arquivo de **hosts**.  
  
3.  Adicione o endereço IP e o nome de host de um servidor de Federação no parceiro de conta para o arquivo de **hosts** , conforme mostrado no exemplo a seguir:  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  Salve e feche o arquivo.  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Adicionar um host \(um registro de recurso\) ao DNS de perímetro para um proxy de servidor de Federação  
Para que os clientes na Internet possam acessar com êxito um servidor de Federação por meio de um proxy de servidor de Federação implantado recentemente, você deve primeiro criar um host \(um registro de recurso de\) no DNS de perímetro. Esse registro de recurso resolve o nome do host do servidor de Federação da conta \(por exemplo, fs.fabrikam.com\) para o endereço IP do proxy do servidor de Federação da conta \(por exemplo, 131.107.27.68\) na rede de perímetro.  
  
> [!NOTE]  
> Supõe-se que você esteja usando um servidor DNS, executando o Windows 2000 Server, o Windows Server 2003 ou o Windows Server 2008 com o serviço do servidor DNS, para controlar a zona DNS do perímetro.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Para adicionar um host \(um registro de recurso\) ao DNS de perímetro para um proxy de servidor de Federação  
  
1.  Em um servidor DNS para a rede de perímetro, abra o snap-in DNS\-em. Clique em **Iniciar**, aponte para **Ferramentas administrativas**e clique em **DNS**.  
  
2.  Na árvore de console, clique com o\-botão direito do mouse na zona de pesquisa direta aplicável e clique em **novo Host \(A ou AAAA\)** .  
  
3.  Em **nome**, digite apenas o nome do computador do servidor de Federação. Por exemplo, para o nome de domínio totalmente qualificado \(FQDN\) fs.fabrikam.com, digite **FS**.  
  
4.  Em **endereço IP**, digite o endereço IP para o novo proxy de servidor de Federação, por exemplo, **131.107.27.68**.  
  
5.  Clique em **Adicionar Host**.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolução de nome para proxies de servidor de federação](https://technet.microsoft.com/library/dd807055.aspx)  
  

