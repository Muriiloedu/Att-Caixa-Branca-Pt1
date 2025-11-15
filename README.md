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



    
