# Att-Caixa-Branca-Pt1
Deixo um README.MD da atividade deixada por Daniel Ohata

Este repositório contém a análise completa de **teste de caixa branca** aplicada
com codigo em Java, com responsabilidades de conexão com o banco de dados e 
verificação de usuário.  

A documentação segue os padrões da disciplina de UX/UI – Testes Funcionais 
(Caixa Branca), incluindo grafo de fluxo (CFG), complexidade ciclomática, 
caminhos básicos e análise estrutural.


public Connection conectarBD() {
    try {
        Class.forName("com.mysql.cj.jdbc.Driver");
        return DriverManager.getConnection(url, user, pass);
    } catch (Exception e){
        return null;
    }
}

public boolean verificarUsuario(String usuario, String senha) {
    Connection conn = conectarBD();
    try {
        String sql = "SELECT * FROM usuarios WHERE usuario = '" + usuario + "' AND senha = '" + senha + "'";
        Statement st = conn.createStatement();
        ResultSet rs = st.executeQuery(sql);
        
        if (rs.next()) {
            nome = rs.getString("nome");
            return true;
        }
    } catch (Exception e) {
        return false;
    }
    return false;
}
