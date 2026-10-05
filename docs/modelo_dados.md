### Modelo de dados do sistema

#### Identificador
```
Int id: Único para cada eleitor
Int idVerificacao: Único para cada verificação
Int estado: Número do estado*

```
**Estados:* 0 - não verificado; 1 - reservado; 2 - votado

#### Voto Host
```
Int id: Único para cada voto
Int votoChapa: Número da chapa escolhida
Dict votosIndependentes: {
    cargo: Número candidato independente escolhido,
    (Se repete aqui para cada cargo)
}
Int idUrna: Único para cada urna
```

#### Voto Urna
```
Int id: Único para cada voto
Int votoChapa: Número da chapa escolhida
Dict votosIndependentes: {
    cargo: Número candidato independente escolhido,
    (Se repete aqui para cada cargo)
}
Bool confirmado: 0 - Não confirmado 1 - Confirmado
Int idUrna: Único para cada urna
```

#### Chapa
```
Int id: Único para cada chapa, número que o eleitor vota
String nome: Nome da chapa pra mostrar na tela
Path foto: Foto da chapa reunida
Int votos: Número de votos naquele chapa**
List<Dict> membros: [{
    Int id: Único para cada membro, deve começar com id da chapa, modelo: idChapa * 100 + contador***
    String cargo: Cargo que concorre
    String nome: Nome do membro
}]
```
****Esse contador segue os cargos que serão:*
- 1 - Presidente
- 2 - Vice-presidente
- 3 - 1º Tesoureiro
- 4 - 2º Tesoureiro
- 5 - 1º Secretário
- 6 - 2º Secretário

#### Candidato
```
Int id: Único para cada candidato, número que o eleitor vota
String nome: Nome do candidato pra mostrar na tela
Path foto: Foto do candidato se quiser usar
String cargo: Cargo que o candidato concorre
Int votos: Número de votos naquele candidato**

```
***Chapa.votos e Candidato.votos são contadores reconstruídos a partir do log do host; em caso de divergência, o log quem manda*

#### Log 

Estrutura geral:
```
[evento](data-hora): dados
```

Log do Host:
```
[VERIFICACAO](data-hora): Identificador
[VOTO](data-hora): Voto Host
[ESTADO_ELEICAO](data-hora): { novoEstado: string }
```

Log da Urna:
```
[VOTO_ENVIADO](data-hora): Voto Host
[VOTO_CONFIRMADO](data-hora): { id: Int }
```

*Voto Urna é a estrutura reconstruída em memória ao ler o log local
(VOTO_ENVIADO + VOTO_CONFIRMADO, se existir) — não corresponde a uma
única linha gravada no arquivo*