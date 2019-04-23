---
title: Implantar Pastas de Trabalho com o AD FS e o Proxy de aplicativo Web - Etapa 5, Configurar clientes
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: f168292b-0dbc-44b9-965f-d480e5134a0c
ms.openlocfilehash: fa8b2b15ff411a59b28308a329d7ca2341ef0886
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871937"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-5-set-up-clients"></a>Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 5, instalação de clientes

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve a quinta etapa da implantação das Pastas de Trabalho com o AD FS (Serviços de Federação do Active Directory) e o Proxy de aplicativo Web. Você encontrará as outras etapas desse processo nestes tópicos:  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Visão geral](deploy-work-folders-adfs-overview.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 1, configure o AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 2, o trabalho de pós-configuração do AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 3, configurar as pastas de trabalho](deploy-work-folders-adfs-step3.md)  
  
-   [Implante pastas de trabalho com o AD FS e Proxy de aplicativo Web: Etapa 4, configurar o Proxy de aplicativo Web](deploy-work-folders-adfs-step4.md)  
  
Use os procedimentos a seguir para configurar os clientes Windows ingressados no domínio e não ingressados no domínio. Você pode usar esses clientes para testar se arquivos estão realizando a sincronização corretamente entre as Pastas de Trabalho dos clientes.  
  
## <a name="set-up-a-domain-joined-client"></a>Configurar um cliente ingressado no domínio  
  
### <a name="install-the-ad-fs-and-work-folder-certificates"></a>Instalar os certificados do AD FS e de Pastas de Trabalho  
Você deve instalar os certificados do AD FS e de Pastas de Trabalho criados anteriormente no repositório de certificados de computador local, no computador do cliente ingressado no domínio.  
  
Como você está instalando os certificados autoassinados que não podem ser rastreados novamente para um editor no repositório de certificados Autoridades de Certificação Confiáveis, copie também os certificados para esse repositório.  
  
Para instalar os certificados, siga estas etapas:  
  
1.  Clique em **Iniciar** e em **Executar**.  
  
2.  Digite **MMC**.  
  
3.  No menu **Arquivo** , clique em **Adicionar/Remover Snap-in**.  
  
4.  Na lista **Snap-ins disponíveis**, selecione **Certificados** e clique em **Adicionar**. O Assistente de Snap-in de Certificados é iniciado.  
  
5.  Selecione **Conta de computador** e clique em **Avançar**.  
  
6.  Selecione **Computador local: (o computador no qual este console está sendo executado)** e clique em **Concluir**.  
  
7.  Clique em **OK**.  
  
8.  Expanda a pasta Console Root\Certificates\(Local Computer)\Personal\Certificates.  
  
9. Clique com o botão direito do mouse em **Certificados**, clique em **Todas as Tarefas** e clique em **Importar**.  
  
10. Navegue até a pasta que contém o certificado do AD FS, siga as instruções no assistente para importar o arquivo e coloque-o no repositório de certificados.  
  
11. Repita as etapas 9 e 10, desta vez navegando até o certificado de Pastas de Trabalho e importando-o.  
  
12. Expanda a pasta Console Root\Certificates\(Local Computer)\Trusted Root Certification Authorities\Certificates.  
  
13. Clique com o botão direito do mouse em **Certificados**, clique em **Todas as Tarefas** e clique em **Importar**.  
  
14. Navegue até a pasta que contém o certificado do AD FS, siga as instruções no assistente para importar o arquivo e coloque-o no repositório Autoridades de Certificação Confiáveis.  
  
15. Repita as etapas 13 e 14, desta vez navegando até o certificado de Pastas de Trabalho e importando-o.  
  
### <a name="configure-work-folders-on-the-client"></a>Configurar Pastas de Trabalho no cliente  
Para configurar Pastas de Trabalho no computador cliente, siga estas etapas:  
  
1.  No computador cliente, abra **Painel de Controle** e clique em **Pastas de Trabalho**.  
  
2.  Clique em **Configurar Pastas de Trabalho**.  
  
3.  No **insira seu endereço de email de trabalho** , insira o endereço de email do usuário (por exemplo, user@contoso.com) ou a URL de pastas de trabalho (o exemplo de teste, https://workfolders.contoso.com)e, em seguida, clique em **próxima**.  
  
4.  Se o usuário estiver conectado à rede corporativa, a autenticação será executada pela Autenticação Integrada do Windows. Se o usuário não estiver conectado à rede corporativa, a autenticação será realizada pelo AD FS (OAuth), e o usuário será solicitado a inserir as credenciais. Insira suas credenciais e clique em **OK**.  
  
5.  Quando a autenticação estiver concluída, a página **Apresentando Pastas de Trabalho** será exibida, na qual você poderá alterar o local do diretório de Pastas de Trabalho, se desejar. Clique em **Avançar**.  
  
6.  A página **Políticas de Segurança** lista as políticas de segurança configuradas para Pastas de Trabalho. Clique em **Avançar**.  
  
7.  Será exibida uma mensagem informando que Pastas de Trabalho iniciou a sincronização com o computador. Clique em **Fechar**.  
  
8.  A página **Gerenciar Pastas de Trabalho** mostra a quantidade de espaço disponível no servidor, o status de sincronização e assim por diante. Se necessário, você pode inserir novamente suas credenciais aqui. Feche a janela.  
  
9. A pasta Pastas de Trabalho é aberta automaticamente. Você pode adicionar conteúdo a essa pasta para realizar a sincronização entre os dispositivos.  
  
    Para fins de exemplo de teste, adicione um arquivo de teste à pasta Pastas de Trabalho. Após configurar Pastas de Trabalho no computador não ingressado no domínio, você poderá sincronizar arquivos entre as Pastas de Trabalho em cada computador.  
  
## <a name="set-up-a-non-domain-joined-client"></a>Configurar um cliente não ingressado no domínio  
  
### <a name="install-the-ad-fs-and-work-folder-certificates"></a>Instalar os certificados do AD FS e de Pastas de Trabalho  
Instale os certificados do AD FS e de Pastas de Trabalho no computador não ingressado no domínio, usando o mesmo procedimento utilizado no computador ingressado no domínio.  
  
### <a name="update-the-hosts-file"></a>Atualizar o arquivo hosts  
O arquivo hosts no cliente não ingressado no domínio deve ser atualizado para o ambiente de teste, pois não foram criados registros DNS públicos para Pastas de Trabalho. Adicione estas entradas ao arquivo hosts:  
  
-  workfolders.domínio  
  
-  Nome do serviço AD FS.domínio  
  
-  enterpriseregistration.domínio  
  
No exemplo de teste, use estes valores:  
  
-  **10.0.0.10 workfolders.contoso.com**  
  
-  **10.0.0.10 blueadfs.contoso.com**  
  
-  **10.0.0.10 enterpriseregistration.contoso.com**  
  
### <a name="configure-work-folders-on-the-client"></a>Configurar Pastas de Trabalho no cliente  
Configure Pastas de Trabalho no computador não ingressado no domínio usando o mesmo procedimento utilizado para o computador ingressado no domínio.  
  
Quando a nova pasta Pastas de Trabalho é aberta nesse cliente, você verá que o arquivo do computador ingressado no domínio já está sincronizado com o computador não ingressado no domínio. Você pode começar a adicionar conteúdo à pasta para realizar a sincronização entre os dispositivos.  
  
Isso conclui o procedimento de implantação de Pastas de Trabalho, do AD FS e do Proxy de aplicativo Web por meio da interface do usuário do Windows Server.  
  
## <a name="see-also"></a>Consulte também  
[Visão geral de pastas de trabalho](Work-Folders-Overview.md)  
  

