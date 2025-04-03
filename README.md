# Jogos-Linguagem-C-Eng-Comp-
#include <stdio.h>
#include <stdbool.h>
#include <string.h>
#include <locale.h>

// Funções auxiliares para trabalhar com strings
int comparar_strings(char *s1, char *s2) {
    while(*s1 && *s2) {
        if(*s1 != *s2) return 0;
        s1++;
        s2++;
    }
    return (*s1 == '\0' && *s2 == '\0');
}

void copiar_string(char *destino, char *origem) {
    while(*origem) {
        *destino = *origem;
        destino++;
        origem++;
    }
    *destino = '\0';
}

int contar_caracteres(char *str) {
    int contador = 0;
    while(*str++) contador++;
    return contador;
}

// Função de ordenação: Odd-Even Sort
void ordenar_elementos(int *arr, int tamanho) {
    int ordenado = 0;
    while(!ordenado) {
        ordenado = 1;
        for(int i = 1; i < tamanho - 1; i += 2) {
            if(arr[i] > arr[i + 1]) {
                int temp = arr[i];
                arr[i] = arr[i + 1];
                arr[i + 1] = temp;
                ordenado = 0;
            }
        }
        for(int i = 0; i < tamanho - 1; i += 2) {
            if(arr[i] > arr[i + 1]) {
                int temp = arr[i];
                arr[i] = arr[i + 1];
                arr[i + 1] = temp;
                ordenado = 0;
            }
        }
    }
}

// Definição de cores ANSI para facilitar a visualização no terminal
#define VERMELHO "\033[1;31m"
#define VERDE "\033[1;32m"
#define AZUL "\033[1;34m"
#define AMARELO "\033[1;33m"
#define RESET "\033[0m"

// Quiz de Engenharia
int quiz_engenharia() {
    struct Pergunta {
        char pergunta[150];
        char opcoes[4][50];
        int resposta_correta;
    };

    struct Pergunta questoes[5] = {
        {"Que tipo de linguagem esta sendo usada ?", {"Java", "Python", "C", "Ruby"}, 2},
        {"O que significa CPU?", {"Unidade Central de Processamento", "Unidade de Processamento de Computadores", "Processador Central de Unidade", "Unidade Pessoal de Computador"}, 0},
        {"Qual não é um tipo de dado em C?", {"int", "float", "string", "double"}, 2},
        {"Qual protocolo é utilizado para sites?", {"FTP", "HTTP", "SMTP", "UDP"}, 1},
        {"O que é um algoritmo?", {"Dispositivo de hardware", "Linguagem de programação", "Conjunto de instruções", "Sistema operacional"}, 2}
    };

    int pontos = 0;
    printf(AMARELO "\n=== QUIZ DE ENGENHARIA ===\n" RESET);

    for(int i = 0; i < 5; i++) {
        printf("\nPergunta %d: %s\n", i + 1, questoes[i].pergunta);
        for(int j = 0; j < 4; j++) {
            printf("%d) %s\n", j + 1, questoes[i].opcoes[j]);
        }

        int resposta_usuario;
        printf("Sua resposta: ");
        scanf("%d", &resposta_usuario);
        while(getchar() != '\n');

        if(resposta_usuario - 1 == questoes[i].resposta_correta) {
            printf(VERDE "Resposta correta!\n" RESET);
            pontos++;
        } else {
            printf(VERMELHO "Errado! A resposta certa é: %d\n" RESET, questoes[i].resposta_correta + 1);
        }
    }

    printf("\nSua pontuação final: %d/5\n", pontos);
    return 0;
}

// Jogo da Roleta de Caixas
int roleta_caixas() {
    char jogadores[7][20] = {"John", "Mary", "Carl", "Anna", "Peter", "Sophie", "Mike"};

    printf(AMARELO "\n=== ROLETAO DE CAIXAS ===\n" RESET);

    int jogador1, jogador2;
    printf("Jogador 1 (Escolha de 1 a 7):\n");
    for(int i = 0; i < 7; i++) printf("%d. %s\n", i + 1, jogadores[i]);
    scanf("%d", &jogador1);
    while(getchar() != '\n');

    printf("Jogador 2 (Escolha de 1 a 7):\n");
    for(int i = 0; i < 7; i++) printf("%d. %s\n", i + 1, jogadores[i]);
    scanf("%d", &jogador2);
    while(getchar() != '\n');

    // Geração de número aleatório simples sem usar 'rand'
    int semente = (jogador1 + jogador2) * 1000;
    int premio = (semente % 5) + 1;
    int perigo = (semente % 4) + 1;
    if(perigo >= premio) perigo++;

    int vez = (semente % 2);
    printf(VERDE "\n%s começa!\n" RESET, vez ? jogadores[jogador2 - 1] : jogadores[jogador1 - 1]);

    while(1) {
        printf("\n%s, escolha uma caixa (1-5): ", vez ? jogadores[jogador2 - 1] : jogadores[jogador1 - 1]);
        int escolha;
        scanf("%d", &escolha);
        while(getchar() != '\n');

        if(escolha == premio) {
            printf(VERDE "PARABÉNS! Você encontrou o prêmio!\n" RESET);
            break;
        } else if(escolha == perigo) {
            printf(VERMELHO "PERDEU! Caixa amaldiçoada!\n" RESET);
            break;
        } else {
            printf("Caixa vazia. Próxima vez!\n");
            vez = !vez;
        }
    }

    return 0;
}

