1. Ter um administrador de serviços de diretório para adicionar a conta de computador para o novo nó para o grupo de segurança que contém todos os seus servidores HGS que está recebendo a permissão para permitir que esses servidores usar a conta de gMSA HGS.

2. Reinicialize o novo nó para obter um novo tíquete Kerberos que inclui a associação do computador no grupo de segurança. Após a reinicialização for concluída, entre com uma identidade de domínio que pertence ao grupo Administradores local no computador.

3. Instale a conta de serviço gerenciado de grupo HGS no nó.

   ```powershell
   Install-ADServiceAccount -Identity <HGSgMSAAccount>
   ```
