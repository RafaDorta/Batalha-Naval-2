# Batalha-Naval-2
NAVIOS ALOCADOS ALEATORIAMENTE PELO PROGRAMA

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#include <windows.h>
#include <time.h>

void imprime_mapa(int **mapa, int d, int *vence) //a funçao serve para imprimir o mapa para players
{

    int i,j;
    printf("\n\n\t          Jogador %d", d+1);
    printf("\n\n     1   2   3   4   5   6   7   8   9   10              ACERTOU %d de 20 \n", *vence);
    for(i=0; i<10; i++)
    {
        printf("\n    ---+---+---+---+---+---+---+---+---+---+");
        printf("\n [%c]", 65+ i);
        for(j=0; j<10; j++)
        {
            if (mapa[i][j] == 0 || mapa[i][j] == 1)
            {
                printf(" ~ |");
            }
            else
            {
                if(mapa[i][j] == 2)
                {
                    printf(" X |");
                }
                else
                {
                    if(mapa[i][j] == -1)
                    {
                        printf(" O |");
                    }
                }
            }
        }
    }


};

void imprime_mapa2(int **mapa,int *vence)//a funçao serve para imprimir o mapa para o bot
{

    int i,j;
    printf("\n\n\t              BOT");
    printf("\n\n     1   2   3   4   5   6   7   8   9   10              ACERTOU %d de 20 \n", *vence);
    for(i=0; i<10; i++)
    {
        printf("\n    ---+---+---+---+---+---+---+---+---+---+");
        printf("\n [%c]", 65+ i);
        for(j=0; j<10; j++)
        {
            if (mapa[i][j] == 0 || mapa[i][j] == 1)
            {
                printf(" ~ |");
            }
            else
            {
                if(mapa[i][j] == 2)
                {
                    printf(" X |");
                }
                else
                {
                    if(mapa[i][j] == -1)
                    {
                        printf(" O |");
                    }

                }
            }
        }
    }


};


int **alocacao_espacao_matriz(int N, int M) // a função aloca dinamicamente o mapa de cada player
{

    int **mat,i;
    mat = (int**) calloc (N,sizeof(int*));
    if(mat==NULL)
        exit (1);
    for (i=0; i<N; i++)
    {
        mat[i] = (int*)calloc (M,sizeof(int));
        if(mat[i]==NULL)
            exit (1);
    }
    return mat;
}



void escohe_tiro(int **mapa, char *letra, int *fileira, int *vence) //a funcao serve pra que o player escolha aonde sera o tiro dele
{


    do
    {
        printf("\nLetra: ");
        scanf("%c", letra);
        fflush(stdin);
        *letra = toupper(*letra);
    }
    while(*letra < 65 || *letra >74);

    do
    {
        printf("\n\nFileira: ");
        scanf("%d", fileira);
        fflush(stdin);


    }
    while(*fileira < 1 || *fileira >10);

    if (mapa[*letra -65][*fileira-1]==0)
    {
        system("color 40");
        Sleep(100);
        system("color 70");
        Sleep(100);
        system("color 40");
        Sleep(100);
        system("color 70");
        mapa[*letra -65][*fileira-1] = -1;
        printf("\nAGUAAAA !!!!!!\n");
        Sleep(1000);
    }
    else
    {
        if (mapa[*letra -65][*fileira-1]==1)
        {
            system("color 20");
            Sleep(100);
            system("color 70");
            Sleep(100);
            system("color 20");
            Sleep(100);
            system("color 70");
            mapa[*letra -65][*fileira-1] = 2;
            (*vence) ++;
            printf("\nACERTOUUU !!!!!!\n");
            Sleep(1000);
        }
    }
    system("cls");
}

