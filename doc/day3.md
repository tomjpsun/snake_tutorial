Day 3：


今天學習 Lesson 3 [03_event_driven_programming.cpp](https://lazyfoo.net/tutorials/SDL/03_event_driven_programming/index.php)
程式碼跟 Lesson 2 差不多。因為我們前兩天着重在 CMake 開發環境，今天就專注於這個 program 吧！

我們依照邏輯將初始化的動作放在 init() 裡面，包含了

	SDL_init()
	SDL_CreateWindow()
	SDL_GetWindowSurface()

SDL_init()：詳細說明參考 https://wiki.libsdl.org/CategoryInit
它告訴整個 SDL Library 準備開始動作。

SDL_CreateWindow():詳細說明參考 https://wiki.libsdl.org/SDL_CreateWindow
它告訴 SDL 產生一個視窗，準備在裡面畫畫了，return 會得到一個指標，指向該 Window 的資料。

SDL_GetWindowSurface():詳細說明參考 https://wiki.libsdl.org/SDL_GetWindowSurface
它幫我們取得畫圖的區域，這個區域會隨着 Window 放大、縮小或需要重新畫畫面改變（這個動作叫做 Invalidate）。
因為區域大小改變，所以需要重新呼叫 SDL_GetWindowSurface() 重新取得新的畫圖區域。


loadMedia() 主要是包裝了 SDL_LoadBMP()。它從檔案讀取一個圖案，令 gXOut 指向它，然後利用

	SDL_BlitSurface( gXOut, NULL, gScreenSurface, NULL );

畫到取得的 window surface 上，也就是 gScreenSurface 指標指到的記憶體位置。

好！畫圖的部分大概了解，再來了解 Event 機制。
請參考這裡的圖：https://lazyfoo.net/tutorials/SDL/03_event_driven_programming/index.php

有一張鍵盤 + 滑鼠 + JoyStick 的輸入，累積在某個 Window 物件上，我們程式利用

	SDL_PollEvent( &e )

一次一個取出這些周邊設備的輸入，然後看看要怎麼處理。
取出的"輸入事件"放在

	SDL_Event e

裡面。

我們今天的程式，判斷 e 是否為 “Quit Window”，如果是，
就設定 quit 變數為 True，就可以離開 while(!quit) 迴圈，
如果沒有事件，或不是 Quit 事件，就繼續 while(!quit) loop。

這個有別於昨天的地方，昨天 Lesson 2 是等待 2000 毫秒，即 2 秒就離開。
（請比較一下 Lesson 2 的程式碼）

最後看  close() function：它就是歸還資源；所有我們跟 SDL 拿的 Window、Surface 都要歸還，
然後告訴 SDL Library 離開： SDL_Quit()

以上是今天的學習，我們可以試試看，抓到滑鼠 x,y 坐標 要如何做？

提示 1. 參考 https://wiki.libsdl.org/SDL_Event
知道應該處理 SDL_MouseMotionEvent，參考 https://wiki.libsdl.org/SDL_MouseMotionEvent
要判斷 SDL_MOUSEMOTION

提示 2.

					//User requests quit
					if ( e.type == SDL_QUIT )
					{
						quit = true;
					}
					else if ( e.type == SDL_MOUSEMOTION)
					{
						int x,y;
						SDL_GetMouseState(&x, &y);
						printf("(%d,%d)     ", x, y);
					}

請讀者務必實際修改，體會一下。

今天的學習就到這裡。
#
