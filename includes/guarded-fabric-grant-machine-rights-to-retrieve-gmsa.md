1. Ter um administrador de serviços de diretório adicione a conta de computador do novo nó ao grupo de segurança que contém todos os seus servidores HGS que têm a permissão para permitir que esses servidores usem a conta gMSA do HGS.

2. Reinicialize o novo nó para obter um novo tíquete Kerberos que inclui a associação do computador no grupo de segurança. Depois que a reinicialização for concluída, entre com uma identidade de domínio que pertença ao grupo local de administradores no computador.

3. Instale a conta de serviço gerenciado do grupo HGS no nó.

   ```powershell
   Install-ADServiceAccount -Identity <HGSgMSAAccount>
   ```
