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
ms.openlocfilehash: a5a56d56038d94869c5246f8d4d3eae2796616a3
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297435"
---
# Implantar o seu ambiente de Área de Trabalho Remota

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016

Use as seguintes etapas para implantar os servidores de área de trabalho remota no seu ambiente. Você pode instalar as funções de servidor em computadores físicos ou máquinas virtuais, dependendo se você estiver criando um local, com base em nuvem ou ambiente híbrido. 

Se você estiver usando máquinas virtuais para qualquer um dos servidores de serviços de área de trabalho remota, verifique se que você tiver [preparado as máquinas virtuais](rds-prepare-vms.md).
  
  
1.  Adicione todos os servidores que você vai usar para os serviços de área de trabalho remota ao Gerenciador do servidor:  
    1.  No Gerenciador de servidores clique em **Gerenciar gt _ adicionar servidores**.  
    2.  Clique em **Localizar agora**.  
    3.  Clique em cada servidor na implantação (por exemplo, Contoso Cb1, Contoso WebGw1 e Sh1 Contoso) e **Okey**.  
2.  Crie uma implantação com base em sessão para implantar os componentes de serviços de área de trabalho remota:  
    1.  No Gerenciador do servidor, clique em **Gerenciar gt _ adicionar funções e recursos**.  
    2.  Clique em **instalação de serviços de área de trabalho remota**, **Implantação padrão**e **implantação da área de trabalho baseada em sessão**.  
    3.  Selecione os servidores apropriados para o servidor do agente de Conexão de área de trabalho remota, o servidor de acesso via Web e o servidor Host de sessão de área de trabalho remota (por exemplo, Contoso Cb1, Contoso WebGw1 e Contoso-SH1, respectivamente).  
    4.  Selecione **reiniciar o servidor de destino automaticamente se necessário**e, em seguida, clique em **Deploy**.  
    5.  Aguarde até que a implantação concluída com êxito  
3.  Adicione o servidor de licenças de área de trabalho remota:  
    1.  No Gerenciador do servidor, clique em **Serviços de área de trabalho remota gt _ visão geral gt _ + licenciamento de área de trabalho remota**.  
    2.  Selecione a máquina virtual em que o servidor de licenças de área de trabalho remota será instalado (por exemplo, Contoso Cb1).  
    3.  Clique em **Avançar**e, em seguida, clique em **Adicionar**.  
4.  Ativar o servidor de licenças de área de trabalho remota e adicioná-lo ao grupo de servidores de licenças:  
    1.  No Gerenciador do servidor, clique em **Ferramentas gt _ serviços de Terminal gt _ Gerenciador de licenciamento de área de trabalho remota**.  
    2.  No Gerenciador de licenciamento de área de trabalho remota, selecione o servidor e, em seguida, clique em **ação gt _ Activate Server**.  
    3.  Aceite os valores padrão no Assistente de servidor ativar aceitando padrões até chegar à página **informações da empresa** . Em seguida, insira as informações da sua empresa.  
    4.  Aceite os padrões para as páginas restantes até que a página final. Desmarque **Iniciar instalar licenças assistente agora**e, em seguida, clique em **Concluir**.  
    5.  Clique em **ação gt _ Examinar Configuração gt _ adicionar ao grupo gt _ Okey**. Insira as credenciais para um usuário no grupo Administradores do AAD DC e registrar como SCP. Esta etapa pode não funcionar se você estiver usando os serviços de domínio do Azure AD, mas você pode ignorar os avisos ou erros.  
5.  Adicione o nome de servidor e o certificado de Gateway de área de trabalho remota:  
    1.  No Gerenciador do servidor, clique em **Serviços de área de trabalho remota gt _ visão geral gt _ + Gateway da área de trabalho remota**.  
    2.  No Assistente para adicionar servidores de Gateway de área de trabalho remota, selecione a máquina virtual em que você deseja instalar o servidor de Gateway de área de trabalho remota (por exemplo, Contoso-WebGw1).  
    3.  Insira o nome do certificado SSL do servidor de Gateway de área de trabalho remota usando o externo totalmente DNS nome qualificado (FQDN) do servidor Gateway de área de trabalho remota. No Azure, esse é o **nome DNS** de rótulo e usa o formato servicename.location.cloudapp.azure.com. Por exemplo, contoso.westus.cloudapp.azure.com.  
    4.  Clique em **Avançar**e, em seguida, clique em **Adicionar**.
