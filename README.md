# c_programming_project

🐍 Snake Game in C (Console Version)

A simple Snake Game written in C using standard console functions.
This version works on Windows, Linux, and Mac (with minor changes).

🚀 Features

Real-time movement

Random food generation

Score tracking

Game-over detection

Simple + lightweight C code

▶️ Compile & Run
Windows
gcc snake.c -o snake
snake

Linux / Mac

Use -lncurses for input & screen handling:

gcc snake.c -o snake -lncurses
./snake

🧩 Snake Game Code (C)

✔ Works on Linux/Mac with ncurses
✔ Works on Windows using <conio.h> version
Below is the cross-platform ncurses version:

#include <ncurses.h>
#include <stdlib.h>
#include <time.h>

#define WIDTH 40
#define HEIGHT 20

int x, y, foodX, foodY, score;
int tailX[100], tailY[100];
int nTail = 0;
int gameOver = 0;
int dir; // 1=UP,2=DOWN,3=LEFT,4=RIGHT

void setup() {
    initscr();
    clear();
    noecho();
    cbreak();
    curs_set(0);

    x = WIDTH / 2;
    y = HEIGHT / 2;
    foodX = rand() % WIDTH;
    foodY = rand() % HEIGHT;
    score = 0;

    keypad(stdscr, TRUE);
    timeout(120);
}

void draw() {
    clear();

    // Boundary
    for (int i = 0; i < WIDTH + 2; i++) printw("#");
    printw("\n");

    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            if (j == 0) printw("#");

            if (i == y && j == x)
                printw("O");
            else if (i == foodY && j == foodX)
                printw("*");
            else {
                int printTail = 0;
                for (int t = 0; t < nTail; t++) {
                    if (tailX[t] == j && tailY[t] == i) {
                        printw("o");
                        printTail = 1;
                    }
                }
                if (!printTail) printw(" ");
            }

            if (j == WIDTH - 1) printw("#");
        }
        printw("\n");
    }

    for (int i = 0; i < WIDTH + 2; i++) printw("#");
    printw("\nScore: %d\n", score);
}

void input() {
    int c = getch();

    switch (c) {
        case KEY_UP:    dir = 1; break;
        case KEY_DOWN:  dir = 2; break;
        case KEY_LEFT:  dir = 3; break;
        case KEY_RIGHT: dir = 4; break;
    }
}

void logic() {
    int prevX = tailX[0];
    int prevY = tailY[0];
    int prev2X, prev2Y;

    tailX[0] = x;
    tailY[0] = y;

    for (int i = 1; i < nTail; i++) {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }

    switch (dir) {
        case 1: y--; break;
        case 2: y++; break;
        case 3: x--; break;
        case 4: x++; break;
    }

    if (x < 0 || x >= WIDTH || y < 0 || y >= HEIGHT)
        gameOver = 1;

    for (int i = 0; i < nTail; i++)
        if (tailX[i] == x && tailY[i] == y)
            gameOver = 1;

    if (x == foodX && y == foodY) {
        score++;
        foodX = rand() % WIDTH;
        foodY = rand() % HEIGHT;
        nTail++;
    }
}

int main() {
    srand(time(0));
    setup();

    while (!gameOver) {
        draw();
        input();
        logic();
    }

    clear();
    printw("GAME OVER!\nScore: %d\n", score);
    getch();
    endwin();
    return 0;
}

📺 Sample Terminal Output
##########################################
#                                        #
#      O                                 #
#      o                                 #
#      o     *                            #
#                                        #
##########################################
Score: 3


Game Over screen:

==========================
      GAME OVER!
       Score: 12
==========================
