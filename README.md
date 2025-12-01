# Projeto-SQL---AssistaFlix
A plataforma de streaming “AssistaFlix” oferece um vasto catálogo de títulos que são consumidos por usuários mediante uma assinatura paga. O sistema precisa registrar os seguintes processos e entidades:



-- 1. Tabela PLANO
CREATE TABLE PLANO (
    id_plano INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    preco DECIMAL(10, 2) NOT NULL,
    telas INT NOT NULL
);

-- 2. Tabela ASSINANTE
CREATE TABLE ASSINANTE (
    id_assinante INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    senha VARCHAR(255) NOT NULL, 
    id_plano INT NOT NULL,
    FOREIGN KEY (id_plano) REFERENCES PLANO(id_plano)
);

-- 3. Tabela PERFIL
CREATE TABLE PERFIL(
    id_perfil INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    idade INT NOT NULL,
    perfil_infantil BOOLEAN DEFAULT FALSE, 
    id_assinante INT NOT NULL,
    FOREIGN KEY (id_assinante) REFERENCES ASSINANTE(id_assinante)
);

-- 4. Tabela CONTEUDO (Supertipo)
CREATE TABLE CONTEUDO (
    id_conteudo INT AUTO_INCREMENT PRIMARY KEY, 
    titulo VARCHAR(255) NOT NULL, 
    sinopse TEXT,
    ano_lancamento INT, 
    classificacao_indicativa VARCHAR(50) 
);

-- 5. Tabela FILME (Subtipo)
CREATE TABLE FILME (
    fk_id_conteudo INT PRIMARY KEY, 
    duracao_segundos INT, 
    FOREIGN KEY (fk_id_conteudo) REFERENCES CONTEUDO(id_conteudo)
);

-- 6. Tabela SERIE (Subtipo)
CREATE TABLE SERIE (
    fk_id_conteudo INT PRIMARY KEY, 
    FOREIGN KEY (fk_id_conteudo) REFERENCES CONTEUDO(id_conteudo)
);

-- 7. Tabela TEMPORADA
CREATE TABLE TEMPORADA (
    id_temporada INT AUTO_INCREMENT PRIMARY KEY, 
    fk_id_conteudo INT NOT NULL,
    numero_temporada INT NOT NULL, 
    
    UNIQUE (fk_id_conteudo, numero_temporada),
    
    FOREIGN KEY (fk_id_conteudo) REFERENCES SERIE(fk_id_conteudo)
);

-- 8. Tabela EPISODIO
CREATE TABLE EPISODIO (
    id_episodio INT AUTO_INCREMENT PRIMARY KEY, 
    fk_id_temporada INT NOT NULL,
    titulo_episodio VARCHAR(255) NOT NULL, 
    
    FOREIGN KEY (fk_id_temporada) REFERENCES TEMPORADA(id_temporada)
);

-- 9. Tabela GENERO (Domínio)
CREATE TABLE GENERO (
    id_genero INT AUTO_INCREMENT PRIMARY KEY, 
    nome_genero VARCHAR(100) NOT NULL UNIQUE 
);

-- 10. Tabela CONTEUDO_GENERO (Associação M:N)
CREATE TABLE CONTEUDO_GENERO (
    fk_id_conteudo INT NOT NULL,
    fk_id_genero INT NOT NULL,
    
    PRIMARY KEY (fk_id_conteudo, fk_id_genero),
    
    FOREIGN KEY (fk_id_conteudo) REFERENCES CONTEUDO(id_conteudo),
    FOREIGN KEY (fk_id_genero) REFERENCES GENERO(id_genero)
);

/* #################################################
# INSERÇÃO DE DADOS 
################################################# */

-- Inserção 1. Tabela PLANO
INSERT INTO PLANO (nome, preco, telas) VALUES ('Básico', 19.90, 1); 
INSERT INTO PLANO (nome, preco, telas) VALUES ('Plus', 29.90, 4); 

