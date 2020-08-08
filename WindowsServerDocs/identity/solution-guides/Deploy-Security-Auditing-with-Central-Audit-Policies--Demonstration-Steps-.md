---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: Implantar a auditoria de segurança com as políticas de auditoria central (etapas de demonstração)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 9fed82f80c03c0d8490dc88b1ec7c7d7c48a4c06
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956073"
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>Implantar a auditoria de segurança com as políticas de auditoria central (etapas de demonstração)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Nesse cenário, você auditará o acesso a arquivos na pasta de documentos financeiros usando a política de Finanças que você criou em [implantar uma política de acesso Central &#40;etapas de demonstração&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md). Se um usuário que não estiver autorizado a acessar a pasta tentar acessá-la, a atividade será capturada no visualizador de eventos.
As etapas a seguir são necessárias para testar este cenário.

|Tarefa|Descrição|
|--------|---------------|
|[Configurar o acesso a objetos globais](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|Nesta etapa, configure a política de acesso a objetos globais no controlador de domínio.|
|[Atualizar Política de Grupo configurações](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|Entre no servidor de arquivos e aplique a atualização de Política de Grupo.|
|[Verificar se a política de acesso a objetos globais foi aplicada](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|Exiba os eventos relevantes no visualizador de eventos. Os eventos devem incluir metadados para o tipo de documento e o país.|

## <a name="configure-global-object-access-policy"></a><a name="BKMK_1"></a>Configurar a política de acesso a objetos globais
Nesta etapa, configure a política de acesso a objetos globais no controlador de domínio.

#### <a name="to-configure-a-global-object-access-policy"></a>Para configurar uma política de acesso a objetos globais

1. Entre no controlador de domínio DC1 como CONTOSO\Administrator com a senha <strong>pass@word1</strong> .

2. No Gerenciador do Servidor, aponte para **Ferramentas** e clique em **Gerenciamento de Política de Grupo**.

3. Na árvore do console, clique duas vezes em **Domínios**, clique duas vezes em **contoso.com**, clique em **Contoso** e clique duas vezes em **Servidores de Arquivos**.

4. Clique com o botão direito do mouse em **FlexibleAccessGPO** e clique em **Editar**.

5. Clique duas vezes em **Configuração do Computador**, clique duas vezes em **Políticas** e clique duas vezes em **Configurações do Windows**.

6. Clique duas vezes em **Configurações de Segurança**, clique duas vezes em **Configuração Avançada de Política de Auditoria** e clique duas vezes em **Políticas de Auditoria**.

7. Clique duas vezes em **Acesso a Objeto** e clique duas vezes em **Sistema de Arquivos de Auditoria**.

8. Marque a caixa de seleção **Configurar estes eventos**, marque as caixas de seleção **Êxito** e **Falha** e clique em **OK**.

9. No painel de navegação, clique duas vezes em **Auditoria de Acesso a Objetos Globais** e clique duas vezes em **Sistema de arquivos**.

10. Marque a caixa de seleção **Definir esta configuração de política** e clique em **Configurar**.

11. Na caixa **Configurações de Segurança Avançadas de SACL de Arquivo Global**, clique em **Adicionar**, clique em **Selecionar uma entidade de segurança**, digite **Todos** e clique em **OK**.

12. Na caixa **Entrada de Auditoria para SACL de Arquivo Global**, selecione **Controle total** na caixa **Permissões**.

13. Na seção **Adicionar uma condição:**, clique em **Adicionar uma condição** e selecione nas listas suspensas[**Recurso**] [**Departamento**] [**Qualquer um**] [**Valor**] [**Finanças**].

14. Clique em **OK** três vezes para concluir a definição da configuração de política de auditoria de acesso a objeto global.

15. No painel de navegação, clique em **Acesso a Objeto** e, no painel de resultados, clique duas vezes em **Auditoria de Manipulação de Identificador**. Clique em **Configurar estes eventos de auditoria**, **Êxito** e **Falha**, clique em **OK** e feche o GPO de acesso flexível.

## <a name="update-group-policy-settings"></a><a name="BKMK_2"></a>Atualizar as configurações de Política de Grupo
Nesta etapa, atualize as configurações de Política de Grupo depois de criar a política de auditoria.

#### <a name="to-update-group-policy-settings"></a>Para atualizar as configurações de Política de Grupo.

1. Entre no servidor de arquivos, FILE1 como contoso\Administrator, com a senha <strong>pass@word1</strong> .

2. Pressione a tecla do Windows+R e digite **cmd** para abrir uma janela do Prompt de Comando.

   > [!NOTE]
   > Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.

3. Digite **gpupdate /force** e pressione ENTER.

## <a name="verify-that-the-global-object-access-policy-has-been-applied"></a><a name="BKMK_3"></a>Verificar se a política de acesso a objetos globais foi aplicada
Depois que as configurações de Política de Grupo forem aplicadas, você poderá verificar se as configurações de política de auditoria foram aplicadas corretamente.

#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>Para verificar se a política de acesso a objetos globais foi aplicada

1.  Entre no computador cliente, CLIENT1 como Contoso\MReid. Navegue até a pasta HYPERLINK "file:/// \\ \\ \\ \ ID_AD_FILE1 \\ \Finance" \\ \ FILE1\Finance documentos e modifique o documento do Word 2.

2.  Entre no servidor de arquivos, FILE1, como contoso\administrator. Abra o Visualizador de Eventos, navegue até **Logs do Windows**, selecione **Segurança** e confirme se suas atividades resultaram nos eventos de auditoria **4656** e **4663** (embora você não tenha definido SACLs de auditoria explícitas nos arquivos ou pastas que criou, modificou e excluiu).

> [!IMPORTANT]
> Um novo evento de logon é gerado no computador onde o recurso está localizado, em nome do usuário para quem o acesso efetivo está sendo verificado. Durante a análise dos logs de auditoria de segurança da atividade de logon do usuário, para diferenciar entre os eventos de logon que são gerados pelo acesso efetivo e aqueles gerados por um logon de usuário de rede interativo, as informações de Nível de Representação estão incluídas. Quando o evento de logon for gerado pelo acesso efetivo, o Nível de Representação será a Identidade. Um logon de usuário de rede interativo normalmente gera um evento de logon com o Nível de Representação = Representação ou Delegação.

## <a name="see-also"></a><a name="BKMK_Links"></a>Consulte também

-   [Cenário: auditoria de acesso a arquivos](Scenario--File-Access-Auditing.md)

-   [Planejar a auditoria de acesso a arquivos](Plan-for-File-Access-Auditing.md)

-   [Controle de acesso dinâmico: visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)


