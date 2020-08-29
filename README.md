# Auxílio Jurídico
Trabalho desenvolvido durante a disciplina de BD1

# Sumário

### 1. COMPONENTES<br>
Integrantes do grupo<br>
Lucas Neves de Oliveira: lucas.7.snow@gmail.com<br>
Marlon Santos Macedo: wazymazy@gmail.com<br>

### 2. INTRODUÇÃO E MOTIVAÇÃO<br>
Este documento contém a especificação do projeto do banco de dados Auxílio Jurídico
<br>e motivação da escolha realizada. <br>

O Sistema “Auxílio Jurídico” tem como objetivo auxiliar juridicamente as pessoas de baixa renda na pandemia. Muitos cidadãos tiveram problemas em receber seus benefícios, como: seguro-desemprego, auxílio emergencial etc. Os cidadãos não podem sair de casa para poder resolver esses problemas, e muitos não podem pagar advogados ou nem sabem como recorrer, e é nessa hora que entra o “Auxílio Jurídico”.

### 3. MINI-MUNDO<br>

Para utilizar esse auxílio o auxiliado precisa acessar o sistema e informar seu nome, CPF, senha, CTPS (se possuir), RG, número de telefone e data de nascimento. Após isso, o auxiliado pode abrir uma ou mais solicitações. Cada solicitação possui um código único, o registro de seu estado atual, do auxiliado que a abriu, e da data em que foi aberta. Uma solicitação só pode estar vinculada a apenas um auxiliado, o que a abriu. As solicitações podem ser atendidas por advogados, que devem se cadastrar no sistema informando seu nome, CPF, senha e número do registro na OAB. Um advogado pode atender a várias solicitações, mas uma solicitação só pode ser atendida por um advogado. Após a solicitação ser atendida, o auxiliado e o advogado podem trocar mensagens, que possuem um código único, o texto enviado, a data de envio e o remetente, que pode ser tanto um auxiliado, quanto um advogado. Toda mensagem deve estar vinculada a apenas uma solicitação, já as solicitações podem possuir várias mensagens. Durante o processo, o estado da solicitação pode ser alterada pelo advogado para indicar se está em aguardando atendimento, em andamento, aguardando informações ou finalizada.

### 4. PROTOTIPAÇÃO, PERGUNTAS A SEREM RESPONDIDAS E TABELA DE DADOS<br>
#### 4.1. RASCUNHOS BÁSICOS DA INTERFACE (MOCKUPS)<br>
[Arquivo PDF do Protótipo Balsamiq feito para Auxílio Jurídico](https://github.com/discipbd1/trab01/blob/master/arquivos/Auxílio%20Jurídico.pdf?raw=true)

#### 4.2 QUAIS PERGUNTAS PODEM SER RESPONDIDAS COM O SISTEMA PROPOSTO?
    a) O sistema proposto poderá fornecer quais tipos de relatórios e informaçes? 
    b) Crie uma lista com os 5 principais relatórios que poderão ser obtidos por meio do sistema proposto!
    
> A Empresa DevCom precisa inicialmente dos seguintes relatórios:
* Relatório que mostre o nome de cada supervisor(a) e a quantidade de empregados supervisionados.
* Relatório relativo aos os supervisores e supervisionados. O resultado deve conter o nome do supervisor e nome do supervisionado além da quantidade total de horas que cada supervisionado tem alocada aos projetos existentes na empresa.
* Relatorio que mostre para cada linha obtida o nome do departamento, o valor individual de cada salario existente no  departamento e a média geral de salarios dentre todos os empregados. Os resultados devem ser apresentados ordenados por departamento.
* Relatório que mostre as informações relacionadas a todos empregados de empresa (sem excluir ninguém). As linhas resultantes devem conter informações sobre: rg, nome, salario do empregado, data de início do salario atual, nomes dos projetos que participa, quantidade de horas e localização nos referidos projetos, numero e nome dos departamentos aos quais está alocado, informações do historico de salário como inicio, fim, e valores de salarios antigos que foram inclusos na referida tabela (caso possuam informações na mesma), além de todas informações relativas aos dependentes. 
>> ##### Observações: <br> a) perceba que este relatório pode conter linhas com alguns dados repetidos (mas não todos). <br>  b) para os empregados que não possuirem alguma destas informações o valor no registro deve aparecer sem informação/nulo. 
* Relatório que obtenha a frequencia absoluta e frequencia relativa da quantidade de cpfs únicos no relatório anterior. Apresente os resultados ordenados de forma decrescente pela frequencia relativa.

 
 
