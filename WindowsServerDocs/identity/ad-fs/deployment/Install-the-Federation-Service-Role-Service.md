---
ms.assetid: e33673ff-ea1c-4476-a549-3bf5899a47dd
title: Instalar o serviço de função Serviço de Federação
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7ed7caad187181b4ad52cf4032d6343a3353c9f7
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519975"
---
# <a name="install-the-federation-service-role-service"></a>Instalar o serviço de função Serviço de Federação

Agora que você configurou corretamente um computador com os aplicativos e certificados de pré-requisito, você está pronto para instalar o serviço de função de Serviço de Federação do Serviços de Federação do Active Directory (AD FS) (AD FS). Quando você instala o Serviço de Federação em um computador, esse computador torna-se um servidor de Federação.

> [!NOTE]
> Para o design de SSO (logon único) da Web federado, você deve ter pelo menos um servidor de Federação na organização do parceiro de conta e pelo menos um servidor de Federação na organização do parceiro de recurso. Para obter mais informações, consulte [Onde colocar um servidor de federação](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807127(v=ws.11)).

Você pode usar o procedimento a seguir para instalar o serviço de função Serviço de Federação do AD FS em um computador que se tornará o primeiro servidor de Federação ou em um computador que se tornará um servidor de Federação para um farm de servidores de Federação existente.

## <a name="prerequisites"></a>Pré-requisitos
Verifique se um certificado SSL com a chave privada já foi instalado ou importado para o repositório de certificados local (repositório pessoal) antes de iniciar este procedimento. Se você estiver usando um certificado de autenticação de tokens emitido por uma autoridade de certificação (CA), verifique se um certificado de autenticação de tokens com a chave privada já foi instalado ou importado no repositório de certificados local (repositório pessoal) antes de iniciar este procedimento. Como alternativa, você pode criar um certificado de assinatura de token autoassinado usando o assistente para adicionar funções, conforme descrito neste procedimento. Para obter mais informações sobre certificados de assinatura de token, consulte [requisitos de certificado para servidores de Federação](../design/certificate-requirements-for-federation-servers.md).

A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento. Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).

#### <a name="to-install-the-federation-service-role-service"></a>Para instalar o serviço de função Serviço de Federação

1. Na tela **Iniciar** , digite**Gerenciador do servidor**e pressione Enter.

2. Clique em **Gerenciar** e clique em **Adicionar Funções e Recursos** para iniciar o Assistente para Adicionar Funções e Recursos.

3. Na página **Antes de começar** , clique em **Avançar**.

4. Na página **Selecionar tipo de instalação**, clique em **Instalação baseada em Função ou Recurso** e clique em **Avançar**.

5. Na página **Selecionar servidor de destino**, clique em **Selecionar um servidor no pool de servidores**, verifique se o computador alvo está selecionado e, depois, clique em **Avançar**.

6. Na página **Selecionar funções do servidor**, clique em **Serviços de Federação do Active Directory** e, depois, clique em Avançar.

    > [!NOTE]
    > Se for solicitado instalar recursos adicionais do Framework .NET adicional ou do Serviço de Ativação do Processo do Windows, clique em **Adicionar Recursos** para instalar eles.

7. Na página **Selecionar recursos**, verifique que os recursos estejam definidos, e em seguida clique em **Avançar**.

8. Na página **Serviço de Federação do Active Directory (AD FS)**, clique em **Avançar**.

9. Na página **Selecionar serviços de função**, marque a caixa de seleção **Serviço de Federação** e clique em **Avançar**.

10. Na página **Função de Servidor Web (IIS)**, clique em **Avançar**.

11. Na página **Selecionar serviços de função**, clique em **Avançar**.

12. Após ter verificado as informações na página **Confirmar as seleções de instalação**, marque a caixa de seleção **Reiniciar o servidor de destino automaticamente se for necessário**, e em seguida clique em **Instalar**.

13. Na página **Progresso da instalação**, verifique se tudo está instalado corretamente e, depois, clique em **Fechar**.