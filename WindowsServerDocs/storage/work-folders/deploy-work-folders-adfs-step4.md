---
title: Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web - Etapa 4, Configurar o Proxy de aplicativo Web
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 6/242017
ms.assetid: 4a11ede0-b000-4188-8190-790971504e17
ms.openlocfilehash: 1f452fd1e2f054c449660eb0ee12642fefe4da8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865047"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-4-set-up-web-application-proxy"></a>Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 4, o Proxy de aplicativo Web de configuração

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a quarta etapa da implantação das Pastas de Trabalho com o AD FS (Serviços de Federação do Active Directory) e o Proxy de aplicativo Web. Você encontrará as outras etapas desse processo nestes tópicos:  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Visão geral](deploy-work-folders-adfs-overview.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 1, configure o AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 2, o trabalho de pós-configuração do AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 3, configurar as pastas de trabalho](deploy-work-folders-adfs-step3.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 5, configurar os clientes](deploy-work-folders-adfs-step5.md)  

> [!NOTE]
>   As instruções abordadas nesta seção destinam-se a um ambiente do Server 2016. Se você estiver usando o Windows Server 2012 R2, siga as [instruções do Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

Para configurar o Proxy de aplicativo Web para uso com Pastas de Trabalho, use os procedimentos a seguir.  
  
## <a name="install-the-ad-fs-and-work-folder-certificates"></a>Instalar os certificados do AD FS e de Pastas de Trabalho  
Você deve instalar os certificados do AD FS e de Pastas de Trabalho criados anteriormente (na etapa 1, Configurar o AD FS, e na etapa 3, Configurar Pastas de Trabalho) no repositório de certificados de computador local, no computador em que a função Proxy de aplicativo Web será instalada.  
  
Como você está instalando os certificados autoassinados que não podem ser rastreados novamente para um editor no repositório de certificados Autoridades de Certificação Confiáveis, copie também os certificados para esse repositório.  
  
Para instalar os certificados, siga estas etapas:  
  
1.  Clique em **Iniciar** e em **Executar**.  
  
2.  Digite **MMC**.  
  
3.  No menu **Arquivo** , clique em **Adicionar/Remover Snap-in**.  
  
4.  Na lista **Snap-ins disponíveis**, selecione **Certificados** e clique em **Adicionar**. O Assistente de Snap-in de Certificados é iniciado.  
  
5.  Selecione **Conta de computador** e clique em **Avançar**.  
  
6.  Selecione **Computador local: (o computador no qual este console está sendo executado)** e clique em **Concluir**.  
  
7.  Clique em **OK**.  
  
8.  Expanda a pasta **Console Root\Certificates\(Local Computer)\Personal\Certificates**.  
  
9. Clique com o botão direito do mouse em **Certificados**, clique em **Todas as Tarefas** e clique em **Importar**.  
  
10. Navegue até a pasta que contém o certificado do AD FS, siga as instruções no assistente para importar o arquivo e coloque-o no repositório de certificados.  
  
11. Repita as etapas 9 e 10, desta vez navegando até o certificado de Pastas de Trabalho e importando-o.  
  
12. Expanda a pasta **Console Root\Certificates\(Local Computer) \Trusted Root Certification Authorities\Certificates**.  
  
13. Clique com o botão direito do mouse em **Certificados**, clique em **Todas as Tarefas** e clique em **Importar**.  
  
14. Navegue até a pasta que contém o certificado do AD FS, siga as instruções no assistente para importar o arquivo e coloque-o no repositório Autoridades de Certificação Confiáveis.  
  
15. Repita as etapas 13 e 14, desta vez navegando até o certificado de Pastas de Trabalho e importando-o.  
  
## <a name="install-web-application-proxy"></a>Instalar o Proxy de aplicativo Web  
Para instalar o Proxy de aplicativo Web, siga estas etapas:  
  
1.  No servidor em que você pretende instalar o Proxy de aplicativo Web, abra o **Gerenciador do Servidor** e inicie o **Assistente de Adição de Funções e Recursos**.  
  
2.  Clique em **Avançar** na primeira e segunda páginas do assistente.  
  
3.  Na página **Seleção de Servidor**, selecione o servidor e clique em **Avançar**.  
  
4.  Na página **Função de Servidor**, selecione a função **Acesso Remoto** e clique em **Avançar**.  
  
5.  Na página Recursos e na página Acesso Remoto, clique em **Avançar**.  
  
6.  Na página **Serviços de Função**, selecione **Proxy de aplicativo Web**, clique em **Adicionar Recursos** e clique em **Avançar**.

7.  Na página **Confirmar seleções de instalação** , clique em **Instalar**.  
  
## <a name="configure-web-application-proxy"></a>Configurar o proxy de aplicativo Web  
Para configurar o Proxy de aplicativo Web, siga estas etapas:  
  
1.  Clique no sinalizador de aviso na parte superior do Gerenciador do Servidor Proxy Configuration Wizard Application Web e, em seguida, Configuração do Proxy de Aplicativo Web.  
  
2.  Na página Bem-vindo, pressione **Avançar**.  
  
3.  Na página **Servidor de Federação**, insira o nome do serviço de federação. No exemplo de teste, esse serviço é **blueadfs.contoso.com**.  
  
4.  Insira as credenciais de uma conta de administrador local nos servidores de federação. Não insira as credenciais de domínio (por exemplo, contoso\administrador), mas as credenciais locais (por exemplo, administrador).  
  
5.  Na página **Certificado de Proxy do AD FS**, selecione o certificado do AD FS importado anteriormente. No caso de teste, esse certificado é **blueadfs.contoso.com**. Clique em **Avançar**.  
  
6.  A página de confirmação mostra o comando do Windows PowerShell que será executado para configurar o serviço. Clique em **Configurar**.  
  
## <a name="publish-the-work-folders-web-application"></a>Publicar o aplicativo Web de Pastas de Trabalho  
A próxima etapa é publicar um aplicativo Web que disponibilizará Pastas de Trabalho aos clientes. Para publicar o aplicativo Web de Pastas de Trabalho, siga estas etapas:  
  
1.  Abra **Gerenciador do Servidor** e, no menu **Ferramentas**, clique em **Gerenciamento de Acesso Remoto** para abrir o Console de Gerenciamento de Acesso Remoto.  
  
2.  Em **Configuração**, clique em **Proxy de aplicativo Web**.  
  
3.  Em **Tarefas**, clique em **Publicar**. O Assistente para Publicar Novos Aplicativos é aberto.  
  
4.  Na página Bem-vindo, clique em **Avançar**.  
  
5.  Na página **Pré-autenticação**, selecione **Serviços de Federação do Active Directory (AD FS)** e clique em **Avançar**.  
  
6.  Na página **Dar Suporte a Clientes**, selecione **OAuth2** e clique em **Avançar**.

7.  Na página **Terceira Parte Confiável**, selecione **Pastas de Trabalho** e clique em **Avançar**. Essa lista é publicada no Proxy de aplicativo Web do AD FS.  
  
8.  Na página **Configurações de Publicação**, insira os dados a seguir e clique em **Avançar**:  
  
    -   O nome que você deseja usar para o aplicativo Web  
  
    -   A URL externa de Pastas de Trabalho  
  
    -   O nome do certificado de Pastas de Trabalho  
  
    -   A URL de back-end para Pastas de Trabalho  
  
    Por padrão, o assistente torna a URL de back-end idêntica à URL externa.  
  
    No exemplo de teste, use estes valores:  
  
    Nome: **WorkFolders**  
  
    URL externa: **https://workfolders.contoso.com**  
  
    Certificado externo: **O certificado de pastas de trabalho que você instalou anteriormente**  
  
    URL do servidor de back-end: **https://workfolders.contoso.com**  
  
9.  A página de confirmação mostra o comando do Windows PowerShell que será executado para publicar o aplicativo. Clique em **Publicar**.  
  
10. Na página **Resultados**, você verá que o aplicativo foi publicado com êxito.
   >[!NOTE]
   > Se você tiver vários servidores de Pastas de Trabalho, você precisa publicar um aplicativo Web de Pastas de Trabalho para cada servidor das Pastas de Trabalho (repita as etapas 1 a 10).  
  
Próxima etapa: [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 5, configurar os clientes](deploy-work-folders-adfs-step5.md)  
  
## <a name="see-also"></a>Consulte também  
[Visão geral de pastas de trabalho](Work-Folders-Overview.md)  
  