// Jogo de Batalha de Cavaleiros
int batalha_cavaleiros() {
    typedef struct {
        char nome[20];
        int cavaleiros[2];
    } Jogador;

    printf(AMARELO "\n=== BATALHA DE CAVALEIROS ===\n" RESET);

    Jogador jogador1, jogador2;

    printf("Nome do Jogador 1: ");
    fgets(jogador1.nome, 20, stdin);
    jogador1.nome[contar_caracteres(jogador1.nome) - 1] = '\0';

    printf("Nome do Jogador 2: ");
    fgets(jogador2.nome, 20, stdin);
    jogador2.nome[contar_caracteres(jogador2.nome) - 1] = '\0';

    jogador1.cavaleiros[0] = jogador1.cavaleiros[1] = 10;
    jogador2.cavaleiros[0] = jogador2.cavaleiros[1] = 10;

    int turno = 0;

    while(1) {
        Jogador *atual = turno ? &jogador2 : &jogador1;
        Jogador *oponente = turno ? &jogador1 : &jogador2;

        printf(AZUL "\n--- Turno de %s ---\n" RESET, atual->nome);
        printf("Seus cavaleiros: %d | %d\n", atual->cavaleiros[0], atual->cavaleiros[1]);
        printf("Cavaleiros do oponente: %d | %d\n", oponente->cavaleiros[0], oponente->cavaleiros[1]);

        printf("\n1. Atacar\n2. Transferir aura\n3. Meditar\n");
        int acao;
        printf("Escolha: ");
        scanf("%d", &acao);
        while(getchar() != '\n');

        if(acao == 1) {
            int atacante, defensor;
            printf("Cavaleiro atacante (1-2): ");
            scanf("%d", &atacante);
            printf("Cavaleiro defensor (1-2): ");
            scanf("%d", &defensor);
            while(getchar() != '\n');

            int dano = (atacante + defensor) % 3 + 2;
            oponente->cavaleiros[defensor - 1] -= dano;

            printf(VERMELHO "%d de dano causado!\n" RESET, dano);

            if(oponente->cavaleiros[defensor - 1] <= 0) {
                oponente->cavaleiros[defensor - 1] = 0;
                printf("Cavaleiro %d derrotado!\n", defensor);
            }
        } else if(acao == 2) {
            int origem, destino;
            printf("Transferir de (1-2): ");
            scanf("%d", &origem);
            printf("Para (1-2): ");
            scanf("%d", &destino);
            while(getchar() != '\n');

            int quantidade = atual->cavaleiros[origem - 1] / 2;
            atual->cavaleiros[origem - 1] -= quantidade;
            atual->cavaleiros[destino - 1] += quantidade;

            printf(VERDE "%d de aura transferida\n" RESET, quantidade);
        } else if(acao == 3) {
            int cavaleiro;
            printf("Cavaleiro para meditar (1-2): ");
            scanf("%d", &cavaleiro);
            while(getchar() != '\n');

            int recuperacao = (cavaleiro + turno) % 2 + 3;
            atual->cavaleiros[cavaleiro - 1] += recuperacao;

            printf(AMARELO "%d de aura recuperada\n" RESET, recuperacao);
        }

        if((oponente->cavaleiros[0] <= 0 && oponente->cavaleiros[1] <= 0)) {
            printf(VERDE "\n%s VENCEU!\n" RESET, atual->nome);
            break;
        }

        turno = !turno;
    }

    return 0;
}

// Menu principal
int mostrar_menu() {
    int escolha;
    printf(AZUL "\nMENU PRINCIPAL\n" RESET);
    printf("1. Quiz de Engenharia\n");
    printf("2. Roletão de Caixas\n");
    printf("3. Batalha de Cavaleiros\n");
    printf("4. Sair\n");
    
    printf("Escolha: ");
    scanf("%d", &escolha);
    while(getchar() != '\n');
    
    return escolha;
}

int main() {
	setlocale(LC_ALL, "");
    while(1) {
        int opcao = mostrar_menu();
        
        if(opcao == 1) quiz_engenharia();
        else if(opcao == 2) roleta_caixas();
        else if(opcao == 3) batalha_cavaleiros();
        else if(opcao == 4) break;
        
        if(opcao != 4) {
            printf("\nPressione ENTER para continuar...");
            while(getchar() != '\n');
        }
    }
    
    printf("\nAté logo!\n");
    return 0;
}
