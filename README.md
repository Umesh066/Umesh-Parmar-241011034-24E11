#include <stdio.h>

char board[3][3];

void initializeBoard() {
    char ch = '1';
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            board[i][j] = ch++;
}

void printBoard() {
    printf("\n");
    for (int i = 0; i < 3; i++) {
        printf(" %c | %c | %c \n", board[i][0], board[i][1], board[i][2]);
        if (i < 2)
            printf("---|---|---\n");
    }
    printf("\n");
}

int checkWin() {
    for (int i = 0; i < 3; i++) {
        if (board[i][0]==board[i][1] && board[i][1]==board[i][2])
            return 1;
        if (board[0][i]==board[1][i] && board[1][i]==board[2][i])
            return 1;
    }
    if (board[0][0]==board[1][1] && board[1][1]==board[2][2])
        return 1;
    if (board[0][2]==board[1][1] && board[1][1]==board[2][0])
        return 1;
    return 0;
}

int isDraw() {
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            if (board[i][j] != 'X' && board[i][j] != 'O')
                return 0;
    return 1;
}

int validMove(int pos) {
    if (pos < 1 || pos > 9)
        return 0;
    int row = (pos - 1) / 3;
    int col = (pos - 1) % 3;
    if (board[row][col] == 'X' || board[row][col] == 'O')
        return 0;
    return 1;
}

void makeMove(int pos, char mark) {
    int row = (pos - 1) / 3;
    int col = (pos - 1) % 3;
    board[row][col] = mark;
}

int main() {
    int player = 1, pos;
    char mark;
    initializeBoard();

    while (1) {
        printBoard();
        mark = (player == 1) ? 'X' : 'O';
        printf("Player %d (%c), enter position (1-9): ", player, mark);
        scanf("%d", &pos);

        if (!validMove(pos)) {
            printf("Invalid move! Try again.\n");
            continue;
        }

        makeMove(pos, mark);

        if (checkWin()) {
            printBoard();
            printf("Player %d (%c) wins!\n", player, mark);
            break;
        }

        if (isDraw()) {
            printBoard();
            printf("It's a draw!\n");
            break;
        }

        player = (player == 1) ? 2 : 1;
    }

    return 0;
}
