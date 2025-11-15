# Att-Caixa-Branca-Pt1
Deixo um README.MD da atividade deixada por Daniel Ohata

Este repositório contém a análise completa de **teste de caixa branca** aplicada
com codigo em Java, com responsabilidades de conexão com o banco de dados e 
verificação de usuário.  

A documentação segue os padrões da disciplina de UX/UI – Testes Funcionais 
(Caixa Branca), incluindo grafo de fluxo (CFG), complexidade ciclomática, 
caminhos básicos e análise estrutural.

Código para revisar disponibilizado pelo professor abaixo;

    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.ResultSet;
    import java.sql.Statement;
    
    public class User{
    public static Connection conectarBD() {
        Connection conn = null;
        try {
            Class.forName("com.mysql.Driver.Manager").newInstance();
            String url = "jdbc:mysql://127.0.0.1/test?user=Lopes&password=123";
            return DriverManager.getConnection(url);
        } catch (Exception e) { }
        return conn;}
        
    public String nome = "";
    public boolean result = false;
    public boolean verificarUsuario(String login, String senha) {
        String sql = "";
        Connection conn = conectarBD();

        //Intrução SQL 
        sql += "SELECT nome FROM usuarios ";
        sql += "WHERE login = '" + login + "' ";
        sql += "AND senha = '" + senha + "';";
        try {
            Statement st = conn.createStatement();
            ResultSet rs = st.executeQuery(sql);
            if (rs.next()) {
                return true;
                nome = rs.getString("nome");
            }
        } catch (Exception e) {}
        return false; }
    }


<img width="864" height="152" alt="image" src="IMG-Erro-Caixa Branca.png" />

Erros detectados: Código sem documentação → métodos conectarBD e verificarUsuario não tinham comentários explicando a função.
Nomes de variáveis pouco claros → st, rs e conn são genéricos.
Legibilidade e organização → try-catch sem tratamento e blocos grandes dificultam a leitura.
NullPointer não tratado → conn.createStatement() poderia falhar se conn for null.
Recursos não fechados → Connection, Statement e ResultSet não eram fechados, podendo travar o banco.


Soluçoes para esses erros, vão ser aplicados em um novo código reestruturado, junto com o fluxo de grafo adaptado para o novo código.
Adicionada documentação → comentários JavaDoc explicando cada método.
Variáveis renomeadas → conn → connection, st → preparedStatement, rs → resultSet.
Melhor legibilidade → try-with-resources e tratamento de exceções claro.
Tratamento de NullPointer → try-with-resources garante que a conexão foi aberta; mensagem de erro se falhar.
Fechamento automático de recursos → Connection, PreparedStatement e ResultSet fecham automaticamente.

    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.PreparedStatement;
    import java.sql.ResultSet;
    import java.sql.SQLException;
    
    /**
     * Classe User para autenticação no banco de dados.
     */
    public class User {
    
        private String nome = ""; // NÓ 1 - início / atributo do usuário
    
        /**
         * Retorna o nome do usuário autenticado.
         */
        public String getNome() { // NÓ 2
            return nome; // NÓ 2
        }
    
        /**
         * Conecta ao banco de dados MySQL.
         * @return Connection
         * @throws SQLException caso a conexão falhe
         */
        public static Connection conectarBD() throws SQLException { // NÓ 3
            String url = "jdbc:mysql://127.0.0.1/test"; // NÓ 4
            String user = System.getenv("Lopes");      // NÓ 5 - usuário
            String password = System.getenv("123");    // NÓ 6 - senha
    
            // Driver moderno, sem newInstance() // NÓ 7
            return DriverManager.getConnection(url, user, password); // NÓ 8
        }
    
        /**
         * Verifica se o usuário e senha existem no banco de dados.
         * @param login Usuário
         * @param senha Senha
         * @return true se usuário existe, false caso contrário
         */
        public boolean verificarUsuario(String login, String senha) { // NÓ 9 - início do método
            String sql = "SELECT nome FROM usuarios WHERE login = ? AND senha = ?"; // NÓ 10 - preparação da query
    
            // NÓ 11 - tentativa de conexão e PreparedStatement
            try (Connection connection = conectarBD();
                 PreparedStatement preparedStatement = connection.prepareStatement(sql)) {
    
                // NÓ 12 - definir parâmetros da query
                preparedStatement.setString(1, login);
                preparedStatement.setString(2, senha);
    
                // NÓ 13 - executar query
                try (ResultSet resultSet = preparedStatement.executeQuery()) {
                    // NÓ 14 - decisão: usuário encontrado?
                    if (resultSet.next()) {
                        nome = resultSet.getString("nome"); // NÓ 14a - atribuição do nome
                        return true;                        // NÓ 14b - retorno true
                    }
                    // NÓ 14c - usuário não encontrado
                    return false; // return false caso rs.next() seja false
                }
    
            } catch (SQLException e) { // NÓ 15 - captura de exceção
                System.err.println("Erro ao verificar usuário: " + e.getMessage()); // NÓ 15a
                return false; // NÓ 15b - retorno em caso de erro
            }
        } // NÓ 16 - fim do método verificarUsuario
    } // NÓ 17 - fim da classe