#### 4.3 TABELA DE DADOS DO SISTEMA:
    a) Esta tabela deve conter todos os atributos do sistema e um mínimo de 10 linhas/registros de dados.
    b) Esta tabela tem a intenção de simular um relatório com todos os dados que serão armazenados 
    
![Exemplo de Tabela de dados da Empresa Devcom](https://github.com/discipbd1/trab01/blob/master/arquivos/TabelaEmpresaDevCom_sample.xlsx?raw=true "Tabela - Empresa Devcom")
    
    
### 5.MODELO CONCEITUAL<br>
![Modelo Conceitual](https://github.com/discipbd1/trab01/blob/master/images/modeloconceitual.png?raw=true)
    
    
        
    
#### 5.1 Validação do Modelo Conceitual
    ATVGen: Matheus Costa Evangelista, Natan Paschoal Cypriano, Rafael de Almeida Viana Gusmão.
     Não tiveram observação.


#### 5.2 Descrição dos dados 
    USUARIO: É a tabela responsavel por armazenar os dados cruciais usuário(nome,cpf e senha).
    Auxiliado (Usuário): A tabela responsavel por pegar os dados de quem é auxiliado (CTPS, RG, telefone e data de nascimento).
    Advogado (Usuário): pega o  nº OAB do profissional.
    Solicitação: Guarda os dados da solicitação (código, estado atual e data de abertura).
    Mensagens: Armazenas as mensagens e os dados do remetente e da mensagem(código, texto, data de envio e remetente).
    

### 6	MODELO LÓGICO<br>
        a) inclusão do esquema lógico do banco de dados
        b) verificação de correspondencia com o modelo conceitual 
        (não serão aceitos modelos que não estejam em conformidade)

### 7	MODELO FÍSICO<br>
    create table USUARIO (
    nome varchar(60),
    cpf bigint primary key,
    senha varchar(30)
    );

    create table PROFISSIONAL_JURIDICO (
    cpf_usuario bigint references USUARIO(cpf) primary key,
    numero_oab bigint
    );

    create table AUXILIADO (
    cpf_usuario bigint references USUARIO(cpf) primary key,
    ctps bigint,
    rg bigint,
    numero_telefone int,
    data_nascimento date
    );

    create table SOLICITACAO (
    codigo serial primary key,
    estado_atual varchar(100),
    data_abertura date,
    cpf_auxiliado bigint references AUXILIADO(cpf_usuario),
    cpf_profissional bigint references PROFISSIONAL_JURIDICO(cpf_usuario)
    );

    create table MENSAGEM (
    codigo serial primary key,
    codigo_solicitacao serial references SOLICITACAO(codigo),
    texto varchar(255),
    data_envio timestamp,
    cpf_remetente bigint references USUARIO(cpf)
    );
        
       
### 8	INSERT APLICADO NAS TABELAS DO BANCO DE DADOS<br>
        a) inclusão das instruções de inserção dos dados nas tabelas criadas pelo script de modelo físico
        (Drop para exclusão de tabelas + create definição de para tabelas e estruturas de dados + insert para dados a serem inseridos)
        b) Criar um novo banco de dados para testar a restauracao 
        (em caso de falha na restauração o grupo não pontuará neste quesito)
        c) formato .SQL


### 9	TABELAS E PRINCIPAIS CONSULTAS<br>
    OBS: Incluir para cada tópico as instruções SQL + imagens (print da tela) mostrando os resultados.<br>
#### 9.1	CONSULTAS DAS TABELAS COM TODOS OS DADOS INSERIDOS (Todas) <br>

># Marco de Entrega 01: Do item 1 até o item 9.1<br>

#### 9.2	CONSULTAS DAS TABELAS COM FILTROS WHERE (Mínimo 4)<br>
#### 9.3	CONSULTAS QUE USAM OPERADORES LÓGICOS, ARITMÉTICOS E TABELAS OU CAMPOS RENOMEADOS (Mínimo 11)
    a) Criar 5 consultas que envolvam os operadores lógicos AND, OR e Not
    b) Criar no mínimo 3 consultas com operadores aritméticos 
    c) Criar no mínimo 3 consultas com operação de renomear nomes de campos ou tabelas

