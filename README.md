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

Código original comentado;

    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.ResultSet;
    import java.sql.Statement;
    
    public class User {
    
        // Nó 1: início do método conectarBD()
        public Connection conectarBD() {
            // Nó 2: tenta criar a conexão
            Connection conn = null; 
    
            try {
                // Nó 3: execução normal - tenta carregar driver e abrir conexão
                Class.forName("com.mysql.Driver.Manager").newInstance();
                String url = "jdbc:mysql://127.0.0.1/test?user=Lopes&password=123";
                return DriverManager.getConnection(url);
    
            } catch (Exception e) {
                // Nó 4: se der erro, captura mas não faz nada
            }
    
            // Nó 5: retorna conexão nula ou válida
            return conn;
        }
    
        // Nó 6: atributo público para guardar o nome do usuário
        public String nome = "";
    
        // Nó 7: atributo público booleano para resultado
        public boolean result = false;
    
        // Nó 8: início do método verificarUsuario
        public boolean verificarUsuario(String login, String senha) {
            String sql = ""; // Nó 9: inicializa string SQL
            Connection conn = conectarBD(); // Nó 10: cria conexão com o DB
    
            // Nó 11: construção da instrução SQL
            sql += "SELECT nome FROM usuarios ";
            sql += "WHERE login = '" + login + "' ";
            sql += "AND senha = '" + senha + "';";
    
            try {
                // Nó 12: cria Statement e executa a query
                Statement st = conn.createStatement();
                ResultSet rs = st.executeQuery(sql);
    
                // Nó 13: decisão - existe resultado?
                if (rs.next()) {
                    // Nó 14: retorna true se encontrou usuário
                    return true;
    
                    // Nó 15: atribuição do nome do usuário (linha inatingível)
                    nome = rs.getString("nome");
                }
    
            } catch (Exception e) {
                // Nó 16: captura exceção sem tratamento
            }
    
            // Nó 17: retorna false caso não encontre ou erro
            return false;
        }
    }



Soluçoes para esses erros, vão ser aplicados em um novo código reestruturado, junto com o fluxo de grafo adaptado para o novo código.
Adicionada documentação → comentários JavaDoc explicando cada método.
Variáveis renomeadas → conn → connection, st → preparedStatement, rs → resultSet.
Melhor legibilidade → try-with-resources e tratamento de exceções claro.
Tratamento de NullPointer → try-with-resources garante que a conexão foi aberta; mensagem de erro se falhar.
Fechamento automático de recursos → Connection, PreparedStatement e ResultSet fecham automaticamente.

    