6.  Crie e instale os certificados autoassinados para os servidores do agente de Conexão de área de trabalho remota e Gateway de área de trabalho remota.

       > [!NOTE]
       > Se você estiver fornecendo e instalando os certificados de autoridade de certificação confiáveis, execute os procedimentos de h etapa a etapa k para cada função. Você precisará ter o arquivo. pfx disponível para cada um desses certificados.
       
    1.  No Gerenciador do servidor, clique em **Serviços de área de trabalho remota gt _ visão geral gt _ tarefas gt _ Editar propriedades de implantação**.  
    2.  Expanda **certificados**e, em seguida, role até a tabela. Clique em **Gateway RD gt _ criar novo certificado**.  
    3.  Insira o nome do certificado, usando o FQDN externo do servidor Gateway de área de trabalho remota (por exemplo, contoso.westus.cloudapp.azure.com) e, em seguida, insira a senha.  
    4.  Selecione **armazenamento esse certificado** e, em seguida, navegue até a pasta compartilhada que você criou para certificados em uma etapa anterior. (Por exemplo, \Contoso Cb1\Certificates.)  
    5.  Insira um nome de arquivo para o certificado (por exemplo, ContosoRdGwCert) e, em seguida, clique em **Salvar**.  
    6.  Selecione **Permitir o certificado a ser adicionado ao repositório de certificados autoridades de certificação raiz confiáveis nos computadores de destino**e, em seguida, clique em **Okey**.  
    7.  Clique em **Aplicar**e, em seguida, aguarde o certificado a ser aplicada com êxito para o servidor de Gateway de área de trabalho remota.  
    8.  Clique em **acesso via Web gt _ Selecionar certificado existente**.  
    9.  Navegue até o certificado criado para o servidor de Gateway de área de trabalho remota (por exemplo, ContosoRdGwCert) e, em seguida, clique em **Abrir**.  
    10. Insira a senha do certificado, selecione **permitem que o certificado a ser adicionado ao repositório de certificados de raiz confiáveis nos computadores de destino**e, em seguida, clique em **Okey**.  
    11. Clique em **Aplicar**e, em seguida, aguarde o certificado a ser aplicada com êxito para o servidor de acesso via Web.  
    12. Repita sub-etapas 1-11 para o **Agente de Conexão de área de trabalho remota - serviços de publicação**, usando o FQDN interno do servidor de agente de Conexão de área de trabalho remota para o novo nome do certificado (por exemplo e o **Agente de Conexão de área de trabalho remota - habilitar logon único** Contoso-Cb1.Contoso.com).  
7.  Exportar certificados autoassinados públicos e copiá-los para um computador cliente. Se você estiver usando certificados de uma autoridade de certificado confiável, você pode ignorar esta etapa.  
    1.  Inicie certlm.  
    2.  Expanda **pessoal**e, em seguida, clique em **certificados**.  
    3.  No painel direito, clique com botão direito o certificado de agente de Conexão de área de trabalho remota se destina à autenticação de cliente, por exemplo **Cb1.Contoso.com Contoso**.  
    4.  Clique em **todas as tarefas gt _ exportação**.  
    5.  Aceite que as opções padrão do Assistente para exportação de certificado aceitam padrões até chegar à página do **arquivo de exportação** .  
    6.  Navegue até a pasta compartilhada que você criou para certificados, por exemplo \Contoso-Cb1\Certificates.  
    7.  Insira um nome de arquivo, por exemplo ContosoCbClientCert e, em seguida, clique em **Salvar**.  
    8.  Clique em **Avançar**e, em seguida, clique em **Concluir**.  
    9.  Repita sub-etapas 1 a 8 para o Gateway de área de trabalho remota e o certificado da Web, (por exemplo contoso.westus.cloudapp.azure.com), dando o certificado exportado um nome de arquivo apropriado, por exemplo **ContosoWebGwClientCert**.  
    10. No Explorador de arquivos, navegue até a pasta onde os certificados são armazenados, por exemplo \Contoso-Cb1\Certificates.  
    11. Selecione os dois certificados de cliente exportado, em seguida, clique com botão direito-los e clique em **Copiar**.  
    12. Cole os certificados no computador cliente local.  
8.  Configure as propriedades de implantação do Gateway de área de trabalho remota e licenciamento de área de trabalho remota:  
    1.  No Gerenciador do servidor, clique em **Serviços de área de trabalho remota gt _ visão geral gt _ tarefas gt _ Editar propriedades de implantação**.  
    2.  Expanda o **Gateway de área de trabalho remota** e desmarque a opção de **servidor de Gateway de área de trabalho remota Bypass para endereços locais** .  
    3.  Expanda o **Licenciamento de área de trabalho remota** e selecione **Por usuário**  
    4.  Clique em **OK**.  
10. Crie uma coleção de sessão. Estas etapas criam um conjunto básico. Confira a [criar uma coleção de serviços de área de trabalho remota para desktops e aplicativos sejam executados](rds-create-collection.md) para obter mais informações sobre coleções.
 
    1.  No Gerenciador do servidor, clique em **Serviços de área de trabalho remota gt _ coleções gt _ tarefas gt _ Criar coleção de sessão**.  
    2.  Insira uma nome (por exemplo, ContosoDesktop) da coleção.  
    3.  Selecione um servidor de Host de sessão de área de trabalho remota (Sh1 Contoso), aceite os grupos de usuários padrão (Contoso\Domain usuários) e digite o caminho da convenção de nomenclatura Universal (UNC) para os discos de perfil de usuário criados acima (\Contoso-Cb1\UserDisks).  
    4.  Definir um tamanho máximo e, em seguida, clique em **criar**.  
  

Agora você criou uma infraestrutura básica de serviços de área de trabalho remota. Se você precisar criar uma implantação altamente disponível, você pode adicionar um [cluster do agente de conexão](rds-connection-broker-cluster.md) ou um [segundo servidor de Host de sessão de área de trabalho remota](rds-scale-rdsh-farm.md).

