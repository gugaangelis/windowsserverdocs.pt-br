---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: Exportar a parte da chave privada de um Certificado de Autenticação de Servidor
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c3a39f9d51ed8243118522ae37bc7d205a7ea416
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192143"
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Exportar a parte da chave privada de um Certificado de Autenticação de Servidor

Cada servidor de Federação em um Active Directory Federation Services \(do AD FS\) farm deve ter acesso à chave privada do certificado de autenticação do servidor. Se você estiver implementando um farm de servidores de Federação ou servidores Web, você deve ter um certificado de autenticação único. Esse certificado deve ser emitido por uma autoridade de certificação corporativa \(autoridade de certificação\), e deve ter uma chave privada exportável. A chave privada do certificado de autenticação de servidor deve ser exportável para que possa ser disponibilizada para todos os servidores no farm.  
  
Este mesmo conceito é verdadeiro para farms de servidores do proxy de servidor de federação no sentido de que todos os proxies de servidor de Federação em um farm devem compartilhar a parte da chave privada do mesmo certificado de autenticação de servidor.  
  
> [!NOTE]  
> O snap de gerenciamento do AD FS\-refere-se aos certificados de autenticação de servidor para servidores de federação como certificados de comunicação de serviço.  
  
Dependendo de qual função neste computador, use este procedimento no computador do servidor de Federação ou computador de proxy do servidor de Federação em que você instalou o certificado de autenticação de servidor com a chave privada. Ao terminar este procedimento, você poderá importar o certificado no Site Padrão de cada servidor no farm. Para obter mais informações, consulte [importar um certificado de autenticação de servidor para o Site padrão](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Para exportar a parte da chave privada de um certificado de autenticação de servidor  
  
1.  Sobre o **iniciar** tela, digite**serviços de informações da Internet \(IIS\) Manager**, e, em seguida, pressione ENTER.  
  
2.  Na árvore de console, clique em **ComputerName**.  
  
3.  No painel central, clique duas vezes\-clique em **certificados de servidor**.  
  
4.  No painel central, com o botão direito\-clique no certificado que você deseja exportar e, em seguida, clique em **exportar**.  
  
5.  Na caixa de diálogo **Exportar Certificado** , clique no botão **…** .  
  
6.  No **nome do arquivo**, digite **c:\\* * * Nomedocertificado*e, em seguida, clique em **abrir**.  
  
7.  Digite uma senha para o certificado, confirme e clique em **OK**.  
  
8.  Verifique se a exportação foi concluída com êxito e confirme se o arquivo desejado foi criado no local especificado.  
  
    > [!IMPORTANT]  
    > Para que este certificado possa ser importado para o repositório de certificados local no novo servidor, você precisará transferir para mídia física e proteger sua segurança durante a transição para o novo servidor. É extremamente importante proteger a segurança da chave privada. Se essa chave for comprometida, a segurança de toda a implantação do AD FS \(incluindo recursos na sua organização e em organizações parceiras\) estiver comprometido.  
  
9. Importe o certificado de autenticação de servidor exportado para o repositório de certificados do novo servidor antes de instalar o Serviço de Federação. Para obter informações sobre como importar o certificado, consulte Importar um certificado de servidor \( [http:\/\/go.microsoft.com\/fwlink\/? LinkId\=108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Como configurar um proxy do servidor de federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de certificado para servidores de federação](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisitos de certificado para proxies do servidor de federação](https://technet.microsoft.com/library/dd807054.aspx)  
  