-- Inserção 2. Tabela ASSINANTE
INSERT INTO ASSINANTE (nome, email, senha, id_plano) VALUES ('Ana Santos', 'anasantos@email.com', 'senha123', 1); 
INSERT INTO ASSINANTE (nome, email, senha, id_plano) VALUES ('José Dias', 'josedias@email.com', 'senha456', 2); 

-- Inserção 3. Tabela PERFIL
INSERT INTO PERFIL (nome, idade, perfil_infantil, id_assinante) VALUES ('Adulto', 22, FALSE, 1); 
INSERT INTO PERFIL (nome, idade, perfil_infantil, id_assinante) VALUES ('Adulto', 24, FALSE, 2);
INSERT INTO PERFIL (nome, idade, perfil_infantil, id_assinante) VALUES ('Infantil', 12, TRUE, 2);

-- Inserção 9. Tabela GENERO 
INSERT INTO GENERO (nome_genero) VALUES ('Animação'); 
INSERT INTO GENERO (nome_genero) VALUES ('Terror');    
INSERT INTO GENERO (nome_genero) VALUES ('Drama');     
INSERT INTO GENERO (nome_genero) VALUES ('Investigação'); 

-- Inserção 4. Tabela CONTEUDO e 5. Tabela FILME
-- Filme 1: O Rei Leão (ID_Conteudo = 1)
INSERT INTO CONTEUDO (titulo, sinopse, ano_lancamento, classificacao_indicativa) VALUES ('O Rei Leão', 'Desenho animado', 1994, 'Livre');
INSERT INTO FILME (fk_id_conteudo, duracao_segundos) VALUES (1, 5280); 

-- Filme 2: O Predador (ID_Conteudo = 2)
INSERT INTO CONTEUDO (titulo, sinopse, ano_lancamento, classificacao_indicativa) VALUES ('O Predador', 'Terror', 2022, '18+');
INSERT INTO FILME (fk_id_conteudo, duracao_segundos) VALUES (2, 6000); 

-- Filme 3: Estrelas Além do Tempo (ID_Conteudo = 3)
INSERT INTO CONTEUDO (titulo, sinopse, ano_lancamento, classificacao_indicativa) VALUES ('Estrelas Além do Tempo', 'Drama', 2016, 'Livre');
INSERT INTO FILME (fk_id_conteudo, duracao_segundos) VALUES (3, 7380); 

-- Inserção 4. Tabela CONTEUDO e 6. Tabela SERIE
-- Série 1: C.S.I.: Investigação Criminal (ID_Conteudo = 4)
INSERT INTO CONTEUDO (titulo, sinopse, ano_lancamento, classificacao_indicativa) VALUES ('C.S.I.: Investigação Criminal', 'Série de investigação policial', 2000, '14+');
INSERT INTO SERIE (fk_id_conteudo) VALUES (4);

-- Inserção 7. Tabela TEMPORADA
-- Temporada 1 da série ID 4 (C.S.I.) - ID_Temporada = 1
INSERT INTO TEMPORADA (fk_id_conteudo, numero_temporada) VALUES (4, 1); 
-- Temporada 2 da série ID 4 (C.S.I.) - ID_Temporada = 2
INSERT INTO TEMPORADA (fk_id_conteudo, numero_temporada) VALUES (4, 2); 

-- Inserção 8. Tabela EPISODIO
-- Episódios da Temporada 1 (ID 1)
INSERT INTO EPISODIO (fk_id_temporada, titulo_episodio) VALUES (1, 'Episódio Piloto');
INSERT INTO EPISODIO (fk_id_temporada, titulo_episodio) VALUES (1, 'Episódio 2 - O Suspeito');
-- Episódio da Temporada 2 (ID 2)
INSERT INTO EPISODIO (fk_id_temporada, titulo_episodio) VALUES (2, 'Episódio 1 da Segunda Temporada');

