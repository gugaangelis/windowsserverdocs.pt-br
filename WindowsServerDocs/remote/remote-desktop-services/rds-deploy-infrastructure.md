---
title: Implantar o seu ambiente de Área de Trabalho Remota
ms.prod: windows-server
description: Etapas básicas para implantar um ambiente de Área de Trabalho Remota.
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.topic: article
author: lizap
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 31bb6afaca92b36453d4565c1f79aae35a6f0900
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855749"
---
# <a name="deploy-your-remote-desktop-environment"></a>Implantar o seu ambiente de Área de Trabalho Remota

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Use as etapas a seguir para implantar os servidores de Área de Trabalho Remota em seu ambiente. É possível instalar as funções de servidor em computadores físicos ou máquinas virtuais, dependendo de onde estão sendo criadas: no local, baseadas na nuvem ou em ambiente híbrido. 

Se você estiver usando máquinas virtuais para qualquer um dos servidores dos Serviços de Área de Trabalho Remota, verifique se [essas máquinas virtuais estão preparadas](rds-prepare-vms.md).
  
  
1.  Adicione todos os servidores que pretende usar dos Serviços de Área de Trabalho Remota para o Gerenciador do Servidor:  
    1.  No Gerenciador do Servidor, clique em **Gerenciar** > **Adicionar Servidores**.  
    2.  Clique em **Localizar Agora**.  
    3.  Clique em cada servidor na implantação (por exemplo, Contoso Cb1, Contoso-WebGw1 e Contoso-Sh1) e clique em **OK**.  
2.  Crie uma implantação baseada em sessão para implantar os componentes dos Serviços de Área de Trabalho Remota:  
    1.  No Gerenciador do Servidor, clique em **Gerenciar** > **Adicionar Funções e Recursos**.  
    2.  Clique em **Instalação de Serviços de Área de Trabalho Remota**, **Implantação Padrão** e **Implantação de desktop baseada em sessão**.  
    3.  Selecione os servidores apropriados para o servidor do Agente de Conexão de Área de Trabalho Remota, o servidor de Acesso via Web à Área de Trabalho Remota e o servidor do Host da Sessão da Área de Trabalho Remota (por exemplo, Contoso-Cb1, Contoso-WebGw1 e Contoso-SH1, respectivamente).  
    4.  Selecione **Reiniciar cada servidor de destino automaticamente, se necessário** e clique em **Implantar**.  
    5.  Aguarde até que a implantação seja concluída com êxito  
3.  Adicione o Servidor de Licenças RD:  
    1.  No Gerenciador do Servidor, clique em **Serviços de Área de Trabalho Remota > Visão geral > +Licenciamento de Área de Trabalho Remota**.  
    2.  Selecione a máquina virtual em que o servidor de licenças da área de trabalho remota será instalado (por exemplo, Contoso-Cb1).  
    3.  Clique em **Avançar** e em **Adicionar**.  
4.  Ative o Servidor de Licenças RD e adicione-o ao grupo Servidores de Licença:  
    1.  No Gerenciador do Servidor, clique em **Ferramentas > Serviços de Terminal > Gerenciador de Licenciamento de Área de Trabalho Remota**.  
    2.  No Gerenciador de Licenciamento de Área de Trabalho Remota, selecione o servidor e clique em **Ação > Ativar Servidor**.  
    3.  Aceite os valores padrão no Assistente para Ativação do Servidor. Continue aceitando valores padrão até chegar à página **Informações sobre a empresa**. Verifique as informações da sua empresa.  
    4.  Aceite os padrões para as páginas restantes até a página final. Desmarque **Iniciar Assistente para Instalação de Licenças agora** e clique em **Concluir**.  
    5.  Clique em **Ação > Examinar Configuração > Adicionar ao Grupo > OK**. Insira as credenciais para um usuário no grupo Administradores do DC AAD e registre como SCP. Esta etapa pode não funcionar se você estiver usando o Azure AD Domain Services, porém, é possível ignorar os avisos ou erros.  
5.  Adicione o servidor de Gateway de Área de Trabalho Remota e o nome do certificado:  
    1.  No Gerenciador do Servidor, clique em **Serviços de Área de Trabalho Remota > Visão geral > +Gateway de Área de Trabalho Remota**.  
    2.  No assistente para Adicionar Servidores de Gateway de Área de Trabalho Remota, selecione a máquina virtual em que você deseja instalar o servidor de Gateway de Área de Trabalho Remota (por exemplo, Contoso-WebGw1).  
    3.  Insira o nome do certificado SSL para o servidor de Gateway de Área de Trabalho Remota usando o FQDN (Nome DNS totalmente qualificado) externo do servidor de Gateway de Área de Trabalho Remota. No Azure, esse é o rótulo **Nome DNS** e usa o formato servicename.location.cloudapp.azure.com. Por exemplo, contoso.westus.cloudapp.azure.com.  
    4.  Clique em **Avançar** e em **Adicionar**.
