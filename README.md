<p align="center"><img src="https://github.com/user-attachments/assets/176d345c-dcf6-4e13-9300-0e6d3ab901cc" width="20%"></p>
<p align="center">Sistema de Contenção</p>

## Clonando o Repositório
Clone este repositório usando o comando:
```bash
git clone https://github.com/aptivjam/new-contencao
```

## SQL
Estrutura SQL da aplicação
```bash
CREATE TABLE TAB_JBC (
  JBC_ID INT,
  JBC_EMISSAO DATETIME NOT NULL,
  JBC_EMITENTE INT,
  JBC_TIPO VARCHAR(50) NOT NULL,
  JBC_DESCRICAO VARCHAR(MAX) NOT NULL,
  JBC_RESPONSAVEL VARCHAR(50),
  JBC_RAZAO VARCHAR(100) NOT NULL,
  JBC_LINHA VARCHAR(50) NOT NULL,
  JBC_IDENTIFICACAO VARCHAR(100) NOT NULL,
  JBC_REACAO VARCHAR(100) NOT NULL,
  JBC_FIM DATETIME,
  PRIMARY KEY (JBC_ID)
);

CREATE TABLE TAB_JBC_PN (
  JBC_ID INT,
  PN_PARTNUMBER VARCHAR(20),
  PRIMARY KEY (JBC_ID, PN_PARTNUMBER),
  FOREIGN KEY (JBC_ID) REFERENCES TAB_JBC (JBC_ID) ON DELETE CASCADE
);

CREATE TABLE TAB_JBC_REVISAO (
  JBC_ID INT,
  REV_SEQUENCIA INT,
  REV_RESPONSAVEL INT,
  REV_DATA DATETIME,
  REV_MOTIVO VARCHAR(100),
  REV_PATH VARCHAR(100),
  REV_FLAG INT,
  PRIMARY KEY (REV_SEQUENCIA,JBC_ID),
  FOREIGN KEY (JBC_ID) REFERENCES TAB_JBC (JBC_ID) ON DELETE CASCADE
);

CREATE TABLE TAB_JBC_TREINADOR (
  TRE_TREINADOR INT,
  PRIMARY KEY (TRE_TREINADOR),
  FOREIGN KEY (TRE_TREINADOR) REFERENCES FUNCIONARIO (FUN_REGISTRO) ON DELETE CASCADE
);

CREATE TABLE TAB_JBC_TREINAMENTO (
  JBC_ID INT,
  REV_SEQUENCIA INT,
  TRE_FUNCIONARIO INT,
  TRE_TREINADOR INT,
  TRE_DATA DATETIME,
  PRIMARY KEY (REV_SEQUENCIA, JBC_ID, TRE_FUNCIONARIO),
  FOREIGN KEY (REV_SEQUENCIA,JBC_ID) REFERENCES TAB_JBC_REVISAO (REV_SEQUENCIA,JBC_ID) ON DELETE CASCADE,
  FOREIGN KEY (TRE_TREINADOR) REFERENCES TAB_JBC_TREINADOR (TRE_TREINADOR) ON DELETE NO ACTION,
  FOREIGN KEY (TRE_FUNCIONARIO) REFERENCES FUNCIONARIO (FUN_REGISTRO) ON DELETE NO ACTION
);

CREATE TABLE TAB_JBC_CONTENCAO (
  CON_ID INT IDENTITY,
  JBC_ID INT NOT NULL,
  CON_CAIXA VARCHAR(10),
  PN_PARTNUMBER VARCHAR(20) NOT NULL,
  CON_QUANTIDADE INT NOT NULL,
  CON_ENTRADA DATETIME NOT NULL,
  CON_SAIDA DATETIME,
  CON_AREA VARCHAR(50) NOT NULL,
  CARTAO_ID VARCHAR(20),
  FUN_REGISTRO INT NOT NULL,
  CON_INS_INICIO DATETIME NOT NULL,
  CON_INS_REGISTRO DATETIME NOT NULL,
  PRIMARY KEY (CON_ID),
  FOREIGN KEY (JBC_ID,PN_PARTNUMBER) REFERENCES TAB_JBC_PN (JBC_ID,PN_PARTNUMBER) ON DELETE CASCADE,
  FOREIGN KEY (FUN_REGISTRO) REFERENCES FUNCIONARIO (FUN_REGISTRO) ON DELETE NO ACTION
);

ALTER TABLE TAB_JBC_CONTENCAO ADD FOREIGN KEY (CON_INS_REGISTRO) REFERENCES FUNCIONARIO (FUN_REGISTRO) ON DELETE NO ACTION

CREATE TABLE TAB_JBC_CODIGOS (
  COD_ID INT IDENTITY,
  COD_MOTIVO VARCHAR(100) NOT NULL
  PRIMARY KEY (COD_ID)
);

CREATE TABLE TAB_JBC_SCRAP (
  SC_ID INT IDENTITY,
  CON_ID INT NOT NULL,
  COD_ID INT NOT NULL,
  SC_QUANTIDADE INT NOT NULL,
  SC_MOTIVO VARCHAR(100),
  PRIMARY KEY (SC_ID),
  FOREIGN KEY (CON_ID) REFERENCES TAB_JBC_CONTENCAO (CON_ID) ON DELETE CASCADE,
  FOREIGN KEY (COD_ID) REFERENCES TAB_JBC_CODIGOS (COD_ID),
);
```
<br>
<p align="center"><img src="https://github.com/user-attachments/assets/bb4c0c56-bf3e-4471-9d88-8056135c8f9f" width="20%"></p>

