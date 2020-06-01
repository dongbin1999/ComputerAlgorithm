## 원의 경계점까지 포함해서 카운트한 경우

~~~c++
#include <cstdio>
#include <Windows.h>
#include <ctime>
#include <conio.h>
#include <cstdlib>
using namespace std;
#define SIZE 400

struct point
{
	double x, y;
};

double dist(point a, point b)
{
	return ((a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y));
}

int main(void)
{
	srand(time(NULL));
	HWND hwnd = GetConsoleWindow();
	HDC hdc = GetDC(hwnd);

	int x, y, i = 0, cnt = 0;
	point center = { (double)SIZE / (double)2, (double)SIZE / (double)2 }, p, cursor = { 0,0 };
	double R = (double)SIZE / (double)2;
	while (i++ <= 10000000)
	{
		x = rand() % SIZE, y = rand() % SIZE;
		p = { (double)x, (double)y };
		double distance = dist(p, center);
		if (distance > R * R)
			SetPixel(hdc, x + SIZE, y, RGB(0, 255, 0));
		else
		{
			cnt++;
			SetPixel(hdc, x + SIZE, y, RGB(0, 0, 255));
		}
		if (i % 1000000 == 0)
		{
			COORD Pos = { cursor.x, cursor.y++ };
			SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), Pos);
			printf("시행횟수 : %d번, 근사값 : %lf\n", i, (double)cnt / (double)i * (double)4.0);
		}
	}
	ReleaseDC(hwnd, hdc);
	getch();
	return 0;
}
~~~

![example](\images\example.JPG)