6.  Crie e instale certificados autoassinados para os servidores de Gateway de Área de Trabalho Remota e de Agente de Conexão de Área de Trabalho Remota.

       > [!NOTE]
       > Se você estiver fornecendo e instalando certificados de uma autoridade de certificação confiável, execute os procedimentos de etapa h até a k para cada função. Você precisará ter o arquivo .pfx disponível para cada um desses certificados.
       
    1.  No Gerenciador do Servidor, clique em **Serviços de Área de Trabalho Remota > Visão geral > Tarefas > Editar Propriedades de Implantação**.  
    2.  Expanda **Certificados** e role para baixo até a tabela. Clique em **Gateway de Área de Trabalho Remota > Criar novo certificado**.  
    3.  Digite o nome do certificado usando o FQDN externo do servidor de Gateway de Área de Trabalho Remota (por exemplo, contoso.westus.cloudapp.azure.com) e, em seguida, digite a senha.  
    4.  Selecione **Armazenar este certificado** e navegue até a pasta compartilhada que você criou para certificados em uma etapa anterior. (Por exemplo, \Contoso-Cb1\Certificates).  
    5.  Insira um nome de arquivo para o certificado (por exemplo, ContosoRdGwCert) e clique em **Salvar**.  
    6.  Selecione **Permitir que o certificado seja adicionado ao repositório de certificados de Autoridades de Certificação Raiz Confiáveis nos computadores de destino** e clique em **OK**.  
    7.  Clique em **Aplicar** e aguarde até que o certificado seja aplicado com êxito ao servidor de Gateway de Área de Trabalho Remota.  
    8.  Clique em **Acesso via Web à Área de Trabalho Remota > Selecionar certificado existente**.  
    9.  Navegue até o certificado criado para o servidor de Gateway de Área de Trabalho Remota (por exemplo, ContosoRdGwCert) e clique em **Abrir**.  
    10. Insira a senha do certificado, selecione **Permitir que o certificado seja adicionado ao repositório de Certificados Raiz Confiáveis nos computadores de destino** e clique em **OK**.  
    11. Clique em **Aplicar** e aguarde até que o certificado seja aplicado com êxito ao servidor de Acesso via Web à Área de Trabalho Remota.  
    12. Repita as subetapas de 1 a 11 para **Agente de Conexão de Área de Trabalho Remota – Habilitar Logon Único** e **Agente de Conexão de Área de Trabalho Remota – Publicando serviços** usando o FQDN interno do servidor de Agente de Conexão de Área de Trabalho Remota para o nome do novo certificado (por exemplo, Contoso-Cb1.Contoso.com).  
7.  Exporte os certificados públicos autoassinados e copie-os em um computador cliente. Se você estiver usando certificados de uma autoridade de certificação confiável, ignore essa etapa.  
    1.  Inicie o certlm.msc.  
    2.  Expanda **Pessoal** e clique em **Certificados**.  
    3.  No painel do lado direito, clique com o botão direito do mouse no certificado do Agente de Conexão de Área de Trabalho Remota destinado para autenticação de cliente, por exemplo, **Contoso-Cb1.Contoso.com**.  
    4.  Clique em **Todas as Tarefas > Exportar**.  
    5.  Adote as opções padrão no Assistente para Exportação de Certificados aceitando os padrões até chegar à página **Arquivo a ser Exportado**.  
    6.  Navegue até a pasta compartilhada que você criou para certificados, por exemplo \Contoso-Cb1\Certificates.  
    7.  Insira um nome de arquivo, por exemplo, ContosoCbClientCert, e clique em **Salvar**.  
    8.  Clique em **Avançar**e em **Concluir**.  
    9.  Repita as subetapas de 1 a 8 para o certificado de Gateway de Área de Trabalho Remota e da Web, (por exemplo contoso.westus.cloudapp.azure.com), fornecendo ao certificado exportado um nome de arquivo apropriado, por exemplo **ContosoWebGwClientCert**.  
    10. No Explorador de Arquivos, navegue até a pasta onde os certificados estão armazenados, por exemplo \Contoso-Cb1\Certificates.  
    11. Selecione os dois certificados de cliente exportados, clique neles com botão direito do mouse e clique em **Copiar**.  
    12. Cole os certificados no computador cliente local.  
8.  Configure as propriedades de implantação do Gateway de Área de Trabalho Remota e do Licenciamento de Área de Trabalho Remota:  
    1.  No Gerenciador do Servidor, clique em **Serviços de Área de Trabalho Remota > Visão geral > Tarefas > Editar Propriedades de Implantação**.  
    2.  Expanda **Gateway de Área de Trabalho Remota** e desmarque a opção **Ignorar servidor de Gateway de Área de Trabalho Remota para endereços locais**.  
    3.  Expanda **Licenciamento de Área de Trabalho Remota** e selecione **Por Usuário**  
    4.  Clique em **OK**.  
10. Crie um conjunto de sessões. Estas etapas criam um conjunto básico. Confira [Criar um conjunto de Serviços de Área de Trabalho Remota para execução de desktops e aplicativos](rds-create-collection.md) para obter mais informações sobre coleções.
 
    1.  No Gerenciador do Servidor, clique em **Serviços de Área de Trabalho Remota > Conjuntos > Tarefas > Criar Conjuntos de Sessões**.  
    2.  Digite um nome para o conjunto (por exemplo, ContosoDesktop).  
    3.  Selecione um Servidor do Host da Sessão de Área de Trabalho Remota (Contoso-Sh1), aceite os grupos de usuários padrão (Contoso\Domain Users) e insira o caminho UNC para os discos de perfil do usuário criados acima (\Contoso-Cb1\UserDisks).  
    4.  Defina um tamanho máximo e clique em **Criar**.  
  

Agora você criou uma infraestrutura básica de Serviços de Área de Trabalho Remota. Se precisar criar uma implantação altamente disponível, você poderá adicionar um [cluster do agente de conexão](rds-connection-broker-cluster.md) ou um [segundo servidor do Host da Sessão de Área de Trabalho Remota](rds-scale-rdsh-farm.md).

