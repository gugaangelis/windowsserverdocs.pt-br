---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: Exportar a parte da chave privada de um Certificado de Autenticação de Servidor
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6baa734e3fc346d94f4387e2ed54d3e707e5af75
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855419"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Exportar a parte da chave privada de um Certificado de Autenticação de Servidor

Cada servidor de Federação em um Serviços de Federação do Active Directory (AD FS) \(AD FS farm\) deve ter acesso à chave privada do certificado de autenticação do servidor. Se você estiver implementando um farm de servidores de servidor de Federação ou servidores Web, deverá ter um único certificado de autenticação. Esse certificado deve ser emitido por uma autoridade de certificação corporativa \(\)de AC e deve ter uma chave privada exportável. A chave privada do certificado de autenticação de servidor deve ser exportável para que possa ser disponibilizada para todos os servidores no farm.  
  
Esse mesmo conceito é verdadeiro para farms de proxy de servidor de Federação no sentido de que todos os proxies de servidor de Federação em um farm devem compartilhar a parte de chave privada do mesmo certificado de autenticação de servidor.  
  
> [!NOTE]  
> O\-snap de gerenciamento de AD FS no refere-se aos certificados de autenticação de servidor para servidores de Federação como certificados de comunicação de serviço.  
  
Dependendo da função que este computador executará, use esse procedimento no computador do servidor de Federação ou no computador proxy do servidor de Federação em que você instalou o certificado de autenticação do servidor com a chave privada. Ao terminar este procedimento, você poderá importar o certificado no Site Padrão de cada servidor no farm. Para obter mais informações, consulte [importar um certificado de autenticação de servidor para o site da Web padrão](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Para exportar a parte da chave privada de um certificado de autenticação de servidor  
  
1. Na tela **Iniciar** , digite**serviços de informações da Internet \(o Gerenciador de\) do IIS**e pressione Enter.  
  
2. Na árvore de console, clique em **ComputerName**.  
  
3. No painel central, clique duas vezes\-em **certificados de servidor**.  
  
4. No painel central, clique com o botão direito do\-mouse no certificado que você deseja exportar e clique em **Exportar**.  
  
5. Na caixa de diálogo **Exportar Certificado** , clique no botão **…** .  
  
6. Em **nome do arquivo**, digite **C:\\** <em>NameofCertificate</em>e clique em **abrir**.  
  
7. Digite uma senha para o certificado, confirme e clique em **OK**.  
  
8. Verifique se a exportação foi concluída com êxito e confirme se o arquivo desejado foi criado no local especificado.  
  
   > [!IMPORTANT]  
   > Para que este certificado possa ser importado para o repositório de certificados local no novo servidor, você precisará transferir para mídia física e proteger sua segurança durante a transição para o novo servidor. É extremamente importante proteger a segurança da chave privada. Se essa chave for comprometida, a segurança de toda a implantação de AD FS \(incluindo recursos em sua organização e em organizações de parceiros de recursos\) será comprometida.  
  
9. Importe o certificado de autenticação de servidor exportado para o repositório de certificados do novo servidor antes de instalar o Serviço de Federação. Para obter informações sobre como importar o certificado, consulte importar um certificado do servidor \([http:\/\/go.microsoft.com\/fwlink\/? LinkId\=\)108283](https://go.microsoft.com/fwlink/?LinkId=108283) .  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Configurando um proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de certificado para servidores de federação](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisitos de certificado para proxies do servidor de federação](https://technet.microsoft.com/library/dd807054.aspx)  
  

