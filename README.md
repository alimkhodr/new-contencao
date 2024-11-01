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

## Demonstração do sistema
Screenshots do sistema

  <details>
    <summary><b>Home</b></summary>
    <ul><p align="center"><img src="https://github.com/alimkhodr/Contencao-Aptiv/assets/85517447/04675c4f-671e-4a21-9274-0fdbbe17ad05"></img></p></ul>
    Visualização das contenções abertas.
  </details>
    <details>
    <summary><b>Treinamentos</b></summary>
    <ul><p align="center"><img src="https://github.com/alimkhodr/Contencao-Aptiv/assets/85517447/c2104aa5-fec7-4fe9-bf1b-e5f62ec332a2"></img></p></ul>
      Treinar funcionário para ele poder fazer apontamentos na área da contenção.
  </details>
    <details>
    <summary><b>Apontamento</b></summary>    
    <ul><p align="center"><img src="https://github.com/alimkhodr/Contencao-Aptiv/assets/85517447/f8d4af92-f086-4bca-a6b1-32e2c9427cb0"></img></p></ul>
    Após selecionar uma contenção pode apontar a entrada de uma caixa ou inspecionar uma ja entrada ou finalizar caso já fez a inspeção.
  </details>
  <details>
    <summary><b>Entrada</b></summary>
    <ul><p align="center"><img src="https://github.com/alimkhodr/Contencao-Aptiv/assets/85517447/ab7f6115-4e3a-4bef-948c-99618ec7b925)"></img></p></ul>
    Entrada na caixa (quantidade e pn entra automatico do banco) .
  </details>
    <details>
    <summary><b>Inspeção</b></summary>
    <ul><p align="center"><img src="https://github.com/alimkhodr/Contencao-Aptiv/assets/85517447/846de7f6-0efd-4dd9-bce8-4c6b68dc3a1e"></img></p></ul>
      Inspeção na caixa.
  </details>
  <details>
    <summary><b>Saída</b></summary>
    <ul><p align="center"><img src="https://github.com/alimkhodr/Contencao-Aptiv/assets/85517447/386cb2a3-30af-4bdc-b134-002e947ec0f1"></img></p></ul>
    Finalizar caixa caso já fez a inspeção.
  </details>
  <details>
    <summary><b>Códigos de defeito</b></summary>
    <ul><p align="center"><img src="https://github.com/alimkhodr/Contencao-Aptiv/assets/85517447/c754d033-23be-4ef8-9df5-2697bbbeb1be"></img></p></ul>
    Códigos de defeito para apontar na saída.
      </details>
    <details>
    <summary><b>Configuração</b></summary>
    <ul><p align="center"><img src="https://github.com/alimkhodr/Contencao-Aptiv/assets/85517447/3d217b5b-132f-49fa-b80d-267fb450eba5"></img></p></ul>
    Mudança de área do sistema (mudança do arquivo area.txt salvo no pc onde foi instalado).
  </details>
    <details>
    <summary><b>Login</b></summary>
    <ul><p align="center"><img src="https://github.com/alimkhodr/Contencao-Aptiv/assets/85517447/fdbd8812-25e9-4f0f-ba0e-40660a89128d"></img></p></ul>
    Login para apontamento e mudança de área do sistema.
  </details>

<br/>

<p align="center"><img src="https://github.com/user-attachments/assets/bb4c0c56-bf3e-4471-9d88-8056135c8f9f" width="20%"></p>

