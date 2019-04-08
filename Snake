#include "pch.h"
#include <iostream>
#include <Windows.h>
#include <conio.h>
#include <ctime>

using namespace std;
bool gameOver;
const int width = 20;
const int height = 10;
int x, y, fruitX, fruitY, score;
int tailX[100], tailY[100]; //координаты хвоста
int nTail; // количество элементов в хвосте
int Speed = 600; //скорость передвижения "змеи"
enum eDirection
{
	STOP = 0, LEFT, RIGHT, UP, DOWN
};
eDirection dir;
int p = 0;                                            // Переменная служит для проверки "о" в верхнем левом углу

void Setup()
{
	gameOver = false;
	dir = STOP;
	x = width / 2 - 1; // стартовая позиция - середина
	y = height / 2 - 1; // стартовая позиция - середина
	fruitX = rand() % width; // случайная координата по x на всю ширину
	fruitY = rand() % height; // случайная координата по y на всю высоту
	score = 0; // общий счет равен 0 при старте

}
void Draw() // функция рисования на карте
{
	system("cls"); // очищение экрана
	for (int i = 0; i <= width; i++) //прорисовка верхней границы игры
	{
		cout << "#";
	}
	cout << endl;

	for (int i = 0; i < height; i++) // прорисовка боковых границ и заполнение поля пустыми ячейками
	{
		for (int j = 0; j < width; j++)
		{
			if (j == 0 || j == width - 1)
			{
				cout << "#";
			}

			if (i == y && j == x)
			{
				cout << "0";
			}
			else if (i == fruitY && j == fruitX)
			{
				cout << "F";
			}
			else
			{
				bool print = false;
				for (int k = 0; k < nTail; k++)
				{
					if (tailX[k] == j && tailY[k] == i)
					{
						if (p != 1)
						{
							print = true;
							cout << 'o';
						}
					}
				} 
				if (!print)
					cout << " ";
			}
		}
		cout << endl;
	}

	for (int i = 0; i < width + 1; i++) //прорисовка нижней границы игры
	{
		cout << "#";
	}
	cout << endl;
	cout << "Score: " << score << endl;

	

}
void Input()
{
	if (score == 0) {
		if (_kbhit()) // функция проверяет, что нажал пользователь на клавиатуре
		{
			switch (_getch()) // функция ждёт нажатия любой клавиши и возвращает её код;
			{
			case 'a':
				dir = LEFT;
				break;
			case 'd':
				dir = RIGHT;
				break;
			case 'w':
				dir = UP;
				break;
			case 's':
				dir = DOWN;
				break;
			case 'x':
				gameOver = true;
				break;
			}
		}
	}
	else {
		if (_kbhit()) { // функция для считывания нажатия клавиш и движения "змеи"
			switch (_getch())
			{
			case 'a':
				if (dir != RIGHT)
					dir = LEFT;
				break;
			case 's':
				if (dir != UP)
					dir = DOWN;
				break;
			case 'd':
				if (dir != LEFT)
					dir = RIGHT;
				break;
			case 'w':
				if (dir != DOWN)
					dir = UP;
				break;
			case 'x':
				gameOver = true;
				break;
			}
		}
	}
}

void Logic()
{
	int prevX = tailX[0]; //предыдущая позиция по X (первый элемент хвоста)
	int prevY = tailY[0]; //предыдущая позиция по Y (первый элемент хвоста)
	int prev2X, prev2Y; //переменные в которые помещаются следующие элементы хвоста
	tailX[0] = x;
	tailY[0] = y;
	for (int i = 1; i < nTail; i++)
	{
		prev2X = tailX[i];
		prev2Y = tailY[i];
		tailX[i] = prevX;
		tailY[i] = prevY;
		prevX = prev2X;
		prevY = prev2Y;
	}
	switch (dir)
	{
	case LEFT:
		x--;
		break;
	case RIGHT:
		x++;
		break;
	case UP:
		y--;
		break;
	case DOWN:
		y++;
		break;
	}

	/*if (x > width-2 || x < 0 || y > height-1 || y < 0) // чтобы змея не проходила сквозь стенку
	{
		gameOver = true;
	}*/
	if (x >= width - 1)
	{
		x = 0;
	}
	else if (x < 0)
	{
		x = width - 2;
	}
	if (y >= height)
	{
		y = 0;
	}
	else if (y < 0)
	{
		y = height - 1;
	}
	for (int i = 0; i < nTail; i++) // если прикоснулся к хвосту - проиграл
	{
		if (tailX[i] == x && tailY[i] == y)
		{
			gameOver = true;
		}
	}
	p = 0;
	if (x == fruitX && y == fruitY) // функция, чтобы змея "ела" фрукты и прибавлялись очки
	{
		score += 10;
		do
		{
			fruitX = rand() % width-1;
			fruitY = rand() % height;
		} while (fruitX < 1 || fruitX > width || fruitY < 1 || fruitY > height);
		
		nTail++;
		Speed -= 25;

		p = 1;
	}
	if (Speed <= 100)
	{
		Speed = 100;
	}
}

int main()
{
	srand(time(NULL));
	Setup();
	while (!gameOver)
	{
		Draw();
		Input();
		Sleep(Speed); //задержка, чтобы змея медленнее перемещалась по экрану
		Logic();
	}
	return 0;
}
