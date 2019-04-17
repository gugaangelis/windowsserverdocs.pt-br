---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: "Criar o primeiro servidor de Federação em um Farm de servidores de Federação"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: af0aa61f0d16d4ca567b140c95d74445d09f1cf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Criar o primeiro servidor de Federação em um Farm de servidores de Federação

 >Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de instalar o serviço de função do serviço de Federação e configurar os certificados necessários em um computador, você estará pronto para configurar o computador para se tornar um servidor de Federação. Você pode usar o procedimento a seguir para configurar o computador para se tornar o primeiro servidor de Federação em um novo farm de servidores de Federação usando o Assistente para configuração do servidor de Federação do AD FS.  
  
O ato de criar o primeiro servidor de Federação em um farm também cria um novo serviço de Federação e faz o servidor de Federação principal com este computador. Isso significa que este computador será configurado com uma cópia read\/gravação do banco de dados de configuração do AD FS. Todos os outros servidores de Federação neste farm devem replicar as alterações feitas no servidor de Federação principal em suas cópias somente read\ do banco de dados de configuração do AD FS armazenados localmente por eles. Para obter mais informações sobre esse processo de replicação, consulte [a função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Para o design \(SSO\) federados Single\-Sign\-On da Web, você deve ter pelo menos um servidor de federação na organização do parceiro de conta e pelo menos um servidor de federação na organização do parceiro de recurso. Para obter mais informações, consulte [onde colocar um servidor de Federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
A associação ao grupo Admins. do domínio ou uma conta de domínio delegado concedeu acesso de gravação para o contêiner de dados de programa no Active Directory, é o requisito mínimo para concluir este procedimento.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Para criar o primeiro servidor de Federação em um farm de servidores de Federação  
  
1.  Há duas maneiras de iniciar o Assistente para configuração do servidor de Federação do AD FS. Para iniciá-lo, siga um destes procedimentos:  
  
    -   Após a instalação de serviço de função do serviço de Federação for concluída, abra o AD FS snap\-in Gerenciamento e clique no **Assistente de configuração de servidor de Federação do AD FS** vincular no **visão geral** página ou no **ações** painel.  
  
    -   Qualquer momento depois que o Assistente para instalação for concluída, abra Windows Explorer, navegue até o **C:\\Windows\\ADFS** pasta e, em seguida, clique em double\ **FsConfigWizard.exe**.  
  
2.  No **boas-vindas** página, verifique **criar um novo serviço de Federação** está selecionado e clique em **próxima**.  
  
3.  No **selecione Stand\-sozinho ou implantação Farm** página, clique em **novo farm de servidores de Federação**e clique em **próxima**.  
  
4.  No **especificar o nome do serviço de Federação** página, verifique o **certificado SSL** que está mostrando está correta. Se isso não for o certificado correto, selecione o certificado apropriado do **certificado SSL** lista.  
  
    Esse certificado é gerado das configurações de Secure Sockets Layer \(SSL\) do site padrão. Se o site padrão tem apenas um certificado SSL configurado, esse certificado é apresentado e selecionado automaticamente para uso. Se vários certificados SSL são configurados para o site padrão, todos esses certificados são listados aqui e você deve selecionar dentre elas. Se não houver nenhuma configuração de SSL para o site padrão, a lista é gerada a partir dos certificados que estão disponíveis no repositório de certificados pessoais no computador local.  
  
    > [!NOTE]  
    > O assistente não permitirá que você substituir o certificado se um certificado SSL está configurado para o IIS. Isso garante que qualquer destinado a configuração anterior do IIS para certificados SSL é preservada. Para contornar essa restrição, você pode remover o certificado ou reconfigurá-lo manualmente com o Console de gerenciamento do IIS.  
  
5.  Se o banco de dados do AD FS que você selecionou já existir, o **existente AD FS configuração do banco de dados detectado** página será exibida. Se essa página for exibida, clique em **banco de dados de excluir**e clique em **próxima**.  
  
    > [!CAUTION]  
    > Selecione essa opção somente quando tiver certeza de que os dados neste banco de dados do AD FS não são importantes ou que não é usado em um farm de servidores de federação de produção.  
  
6.  Sobre o **especificar uma conta de serviço** página, clique em **procurar**. No **procurar** caixa de diálogo caixa, localize a conta de domínio que será usada como a conta de serviço neste novo farm de servidores de Federação e, em seguida, clique em **Okey**. Digite a senha dessa conta, confirmá-la e, em seguida, clique em **próxima**.  
  
    > [!NOTE]  
    > Consulte [manualmente configurar uma conta de serviço para um Farm de servidores de Federação](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) para obter mais informações sobre como especificar uma conta de serviço para um farm de servidores de Federação. Cada servidor de federação no farm de servidor de federação deve especificar a mesma conta de serviço para que o farm estar funcionando. Por exemplo, se a conta de serviço que foi criada foi contoso\\ADFS2SVC, cada computador que você definir para a função de servidor de Federação e que participarão mesmo farm deve especificar contoso\\ADFS2SVC nessa etapa no Assistente de configuração de servidor de Federação do farm se torne operacional.  
  
7.  Sobre o **pronto para aplicar configurações** página, examine os detalhes. Se as configurações parecerem corretas, clique em **próxima** para começar a configurar o AD FS com essas configurações.  
  
8.  Sobre o **resultados de configuração** página, examinar os resultados. Quando terminarem de todas as etapas de configuração, clique em **fechar** para sair do assistente.  
  
    > [!IMPORTANT]  
    > Para fins de implantação segura, detecção de resolução e responder artefato são desabilitados quando você usar o Assistente para configuração do servidor de Federação do AD FS para configurar um farm de servidores de Federação. Este assistente configura automaticamente o banco de dados interno Windows para armazenar dados de configuração do serviço. Você pode, no entanto, por engano desfazer essa alteração, permitindo que o ponto de extremidade de resolução de artefato usando o **pontos de extremidade** nó no AD FS snap\-in Gerenciamento ou o cmdlet Enable\ ADFSEndpoint no Windows PowerShell. Tome cuidado para não reconfigurar a configuração padrão para que esse ponto de extremidade permanece desativado quando você usa um farm de servidores de Federação e o banco de dados interno do Windows juntos.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurar um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