void escolhe_fase(int **mapa)// a funcao sorteia aleatoriamente as posiçoes dos navios
{


    int j;
    srand((unsigned)time(NULL));
    int x,y,i;

    do
    {
        x =  rand() % 10 ;
        y =  rand() % 10 ;

    }
    while(y == 0 || y == 8 || y==9);

    mapa[x][y-1] = 1;
    mapa[x][y] = 1;
    mapa[x][y+1] = 1;
    mapa[x][y + 2] = 1;


    for (i=0; i<2; i++)
    {

        do
        {
            x =  rand() % 10 ;
            y =  rand() % 10 ;
        }
        while( x == 0 || mapa[x][y] == 2 || mapa[x-1][y] == 2|| mapa[x+1][y] == 2 || x == 9);
        mapa[x][y] = 1;
        mapa[x-1][y] = 1;
        mapa[x+1][y] = 1;

    }


    for (i=0; i<3; i++)
    {

        do
        {
            x =  rand() % 10 ;
            y =  rand() % 10 ;

        }
        while( mapa[x][y] == 2 || mapa[x][y+1] == 2 || y == 9);
        mapa[x][y] = 1;
        mapa[x][y + 1] = 1;



    }



    for (i=0; i<4; i++)
    {
        do
        {
            x =  rand() % 10 ;
            y =  rand() % 10 ;

        }
        while(mapa[x][y] == 2);
        mapa[x][y] = 1;



    }




}

void bot_escolhe(int **mapa, char *letra, int *fileira, int *vence) // a funcao escolhe aonde sera o tiro do bot
{

    srand((unsigned)time(NULL));
    do
    {
        char x;
        do
        {
            x = 65 + ( rand() % 10 );

            *letra = x;
        }
        while(*letra < 65 || *letra >74);


        int y;
        do
        {
            y = 1 + ( rand() % 10 );
            *fileira = y;
        }
        while(*fileira < 1 || *fileira >10);
    }
    while(mapa[*letra -65][*fileira-1] == 2 || mapa[*letra -65][*fileira-1] ==-1);

    if (mapa[*letra -65][*fileira-1]==0)
    {
        system("color 40");
        Sleep(100);
        system("color 70");
        Sleep(100);
        system("color 40");
        Sleep(100);
        system("color 70");
        mapa[*letra -65][*fileira-1] = -1;
        printf("\nAGUAAAA !!!!!!\n");
        Sleep(1000);
    }
    else
    {
        if (mapa[*letra -65][*fileira-1]==1)
        {
            system("color 20");
            Sleep(100);
            system("color 70");
            Sleep(100);
            system("color 20");
            Sleep(100);
            system("color 70");
            mapa[*letra -65][*fileira-1] = 2;
            printf("\nACERTOUUU !!!!!!\n");
            Sleep(1000);
            (*vence)++;
        }
    }

}



void escolha_bot(int **mapa)// a funcao guarda as posicoes que o bot devera acertar
{

    mapa[6][2]= mapa[5][4] = mapa[5][0] = mapa[3][9]  = 1;
    mapa[4][18] = mapa [4][9] = 1;
    mapa[5][7] = mapa[5][8] = 1;
    mapa[4][3] = mapa[3][3] = 1;
    mapa[0][2] = mapa[0][3] = mapa[0][1] = 1;
    mapa[7][3] = mapa[7][4] = mapa[7][5] = 1;
    mapa[6][7] = mapa[6][8] = mapa[6][6] = mapa[6][9] =1;
}
void comeco() // a funcao imprime o cabecalho
{

    system("cls");
    printf("==========================================================\n");
    printf("\n             BEM-VINDO AO BATALHA NAVAL\n\n==========================================================\n    \n DESENVOLVIDO POR:\n-Rafael Alves Dorta      RA: 20032256\n-J\243lia de Miranda Gomes  RA: 20052387\n-Jo\306o Pedro C. Zanholo   RA: 20039822\n");
    printf("==========================================================\n");
    printf("\n\n REGRAS:\n- X -> ACERTO\n- O -> ERROU\n- ~ -> AINDA NAO FOI ATINGIDO\n\n");

}