#### 9.4	CONSULTAS QUE USAM OPERADORES LIKE E DATAS (Mínimo 12) <br>
    a) Criar outras 5 consultas que envolvam like ou ilike
    b) Criar uma consulta para cada tipo de função data apresentada.

#### 9.5	INSTRUÇÕES APLICANDO ATUALIZAÇÃO E EXCLUSÃO DE DADOS (Mínimo 6)<br>
    a) Criar minimo 3 de exclusão
    b) Criar minimo 3 de atualização

#### 9.6	CONSULTAS COM INNER JOIN E ORDER BY (Mínimo 6)<br>
    a) Uma junção que envolva todas as tabelas possuindo no mínimo 2 registros no resultado
    b) Outras junções que o grupo considere como sendo as de principal importância para o trabalho

#### 9.7	CONSULTAS COM GROUP BY E FUNÇÕES DE AGRUPAMENTO (Mínimo 6)<br>
    a) Criar minimo 2 envolvendo algum tipo de junção

#### 9.8	CONSULTAS COM LEFT, RIGHT E FULL JOIN (Mínimo 4)<br>
    a) Criar minimo 1 de cada tipo

#### 9.9	CONSULTAS COM SELF JOIN E VIEW (Mínimo 6)<br>
        a) Uma junção que envolva Self Join (caso não ocorra na base justificar e substituir por uma view)
        b) Outras junções com views que o grupo considere como sendo de relevante importância para o trabalho

#### 9.10	SUBCONSULTAS (Mínimo 4)<br>
     a) Criar minimo 1 envolvendo GROUP BY
     b) Criar minimo 1 envolvendo algum tipo de junção

># Marco de Entrega 02: Do item 9.2 até o ítem 9.10<br>

### 10 RELATÓRIOS E GRÁFICOS

#### a) análises e resultados provenientes do banco de dados desenvolvido (usar modelo disponível)
#### b) link com exemplo de relatórios será disponiblizado pelo professor no AVA
#### OBS: Esta é uma atividade de grande relevância no contexto do trabalho. Mantenha o foco nos 5 principais relatórios/resultados visando obter o melhor resultado possível.

    

### 11	AJUSTES DA DOCUMENTAÇÃO, CRIAÇÃO DOS SLIDES E VÍDEO PARA APRESENTAÇAO FINAL <br>

#### a) Modelo (pecha kucha)<br>
#### b) Tempo de apresentação 6:40 

># Marco de Entrega 03: Itens 10 e 11<br>
<br>
<br>
<br> 



### 12 FORMATACAO NO GIT:<br> 
https://help.github.com/articles/basic-writing-and-formatting-syntax/
<comentario no git>
    
##### About Formatting
    https://help.github.com/articles/about-writing-and-formatting-on-github/
    
##### Basic Formatting in Git
    
    https://help.github.com/articles/basic-writing-and-formatting-syntax/#referencing-issues-and-pull-requests
    
    
##### Working with advanced formatting
    https://help.github.com/articles/working-with-advanced-formatting/
#### Mastering Markdown
    https://guides.github.com/features/mastering-markdown/

    
### OBSERVAÇÕES IMPORTANTES

#### Todos os arquivos que fazem parte do projeto (Imagens, pdfs, arquivos fonte, etc..), devem estar presentes no GIT. Os arquivos do projeto vigente não devem ser armazenados em quaisquer outras plataformas.
1. <strong>Caso existam arquivos com conteúdos sigilosos<strong>, comunicar o professor que definirá em conjunto com o grupo a melhor forma de armazenamento do arquivo.

#### Todos os grupos deverão fazer Fork deste repositório e dar permissões administrativas ao usuário do git "profmoisesomena", para acompanhamento do trabalho.

#### Os usuários criados no GIT devem possuir o nome de identificação do aluno (não serão aceitos nomes como Eu123, meuprojeto, pro456, etc). Em caso de dúvida comunicar o professor.


Link para BrModelo:<br>
http://www.sis4.com/brModelo/download.html
<br>


Link para curso de GIT<br>
![https://www.youtube.com/curso_git](https://www.youtube.com/playlist?list=PLo7sFyCeiGUdIyEmHdfbuD2eR4XPDqnN2?raw=true "Title")


