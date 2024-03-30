# ![DevSuperior logo](https://raw.githubusercontent.com/devsuperior/bds-assets/main/ds/devsuperior-logo-small.png) Job responsible for sending emails automatically using Spring Batch
>  Case study to implement a job to send emails using Spring Batch

#### Antes de começar

>  Docker Compose

MySQL 8.0 e phpMyAdmin

Script:

https://github.com/kaiquesilvadev/user-request-spring-batch/blob/main/docker-compose.yml

Subir a estrutura:

``` 
docker-compose up -d
```

Parar a estrutura:
```
docker-compose down
```
#### Dados de conexão para o phpMyAdmin
- Server: mysql-docker
- Usuário: root / user
- Senha: 1234567

#### Dados de conexão para o MySQL Workbench
- Host: 127.0.0.1
- Porta: 3307
- Usuário: root / user

### Etapas do Spring Batch:

- Leitura: Leia usuários com empréstimos próximos do retorno (número de dias para devolver ou renovar o livro menos 1 dia)
- Processamento: Gerar mensagem de e-mail (modelo)
- Escrita: Enviar e-mail notificando o retorno

### Recursos:

#### import.sql

```sql
CREATE TABLE tb_user (
	id INT NOT NULL AUTO_INCREMENT,
	name VARCHAR(60) NOT NULL,
	email VARCHAR(60) NOT NULL,
	PRIMARY KEY(id)
);

CREATE TABLE tb_book (
	id INT NOT NULL AUTO_INCREMENT,
	name VARCHAR(60) NOT NULL,
	description TEXT NOT NULL,
	author VARCHAR(60) NOT NULL,
	category VARCHAR(20) NOT NULL,
	PRIMARY KEY(id)
);

CREATE TABLE tb_user_book_loan (
	user_id INT NOT NULL,
	book_id INT NOT NULL,
	loan_date DATE NOT NULL,
	PRIMARY KEY(user_id, book_id),
	FOREIGN KEY (user_id) REFERENCES tb_user(id),
	FOREIGN KEY (book_id) REFERENCES tb_book(id)
);

INSERT INTO tb_user(id, name, email) VALUES (1, 'Maria', 'seu-email@mailslurp.com');
INSERT INTO tb_user(id, name, email) VALUES (2, 'João', 'seu-email@mailslurp.com');
INSERT INTO tb_user(id, name, email) VALUES (3, 'Ana', 'seu-email@mailslurp.com');

INSERT INTO tb_book (id, name, description, author, category) VALUES(1, 'Capitães de Areia', 'Lorem ipsum dolor sit amet. Est dicta voluptate sed pariatur laboriosam repellendus!', 'Jorge Amado', 'Romance');
INSERT INTO tb_book (id, name, description, author, category) VALUES(2, 'Dom Casmurro', 'Lorem ipsum dolor sit amet. Et praesentium nobis ut quaerat voluptate eum volup.', 'Machado de Assis', 'Romance');
INSERT INTO tb_book (id, name, description, author, category) VALUES(3, 'Quincas Borba', 'Eos doloribus impedit ut dolor sunt sit nostrum libero', 'Machado de Assis', 'Romance');
INSERT INTO tb_book (id, name, description, author, category) VALUES(4, 'Alguma poesia', 'Lorem ipsum dolor sit amet. Quo voluptates soluta sit.', 'Carlos Drummond de Andrade', 'Poesia');
INSERT INTO tb_book (id, name, description, author, category) VALUES(5, 'A hora da estrela', 'Et sunt quaerat vel provident dolores quo Quis', 'Clarisse Lispector', 'Poesia');
INSERT INTO tb_book (id, name, description, author, category) VALUES(6, 'Tudo sobre o amor', 'Lorem ipsum dolor sit amet. Ut corrupti ullam aut', 'Bell Hooks', 'Humanidade');
INSERT INTO tb_book (id, name, description, author, category) VALUES(7, 'Torto Arado', 'Id tempore quas et aperiam minima ut dolores', 'Itamar Vieira Junior', 'Romance');
INSERT INTO tb_book (id, name, description, author, category) VALUES(8, 'Os Miseráveis', 'Lorem ipsum dolor sit amet. Aut voluptates', 'Victor Hugo', 'Romance');
INSERT INTO tb_book (id, name, description, author, category) VALUES(9, 'Dom Quixote', 'Hic nobis dolor ut praesentium aspernatur', 'Miguel de Cervantes', 'Romance');

INSERT INTO tb_user_book_loan(user_id, book_id, loan_date) VALUES(1, 9, '2023-01-29');
INSERT INTO tb_user_book_loan(user_id, book_id, loan_date) VALUES(1, 4, '2023-01-31');
INSERT INTO tb_user_book_loan(user_id, book_id, loan_date) VALUES(2, 6, '2023-01-29');
INSERT INTO tb_user_book_loan(user_id, book_id, loan_date) VALUES(3, 2, '2023-01-31');
```
#### Add environment variable
```
spring.sendgrid.api-key=KEY_SENDGRID
```
```
spring.batch.job.enabled=false
```

See all :
- https://stackoverflow.com/questions/28928128/schedule-a-task-to-run-at-everyday-on-a-specific-time
- http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/tutorial-lesson-06.html






