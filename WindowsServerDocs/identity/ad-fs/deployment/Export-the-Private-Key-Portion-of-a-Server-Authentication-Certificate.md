---
ms.assetid: cd4d4902-dcdf-49dd-8059-82a56bf4b585
title: "Exportar a parte de chave privada de um certificado de autenticação de servidor"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c968f0702d56b56d0a80459e5cf0c9e658c56741
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="export-the-private-key-portion-of-a-server-authentication-certificate"></a>Exportar a parte de chave privada de um certificado de autenticação de servidor

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada servidor de Federação em um farm de \(AD FS\) de serviços de Federação do Active Directory deve ter acesso à chave privada do certificado de autenticação do servidor. Se você estiver implementando um farm de servidores de Federação ou servidores Web, você deve ter um certificado de autenticação simples. Esse certificado deve ser emitido por uma autoridade de certificação corporativa \(CA\) e ele devem ter uma chave privada exportable. A chave privada do certificado de autenticação do servidor deve ser exportable para que ele pode ficar disponível para todos os servidores no farm.  
  
Esse conceito mesmo é verdadeiro farms de proxy de servidor de federação no sentido de que todos os proxies de servidor de Federação em um farm devem compartilhar parte da chave privada do certificado de autenticação do mesmo servidor.  
  
> [!NOTE]  
> O AD FS snap\-in Gerenciamento refere-se aos certificados de autenticação de servidor para servidores de federação como certificados de comunicação do serviço.  
  
Dependendo de qual função reproduzirá neste computador, use este procedimento no computador do servidor de Federação ou computador de proxy do servidor de Federação em que você instalou o certificado de autenticação de servidor com a chave privada. Quando você concluir o procedimento, você pode importar esse certificado no site do padrão de cada servidor no farm. Para obter mais informações, consulte [importar um certificado de autenticação de servidor para o site padrão](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-export-the-private-key-portion-of-a-server-authentication-certificate"></a>Para exportar parte da chave privada de um certificado de autenticação de servidor  
  
1.  Sobre o **iniciar** de tela, digite**\(IIS\) Internet Information Services Manager**, e pressione ENTER.  
  
2.  Na árvore de console, clique em **ComputerName**.  
  
3.  No painel central, clique em double\ **certificados de servidor**.  
  
4.  No painel central, clique right\ o certificado que você deseja exportar e, em seguida, clique em **exportar**.  
  
5.  No **exportar o certificado** caixa de diálogo, clique no **...** botão.  
  
6.  Em **nome do arquivo**, tipo **C:\\***NameofCertificate*e clique em **abrir**.  
  
7.  Digite uma senha para o certificado, confirmá-la e, em seguida, clique em **Okey**.  
  
8.  Valide o sucesso da exportação confirmando que o arquivo especificado é criado no local especificado.  
  
    > [!IMPORTANT]  
    > Para que esse certificado pode ser importado para o repositório de certificados local no novo servidor, você deve transferir o arquivo de mídia física e proteger sua segurança durante o transporte para o novo servidor. É extremamente importante proteger a segurança da chave privada. Se essa chave for comprometida, a segurança da implantação do AD FS inteira \ (incluindo os recursos em sua organização e no recurso parceiro organizations\) for comprometido.  
  
9. Importe o certificado de autenticação de servidor exportado para o repositório de certificados no novo servidor antes de instalar o serviço de Federação. Para obter informações sobre como importar o certificado, consulte Importar um certificado de servidor \ ([http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 108283](https://go.microsoft.com/fwlink/?LinkId=108283)\).  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurar um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Lista de verificação: Configurando um Proxy de servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de certificado para servidores de Federação](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Requisitos de certificado para Proxies de servidor de Federação](https://technet.microsoft.com/library/dd807054.aspx)  
  

