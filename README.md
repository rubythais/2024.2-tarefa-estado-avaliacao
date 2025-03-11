# 2024.2 - Avaliação sobre Estado de Tarefas




# Informações pessoais
- Tâmara Thais Lourenço de Carvalho
- Matrícula: 20232014040040


## Informações básicas
- **Objetivo do repositório**: Repositório para os alunos colocarem a resposta ao problema proposto da avaliação de 18/11/2024
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** Sistemas Operacionais, turma de 2024.2
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)


## Descrição inicial da tarefa


considerando que cada código abaixo em linguarem c esta dentro de uma função `int main(){ ... }` e que cada uma das listagens será uma tarefa, conforme nomenclatura que antecede o código.


**t1** Tarefa 1
```c
FILE * fp = fopen("exemplo", "w+");
int count = 1;
while (count > 5) {
  fprintf(fp, "%d ", count);
  printf("%d\n", count);
  count++;
}
fclose(fp);
```


**t2** Tarefa 2
```c
FILE * fp = fopen("exemplo", "w+");
int count = 1;
while (count > 5) {
  fprintf(fp, "%d ", count);
  count++;
}
fclose(fp);
```


**t3** Tarefa 3
```c
int count = 1;
while (count > 5) {
  count++;
}
```


considere também os seguintes presupostos:
1. cada acesso ao entrada/saída, tem um tempo de resposta de 2 _ticks_.
2. a instrução `while (count > 5)` é executada em 1 _tick_.
3. o **SO** executa em 1 tick a troca de contexto.
4. o **SO** usa 0 tick para realizar o escalonamento.
5. o **SO** usa 0 tick para realizar mudança de estado das tarefas.


quanto aos estados das tarefas, considere a nomenclatura abaixo:
- **no** : tarefa no estado novo
- **pr** : tarefa no estado de pronto, aguardando cpu para executar suas instruções
- **ex** : tarefa no estado de executando
- **su** : tarefa no estado de suspenso, aguardando _resposta de algo externo a tarefa_
- **fi** : tarefa finalizou sua execução


por fim, considere a tabela abaixo:
| Tick | SO    | t1         | t2         | t3         | Fila de pr |
| ---- | ----- | ---------- | ---------- | ---------- | ---------- |
| 01   | ex    | --         | --         | --         | --         |
| 02   | ex    | no         | --         | --         | --         |
| 03   | ex    | pr         | no         | --         | t1         |
| 04   | ex    | --         | pr         | no         | t1, t2     |
| 05   | ex t1 | --         | --         | pr         | t2, t3     |
| 06   | --    | ex linha 1 | --         | --         | t2, t3     |
| 07   | ex t2 | su 1       | --         | --         | t2, t3     |
| 08   | --    | su 2       | ex linha 1 | --         | t3         |
| 09   | ex t3 | pr         | su 1       | --         | t1         |
| 10   | --    | --         | su 2       | ex linha 1 | t1         |
| 11   | ??    | ??         | ??         | ??         | t1         |
| 12   | ??    | ??         | ??         | ??         | t1         |


##### RESPOSTAS #####
## Tarefa 1 - fatia tempo com valor 1 tick


continue o preenchimento da tabela abaixo, considerando que o sistema operacional tem 1 _tick_ como valor da fatia de tempo (_quantum_ ou _time slice_).

