# RaspBerry-Club

###### '라즈베리파이 동아리(2021)'를 위해 만들어진 레포입니다.

## 1. 코드 다운로드

[[ 클릭시 다운로드 ]](https://github.com/8BookIt8/RaspBerry-Club/releases/download/v.0.0.1/Tetris.zip)

###### 위의 링크를 통해 파일을 다운로드한 후 아래의 사진을 따라 진행해 주세요.



![압출 풀기](https://user-images.githubusercontent.com/83459153/121510834-03b82d00-ca23-11eb-95ae-6359db3bb2bf.png)

###### 다운로드한 파일을 바탕화면에 꺼내고, 압축을 풀어줍니다.

###### 단, 압축 파일 속 파일 2개가 모두 한 폴더 안에 위치하도록 풀어주세요.

![image](https://user-images.githubusercontent.com/83459153/121511074-3feb8d80-ca23-11eb-90ac-cb19538ac84f.png)

###### < 파일이 한 폴더 안에 위치한 모습 >

## 2. 파일 소개

###### 1. tetris.h

> 테트리스 구현에 필요한 함수들이 담겨 있는 헤더 파일



###### 2. main.c

> 실행시킬 main 파일

## 3. 제작

###### main.c 파일과 tetris.h 파일을 코드 블록을 이용해 열어줍니다.

> 저희는 tetris.h 헤더 파일을 이용하여 main.c 파일을 수정합니다
>
> 주석 처리된 부분의 코드만 구현하면 됩니다.



```c
#define _CRT_SECURE_NO_WARNINGS
#include <time.h>
#include "tetris.h"

int main() {
    pBLOCK blockHead = malloc(sizeof(BLOCK));
    blockHead->next = NULL;

    pBLOCK nextBlockHead = malloc(sizeof(BLOCK));
    nextBlockHead->next = NULL;

    clock_t time_start = clock();

    // 게임 초기화
    // 첫 번째 블럭 생성

    while(1) {
        if (checkTopLine() != 0) {
            // 게임 종료
        }

        if (blockHead->next == NULL) {
            exchgBlock(blockHead, nextBlockHead);
        }

        if (nextBlockHead->next == NULL) {
            createBlock(nextBlockHead);
            printNextBlock(nextBlockHead);
        }

        clearBlock(blockHead);

        // 현재 시간 변수(time_now)
        // 시간 차이 변수(time_diff)
        /* 
        * 시간 차이(time_diff)와 게임 속도(speed)를 비교
        * 블럭 한 칸 아래로 이동
        */

        if (_kbhit() != 0) {
            int key = _getch();
            /*
            * 스페이스바(32)를 눌렀을 때 블럭 드랍
            */

            if (key == 224) {
                key = _getch();
                if (key == 72) {
                    if (checkLeftBlock(blockHead) != 1 || checkRightBlock(blockHead) != 1)
                        // 블럭 회전
                } else if (key == 80) {
                    // 블럭 한 칸 아래로 이동
                } else if (key == 75 && checkSide(blockHead, 1) >= 1 && checkLeftBlock(blockHead) != 1) {
                    // 블럭 한 칸 왼쪽으로 이동
                } else if (key == 77 && checkSide(blockHead, 0) <= FIELD_WIDTH - 2 && checkRightBlock(blockHead) != 1) {
                    // 불록 한 칸 오른쪽으로 이동
                }
            }
        }

        if (getDistance(blockHead) <= 0) {
            putBlocksInField(blockHead);
            exchgBlock(blockHead, nextBlockHead);
        }

        // 필드 청소
        int numberOfLines = checkFieldLine();
        // 게임 속도 업데이트

        // 필드 출력
        // 현재 블럭 출력

        Sleep(37);
    }

    // 게임 오버 화면 출력
    freeBlockMemory(blockHead);
    freeBlockMemory(nextBlockHead);
    return 0;
}

```

###### < main.c 파일 속 코드 >



#### 1. 게임 초기화와 첫 블럭 생성 (Line 14 ~ 15)

```c
clock_t time_start = clock();

// 게임 초기화
// 첫 번째 블럭 생성

while(1) {
```

###### 첫 번째로 구현해야 할 기능은 게임 초기화(기본 설정)과 첫 블럭 생성입니다.

###### 예시로 게임 초기화 부분만 보여드리겠습니다.

```c
// 게임 시작시 
void init() {
    srand(time(NULL));
    hideConsoleCursor();
    resetField();
    printStartingScreen();
    system("cls");
    printGameScreen();
}
```

###### < tetris.h 헤더 파일 속 init 함수와 그 주석 >

###### 위의 init 함수를 이용하여 게임 초기화 코드를 완성하자면 다음과 같습니다.

```c
clock_t time_start = clock();

init()
// 첫 번째 블럭 생성

while(1) {
```

###### 이러한 방식으로 main.c 파일에서 요구하는 코드를(주석으로 표시) tetris.h 헤더 파일(주석으로 된 함수 설명 참조)나 다른 함수들을 이용하여 구현하면 됩니다.



#### 2. 게임 오버 (Line : 19)

```c
if (checkTopLine() != 0) {
    // 게임 종료
}
```

###### 블럭이 천장(끝)까지 닿았을 때 게임을 종료시키는 코드입니다.

###### 어떤 함수를 사용해야 while 반복문을 탈출할 수 있을지 고민해 보고 작성하면 됩니다.



#### 3. 블럭 떨어짐 (Line : 33 ~ 38)

```c
clearBlock(blockHead);

// 현재 시간 변수(time_now)
// 시간 차이 변수(time_diff)
/* 
* 시간 차이(time_diff)와 게임 속도(speed)를 비교
* 블럭 한 칸 아래로 이동
*/

if (_kbhit() != 0) {
```

###### 일정 시간(게임 속도에 따라 달라짐)마다 블럭을 아래로 움직이는 코드입니다.

###### 위 주석에서 알 수 있듯 시작 시간(time_start)와 현재 시간(time_now)의 차이를 이용합니다.

###### 현재 시간을 불러오는 방법은 time_start 변수에 값을 할당한 방법을 참고하면 됩니다.

```c
clock_t time_start = clock();
```

###### 두 시간의 차이를 구하는 함수와 블럭을 한 칸 아래로 이동시키는 함수는 모두 tetris.h 파일에 있으나

###### 블럭을 한 칸 이동시키는 함수의 경우에는 매개변수를 필요로 하기에 예시를 보여드리겠습니다.

```c
clearBlock(blockHead);

// 현재 시간 변수(time_now)
// 시간 차이 변수(time_diff)
// 
// 시간 차이(time_diff)와 게임 속도(speed)를 비교
	moveDown(blockHead);
//

if (_kbhit() != 0) {
```

###### 위와 같이 블럭을 이동(및 회전, 드랍)시키는 함수에는 매개변수(괄호 안)로 blockHead라는 변수를 사용하시면 됩니다.

###### 또한 블럭을 움직인 후 time_start 값을 변경하는 것을 잊어서는 안됩니다.



#### 4. 블럭 조정 (Line : 40 ~ 59)

```c
if (_kbhit() != 0) {
	int key = _getch();
	/*
	* 스페이스바(32)를 눌렀을 때 블럭 드랍
	*/

	if (key == 224) {
		key = _getch();
		if (key == 72) {
			if (checkLeftBlock(blockHead) != 1 || checkRightBlock(blockHead) != 1)
				// 블럭 회전
		} else if (key == 80) {
			// 블럭 한 칸 아래로 이동
		} else if (key == 75 && checkSide(blockHead, 1) >= 1 && checkLeftBlock(blockHead) != 1) {
			// 블럭 한 칸 왼쪽으로 이동
		} else if (key == 77 && checkSide(blockHead, 0) <= FIELD_WIDTH - 2 && checkRightBlock(blockHead) != 1) {
			// 불록 한 칸 오른쪽으로 이동
        	}
	}
}
```

###### 이번에는 방향키를 이용한 블럭 이동과 블럭 드랍 기능입니다.

###### _getch() 함수는 키보드로 입력받은 값을 정수 형태로 반환해 줍니다. ( 여기서는 key 변수에 저장 )

###### 스페이스바는 32, 방향키는 224를 반환 후 추가적으로 위, 아래, 왼쪽, 오른쪽 각각 72, 80, 75, 77의 값을 반환합니다.

###### 블럭 드랍, 회전, 삼방향 이동 함수는 tetris.h 파일에서 제공하니 잘 찾아 사용하시면 됩니다.



#### 5. 필드 업데이트 (Line : 66 ~ 71)

```c
// 필드 청소
int numberOfLines = checkFieldLine();
// 게임 속도 업데이트

// 필드 출력
// 현재 블럭 출력

Sleep(37);
```

###### 거의 다 왔으니 조금만 힘을 내시기 바랍니다.

###### 이번에는 필드(게임창) 업데이트 코드입니다.

###### 필드 청소는 말 그대로 필드를 깔끔하게 지우는 것입니다. tetris.h 파일에서 clearField()라는 함수로 지원합니다.

###### 게임 속도 업데이트는 게임의 난이도를 후반으로 갈수록 증가시키기 위한 기능입니다.

###### 필드 출력과 현재 블럭 출력은 각각 이미 블럭이 쌓인 곳에 '□'를, 현재 떨어지고 있는 블럭을 출력하는 기능입니다.

###### 앞서 설명한 모든 기능들은 tetris.h 파일에서 함수 형태로 지원하고 있습니다.



#### 6. 게임 오버 스크린 출력 (Line : 76)

```c
// 게임 오버 화면 출력
freeBlockMemory(blockHead);
freeBlockMemory(nextBlockHead);
```

###### '2. 게임 오버'에서 제작한 코드로 인해 반복문이 종료되면 게임이 끝났다는 화면을 출력하는 코드입니다.

###### 이 또한 tetris.h 파일에서 지원하는 함수 중 잘 찾아 사용하시면 됩니다.

*****

###### 학교에서 동아리 수업을 진행 중인 학생들은 회장 및 부회장에게 코드 실행 확인 검사를 맡고,

###### 집에서 동아리 수업을 듣고 있는 학생들은 코드가 구동되는 장면을 스크린샷 또는 핸드폰 카메라로 찍어 카카오톡 채팅방에 올려,

###### 회장 또는 부회장의 허락을 맡고 남은 시간 자습하면 됩니다.



#### 시험공부 힘들겠지만 조금만 더 노력합시다!!