void ganhou_1() // a funcao imprime se o player 1 ganhar
{

    system("cls");
    printf("\n\n=========================================================\n");
    printf("             Parabens Jogador 1 voce Ganhou !");
    printf("\n=========================================================\n");
    Sleep(3000);
}
void ganhou_2()// a funcao imprime se o player 2 ganhar
{

    system("cls");
    printf("\n\n=========================================================\n");
    printf("             Parabens Jogador 2 voce Ganhou !");
    printf("\n=========================================================\n");
    Sleep(3000);
}
void perdeu()// a funcao imprime se o bot ganhar
{

    system("cls");
    printf("\n\n=========================================================\n");
    printf("                      Voce Perdeu !");
    printf("\n=========================================================\n");
    Sleep(3000);
}

void libera(int **mapa, int **mapa1) // a funcao libera os mapas que foram alocadas
{

    int i;

    for (i=0; i<10; i++)
    {
        free(mapa[i]);
        free(mapa1[i]);
    }
    free(mapa);
    free(mapa1);

}

int main()
{

    int saida = 2; // variavel para a saida do programa
    int **M1[2]; //variavel que ficara os mapas

    int  v1,v2; // controle de acertos de tiros
    int fileira,n; // escolha da fileira do tiro e controle para saber se é contra player ou bot
    char letra; // escolha da letra do tiro
    do
    {


        system("color 70");
        v1 = 0;
        v2 = 0;
        comeco();
        M1[0] = alocacao_espacao_matriz(10,10);
        M1[1] = alocacao_espacao_matriz(10,10);

        escolhe_fase(M1[0]);
        printf("====================================================================\n");
        printf("\nDigite 1 para jogar contra o BOT ou 2 para ter um segundo jogador: ");
        scanf("%d", &n);
        fflush(stdin);
        if(n==2)
        {
            escolhe_fase(M1[1]);

            do
            {
                system("cls");
                imprime_mapa(M1[0], 0,&v1);
                escohe_tiro(M1[0], &letra, &fileira, &v1);
                printf("\n\t\t\t\tVIZUALIZACAO DO TIRO\n");
                imprime_mapa(M1[0], 0,&v1);
                printf("\n\n");
                system("pause");
                system("cls");
                imprime_mapa(M1[1], 1,&v2);
                escohe_tiro(M1[1], &letra, &fileira, &v2);
                printf("\n\t\t\t\tVIZUALIZACAO DO TIRO\n");
                imprime_mapa(M1[1], 1,&v2);
                printf("\n\n");
                system("pause");

            }
            while (v1 <20 && v2 < 20);
            if(v1 == 20)
            {
                ganhou_1();
            }
            else
            {
                ganhou_2();
            }
        }
        else
        {
            if (n==1)
            {
                do
                {
                    system("cls");
                    escolha_bot(M1[1]);
                    imprime_mapa(M1[0], 0,&v1);
                    escohe_tiro(M1[0], &letra, &fileira, &v1);
                    printf("\n\t\t\t\tVIZUALIZACAO DO TIRO\n");
                    imprime_mapa(M1[0], 0,&v1);
                    printf("\n\n");
                    system("pause");
                    system("cls");
                    imprime_mapa2(M1[1],&v2);
                    bot_escolhe(M1[1], &letra, &fileira, &v2);
                    system("cls");
                    printf("\n\t\t\t\tVIZUALIZACAO DO TIRO\n");
                    imprime_mapa2(M1[1],&v2);
                    printf("\n\n");
                    system("pause");
                }
                while (v1 <2 && v2 < 20);

                if(v1 == 2)
                {
                    ganhou_1();
                }
                else
                {
                    perdeu();
                }
            }
        }
        libera(M1[0],M1[1]);
        printf("\n\nDESEJA JOGAR NOVAMENTE ? <1> PARA SAIR e <2> PARA CONTINUAR: ");
        scanf("%d", &saida);
        fflush(stdin);
    }
    while(saida != 1);
    printf("\n=====================================================");
    printf("\n                OBRIGADO POR JOGAR !   \n");
    printf("\n=====================================================");
    Sleep(3000);
    return 0;
}
