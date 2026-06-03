#include <stdio.h>
#include <string.h>

int main() {

    int opcao = 0;

    char nomeArquivo[50];
    char mensagem[100];
    char texto[100];
    char nomeArquivoHorario[70];
    int opHorario;

    FILE *arquivo;
    FILE *lista;

    while(opcao != 4) {

        printf("\n========== MENU ==========\n");
        printf("1 - CADASTRO DOS REMEDIOS E SUAS ESPECIFICACOES\n");
        printf("2 - REMEDIOS CADASTRADOS\n");
        printf("3 - HORARIOS E DIAS DOS REMEDIOS\n");
        printf("4 - Sair\n");
        printf("==========================\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);

        switch(opcao) {

            case 1:

                printf("Digite o nome do seu remedio: ");
                scanf("%s", nomeArquivo);

                // Salva o nome do remÃ©dio na lista geral
                lista = fopen("remedios.txt", "a");

                if(lista != NULL) {
                    fprintf(lista, "%s\n", nomeArquivo);
                    fclose(lista);
                }

                arquivo = fopen(nomeArquivo, "a");

                if(arquivo == NULL) {
                    printf("Erro ao abrir o seu Remedio!\n");
                    break;
                }

                printf("Digite as especificacoes do seu remedio: ");
                scanf(" %[^\n]", mensagem);

                fprintf(arquivo, "%s\n", mensagem);

                fclose(arquivo);

                printf("Remedio %s cadastrado com sucesso!\n", nomeArquivo);

                break;

            case 2:

                arquivo = fopen("remedios.txt", "r");

                if(arquivo == NULL) {
                    printf("Nenhum remedio cadastrado!\n");
                    break;
                }

                printf("\n=== REMEDIOS CADASTRADOS ===\n\n");

                while(fgets(texto, sizeof(texto), arquivo) != NULL) {
                    printf("%s", texto);
                }

                fclose(arquivo);

                break;

            case 3:

                printf("Digite o nome do remedio para ver os horarios: ");
                scanf("%s", nomeArquivo);

                strcpy(nomeArquivoHorario, nomeArquivo);
                strcat(nomeArquivoHorario, "_horarios.txt");

                arquivo = fopen(nomeArquivoHorario, "r");

                if(arquivo == NULL) {

                    printf("Nenhum horario cadastrado para o remedio '%s' ainda.\n", nomeArquivo);
                    printf("Deseja cadastrar os dias e horarios agora? (1 - Sim / 0 - Nao): ");
                    scanf("%d", &opHorario);

                    if(opHorario == 1) {

                        arquivo = fopen(nomeArquivoHorario, "w");

                        if(arquivo == NULL) {
                            printf("Erro ao criar arquivo de horarios!\n");
                            break;
                        }

                        printf("Digite os dias e horarios (Ex: Segunda a Sexta as 08:00 e 20:00): ");
                        scanf(" %[^\n]", mensagem);

                        fprintf(arquivo, "%s\n", mensagem);

                        fclose(arquivo);

                        printf("Horarios salvos com sucesso!\n");
                    }

                } else {

                    printf("\n=== HORARIOS E DIAS DO REMEDIO %s ===\n\n", nomeArquivo);

                    while(fgets(texto, sizeof(texto), arquivo) != NULL) {
                        printf("%s", texto);
                    }

                    fclose(arquivo);
                }

                break;

            case 4:

                printf("Encerrando programa...\n");
                break;

            default:

                printf("Opcao invalida!\n");
        }
    }

    return 0;
}