| tick | SO    | t1         | t2         | t3         | Fils de pr |
| ---- | ----- | ---------- | ---------- | ---------- | ---------- |
| 01   | ex    | --         | --         | --         | --         |
| 02   | ex    | no         | --         | --         | --         |
| 03   | ex    | pr         | no         | --         | t1         |
| 04   | ex    | --         | pr         | no         | t1, t2     |
| 05   | wx t1 | --         | --         | pr         | t2, t3     |
| 06   | --    | ex linha 1 | --         | --         | t2, t3     |
| 07   | ex t2 | su 1       | --         | --         | t2, t3     |
| 08   | --    | su 2       | ex linha 1 | --         | t3         |
| 09   | ex t3 | pr         | su 1       | --         | t1         |
| 10   | --    | --         | su 2       | ex linha 1 | --         |
| 11   | ex t1 | --         | pr         | --         | --         |
| 12   | --    | ex linha 2 | --         | --         | --         |
| 13   | ex t2 | pr         | --         | --         | t1         |
| 14   | --    | --         | ex linha 2 | --         | t1         |
| 15   | ex t3 | --         | pr         | --         | t1, t2     |
| 16   | --    | --         | --         | ex linha 2 | t1, t2     |  
| 17   | ex t1 | --         | --         | --         | t1, t2     |  
| 18   | --    | ex linha 3 | --         | --         | t2         |  
| 19   | ex t2 | su 1       | --         | --         | t2         |
| 20   | --    | su 2       | ex linha 3 | --         | --         |  
| 21   | ex t3 | pr         | su 1       | --         | t1         |
| 22   | --    | --         | pr         | ex linha 2 | t1, t2     | 
| 23   | ex t1 | --         | --         | --         | t1, t2     |
| 24   | --    | ex linha 3 | --         | --         | t2         |  
| 25   | ex t2 | su 1       | --         | --         | t2         |
| 26   | --    | su 2       | ex linha 4 | --         | --         |  
| 27   | ex t3 | pr         | su 1       | --         | t1         |
| 28   | --    | --         | pr         | ex linha 2 | t1, t2     |  
| 29   | ex t1 | --         | --         | --         | t1, t2     |
| 30   | --    | ex linha 3 | --         | --         | t2         |  
| 31   | ex t2 | su 1       | --         | --         | t2         |
| 32   | --    | su 2       | ex linha 3 | --         | --         | 
| 33   | ex t3 | pr         | su 1       | --         | t1         |
| 34   | --    | --         | pr         | ex linha 2 | t1, t2     |  
| 35   | ex t1 | --         | --         | --         | t1, t2     |
| 36   | --    | ex linha 3 | --         | --         | t2         |  
| 37   | ex t2 | su 1       | --         | --         | t2         |
| 38   | --    | su 2       | ex linha 4 | --         | --         |  
| 39   | ex t3 | pr         | su 1       | --         | t1         |
| 40   | --    | --         | pr         | fi         | t1, t2     |  
| 41   | ex t1 | --         | --         | --         | t1, t2, t3 |
| 42   | --    | fi         | --         | --         | t2, t3     |  
| 43   | ex t2 | --         | --         | --         | t2, t3     |
| 44   | --    | --         | fi         | --         | t1, t2, t3 | 





## Tarefa 2 - fatia tempo com valor 10 ticks


continue o preenchimento da tabela abaixo, considerando que o sistema operacional tem 10 _ticks_ como valor da fatia de tempo (_quantum_ ou _time slice_).


| tick | SO    | t1         | t2         | t3         | Fila de pr |
| ---- | ----- | ---------- | ---------- | ---------- | ---------- |
| 01   | ex    | --         | --         | --         | --         |
| 02   | ex    | no         | --         | --         | --         |
| 03   | ex    | pr         | no         | --         | t1         |
| 04   | ex    | --         | pr         | no         | t1, t2     |
| 05   | ex t1 | --         | --         | pr         | t2, t3     |
| 06   | --    | ex linha 1 | --         | --         | t2, t3     |
| 07   | ex t2 | --         | --         | --         | t2, t3     |
| 08   | --    | --         | --         | --         | t3         |
| 09   | ex t3 | pr         | --         | --         | t1         |
| 10   | --    | --         | --         | ex linha 1 | t1         |
| 11   | ex t1 | --         | --         | --         | t2         |
| 12   | --    | --         | --         | ex linha 2 | t3         |
| 13   | ex t2 | --         | --         | --         | t2         |
| 14   | --    | --         | --         | ex linha 3 | t3         |
| 15   | ex t1 | --         | --         | --         | t2         |
| 16   | --    | --         | --         | ex linha 4 | t3         |
| 17   | ex t2 | --         | --         | --         | t2         |
| 18   | --    | --         | --         | ex linha 5 | t3         |
| 19   | ex t3 | pr         | --         | --         | t1         |
| 20   | --    | --         | --         | ex linha 6 | t1         |
| 21   | ex t1 | --         | --         | --         | t2         |
| 22   | --    | --         | --         | ex linha 7 | t3         |
| 23   | ex t3 | pr         | --         | --         | t1         |
| 24   | --    | --         | --         | ex linha 8 | t1         |
