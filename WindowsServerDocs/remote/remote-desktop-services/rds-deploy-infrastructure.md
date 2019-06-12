---
title: Implantar o seu ambiente de Área de Trabalho Remota
ms.custom: na
ms.prod: windows-server-threshold
description: Etapas básicas para implantar um ambiente de área de trabalho remota.
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 5b9ce1bb87a7a2ad8819235edc412fd095bc2985
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805136"
---
# <a name="deploy-your-remote-desktop-environment"></a>Implantar o seu ambiente de Área de Trabalho Remota

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016

Use as etapas a seguir para implantar os servidores de área de trabalho remota em seu ambiente. Você pode instalar as funções de servidor em computadores físicos ou máquinas virtuais, dependendo se você estiver criando um local, com base em nuvem ou ambiente híbrido. 

Se você estiver usando máquinas virtuais para qualquer um dos servidores dos serviços de área de trabalho remota, verifique se você tem [preparado essas máquinas virtuais](rds-prepare-vms.md).
  
  
1.  Adicione todos os servidores que você pretende usar para serviços de área de trabalho remota ao Gerenciador do servidor:  
    1.  No Gerenciador do servidor, clique em **Manage** > **adicionar servidores**.  
    2.  Clique em **Localizar Agora**.  
    3.  Clique em cada servidor na implantação (por exemplo, Contoso Cb1, Contoso-WebGw1 e Sh1 Contoso) e clique em **Okey**.  
2.  Crie uma implantação baseada em sessão para implantar os componentes de serviços de área de trabalho remota:  
    1.  No Gerenciador do servidor, clique em **Manage** > **adicionar funções e recursos**.  
    2.  Clique em **instalação de serviços de área de trabalho remota**, **implantação padrão**, e **implantação de desktop baseada em sessão**.  
    3.  Selecione os servidores apropriados para o servidor do agente de Conexão de área de trabalho remota, o servidor de acesso via Web RD e o servidor Host de sessão de área de trabalho remota (por exemplo, Contoso-Cb1, Contoso-WebGw1 e Contoso-SH1, respectivamente).  
    4.  Selecione **reiniciar o servidor de destino automaticamente se necessário**e, em seguida, clique em **implantar**.  
    5.  Aguarde até que a implantação seja concluída com êxito  
3.  Adicione o servidor de licenças de área de trabalho remota:  
    1.  No Gerenciador do servidor, clique em **Remote Desktop Services > Visão geral > + licenciamento de área de trabalho remota**.  
    2.  Selecione a máquina virtual em que o servidor de licenças de área de trabalho remota será instalado (por exemplo, Contoso-Cb1).  
    3.  Clique em **próxima**e, em seguida, clique em **Add**.  
4.  Ativar o servidor de licenças de área de trabalho remota e adicioná-lo ao grupo de servidores de licença:  
    1.  No Gerenciador do servidor, clique em **Ferramentas > Serviços de Terminal > Gerenciador de licenciamento de área de trabalho remota**.  
    2.  No Gerenciador de licenciamento de área de trabalho remota, selecione o servidor e, em seguida, clique em **ação > Ativar servidor**.  
    3.  Aceite os valores padrão no Assistente de servidor ativar aceitando os padrões, até chegar a **informações da empresa** página. Em seguida, insira as informações da sua empresa.  
    4.  Aceite os padrões para as páginas restantes até que a página final. Desmarque **iniciar instalar licenças o assistente agora**e, em seguida, clique em **concluir**.  
    5.  Clique em **ação > Examinar Configuração > Adicionar ao grupo > Okey**. Insira as credenciais para um usuário no grupo de administradores do AAD DC e registrar o SCP. Esta etapa pode não funcionar se você estiver usando o Azure AD Domain Services, mas você pode ignorar os avisos ou erros.  
5.  Adicione o nome de servidor e o certificado de Gateway de área de trabalho remota:  
    1.  No Gerenciador do servidor, clique em **Remote Desktop Services > Visão geral > + Gateway de área de trabalho remota**.  
    2.  No Assistente para adicionar servidores de Gateway de área de trabalho remota, selecione a máquina virtual em que você deseja instalar o servidor de Gateway de área de trabalho remota (por exemplo, Contoso-WebGw1).  
    3.  Insira o nome do certificado SSL para o servidor de Gateway de área de trabalho remota usando o externo totalmente qualificado nome DNS (FQDN) do servidor de Gateway de área de trabalho remota. No Azure, isso é o **nome DNS** rotular e usa o formato servicename.location.cloudapp.azure.com. Por exemplo, contoso.westus.cloudapp.azure.com.  
    4.  Clique em **próxima**e, em seguida, clique em **Add**.
