# Getlista-em-Java.util



Crie	o	método		getLista		na	classe		ContatoDao	.	Importe		List		de		java.util	:


		public	List<Contato>	getLista() {
						try	{
										List<Contato>	contatos	=	new	ArrayList<Contato>();
										PreparedStatement	stmt	=	this.connection.
																		prepareStatement("select	*	from	contatos");
										ResultSet	rs	=	stmt.executeQuery();
										while	(rs.next())	{
														//	criando	o	objeto	Contato
														Contato	contato	=	new	Contato();
														contato.setId(rs.getLong("id"));
														contato.setNome(rs.getString("nome"));
														contato.setEmail(rs.getString("email"));
														contato.setEndereco(rs.getString("endereco"));
														//	montando	a	data	através	do	Calendar
														Calendar	data	=	Calendar.getInstance();
														data.setTime(rs.getDate("dataNascimento"));
														contato.setDataNascimento(data);
														
														//	adicionando	o	objeto	à	lista
														
														
														contatos.add(contato);
														
										}
										rs.close();
										stmt.close();
										return	contatos;
						}	catch	(SQLException	e)	{
										throw new	RuntimeException(e);
						}
		}
    
    
    
Vamos	usar	o	método		getLista		para	listar	todos	os	contatos	do	nosso	banco	de	dados.   
Crie	uma	classe	chamada		TestaLista		com	um	método		main	:
Crie	um	ContatoDao:



		ContatoDao	dao	=	new	ContatoDao();
Liste	os	contatos	com	o	DAO:
		List<Contato>	contatos	=	dao.getLista();
Itere	nessa	lista	e	imprima	as	informações	dos	contatos:
		for	(Contato	contato	:	contatos)	{
						System.out.println("Nome:	"	+	contato.getNome());
						System.out.println("Email:	"	+	contato.getEmail());
						System.out.println("Endereço:	"	+	contato.getEndereco());
						System.out.println("Data	de	Nascimento:	"	+
																		contato.getDataNascimento().getTime()	+	"\n");
		}
	
    
Rode	o	programa	acima	clicando	em	Run	>	Run	as	>	Java	Application	(aproveite	para	aprender	a
tecla	de	atalho	para	executar	a	aplicação).  

	
Assim	como	o	MySQL	existem	outros	bancos	de	dados	gratuitos	e	opensource	na	internet.	O	H2	é
um	banco	desenvolvido	em	Java	que	pode	ser	acoplado	a	qualquer	aplicação	e	libera	o	cliente	da
necessidade	de	baixar	qualquer	banco	de	dados	antes	da	instalação	de	um	produto	Java!
O	Hibernate	tomou	 conta	 do	mercado	 e	 virou	 febre	mundial	 pois	 não	 se	 faz	 necessário	 escrever
uma	linha	de	código	SQL!
Se	um	projeto	não	usa	nenhuma	das	tecnologias	de	ORM	(Object	Relational	Mapping)	disponíveis,
o	mínimo	a	ser	feito	é	seguir	o	DAO.
    
1.	 A	 impressão	 da	 data	 de	 nascimento	 ficou	 um	 pouco	 estranha.	 Para	 formatá-la,	 pesquise	 sobre	 a
classe		SimpleDateFormat.
	
2.	 Crie	 uma	 classe	 chamada	 DAOException	 que	 estenda	 de	 	RuntimeException		 e	 utilize-a	 no	 seu
	ContatoDao.
	
3.	 Use	cláusulas		where		para	refinar	sua	pesquisa	no	banco	de	dados.	Por	exemplo:		where	nome	like
'C%'.
	
4.	 Crie	o	método		pesquisar		que	recebe	um	id	(int)	e	retorna	um	objeto	do	tipo		
	
1.	 Faça	conexões	para	outros	tipos	de	banco	de	dados	disponíveis.
Agora	que	você	já	sabe	usar	o		PreparedStatement		para	executar	qualquer	tipo	de	código	SQL	e
	ResultSet		para	receber	os	dados	retornados	da	sua	pesquisa	fica	simples,	porém	maçante,	escrever	o
código	de	diferentes	métodos	de	uma	classe	típica	de	DAO.
	
	
Veja	primeiro	o	método		altera	,	que	recebe	um		contato		cujos	valores	devem	ser	alterados:


	
public	void	altera(Contato	contato) {
				String	sql	=	"update	contatos	set	nome=?,	email=?,	endereco=?,"	+
												"dataNascimento=?	where	id=?";
				try	{
								PreparedStatement	stmt	=	connection.prepareStatement(sql);
								stmt.setString(1,	contato.getNome());
								stmt.setString(2,	contato.getEmail());
								stmt.setString(3,	contato.getEndereco());
								stmt.setDate(4,	new	Date(contato.getDataNascimento()
																.getTimeInMillis()));
								stmt.setLong(5,	contato.getId());
								stmt.execute();
								stmt.close();
				}	catch	(SQLException	e)	{
								throw new	RuntimeException(e);
				}
}



Não	existe	nada	de	novo	nas	linhas	acima.	Uma	execução	de	query!	Simples,	não?
O	código	para	remoção:	começa	com	uma	query	baseada	em	um	contato,	mas	usa	somente	o	id	dele
para	executar	a	query	do	tipo	delete:


	
public	void	remove(Contato	contato) {
				try	{
								PreparedStatement	stmt	=	connection.prepareStatement("delete	"	+
																"from	contatos	where	id=?");
								stmt.setLong(1,	contato.getId());    
                stmt.execute();
								stmt.close();
				}	catch	(SQLException	e)	{
								throw new	RuntimeException(e);
				}
}
    