-- Inserção 10. Tabela CONTEUDO_GENERO (Relacionando Conteúdos com Gêneros)
INSERT INTO CONTEUDO_GENERO (fk_id_conteudo, fk_id_genero) VALUES (1, 1); 
INSERT INTO CONTEUDO_GENERO (fk_id_conteudo, fk_id_genero) VALUES (2, 2); 
INSERT INTO CONTEUDO_GENERO (fk_id_conteudo, fk_id_genero) VALUES (3, 3); 
INSERT INTO CONTEUDO_GENERO (fk_id_conteudo, fk_id_genero) VALUES (4, 4);


/* #################################################
# CONSULTAS
################################################# */

SELECT * FROM PLANO;
SELECT nome, preco FROM PLANO;
SELECT
    A.nome AS Nome_Assinante,
    P.nome AS Nome_Plano,
    P.preco AS Preco_Plano
FROM
    ASSINANTE A -- 'A' é o alias (apelido) para a tabela ASSINANTE
JOIN
    PLANO P ON A.id_plano = P.id_plano -- 'P' é o alias para a tabela PLANO
WHERE
    A.id_assinante = 1; -- Filtra pelo ID do assinante desejado
    
    SELECT
    COUNT(id_perfil) AS Total_Perfis
FROM
    PERFIL
WHERE
    id_assinante = 2;
    SELECT
    COUNT(P.id_perfil) AS Total_Perfis_Infantis,
    P.nome AS Nome_do_Perfil,
    A.nome AS Nome_do_Assinante
FROM
    PERFIL P
JOIN
    ASSINANTE A ON P.id_assinante = A.id_assinante
WHERE
    P.perfil_infantil = TRUE -- Filtra apenas perfis infantis
GROUP BY
    P.nome, A.nome;
    
    
    SELECT
    C.titulo,
    C.ano_lancamento,
    C.classificacao_indicativa
FROM
    CONTEUDO C -- Alias 'C' para a tabela CONTEUDO
JOIN
    FILME F ON C.id_conteudo = F.fk_id_conteudo -- Conecta apenas aos itens que são FILME
WHERE
    C.classificacao_indicativa = 'Livre'; -- Filtra pela classificação 'Livre'
    
    
    SELECT
    C.titulo,
    C.ano_lancamento,
    C.classificacao_indicativa
FROM
    CONTEUDO C -- Alias 'C' para a tabela CONTEUDO
JOIN
    SERIE S ON C.id_conteudo = S.fk_id_conteudo; -- Conecta apenas aos itens que são SERIES
    
    
    
SELECT
    C.titulo AS Nome_da_Serie,
    T.numero_temporada AS Temporada,
    E.titulo_episodio AS Titulo_do_Episodio
FROM
    CONTEUDO C
JOIN
    TEMPORADA T ON C.id_conteudo = T.fk_id_conteudo
JOIN
    EPISODIO E ON T.id_temporada = E.fk_id_temporada
WHERE
    C.titulo = 'C.S.I.: Investigação Criminal' -- 1. Filtra pela Série
    AND T.numero_temporada = 1;                 -- 2. Filtra pela Temporada 1
    
    
   SELECT
    C.titulo AS Nome_da_Serie,
    T.numero_temporada AS Temporada,
    E.titulo_episodio AS Titulo_do_Episodio
FROM
    CONTEUDO C
JOIN
    TEMPORADA T ON C.id_conteudo = T.fk_id_conteudo
JOIN
    EPISODIO E ON T.id_temporada = E.fk_id_temporada
WHERE
    C.titulo = 'C.S.I.: Investigação Criminal' -- Filtra pela Série
    AND T.numero_temporada = 2;                 -- Filtra pela Temporada 2
    
    
    
    -- Deleta os episódios da Temporada 2 (ID = 2)
DELETE FROM EPISODIO
WHERE fk_id_temporada = 2;


-- 1. Tabela a ser atualizada: PLANO
UPDATE PLANO

-- 2. Definindo os novos valores
SET nome = 'Plus', -- Você está alterando o nome para 'Plus'
    preco = 34.90

-- 3. Identificando a linha a ser atualizada
WHERE id_plano = 2;

SELECT
    id_plano,
    nome,
    preco,
    telas
FROM
    PLANO
WHERE
    id_plano = 2;



