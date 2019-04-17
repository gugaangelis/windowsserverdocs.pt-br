---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: "Implantar com políticas de auditoria Central (etapas de demonstração) de auditoria de segurança"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac2b1643ed151e94c3815abca9a57eb3706c845a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>Implantar com políticas de auditoria Central (etapas de demonstração) de auditoria de segurança

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Nesse cenário, você fará a auditoria acesso a arquivos na pasta documentos de finanças usando a política de finanças que você criou na [implantar uma política de acesso Central #40 etapas de demonstração &; 41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md). Se um usuário que não está autorizado a acessar a pasta tenta acessá-lo, a atividade é capturada no Visualizador de eventos.   
 As etapas a seguir são necessários para testar esse cenário.  
  
|Tarefa|Descrição|  
|--------|---------------|  
|[Configurar o acesso a objetos globais](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|Nesta etapa, você pode configurar a política de acesso do objeto global no controlador de domínio.|  
|[Configurações de política de grupo de atualização](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|Faça logon no servidor de arquivos e aplique a atualização de política de grupo.|  
|[Verificar se a política acesso a objetos globais foi aplicada](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|Exiba os eventos relevantes no Visualizador de eventos. Os eventos devem incluir metadados para o tipo de documento e país.|  
  
## <a name="BKMK_1"></a>Configurar a política de acesso a objetos globais  
Nesta etapa, você deve configurar a política de acesso do objeto global no controlador de domínio.  
  
#### <a name="to-configure-a-global-object-access-policy"></a>Para configurar uma política de acesso do objeto global  
  
1.  Faça logon controlador de domínio DC1 como Contoso\administrador com a senha **pass@word1**.  
  
2.  No Gerenciador do servidor, aponte para **ferramentas**e clique em **Group Policy Management **.  
  
3.  Na árvore de console, clique duas vezes em **domínios**, clique duas vezes em **contoso.com**, clique em **Contoso**e clique duas vezes em **servidores de arquivos **.  
  
4.  Clique com botão direito **FlexibleAccessGPO**e clique em **editar **.  
  
5.  Clique duas vezes em **configuração do computador**, clique duas vezes em **políticas**e clique duas vezes em **configurações do Windows **.  
  
6.  Clique duas vezes em **configurações de segurança**, clique duas vezes em **configuração avançada de política de auditoria**e clique duas vezes em **políticas de auditoria **.  
  
7.  Clique duas vezes em **acesso a objetos**e clique duas vezes em **sistema de arquivos de auditoria **.  
  
8.  Selecione o **configurar os seguintes eventos** caixa de seleção, selecione o **sucesso** e **falha** caixas de seleção e, em seguida, clique em **Okey **.  
  
9. No painel de navegação, clique duas vezes em **Global Object Access Auditing**e clique duas vezes em **sistema de arquivos **.  
  
10. Selecione o **definir esta configuração de política** caixa de seleção e clique em **configurar **.  
  
11. No **configurações de segurança avançadas para SACL Global do arquivo**, clique em **adicionar**, clique em **selecionar uma entidade de segurança**, tipo **todos**e clique em **Okey **.  
  
12. No **Auditing Entry for Global SACL de arquivo** caixa, selecione **controle total** no **permissões** caixa.  
  
13. No **adicionar uma condição:** seção, clique em **adicionar uma condição** e na lista suspensa lista Selecione   
    [**Recurso**] [**Departamento**] [**Any of**] [**Value**] [**Finanças**].  
  
14. Clique em **Okey** configuração de política de auditoria de três vezes para concluir a configuração de acesso a objetos globais.  
  
15. No painel de navegação, clique em **acesso a objetos**e no painel de resultados, clique duas vezes em **auditoria de manipulação de identificador **. Clique em **configurar os seguintes eventos de auditoria**, **sucesso**, e **falha**, clique em **Okey**e feche o GPO de acesso flexível.  
  
## <a name="BKMK_2"></a>Atualizar configurações de política de grupo  
Nesta etapa, você atualiza as configurações de política de grupo depois que tiver criado a política de auditoria.  
  
#### <a name="to-update-group-policy-settings"></a>Para atualizar as configurações de política de grupo  
  
1.  Faça logon no servidor de arquivos, arquivo1 como Contoso\administrador, com a senha **pass@word1**.  
  
2.  Pressione Windows tecla + R, digite **cmd** para abrir uma janela de Prompt de comando.  
  
    > [!NOTE]  
    > Se o **User Account Control** caixa de diálogo aparecer, confirme se a ação exibida é o que você deseja e clique em **Sim **.  
  
3.  Tipo **gpupdate /force** e pressione ENTER.  
  
## <a name="BKMK_3"></a>Verificar se a política acesso a objetos globais foi aplicada  
Depois que as configurações de política de grupo tiverem sido aplicadas, você pode verificar se as configurações de política de auditoria foram aplicadas corretamente.  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>Para verificar se a política acesso a objetos globais foi aplicada  
  
1.  Faça logon computador cliente, CLIENT1 como Contoso\MReid. Navegue até a pasta hiperlink "file:///\\\ID_AD_FILE1\\\Finance" \\\ FILE1\Finance documentos e modifique 2 de documento do Word.  
  
2.  Faça logon servidor de arquivos, arquivo1 como Contoso\administrador. Abra o Visualizador de eventos, navegue até **Logs do Windows**, selecione **segurança**e confirme se as atividades resultaram em eventos de auditoria **4656** e **4663** (mesmo que você não tenha definido SACLs de auditoria explícitas nos arquivos ou pastas que você criou, modificou e excluiu).  
  
> [!IMPORTANT]  
> Um novo evento de logon é gerado no computador onde o recurso está localizado, em nome do usuário para o qual o acesso eficaz está sendo verificado. Ao analisar os logs de auditoria de segurança para entrar atividade do usuário, para diferenciar entre eventos de logon são gerados por causa do acesso eficaz e os gerados devido um usuário interativo rede login, as informações do nível de representação estão incluídas. Quando o evento de logon é gerado por causa do acesso eficaz, o nível de representação será identidade. Um log de usuário interativa de rede em normalmente gera um evento de logon com o nível de representação = representação ou delegação.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Cenário: Auditoria de acesso de arquivos](Scenario--File-Access-Auditing.md)  
  
-   [Planeje para o arquivo de auditoria de acesso](Plan-for-File-Access-Auditing.md)  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  

