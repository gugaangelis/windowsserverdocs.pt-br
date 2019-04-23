---
title: Recuperação de floresta do AD - serviço de configuração de servidor DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2c37428a0fb685e6a7fa4875366f3cd13401bd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842957"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>Recuperação de floresta do AD – Configurando o serviço servidor DNS

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Se a função de servidor DNS não estiver instalada no controlador de domínio que você restaurar do backup, você deve instalar e configurar o servidor DNS. 

## <a name="install-and-configure-the-dns-server-service"></a>Instalar e configurar o serviço servidor DNS

Conclua esta etapa para cada controlador de domínio restaurado que não está em execução como um servidor DNS depois que a restauração for concluída. 

> [!NOTE]
> Se o controlador de domínio restaurado do backup está executando o Windows Server 2008 R2, você deve conectar o controlador de domínio a uma rede isolada para instalar o servidor DNS. Em seguida, conecte-se cada um dos servidores DNS restaurados a uma rede isolada mutuamente compartilhada. Execute repadmin /replsum para verificar que a replicação está funcionando entre os servidores DNS restaurados. Depois de verificar a replicação, você pode se conectar as controladores de domínio restaurados para a rede de produção se a função de servidor DNS já estiver instalada, você pode aplicar um hotfix que torna possível para um servidor DNS iniciar enquanto o servidor não está conectado a nenhuma rede. Você deve corrigir o hotfix na imagem de instalação do sistema operacional durante os processos de compilação automatizado. Para obter mais informações sobre o hotfix, consulte [artigo 975654](https://go.microsoft.com/fwlink/?LinkId=184691) na Base de dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=184691). 

Conclua as etapas de instalação e configuração abaixo.

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>A instalação e o serviço servidor DNS usando o Gerenciador do servidor  

1. Abra o Gerenciador do servidor e clique em **adicionar funções e recursos**. 
2. No Assistente para Adicionar Funções, se for exibida a página **Antes de Começar**, clique em **Avançar**. 
3. Sobre o **tipo de instalação** , selecione **baseado em função ou instalação baseada em recurso** e clique em **próxima**.
4. Sobre o **seleção de servidor** tela, selecione o servidor e clique em **próxima**.
5. Sobre o **funções de servidor** , selecione **servidor DNS**, se solicitado que você clique em **adicionar recursos** e clique em **Avançar**.
6. Sobre o **recursos** tela clique **próxima**.
7. Leia as informações sobre o **servidor DNS** página e, em seguida, clique em **próxima**.
   ![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8. Sobre o **confirmação** página, verifique se a função servidor DNS será instalado e, em seguida, clique em **instalar**. 

### <a name="to-configure-the-dns-server-service"></a>Para configurar o serviço servidor DNS

1. Abra o Gerenciador do servidor, clique em **ferramentas** e clique em **DNS**.
   ![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns2.png)
2. Crie zonas DNS para os mesmos nomes de domínio DNS que foram hospedadas nos servidores DNS antes do mal funcionamento crítico. Para obter mais informações, consulte Adicionar uma zona de pesquisa direta ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)).
3. Configure os dados DNS conforme se encontravam antes do mal funcionamento crítico. Por exemplo:  

   - Configure zonas DNS a ser armazenado no AD DS. Para obter mais informações, consulte alterar o tipo de zona ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)).
   - Configure a zona DNS que está autorizada para registros de recursos de localizador (localizador de controlador de domínio) de controlador de domínio permitir que a atualização dinâmica segura. Para obter mais informações, consulte permitir somente atualizações dinâmicas seguras ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)).

4. Certifique-se de que a zona DNS pai contém registros de recurso delegação (nome do servidor (NS) e cola recursos do host (A) registros) para a zona filho que é hospedado neste servidor DNS. Para obter mais informações, consulte Criar uma delegação de zona ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)).
5. Depois de configurar o DNS, você pode acelerar o registro dos registros de NETLOGON.

   > [!NOTE]
   > Atualizações dinâmicas seguras só funcionam quando um servidor de catálogo global está disponível. 

   No prompt de comando, digite o seguinte comando e pressione ENTER:  

   **net stop netlogon**  

6. Digite o seguinte comando e pressione ENTER:  

   **net start netlogon**  

   ![Servidor DNS](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
