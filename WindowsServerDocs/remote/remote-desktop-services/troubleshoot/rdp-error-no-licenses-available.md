---
title: Os clientes não conseguem se conectar e veem o erro "Nenhuma licença disponível"
description: Solução do erro “Nenhuma licença disponível” com a conexão com a área de trabalho remota
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: f163908277b2ab8cc0e3bfbcbc4ae5e8001a2b4a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857169"
---
# <a name="clients-cant-connect-and-see-no-licenses-available-error"></a>Os clientes não conseguem se conectar e veem o erro "Nenhuma licença disponível"

Essa situação se aplica a implantações que incluem um servidor RDSH e um servidor de Licenciamento de Área de Trabalho Remota.

Primeiro, identifique qual comportamento os usuários estão vendo:

- A sessão foi desconectada porque não há licenças disponíveis ou nenhum servidor de licença está disponível.
- O acesso foi negado devido a um erro de segurança.

Entre no Host da Sessão RD como um administrador de domínio e abra o Diagnosticador de Licença da RD. Procure mensagens como a seguinte:

  - O período de cortesia do servidor Host da Sessão da Área de Trabalho Remota expirou, mas o servidor Host da Sessão da Área de Trabalho Remota não foi configurado com nenhum servidor de licença. Conexões com o servidor Host da Sessão da Área de Trabalho Remota serão negadas, a menos que um servidor de licença esteja configurado para o servidor Host da Sessão da Área de Trabalho Remota.
  - O servidor de licença \<nome do computador\> não está disponível. Isso pode ser causado por problemas de conectividade de rede, pelo serviço de Licenciamento da Área de Trabalho Remota ter sido interrompido no servidor de licença ou pelo Licenciamento de Área de Trabalho Remota não estar disponível.

Esses problemas tendem a ser associados com as seguintes mensagens de usuário:

  - A sessão remota foi desconectada porque não há licenças de acesso para cliente de Área de Trabalho Remota disponíveis para este computador.
  - A sessão remota foi desconectada porque não há servidores de licença Área de Trabalho Remota disponíveis para fornecer uma licença.

Nesse caso, [configure o serviço de Licenciamento de Área de Trabalho Remota](#configure-the-rd-licensing-service).

Se o Diagnosticador de Licença RD listar outros problemas, tais como "O componente de protocolo RDP X.224 detectou um erro no fluxo do protocolo e desconectou o cliente," poderá haver um problema que afeta os certificados de licença. Esses problemas tendem a ser associados com mensagens de usuário, tais como as seguintes:

Devido a um erro de segurança, o cliente não pôde se conectar ao servidor Host da Sessão da Área de Trabalho Remota. Após verificar se você está conectado à rede, tente se conectar ao servidor novamente.

Nesse caso, [atualize as chaves do Registro de Certificado X509](#refresh-the-x509-certificate-registry-keys).

## <a name="configure-the-rd-licensing-service"></a>Configurar o serviço de Licenciamento de Área de Trabalho Remota

O procedimento a seguir usa o Gerenciador do Servidor para fazer as alterações de configuração. Para saber mais sobre como configurar e usar o Gerenciador do Servidor, confira [Gerenciador do Servidor](../../../administration/server-manager/server-manager.md)

1. Abra **Gerenciador do Servidor** e navegue até **Serviços de Área de Trabalho Remota**.
2. Em **Visão Geral da Implantação**, selecione **Tarefas** e, em seguida, selecione **Editar Propriedades de Implantação**.
3. Selecione **Licenciamento de Área de Trabalho Remota** e, em seguida, selecione o modo de licenciamento apropriado para sua implantação (**Por Dispositivo** ou **Por Usuário**).
4. Insira o FQDN (nome de domínio totalmente qualificado) do seu servidor de Licença de Área de Trabalho Remota e, em seguida, selecione **Adicionar**.
5. Se você tiver mais de um servidor de Licença de Área de Trabalho Remota, repita a etapa 4 para cada servidor. 
    ![Opções de configuração de servidor de Licença de Área de Trabalho Remota no Gerenciador do Servidor.](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

## <a name="refresh-the-x509-certificate-registry-keys"></a>Atualizar as chaves do Registro de Certificado X509

> [!IMPORTANT]  
> Siga as instruções desta seção com cuidado. Poderão ocorrer problemas sérios se o Registro for modificado incorretamente. Antes de começar a modificar o Registro, [faça backup dele](https://support.microsoft.com/help/322756) para que você possa restaurá-lo caso algo dê errado.

Para resolver esse problema, faça backup das chaves do Registro de Certificado X509 e então remova-as, reinicie o computador e, em seguida, reative o servidor de Licenciamento de Área de Trabalho Remota. Siga estas etapas.

> [!NOTE]
> Execute o seguinte procedimento em cada um dos servidores do RDSH.

Veja como reativar o servidor de Licenciamento de Área de Trabalho Remota:

1. Abra o Editor do Registro e navegue até **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Controle\\Servidor Host da Sessão da Área de Trabalho Remota\\RCM**.
2. No menu Registro, selecione **Exportar Arquivo do Registro**.
3. Insira **exportado – Certificado** na caixa **Nome do arquivo** e, em seguida, selecione **Salvar**.
4. Clique com o botão direito do mouse em cada um dos valores a seguir, selecione **Excluir** e, em seguida, selecione **Sim** para verificar a exclusão:  
      - **Certificado**
      - **Certificado X509**
      - **ID do Certificado X509**
      - **Certificado X509 2**
5. Saia do Editor do Registro e reinicie o servidor RDSH.