6.  Criar e instalar certificados autoassinados para os servidores de Gateway de área de trabalho remota e o agente de Conexão de área de trabalho.

       > [!NOTE]
       > Se você estiver fornecendo e instalação de certificados de autoridade de certificação confiável, execute os procedimentos de etapa h para a etapa k para cada função. Você precisará ter o arquivo. pfx está disponível para cada um desses certificados.
       
    1.  No Gerenciador do servidor, clique em **Remote Desktop Services > Visão geral > tarefas > Editar propriedades de implantação**.  
    2.  Expandir **certificados**e, em seguida, role para baixo até a tabela. Clique em **Gateway de área de trabalho remota > Criar novo certificado**.  
    3.  Digite o nome do certificado usando o FQDN externo do servidor de Gateway de área de trabalho remota (por exemplo, contoso.westus.cloudapp.azure.com) e, em seguida, digite a senha.  
    4.  Selecione **Store esse certificado** e, em seguida, navegue até a pasta compartilhada que você criou para certificados em uma etapa anterior. (Por exemplo, \Contoso Cb1\Certificates.)  
    5.  Insira um nome de arquivo para o certificado (por exemplo, ContosoRdGwCert) e, em seguida, clique em **salvar**.  
    6.  Selecione **permitir que o certificado a ser adicionado ao repositório de certificados de autoridades de certificação raiz confiáveis nos computadores de destino**e, em seguida, clique em **Okey**.  
    7.  Clique em **aplicar**e, em seguida, aguarde o certificado a ser aplicado com êxito para o servidor de Gateway de área de trabalho remota.  
    8.  Clique em **acesso via Web RD > selecione o certificado existente**.  
    9.  Navegue até o certificado criado para o servidor de Gateway de área de trabalho remota (por exemplo, ContosoRdGwCert) e, em seguida, clique em **aberto**.  
    10. Insira a senha do certificado, selecione **permitir que o certificado a ser adicionado ao repositório de certificados de raiz confiáveis nos computadores de destino**e, em seguida, clique em **Okey**.  
    11. Clique em **aplicar**e, em seguida, aguarde o certificado a ser aplicado com êxito para o servidor de acesso via Web RD.  
    12. Repetir substeps 1 a 11 para o **agente de Conexão de área de trabalho - habilitar logon único** e **agente de Conexão de área de trabalho - serviços de publicação**, usando o FQDN interno do servidor do agente de Conexão de área de trabalho remota para o novo nome do certificado (por exemplo, Contoso-Cb1.Contoso.com).  
7.  Exportar certificados autoassinados do públicos e copiá-los para um computador cliente. Se você estiver usando certificados de autoridade de certificação confiável, você pode ignorar esta etapa.  
    1.  Inicie o certlm.  
    2.  Expandir **pessoais**e, em seguida, clique em **certificados**.  
    3.  No painel direito, clique no certificado de agente de Conexão de área de trabalho usado para autenticação de clientes, por exemplo **Contoso-Cb1.Contoso.com**.  
    4.  Clique em **todas as tarefas > Exportar**.  
    5.  Aceite as opções padrão no Assistente para exportação de certificado aceitam os padrões até chegar a **arquivo a ser exportado** página.  
    6.  Navegue até a pasta compartilhada que você criou para certificados, por exemplo \Contoso-Cb1\Certificates.  
    7.  Insira um nome de arquivo, por exemplo, ContosoCbClientCert e, em seguida, clique em **salvar**.  
    8.  Clique em **Avançar**e em **Concluir**.  
    9.  Repita subetapas 1 a 8 para o certificado de Gateway de área de trabalho remota e da Web, (por exemplo contoso.westus.cloudapp.azure.com), fornecendo o certificado exportado um nome de arquivo apropriado, por exemplo **ContosoWebGwClientCert**.  
    10. No Explorador de arquivos, navegue até a pasta onde os certificados estão armazenados, por exemplo \Contoso-Cb1\Certificates.  
    11. Selecione os dois certificados de cliente exportado, em seguida, clique com botão direito-los e, em seguida, clique **cópia**.  
    12. Cole os certificados no computador cliente local.  
8.  Configure as propriedades de implantação do Gateway de área de trabalho remota e licenciamento de área de trabalho remota:  
    1.  No Gerenciador do servidor, clique em **Remote Desktop Services > Visão geral > tarefas > Editar propriedades de implantação**.  
    2.  Expandir **Gateway de área de trabalho remota** e desmarque as **servidor de Gateway de área de trabalho remota de Bypass para endereços locais** opção.  
    3.  Expandir **licenciamento de área de trabalho remota** e selecione **por usuário**  
    4.  Clique em **OK**.  
10. Crie uma coleção de sessão. Estas etapas criam um conjunto básico. Fazer check-out [criar uma coleção de serviços de área de trabalho remota para áreas de trabalho e aplicativos sejam executados](rds-create-collection.md) para obter mais informações sobre coleções.
 
    1.  No Gerenciador do servidor, clique em **Remote Desktop Services > coleções > tarefas > Criar coleção de sessão**.  
    2.  Insira uma nome (por exemplo, ContosoDesktop) da coleção.  
    3.  Selecione um servidor de Host de sessão de área de trabalho remota (Contoso-Sh1), aceite os grupos de usuários padrão (Contoso\Domain usuários) e insira o caminho de convenção de nomenclatura Universal (UNC) para os discos de perfil de usuário criados acima (\Contoso-Cb1\UserDisks).  
    4.  Definir um tamanho máximo e, em seguida, clique em **criar**.  
  

Agora você criou uma infra-estrutura básica de serviços de área de trabalho remota. Se você precisa criar uma implantação altamente disponível, você pode adicionar um [cluster do agente de conexão](rds-connection-broker-cluster.md) ou um [segundo servidor Host de sessão de área de trabalho remota](rds-scale-rdsh-farm.md).

