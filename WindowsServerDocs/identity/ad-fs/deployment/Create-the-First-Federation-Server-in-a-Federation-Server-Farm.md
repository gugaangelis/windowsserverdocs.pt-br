---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: Criar o primeiro servidor de federação em um farm de servidores de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: e16289142ea2e53adba52a4ed8f6c01a929a530d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192222"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Criar o primeiro servidor de federação em um farm de servidores de federação

Depois de instalar o serviço de função serviço de Federação e configurar os certificados necessários em um computador, você está pronto para configurar o computador para se tornar um servidor de Federação. Você pode usar o procedimento a seguir para configurar o computador para se tornar o primeiro servidor de Federação em um novo farm de servidores de Federação usando o Assistente de configuração do servidor de Federação do AD FS.  
  
O ato de criar o primeiro servidor de federação em um farm também cria um novo Serviço de Federação e torna esse computador o servidor de federação primário. Isso significa que esse computador será configurado com uma leitura\/gravar cópia de banco de dados de configuração do AD FS. Todos os outros servidores de Federação desse farm devem replicar todas as alterações são feitas no servidor de Federação primário para sua leitura\-somente cópias do banco de dados de configuração do AD FS que eles armazenam localmente. Para obter mais informações sobre esse processo de replicação, consulte [The Role of the AD FS Configuration Database (A função do banco de dados de configuração do AD FS)](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Para um único de Web federado\-sinal\-na \(SSO\) design, você deve ter pelo menos um servidor de federação na organização do parceiro de conta e pelo menos um servidor de federação na organização do parceiro de recurso . Para obter mais informações, consulte [Onde colocar um servidor de federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
O mínimo necessário para concluir esse procedimento é a associação em Admins. do Domínio, ou uma conta de domínio delegada à qual foi atribuído acesso de gravação ao contêiner Dados do Programa no Active Directory.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Para criar o primeiro servidor de federação em um farm de servidores de federação  
  
1.  Há duas maneiras de iniciar o Assistente de configuração do servidor de Federação do AD FS. Para iniciar o assistente, tome uma das seguintes ações:  
  
    -   Após a instalação do serviço de função serviço de Federação for concluída, abra o snap de gerenciamento do AD FS\-em e clique em de **Assistente de configuração do servidor de Federação do AD FS** no link a **visão geral** página ou o **ações** painel.  
  
    -   Qualquer momento depois que o Assistente de instalação for concluído, abra Windows Explorer, navegue até a **c:\\Windows\\ADFS** pasta e, em seguida, double\-clique **FsConfigWizard.exe**.  
  
2.  Na página **Bem-vindo**, verifique se **Criar um novo Serviço de Federação** está selecionado e clique em **Avançar**.  
  
3.  No **selecionar espera\-Alone ou implantação de Farm** , clique em **novo farm de servidores de Federação**e, em seguida, clique em **próximo**.  
  
4.  Na página **Especificar o Nome do Serviço de Federação**, verifique se o **Certificado SSL** que aparece está correto. Se não for o certificado correto, selecione o certificado apropriado na lista **Certificado SSL**.  
  
    Esse certificado é gerado a partir do Secure Sockets Layer \(SSL\) configurações para o Site da Web padrão. Se o Site Padrão tiver somente um certificado SSL configurado, esse certificado será apresentado e automaticamente selecionado para uso. Se vários certificados SSL forem configurados para o Site Padrão, todos esses certificados serão listados aqui e você deverá selecionar um dentre eles. Se não houver configurações do SSL definidas para o Site Padrão, a lista será gerada dos certificados disponíveis no repositório de certificados pessoal no computador local.  
  
    > [!NOTE]  
    > O assistente não permitirá que você substitua o certificado se um certificado SSL estiver configurado para IIS. Isso assegura que qualquer configuração anterior do IIS pretendida para certificados SSL seja preservada. Para resolver essa restrição, você poderá remover o certificado ou reconfigurá-lo manualmente com o Console de Gerenciamento do IIS.  
  
5.  Se o banco de dados do AD FS que você selecionou já existir, o **banco de dados existente do AD FS Configuration detectado** página será exibida. Se essa página aparecer, clique em **Excluir banco de dados** e clique em **Avançar**.  
  
    > [!CAUTION]  
    > Selecione esta opção apenas quando tiver certeza de que os dados neste banco de dados do AD FS não são importantes ou que não é usado em um farm de servidores de federação de produção.  
  
6.  Na página **Especificar uma Conta de Serviço**, clique em **Procurar**. Na caixa de diálogo **Procurar**, localize a conta de domínio que será usada como a conta de serviço nesse novo farm de servidores de federação, e clique em **OK**. Digite uma senha para essa conta, confirme-a e clique em **Avançar**.  
  
    > [!NOTE]  
    > Ver [configurar manualmente uma conta de serviço para um Farm de servidores de Federação](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) para obter mais informações sobre como especificar uma conta de serviço para um farm de servidores de Federação. Cada servidor de federação no farm de servidores de federação deve especificar a mesma conta de serviço para o farm fique operacional. Por exemplo, se a conta de serviço que foi criada era contoso\\ADFS2SVC, cada computador que você configurar para a função de servidor de Federação e que participará no mesmo farm deve especificar contoso\\ADFS2SVC isso entrar a Federação Server Assistente de configuração para o farm fique operacional.  
  
7.  Na página **Pronto para Aplicar Configurações**, examine os detalhes. Se as configurações parecerem corretas, clique em **próxima** para começar a configurar o AD FS com essas configurações.  
  
8.  Na página **Resultados da Configuração**, examine os resultados. Quando todas as etapas de configuração estiverem concluídas, clique em **fechar** para sair do assistente.  
  
    > [!IMPORTANT]  
    > Para fins de implantação segura, a resolução de artefato e a detecção de resposta são desabilitadas quando você usa o Assistente de Configuração do Servidor de Federação do AD FS para configurar um farm de servidores de federação. Esse assistente configura automaticamente o Banco de Dados Interno do Windows para armazenar dados de configuração de serviço. Você pode, no entanto, erradamente, desfazer essa alteração, permitindo que o ponto de extremidade de resolução de artefato usando qualquer um de **pontos de extremidade** nó no snap do gerenciamento do AD FS\-em ou permitir\-cmdlet ADFSEndpoint no Windows PowerShell. Tenha cuidado para não reconfigurar a definição padrão de forma que esse ponto de extremidade permaneça desabilitado ao usar um farm de servidores de federação junto com o Banco de Dados Interno do Windows.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

