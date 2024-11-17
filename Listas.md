#include <stdio.h>
#include <string.h>

#define MAX_ALUNOS 40

typedef struct {
    char nome[20];
    float nota;
    char status;
} Aluno;

typedef struct {
    Aluno alunos[MAX_ALUNOS];
    int total;
} ListaAlunos;


void incluirAluno(ListaAlunos *lista, char nome[], float nota, char status);
void excluirAluno(ListaAlunos *lista, char nome[]);
void ordenarPorNota(ListaAlunos *lista);
void listarAlunos(ListaAlunos *lista);
void listarAlunosAtivos(ListaAlunos *lista);


void incluirAluno(ListaAlunos *lista, char nome[], float nota, char status) {
    if (lista->total < MAX_ALUNOS) {
        strcpy(lista->alunos[lista->total].nome, nome);
        lista->alunos[lista->total].nota = nota;
        lista->alunos[lista->total].status = status;
        lista->total++;
    } else {
        printf("Limite de alunos atingido!\n");
    }
}


void excluirAluno(ListaAlunos *lista, char nome[]) {
    int i, encontrado = 0;
    for (i = 0; i < lista->total; i++) {
        if (strcmp(lista->alunos[i].nome, nome) == 0) {
            encontrado = 1;
            for (int j = i; j < lista->total - 1; j++) {
                lista->alunos[j] = lista->alunos[j + 1];
            }
            lista->total--;
            break;
        }
    }
    if (!encontrado) {
        printf("Aluno não encontrado!\n");
    }
}


void ordenarPorNota(ListaAlunos *lista) {
    int i, j;
    Aluno temp;
    for (i = 0; i < lista->total - 1; i++) {
        for (j = i + 1; j < lista->total; j++) {
            if (lista->alunos[i].nota < lista->alunos[j].nota) {
                temp = lista->alunos[i];
                lista->alunos[i] = lista->alunos[j];
                lista->alunos[j] = temp;
            }
        }
    }
}


void listarAlunos(ListaAlunos *lista) {
    if (lista->total == 0) {
        printf("Nenhum aluno cadastrado.\n");
        return;
    }
    printf("Lista de Alunos:\n");
    for (int i = 0; i < lista->total; i++) {
        printf("Nome: %s, Nota: %.2f, Status: %c\n",
               lista->alunos[i].nome,
               lista->alunos[i].nota,
               lista->alunos[i].status);
    }
}


void listarAlunosAtivos(ListaAlunos *lista) {
    int encontrados = 0;
    printf("Alunos com matrícula ativa:\n");
    for (int i = 0; i < lista->total; i++) {
        if (lista->alunos[i].status == 'A') {
            printf("Nome: %s, Nota: %.2f\n",
                   lista->alunos[i].nome,
                   lista->alunos[i].nota);
            encontrados++;
        }
    }
    if (encontrados == 0) {
        printf("Nenhum aluno com matrícula ativa.\n");
    }
}


int main() {
    ListaAlunos lista = {{}, 0};


    incluirAluno(&lista, "Alice", 9.5, 'A');
    incluirAluno(&lista, "Bob", 8.0, 'A');
    incluirAluno(&lista, "Carlos", 7.5, 'C');
    incluirAluno(&lista, "Diana", 10.0, 'A');


    printf("\nLista de Alunos antes da ordenação:\n");
    listarAlunos(&lista);


    ordenarPorNota(&lista);
    printf("\nLista de Alunos após a ordenação por nota:\n");
    listarAlunos(&lista);


    printf("\nListando alunos com matrícula ativa:\n");
    listarAlunosAtivos(&lista);


    excluirAluno(&lista, "Bob");
    printf("\nLista de Alunos após excluir Bob:\n");
    listarAlunos(&lista);

    return 0;
}
