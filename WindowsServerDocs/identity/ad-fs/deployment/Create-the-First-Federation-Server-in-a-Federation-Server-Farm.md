---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: Criar o primeiro servidor de federação em um farm de servidores de federação
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0fe5c3160f661357536ef3bd60762873063c8ed0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855469"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Criar o primeiro servidor de federação em um farm de servidores de federação

Depois de instalar o Serviço de Federação serviço de função e configurar os certificados necessários em um computador, você estará pronto para configurar o computador para se tornar um servidor de Federação. Você pode usar o procedimento a seguir para configurar o computador para se tornar o primeiro servidor de Federação em um novo farm de servidores de Federação usando o assistente de configuração do servidor de Federação AD FS.  
  
O ato de criar o primeiro servidor de federação em um farm também cria um novo Serviço de Federação e torna esse computador o servidor de federação primário. Isso significa que esse computador será configurado com uma cópia de leitura\/gravação do banco de dados de configuração do AD FS. Todos os outros servidores de Federação neste farm devem replicar todas as alterações feitas no servidor de Federação primário para suas leituras\-apenas cópias do banco de dados de configuração de AD FS que eles armazenam localmente. Para obter mais informações sobre esse processo de replicação, consulte [The Role of the AD FS Configuration Database (A função do banco de dados de configuração do AD FS)](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Para o\-de logon\-único da Web federado em \(design de\) SSO, você deve ter pelo menos um servidor de Federação na organização do parceiro de conta e pelo menos um servidor de Federação na organização do parceiro de recurso. Para obter mais informações, consulte [Onde colocar um servidor de federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
O mínimo necessário para concluir esse procedimento é a associação em Admins. do Domínio, ou uma conta de domínio delegada à qual foi atribuído acesso de gravação ao contêiner Dados do Programa no Active Directory.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Para criar o primeiro servidor de federação em um farm de servidores de federação  
  
1.  Ha duas maneiras para iniciar o Assistente de Configuração do Servidor de Federação AD FS. Para iniciar o assistente,  faça o seguinte:  
  
    -   Depois que a instalação do serviço de função Serviço de Federação for concluída, abra o\-snap de gerenciamento de AD FS no e clique no link **Assistente de configuração do servidor de federação AD FS** na página **visão geral** ou no painel **ações** .  
  
    -   Sempre que o assistente de instalação for concluído, abra o Windows Explorer, navegue até a pasta **C:\\Windows\\ADFS** e clique duas vezes\-clique em **FsConfigWizard. exe**.  
  
2.  Na página **Bem-vindo**, verifique que o **Cria um novo Serviço de Federação** esteja selecionado, e então clique em **Próximo**.  
  
3.  Na página **selecionar\-autônomo ou implantação de farm** , clique em **novo farm de servidores de Federação**e clique em **Avançar**.  
  
4.  Na página **Especificar o Nome do Serviço de Federação**, verifique que o **Certificado SSL** que é exibido seja o correto. Si este não for o certificado correto, selecione o certificado apropriado da lista **Certificado SSL**.  
  
    Esse certificado é gerado a partir do protocolo SSL \(configurações de\) SSL para o site padrão. Se o Site Padrão só tiver um certificado SSL configurado, aquele certificado é apresentado e automaticamente selecionado para uso. Se vários certificados SSL são configurados para o Site Padrão, todos aqueles certificados são listados aqui e você deve selecionar de entre eles. Se não houver configurações SSL definidas para o Site Padrão, a lista é gerada a partir dos certificados que estiverem disponíveis no armazenamento pessoal de certificados no computador local.  
  
    > [!NOTE]  
    > O assistente não permitirá a substituição do certificado se um certificado SSL for configurado para o IIS. Isso assegura que qualquer objetivo antes da configuração do IIS para certificados SSL seja preservado. Para solucionar alternativamente esta restrição, você pode remover o certificado ou reconfigurá-lo manualmente com o Console de Gerenciamento IIS.  
  
5.  Se o banco de dados do AD FS que você selecionou já existir, a página **Banco de Dados de Configuração do AD FS Existente Detectada** aparece. Se aquela página aparecer, clique em **Excluir banco de dados**, e depois clique em **Próximo**.  
  
    > [!CAUTION]  
    > Selecione esta opção apenas quando você tiver certeza que os dados neste banco de dados do AD FS não forem importantes ou que não é usado em um farm de servidor de federação.  
  
6.  Na página **Especificar uma Conta de Serviço**, clique em **Navegar**. Na caixa de diálogo **Navegar**, localizar a conta de domínio que será usada como a conta de serviço neste farm de servidor de federação, e em seguida clique em **OK**. Digite a senha para esta conta, confirme ela, e em seguida clique em **Próximo**.  
  
    > [!NOTE]  
    > Confira [configurar manualmente uma conta de serviço para um farm de servidores de Federação](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) para obter mais informações sobre como especificar uma conta de serviço para um farm de servidores de Federação. Cada servidor de Federação no farm de servidores de Federação deve especificar a mesma conta de serviço para o farm estar operacional. Por exemplo, se a conta de serviço criada foi contoso\\ADFS2SVC, cada computador configurado para a função de servidor de Federação e que participará do mesmo farm deverá especificar contoso\\ADFS2SVC nesta etapa no assistente de configuração do servidor de Federação para que o farm esteja operacional.  
  
7.  Na página **Pronto para Aplicar as Configurações**, verificar os detalhes. Se as configurações parecem estar certas, clique em **Próximo** para começar configurar o AD FS com estas configurações.  
  
8.  Na página **Resultados de Configuração**, analise os resultados. Quando todas as etapas de configuração forem concluídas, clique em **fechar** para sair do assistente.  
  
    > [!IMPORTANT]  
    > Para propósitos de implantação seguros, a resolução de artefato e detecção de resposta são desabilitados quando você usa o Assistente de Configuração do Servidor de Federação AD FS para configurar um farm do servidor de federação. Este assistente configura automaticamente o Banco de dados Interno do Windows para armazenar os dados de configuração do serviço. No entanto, você pode desfazer essa alteração por engano, habilitando o ponto de extremidade de resolução de artefatos usando o nó **pontos** de extremidade no snap\-de gerenciamento de AD FS no ou o cmdlet Enable\-ADFSEndpoint no Windows PowerShell. Seja cuidadoso em não reconfigurar as configurações padrão de modo que o ponto de extremidade permaneça desabilitado quando você usar um farm de servidor de federação e o Banco de Dados Interno do Windows juntos.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

