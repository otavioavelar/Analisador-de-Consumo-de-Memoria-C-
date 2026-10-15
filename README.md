# Analisador-de-Consumo-de-Memoria-C-
Este programa em C coleta, armazena em "memoria.txt" e analisa dados de consumo de memória (em MB). Ele interage com o usuário para registrar medições sequenciais e produz um relatório estatístico completo contendo: total de medições, média, menor e maior consumo, itens acima da média e o maior aumento consecutivo.



// Identificação
  //Nome: Otávio Guimarães Nogueira Avelar
  //Curso: Ciência da Computação
  //Faculdade : Puc Minas 


// Analisador de Consumo de Memoria (C)

// Descrição e Objetivo : 
//Este projeto consiste em um programa desenvolvido em linguagem C que tem como objetivo coletar, armazenar, ler e analisar dados de consumo de memória (em MB). 
//O sistema interage com o usuário para registrar medições sequenciais em um arquivo de texto (`memoria.txt`), realizando posteriormente o processamento desses dados para gerar um relatório estatístico completo. O relatório inclui:
// - Quantidade total de medições realizadas.
// - Consumo médio de memória.
// - Menor e maior consumo registrado.
// - Quantidade de medições com consumo acima da média.
// - O maior aumento consecutivo de consumo registrado entre uma medição e outra.

// ---

// ##  Linguagem e Tecnologias Utilizadas
// - Linguagem: C (Padrão Cpp ou superior)
// - Manipulação de Arquivos: Biblioteca padrão (`stdio.h`, `stdlib.h`)
// - Sistema Operacional: Compatível com qualquer SO que possua um compilador C (Linux, Windows, macOS).

// ---

// Orientações para Execução e Utilização

// ### Pré-requisitos
//Você precisará de um compilador de C instalado em sua máquina (como o **GCC**, **Clang** ou o ambiente do **Dev-C++** / **Code::Blocks**).

// ### Passo a passo para compilar e executar via terminal (Linux / macOS / Windows com MinGW):

// 1. **Clone o repositório ou salve o código** em um arquivo chamado `main.c`.
// 2. **Compile o programa** usando o GCC no terminal:
//   ```bash
//   gcc main.c -o analisador_memoria







#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *arquivo;
    float valor, soma = 0, menor = 0, maior = 0, anterior = 0, maior_aumento = 0, media = 0;
    int qtd = 0, acima_media = 0;

    // 1. Gravação no arquivo memoria.txt
    arquivo = fopen("memoria.txt", "w");
    if (arquivo == NULL) {
        printf("Erro ao abrir o arquivo para gravacao!\n");
        return 1;
    }

    printf("Digite o consumo de memoria em MB (digite 0 para encerrar):\n");
    while (1) {
        printf("Valor: ");
        if (scanf("%f", &valor) != 1) {
            printf("Entrada invalida.\n");
            while(getchar() != '\n'); // Limpa buffer
            continue;
        }
        if (valor == 0) {
            break;
        }
        if (valor < 0) {
            printf("O consumo nao pode ser negativo. Tente novamente.\n");
            continue;
        }
        fprintf(arquivo, "%.2f\n", valor);
    }
    fclose(arquivo);

    // 2. Leitura e processamento inicial com fscanf()
    arquivo = fopen("memoria.txt", "r");
    if (arquivo == NULL) {
        printf("Arquivo memoria.txt nao encontrado ou vazio.\n");
        return 1;
    }

    while (fscanf(arquivo, "%f", &valor) == 1) {
        if (qtd == 0) {
            menor = valor;
            maior = valor;
        } else {
            if (valor < menor) menor = valor;
            if (valor > maior) maior = valor;
            
            float aumento = valor - anterior;
            if (aumento > maior_aumento) {
                maior_aumento = aumento;
            }
        }
        soma += valor;
        anterior = valor;
        qtd++;
    }
    fclose(arquivo);

    if (qtd == 0) {
        printf("\nNenhuma medicao foi realizada.\n");
        return 0;
    }

    media = soma / (float)qtd;

    // 3. Segunda leitura para contar medições acima da média
    arquivo = fopen("memoria.txt", "r");
    if (arquivo != NULL) {
        while (fscanf(arquivo, "%f", &valor) == 1) {
            if (valor > media) {
                acima_media++;
            }
        }
        fclose(arquivo);
    }

    // 4. Exibição dos Resultados Finais
    printf("\n--- RELATORIO DE CONSUMO DE MEMORIA ---\n");
    printf("Quantidade de medicoes realizadas: %d\n", qtd);
    printf("Consumo medio de memoria: %.2f MB\n", media);
    printf("Menor consumo registrado: %.2f MB\n", menor);
    printf("Maior consumo registrado: %.2f MB\n", maior);
    printf("Medicoes com consumo superior a media: %d\n", acima_media);
    printf("Maior aumento consecutivo entre medicoes: %.2f MB\n", maior_aumento);

    return 0;
}
